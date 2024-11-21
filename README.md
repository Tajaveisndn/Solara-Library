local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local CoreGui = game:GetService("CoreGui")

local Library = {}
Library.__index = Library

local function Tween(obj, time, properties)
    local tweenInfo = TweenInfo.new(time, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
    local tween = TweenService:Create(obj, tweenInfo, properties)
    tween:Play()
    return tween
end

function Library.new(title)
    local NyoxLibrary = Instance.new("ScreenGui")
    NyoxLibrary.Name = "NyoxLibrary"
    NyoxLibrary.Parent = CoreGui
    NyoxLibrary.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    
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
    
    local TopBar = Instance.new("Frame")
    TopBar.Name = "TopBar"
    TopBar.BackgroundColor3 = Color3.fromRGB(40, 40, 45)
    TopBar.BorderSizePixel = 0
    TopBar.Size = UDim2.new(1, 0, 0, 30)
    TopBar.Parent = Main
    
    local UICorner_2 = Instance.new("UICorner")
    UICorner_2.CornerRadius = UDim.new(0, 6)
    UICorner_2.Parent = TopBar
    
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
    
    -- Make GUI draggable
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
    
    -- Close button
    CloseButton.MouseButton1Click:Connect(function()
        NyoxLibrary:Destroy()
    end)
    
    -- Minimize button
    MinimizeButton.MouseButton1Click:Connect(function()
        lib:Minimize()
    end)
    
    return lib
end

function Library:Minimize()
    self.minimized = not self.minimized
    if self.minimized then
        Tween(self.main, 0.3, {Size = UDim2.new(0, 500, 0, 30)})
    else
        Tween(self.main, 0.3, {Size = UDim2.new(0, 500, 0, 350)})
    end
end

function Library:CreateTab(name)
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
    
    local TabContent = Instance.new("ScrollingFrame")
    TabContent.Name = name
    TabContent.BackgroundTransparency = 1
    TabContent.BorderSizePixel = 0
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
    
    local tab = {
        button = TabButton,
        content = TabContent
    }
    
    table.insert(self.tabs, tab)
    
    if #self.tabs == 1 then
        self:SelectTab(tab)
    end
    
    TabButton.MouseButton1Click:Connect(function()
        self:SelectTab(tab)
    end)
    
    return tab
end

function Library:SelectTab(tab)
    if self.activeTab then
        self.activeTab.button.BackgroundColor3 = Color3.fromRGB(45, 45, 50)
        self.activeTab.content.Visible = false
    end
    
    tab.button.BackgroundColor3 = Color3.fromRGB(100, 100, 255)
    tab.content.Visible = true
    self.activeTab = tab
end

return Library
