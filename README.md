-- Hitbox Settings UI - Título Ajustado e Estética Refinada
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local CoreGui = game:GetService("CoreGui")

local player = Players.LocalPlayer
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "HitboxSettings"
ScreenGui.Parent = CoreGui
ScreenGui.ResetOnSpawn = false
ScreenGui.IgnoreGuiInset = true 
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

-- ESTADO
local ativo = false
local visivel = false
local tamanhoAtual = 20

-- MAINFRAME
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainPanel"
MainFrame.Parent = ScreenGui
MainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
MainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
MainFrame.Size = UDim2.new(0, 420, 0, 320)
MainFrame.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
MainFrame.BackgroundTransparency = 0.25 
MainFrame.ClipsDescendants = true
MainFrame.Active = true
MainFrame.Draggable = true

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 30)
MainCorner.Parent = MainFrame

-- BACKDROP (Camada de Contraste)
local Backdrop = Instance.new("Frame")
Backdrop.Name = "Backdrop"
Backdrop.Parent = MainFrame
Backdrop.ZIndex = -1
Backdrop.Size = UDim2.new(1, 0, 1, 0)
Backdrop.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
Backdrop.BackgroundTransparency = 0.6 
local BackdropCorner = Instance.new("UICorner")
BackdropCorner.CornerRadius = UDim.new(0, 30)
BackdropCorner.Parent = Backdrop

local gradient = Instance.new("UIGradient")
gradient.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(60, 60, 60)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(30, 30, 30))
}
gradient.Rotation = 90
gradient.Parent = MainFrame

local MainStroke = Instance.new("UIStroke")
MainStroke.Thickness = 1
MainStroke.Color = Color3.fromRGB(255, 255, 255)
MainStroke.Transparency = 0.85 
MainStroke.Parent = MainFrame

-- HEADER
local HeaderFrame = Instance.new("Frame")
HeaderFrame.Name = "Header"
HeaderFrame.Parent = MainFrame
HeaderFrame.BackgroundTransparency = 1
HeaderFrame.Size = UDim2.new(1, 0, 0, 85) -- Aumentado levemente para o título maior

local TitleLabel = Instance.new("TextLabel")
TitleLabel.Name = "Title"
TitleLabel.Parent = HeaderFrame
TitleLabel.BackgroundTransparency = 1
TitleLabel.Position = UDim2.new(0, 30, 0.4, 0) -- Ajuste de posição vertical
TitleLabel.Size = UDim2.new(1, -60, 0, 35)
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.Text = "Hitbox Settings"
TitleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
TitleLabel.TextSize = 24 -- Aumentado conforme solicitado
TitleLabel.TextXAlignment = Enum.TextXAlignment.Left

-- CONTROLS SCROLL
local ControlsScroll = Instance.new("ScrollingFrame")
ControlsScroll.Name = "Controls"
ControlsScroll.Parent = MainFrame
ControlsScroll.BackgroundTransparency = 1
ControlsScroll.Position = UDim2.new(0, 0, 0, 100) -- Descido para não colar no título
ControlsScroll.Size = UDim2.new(1, 0, 1, -120)
ControlsScroll.CanvasSize = UDim2.new(0, 0, 0, 0)
ControlsScroll.ScrollBarThickness = 0
ControlsScroll.Active = true

local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Parent = ControlsScroll
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.Padding = UDim.new(0, 10)

local UIPadding = Instance.new("UIPadding")
UIPadding.Parent = ControlsScroll
UIPadding.PaddingLeft = UDim.new(0, 25)
UIPadding.PaddingRight = UDim.new(0, 25)

-- FUNÇÃO CREATECARD
local function CreateCard(name, text, layoutOrder)
    local Card = Instance.new("Frame")
    Card.Name = name
    Card.Parent = ControlsScroll
    Card.Size = UDim2.new(1, 0, 0, 52)
    Card.BackgroundColor3 = Color3.fromRGB(50, 50, 50) 
    Card.BackgroundTransparency = 0.1
    Card.LayoutOrder = layoutOrder
    
    local Corner = Instance.new("UICorner")
    Corner.CornerRadius = UDim.new(0, 14)
    Corner.Parent = Card

    local Stroke = Instance.new("UIStroke")
    Stroke.Color = Color3.fromRGB(255, 255, 255)
    Stroke.Transparency = 0.95
    Stroke.Parent = Card
    
    local Label = Instance.new("TextLabel")
    Label.Name = "Label"
    Label.Parent = Card
    Label.AnchorPoint = Vector2.new(0, 0.5)
    Label.Position = UDim2.new(0, 18, 0.5, 0)
    Label.Size = UDim2.new(0.5, 0, 0.6, 0)
    Label.BackgroundTransparency = 1
    Label.Font = Enum.Font.GothamMedium
    Label.Text = text
    Label.TextColor3 = Color3.fromRGB(255, 255, 255)
    Label.TextSize = 14
    Label.TextXAlignment = Enum.TextXAlignment.Left
    
    return Card
end

