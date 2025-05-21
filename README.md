local player = game.Players.LocalPlayer
local PlayerGui = player:WaitForChild("PlayerGui")

-- ANTI DETECÇÃO
pcall(function()
	for _, v in pairs(PlayerGui:GetChildren()) do
		if v.Name == "HYPER_HUB_PRO" or v.Name:find("HYPER") then
			v:Destroy()
		end
	end
end)

local gui = Instance.new("ScreenGui")
gui.Name = "HYP_"..math.random(1000,9999)
gui.ResetOnSpawn = false
gui.IgnoreGuiInset = true
gui.Parent = (gethui and gethui()) or PlayerGui
pcall(function() gui.Parent = gethui() end)
gui.DisplayOrder = 999999

-- Painel principal
local main = Instance.new("Frame")
main.Size = UDim2.new(0, 500, 0, 320)
main.Position = UDim2.new(0.5, -250, 0.5, -160)
main.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
main.BorderSizePixel = 0
main.Active = true
main.Draggable = true
main.Parent = gui

Instance.new("UICorner", main).CornerRadius = UDim.new(0, 10)

-- Sombra
local shadow = Instance.new("ImageLabel", main)
shadow.AnchorPoint = Vector2.new(0.5, 0.5)
shadow.Position = UDim2.new(0.5, 0, 0.5, 4)
shadow.Size = UDim2.new(1, 60, 1, 60)
shadow.Image = "rbxassetid://1316045217"
shadow.ImageTransparency = 0.7
shadow.ScaleType = Enum.ScaleType.Slice
shadow.SliceCenter = Rect.new(10,10,118,118)
shadow.BackgroundTransparency = 1
shadow.ZIndex = 0

-- Título
local title = Instance.new("TextLabel", main)
title.Size = UDim2.new(1, 0, 0, 35)
title.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
title.Text = "HYPER HUB"
title.TextColor3 = Color3.fromRGB(0, 255, 127)
title.Font = Enum.Font.GothamBlack
title.TextSize = 18
title.BorderSizePixel = 0
title.TextStrokeTransparency = 0.8
Instance.new("UICorner", title).CornerRadius = UDim.new(0, 6)

-- Minimizar
local minimizeBtn = Instance.new("TextButton", main)
minimizeBtn.Size = UDim2.new(0, 30, 0, 30)
minimizeBtn.Position = UDim2.new(0, 5, 0, 2)
minimizeBtn.Text = "="
minimizeBtn.TextColor3 = Color3.fromRGB(0, 255, 100)
minimizeBtn.Font = Enum.Font.GothamBold
minimizeBtn.TextSize = 18
minimizeBtn.BackgroundColor3 = Color3.fromRGB(38, 38, 38)
Instance.new("UICorner", minimizeBtn).CornerRadius = UDim.new(1, 0)

-- Sidebar
local sidebar = Instance.new("Frame", main)
sidebar.Size = UDim2.new(0, 120, 1, -40)
sidebar.Position = UDim2.new(0, 5, 0, 40)
sidebar.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
Instance.new("UICorner", sidebar).CornerRadius = UDim.new(0, 6)
Instance.new("UIListLayout", sidebar).Padding = UDim.new(0, 6)

-- Conteúdo
local content = Instance.new("ScrollingFrame", main)
content.Size = UDim2.new(1, -135, 1, -40)
content.Position = UDim2.new(0, 130, 0, 40)
content.BackgroundColor3 = Color3.fromRGB(32, 32, 32)
content.CanvasSize = UDim2.new(0, 0, 1, 0)
content.ScrollBarThickness = 5
content.BorderSizePixel = 0
Instance.new("UICorner", content).CornerRadius = UDim.new(0, 6)
Instance.new("UIListLayout", content).Padding = UDim.new(0, 5)

-- BOTÃO ESTILIZADO
local function criarBotao(nome, acao)
	local btn = Instance.new("TextButton")
	btn.Size = UDim2.new(1, -10, 0, 35)
	btn.Text = nome
	btn.Font = Enum.Font.GothamBold
	btn.TextSize = 14
	btn.TextColor3 = Color3.fromRGB(255, 255, 255)
	btn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
	Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)

	-- Hover
	btn.MouseEnter:Connect(function()
		btn.BackgroundColor3 = Color3.fromRGB(0, 255, 127)
		btn.TextColor3 = Color3.fromRGB(0, 0, 0)
	end)
	btn.MouseLeave:Connect(function()
		btn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
		btn.TextColor3 = Color3.fromRGB(255, 255, 255)
	end)

	btn.MouseButton1Click:Connect(acao)
	btn.Parent = content
