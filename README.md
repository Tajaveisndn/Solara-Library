--[[
    Solara.lua
    Minimal C++-style UI Library for Roblox
    Version 1.0.0
]]

-- Services & Constants
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local CoreGui = game:GetService("CoreGui")

-- Theme
local Theme = {
    Primary     = Color3.fromRGB(147, 112, 219),   -- Lavender
    Background  = Color3.fromRGB(20, 20, 25),      -- Dark
    Surface     = Color3.fromRGB(30, 30, 35),      -- Surface
    Border      = Color3.fromRGB(50, 50, 55),      -- Border
    Text        = Color3.fromRGB(240, 240, 245),   -- Text
    Disabled    = Color3.fromRGB(80, 80, 85),      -- Disabled
    Accent      = Color3.fromRGB(186, 85, 211),    -- Accent
}

-- Animation configs
local Animate = {
    Short = TweenInfo.new(0.2, Enum.EasingStyle.Quart),
    Long  = TweenInfo.new(0.4, Enum.EasingStyle.Quart)
}

-- Core utility functions
local Utils = {}

function Utils:Create(class, properties)
    local instance = Instance.new(class)
    for k, v in pairs(properties) do
        instance[k] = v
    end
    return instance
end

-- Solara Library
local Solara = {
    Windows = {},
    Flags = {},
}

-- Window Class
function Solara.new(title)
    -- Core UI Elements
    local Window = Utils:Create("ScreenGui", {
        Name = "Solara",
        Parent = CoreGui
    })

    local Main = Utils:Create("Frame", {
        Name = "Main",
        Size = UDim2.new(0, 300, 0, 380),
        Position = UDim2.new(0.5, -150, 0.5, -190),
        BackgroundColor3 = Theme.Background,
        BorderSizePixel = 0,
        Parent = Window
    })

    -- Container with pixel-perfect shadow
    local Container = Utils:Create("Frame", {
        Name = "Container",
        Size = UDim2.new(1, -2, 1, -2),
        Position = UDim2.new(0, 1, 0, 1),
        BackgroundColor3 = Theme.Background,
        BorderSizePixel = 0,
        Parent = Main
    })

    -- Title bar
    local TitleBar = Utils:Create("Frame", {
        Name = "TitleBar",
        Size = UDim2.new(1, 0, 0, 30),
        BackgroundColor3 = Theme.Surface,
        BorderSizePixel = 0,
        Parent = Container
    })

    Utils:Create("TextLabel", {
        Position = UDim2.new(0, 10, 0, 0),
        Size = UDim2.new(1, -20, 1, 0),
        BackgroundTransparency = 1,
        Text = title,
        TextColor3 = Theme.Primary,
        TextSize = 13,
        Font = Enum.Font.Code,
        TextXAlignment = Enum.TextXAlignment.Left,
        Parent = TitleBar
    })

    -- Content
    local Content = Utils:Create("ScrollingFrame", {
        Position = UDim2.new(0, 0, 0, 30),
        Size = UDim2.new(1, 0, 1, -30),
        BackgroundTransparency = 1,
        ScrollBarThickness = 0,
        ScrollBarImageTransparency = 1,
        CanvasSize = UDim2.new(0, 0, 0, 0),
        Parent = Container
    })

    Utils:Create("UIPadding", {
        PaddingLeft = UDim.new(0, 10),
        PaddingRight = UDim.new(0, 10),
        PaddingTop = UDim.new(0, 10),
        PaddingBottom = UDim.new(0, 10),
        Parent = Content
    })

    Utils:Create("UIListLayout", {
        Padding = UDim.new(0, 8),
        Parent = Content
    })

    -- Window Methods
    local Window = {}
    local ElementY = 0

    function Window:Button(text, callback)
        local Button = Utils:Create("TextButton", {
            Size = UDim2.new(1, 0, 0, 30),
            BackgroundColor3 = Theme.Surface,
            BorderSizePixel = 0,
            Text = "",
            AutoButtonColor = false,
            Parent = Content
        })

        local Label = Utils:Create("TextLabel", {
            Position = UDim2.new(0, 10, 0, 0),
            Size = UDim2.new(1, -20, 1, 0),
            BackgroundTransparency = 1,
            Text = text,
            TextColor3 = Theme.Text,
            TextSize = 13,
            Font = Enum.Font.Code,
            TextXAlignment = Enum.TextXAlignment.Left,
            Parent = Button
        })

        -- Hover & Click Effects
        Button.MouseEnter:Connect(function()
            TweenService:Create(Button, Animate.Short, {
                BackgroundColor3 = Theme.Primary
            }):Play()
            TweenService:Create(Label, Animate.Short, {
                TextColor3 = Theme.Background
            }):Play()
        end)

        Button.MouseLeave:Connect(function()
            TweenService:Create(Button, Animate.Short, {
                BackgroundColor3 = Theme.Surface
            }):Play()
            TweenService:Create(Label, Animate.Short, {
                TextColor3 = Theme.Text
            }):Play()
        end)

        Button.MouseButton1Click:Connect(function()
            callback()
        end)

        ElementY = ElementY + 38
        Content.CanvasSize = UDim2.new(0, 0, 0, ElementY)
        return Button
    end

    function Window:Toggle(text, default, callback)
        local Toggle = Utils:Create("TextButton", {
            Size = UDim2.new(1, 0, 0, 30),
            BackgroundColor3 = Theme.Surface,
            BorderSizePixel = 0,
            Text = "",
            AutoButtonColor = false,
            Parent = Content
        })

        local Label = Utils:Create("TextLabel", {
            Position = UDim2.new(0, 10, 0, 0),
            Size = UDim2.new(1, -50, 1, 0),
            BackgroundTransparency = 1,
            Text = text,
            TextColor3 = Theme.Text,
            TextSize = 13,
            Font = Enum.Font.Code,
            TextXAlignment = Enum.TextXAlignment.Left,
            Parent = Toggle
        })

        local Indicator = Utils:Create("Frame", {
            Position = UDim2.new(1, -35, 0.5, -8),
            Size = UDim2.new(0, 25, 0, 16),
            BackgroundColor3 = default and Theme.Primary or Theme.Disabled,
            BorderSizePixel = 0,
            Parent = Toggle
        })

        local Circle = Utils:Create("Frame", {
            Position = UDim2.new(default and 1 or 0, default and -14 or 2, 0.5, -6),
            Size = UDim2.new(0, 12, 0, 12),
            BackgroundColor3 = Theme.Text,
            BorderSizePixel = 0,
            Parent = Indicator
        })

        -- Make circle round
        Utils:Create("UICorner", {
            CornerRadius = UDim.new(1, 0),
            Parent = Circle
        })

        -- Make indicator round
        Utils:Create("UICorner", {
            CornerRadius = UDim.new(1, 0),
            Parent = Indicator
        })

        local enabled = default
        Toggle.MouseButton1Click:Connect(function()
            enabled = not enabled
            
            TweenService:Create(Indicator, Animate.Short, {
                BackgroundColor3 = enabled and Theme.Primary or Theme.Disabled
            }):Play()

            TweenService:Create(Circle, Animate.Short, {
                Position = enabled and UDim2.new(1, -14, 0.5, -6) or UDim2.new(0, 2, 0.5, -6)
            }):Play()

            callback(enabled)
        end)

        ElementY = ElementY + 38
        Content.CanvasSize = UDim2.new(0, 0, 0, ElementY)
        return Toggle
    end