-- 1. HITBOX TOGGLE
local HitboxEnabledFrame = CreateCard("HitboxToggle", "Ativar Hitbox", 1)
local ToggleTrack = Instance.new("Frame")
ToggleTrack.Name = "Track"
ToggleTrack.Parent = HitboxEnabledFrame
ToggleTrack.AnchorPoint = Vector2.new(1, 0.5)
ToggleTrack.Position = UDim2.new(1, -15, 0.5, 0)
ToggleTrack.Size = UDim2.new(0, 42, 0, 22)
ToggleTrack.BackgroundColor3 = Color3.fromRGB(75, 75, 75)
local ToggleCorner = Instance.new("UICorner")
ToggleCorner.CornerRadius = UDim.new(1, 0)
ToggleCorner.Parent = ToggleTrack
local ToggleThumb = Instance.new("Frame")
ToggleThumb.Name = "Thumb"
ToggleThumb.Parent = ToggleTrack
ToggleThumb.Size = UDim2.new(0, 18, 0, 18)
ToggleThumb.Position = UDim2.new(0, 2, 0.5, 0)
ToggleThumb.AnchorPoint = Vector2.new(0, 0.5)
ToggleThumb.BackgroundColor3 = Color3.fromRGB(200, 200, 200)
local ThumbCorner = Instance.new("UICorner")
ThumbCorner.CornerRadius = UDim.new(1, 0)
ThumbCorner.Parent = ToggleThumb
local ToggleButton = Instance.new("TextButton")
ToggleButton.Parent = ToggleTrack
ToggleButton.Size = UDim2.new(1, 0, 1, 0)
ToggleButton.BackgroundTransparency = 1
ToggleButton.Text = ""

-- 2. HITBOX SIZE
local HitboxSizeFrame = CreateCard("HitboxSize", "Tamanho da Hitbox", 2)
local SizeInput = Instance.new("TextBox")
SizeInput.Name = "Input"
SizeInput.Parent = HitboxSizeFrame
SizeInput.AnchorPoint = Vector2.new(1, 0.5)
SizeInput.Position = UDim2.new(1, -15, 0.5, 0)
SizeInput.Size = UDim2.new(0, 60, 0, 26)
SizeInput.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
SizeInput.Font = Enum.Font.GothamMedium
SizeInput.Text = "20"
SizeInput.TextColor3 = Color3.fromRGB(255, 255, 255)
SizeInput.TextSize = 14
local InputCorner = Instance.new("UICorner")
InputCorner.CornerRadius = UDim.new(0, 10)
InputCorner.Parent = SizeInput

-- 3. VISUALIZE
local VisualizeFrame = CreateCard("Visualize", "Visualizar Hitboxes", 3)
local VisualizeBtn = Instance.new("TextButton")
VisualizeBtn.Parent = VisualizeFrame
VisualizeBtn.AnchorPoint = Vector2.new(1, 0.5)
VisualizeBtn.Position = UDim2.new(1, -15, 0.5, 0)
VisualizeBtn.Size = UDim2.new(0, 60, 0, 26)
VisualizeBtn.BackgroundTransparency = 1
VisualizeBtn.Font = Enum.Font.GothamBold
VisualizeBtn.Text = "OFF"
VisualizeBtn.TextColor3 = Color3.fromRGB(220, 220, 220)
VisualizeBtn.TextSize = 13

-- LÓGICA (Mantida)
local function animateToggle(state)
    local targetPos = state and UDim2.new(1, -20, 0.5, 0) or UDim2.new(0, 2, 0.5, 0)
    local targetColor = state and Color3.fromRGB(0, 255, 127) or Color3.fromRGB(200, 200, 200)
    local trackColor = state and Color3.fromRGB(0, 100, 50) or Color3.fromRGB(75, 75, 75)
    TweenService:Create(ToggleThumb, TweenInfo.new(0.3, Enum.EasingStyle.Quart), {Position = targetPos, BackgroundColor3 = targetColor}):Play()
    TweenService:Create(ToggleTrack, TweenInfo.new(0.3, Enum.EasingStyle.Quart), {BackgroundColor3 = trackColor}):Play()
end

local function atualizarHitboxes()
    tamanhoAtual = tonumber(SizeInput.Text) or 20
    for _, plr in pairs(Players:GetPlayers()) do
        if plr ~= player and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
            local hrp = plr.Character.HumanoidRootPart
            if ativo then
                hrp.Size = Vector3.new(tamanhoAtual, tamanhoAtual, tamanhoAtual)
                hrp.Transparency = visivel and 0.7 or 1
                hrp.Material = visivel and Enum.Material.Neon or Enum.Material.Plastic
                hrp.CanCollide = false
                hrp.BrickColor = BrickColor.new("Really blue")
            else
                hrp.Size = Vector3.new(2, 2, 1)
                hrp.Transparency = 1
                hrp.CanCollide = true
            end
        end
    end
end

ToggleButton.MouseButton1Click:Connect(function()
    ativo = not ativo
    animateToggle(ativo)
    atualizarHitboxes()
end)

VisualizeBtn.MouseButton1Click:Connect(function()
    visivel = not visivel
    VisualizeBtn.Text = visivel and "ON" or "OFF"
    VisualizeBtn.TextColor3 = visivel and Color3.fromRGB(0, 255, 127) or Color3.fromRGB(220, 220, 220)
    atualizarHitboxes()
end)

SizeInput.FocusLost:Connect(function()
    if not tonumber(SizeInput.Text) then SizeInput.Text = "20" end
    atualizarHitboxes()
end)

task.spawn(function()
    while task.wait(0.5) do
        if ativo then atualizarHitboxes() end
    end
end)

UIListLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    ControlsScroll.CanvasSize = UDim2.new(0, 0, 0, UIListLayout.AbsoluteContentSize.Y + 20)
end)
