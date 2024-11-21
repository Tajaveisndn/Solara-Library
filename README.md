-- Services
local TweenService = game:GetService("TweenService")
local UIS = game:GetService("UserInputService")
local CoreGui = game:GetService("CoreGui")

-- Configurações
local Config = {
    Name = "Nyox Library",
    Theme = {
        Primary = Color3.fromRGB(147, 51, 234),     -- Roxo principal
        Secondary = Color3.fromRGB(123, 31, 204),   -- Roxo mais escuro
        Background = Color3.fromRGB(25, 25, 25),    -- Fundo escuro
        LightBG = Color3.fromRGB(30, 30, 30),       -- Fundo um pouco mais claro
        DarkBG = Color3.fromRGB(20, 20, 20),        -- Fundo mais escuro
        Text = Color3.fromRGB(255, 255, 255),       -- Texto branco
        DarkText = Color3.fromRGB(180, 180, 180),   -- Texto cinza
        Border = Color3.fromRGB(50, 50, 50),        -- Bordas
        PlaceholderText = Color3.fromRGB(120, 120, 120) -- Texto de placeholder
    },
    Font = Enum.Font.GothamBold,
    TextSize = 14,
    ElementHeight = 32,
    Padding = 5,
    CornerRadius = 5,
    AnimationSpeed = 0.2
}

-- Utilitários
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
function Utility.Tween(instance, properties, duration, style)
    local tween = TweenService:Create(
        instance,
        TweenInfo.new(
            duration or Config.AnimationSpeed,
            style or Enum.EasingStyle.Quart,
            Enum.EasingDirection.Out
        ),
        properties
    )
    tween:Play()
    return tween
end

-- Adicionar cantos arredondados
function Utility.AddCorner(instance, radius)
    local corner = Utility.Create("UICorner", {
        CornerRadius = UDim.new(0, radius or Config.CornerRadius),
        Parent = instance
    })
    return corner
end

-- Adicionar stroke (borda)
function Utility.AddStroke(instance, color, thickness)
    local stroke = Utility.Create("UIStroke", {
        Color = color or Config.Theme.Border,
        Thickness = thickness or 1,
        Parent = instance
    })
    return stroke
end

-- Adicionar padding
function Utility.AddPadding(instance, padding)
    local uiPadding = Utility.Create("UIPadding", {
        PaddingLeft = UDim.new(0, padding or Config.Padding),
        PaddingRight = UDim.new(0, padding or Config.Padding),
        PaddingTop = UDim.new(0, padding or Config.Padding),
        PaddingBottom = UDim.new(0, padding or Config.Padding),
        Parent = instance
    })
    return uiPadding
end

-- Adicionar efeito hover
function Utility.AddHover(button, colorLight, colorDark)
    button.MouseEnter:Connect(function()
        Utility.Tween(button, {BackgroundColor3 = colorLight}, 0.2)
    end)
    
    button.MouseLeave:Connect(function()
        Utility.Tween(button, {BackgroundColor3 = colorDark}, 0.2)
    end)
end

-- Adicionar sombra
function Utility.AddShadow(instance, transparency)
    local shadow = Utility.Create("ImageLabel", {
        Name = "Shadow",
        BackgroundTransparency = 1,
        Image = "rbxassetid://6015897843",
        ImageColor3 = Color3.new(0, 0, 0),
        ImageTransparency = transparency or 0.5,
        Size = UDim2.new(1, 47, 1, 47),
        Position = UDim2.new(0, -23, 0, -23),
        ZIndex = instance.ZIndex - 1,
        Parent = instance
    })
    return shadow
end

-- Adicionar gradiente
function Utility.AddGradient(instance, color1, color2, rotation)
    local gradient = Utility.Create("UIGradient", {
        Color = ColorSequence.new({
            ColorSequenceKeypoint.new(0, color1 or Config.Theme.Primary),
            ColorSequenceKeypoint.new(1, color2 or Config.Theme.Secondary)
        }),
        Rotation = rotation or 45,
        Parent = instance
    })
    return gradient
end

-- Detectar tecla pressionada
function Utility.KeyPressed(key, callback)
    UIS.InputBegan:Connect(function(input, gameProcessed)
        if not gameProcessed and input.KeyCode == key then
            callback()
        end
    end)
end

return {
    Config = Config,
    Utility = Utility
}
