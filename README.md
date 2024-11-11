-- Solara Library Professional V1.0
-- Desenvolvido para máxima performance e estilo

local Solara = {}
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

-- Configurações de UI avançadas
local THEME = {
    Primary = Color3.fromRGB(111, 76, 255),    -- Roxo principal
    Secondary = Color3.fromRGB(80, 55, 190),   -- Roxo secundário
    Background = Color3.fromRGB(20, 20, 25),   -- Fundo escuro
    LightBG = Color3.fromRGB(30, 30, 35),      -- Fundo claro
    DarkBG = Color3.fromRGB(15, 15, 20),       -- Fundo mais escuro
    Text = Color3.fromRGB(240, 240, 255),      -- Texto claro
    SubText = Color3.fromRGB(180, 180, 195),   -- Texto secundário
    Accent = Color3.fromRGB(130, 100, 255),    -- Destaque
    Success = Color3.fromRGB(80, 255, 160),    -- Verde sucesso
    Warning = Color3.fromRGB(255, 180, 70),    -- Amarelo aviso
    Error = Color3.fromRGB(255, 80, 80)        -- Vermelho erro
}

-- Configurações de animação
local ANIMATION = {
    DefaultTween = TweenInfo.new(0.3, Enum.EasingStyle.Quart, Enum.EasingDirection.Out),
    FastTween = TweenInfo.new(0.15, Enum.EasingStyle.Quart, Enum.EasingDirection.Out),
    SlowTween = TweenInfo.new(0.5, Enum.EasingStyle.Quart, Enum.EasingDirection.Out)
}

-- Sistema de notificações
local NotificationSystem = {}

function NotificationSystem.new()
    local notifFrame = Instance.new("Frame")
    notifFrame.Size = UDim2.new(0, 300, 1, 0)
    notifFrame.Position = UDim2.new(1, -310, 0, 0)
    notifFrame.BackgroundTransparency = 1
    notifFrame.Parent = CoreGui:WaitForChild("SolaraUI")
    
    local layout = Instance.new("UIListLayout")
    layout.Padding = UDim.new(0, 5)
    layout.VerticalAlignment = Enum.VerticalAlignment.Bottom
    layout.Parent = notifFrame
    
    return notifFrame
end

function NotificationSystem:Notify(title, message, type, duration)
    duration = duration or 5
    local notif = Instance.new("Frame")
    notif.Size = UDim2.new(1, -10, 0, 80)
    notif.BackgroundColor3 = THEME.LightBG
    notif.BorderSizePixel = 0
    notif.Position = UDim2.new(1, 0, 0, 0)
    
    local color
    if type == "success" then color = THEME.Success
    elseif type == "warning" then color = THEME.Warning
    elseif type == "error" then color = THEME.Error
    else color = THEME.Primary end
    
    -- Criar elementos da notificação
    local colorBar = Instance.new("Frame")
    colorBar.Size = UDim2.new(0, 5, 1, 0)
    colorBar.BackgroundColor3 = color
    colorBar.BorderSizePixel = 0
    colorBar.Parent = notif
    
    local titleLabel = Instance.new("TextLabel")
    titleLabel.Size = UDim2.new(1, -20, 0, 25)
    titleLabel.Position = UDim2.new(0, 15, 0, 5)
    titleLabel.BackgroundTransparency = 1
    titleLabel.Text = title
    titleLabel.TextColor3 = THEME.Text
    titleLabel.TextSize = 16
    titleLabel.Font = Enum.Font.GothamBold
    titleLabel.TextXAlignment = Enum.TextXAlignment.Left
    titleLabel.Parent = notif
    
    local messageLabel = Instance.new("TextLabel")
    messageLabel.Size = UDim2.new(1, -20, 0, 40)
    messageLabel.Position = UDim2.new(0, 15, 0, 30)
    messageLabel.BackgroundTransparency = 1
    messageLabel.Text = message
    messageLabel.TextColor3 = THEME.SubText
    messageLabel.TextSize = 14
    messageLabel.Font = Enum.Font.Gotham
    messageLabel.TextXAlignment = Enum.TextXAlignment.Left
    messageLabel.TextWrapped = true
    messageLabel.Parent = notif
    
    -- Animação de entrada
    notif.Parent = self.frame
    TweenService:Create(notif, ANIMATION.FastTween, {Position = UDim2.new(0, 0, 0, 0)}):Play()
    
    -- Remover após duração
    task.delay(duration, function()
        TweenService:Create(notif, ANIMATION.FastTween, {Position = UDim2.new(1, 0, 0, 0)}):Play()
        task.wait(0.3)
        notif:Destroy()
    end)
