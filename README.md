-- Solara Library
-- Uma biblioteca GUI elegante para Roblox

local Solara = {}
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")

-- Configurações de cores
local THEME = {
    Background = Color3.fromRGB(30, 30, 40),
    Primary = Color3.fromRGB(130, 90, 240),
    Secondary = Color3.fromRGB(80, 60, 170),
    Text = Color3.fromRGB(255, 255, 255),
    Border = Color3.fromRGB(60, 50, 80)
}

-- Função para criar a janela principal
function Solara.CreateWindow(title)
    local ScreenGui = Instance.new("ScreenGui")
    local MainFrame = Instance.new("Frame")
    local TitleBar = Instance.new("Frame")
    local TitleText = Instance.new("TextLabel")
    local Container = Instance.new("ScrollingFrame")
    
    -- Configuração do ScreenGui
    ScreenGui.Name = "SolaraLibrary"
    ScreenGui.Parent = game.CoreGui
    
    -- Configuração do Frame principal
    MainFrame.Name = "MainFrame"
    MainFrame.Size = UDim2.new(0, 300, 0, 350)
    MainFrame.Position = UDim2.new(0.5, -150, 0.5, -175)
    MainFrame.BackgroundColor3 = THEME.Background
    MainFrame.BorderSizePixel = 0
    MainFrame.Parent = ScreenGui
    
    -- Barra de título
    TitleBar.Name = "TitleBar"
    TitleBar.Size = UDim2.new(1, 0, 0, 30)
    TitleBar.BackgroundColor3 = THEME.Primary
    TitleBar.BorderSizePixel = 0
    TitleBar.Parent = MainFrame
    
    TitleText.Name = "TitleText"
    TitleText.Size = UDim2.new(1, 0, 1, 0)
    TitleText.BackgroundTransparency = 1
    TitleText.Text = title
    TitleText.TextColor3 = THEME.Text
    TitleText.TextSize = 16
    TitleText.Font = Enum.Font.GothamBold
    TitleText.Parent = TitleBar
    
    -- Container para elementos
    Container.Name = "Container"
    Container.Position = UDim2.new(0, 0, 0, 35)
    Container.Size = UDim2.new(1, 0, 1, -35)
    Container.BackgroundTransparency = 1
    Container.ScrollBarThickness = 4
    Container.Parent = MainFrame
    
    -- Tornar a janela arrastável
    local dragging = false
    local dragStart = nil
    local startPos = nil
    
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
    
    local window = {}
    local elementSpacing = 10
    local currentY = 10
    
    -- Função para criar Slider
    function window:CreateSlider(text, min, max, default)
        local SliderFrame = Instance.new("Frame")
        local SliderText = Instance.new("TextLabel")
        local SliderBar = Instance.new("Frame")
        local SliderButton = Instance.new("TextButton")
        local ValueText = Instance.new("TextLabel")
        
        SliderFrame.Name = "SliderFrame"
        SliderFrame.Size = UDim2.new(0.9, 0, 0, 50)
        SliderFrame.Position = UDim2.new(0.05, 0, 0, currentY)
        SliderFrame.BackgroundTransparency = 1
        SliderFrame.Parent = Container
        
        SliderText.Name = "SliderText"
        SliderText.Size = UDim2.new(1, 0, 0, 20)
        SliderText.BackgroundTransparency = 1
        SliderText.Text = text
        SliderText.TextColor3 = THEME.Text
        SliderText.TextSize = 14
        SliderText.Font = Enum.Font.Gotham
        SliderText.TextXAlignment = Enum.TextXAlignment.Left
        SliderText.Parent = SliderFrame
        
        SliderBar.Name = "SliderBar"
        SliderBar.Size = UDim2.new(1, 0, 0, 4)
        SliderBar.Position = UDim2.new(0, 0, 0.7, 0)
        SliderBar.BackgroundColor3 = THEME.Secondary
        SliderBar.BorderSizePixel = 0
        SliderBar.Parent = SliderFrame
        
        SliderButton.Name = "SliderButton"
        SliderButton.Size = UDim2.new(0, 16, 0, 16)
        SliderButton.Position = UDim2.new((default - min)/(max - min), -8, 0.7, -6)
        SliderButton.BackgroundColor3 = THEME.Primary
        SliderButton.BorderSizePixel = 0
        SliderButton.Text = ""
        SliderButton.Parent = SliderFrame
        
        ValueText.Name = "ValueText"
        ValueText.Size = UDim2.new(0, 50, 0, 20)
        ValueText.Position = UDim2.new(1, -50, 0, 0)
        ValueText.BackgroundTransparency = 1
        ValueText.Text = tostring(default)
        ValueText.TextColor3 = THEME.Text
        ValueText.TextSize = 14
        ValueText.Font = Enum.Font.Gotham
        ValueText.Parent = SliderFrame
        
        local dragging = false
        local value = default
        
        local function updateSlider(input)
            local pos = math.clamp((input.Position.X - SliderBar.AbsolutePosition.X) / SliderBar.AbsoluteSize.X, 0, 1)
            value = math.floor(min + (max - min) * pos)
            ValueText.Text = tostring(value)
            SliderButton.Position = UDim2.new(pos, -8, 0.7, -6)
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
        
        currentY = currentY + 60
        Container.CanvasSize = UDim2.new(0, 0, 0, currentY)
        
        return {
            GetValue = function()
                return value
            end
        }
    end
    
    -- Função para criar Dropdown
    function window:CreateDropdown(text, options)
        local DropdownFrame = Instance.new("Frame")
        local DropdownButton = Instance.new("TextButton")
        local OptionsList = Instance.new("Frame")
        
        DropdownFrame.Name = "DropdownFrame"
        DropdownFrame.Size = UDim2.new(0.9, 0, 0, 30)
        DropdownFrame.Position = UDim2.new(0.05, 0, 0, currentY)
        DropdownFrame.BackgroundTransparency = 1
        DropdownFrame.Parent = Container
        
        DropdownButton.Name = "DropdownButton"
        DropdownButton.Size = UDim2.new(1, 0, 1, 0)
        DropdownButton.BackgroundColor3 = THEME.Secondary
        DropdownButton.BorderSizePixel = 0
        DropdownButton.Text = text
        DropdownButton.TextColor3 = THEME.Text
        DropdownButton.TextSize = 14
        DropdownButton.Font = Enum.Font.Gotham
        DropdownButton.Parent = DropdownFrame
        
        OptionsList.Name = "OptionsList"
        OptionsList.Size = UDim2.new(1, 0, 0, #options * 25)
        OptionsList.Position = UDim2.new(0, 0, 1, 0)
        OptionsList.BackgroundColor3 = THEME.Secondary
        OptionsList.BorderSizePixel = 0
        OptionsList.Visible = false
        OptionsList.Parent = DropdownFrame
        
        local selectedOption = nil
        
        for i, option in ipairs(options) do
            local OptionButton = Instance.new("TextButton")
            OptionButton.Size = UDim2.new(1, 0, 0, 25)
            OptionButton.Position = UDim2.new(0, 0, 0, (i-1) * 25)
            OptionButton.BackgroundColor3 = THEME.Secondary
            OptionButton.BorderSizePixel = 0
            OptionButton.Text = option
            OptionButton.TextColor3 = THEME.Text
            OptionButton.TextSize = 14
            OptionButton.Font = Enum.Font.Gotham
            OptionButton.Parent = OptionsList
            
            OptionButton.MouseButton1Click:Connect(function()
                selectedOption = option
                DropdownButton.Text = text .. ": " .. option
                OptionsList.Visible = false
            end)
        end
        
        DropdownButton.MouseButton1Click:Connect(function()
            OptionsList.Visible = not OptionsList.Visible
        end)
        
        currentY = currentY + 40
        Container.CanvasSize = UDim2.new(0, 0, 0, currentY)
        
        return {
            GetSelected = function()
                return selectedOption
            end
        }
    end
    
    -- Função para criar Toggle Switch
    function window:CreateToggle(text)
        local ToggleFrame = Instance.new("Frame")
        local ToggleText = Instance.new("TextLabel")
        local ToggleButton = Instance.new("TextButton")
        local ToggleIndicator = Instance.new("Frame")
        
        ToggleFrame.Name = "ToggleFrame"
        ToggleFrame.Size = UDim2.new(0.9, 0, 0, 30)
        ToggleFrame.Position = UDim2.new(0.05, 0, 0, currentY)
        ToggleFrame.BackgroundTransparency = 1
        ToggleFrame.Parent = Container
        
        ToggleText.Name = "ToggleText"
        ToggleText.Size = UDim2.new(0.7, 0, 1, 0)
        ToggleText.BackgroundTransparency = 1
        ToggleText.Text = text
        ToggleText.TextColor3 = THEME.Text
        ToggleText.TextSize = 14
        ToggleText.Font = Enum.Font.Gotham
        ToggleText.TextXAlignment = Enum.TextXAlignment.Left
        ToggleText.Parent = ToggleFrame
        
        ToggleButton.Name = "ToggleButton"
        ToggleButton.Size = UDim2.new(0, 44, 0, 24)
        ToggleButton.Position = UDim2.new(1, -44, 0.5, -12)
        ToggleButton.BackgroundColor3 = THEME.Secondary
        ToggleButton.BorderSizePixel = 0
        ToggleButton.Text = ""
        ToggleButton.Parent = ToggleFrame
        
        ToggleIndicator.Name = "ToggleIndicator"
        ToggleIndicator.Size = UDim2.new(0, 20, 0, 20)
        ToggleIndicator.Position = UDim2.new(0, 2, 0.5, -10)
        ToggleIndicator.BackgroundColor3 = THEME.Text
        ToggleIndicator.BorderSizePixel = 0
        ToggleIndicator.Parent = ToggleButton
        
        local enabled = false
        local tweenInfo = TweenInfo.new(0.2)
        
        ToggleButton.MouseButton1Click:Connect(function()
            enabled = not enabled
            local position = enabled and UDim2.new(1, -22, 0.5, -10) or UDim2.new(0, 2, 0.5, -10)
            local color = enabled and THEME.Primary or THEME.Secondary
            
            TweenService:Create(ToggleIndicator, tweenInfo, {Position = position}):Play()
            TweenService:Create(ToggleButton, tweenInfo, {BackgroundColor3 = color}):Play()
        end)
        
        currentY = currentY + 40
        Container.CanvasSize = UDim2.new(0, 0, 0, currentY)
        
        return {
            GetState = function()
                return enabled
            end
        }
    end
    
    -- Função para criar Key Bind
    function window:CreateKeybind(text, default)
        local KeybindFrame = Instance.new("Frame")
        local KeybindText = Instance.new("TextLabel")
        local KeybindButton = Instance.new("TextButton")
        
        KeybindFrame.Name = "KeybindFrame"
        KeybindFrame.Size = UDim2.new(0.9, 0, 0, 30)
        KeybindFrame.Position = UDim2.new(0.05, 0, 0, currentY)
        KeybindFrame.BackgroundTransparency = 1
        KeybindFrame.Parent = Container
        
        KeybindText.Name = "KeybindText"
        KeybindText.Size = UDim2.new(0.7, 0, 1, 0)
        KeybindText.BackgroundTransparency = 1
        KeybindText.Text = text
        KeybindText.TextColor3 = THEME.Text
        KeybindText.TextSize = 14
        KeybindText.Font = Enum.Font.Gotham
        KeybindText.TextXAlignment = Enum.TextXAlignment.Left
        KeybindText.Parent = KeybindFrame
        
        KeybindButton.Name = "KeybindButton"
        KeybindButton.Size = UDim2.new(0, 100, 0, 24)
        KeybindButton.Position = UDim2.new(1, -100, 0.5, -12)
        KeybindButton.BackgroundColor3 = THEME.Secondary
        KeybindButton.BorderSizePixel = 0
        KeybindButton.Text = default and default.Name or "..."
        KeybindButton.TextColor3 = THEME.Text
        KeybindButton.TextSize = 14
        KeybindButton.Font = Enum.Font.Gotham
        KeybindButton.Parent = KeybindFrame
        
        local selectedKey = default
        local listening = false
        
        KeybindButton.MouseButton1Click:Connect(function()
            listening = true
            KeybindButton.Text = "..."
        end)
        
        UserInputService.InputBegan:Connect(function(input)
            if listening and input.UserInputType == Enum.UserInputType.Keyboard then
                selectedKey = input.KeyCode
                KeybindButton.Text = selectedKey.Name
                listening = false
            end
        end)
        
        currentY = currentY + 40
        Container.CanvasSize = UDim2.new(0, 0, 0, currentY)
        
        return {
            GetKey = function()
                return selectedKey
            end
        }
    end
    
    -- Função para adicionar espaçamento
    function window:AddSpace(pixels)
        currentY = currentY + (pixels or 10)
        Container.CanvasSize = UDim2.new(0, 0, 0, currentY)
    end
    
    -- Função para criar uma label
    function window:CreateLabel(text)
        local LabelFrame = Instance.new("Frame")
        local Label = Instance.new("TextLabel")
        
        LabelFrame.Name = "LabelFrame"
        LabelFrame.Size = UDim2.new(0.9, 0, 0, 25)
        LabelFrame.Position = UDim2.new(0.05, 0, 0, currentY)
        LabelFrame.BackgroundTransparency = 1
        LabelFrame.Parent = Container
        
        Label.Name = "Label"
        Label.Size = UDim2.new(1, 0, 1, 0)
        Label.BackgroundTransparency = 1
        Label.Text = text
        Label.TextColor3 = THEME.Text
        Label.TextSize = 14
        Label.Font = Enum.Font.GothamBold
        Label.TextXAlignment = Enum.TextXAlignment.Left
        Label.Parent = LabelFrame
        
        currentY = currentY + 35
        Container.CanvasSize = UDim2.new(0, 0, 0, currentY)
    end
    
    -- Função para criar um botão
    function window:CreateButton(text, callback)
        local ButtonFrame = Instance.new("Frame")
        local Button = Instance.new("TextButton")
        
        ButtonFrame.Name = "ButtonFrame"
        ButtonFrame.Size = UDim2.new(0.9, 0, 0, 35)
        ButtonFrame.Position = UDim2.new(0.05, 0, 0, currentY)
        ButtonFrame.BackgroundTransparency = 1
        ButtonFrame.Parent = Container
        
        Button.Name = "Button"
        Button.Size = UDim2.new(1, 0, 1, 0)
        Button.BackgroundColor3 = THEME.Primary
        Button.BorderSizePixel = 0
        Button.Text = text
        Button.TextColor3 = THEME.Text
        Button.TextSize = 14
        Button.Font = Enum.Font.GothamBold
        Button.Parent = ButtonFrame
        
        -- Efeito hover e clique
        local originalColor = Button.BackgroundColor3
        
        Button.MouseEnter:Connect(function()
            TweenService:Create(Button, TweenInfo.new(0.2), {
                BackgroundColor3 = THEME.Secondary
            }):Play()
        end)
        
        Button.MouseLeave:Connect(function()
            TweenService:Create(Button, TweenInfo.new(0.2), {
                BackgroundColor3 = originalColor
            }):Play()
        end)
        
        Button.MouseButton1Down:Connect(function()
            TweenService:Create(Button, TweenInfo.new(0.1), {
                BackgroundColor3 = THEME.Border
            }):Play()
        end)
        
        Button.MouseButton1Up:Connect(function()
            TweenService:Create(Button, TweenInfo.new(0.1), {
                BackgroundColor3 = THEME.Secondary
            }):Play()
            if callback then
                callback()
            end
        end)
        
        currentY = currentY + 45
        Container.CanvasSize = UDim2.new(0, 0, 0, currentY)
    end
    
    return window
end

return Solara