function Window:Slider(text, min, max, default, callback)
        local Slider = Utils:Create("Frame", {
            Size = UDim2.new(1, 0, 0, 40),
            BackgroundColor3 = Theme.Surface,
            BorderSizePixel = 0,
            Parent = Content
        })

        local Label = Utils:Create("TextLabel", {
            Position = UDim2.new(0, 10, 0, 0),
            Size = UDim2.new(1, -70, 0, 20),
            BackgroundTransparency = 1,
            Text = text,
            TextColor3 = Theme.Text,
            TextSize = 13,
            Font = Enum.Font.Code,
            TextXAlignment = Enum.TextXAlignment.Left,
            Parent = Slider
        })

        local Value = Utils:Create("TextLabel", {
            Position = UDim2.new(1, -60, 0, 0),
            Size = UDim2.new(0, 50, 0, 20),
            BackgroundTransparency = 1,
            Text = tostring(default),
            TextColor3 = Theme.Primary,
            TextSize = 13,
            Font = Enum.Font.Code,
            TextXAlignment = Enum.TextXAlignment.Right,
            Parent = Slider
        })

        local SliderBar = Utils:Create("Frame", {
            Position = UDim2.new(0, 10, 0, 25),
            Size = UDim2.new(1, -20, 0, 2),
            BackgroundColor3 = Theme.Border,
            BorderSizePixel = 0,
            Parent = Slider
        })

        local SliderFill = Utils:Create("Frame", {
            Size = UDim2.new((default - min)/(max - min), 0, 1, 0),
            BackgroundColor3 = Theme.Primary,
            BorderSizePixel = 0,
            Parent = SliderBar
        })

        local SliderButton = Utils:Create("TextButton", {
            Position = UDim2.new((default - min)/(max - min), -6, 0.5, -6),
            Size = UDim2.new(0, 12, 0, 12),
            BackgroundColor3 = Theme.Primary,
            BorderSizePixel = 0,
            Text = "",
            Parent = SliderBar
        })

        -- Make circle
        Utils:Create("UICorner", {
            CornerRadius = UDim.new(1, 0),
            Parent = SliderButton
        })

        -- Slider functionality
        local dragging = false

        SliderButton.MouseButton1Down:Connect(function()
            dragging = true
        end)

        UserInputService.InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 then
                dragging = false
            end
        end)

        UserInputService.InputChanged:Connect(function(input)
            if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
                local pos = math.clamp((input.Position.X - SliderBar.AbsolutePosition.X) / SliderBar.AbsoluteSize.X, 0, 1)
                local value = math.floor(min + (max - min) * pos)
                
                TweenService:Create(SliderFill, Animate.Short, {
                    Size = UDim2.new(pos, 0, 1, 0)
                }):Play()
                
                TweenService:Create(SliderButton, Animate.Short, {
                    Position = UDim2.new(pos, -6, 0.5, -6)
                }):Play()
                
                Value.Text = tostring(value)
                callback(value)
            end
        end)

        ElementY = ElementY + 48
        Content.CanvasSize = UDim2.new(0, 0, 0, ElementY)
        return Slider
    end

    function Window:Dropdown(text, options, callback)
        local Dropdown = Utils:Create("Frame", {
            Size = UDim2.new(1, 0, 0, 30),
            BackgroundColor3 = Theme.Surface,
            BorderSizePixel = 0,
            ClipsDescendants = true,
            Parent = Content
        })

        local DropButton = Utils:Create("TextButton", {
            Size = UDim2.new(1, 0, 0, 30),
            BackgroundTransparency = 1,
            Text = "",
            Parent = Dropdown
        })

        local Label = Utils:Create("TextLabel", {
            Position = UDim2.new(0, 10, 0, 0),
            Size = UDim2.new(1, -40, 0, 30),
            BackgroundTransparency = 1,
            Text = text .. ": " .. options[1],
            TextColor3 = Theme.Text,
            TextSize = 13,
            Font = Enum.Font.Code,
            TextXAlignment = Enum.TextXAlignment.Left,
            Parent = DropButton
        })

        local Arrow = Utils:Create("TextLabel", {
            Position = UDim2.new(1, -25, 0, 0),
            Size = UDim2.new(0, 20, 0, 30),
            BackgroundTransparency = 1,
            Text = "▼",
            TextColor3 = Theme.Primary,
            TextSize = 12,
            Font = Enum.Font.Code,
            Parent = DropButton
        })

        local OptionContainer = Utils:Create("Frame", {
            Position = UDim2.new(0, 0, 0, 30),
            Size = UDim2.new(1, 0, 0, #options * 25),
            BackgroundColor3 = Theme.Surface,
            BorderSizePixel = 0,
            Visible = false,
            Parent = Dropdown
        })

        local isOpen = false
        local selected = options[1]

        -- Create option buttons
        for i, option in ipairs(options) do
            local OptionButton = Utils:Create("TextButton", {
                Position = UDim2.new(0, 0, 0, (i-1) * 25),
                Size = UDim2.new(1, 0, 0, 25),
                BackgroundColor3 = Theme.Surface,
                BackgroundTransparency = 0.5,
                BorderSizePixel = 0,
                Text = "",
                Parent = OptionContainer
            })

            local OptionLabel = Utils:Create("TextLabel", {
                Position = UDim2.new(0, 10, 0, 0),
                Size = UDim2.new(1, -20, 1, 0),
                BackgroundTransparency = 1,
                Text = option,
                TextColor3 = Theme.Text,
                TextSize = 13,
                Font = Enum.Font.Code,
                TextXAlignment = Enum.TextXAlignment.Left,
                Parent = OptionButton
            })

            OptionButton.MouseEnter:Connect(function()
                TweenService:Create(OptionButton, Animate.Short, {
                    BackgroundColor3 = Theme.Primary
                }):Play()
            end)

            OptionButton.MouseLeave:Connect(function()
                TweenService:Create(OptionButton, Animate.Short, {
                    BackgroundColor3 = Theme.Surface
                }):Play()
            end)

            OptionButton.MouseButton1Click:Connect(function()
                selected = option
                Label.Text = text .. ": " .. selected
                
                -- Close dropdown
                TweenService:Create(Dropdown, Animate.Short, {
                    Size = UDim2.new(1, 0, 0, 30)
                }):Play()
                
                TweenService:Create(Arrow, Animate.Short, {
                    Rotation = 0
                }):Play()
                
                isOpen = false
                callback(selected)
            end)
        end

        DropButton.MouseButton1Click:Connect(function()
            isOpen = not isOpen
            
            TweenService:Create(Dropdown, Animate.Short, {
                Size = isOpen and UDim2.new(1, 0, 0, 30 + #options * 25) or UDim2.new(1, 0, 0, 30)
            }):Play()
            
            TweenService:Create(Arrow, Animate.Short, {
                Rotation = isOpen and 180 or 0
            }):Play()
            
            OptionContainer.Visible = true
        end)

        ElementY = ElementY + 38
        Content.CanvasSize = UDim2.new(0, 0, 0, ElementY)
        return Dropdown
    end

    -- Make window draggable
    local dragging = false
    local dragStart = nil
    local startPos = nil

    TitleBar.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
            dragStart = input.Position
            startPos = Main.Position
        end
    end)

    UserInputService.InputChanged:Connect(function(input)
        if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
            local delta = input.Position - dragStart
            Main.Position = UDim2.new(
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

    return Window
end

return Solara
