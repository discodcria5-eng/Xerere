--[[
    STEAL A EGG
    Painel GUI moderno e móvel
    Estrutura para funções permitidas pelo jogo

    Compatível com:
    - PC
    - Celular
]]

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")

local player = Players.LocalPlayer

-- =========================
-- CONFIGURAÇÃO
-- =========================

local gui = Instance.new("ScreenGui")
gui.Name = "StealAEggPanel"
gui.ResetOnSpawn = false
gui.Parent = player:WaitForChild("PlayerGui")

-- Painel principal
local main = Instance.new("Frame")
main.Size = UDim2.new(0, 300, 0, 380)
main.Position = UDim2.new(0.5, -150, 0.5, -190)
main.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
main.BorderSizePixel = 0
main.Parent = gui

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 14)
corner.Parent = main

-- Título
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, -55, 0, 50)
title.Position = UDim2.new(0, 15, 0, 0)
title.BackgroundTransparency = 1
title.Text = "🥚 STEAL A EGG"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.TextSize = 20
title.Font = Enum.Font.GothamBold
title.TextXAlignment = Enum.TextXAlignment.Left
title.Parent = main

-- Botão minimizar
local minimize = Instance.new("TextButton")
minimize.Size = UDim2.new(0, 40, 0, 40)
minimize.Position = UDim2.new(1, -45, 0, 5)
minimize.Text = "−"
minimize.TextSize = 25
minimize.TextColor3 = Color3.fromRGB(255, 255, 255)
minimize.BackgroundColor3 = Color3.fromRGB(45, 45, 55)
minimize.Parent = main

local minCorner = Instance.new("UICorner")
minCorner.CornerRadius = UDim.new(0, 10)
minCorner.Parent = minimize

-- Área dos botões
local container = Instance.new("ScrollingFrame")
container.Size = UDim2.new(1, -20, 1, -65)
container.Position = UDim2.new(0, 10, 0, 55)
container.BackgroundTransparency = 1
container.BorderSizePixel = 0
container.ScrollBarThickness = 5
container.Parent = main

local layout = Instance.new("UIListLayout")
layout.Padding = UDim.new(0, 8)
layout.HorizontalAlignment = Enum.HorizontalAlignment.Center
layout.Parent = container

-- =========================
-- SISTEMA DE ARRASTAR
-- =========================

local dragging = false
local dragStart
local startPosition

local function updateDrag(input)
    local delta = input.Position - dragStart

    main.Position = UDim2.new(
        startPosition.X.Scale,
        startPosition.X.Offset + delta.X,
        startPosition.Y.Scale,
        startPosition.Y.Offset + delta.Y
    )
end

title.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1
        or input.UserInputType == Enum.UserInputType.Touch then

        dragging = true
        dragStart = input.Position
        startPosition = main.Position
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if dragging and (
        input.UserInputType == Enum.UserInputType.MouseMovement
        or input.UserInputType == Enum.UserInputType.Touch
    ) then
        updateDrag(input)
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1
        or input.UserInputType == Enum.UserInputType.Touch then
        dragging = false
    end
end)

-- =========================
-- BOTÃO DE FUNÇÃO
-- =========================

local function createToggle(name, callback)

    local enabled = false

    local button = Instance.new("TextButton")
    button.Size = UDim2.new(1, -5, 0, 55)
    button.BackgroundColor3 = Color3.fromRGB(40, 40, 48)
    button.Text = name .. "  [OFF]"
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.TextSize = 16
    button.Font = Enum.Font.GothamSemibold
    button.AutoButtonColor = false
    button.Parent = container

    local buttonCorner = Instance.new("UICorner")
    buttonCorner.CornerRadius = UDim.new(0, 10)
    buttonCorner.Parent = button

    button.MouseButton1Click:Connect(function()

        enabled = not enabled

        if enabled then
            button.Text = name .. "  [ON]"
            button.BackgroundColor3 = Color3.fromRGB(45, 130, 75)
        else
            button.Text = name .. "  [OFF]"
            button.BackgroundColor3 = Color3.fromRGB(40, 40, 48)
        end

        callback(enabled)
    end)

    return button
end

-- =========================
-- FUNÇÕES PERMITIDAS
-- =========================

createToggle("🔔 Notificações", function(enabled)
    print("Notificações:", enabled)
end)

createToggle("🔊 Sons do jogo", function(enabled)
    print("Sons:", enabled)
end)

createToggle("✨ Efeitos visuais", function(enabled)
    print("Efeitos:", enabled)
end)

createToggle("📱 Interface compacta", function(enabled)
    print("Interface compacta:", enabled)
end)

createToggle("🎯 Indicadores visuais", function(enabled)
    print("Indicadores:", enabled)
end)

-- =========================
-- MINIMIZAR / ABRIR
-- =========================

local minimized = false

minimize.MouseButton1Click:Connect(function()

    minimized = not minimized

    if minimized then
        container.Visible = false
        main.Size = UDim2.new(0, 300, 0, 55)
        minimize.Text = "+"
    else
        container.Visible = true
        main.Size = UDim2.new(0, 300, 0, 380)
        minimize.Text = "−"
    end
end)

-- =========================
-- BOTÃO FLUTUANTE PARA REABRIR
-- =========================

local openButton = Instance.new("TextButton")
openButton.Size = UDim2.new(0, 60, 0, 60)
openButton.Position = UDim2.new(0, 15, 0.5, -30)
openButton.BackgroundColor3 = Color3.fromRGB(35, 35, 42)
openButton.Text = "🥚"
openButton.TextSize = 28
openButton.Visible = false
openButton.Parent = gui

local openCorner = Instance.new("UICorner")
openCorner.CornerRadius = UDim.new(1, 0)
openCorner.Parent = openButton

openButton.MouseButton1Click:Connect(function()
    main.Visible = true
    openButton.Visible = false
end)

print("Steal a Egg GUI carregado!")# Xerere
