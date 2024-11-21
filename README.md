local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local CoreGui = game:GetService("CoreGui")

local Library = {}
Library.__index = Library

-- Função para criar Tweens
local function Tween(obj, time, properties)
    local tweenInfo = TweenInfo.new(time, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
    local tween = TweenService:Create(obj, tweenInfo, properties)
    tween:Play()
    return tween
end

-- Criação da Library principal
function Library.new(title)
    local NyoxLibrary = Instance.new("ScreenGui")
    NyoxLibrary.Name = "NyoxLibrary"
    NyoxLibrary.Parent = CoreGui
    NyoxLibrary.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    
    -- Frame Principal
    local Main = Instance.new("Frame")
    Main.Name = "Main"
    Main.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
    Main.BorderSizePixel = 0
    Main.Position = UDim2.new(0.5, -250, 0.5, -175)
    Main.Size = UDim2.new(0, 500, 0, 350)
    Main.Parent = NyoxLibrary
    
    local UICorner = Instance.new("UICorner")
    UICorner.CornerRadius = UDim.new(0, 6)
    UICorner.Parent = Main
    
    -- Barra Superior
    local TopBar = Instance.new("Frame")
    TopBar.Name = "TopBar"
    TopBar.BackgroundColor3 = Color3.fromRGB(40, 40, 45)
    TopBar.BorderSizePixel = 0
    TopBar.Size = UDim2.new(1, 0, 0, 30)
    TopBar.Parent = Main
    
    local UICorner_2 = Instance.new("UICorner")
    UICorner_2.CornerRadius = UDim.new(0, 6)
    UICorner_2.Parent = TopBar
    
    -- Título
    local Title = Instance.new("TextLabel")
    Title.Name = "Title"
    Title.BackgroundTransparency = 1
    Title.Position = UDim2.new(0, 10, 0, 0)
    Title.Size = UDim2.new(1, -20, 1, 0)
    Title.Font = Enum.Font.GothamBold
    Title.Text = title
    Title.TextColor3 = Color3.fromRGB(255, 255, 255)
    Title.TextSize = 14
    Title.TextXAlignment = Enum.TextXAlignment.Left
    Title.Parent = TopBar
    
    -- Botão de Fechar
    local CloseButton = Instance.new("TextButton")
    CloseButton.Name = "CloseButton"
    CloseButton.BackgroundTransparency = 1
    CloseButton.Position = UDim2.new(1, -25, 0, 5)
    CloseButton.Size = UDim2.new(0, 20, 0, 20)
    CloseButton.Font = Enum.Font.GothamBold
    CloseButton.Text = "X"
    CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    CloseButton.TextSize = 14
    CloseButton.Parent = TopBar
    
    -- Botão de Minimizar
    local MinimizeButton = Instance.new("TextButton")
    MinimizeButton.Name = "MinimizeButton"
    MinimizeButton.BackgroundTransparency = 1
    MinimizeButton.Position = UDim2.new(1, -50, 0, 5)
    MinimizeButton.Size = UDim2.new(0, 20, 0, 20)
    MinimizeButton.Font = Enum.Font.GothamBold
    MinimizeButton.Text = "-"
    MinimizeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    MinimizeButton.TextSize = 14
    MinimizeButton.Parent = TopBar
    
    -- Container de Abas
    local TabContainer = Instance.new("Frame")
    TabContainer.Name = "TabContainer"
    TabContainer.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
    TabContainer.BorderSizePixel = 0
    TabContainer.Position = UDim2.new(0, 0, 0, 30)
    TabContainer.Size = UDim2.new(0, 120, 1, -30)
    TabContainer.Parent = Main
    
    local UICorner_3 = Instance.new("UICorner")
    UICorner_3.CornerRadius = UDim.new(0, 6)
    UICorner_3.Parent = TabContainer
    
    -- Lista de Abas
    local TabList = Instance.new("ScrollingFrame")
    TabList.Name = "TabList"
    TabList.BackgroundTransparency = 1
    TabList.BorderSizePixel = 0
    TabList.Position = UDim2.new(0, 0, 0, 5)
    TabList.Size = UDim2.new(1, 0, 1, -10)
    TabList.CanvasSize = UDim2.new(0, 0, 0, 0)
    TabList.ScrollBarThickness = 2
    TabList.Parent = TabContainer
    
    local UIListLayout = Instance.new("UIListLayout")
    UIListLayout.Parent = TabList
    UIListLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
    UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
    UIListLayout.Padding = UDim.new(0, 5)
    
    -- Container de Conteúdo
    local ContentContainer = Instance.new("Frame")
    ContentContainer.Name = "ContentContainer"
    ContentContainer.BackgroundTransparency = 1
    ContentContainer.Position = UDim2.new(0, 125, 0, 35)
    ContentContainer.Size = UDim2.new(1, -130, 1, -40)
    ContentContainer.Parent = Main
    
    local lib = setmetatable({
        gui = NyoxLibrary,
        main = Main,
        tabs = {},
        activeTab = nil,
        minimized = false
    }, Library)
    
    -- Sistema de Arraste
    local dragging
    local dragInput
    local dragStart
    local startPos
    
    TopBar.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
            dragStart = input.Position
            startPos = Main.Position
        end
    end)
    
    TopBar.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = false
        end
    end)
    
    UserInputService.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement then
            dragInput = input
        end
    end)
    
    RunService.RenderStepped:Connect(function()
        if dragging and dragInput then
            local delta = dragInput.Position - dragStart
            Main.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)
    
    -- Funções dos botões
    CloseButton.MouseButton1Click:Connect(function()
        NyoxLibrary:Destroy()
    end)
    
    MinimizeButton.MouseButton1Click:Connect(function()
        lib:Minimize()
    end)
    
    -- Tecla para minimizar
    UserInputService.InputBegan:Connect(function(input)
        if input.KeyCode == Enum.KeyCode.RightControl then
            lib:Minimize()
        end
    end)
    
    return lib
