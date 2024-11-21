local library = {}
local TweenService = game:GetService("TweenService")
local UIS = game:GetService("UserInputService")

function library.new(title)
    -- Remove qualquer GUI existente com o mesmo nome
    if game:GetService("CoreGui"):FindFirstChild("NyoxLibrary") then
        game:GetService("CoreGui"):FindFirstChild("NyoxLibrary"):Destroy()
    end
    
    -- Cria a GUI principal
    local NyoxLibrary = Instance.new("ScreenGui")
    local Main = Instance.new("Frame")
    local UICorner = Instance.new("UICorner")
    local TopBar = Instance.new("Frame")
    local Title = Instance.new("TextLabel")
    local CloseButton = Instance.new("TextButton")
    local MinButton = Instance.new("TextButton")
    local TabContainer = Instance.new("Frame")
    local ElementContainer = Instance.new("Frame")
    
    -- Configura a GUI
    NyoxLibrary.Name = "NyoxLibrary"
    NyoxLibrary.Parent = game:GetService("CoreGui")
    
    Main.Name = "Main"
    Main.Parent = NyoxLibrary
    Main.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    Main.Position = UDim2.new(0.5, -250, 0.5, -150)
    Main.Size = UDim2.new(0, 500, 0, 300)
    Main.Active = true
    Main.Draggable = true
    
    UICorner.Parent = Main
    UICorner.CornerRadius = UDim.new(0, 6)
    
    TopBar.Name = "TopBar"
    TopBar.Parent = Main
    TopBar.BackgroundColor3 = Color3.fromRGB(147, 51, 234)
    TopBar.Size = UDim2.new(1, 0, 0, 30)
    
    Title.Name = "Title"
    Title.Parent = TopBar
    Title.BackgroundTransparency = 1
    Title.Position = UDim2.new(0, 10, 0, 0)
    Title.Size = UDim2.new(1, -60, 1, 0)
    Title.Font = Enum.Font.GothamBold
    Title.Text = title or "Nyox Library"
    Title.TextColor3 = Color3.fromRGB(255, 255, 255)
    Title.TextSize = 14
    Title.TextXAlignment = Enum.TextXAlignment.Left
    
    CloseButton.Name = "CloseButton"
    CloseButton.Parent = TopBar
    CloseButton.BackgroundTransparency = 1
    CloseButton.Position = UDim2.new(1, -25, 0, 0)
    CloseButton.Size = UDim2.new(0, 25, 1, 0)
    CloseButton.Font = Enum.Font.GothamBold
    CloseButton.Text = "X"
    CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    CloseButton.TextSize = 14
    
    MinButton.Name = "MinButton"
    MinButton.Parent = TopBar
    MinButton.BackgroundTransparency = 1
    MinButton.Position = UDim2.new(1, -50, 0, 0)
    MinButton.Size = UDim2.new(0, 25, 1, 0)
    MinButton.Font = Enum.Font.GothamBold
    MinButton.Text = "-"
    MinButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    MinButton.TextSize = 14
    
    TabContainer.Name = "TabContainer"
    TabContainer.Parent = Main
    TabContainer.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    TabContainer.Position = UDim2.new(0, 5, 0, 35)
    TabContainer.Size = UDim2.new(0, 100, 1, -40)
    
    ElementContainer.Name = "ElementContainer"
    ElementContainer.Parent = Main
    ElementContainer.BackgroundTransparency = 1
    ElementContainer.Position = UDim2.new(0, 110, 0, 35)
    ElementContainer.Size = UDim2.new(1, -115, 1, -40)
    
    -- Funcionalidades
    local minimized = false
    
    CloseButton.MouseButton1Click:Connect(function()
        NyoxLibrary:Destroy()
    end)
    
    MinButton.MouseButton1Click:Connect(function()
        minimized = not minimized
        if minimized then
            Main:TweenSize(UDim2.new(0, 500, 0, 30), "Out", "Quad", 0.3, true)
        else
            Main:TweenSize(UDim2.new(0, 500, 0, 300), "Out", "Quad", 0.3, true)
        end
    end)
    
    local window = {}
    
    function window:CreateTab(name)
        local tab = {}
        
        local TabButton = Instance.new("TextButton")
        local TabContent = Instance.new("ScrollingFrame")
        local UIListLayout = Instance.new("UIListLayout")
        
        TabButton.Name = name.."Tab"
        TabButton.Parent = TabContainer
        TabButton.BackgroundColor3 = Color3.fromRGB(147, 51, 234)
        TabButton.BackgroundTransparency = 0.9
        TabButton.Size = UDim2.new(1, -4, 0, 25)
        TabButton.Font = Enum.Font.Gotham
        TabButton.Text = name
        TabButton.TextColor3 = Color3.fromRGB(255, 255, 255)
        TabButton.TextSize = 12
        
        TabContent.Name = name.."Content"
        TabContent.Parent = ElementContainer
        TabContent.BackgroundTransparency = 1
        TabContent.Size = UDim2.new(1, 0, 1, 0)
        TabContent.ScrollBarThickness = 2
        TabContent.Visible = false
        
        UIListLayout.Parent = TabContent
        UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
        UIListLayout.Padding = UDim.new(0, 5)
        
        function tab:AddToggle(text, callback)
            callback = callback or function() end
            
            local Toggle = Instance.new("Frame")
            local ToggleButton = Instance.new("TextButton")
            local ToggleInner = Instance.new("Frame")
            local ToggleText = Instance.new("TextLabel")
            
            Toggle.Name = "Toggle"
            Toggle.Parent = TabContent
            Toggle.BackgroundTransparency = 1
            Toggle.Size = UDim2.new(1, -10, 0, 25)
            
            ToggleButton.Name = "ToggleButton"
            ToggleButton.Parent = Toggle
            ToggleButton.BackgroundColor3 = Color3.fromRGB(147, 51, 234)
            ToggleButton.BackgroundTransparency = 0.9
            ToggleButton.Size = UDim2.new(0, 25, 0, 25)
            ToggleButton.Text = ""
            
            ToggleInner.Name = "ToggleInner"
            ToggleInner.Parent = ToggleButton
            ToggleInner.BackgroundColor3 = Color3.fromRGB(147, 51, 234)
            ToggleInner.BorderSizePixel = 0
            ToggleInner.Size = UDim2.new(0, 0, 1, 0)
            
            ToggleText.Name = "ToggleText"
            ToggleText.Parent = Toggle
            ToggleText.BackgroundTransparency = 1
            ToggleText.Position = UDim2.new(0, 30, 0, 0)
            ToggleText.Size = UDim2.new(1, -30, 1, 0)
            ToggleText.Font = Enum.Font.Gotham
            ToggleText.Text = text
            ToggleText.TextColor3 = Color3.fromRGB(255, 255, 255)
            ToggleText.TextSize = 12
            ToggleText.TextXAlignment = Enum.TextXAlignment.Left
            
            local toggled = false
            
            ToggleButton.MouseButton1Click:Connect(function()
                toggled = not toggled
                TweenService:Create(ToggleInner, TweenInfo.new(0.2), {
                    Size = toggled and UDim2.new(1, 0, 1, 0) or UDim2.new(0, 0, 1, 0)
                }):Play()
                callback(toggled)
            end)
        end
        
        function tab:AddSlider(text, min, max, default, callback)
            callback = callback or function() end
            
            local Slider = Instance.new("Frame")
            local SliderText = Instance.new("TextLabel")
            local SliderBar = Instance.new("Frame")
            local SliderFill = Instance.new("Frame")
            local SliderValue = Instance.new("TextBox")
            
            Slider.Name = "Slider"
            Slider.Parent = TabContent
            Slider.BackgroundTransparency = 1
            Slider.Size = UDim2.new(1, -10, 0, 40)
            
            SliderText.Name = "SliderText"
            SliderText.Parent = Slider
            SliderText.BackgroundTransparency = 1
            SliderText.Size = UDim2.new(1, 0, 0, 20)
            SliderText.Font = Enum.Font.Gotham
            SliderText.Text = text
            SliderText.TextColor3 = Color3.fromRGB(255, 255, 255)
            SliderText.TextSize = 12
            SliderText.TextXAlignment = Enum.TextXAlignment.Left
            
            SliderBar.Name = "SliderBar"
            SliderBar.Parent = Slider
            SliderBar.BackgroundColor3 = Color3.fromRGB(147, 51, 234)
            SliderBar.BackgroundTransparency = 0.9
            SliderBar.Position = UDim2.new(0, 0, 0, 25)
            SliderBar.Size = UDim2.new(1, -50, 0, 5)
            
            SliderFill.Name = "SliderFill"
            SliderFill.Parent = SliderBar
            SliderFill.BackgroundColor3 = Color3.fromRGB(147, 51, 234)
            SliderFill.Size = UDim2.new((default - min)/(max - min), 0, 1, 0)
            
            SliderValue.Name = "SliderValue"
            SliderValue.Parent = Slider
            SliderValue.BackgroundTransparency = 1
            SliderValue.Position = UDim2.new(1, -45, 0, 20)
            SliderValue.Size = UDim2.new(0, 45, 0, 15)
            SliderValue.Font = Enum.Font.Gotham
            SliderValue.Text = tostring(default)
            SliderValue.TextColor3 = Color3.fromRGB(255, 255, 255)
            SliderValue.TextSize = 12
            
            local dragging = false
            
            SliderBar.InputBegan:Connect(function(input)
                if input.UserInputType == Enum.UserInputType.MouseButton1 then
                    dragging = true
                end
            end)
            
            UIS.InputEnded:Connect(function(input)
                if input.UserInputType == Enum.UserInputType.MouseButton1 then
                    dragging = false
                end
            end)
            
            UIS.InputChanged:Connect(function(input)
                if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
                    local pos = UDim2.new(math.clamp((input.Position.X - SliderBar.AbsolutePosition.X) / SliderBar.AbsoluteSize.X, 0, 1), 0, 1, 0)
                    SliderFill:TweenSize(pos, "Out", "Quad", 0.1, true)
                    local value = math.floor(min + ((max - min) * pos.X.Scale))
                    SliderValue.Text = tostring(value)
                    callback(value)
                end
            end)
        end
        
        return tab
    end
    
    return window
end

return library