end

-- SLIDER MELHORADO
local function criarSlider()
	local holder = Instance.new("Frame", content)
	holder.Size = UDim2.new(1, -10, 0, 50)
	holder.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
	Instance.new("UICorner", holder).CornerRadius = UDim.new(0, 6)

	local label = Instance.new("TextLabel", holder)
	label.Size = UDim2.new(1, -20, 0, 20)
	label.Position = UDim2.new(0, 10, 0, 5)
	label.Text = "Valor: 0"
	label.TextColor3 = Color3.fromRGB(255, 255, 255)
	label.Font = Enum.Font.Gotham
	label.TextSize = 14
	label.BackgroundTransparency = 1

	local slider = Instance.new("Frame", holder)
	slider.Position = UDim2.new(0, 10, 0, 28)
	slider.Size = UDim2.new(1, -20, 0, 10)
	slider.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
	slider.Name = "SliderBar"
	Instance.new("UICorner", slider).CornerRadius = UDim.new(0, 6)

	local fill = Instance.new("Frame", slider)
	fill.Size = UDim2.new(0, 0, 1, 0)
	fill.BackgroundColor3 = Color3.fromRGB(0, 255, 127)
	fill.BorderSizePixel = 0
	fill.Name = "Fill"
	Instance.new("UICorner", fill).CornerRadius = UDim.new(0, 6)
	fill.Parent = slider

	local dragging = false

	local function updateSlider(x)
		local rel = x - slider.AbsolutePosition.X
		local percent = math.clamp(rel / slider.AbsoluteSize.X, 0, 1)
		fill.Size = UDim2.new(percent, 0, 1, 0)
		label.Text = "Valor: " .. math.floor(percent * 100)
	end

	slider.InputBegan:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.MouseButton1 then
			dragging = true
			updateSlider(input.Position.X)
		end
	end)

	game:GetService("UserInputService").InputChanged:Connect(function(input)
		if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
			updateSlider(input.Position.X)
		end
	end)

	game:GetService("UserInputService").InputEnded:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.MouseButton1 then
			dragging = false
		end
	end)
end

-- ABA
local function criarAba(nome, conteudos)
	local aba = Instance.new("TextButton", sidebar)
	aba.Size = UDim2.new(1, -10, 0, 30)
	aba.Text = nome
	aba.Font = Enum.Font.GothamBold
	aba.TextSize = 14
	aba.TextColor3 = Color3.fromRGB(0, 255, 100)
	aba.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
	Instance.new("UICorner", aba).CornerRadius = UDim.new(0, 6)

	aba.MouseButton1Click:Connect(function()
		for _, v in pairs(content:GetChildren()) do
			if v:IsA("TextButton") or v:IsA("Frame") then
				v:Destroy()
			end
		end

		for _, info in pairs(conteudos) do
			if info.tipo == "botao" then
				criarBotao(info.nome, info.funcao)
			elseif info.tipo == "slider" then
				criarSlider()
			end
		end
	end)
end

-- Exemplo de abas
criarAba("Funções", {
	{tipo = "botao", nome = "ESP Nick", funcao = function() print("ESP Ativado") end},
	{tipo = "botao", nome = "Aimbot", funcao = function() print("Aimbot Ligado") end},
})

criarAba("Ajustes", {
	{tipo = "slider"}
})

-- Minimizar
local minimized = false
minimizeBtn.MouseButton1Click:Connect(function()
	minimized = not minimized
	if minimized then
		sidebar.Visible = false
		content.Visible = false
		title.Visible = false
		main:TweenSize(UDim2.new(0, 80, 0, 30), "Out", "Quad", 0.3, true)
	else
		main:TweenSize(UDim2.new(0, 500, 0, 320), "Out", "Quad", 0.3, true)
		wait(0.3)
		sidebar.Visible = true
		content.Visible = true
		title.Visible = true
	end
end)