end

-- Sistema de salvamento
local SaveSystem = {
    fileName = "SolaraSettings.json",
    settings = {}
}

function SaveSystem:Save()
    local json = game:GetService("HttpService"):JSONEncode(self.settings)
    writefile(self.fileName, json)
end

function SaveSystem:Load()
    if isfile(self.fileName) then
        local json = readfile(self.fileName)
        self.settings = game:GetService("HttpService"):JSONDecode(json)
    end
end

-- Função principal para criar a janela
function Solara.CreateWindow(title, config)
    config = config or {}
    
    -- Criar ScreenGui principal
    local SolaraUI = Instance.new("ScreenGui")
    SolaraUI.Name = "SolaraUI"
    SolaraUI.Parent = CoreGui
    
    -- Criar sistema de notificações
    local notifications = NotificationSystem.new()
    notifications.Parent = SolaraUI
    
    -- Criar janela principal
    local MainFrame = Instance.new("Frame")
    MainFrame.Name = "MainFrame"
    MainFrame.Size = UDim2.new(0, 350, 0, 400)
    MainFrame.Position = UDim2.new(0.5, -175, 0.5, -200)
    MainFrame.BackgroundColor3 = THEME.Background
    MainFrame.BorderSizePixel = 0
    MainFrame.Parent = SolaraUI
    
    -- Adicionar efeitos de sombra e arredondamento
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = MainFrame
    
    local shadow = Instance.new("ImageLabel")
    shadow.Size = UDim2.new(1, 40, 1, 40)
    shadow.Position = UDim2.new(0, -20, 0, -20)
    shadow.BackgroundTransparency = 1
    shadow.Image = "rbxassetid://297774371"
    shadow.ImageColor3 = Color3.fromRGB(0, 0, 0)
    shadow.ImageTransparency = 0.6
    shadow.Parent = MainFrame
    
    -- Barra de título
    local TitleBar = Instance.new("Frame")
    TitleBar.Size = UDim2.new(1, 0, 0, 40)
    TitleBar.BackgroundColor3 = THEME.Primary
    TitleBar.BorderSizePixel = 0
    TitleBar.Parent = MainFrame
    
    local titleCorner = Instance.new("UICorner")
    titleCorner.CornerRadius = UDim.new(0, 8)
    titleCorner.Parent = TitleBar
    
    -- Texto do título
    local TitleText = Instance.new("TextLabel")
    TitleText.Size = UDim2.new(1, -20, 1, 0)
    TitleText.Position = UDim2.new(0, 15, 0, 0)
    TitleText.BackgroundTransparency = 1
    TitleText.Text = title
    TitleText.TextColor3 = THEME.Text
    TitleText.TextSize = 18
    TitleText.Font = Enum.Font.GothamBold
    TitleText.TextXAlignment = Enum.TextXAlignment.Left
    TitleText.Parent = TitleBar
    
    -- Botões de controle
    local CloseButton = Instance.new("TextButton")
    CloseButton.Size = UDim2.new(0, 30, 0, 30)
    CloseButton.Position = UDim2.new(1, -35, 0, 5)
    CloseButton.BackgroundColor3 = THEME.Error
    CloseButton.Text = "×"
    CloseButton.TextColor3 = THEME.Text
    CloseButton.TextSize = 20
    CloseButton.Font = Enum.Font.GothamBold
    CloseButton.Parent = TitleBar
    
    local closeCorner = Instance.new("UICorner")
    closeCorner.CornerRadius = UDim.new(0, 6)
    closeCorner.Parent = CloseButton
    
    -- Container principal
    local Container = Instance.new("ScrollingFrame")
    Container.Name = "Container"
    Container.Size = UDim2.new(1, -20, 1, -50)
    Container.Position = UDim2.new(0, 10, 0, 45)
    Container.BackgroundTransparency = 1
    Container.ScrollBarThickness = 3
    Container.ScrollBarImageColor3 = THEME.Primary
    Container.Parent = MainFrame
    
    -- Layout do container
    local layout = Instance.new("UIListLayout")
    layout.Padding = UDim.new(0, 8)
    layout.Parent = Container
    
    -- Sistema de arrastar
    local dragging, dragStart, startPos
    
    TitleBar.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
            dragStart = input.Position
            startPos = MainFrame.Position
        end
    end)
    
    UserInputService.InputChanged:Connect(function(input)
        if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
            local delta = input.Position - dragStart
            MainFrame.Position = UDim2.new(
                startPos.X.Scale,
                startPos.X.Offset + delta.X,
                startPos.Y.Scale,
                startPos.Y.Offset + delta.Y
            )
        end
    end)
    
    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = false
        end
    end)
    
    -- Sistema de elementos
    local elements = {}
    local currentY = 0
    
    -- Funções para criar elementos (igual ao código anterior mas com melhorias visuais)
    -- [Continua no próximo bloco devido ao limite de caracteres]
    -- Função para criar Slider Profissional
    function elements:CreateSlider(config)
        config = config or {}
        local title = config.title or "Slider"
        local min = config.min or 0
        local max = config.max or 100
        local default = config.default or min
        local callback = config.callback or function() end
        
        local SliderFrame = Instance.new("Frame")
        SliderFrame.Size = UDim2.new(1, -10, 0, 50)
        SliderFrame.BackgroundColor3 = THEME.LightBG
        SliderFrame.BorderSizePixel = 0
        
        local corner = Instance.new("UICorner")
        corner.CornerRadius = UDim.new(0, 6)
        corner.Parent = SliderFrame
        
        local SliderTitle = Instance.new("TextLabel")
        SliderTitle.Size = UDim2.new(1, -20, 0, 20)
        SliderTitle.Position = UDim2.new(0, 10, 0, 5)
        SliderTitle.BackgroundTransparency = 1
        SliderTitle.Text = title
        SliderTitle.TextColor3 = THEME.Text
        SliderTitle.TextSize = 14
        SliderTitle.Font = Enum.Font.GothamSemibold
        SliderTitle.TextXAlignment = Enum.TextXAlignment.Left
        SliderTitle.Parent = SliderFrame
        
        local ValueDisplay = Instance.new("TextLabel")
        ValueDisplay.Size = UDim2.new(0, 50, 0, 20)
        ValueDisplay.Position = UDim2.new(1, -60, 0, 5)
        ValueDisplay.BackgroundTransparency = 1
        ValueDisplay.Text = tostring(default)
        ValueDisplay.TextColor3 = THEME.Primary
        ValueDisplay.TextSize = 14
        ValueDisplay.Font = Enum.Font.GothamBold
        ValueDisplay.Parent = SliderFrame
        
        local SliderBar = Instance.new("Frame")
        SliderBar.Size = UDim2.new(1, -20, 0, 4)
        SliderBar.Position = UDim2.new(0, 10, 0, 35)
        SliderBar.BackgroundColor3 = THEME.DarkBG
        SliderBar.BorderSizePixel = 0
        SliderBar.Parent = SliderFrame
        
        local sliderCorner = Instance.new("UICorner")
        sliderCorner.CornerRadius = UDim.new(1, 0)
        sliderCorner.Parent = SliderBar
        
        local Progress = Instance.new("Frame")
        Progress.Size = UDim2.new((default - min)/(max - min), 0, 1, 0)
        Progress.BackgroundColor3 = THEME.Primary
        Progress.BorderSizePixel = 0
        Progress.Parent = SliderBar
        
        local progressCorner = Instance.new("UICorner")
        progressCorner.CornerRadius = UDim.new(1, 0)
        progressCorner.Parent = Progress
        
        local SliderButton = Instance.new("TextButton")
        SliderButton.Size = UDim2.new(0, 16, 0, 16)
        SliderButton.Position = UDim2.new((default - min)/(max - min), -8, 0, -6)
        SliderButton.BackgroundColor3 = THEME.Primary
        SliderButton.Text = ""
        SliderButton.Parent = SliderBar
        
        local buttonCorner = Instance.new("UICorner")
        buttonCorner.CornerRadius = UDim.new(1, 0)
        buttonCorner.Parent = SliderButton
        
        local buttonGlow = Instance.new("ImageLabel")
        buttonGlow.Size = UDim2.new(1.5, 0, 1.5, 0)
        buttonGlow.Position = UDim2.new(-0.25, 0, -0.25, 0)
        buttonGlow.BackgroundTransparency = 1
        buttonGlow.Image = "rbxassetid://7912134082"
        buttonGlow.ImageColor3 = THEME.Primary
        buttonGlow.ImageTransparency = 0.5
        buttonGlow.Parent = SliderButton
        
        -- Funcionalidade do Slider
        local dragging = false
        local function updateSlider(input)
            local pos = math.clamp((input.Position.X - SliderBar.AbsolutePosition.X) / SliderBar.AbsoluteSize.X, 0, 1)
            local value = math.floor(min + (max - min) * pos)
            
            TweenService:Create(Progress, ANIMATION.FastTween, {
                Size = UDim2.new(pos, 0, 1, 0)
            }):Play()
            
            TweenService:Create(SliderButton, ANIMATION.FastTween, {
                Position = UDim2.new(pos, -8, 0, -6)
            }):Play()
            
            ValueDisplay.Text = tostring(value)
            callback(value)
        end
        
        SliderButton.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 then
                dragging = true
            end
        end)
        
        UserInputService.InputChanged:Connect(function(input)
            if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
                updateSlider(input)
            end
        end)
        
        UserInputService.InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 then
                dragging = false
            end
        end)
        
        -- Hover Effects
        SliderButton.MouseEnter:Connect(function()
            TweenService:Create(buttonGlow, ANIMATION.FastTween, {
                ImageTransparency = 0.3
            }):Play()
        end)
        
        SliderButton.MouseLeave:Connect(function()
            TweenService:Create(buttonGlow, ANIMATION.FastTween, {
                ImageTransparency = 0.5
            }):Play()
        end)
        
        SliderFrame.Parent = Container
        return SliderFrame
    end
    
    -- Função para criar Dropdown Profissional
    function elements:CreateDropdown(config)
        config = config or {}
        local title = config.title or "Dropdown"
        local options = config.options or {}
        local default = config.default
        local callback = config.callback or function() end
        
        local DropdownFrame = Instance.new("Frame")
        DropdownFrame.Size = UDim2.new(1, -10, 0, 40)
        DropdownFrame.BackgroundColor3 = THEME.LightBG
        DropdownFrame.BorderSizePixel = 0
        
        local corner = Instance.new("UICorner")
        corner.CornerRadius = UDim.new(0, 6)
        corner.Parent = DropdownFrame
        
        local DropdownButton = Instance.new("TextButton")
        DropdownButton.Size = UDim2.new(1, 0, 1, 0)
        DropdownButton.BackgroundTransparency = 1
        DropdownButton.Text = ""
        DropdownButton.Parent = DropdownFrame
        
        local Title = Instance.new("TextLabel")
        Title.Size = UDim2.new(1, -40, 0, 20)
        Title.Position = UDim2.new(0, 10, 0.5, -10)
        Title.BackgroundTransparency = 1
        Title.Text = title
        Title.TextColor3 = THEME.Text
        Title.TextSize = 14
        Title.Font = Enum.Font.GothamSemibold
        Title.TextXAlignment = Enum.TextXAlignment.Left
        Title.Parent = DropdownFrame
        
        local Arrow = Instance.new("ImageLabel")
        Arrow.Size = UDim2.new(0, 20, 0, 20)
        Arrow.Position = UDim2.new(1, -25, 0.5, -10)
        Arrow.BackgroundTransparency = 1
        Arrow.Image = "rbxassetid://6034818372"
        Arrow.ImageColor3 = THEME.Primary
        Arrow.Parent = DropdownFrame
        
        local OptionsFrame = Instance.new("Frame")
        OptionsFrame.Size = UDim2.new(1, 0, 0, 0)
        OptionsFrame.Position = UDim2.new(0, 0, 1, 5)
        OptionsFrame.BackgroundColor3 = THEME.LightBG
        OptionsFrame.BorderSizePixel = 0
        OptionsFrame.ClipsDescendants = true
        OptionsFrame.Visible = false
        OptionsFrame.Parent = DropdownFrame
        
        local optionsCorner = Instance.new("UICorner")
        optionsCorner.CornerRadius = UDim.new(0, 6)
        optionsCorner.Parent = OptionsFrame
        
        local OptionsList = Instance.new("UIListLayout")
        OptionsList.Padding = UDim.new(0, 5)
        OptionsList.Parent = OptionsFrame
        
        -- Continua na parte 3...
        -- Continuação do Dropdown
        local selected = default
        local open = false
        
        local function updateDropdown()
            for _, option in ipairs(options) do
                local OptionButton = Instance.new("TextButton")
                OptionButton.Size = UDim2.new(1, -10, 0, 30)
                OptionButton.Position = UDim2.new(0, 5, 0, 5)
                OptionButton.BackgroundColor3 = THEME.DarkBG
                OptionButton.Text = option
                OptionButton.TextColor3 = option == selected and THEME.Primary or THEME.Text
                OptionButton.TextSize = 14
                OptionButton.Font = Enum.Font.Gotham
                OptionButton.Parent = OptionsFrame
                
                local optionCorner = Instance.new("UICorner")
                optionCorner.CornerRadius = UDim.new(0, 4)
                optionCorner.Parent = OptionButton
                
                OptionButton.MouseButton1Click:Connect(function()
                    selected = option
                    Title.Text = title .. ": " .. option
                    callback(option)
                    
                    -- Fechar dropdown
                    TweenService:Create(Arrow, ANIMATION.FastTween, {
                        Rotation = 0
                    }):Play()
                    
                    TweenService:Create(OptionsFrame, ANIMATION.FastTween, {
                        Size = UDim2.new(1, 0, 0, 0)
                    }):Play()
                    
                    wait(0.2)
                    OptionsFrame.Visible = false
                    open = false
                end)
                
                -- Hover effect
                OptionButton.MouseEnter:Connect(function()
                    TweenService:Create(OptionButton, ANIMATION.FastTween, {
                        BackgroundColor3 = THEME.Secondary
                    }):Play()
                end)
                
                OptionButton.MouseLeave:Connect(function()
                    TweenService:Create(OptionButton, ANIMATION.FastTween, {
                        BackgroundColor3 = THEME.DarkBG
                    }):Play()
                end)
            end
        end
        
        DropdownButton.MouseButton1Click:Connect(function()
            open = not open
            
            if open then
                OptionsFrame.Visible = true
                TweenService:Create(Arrow, ANIMATION.FastTween, {
                    Rotation = 180
                }):Play()
                
                TweenService:Create(OptionsFrame, ANIMATION.FastTween, {
                    Size = UDim2.new(1, 0, 0, #options * 35 + 10)
                }):Play()
            else
                TweenService:Create(Arrow, ANIMATION.FastTween, {
                    Rotation = 0
                }):Play()
                
                TweenService:Create(OptionsFrame, ANIMATION.FastTween, {
                    Size = UDim2.new(1, 0, 0, 0)
                }):Play()
                
                wait(0.2)
                OptionsFrame.Visible = false
            end
        end)
        
        updateDropdown()
        DropdownFrame.Parent = Container
        return DropdownFrame
    end
    
    -- Função para criar Toggle Profissional
    function elements:CreateToggle(config)
        config = config or {}
        local title = config.title or "Toggle"
        local default = config.default or false
        local callback = config.callback or function() end
        
        local ToggleFrame = Instance.new("Frame")
        ToggleFrame.Size = UDim2.new(1, -10, 0, 40)
        ToggleFrame.BackgroundColor3 = THEME.LightBG
        ToggleFrame.BorderSizePixel = 0
        
        local corner = Instance.new("UICorner")
        corner.CornerRadius = UDim.new(0, 6)
        corner.Parent = ToggleFrame
        
        local Title = Instance.new("TextLabel")
        Title.Size = UDim2.new(1, -60, 1, 0)
        Title.Position = UDim2.new(0, 10, 0, 0)
        Title.BackgroundTransparency = 1
        Title.Text = title
        Title.TextColor3 = THEME.Text
        Title.TextSize = 14
        Title.Font = Enum.Font.GothamSemibold
        Title.TextXAlignment = Enum.TextXAlignment.Left
        Title.Parent = ToggleFrame
        
        local ToggleButton = Instance.new("TextButton")
        ToggleButton.Size = UDim2.new(0, 40, 0, 24)
        ToggleButton.Position = UDim2.new(1, -50, 0.5, -12)
        ToggleButton.BackgroundColor3 = default and THEME.Primary or THEME.DarkBG
        ToggleButton.BorderSizePixel = 0
        ToggleButton.Text = ""
        ToggleButton.Parent = ToggleFrame
        
        local toggleCorner = Instance.new("UICorner")
        toggleCorner.CornerRadius = UDim.new(1, 0)
        toggleCorner.Parent = ToggleButton
        
        local Circle = Instance.new("Frame")
        Circle.Size = UDim2.new(0, 20, 0, 20)
        Circle.Position = default and UDim2.new(1, -22, 0.5, -10) or UDim2.new(0, 2, 0.5, -10)
        Circle.BackgroundColor3 = THEME.Text
        Circle.BorderSizePixel = 0
        Circle.Parent = ToggleButton
        
        local circleCorner = Instance.new("UICorner")
        circleCorner.CornerRadius = UDim.new(1, 0)
        circleCorner.Parent = Circle
        local enabled = default
        
        ToggleButton.MouseButton1Click:Connect(function()
            enabled = not enabled
            
            local pos = enabled and UDim2.new(1, -22, 0.5, -10) or UDim2.new(0, 2, 0.5, -10)
            local color = enabled and THEME.Primary or THEME.DarkBG
            
            TweenService:Create(Circle, ANIMATION.FastTween, {
                Position = pos
            }):Play()
            
            TweenService:Create(ToggleButton, ANIMATION.FastTween, {
                BackgroundColor3 = color
            }):Play()
            
            callback(enabled)
        end)
        
        -- Hover effect
        ToggleButton.MouseEnter:Connect(function()
            TweenService:Create(Circle, ANIMATION.FastTween, {
                Size = UDim2.new(0, 22, 0, 22),
                Position = enabled and UDim2.new(1, -23, 0.5, -11) or UDim2.new(0, 1, 0.5, -11)
            }):Play()
        end)
        
        ToggleButton.MouseLeave:Connect(function()
            TweenService:Create(Circle, ANIMATION.FastTween, {
                Size = UDim2.new(0, 20, 0, 20),
                Position = enabled and UDim2.new(1, -22, 0.5, -10) or UDim2.new(0, 2, 0.5, -10)
            }):Play()
        end)
        
        ToggleFrame.Parent = Container
        return ToggleFrame
    end
    
    -- Função para criar Keybind Profissional
    function elements:CreateKeybind(config)
        config = config or {}
        local title = config.title or "Keybind"
        local default = config.default or Enum.KeyCode.Unknown
        local callback = config.callback or function() end
        
        local KeybindFrame = Instance.new("Frame")
        KeybindFrame.Size = UDim2.new(1, -10, 0, 40)
        KeybindFrame.BackgroundColor3 = THEME.LightBG
        KeybindFrame.BorderSizePixel = 0
        
        local corner = Instance.new("UICorner")
        corner.CornerRadius = UDim.new(0, 6)
        corner.Parent = KeybindFrame
        
        local Title = Instance.new("TextLabel")
        Title.Size = UDim2.new(1, -110, 1, 0)
        Title.Position = UDim2.new(0, 10, 0, 0)
        Title.BackgroundTransparency = 1
        Title.Text = title
        Title.TextColor3 = THEME.Text
        Title.TextSize = 14
        Title.Font = Enum.Font.GothamSemibold
        Title.TextXAlignment = Enum.TextXAlignment.Left
        Title.Parent = KeybindFrame
        
        local KeybindButton = Instance.new("TextButton")
        KeybindButton.Size = UDim2.new(0, 90, 0, 30)
        KeybindButton.Position = UDim2.new(1, -100, 0.5, -15)
        KeybindButton.BackgroundColor3 = THEME.DarkBG
        KeybindButton.BorderSizePixel = 0
        KeybindButton.Text = default.Name
        KeybindButton.TextColor3 = THEME.Text
        KeybindButton.TextSize = 14
        KeybindButton.Font = Enum.Font.GothamBold
        KeybindButton.Parent = KeybindFrame
        
        local buttonCorner = Instance.new("UICorner")
        buttonCorner.CornerRadius = UDim.new(0, 6)
        buttonCorner.Parent = KeybindButton
        
        local binding = false
        local selectedKey = default
        
        KeybindButton.MouseButton1Click:Connect(function()
            binding = true
            KeybindButton.Text = "..."
            
            -- Efeito visual
            TweenService:Create(KeybindButton, ANIMATION.FastTween, {
                BackgroundColor3 = THEME.Primary,
                TextColor3 = THEME.Text
            }):Play()
        end)
        
        UserInputService.InputBegan:Connect(function(input)
            if binding and input.UserInputType == Enum.UserInputType.Keyboard then
                binding = false
                selectedKey = input.KeyCode
                KeybindButton.Text = selectedKey.Name
                
                -- Resetar visual
                TweenService:Create(KeybindButton, ANIMATION.FastTween, {
                    BackgroundColor3 = THEME.DarkBG,
                    TextColor3 = THEME.Text
                }):Play()
                
                callback(selectedKey)
            elseif not binding and input.KeyCode == selectedKey then
                callback(selectedKey)
            end
        end)
        
        -- Hover effect
        KeybindButton.MouseEnter:Connect(function()
            if not binding then
                TweenService:Create(KeybindButton, ANIMATION.FastTween, {
                    BackgroundColor3 = THEME.Secondary
                }):Play()
            end
        end)
        
        KeybindButton.MouseLeave:Connect(function()
            if not binding then
                TweenService:Create(KeybindButton, ANIMATION.FastTween, {
                    BackgroundColor3 = THEME.DarkBG
                }):Play()
            end
        end)
        
        KeybindFrame.Parent = Container
        return KeybindFrame
    end
    
    -- Funções adicionais para melhorar a UI
    
    -- Criar Separador
    function elements:CreateSeparator()
        local SeparatorFrame = Instance.new("Frame")
        SeparatorFrame.Size = UDim2.new(1, -20, 0, 1)
        SeparatorFrame.BackgroundColor3 = THEME.Primary
        SeparatorFrame.BackgroundTransparency = 0.7
        SeparatorFrame.BorderSizePixel = 0
        SeparatorFrame.Parent = Container
        return SeparatorFrame
    end
    
    -- Criar Título de Seção
    function elements:CreateSection(title)
        local SectionFrame = Instance.new("Frame")
        SectionFrame.Size = UDim2.new(1, -10, 0, 30)
        SectionFrame.BackgroundTransparency = 1
        
        local SectionTitle = Instance.new("TextLabel")
        SectionTitle.Size = UDim2.new(1, 0, 1, 0)
        SectionTitle.BackgroundTransparency = 1
        SectionTitle.Text = title
        SectionTitle.TextColor3 = THEME.Primary
        SectionTitle.TextSize = 16
        SectionTitle.Font = Enum.Font.GothamBold
        SectionTitle.Parent = SectionFrame
        
        SectionFrame.Parent = Container
        return SectionFrame
    end
    
    -- Criar Botão
    function elements:CreateButton(config)
        config = config or {}
        local title = config.title or "Button"
        local callback = config.callback or function() end
        
        local ButtonFrame = Instance.new("Frame")
        ButtonFrame.Size = UDim2.new(1, -10, 0, 40)
        ButtonFrame.BackgroundColor3 = THEME.LightBG
        ButtonFrame.BorderSizePixel = 0
        
        local corner = Instance.new("UICorner")
        corner.CornerRadius = UDim.new(0, 6)
        corner.Parent = ButtonFrame
        
        local Button = Instance.new("TextButton")
        Button.Size = UDim2.new(1, 0, 1, 0)
        Button.BackgroundTransparency = 1
        Button.Text = title
        Button.TextColor3 = THEME.Text
        Button.TextSize = 14
        Button.Font = Enum.Font.GothamBold
        Button.Parent = ButtonFrame
        
        -- Efeitos do botão
        Button.MouseEnter:Connect(function()
            TweenService:Create(ButtonFrame, ANIMATION.FastTween, {
                BackgroundColor3 = THEME.Primary
            }):Play()
        end)
        
        Button.MouseLeave:Connect(function()
            TweenService:Create(ButtonFrame, ANIMATION.FastTween, {
                BackgroundColor3 = THEME.LightBG
            }):Play()
        end)
        
        Button.MouseButton1Down:Connect(function()
            TweenService:Create(ButtonFrame, ANIMATION.FastTween, {
                BackgroundColor3 = THEME.Secondary
            }):Play()
        end)
        
        Button.MouseButton1Up:Connect(function()
            TweenService:Create(ButtonFrame, ANIMATION.FastTween, {
                BackgroundColor3 = THEME.Primary
            }):Play()
            callback()
        end)
        
        ButtonFrame.Parent = Container
        return ButtonFrame
    end
    
    return elements
end

-- Retornar a biblioteca
return Solara
