local Library = {}
local TweenService = game:GetService("TweenService")
local UIS = game:GetService("UserInputService")

-- Utility Functions
local function CreateTween(Object, Time, Style, Direction, Properties)
    return TweenService:Create(Object, TweenInfo.new(Time, Enum.EasingStyle[Style], Enum.EasingDirection[Direction]), Properties)
end

function Library:CreateWindow(HubName, GameName)
    -- Main Frame Setup
    local SolaraLibrary = Instance.new("ScreenGui")
    local Main = Instance.new("Frame")
    local UICorner = Instance.new("UICorner")
    local Title = Instance.new("TextLabel")
    local TabHolder = Instance.new("Frame")
    local TabContainer = Instance.new("ScrollingFrame")
    local TabList = Instance.new("UIListLayout")
    local ContainerHolder = Instance.new("Frame")
    
    -- Properties
    SolaraLibrary.Name = "SolaraLibrary"
    SolaraLibrary.Parent = game.CoreGui
    SolaraLibrary.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    
    Main.Name = "Main"
    Main.Parent = SolaraLibrary
    Main.BackgroundColor3 = Color3.fromRGB(30, 30, 45)
    Main.Position = UDim2.new(0.5, -300, 0.5, -175)
    Main.Size = UDim2.new(0, 600, 0, 350)
    Main.ClipsDescendants = true
    
    UICorner.CornerRadius = UDim.new(0, 6)
    UICorner.Parent = Main
    
    Title.Name = "Title"
    Title.Parent = Main
    Title.BackgroundTransparency = 1
    Title.Position = UDim2.new(0, 15, 0, 10)
    Title.Size = UDim2.new(0, 200, 0, 25)
    Title.Font = Enum.Font.GothamBold
    Title.Text = HubName
    Title.TextColor3 = Color3.fromRGB(255, 255, 255)
    Title.TextSize = 14
    Title.TextXAlignment = Enum.TextXAlignment.Left
    
    -- Minimize Button
    local MinimizeBtn = Instance.new("TextButton")
    MinimizeBtn.Name = "MinimizeBtn"
    MinimizeBtn.Parent = Main
    MinimizeBtn.BackgroundTransparency = 1
    MinimizeBtn.Position = UDim2.new(1, -40, 0, 10)
    MinimizeBtn.Size = UDim2.new(0, 25, 0, 25)
    MinimizeBtn.Font = Enum.Font.GothamBold
    MinimizeBtn.Text = "-"
    MinimizeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    MinimizeBtn.TextSize = 20
    
    local Minimized = false
    MinimizeBtn.MouseButton1Click:Connect(function()
        Minimized = not Minimized
        if Minimized then
            CreateTween(Main, 0.5, "Quart", "Out", {Size = UDim2.new(0, 600, 0, 50)}):Play()
            MinimizeBtn.Text = "+"
        else
            CreateTween(Main, 0.5, "Quart", "Out", {Size = UDim2.new(0, 600, 0, 350)}):Play()
            MinimizeBtn.Text = "-"
        end
    end)
    
    -- Tab System
    TabHolder.Name = "TabHolder"
    TabHolder.Parent = Main
    TabHolder.BackgroundColor3 = Color3.fromRGB(25, 25, 35)
    TabHolder.Position = UDim2.new(0, 15, 0, 45)
    TabHolder.Size = UDim2.new(0, 150, 0, 290)
    
    local UICorner_2 = Instance.new("UICorner")
    UICorner_2.CornerRadius = UDim.new(0, 6)
    UICorner_2.Parent = TabHolder
    
    TabContainer.Name = "TabContainer"
    TabContainer.Parent = TabHolder
    TabContainer.BackgroundTransparency = 1
    TabContainer.Position = UDim2.new(0, 0, 0, 5)
    TabContainer.Size = UDim2.new(1, 0, 1, -10)
    TabContainer.ScrollBarThickness = 0
    
    TabList.Parent = TabContainer
    TabList.SortOrder = Enum.SortOrder.LayoutOrder
    TabList.Padding = UDim.new(0, 5)
    
    ContainerHolder.Name = "ContainerHolder"
    ContainerHolder.Parent = Main
    ContainerHolder.BackgroundTransparency = 1
    ContainerHolder.Position = UDim2.new(0, 175, 0, 45)
    ContainerHolder.Size = UDim2.new(0, 410, 0, 290)
    
    local Tabs = {}
    
    function Tabs:CreateTab(TabName)
        local TabButton = Instance.new("TextButton")
        local TabContainer = Instance.new("ScrollingFrame")
        local ItemList = Instance.new("UIListLayout")
        
        TabButton.Name = "TabButton"
        TabButton.Parent = TabContainer
        TabButton.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
        TabButton.Size = UDim2.new(1, -10, 0, 30)
        TabButton.Font = Enum.Font.GothamSemibold
        TabButton.Text = TabName
        TabButton.TextColor3 = Color3.fromRGB(255, 255, 255)
        TabButton.TextSize = 12
        
        local UICorner_3 = Instance.new("UICorner")
        UICorner_3.CornerRadius = UDim.new(0, 6)
        UICorner_3.Parent = TabButton
        
        TabContainer.Name = TabName.."Container"
        TabContainer.Parent = ContainerHolder
        TabContainer.BackgroundTransparency = 1
        TabContainer.Size = UDim2.new(1, 0, 1, 0)
        TabContainer.ScrollBarThickness = 2
        TabContainer.Visible = false
        
        ItemList.Parent = TabContainer
        ItemList.SortOrder = Enum.SortOrder.LayoutOrder
        ItemList.Padding = UDim.new(0, 5)
        
        local Elements = {}
        
        -- Create Toggle
        function Elements:CreateToggle(ToggleName, Callback)
            local Toggle = Instance.new("Frame")
            local ToggleButton = Instance.new("TextButton")
            local UICorner_4 = Instance.new("UICorner")
            local ToggleIndicator = Instance.new("Frame")
            local UICorner_5 = Instance.new("UICorner")
            
            Toggle.Name = "Toggle"
            Toggle.Parent = TabContainer
            Toggle.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
            Toggle.Size = UDim2.new(1, -10, 0, 35)
            
            UICorner_4.CornerRadius = UDim.new(0, 6)
            UICorner_4.Parent = Toggle
            
            ToggleButton.Name = "ToggleButton"
            ToggleButton.Parent = Toggle
            ToggleButton.BackgroundTransparency = 1
            ToggleButton.Size = UDim2.new(1, 0, 1, 0)
            ToggleButton.Font = Enum.Font.GothamSemibold
            ToggleButton.Text = ToggleName
            ToggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
            ToggleButton.TextSize = 12
            
            ToggleIndicator.Name = "ToggleIndicator"
            ToggleIndicator.Parent = Toggle
            ToggleIndicator.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
            ToggleIndicator.Position = UDim2.new(1, -45, 0.5, -10)
            ToggleIndicator.Size = UDim2.new(0, 35, 0, 20)
            
            UICorner_5.CornerRadius = UDim.new(0, 10)
            UICorner_5.Parent = ToggleIndicator
            
            local Toggled = false
            ToggleButton.MouseButton1Click:Connect(function()
                Toggled = not Toggled
                CreateTween(ToggleIndicator, 0.3, "Quad", "Out", {
                    BackgroundColor3 = Toggled and Color3.fromRGB(50, 255, 50) or Color3.fromRGB(255, 50, 50)
                }):Play()
                Callback(Toggled)
            end)
        end
        
        -- Create Slider
        function Elements:CreateSlider(SliderName, Min, Max, Default, Callback)
            local Slider = Instance.new("Frame")
            local UICorner_6 = Instance.new("UICorner")
            local SliderTitle = Instance.new("TextLabel")
            local SliderValue = Instance.new("TextLabel")
            local SliderBar = Instance.new("Frame")
            local UICorner_7 = Instance.new("UICorner")
            local SliderFill = Instance.new("Frame")
            local UICorner_8 = Instance.new("UICorner")
            
            Slider.Name = "Slider"
            Slider.Parent = TabContainer
            Slider.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
            Slider.Size = UDim2.new(1, -10, 0, 45)
            
            UICorner_6.CornerRadius = UDim.new(0, 6)
            UICorner_6.Parent = Slider
            
            SliderTitle.Name = "SliderTitle"
            SliderTitle.Parent = Slider
            SliderTitle.BackgroundTransparency = 1
            SliderTitle.Position = UDim2.new(0, 10, 0, 5)
            SliderTitle.Size = UDim2.new(1, -20, 0, 20)
            SliderTitle.Font = Enum.Font.GothamSemibold
            SliderTitle.Text = SliderName
            SliderTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
            SliderTitle.TextSize = 12
            SliderTitle.TextXAlignment = Enum.TextXAlignment.Left
            
            SliderBar.Name = "SliderBar"
            SliderBar.Parent = Slider
            SliderBar.BackgroundColor3 = Color3.fromRGB(30, 30, 45)
            SliderBar.Position = UDim2.new(0, 10, 0, 30)
            SliderBar.Size = UDim2.new(1, -60, 0, 5)
            
            UICorner_7.CornerRadius = UDim.new(0, 3)
            UICorner_7.Parent = SliderBar
            
            SliderFill.Name = "SliderFill"
            SliderFill.Parent = SliderBar
            SliderFill.BackgroundColor3 = Color3.fromRGB(130, 100, 255)
            SliderFill.Size = UDim2.new((Default - Min)/(Max - Min), 0, 1, 0)
            
            UICorner_8.CornerRadius = UDim.new(0, 3)
            UICorner_8.Parent = SliderFill
            
            SliderValue.Name = "SliderValue"
            SliderValue.Parent = Slider
            SliderValue.BackgroundTransparency = 1
            SliderValue.Position = UDim2.new(1, -45, 0, 5)
            SliderValue.Size = UDim2.new(0, 35, 0, 20)
            SliderValue.Font = Enum.Font.GothamSemibold
            SliderValue.Text = tostring(Default)
            SliderValue.TextColor3 = Color3.fromRGB(255, 255, 255)
            SliderValue.TextSize = 12
            
            local Dragging = false
            SliderBar.InputBegan:Connect(function(Input)
                if Input.UserInputType == Enum.UserInputType.MouseButton1 then
                    Dragging = true
                end
            end)
            
            UIS.InputEnded:Connect(function(Input)
                if Input.UserInputType == Enum.UserInputType.MouseButton1 then
                    Dragging = false
                end
            end)
            
            UIS.InputChanged:Connect(function(Input)
                if Input.UserInputType == Enum.UserInputType.MouseMovement and Dragging then
                    local SizeScale = math.clamp((Input.Position.X - SliderBar.AbsolutePosition.X) / SliderBar.AbsoluteSize.X, 0, 1)
                    local Value = math.floor(Min + ((Max - Min) * SizeScale))
                    SliderValue.Text = tostring(Value)
                    CreateTween(SliderFill, 0.1, "Linear", "Out", {Size = UDim2.new(SizeScale, 0, 1, 0)}):Play()
                    Callback(Value)
                end
            end)
        end
        
        -- Create Dropdown
        function Elements:CreateDropdown(DropdownName, Options, Callback)
            local Dropdown = Instance.new("Frame")
            local UICorner_9 = Instance.new("UICorner")
            local DropdownButton = Instance.new("TextButton")
            local DropdownContainer = Instance.new("Frame")
            local UICorner_10 = Instance.new("UICorner")
            local OptionList = Instance.new("UIListLayout")
            
            Dropdown.Name = "Dropdown"
            Dropdown.Parent = TabContainer
            Dropdown.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
            Dropdown.Size = UDim2.new(1, -10, 0, 35)
            Dropdown.ClipsDescendants = true
            
            UICorner_9.CornerRadius = UDim.new(0, 6)
            UICorner_9.Parent = Dropdown
            
            DropdownButton.Name = "DropdownButton"
            DropdownButton.Parent = Dropdown
            DropdownButton.BackgroundTransparency = 1
            DropdownButton.Size = UDim2.new(1, 0, 0, 35)
            DropdownButton.Font = Enum.Font.GothamSemibold
            DropdownButton.Text = DropdownName
            DropdownButton.TextColor3 = Color3.fromRGB(255, 255, 255)
            DropdownButton.TextSize = 12
            
            DropdownContainer.Name = "DropdownContainer"
            DropdownContainer.Parent = Dropdown
            DropdownContainer.BackgroundColor3 = Color3.fromRGB(35, 35, 50)
	DropdownContainer.Position = UDim2.new(0, 0, 0, 35)
            DropdownContainer.Size = UDim2.new(1, 0, 0, #Options * 25)
            
            UICorner_10.CornerRadius = UDim.new(0, 6)
            UICorner_10.Parent = DropdownContainer
            
            OptionList.Parent = DropdownContainer
            OptionList.SortOrder = Enum.SortOrder.LayoutOrder
            
            local Extended = false
            
            DropdownButton.MouseButton1Click:Connect(function()
                Extended = not Extended
                CreateTween(Dropdown, 0.3, "Quad", "Out", {
                    Size = Extended and UDim2.new(1, -10, 0, 35 + #Options * 25) or UDim2.new(1, -10, 0, 35)
                }):Play()
            end)
            
            for _, Option in ipairs(Options) do
                local OptionButton = Instance.new("TextButton")
                OptionButton.Name = Option
                OptionButton.Parent = DropdownContainer
                OptionButton.BackgroundTransparency = 1
                OptionButton.Size = UDim2.new(1, 0, 0, 25)
                OptionButton.Font = Enum.Font.GothamSemibold
                OptionButton.Text = Option
                OptionButton.TextColor3 = Color3.fromRGB(255, 255, 255)
                OptionButton.TextSize = 12
                
                OptionButton.MouseButton1Click:Connect(function()
                    DropdownButton.Text = DropdownName .. ": " .. Option
                    Extended = false
                    CreateTween(Dropdown, 0.3, "Quad", "Out", {
                        Size = UDim2.new(1, -10, 0, 35)
                    }):Play()
                    Callback(Option)
                end)
            end
        end
        
        -- Create Button
        function Elements:CreateButton(ButtonName, Callback)
            local Button = Instance.new("Frame")
            local UICorner_11 = Instance.new("UICorner")
            local ButtonText = Instance.new("TextButton")
            
            Button.Name = "Button"
            Button.Parent = TabContainer
            Button.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
            Button.Size = UDim2.new(1, -10, 0, 35)
            
            UICorner_11.CornerRadius = UDim.new(0, 6)
            UICorner_11.Parent = Button
            
            ButtonText.Name = "ButtonText"
            ButtonText.Parent = Button
            ButtonText.BackgroundTransparency = 1
            ButtonText.Size = UDim2.new(1, 0, 1, 0)
            ButtonText.Font = Enum.Font.GothamSemibold
            ButtonText.Text = ButtonName
            ButtonText.TextColor3 = Color3.fromRGB(255, 255, 255)
            ButtonText.TextSize = 12
            
            ButtonText.MouseButton1Click:Connect(function()
                Callback()
            end)
        end
        
        return Elements
    end
    
    return Tabs
end

return Library
