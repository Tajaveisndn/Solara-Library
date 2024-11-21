-- Configurações e Utilitários
local Config = {
    Theme = {
        Primary = Color3.fromRGB(147, 51, 234),    -- Roxo principal
        Secondary = Color3.fromRGB(123, 31, 204),  -- Roxo secundário
        Background = Color3.fromRGB(30, 30, 30),   -- Fundo escuro
        LightBackground = Color3.fromRGB(40, 40, 40), -- Fundo claro
        Text = Color3.fromRGB(255, 255, 255),      -- Texto branco
        DarkText = Color3.fromRGB(200, 200, 200)   -- Texto cinza
    },
    Font = Enum.Font.GothamBold,
    TabIndicatorSize = UDim2.new(0, 15, 0, 15),
    AnimationSpeed = 0.3,
}

-- Utilitários
local TweenService = game:GetService("TweenService")
local UIS = game:GetService("UserInputService")

local Utility = {}

-- Criar elementos UI com propriedades padrão
function Utility.Create(className, properties)
    local instance = Instance.new(className)
    for k, v in pairs(properties or {}) do
        instance[k] = v
    end
    return instance
end

-- Criar tweens com configurações padrão
function Utility.Tween(instance, properties, duration)
    local tween = TweenService:Create(
        instance,
        TweenInfo.new(duration or Config.AnimationSpeed, Enum.EasingStyle.Quart),
        properties
    )
    tween:Play()
    return tween
end

-- Criar efeitos hover
function Utility.AddHoverEffect(button, hoverColor, normalColor)
    button.MouseEnter:Connect(function()
        Utility.Tween(button, {BackgroundColor3 = hoverColor}, 0.2)
    end)
    
    button.MouseLeave:Connect(function()
        Utility.Tween(button, {BackgroundColor3 = normalColor}, 0.2)
    end)
end

-- Criar cantos arredondados
function Utility.MakeRound(instance, radius)
    local corner = Utility.Create("UICorner", {
        CornerRadius = UDim.new(0, radius or 6),
        Parent = instance
    })
    return corner
end

-- Criar sombra
function Utility.AddShadow(instance, transparency)
    local shadow = Utility.Create("ImageLabel", {
        Name = "Shadow",
        AnchorPoint = Vector2.new(0.5, 0.5),
        BackgroundTransparency = 1,
        Position = UDim2.new(0.5, 0, 0.5, 0),
        Size = UDim2.new(1, 24, 1, 24),
        ZIndex = -1,
        Image = "rbxassetid://6014261993",
        ImageColor3 = Color3.fromRGB(0, 0, 0),
        ImageTransparency = transparency or 0.5,
        Parent = instance
    })
    return shadow
end

-- Criar gradiente
function Utility.AddGradient(instance, colorSequence)
    local gradient = Utility.Create("UIGradient", {
        Color = colorSequence or ColorSequence.new{
            ColorSequenceKeypoint.new(0, Config.Theme.Primary),
            ColorSequenceKeypoint.new(1, Config.Theme.Secondary)
        },
        Parent = instance
    })
    return gradient
end

return {
    Config = Config,
    Utility = Utility
}
