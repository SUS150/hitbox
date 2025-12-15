-- Hitbox Settings UI - Animação Premium com Selação de Estado
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

-- CONFIGURAÇÕES FINAIS (Obrigatórias)
local TAMANHO_FINAL = UDim2.new(0, 420, 0, 320)
local CORNER_FINAL = UDim.new(0, 30)
local TRANSPARENCIA_FINAL = 0.07 -- Valor sutil que você pediu

-- MAINFRAME
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainPanel"
MainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
MainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
MainFrame.Size = UDim2.new(0, 60, 0, 60)
MainFrame.BackgroundColor3 = Color3.fromRGB(24, 24, 24)
MainFrame.BackgroundTransparency = 0.5 -- Começa mais sumido
MainFrame.ClipsDescendants = true
MainFrame.Active = true
MainFrame.Draggable = true

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(1, 0) -- Círculo inicial
MainCorner.Parent = MainFrame

-- BACKDROP (Camada de vidro que surge depois)
local Backdrop = Instance.new("Frame")
Backdrop.Name = "Backdrop"
Backdrop.Parent = MainFrame
Backdrop.ZIndex = -1
Backdrop.Size = UDim2.new(1, 0, 1, 0)
Backdrop.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
Backdrop.BackgroundTransparency = 1 -- Invisível no início para não criar a "bola"
local BackdropCorner = Instance.new("UICorner")
BackdropCorner.CornerRadius = MainCorner.CornerRadius
BackdropCorner.Parent = Backdrop

local gradient = Instance.new("UIGradient")
gradient.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(45, 45, 45)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(25, 25, 25))
}
gradient.Rotation = 90
gradient.Parent = MainFrame

local MainStroke = Instance.new("UIStroke")
MainStroke.Thickness = 1.2
MainStroke.Color = Color3.fromRGB(255, 255, 255)
MainStroke.Transparency = 1 -- Começa invisível
MainStroke.Parent = MainFrame

-- [HEADER E CONTROLES MANTIDOS EXATAMENTE IGUAIS]
local HeaderFrame = Instance.new("Frame")
HeaderFrame.Parent = MainFrame
HeaderFrame.BackgroundTransparency = 1
HeaderFrame.Size = UDim2.new(1, 0, 0, 85)

local TitleLabel = Instance.new("TextLabel")
TitleLabel.Parent = HeaderFrame
TitleLabel.BackgroundTransparency = 1
TitleLabel.Position = UDim2.new(0, 30, 0.4, 0)
TitleLabel.Size = UDim2.new(1, -60, 0, 35)
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.Text = "Hitbox Settings"
TitleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
TitleLabel.TextSize = 24 
TitleLabel.TextXAlignment = Enum.TextXAlignment.Left

local ControlsScroll = Instance.new("ScrollingFrame")
ControlsScroll.Parent = MainFrame
ControlsScroll.BackgroundTransparency = 1
ControlsScroll.Position = UDim2.new(0, 0, 0, 100)
ControlsScroll.Size = UDim2.new(1, 0, 1, -120)
ControlsScroll.CanvasSize = UDim2.new(0, 0, 0, 0)
ControlsScroll.ScrollBarThickness = 0

local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Parent = ControlsScroll
UIListLayout.Padding = UDim.new(0, 10)

local UIPadding = Instance.new("UIPadding")
UIPadding.Parent = ControlsScroll
UIPadding.PaddingLeft = UDim.new(0, 25)
UIPadding.PaddingRight = UDim.new(0, 25)

-- FUNÇÃO CREATECARD (Sem alterações de hierarquia)
local function CreateCard(text, layoutOrder)
    local Card = Instance.new("Frame")
    Card.Parent = ControlsScroll
    Card.Size = UDim2.new(1, 0, 0, 52)
    Card.BackgroundColor3 = Color3.fromRGB(50, 50, 50) 
    Card.LayoutOrder = layoutOrder
    Instance.new("UICorner", Card).CornerRadius = UDim.new(0, 14)
    local Label = Instance.new("TextLabel", Card)
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

local Card1 = CreateCard("Ativar Hitbox", 1)
local Card2 = CreateCard("Tamanho da Hitbox", 2)
local Card3 = CreateCard("Visualizar Hitboxes", 3)

-- ANIMAÇÃO DE ENTRADA (Refinada 0.8s Quint)
MainFrame.Parent = ScreenGui

local infoPrincipal = TweenInfo.new(0.8, Enum.EasingStyle.Quint, Enum.EasingDirection.Out)
local infoSuave = TweenInfo.new(0.5, Enum.EasingStyle.Sine, Enum.EasingDirection.Out)

local animSize = TweenService:Create(MainFrame, infoPrincipal, {
    Size = TAMANHO_FINAL,
    BackgroundTransparency = TRANSPARENCIA_FINAL
})

local animCorner = TweenService:Create(MainCorner, infoPrincipal, {
    CornerRadius = CORNER_FINAL
})

local animVisuals = TweenService:Create(Backdrop, infoSuave, {
    BackgroundTransparency = 0.4
})

local animStroke = TweenService:Create(MainStroke, infoSuave, {
    Transparency = 0.85
})

-- Execução síncrona
animSize:Play()
animCorner:Play()
animVisuals:Play()
animStroke:Play()

-- SELAGEM DE ESTADO (O pulo do gato para evitar a "bola" fantasma)
animSize.Completed:Connect(function()
    MainFrame.Size = TAMANHO_FINAL
    MainFrame.BackgroundTransparency = TRANSPARENCIA_FINAL
    MainCorner.CornerRadius = CORNER_FINAL
    Backdrop.BackgroundTransparency = 0.4
    MainStroke.Transparency = 0.85
    -- Garante que o backdrop siga o corner final
    BackdropCorner.CornerRadius = CORNER_FINAL
end)

-- [LÓGICA DE FUNCIONAMENTO MANTIDA]
UIListLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    ControlsScroll.CanvasSize = UDim2.new(0, 0, 0, UIListLayout.AbsoluteContentSize.Y + 20)
end)