end

-- Função de Minimizar
function Library:Minimize()
    self.minimized = not self.minimized
    if self.minimized then
        Tween(self.main, 0.3, {Size = UDim2.new(0, 500, 0, 30)})
    else
        Tween(self.main, 0.3, {Size = UDim2.new(0, 500, 0, 350)})
    end
end

-- Função para criar Abas
function Library:CreateTab(name)
    local tab = {}
    
    -- Botão da Aba
    local TabButton = Instance.new("TextButton")
    TabButton.Name = name
    TabButton.BackgroundColor3 = Color3.fromRGB(45, 45, 50)
    TabButton.BorderSizePixel = 0
    TabButton.Size = UDim2.new(0.9, 0, 0, 30)
    TabButton.Font = Enum.Font.Gotham
    TabButton.Text = name
    TabButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    TabButton.TextSize = 12
    TabButton.Parent = self.main.TabList
    
    local UICorner = Instance.new("UICorner")
    UICorner.CornerRadius = UDim.new(0, 4)
    UICorner.Parent = TabButton
    
    -- Conteúdo da Aba
    local TabContent = Instance.new("ScrollingFrame")
    TabContent.Name = name
    TabContent.BackgroundTransparency = 1
    TabContent.BorderSizePixel = 0
    TabContent.Position = UDim2.new(0, 0, 0, 0)
    TabContent.Size = UDim2.new(1, 0, 1, 0)
    TabContent.CanvasSize = UDim2.new(0, 0, 0, 0)
    TabContent.ScrollBarThickness = 2
    TabContent.Visible = false
    TabContent.Parent = self.main.ContentContainer
    
    local UIListLayout = Instance.new("UIListLayout")
    UIListLayout.Parent = TabContent
    UIListLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
    UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
    UIListLayout.Padding = UDim.new(0, 5)
    
    -- Funções da Aba
    function tab:AddButton(text, callback)
        local Button = Instance.new("TextButton")
        Button.Name = text
        Button.BackgroundColor3 = Color3.fromRGB(45, 45, 50)
        Button.Size = UDim2.new(0.9, 0, 0, 30)
        Button.Font = Enum.Font.Gotham
        Button.Text = text
        Button.TextColor3 = Color3.fromRGB(255, 255, 255)
        Button.TextSize = 12
        Button.Parent = TabContent
        
        local UICorner = Instance.new("UICorner")
        UICorner.CornerRadius = UDim.new(0, 4)
        UICorner.Parent = Button
        
        Button.MouseButton1Click:Connect(function()
            callback()
        end)
        
        return Button
    end
    
    function tab:AddToggle(text, default, callback)
        local ToggleFrame = Instance.new("Frame")
        ToggleFrame.Name = text
        ToggleFrame.BackgroundColor3 = Color3.fromRGB(45, 45, 50)
        ToggleFrame.Size = UDim2.new(0.9, 0, 0, 30)
        ToggleFrame.Parent = TabContent
        
        local UICorner = Instance.new("UICorner")
        UICorner.CornerRadius = UDim.new(0, 4)
        UICorner.Parent = ToggleFrame
        
        local ToggleLabel = Instance.new("TextLabel")
        ToggleLabel.BackgroundTransparency = 1
        ToggleLabel.Position = UDim2.new(0, 10, 0, 0)
        ToggleLabel.Size = UDim2.new(1, -50, 1, 0)
        ToggleLabel.Font = Enum.Font.Gotham
        ToggleLabel.Text = text
        ToggleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
        ToggleLabel.TextSize = 12
        ToggleLabel.TextXAlignment = Enum.TextXAlignment.Left
        ToggleLabel.Parent = ToggleFrame
        
        local ToggleButton = Instance.new("Frame")
        ToggleButton.Name = "ToggleButton"
        ToggleButton.BackgroundColor3 = default and Color3.fromRGB(130, 100, 255) or Color3.fromRGB(60, 60, 65)
        ToggleButton.Position = UDim2.new(1, -40, 0.5, -10)
        ToggleButton.Size = UDim2.new(0, 30, 0, 15)
        ToggleButton.Parent = ToggleFrame
        
        local UICorner = Instance.new("UICorner")
        UICorner.CornerRadius = UDim.new(1, 0)
        UICorner.Parent = ToggleButton
        
        local ToggleCircle = Instance.new("Frame")
        ToggleCircle.Name = "Circle"
        ToggleCircle.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        ToggleCircle.Position = default and UDim2.new(1, -15, 0.5, -7.5) or UDim2.new(0, 0, 0.5, -7.5)
        ToggleCircle.Size = UDim2.new(0, 15, 0, 15)
        ToggleCircle.Parent = ToggleButton
        
        local UICorner = Instance.new("UICorner")
        UICorner.CornerRadius = UDim.new(1, 0)
        UICorner.Parent = ToggleCircle
        
        local toggled = default
        local debounce = false
        
        ToggleFrame.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 and not debounce then
                debounce = true
                toggled = not toggled
                
                if toggled then
                    Tween(ToggleButton, 0.2, {BackgroundColor3 = Color3.fromRGB(130, 100, 255)})
                    Tween(ToggleCircle, 0.2, {Position = UDim2.new(1, -15, 0.5, -7.5)})
                else
                    Tween(ToggleButton, 0.2, {BackgroundColor3 = Color3.fromRGB(60, 60, 65)})
                    Tween(ToggleCircle, 0.2, {Position = UDim2.new(0, 0, 0.5, -7.5)})
                end
                
                callback(toggled)
                wait(0.2)
                debounce = false
            end
        end)
        
        return ToggleFrame
    end
    
    function tab:AddSlider(text, min, max, default, callback)
        local SliderFrame = Instance.new("Frame")
        SliderFrame.Name = text
        SliderFrame.BackgroundColor3 = Color3.fromRGB(45, 45, 50)
        SliderFrame.Size = UDim2.new(0.9, 0, 0, 50)
        SliderFrame.Parent = TabContent
        
        local UICorner = Instance.new("UICorner")
        UICorner.CornerRadius = UDim.new(0, 4)
        UICorner.Parent = SliderFrame
        
        local SliderLabel = Instance.new("TextLabel")
        SliderLabel.BackgroundTransparency = 1
        SliderLabel.Position = UDim2.new(0, 10, 0, 0)
        SliderLabel.Size = UDim2.new(1, -20, 0, 25)
        SliderLabel.Font = Enum.Font.Gotham
        SliderLabel.Text = text
        SliderLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
        SliderLabel.TextSize = 12
        SliderLabel.TextXAlignment = Enum.TextXAlignment.Left
        SliderLabel.Parent = SliderFrame
        
        local SliderValueBox = Instance.new("TextBox")
        SliderValueBox.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
        SliderValueBox.Position = UDim2.new(1, -50, 0, 5)
        SliderValueBox.Size = UDim2.new(0, 40, 0, 20)
        SliderValueBox.Font = Enum.Font.Gotham
        SliderValueBox.Text = tostring(default)
        SliderValueBox.TextColor3 = Color3.fromRGB(255, 255, 255)
        SliderValueBox.TextSize = 12
        SliderValueBox.Parent = SliderFrame
        
        local UICorner_2 = Instance.new("UICorner")
        UICorner_2.CornerRadius = UDim.new(0, 4)
        UICorner_2.Parent = SliderValueBox
        
        local SliderBG = Instance.new("Frame")
        SliderBG.Name = "SliderBG"
        SliderBG.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
        SliderBG.Position = UDim2.new(0, 10, 0, 35)
        SliderBG.Size = UDim2.new(1, -20, 0, 5)
        SliderBG.Parent = SliderFrame
        
        local UICorner_3 = Instance.new("UICorner")
        UICorner_3.CornerRadius = UDim.new(1, 0)
        UICorner_3.Parent = SliderBG
        
        local SliderFill = Instance.new("Frame")
        SliderFill.Name = "SliderFill"
        SliderFill.BackgroundColor3 = Color3.fromRGB(130, 100, 255)
        SliderFill.Size = UDim2.new((default - min)/(max - min), 0, 1, 0)
        SliderFill.Parent = SliderBG
        
        local UICorner_4 = Instance.new("UICorner")
        UICorner_4.CornerRadius = UDim.new(1, 0)
        UICorner_4.Parent = SliderFill
        
        local SliderButton = Instance.new("TextButton")
        SliderButton.Name = "SliderButton"
        SliderButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        SliderButton.Position = UDim2.new((default - min)/(max - min), -6, 0.5, -6)
        SliderButton.Size = UDim2.new(0, 12, 0, 12)
        SliderButton.Text = ""
        SliderButton.Parent = SliderBG
        
        local UICorner_5 = Instance.new("UICorner")
        UICorner_5.CornerRadius = UDim.new(1, 0)
        UICorner_5.Parent = SliderButton
        
        local function updateSlider(value)
            value = math.clamp(value, min, max)
            local percent = (value - min)/(max - min)
            
            SliderFill.Size = UDim2.new(percent, 0, 1, 0)
            SliderButton.Position = UDim2.new(percent, -6, 0.5, -6)
            SliderValueBox.Text = tostring(math.round(value))
            
            callback(value)
        end
        
        local dragging = false
        
        SliderButton.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 then
                dragging = true
            end
        end)
        
        UserInputService.InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 then
                dragging = false
            end
        end)
        
        UserInputService.InputChanged:Connect(function(input)
            if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
                local mousePos = UserInputService:GetMouseLocation()
                local relativePos = mousePos.X - SliderBG.AbsolutePosition.X
                local percent = math.clamp(relativePos / SliderBG.AbsoluteSize.X, 0, 1)
                local value = min + (max - min) * percent
                
                updateSlider(value)
            end
        end)
        
        SliderValueBox.FocusLost:Connect(function(enterPressed)
            if enterPressed then
                local value = tonumber(SliderValueBox.Text)
                if value then
                    updateSlider(value)
                end
            end
        end)
        
        return SliderFrame
    end
    
    function tab:AddDropdown(text, options, callback)
        local DropdownFrame = Instance.new("Frame")
        DropdownFrame.Name = text
        DropdownFrame.BackgroundColor3 = Color3.fromRGB(45, 45, 50)
        DropdownFrame.Size = UDim2.new(0.9, 0, 0, 30)
        DropdownFrame.Parent = TabContent
        
        local UICorner = Instance.new("UICorner")
        UICorner.CornerRadius = UDim.new(0, 4)
        UICorner.Parent = DropdownFrame
        
        local DropdownButton = Instance.new("TextButton")
        DropdownButton.Name = "DropdownButton"
        DropdownButton.BackgroundTransparency = 1
        DropdownButton.Size = UDim2.new(1, 0, 1, 0)
        DropdownButton.Font = Enum.Font.Gotham
        DropdownButton.Text = text
        DropdownButton.TextColor3 = Color3.fromRGB(255, 255, 255)
        DropdownButton.TextSize = 12
        DropdownButton.Parent = DropdownFrame
        
        local SearchBox = Instance.new("TextBox")
        SearchBox.Name = "SearchBox"
        SearchBox.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
        SearchBox.Position = UDim2.new(0, 5, 0, 35)
        SearchBox.Size = UDim2.new(1, -10, 0, 25)
        SearchBox.Font = Enum.Font.Gotham
        SearchBox.PlaceholderText = "Pesquisar..."
        SearchBox.Text = ""
        SearchBox.TextColor3 = Color3.fromRGB(255, 255, 255)
        SearchBox.TextSize = 12
        SearchBox.Visible = false
        SearchBox.Parent = DropdownFrame
        
        local UICorner_2 = Instance.new("UICorner")
        UICorner_2.CornerRadius = UDim.new(0, 4)
        UICorner_2.Parent = SearchBox
        
        local OptionList = Instance.new("ScrollingFrame")
        OptionList.Name = "OptionList"
        OptionList.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
        OptionList.Position = UDim2.new(0, 5, 0, 65)
        OptionList.Size = UDim2.new(1, -10, 0, 100)
        OptionList.CanvasSize = UDim2.new(0, 0, 0, 0)
        OptionList.ScrollBarThickness = 2
        OptionList.Visible = false
        OptionList.Parent = DropdownFrame
        
        local UIListLayout = Instance.new("UIListLayout")
        UIListLayout.Parent = OptionList
        UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
        UIListLayout.Padding = UDim.new(0, 2)
        
        local function updateOptions(filter)
            for _, child in pairs(OptionList:GetChildren()) do
                if child:IsA("TextButton") then
                    child:Destroy()
                end
            end
            
            for _, option in pairs(options) do
                if not filter or string.find(string.lower(option), string.lower(filter)) then
                    local OptionButton = Instance.new("TextButton")
                    OptionButton.BackgroundColor3 = Color3.fromRGB(45, 45, 50)
                    OptionButton.Size = UDim2.new(1, -4, 0, 25)
                    OptionButton.Font = Enum.Font.Gotham
                    OptionButton.Text = option
                    OptionButton.TextColor3 = Color3.fromRGB(255, 255, 255)
                    OptionButton.TextSize = 12
                    OptionButton.Parent = OptionList
                    
                    local UICorner = Instance.new("UICorner")
                    UICorner.CornerRadius = UDim.new(0, 4)
                    UICorner.Parent = OptionButton
                    
                    OptionButton.MouseButton1Click:Connect(function()
                        callback(option)
                        DropdownButton.Text = text .. ": " .. option
                        DropdownFrame.Size = UDim2.new(0.9, 0, 0, 30)
                        SearchBox.Visible = false
                        OptionList.Visible = false
                    end)
                end
            end
            
            OptionList.CanvasSize = UDim2.new(0, 0, 0, UIListLayout.AbsoluteContentSize.Y)
        end
        
        local dropped = false
        
        DropdownButton.MouseButton1Click:Connect(function()
            dropped = not dropped
            if dropped then
                DropdownFrame.Size = UDim2.new(0.9, 0, 0, 170)
                SearchBox.Visible = true
                OptionList.Visible = true
                updateOptions()
            else
                DropdownFrame.Size = UDim2.new(0.9, 0, 0, 30)
                SearchBox.Visible = false
                OptionList.Visible = false
            end
        end)
        
        SearchBox:GetPropertyChangedSignal("Text"):Connect(function()
            updateOptions(SearchBox.Text)
        end)
        
        return DropdownFrame
    end
    
    function tab:AddColorPicker(text, default, callback)
        local ColorPickerFrame = Instance.new("Frame")
        ColorPickerFrame.Name = text
        ColorPickerFrame.BackgroundColor3 = Color3.fromRGB(45, 45, 50)
        ColorPickerFrame.Size = UDim2.new(0.9, 0, 0, 30)
        ColorPickerFrame.Parent = TabContent
        
        local UICorner = Instance.new("UICorner")
        UICorner.CornerRadius = UDim.new(0, 4)
        UICorner.Parent = ColorPickerFrame
        
        local ColorButton = Instance.new("TextButton")
        ColorButton.Name = "ColorButton"
        ColorButton.BackgroundColor3 = default
        ColorButton.Position = UDim2.new(1, -35, 0.5, -10)
        ColorButton.Size = UDim2.new(0, 20, 0, 20)
        ColorButton.Text = ""
        ColorButton.Parent = ColorPickerFrame
        
        local UICorner_2 = Instance.new("UICorner")
        UICorner_2.CornerRadius = UDim.new(0, 4)
        UICorner_2.Parent = ColorButton
        
        local ColorLabel = Instance.new("TextLabel")
        ColorLabel.BackgroundTransparency = 1
        ColorLabel.Position = UDim2.new(0, 10, 0, 0)
        ColorLabel.Size = UDim2.new(1, -50, 1, 0)
        ColorLabel.Font = Enum.Font.Gotham
        ColorLabel.Text = text
        ColorLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
        ColorLabel.TextSize = 12
        ColorLabel.TextXAlignment = Enum.TextXAlignment.Left
        ColorLabel.Parent = ColorPickerFrame
        
        local ColorPicker = Instance.new("Frame")
        ColorPicker.Name = "ColorPicker"
        ColorPicker.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
        ColorPicker.Position = UDim2.new(0, 0, 1, 5)
        ColorPicker.Size = UDim2.new(1, 0, 0, 100)
        ColorPicker.Visible = false
        ColorPicker.Parent = ColorPickerFrame
        
        local UICorner_3 = Instance.new("UICorner")
        UICorner_3.CornerRadius = UDim.new(0, 4)
        UICorner_3.Parent = ColorPicker
        
        -- Add RGB sliders here (similar to regular slider but for R, G, B values)
        
        ColorButton.MouseButton1Click:Connect(function()
            ColorPicker.Visible = not ColorPicker.Visible
            if ColorPicker.Visible then
                ColorPickerFrame.Size = UDim2.new(0.9, 0, 0, 135)
            else
                ColorPickerFrame.Size = UDim2.new(0.9, 0, 0, 30)
            end
        end)
        
        return ColorPickerFrame
    end
    
    return tab
end

function Library:SelectTab(tab)
    if self.activeTab then
        self.activeTab.button.BackgroundColor3 = Color3.fromRGB(45, 45, 50)
        self.activeTab.content.Visible = false
    end
    
    tab.button.BackgroundColor3 = Color3.fromRGB(130, 100, 255)
    tab.content.Visible = true
    self.activeTab = tab
end

return Library
