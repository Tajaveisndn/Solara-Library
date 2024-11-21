-- Nyox Library
local NyoxLibrary = {}

-- Configurações da UI
local config = {
    MainColor = Color3.fromRGB(147, 51, 234), -- Roxo
    BackgroundColor = Color3.fromRGB(32, 32, 32),
    TextColor = Color3.fromRGB(255, 255, 255),
    Font = Enum.Font.Gotham,
    ToggleSpeed = 0.3,
}

-- Criar a janela principal
function NyoxLibrary:CreateWindow(name)
    local NyoxGui = Instance.new("ScreenGui")
    local Main = Instance.new("Frame")
    local UICorner = Instance.new("UICorner")
    local TopBar = Instance.new("Frame")
    local Title = Instance.new("TextLabel")
    local CloseButton = Instance.new("TextButton")
    local MinimizeButton = Instance.new("TextButton")
    local TabHolder = Instance.new("Frame")
    local TabContainer = Instance.new("Frame")
    local UIListLayout = Instance.new("UIListLayout")
    local ContentContainer = Instance.new("Frame")

    -- Propriedades da UI principal
    NyoxGui.Name = "NyoxLibrary"
    NyoxGui.Parent = game.CoreGui
    NyoxGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

    Main.Name = "Main"
    Main.Parent = NyoxGui
    Main.BackgroundColor3 = config.BackgroundColor
    Main.Position = UDim2.new(0.5, -300, 0.5, -200)
    Main.Size = UDim2.new(0, 600, 0, 400)
    Main.ClipsDescendants = true
    Main.Active = true
    Main.Draggable = true

    UICorner.Parent = Main
    UICorner.CornerRadius = UDim.new(0, 8)

    -- Barra superior
    TopBar.Name = "TopBar"
    TopBar.Parent = Main
    TopBar.BackgroundColor3 = config.MainColor
    TopBar.Size = UDim2.new(1, 0, 0, 30)

    Title.Name = "Title"
    Title.Parent = TopBar
    Title.BackgroundTransparency = 1
    Title.Position = UDim2.new(0, 10, 0, 0)
    Title.Size = UDim2.new(1, -80, 1, 0)
    Title.Font = config.Font
    Title.Text = name
    Title.TextColor3 = config.TextColor
    Title.TextSize = 16
    Title.TextXAlignment = Enum.TextXAlignment.Left

    -- Botões de controle
    CloseButton.Name = "CloseButton"
    CloseButton.Parent = TopBar
    CloseButton.BackgroundTransparency = 1
    CloseButton.Position = UDim2.new(1, -30, 0, 0)
    CloseButton.Size = UDim2.new(0, 30, 1, 0)
    CloseButton.Font = config.Font
    CloseButton.Text = "×"
    CloseButton.TextColor3 = config.TextColor
    CloseButton.TextSize = 20

    MinimizeButton.Name = "MinimizeButton"
    MinimizeButton.Parent = TopBar
    MinimizeButton.BackgroundTransparency = 1
    MinimizeButton.Position = UDim2.new(1, -60, 0, 0)
    MinimizeButton.Size = UDim2.new(0, 30, 1, 0)
    MinimizeButton.Font = config.Font
    MinimizeButton.Text = "-"
    MinimizeButton.TextColor3 = config.TextColor
    MinimizeButton.TextSize = 20

    -- Container das tabs
    TabHolder.Name = "TabHolder"
    TabHolder.Parent = Main
    TabHolder.BackgroundColor3 = config.MainColor
    TabHolder.Position = UDim2.new(0, 0, 0, 30)
    TabHolder.Size = UDim2.new(0, 120, 1, -30)

    TabContainer.Name = "TabContainer"
    TabContainer.Parent = TabHolder
    TabContainer.BackgroundTransparency = 1
    TabContainer.Position = UDim2.new(0, 0, 0, 10)
    TabContainer.Size = UDim2.new(1, 0, 1, -10)

    UIListLayout.Parent = TabContainer
    UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
    UIListLayout.Padding = UDim.new(0, 5)

    -- Container do conteúdo
    ContentContainer.Name = "ContentContainer"
    ContentContainer.Parent = Main
    ContentContainer.BackgroundTransparency = 1
    ContentContainer.Position = UDim2.new(0, 130, 0, 40)
    ContentContainer.Size = UDim2.new(1, -140, 1, -50)

    -- Funcionalidades
    local window = {}
    
    function window:CreateTab(name)
        local tab = {}
        local TabButton = Instance.new("TextButton")
        local TabContent = Instance.new("ScrollingFrame")
        local UIListLayout2 = Instance.new("UIListLayout")
        
        TabButton.Name = name.."Tab"
        TabButton.Parent = TabContainer
        TabButton.BackgroundTransparency = 0.9
        TabButton.Size = UDim2.new(1, -10, 0, 30)
        TabButton.Font = config.Font
        TabButton.Text = name
        TabButton.TextColor3 = config.TextColor
        TabButton.TextSize = 14
        
        TabContent.Name = name.."Content"
        TabContent.Parent = ContentContainer
        TabContent.BackgroundTransparency = 1
        TabContent.Size = UDim2.new(1, 0, 1, 0)
        TabContent.ScrollBarThickness = 3
        TabContent.Visible = false
        
        UIListLayout2.Parent = TabContent
        UIListLayout2.SortOrder = Enum.SortOrder.LayoutOrder
        UIListLayout2.Padding = UDim.new(0, 5)

        -- Funções para adicionar elementos
        function tab:AddToggle(toggleName, callback)
            local Toggle = Instance.new("Frame")
            local ToggleButton = Instance.new("TextButton")
            local ToggleState = Instance.new("Frame")
            local UICorner2 = Instance.new("UICorner")
            local toggled = false
            
            Toggle.Name = "Toggle"
            Toggle.Parent = TabContent
            Toggle.BackgroundTransparency = 1
            Toggle.Size = UDim2.new(1, 0, 0, 30)
            
            ToggleButton.Name = "ToggleButton"
            ToggleButton.Parent = Toggle
            ToggleButton.BackgroundColor3 = config.MainColor
            ToggleButton.BackgroundTransparency = 0.9
            ToggleButton.Size = UDim2.new(0, 20, 0, 20)
            ToggleButton.Position = UDim2.new(0, 5, 0.5, -10)
            ToggleButton.Text = ""
            
            ToggleState.Name = "ToggleState"
            ToggleState.Parent = ToggleButton
            ToggleState.BackgroundColor3 = config.MainColor
            ToggleState.Size = UDim2.new(0, 0, 1, 0)
            
            UICorner2.Parent = ToggleButton
            UICorner2.CornerRadius = UDim.new(0, 4)
            
            local TextLabel = Instance.new("TextLabel")
            TextLabel.Parent = Toggle
            TextLabel.BackgroundTransparency = 1
            TextLabel.Position = UDim2.new(0, 35, 0, 0)
            TextLabel.Size = UDim2.new(1, -40, 1, 0)
            TextLabel.Font = config.Font
            TextLabel.Text = toggleName
            TextLabel.TextColor3 = config.TextColor
            TextLabel.TextSize = 14
            TextLabel.TextXAlignment = Enum.TextXAlignment.Left
            
            ToggleButton.MouseButton1Click:Connect(function()
                toggled = not toggled
                callback(toggled)
                
                if toggled then
                    game:GetService("TweenService"):Create(ToggleState, 
                        TweenInfo.new(config.ToggleSpeed), 
                        {Size = UDim2.new(1, 0, 1, 0)}
                    ):Play()
                else
                    game:GetService("TweenService"):Create(ToggleState, 
                        TweenInfo.new(config.ToggleSpeed), 
                        {Size = UDim2.new(0, 0, 1, 0)}
                    ):Play()
                end
            end)
        end

        function tab:AddSlider(sliderName, min, max, default, callback)
            local Slider = Instance.new("Frame")
            local SliderBar = Instance.new("Frame")
            local SliderFill = Instance.new("Frame")
            local SliderButton = Instance.new("TextButton")
            local TextBox = Instance.new("TextBox")
            
            Slider.Name = "Slider"
            Slider.Parent = TabContent
            Slider.BackgroundTransparency = 1
            Slider.Size = UDim2.new(1, 0, 0, 50)
            
            local TextLabel = Instance.new("TextLabel")
            TextLabel.Parent = Slider
            TextLabel.BackgroundTransparency = 1
            TextLabel.Position = UDim2.new(0, 5, 0, 0)
            TextLabel.Size = UDim2.new(1, -10, 0, 20)
            TextLabel.Font = config.Font
            TextLabel.Text = sliderName
            TextLabel.TextColor3 = config.TextColor
            TextLabel.TextSize = 14
            TextLabel.TextXAlignment = Enum.TextXAlignment.Left
            
            SliderBar.Name = "SliderBar"
            SliderBar.Parent = Slider
            SliderBar.BackgroundColor3 = config.MainColor
            SliderBar.BackgroundTransparency = 0.9
            SliderBar.Position = UDim2.new(0, 5, 0, 25)
            SliderBar.Size = UDim2.new(1, -70, 0, 6)
            
            SliderFill.Name = "SliderFill"
            SliderFill.Parent = SliderBar
            SliderFill.BackgroundColor3 = config.MainColor
            SliderFill.Size = UDim2.new(0.5, 0, 1, 0)
            
            TextBox.Parent = Slider
            TextBox.BackgroundColor3 = config.MainColor
            TextBox.BackgroundTransparency = 0.9
            TextBox.Position = UDim2.new(1, -60, 0, 20)
            TextBox.Size = UDim2.new(0, 50, 0, 20)
            TextBox.Font = config.Font
            TextBox.Text = tostring(default)
            TextBox.TextColor3 = config.TextColor
            TextBox.TextSize = 14

            -- Adicionar lógica do slider aqui
        end

        function tab:AddDropdown(dropdownName, options, callback)
            local Dropdown = Instance.new("Frame")
            local DropdownButton = Instance.new("TextButton")
            local DropdownContent = Instance.new("Frame")
            local UIListLayout3 = Instance.new("UIListLayout")
            local SearchBox = Instance.new("TextBox")
            
            -- Adicionar lógica do dropdown aqui
        end

        -- Mais funções para outros elementos...

        return tab
    end

    -- Lógica dos botões de controle
    local minimized = false
    MinimizeButton.MouseButton1Click:Connect(function()
        minimized = not minimized
        if minimized then
            Main.Size = UDim2.new(0, 600, 0, 30)
        else
            Main.Size = UDim2.new(0, 600, 0, 400)
        end
    end)

    CloseButton.MouseButton1Click:Connect(function()
        NyoxGui:Destroy()
    end)

    -- Atalho de teclado para minimizar (Ctrl + M)
    game:GetService("UserInputService").InputBegan:Connect(function(input, processed)
        if not processed and input.KeyCode == Enum.KeyCode.M and game:GetService("UserInputService"):IsKeyDown(Enum.KeyCode.LeftControl) then
            MinimizeButton.MouseButton1Click:Fire()
        end
    end)

    return window
end

return NyoxLibrary
