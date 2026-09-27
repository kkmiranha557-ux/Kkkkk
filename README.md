--[[
    ═══════════════════════════════════════════════════════════════
    🍎 BLOX FRUITS - FRUIT ESP & TELEPORT SYSTEM
    ═══════════════════════════════════════════════════════════════
    📁 Localização: StarterPlayer → StarterPlayerScripts
    💻 Compatível com: PC, Android, iOS
    ⚠️  AVISO: Script para uso educacional. Use por sua conta e risco.
    ═══════════════════════════════════════════════════════════════
]]

-- ═══════════════════════════════════════════════════════════════
-- ⚙️ SEÇÃO 1: CONFIGURAÇÕES PRINCIPAIS
-- ═══════════════════════════════════════════════════════════════

local CONFIG = {
    -- UI
    UI_TITLE          = "🍎 FRUIT HUB",
    UI_VERSION        = "v1.0",
    ACCENT_COLOR      = Color3.fromRGB(120, 80, 255),
    SECONDARY_COLOR   = Color3.fromRGB(45, 45, 60),
    BG_COLOR          = Color3.fromRGB(20, 20, 28),
    TEXT_COLOR        = Color3.fromRGB(240, 240, 255),
    SUCCESS_COLOR     = Color3.fromRGB(80, 220, 120),
    DANGER_COLOR      = Color3.fromRGB(255, 80, 80),
    WARNING_COLOR     = Color3.fromRGB(255, 180, 60),
    
    -- ESP
    ESP_UPDATE_RATE   = 0.3,       -- segundos entre atualizações
    MAX_ESP_DISTANCE  = 5000,      -- distância máxima para mostrar frutas
    TEXT_SIZE         = 14,
    HIGHLIGHT_FILL    = 0.4,
    HIGHLIGHT_OUTLINE = 1.5,
    
    -- Teleporte
    TP_OFFSET_Y       = 3,         -- altura do teleporte acima da fruta
    TP_COOLDOWN       = 0.5,       -- cooldown entre teleportes
    
    -- Filtro padrão (raridades)
    FILTER_RARITY     = {
        Common    = false,
        Uncommon  = false,
        Rare      = true,
        Legendary = true,
        Mythical  = true,
    },
}

-- ═══════════════════════════════════════════════════════════════
-- 📋 SEÇÃO 2: BASE DE DADOS DE FRUTAS (Raridades & Cores)
-- ═══════════════════════════════════════════════════════════════

local FRUIT_DATA = {
    -- Mythical
    ["Dragon"]        = {Rarity = "Mythical",  Color = Color3.fromRGB(255, 60, 60)},
    ["Leopard"]       = {Rarity = "Mythical",  Color = Color3.fromRGB(255, 100, 200)},
    ["Kitsune"]       = {Rarity = "Mythical",  Color = Color3.fromRGB(255, 200, 100)},
    ["Dough"]         = {Rarity = "Mythical",  Color = Color3.fromRGB(255, 220, 180)},
    ["Mammoth"]       = {Rarity = "Mythical",  Color = Color3.fromRGB(180, 120, 80)},
    ["T-Rex"]         = {Rarity = "Mythical",  Color = Color3.fromRGB(100, 180, 100)},
    ["Gravity"]       = {Rarity = "Mythical",  Color = Color3.fromRGB(150, 100, 255)},
    
    -- Legendary
    ["Venom"]         = {Rarity = "Legendary", Color = Color3.fromRGB(150, 60, 200)},
    ["Control"]       = {Rarity = "Legendary", Color = Color3.fromRGB(200, 200, 220)},
    ["Spirit"]        = {Rarity = "Legendary", Color = Color3.fromRGB(200, 230, 255)},
    ["Shadow"]        = {Rarity = "Legendary", Color = Color3.fromRGB(80, 60, 120)},
    ["Buddha"]        = {Rarity = "Legendary", Color = Color3.fromRGB(255, 220, 120)},
    ["Portal"]        = {Rarity = "Legendary", Color = Color3.fromRGB(180, 120, 255)},
    ["Blizzard"]      = {Rarity = "Legendary", Color = Color3.fromRGB(150, 220, 255)},
    
    -- Rare
    ["Phoenix"]       = {Rarity = "Rare",      Color = Color3.fromRGB(255, 150, 80)},
    ["Rumble"]        = {Rarity = "Rare",      Color = Color3.fromRGB(180, 180, 220)},
    ["Pain"]          = {Rarity = "Rare",      Color = Color3.fromRGB(200, 80, 80)},
    ["Blade"]         = {Rarity = "Rare",      Color = Color3.fromRGB(180, 180, 200)},
    ["Love"]          = {Rarity = "Rare",      Color = Color3.fromRGB(255, 150, 200)},
    ["Spider"]        = {Rarity = "Rare",      Color = Color3.fromRGB(120, 60, 60)},
    ["Sound"]         = {Rarity = "Rare",      Color = Color3.fromRGB(200, 150, 255)},
    ["Magma"]         = {Rarity = "Rare",      Color = Color3.fromRGB(255, 100, 40)},
    ["Quake"]         = {Rarity = "Rare",      Color = Color3.fromRGB(220, 220, 255)},
    
    -- Uncommon
    ["Door"]          = {Rarity = "Uncommon",  Color = Color3.fromRGB(180, 180, 180)},
    ["Rubber"]        = {Rarity = "Uncommon",  Color = Color3.fromRGB(255, 120, 120)},
    ["Barrier"]       = {Rarity = "Uncommon",  Color = Color3.fromRGB(200, 200, 255)},
    ["Ghost"]         = {Rarity = "Uncommon",  Color = Color3.fromRGB(220, 220, 240)},
    ["Diamond"]       = {Rarity = "Uncommon",  Color = Color3.fromRGB(150, 220, 255)},
    ["Light"]         = {Rarity = "Uncommon",  Color = Color3.fromRGB(255, 255, 180)},
    
    -- Common
    ["Rocket"]        = {Rarity = "Common",    Color = Color3.fromRGB(180, 180, 180)},
    ["Spin"]          = {Rarity = "Common",    Color = Color3.fromRGB(200, 200, 200)},
    ["Chop"]          = {Rarity = "Common",    Color = Color3.fromRGB(200, 180, 150)},
    ["Spring"]        = {Rarity = "Common",    Color = Color3.fromRGB(255, 180, 200)},
    ["Bomb"]          = {Rarity = "Common",    Color = Color3.fromRGB(120, 120, 120)},
    ["Smoke"]         = {Rarity = "Common",    Color = Color3.fromRGB(180, 180, 200)},
    ["Spike"]         = {Rarity = "Common",    Color = Color3.fromRGB(200, 200, 200)},
    ["Flame"]         = {Rarity = "Common",    Color = Color3.fromRGB(255, 150, 60)},
    ["Ice"]           = {Rarity = "Common",    Color = Color3.fromRGB(150, 220, 255)},
    ["Sand"]          = {Rarity = "Common",    Color = Color3.fromRGB(220, 200, 150)},
    ["Dark"]          = {Rarity = "Common",    Color = Color3.fromRGB(100, 80, 150)},
    ["Revive"]        = {Rarity = "Common",    Color = Color3.fromRGB(200, 255, 200)},
    ["Falcon"]        = {Rarity = "Common",    Color = Color3.fromRGB(180, 200, 220)},
}

-- ═══════════════════════════════════════════════════════════════
-- 📦 SEÇÃO 3: SERVIÇOS E VARIÁVEIS GLOBAIS
-- ═══════════════════════════════════════════════════════════════

local Players           = game:GetService("Players")
local RunService        = game:GetService("RunService")
local UserInputService  = game:GetService("UserInputService")
local TweenService      = game:GetService("TweenService")
local Workspace         = game:GetService("Workspace")
local CoreGui           = game:GetService("CoreGui")
local Lighting          = game:GetService("Lighting")

local LocalPlayer       = Players.LocalPlayer
local Camera            = Workspace.CurrentCamera

-- Estado
local State = {
    ESPEnabled       = true,
    HighlightEnabled = true,
    TextEnabled      = true,
    SearchFilter     = "",
    SelectedFruit    = nil,
    LastTeleport     = 0,
    ActiveFruits     = {},   -- [fruitModel] = {highlight, billboard, name, distance}
    FruitListItems   = {},   -- [fruitModel] = uiButton
}

-- ═══════════════════════════════════════════════════════════════
-- 🛠️ SEÇÃO 4: FUNÇÕES UTILITÁRIAS
-- ═══════════════════════════════════════════════════════════════

local function getRarityColor(rarity)
    local colors = {
        Mythical  = Color3.fromRGB(255, 60, 60),
        Legendary = Color3.fromRGB(255, 180, 60),
        Rare      = Color3.fromRGB(80, 180, 255),
        Uncommon  = Color3.fromRGB(120, 255, 120),
        Common    = Color3.fromRGB(180, 180, 180),
    }
    return colors[rarity] or Color3.fromRGB(200, 200, 200)
end

local function getFruitInfo(name)
    -- Remove sufixos comuns (ex: "Dragon-Dragon Fruit")
    local cleanName = name:gsub("%s*Fruit.*", ""):gsub("%-.*", ""):gsub("%s+$", "")
    
    for fruitName, data in pairs(FRUIT_DATA) do
        if cleanName:lower():find(fruitName:lower(), 1, true) or 
           fruitName:lower():find(cleanName:lower(), 1, true) then
            return fruitName, data
        end
    end
    return cleanName, {Rarity = "Common", Color = Color3.fromRGB(200, 200, 200)}
end

local function isFruitObject(obj)
    if not obj or not obj:IsA("Model") and not obj:IsA("Tool") and not obj:IsA("BasePart") then
        return false
    end
    
    local name = obj.Name
    -- Frutas normalmente têm "Fruit" no nome ou são Tools/Models com Handle
    if name:lower():find("fruit") then return true end
    
    -- Verifica por nome conhecido
    for fruitName, _ in pairs(FRUIT_DATA) do
        if name:lower():find(fruitName:lower(), 1, true) then return true end
    end
    
    return false
end

local function getFruitHandle(obj)
    if obj:IsA("BasePart") then return obj end
    if obj:IsA("Tool") then return obj:FindFirstChild("Handle") or obj end
    if obj:IsA("Model") then
        return obj.PrimaryPart or obj:FindFirstChildWhichIsA("BasePart")
    end
    return nil
end

-- ═══════════════════════════════════════════════════════════════
-- 🎨 SEÇÃO 5: CONSTRUÇÃO DA INTERFACE (UI)
-- ═══════════════════════════════════════════════════════════════

-- Remove UI anterior se existir
if CoreGui:FindFirstChild("FruitHubUI") then
    CoreGui.FruitHubUI:Destroy()
end

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "FruitHubUI"
ScreenGui.ResetOnSpawn = false
ScreenGui.IgnoreGuiInset = true
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.Parent = CoreGui

-- ─── Helper para criar instâncias ───
local function new(class, props)
    local inst = Instance.new(class)
    for k, v in pairs(props or {}) do
        inst[k] = v
    end
    return inst
end

-- ─── Toggle Button (abre/fecha o painel) ───
local ToggleBtn = new("TextButton", {
    Name = "ToggleBtn",
    Parent = ScreenGui,
    Size = UDim2.new(0, 55, 0, 55),
    Position = UDim2.new(0, 20, 0.5, -27),
    BackgroundColor3 = CONFIG.ACCENT_COLOR,
    Text = "🍎",
    TextSize = 26,
    Font = Enum.Font.GothamBold,
    TextColor3 = Color3.new(1, 1, 1),
    AutoButtonColor = false,
    ZIndex = 10,
})

new("UICorner", {CornerRadius = UDim.new(0, 14), Parent = ToggleBtn})
new("UIStroke", {Color = Color3.fromRGB(180, 140, 255), Thickness = 1.5, Parent = ToggleBtn})

local toggleShadow = new("ImageLabel", {
    Parent = ToggleBtn,
    Size = UDim2.new(1, 20, 1, 20),
    Position = UDim2.new(0, -10, 0, -10),
    BackgroundTransparency = 1,
    Image = "rbxassetid://5028857084",
    ImageColor3 = Color3.new(0, 0, 0),
    ImageTransparency = 0.6,
    ZIndex = 9,
})

-- ─── Painel Principal ───
local Panel = new("Frame", {
    Name = "MainPanel",
    Parent = ScreenGui,
    Size = UDim2.new(0, 480, 0, 380),
    Position = UDim2.new(0.5, -240, 0.5, -190),
    BackgroundColor3 = CONFIG.BG_COLOR,
    BorderSizePixel = 0,
    ClipsDescendants = true,
    ZIndex = 5,
})

new("UICorner", {CornerRadius = UDim.new(0, 14), Parent = Panel})
new("UIStroke", {Color = CONFIG.ACCENT_COLOR, Thickness = 1.5, Transparency = 0.3, Parent = Panel})

-- Sombra
new("ImageLabel", {
    Parent = Panel,
    Size = UDim2.new(1, 30, 1, 30),
    Position = UDim2.new(0, -15, 0, -15),
    BackgroundTransparency = 1,
    Image = "rbxassetid://5028857084",
    ImageColor3 = Color3.new(0, 0, 0),
    ImageTransparency = 0.5,
    ZIndex = 4,
})

-- ─── Header ───
local Header = new("Frame", {
    Name = "Header",
    Parent = Panel,
    Size = UDim2.new(1, 0, 0, 45),
    BackgroundColor3 = CONFIG.SECONDARY_COLOR,
    BorderSizePixel = 0,
    ZIndex = 6,
})

new("UICorner", {CornerRadius = UDim.new(0, 14), Parent = Header})

-- Corrige cantos inferiores do header
new("Frame", {
    Parent = Header,
    Size = UDim2.new(1, 0, 0, 15),
    Position = UDim2.new(0, 0, 1, -15),
    BackgroundColor3 = CONFIG.SECONDARY_COLOR,
    BorderSizePixel = 0,
    ZIndex = 6,
})

local Title = new("TextLabel", {
    Parent = Header,
    Size = UDim2.new(1, -100, 1, 0),
    Position = UDim2.new(0, 15, 0, 0),
    BackgroundTransparency = 1,
    Text = CONFIG.UI_TITLE .. "  " .. CONFIG.UI_VERSION,
    TextColor3 = CONFIG.TEXT_COLOR,
    TextSize = 16,
    Font = Enum.Font.GothamBold,
    TextXAlignment = Enum.TextXAlignment.Left,
    ZIndex = 7,
})

-- Botão Minimizar
local MinimizeBtn = new("TextButton", {
    Parent = Header,
    Size = UDim2.new(0, 30, 0, 30),
    Position = UDim2.new(1, -75, 0, 8),
    BackgroundColor3 = Color3.fromRGB(80, 80, 100),
    Text = "—",
    TextSize = 18,
    Font = Enum.Font.GothamBold,
    TextColor3 = Color3.new(1, 1, 1),
    AutoButtonColor = false,
    ZIndex = 7,
})
new("UICorner", {CornerRadius = UDim.new(0, 8), Parent = MinimizeBtn})

-- Botão Fechar
local CloseBtn = new("TextButton", {
    Parent = Header,
    Size = UDim2.new(0, 30, 0, 30),
    Position = UDim2.new(1, -40, 0, 8),
    BackgroundColor3 = CONFIG.DANGER_COLOR,
    Text = "✕",
    TextSize = 16,
    Font = Enum.Font.GothamBold,
    TextColor3 = Color3.new(1, 1, 1),
    AutoButtonColor = false,
    ZIndex = 7,
})
new("UICorner", {CornerRadius = UDim.new(0, 8), Parent = CloseBtn})

-- ─── Sistema de Arrastar (PC + Mobile) ───
local dragging, dragStart, startPos = false, nil, nil

local function startDrag(input)
    dragging = true
    dragStart = input.Position
    startPos = Panel.Position
end

local function updateDrag(input)
    if not dragging then return end
    local delta = input.Position - dragStart
    Panel.Position = UDim2.new(
        startPos.X.Scale, startPos.X.Offset + delta.X,
        startPos.Y.Scale, startPos.Y.Offset + delta.Y
    )
end

local function endDrag()
    dragging = false
end

Header.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 
    or input.UserInputType == Enum.UserInputType.Touch then
        startDrag(input)
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement 
    or input.UserInputType == Enum.UserInputType.Touch) then
        updateDrag(input)
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 
    or input.UserInputType == Enum.UserInputType.Touch then
        endDrag()
    end
end)

-- ─── Sistema de Abas ───
local TabBar = new("Frame", {
    Parent = Panel,
    Size = UDim2.new(1, -20, 0, 40),
    Position = UDim2.new(0, 10, 0, 50),
    BackgroundTransparency = 1,
    ZIndex = 6,
})

new("UIListLayout", {
    Parent = TabBar,
    FillDirection = Enum.FillDirection.Horizontal,
    Padding = UDim.new(0, 8),
    SortOrder = Enum.SortOrder.LayoutOrder,
})

local ContentHolder = new("Frame", {
    Parent = Panel,
    Size = UDim2.new(1, -20, 1, -105),
    Position = UDim2.new(0, 10, 0, 95),
    BackgroundTransparency = 1,
    ClipsDescendants = true,
    ZIndex = 6,
})

-- Container de conteúdo por aba
local TabContent = {}

local function createTab(name, icon)
    local btn = new("TextButton", {
        Parent = TabBar,
        Size = UDim2.new(0, 130, 1, 0),
        BackgroundColor3 = CONFIG.SECONDARY_COLOR,
        Text = icon .. " " .. name,
        TextColor3 = CONFIG.TEXT_COLOR,
        TextSize = 13,
        Font = Enum.Font.GothamSemibold,
        AutoButtonColor = false,
        ZIndex = 7,
    })
    new("UICorner", {CornerRadius = UDim.new(0, 8), Parent = btn})
    local stroke = new("UIStroke", {Color = CONFIG.ACCENT_COLOR, Thickness = 0, Parent = btn})
    
    local content = new("ScrollingFrame", {
        Parent = ContentHolder,
        Size = UDim2.new(1, 0, 1, 0),
        BackgroundTransparency = 1,
        BorderSizePixel = 0,
        ScrollBarThickness = 4,
        ScrollBarImageColor3 = CONFIG.ACCENT_COLOR,
        CanvasSize = UDim2.new(0, 0, 0, 0),
        AutomaticCanvasSize = Enum.AutomaticSize.Y,
        Visible = false,
        ZIndex = 6,
    })
    new("UIListLayout", {
        Parent = content,
        Padding = UDim.new(0, 6),
        SortOrder = Enum.SortOrder.LayoutOrder,
    })
    new("UIPadding", {
        Parent = content,
        PaddingTop = UDim.new(0, 5),
        PaddingBottom = UDim.new(0, 5),
        PaddingLeft = UDim.new(0, 3),
        PaddingRight = UDim.new(0, 3),
    })
    
    TabContent[name] = {Button = btn, Content = content, Stroke = stroke}
    
    btn.MouseButton1Click:Connect(function()
        for tabName, tabData in pairs(TabContent) do
            local active = (tabName == name)
            tabData.Content.Visible = active
            TweenService:Create(tabData.Stroke, TweenInfo.new(0.2), 
                {Thickness = active and 1.5 or 0}):Play()
            TweenService:Create(tabData.Button, TweenInfo.new(0.2), 
                {BackgroundColor3 = active and CONFIG.ACCENT_COLOR or CONFIG.SECONDARY_COLOR}):Play()
        end
    end)
    
    return content
end

local FruitsTab = createTab("FRUTAS", "🍎")
local ConfigTab = createTab("CONFIG", "⚙️")

-- Ativar primeira aba
TabContent["FRUTAS"].Content.Visible = true
TabContent["FRUTAS"].Stroke.Thickness = 1.5
TabContent["FRUTAS"].Button.BackgroundColor3 = CONFIG.ACCENT_COLOR

-- ─── Animações dos botões ───
local function buttonHover(btn, normalColor, hoverColor)
    btn.MouseEnter:Connect(function()
        TweenService:Create(btn, TweenInfo.new(0.15), {BackgroundColor3 = hoverColor}):Play()
    end)
    btn.MouseLeave:Connect(function()
        TweenService:Create(btn, TweenInfo.new(0.15), {BackgroundColor3 = normalColor}):Play()
    end)
end

buttonHover(MinimizeBtn, Color3.fromRGB(80,80,100), Color3.fromRGB(110,110,140))
buttonHover(CloseBtn, CONFIG.DANGER_COLOR, Color3.fromRGB(255, 120, 120))
buttonHover(ToggleBtn, CONFIG.ACCENT_COLOR, Color3.fromRGB(150, 110, 255))

-- ─── Minimizar ───
local minimized = false
MinimizeBtn.MouseButton1Click:Connect(function()
    minimized = not minimized
    local targetSize = minimized and UDim2.new(0, 480, 0, 45) or UDim2.new(0, 480, 0, 380)
    TweenService:Create(Panel, TweenInfo.new(0.25, Enum.EasingStyle.Quad), {Size = targetSize}):Play()
    MinimizeBtn.Text = minimized and "+" or "—"
    if minimized then
        -- Salva posição anterior
        local contentSize = Panel.AbsolutePosition
        TweenService:Create(ContentHolder, TweenInfo.new(0.15), 
            {BackgroundTransparency = 1}):Play()
    end
end)

-- ─── Fechar / Abrir painel ───
local panelOpen = true
CloseBtn.MouseButton1Click:Connect(function()
    panelOpen = false
    TweenService:Create(Panel, TweenInfo.new(0.25), {
        Size = UDim2.new(0, 0, 0, 0),
        Position = UDim2.new(Panel.Position.X.Scale, Panel.Position.X.Offset + 240, 
                             Panel.Position.Y.Scale, Panel.Position.Y.Offset + 190),
    }):Play()
    task.wait(0.26)
    Panel.Visible = false
end)

ToggleBtn.MouseButton1Click:Connect(function()
    if panelOpen then
        -- fecha
        panelOpen = false
        TweenService:Create(Panel, TweenInfo.new(0.25), {
            Size = UDim2.new(0, 0, 0, 0),
        }):Play()
        task.wait(0.26)
        Panel.Visible = false
    else
        -- abre
        panelOpen = true
        Panel.Visible = true
        Panel.Size = UDim2.new(0, 0, 0, 0)
        TweenService:Create(Panel, TweenInfo.new(0.3, Enum.EasingStyle.Back), {
            Size = minimized and UDim2.new(0, 480, 0, 45) or UDim2.new(0, 480, 0, 380),
        }):Play()
    end
end)

-- ═══════════════════════════════════════════════════════════════
-- 🍎 SEÇÃO 6: UI DA ABA FRUTAS
-- ═══════════════════════════════════════════════════════════════

-- Barra de pesquisa
local SearchBox = new("TextBox", {
    Parent = FruitsTab,
    Size = UDim2.new(1, -6, 0, 36),
    BackgroundColor3 = CONFIG.SECONDARY_COLOR,
    Text = "",
    PlaceholderText = "🔍 Pesquisar fruta...",
    PlaceholderColor3 = Color3.fromRGB(140, 140, 160),
    TextColor3 = CONFIG.TEXT_COLOR,
    TextSize = 14,
    Font = Enum.Font.Gotham,
    ClearTextOnFocus = false,
    ZIndex = 7,
})
new("UICorner", {CornerRadius = UDim.new(0, 8), Parent = SearchBox})
new("UIStroke", {Color = CONFIG.ACCENT_COLOR, Thickness = 1, Transparency = 0.5, Parent = SearchBox})
new("UIPadding", {PaddingLeft = UDim.new(0, 12), Parent = SearchBox})

-- Painel de fruta selecionada
local SelectedFrame = new("Frame", {
    Parent = FruitsTab,
    Size = UDim2.new(1, -6, 0, 70),
    BackgroundColor3 = CONFIG.SECONDARY_COLOR,
    ZIndex = 7,
})
new("UICorner", {CornerRadius = UDim.new(0, 8), Parent = SelectedFrame})
new("UIStroke", {Color = CONFIG.ACCENT_COLOR, Thickness = 1, Transparency = 0.3, Parent = SelectedFrame})

local SelectedLabel = new("TextLabel", {
    Parent = SelectedFrame,
    Size = UDim2.new(1, -100, 0, 24),
    Position = UDim2.new(0, 12, 0, 6),
    BackgroundTransparency = 1,
    Text = "Nenhuma fruta selecionada",
    TextColor3 = CONFIG.TEXT_COLOR,
    TextSize = 14,
    Font = Enum.Font.GothamBold,
    TextXAlignment = Enum.TextXAlignment.Left,
    ZIndex = 8,
})

local SelectedDist = new("TextLabel", {
    Parent = SelectedFrame,
    Size = UDim2.new(1, -100, 0, 18),
    Position = UDim2.new(0, 12, 0, 28),
    BackgroundTransparency = 1,
    Text = "Clique em uma fruta abaixo ou no ESP",
    TextColor3 = Color3.fromRGB(160, 160, 180),
    TextSize = 12,
    Font = Enum.Font.Gotham,
    TextXAlignment = Enum.TextXAlignment.Left,
    ZIndex = 8,
})

local TeleportBtn = new("TextButton", {
    Parent = SelectedFrame,
    Size = UDim2.new(0, 100, 0, 50),
    Position = UDim2.new(1, -110, 0, 10),
    BackgroundColor3 = CONFIG.ACCENT_COLOR,
    Text = "⚡ TP",
    TextColor3 = Color3.new(1, 1, 1),
    TextSize = 14,
    Font = Enum.Font.GothamBold,
    AutoButtonColor = false,
    ZIndex = 8,
})
new("UICorner", {CornerRadius = UDim.new(0, 8), Parent = TeleportBtn})
buttonHover(TeleportBtn, CONFIG.ACCENT_COLOR, Color3.fromRGB(150, 110, 255))

-- Lista de frutas
local FruitListLabel = new("TextLabel", {
    Parent = FruitsTab,
    Size = UDim2.new(1, -6, 0, 24),
    BackgroundTransparency = 1,
    Text = "📋 FRUTAS DETECTADAS",
    TextColor3 = CONFIG.ACCENT_COLOR,
    TextSize = 12,
    Font = Enum.Font.GothamBold,
    TextXAlignment = Enum.TextXAlignment.Left,
    ZIndex = 7,
})

-- Container da lista
local FruitList = new("Frame", {
    Parent = FruitsTab,
    Size = UDim2.new(1, -6, 0, 200),
    BackgroundTransparency = 1,
    ZIndex = 7,
})
new("UIListLayout", {
    Parent = FruitList,
    Padding = UDim.new(0, 5),
    SortOrder = Enum.SortOrder.LayoutOrder,
})

-- Função para criar item de fruta na lista
local function createFruitListItem(fruitModel, fruitName, rarity, color)
    local item = new("TextButton", {
        Parent = FruitList,
        Size = UDim2.new(1, -4, 0, 36),
        BackgroundColor3 = CONFIG.SECONDARY_COLOR,
        Text = "",
        AutoButtonColor = false,
        ZIndex = 8,
    })
    new("UICorner", {CornerRadius = UDim.new(0, 6), Parent = item})
    local stroke = new("UIStroke", {Color = color, Thickness = 1, Transparency = 0.4, Parent = item})
    
    -- Indicador de cor
    local dot = new("Frame", {
        Parent = item,
        Size = UDim2.new(0, 10, 0, 10),
        Position = UDim2.new(0, 10, 0.5, -5),
        BackgroundColor3 = color,
        ZIndex = 9,
    })
    new("UICorner", {CornerRadius = UDim.new(1, 0), Parent = dot})
    
    local nameLabel = new("TextLabel", {
        Parent = item,
        Size = UDim2.new(1, -120, 1, 0),
        Position = UDim2.new(0, 28, 0, 0),
        BackgroundTransparency = 1,
        Text = fruitName .. "  [" .. rarity .. "]",
        TextColor3 = CONFIG.TEXT_COLOR,
        TextSize = 13,
        Font = Enum.Font.GothamSemibold,
        TextXAlignment = Enum.TextXAlignment.Left,
        TextTruncate = Enum.TextTruncate.AtEnd,
        ZIndex = 9,
    })
    
    local distLabel = new("TextLabel", {
        Parent = item,
        Size = UDim2.new(0, 80, 1, 0),
        Position = UDim2.new(1, -85, 0, 0),
        BackgroundTransparency = 1,
        Text = "---",
        TextColor3 = color,
        TextSize = 12,
        Font = Enum.Font.GothamBold,
        TextXAlignment = Enum.TextXAlignment.Right,
        ZIndex = 9,
    })
    
    -- Item data
    local itemData = {
        Model = fruitModel,
        Name = fruitName,
        Rarity = rarity,
        DistanceLabel = distLabel,
        Button = item,
        Stroke = stroke,
    }
    
    item.MouseButton1Click:Connect(function()
        State.SelectedFruit = itemData
        SelectedLabel.Text = "🍎 " .. fruitName
        SelectedLabel.TextColor3 = color
        SelectedDist.Text = "Raridade: " .. rarity .. "  •  Distância: " .. distLabel.Text
        SelectedDist.TextColor3 = color
        TeleportBtn.BackgroundColor3 = color
    end)
    
    -- Hover
    item.MouseEnter:Connect(function()
        TweenService:Create(item, TweenInfo.new(0.15), 
            {BackgroundColor3 = Color3.fromRGB(60, 60, 80)}):Play()
    end)
    item.MouseLeave:Connect(function()
        TweenService:Create(item, TweenInfo.new(0.15), 
            {BackgroundColor3 = CONFIG.SECONDARY_COLOR}):Play()
    end)
    
    return itemData
end

-- ═══════════════════════════════════════════════════════════════
-- ⚙️ SEÇÃO 7: UI DA ABA CONFIGURAÇÕES
-- ═══════════════════════════════════════════════════════════════

local function createToggle(parent, label, initialValue, callback)
    local frame = new("Frame", {
        Parent = parent,
        Size = UDim2.new(1, -6, 0, 40),
        BackgroundColor3 = CONFIG.SECONDARY_COLOR,
        ZIndex = 7,
    })
    new("UICorner", {CornerRadius = UDim.new(0, 8), Parent = frame})
    
    local lbl = new("TextLabel", {
        Parent = frame,
        Size = UDim2.new(1, -70, 1, 0),
        Position = UDim2.new(0, 12, 0, 0),
        BackgroundTransparency = 1,
        Text = label,
        TextColor3 = CONFIG.TEXT_COLOR,
        TextSize = 13,
        Font = Enum.Font.GothamSemibold,
        TextXAlignment = Enum.TextXAlignment.Left,
        ZIndex = 8,
    })
    
    local toggleBg = new("Frame", {
        Parent = frame,
        Size = UDim2.new(0, 50, 0, 26),
        Position = UDim2.new(1, -62, 0.5, -13),
        BackgroundColor3 = initialValue and CONFIG.SUCCESS_COLOR or Color3.fromRGB(80, 80, 100),
        ZIndex = 8,
    })
    new("UICorner", {CornerRadius = UDim.new(1, 0), Parent = toggleBg})
    
    local knob = new("Frame", {
        Parent = toggleBg,
        Size = UDim2.new(0, 20, 0, 20),
        Position = initialValue and UDim2.new(1, -22, 0.5, -10) or UDim2.new(0, 2, 0.5, -10),
        BackgroundColor3 = Color3.new(1, 1, 1),
        ZIndex = 9,
    })
    new("UICorner", {CornerRadius = UDim.new(1, 0), Parent = knob})
    
    local state = initialValue
    
    toggleBg.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 
        or input.UserInputType == Enum.UserInputType.Touch then
            state = not state
            TweenService:Create(toggleBg, TweenInfo.new(0.2), {
                BackgroundColor3 = state and CONFIG.SUCCESS_COLOR or Color3.fromRGB(80, 80, 100)
            }):Play()
            TweenService:Create(knob, TweenInfo.new(0.2), {
                Position = state and UDim2.new(1, -22, 0.5, -10) or UDim2.new(0, 2, 0.5, -10)
            }):Play()
            if callback then callback(state) end
        end
    end)
    
    return frame
end

createToggle(ConfigTab, "🔍 Ativar ESP de Frutas", true, function(v)
    State.ESPEnabled = v
    for _, data in pairs(State.ActiveFruits) do
        if data.Highlight then data.Highlight.Enabled = v and State.HighlightEnabled end
        if data.Billboard then data.Billboard.Enabled = v and State.TextEnabled end
    end
end)

createToggle(ConfigTab, "🎨 Destaque (Highlight)", true, function(v)
    State.HighlightEnabled = v
    for _, data in pairs(State.ActiveFruits) do
        if data.Highlight then data.Highlight.Enabled = State.ESPEnabled and v end
    end
end)

createToggle(ConfigTab, "📝 Texto acima da fruta", true, function(v)
    State.TextEnabled = v
    for _, data in pairs(State.ActiveFruits) do
        if data.Billboard then data.Billboard.Enabled = State.ESPEnabled and v end
    end
end)

-- Filtros de raridade
local RarityLabel = new("TextLabel", {
    Parent = ConfigTab,
    Size = UDim2.new(1, -6, 0, 26),
    BackgroundTransparency = 1,
    Text = "🎯 FILTRAR POR RARIDADE",
    TextColor3 = CONFIG.ACCENT_COLOR,
    TextSize = 12,
    Font = Enum.Font.GothamBold,
    TextXAlignment = Enum.TextXAlignment.Left,
    ZIndex = 7,
})

for rarity, defaultOn in pairs(CONFIG.FILTER_RARITY) do
    createToggle(ConfigTab, "  " .. rarity, defaultOn, function(v)
        CONFIG.FILTER_RARITY[rarity] = v
        refreshAllESP()
    end)
end

-- Info
new("TextLabel", {
    Parent = ConfigTab,
    Size = UDim2.new(1, -6, 0, 40),
    BackgroundTransparency = 1,
    Text = "💡 Dica: Clique em uma fruta no ESP\nou na lista para selecioná-la.",
    TextColor3 = Color3.fromRGB(160, 160, 180),
    TextSize = 12,
    Font = Enum.Font.Gotham,
    TextXAlignment = Enum.TextXAlignment.Left,
    TextWrapped = true,
    ZIndex = 7,
})

-- ═══════════════════════════════════════════════════════════════
-- 🔍 SEÇÃO 8: SISTEMA DE PESQUISA
-- ═══════════════════════════════════════════════════════════════

SearchBox:GetPropertyChangedSignal("Text"):Connect(function()
    State.SearchFilter = SearchBox.Text:lower()
    -- Atualiza visibilidade dos itens
    for _, itemData in pairs(State.FruitListItems) do
        if itemData.Button then
            local matches = State.SearchFilter == "" 
                or itemData.Name:lower():find(State.SearchFilter, 1, true)
            itemData.Button.Visible = matches
        end
    end
end)

-- ═══════════════════════════════════════════════════════════════
-- 🎯 SEÇÃO 9: ESP DE FRUTAS (CORE)
-- ═══════════════════════════════════════════════════════════════

local function createESPForFruit(fruitObj, fruitName, color, rarity)
    local handle = getFruitHandle(fruitObj)
    if not handle then return end
    
    -- Highlight
    local highlight
    if fruitObj:IsA("Model") or fruitObj:IsA("BasePart") or fruitObj:IsA("Tool") then
        highlight = Instance.new("Highlight")
        highlight.Name = "FruitESP_Highlight"
        highlight.Adornee = fruitObj
        highlight.FillColor = color
        highlight.OutlineColor = color
        highlight.FillTransparency = 1 - CONFIG.HIGHLIGHT_FILL
        highlight.OutlineTransparency = 1 - (CONFIG.HIGHLIGHT_OUTLINE / 2)
        highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
        highlight.Enabled = State.ESPEnabled and State.HighlightEnabled
        highlight.Parent = fruitObj
    end
    
    -- Billboard
    local billboard = Instance.new("BillboardGui")
    billboard.Name = "FruitESP_Billboard"
    billboard.Adornee = handle
    billboard.Size = UDim2.new(0, 200, 0, 50)
    billboard.StudsOffset = Vector3.new(0, 3, 0)
    billboard.AlwaysOnTop = true
    billboard.MaxDistance = CONFIG.MAX_ESP_DISTANCE
    billboard.Enabled = State.ESPEnabled and State.TextEnabled
    billboard.Parent = handle
    
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, 0, 1, 0)
    label.BackgroundTransparency = 1
    label.TextColor3 = color
    label.TextSize = CONFIG.TEXT_SIZE
    label.Font = Enum.Font.GothamBold
    label.TextStrokeTransparency = 0.3
    label.TextStrokeColor3 = Color3.new(0, 0, 0)
    label.Text = fruitName .. "\n[" .. rarity .. "]  ---"
    label.Parent = billboard
    
    -- Botão invisível para selecionar via ESP (clique no texto)
    local espBtn = Instance.new("TextButton")
    espBtn.Size = UDim2.new(1, 0, 1, 0)
    espBtn.BackgroundTransparency = 1
    espBtn.Text = ""
    espBtn.Parent = billboard
    
    local espData = {
        Model = fruitObj,
        Highlight = highlight,
        Billboard = billboard,
        Label = label,
        Name = fruitName,
        Rarity = rarity,
        Color = color,
        Handle = handle,
    }
    
    espBtn.MouseButton1Click:Connect(function()
        State.SelectedFruit = espData
        SelectedLabel.Text = "🍎 " .. fruitName
        SelectedLabel.TextColor3 = color
        SelectedDist.Text = "Raridade: " .. rarity
        SelectedDist.TextColor3 = color
        TeleportBtn.BackgroundColor3 = color
    end)
    
    return espData
end

-- Remove ESP de fruta
local function removeESPForFruit(fruitObj)
    local data = State.ActiveFruits[fruitObj]
    if not data then return end
    
    if data.Highlight and data.Highlight.Parent then
        data.Highlight:Destroy()
    end
    if data.Billboard and data.Billboard.Parent then
        data.Billboard:Destroy()
    end
    
    -- Remove item da lista
    local listItem = State.FruitListItems[fruitObj]
    if listItem and listItem.Button then
        listItem.Button:Destroy()
    end
    
    State.ActiveFruits[fruitObj] = nil
    State.FruitListItems[fruitObj] = nil
end

-- Verifica se fruta passa no filtro de raridade
local function passesRarityFilter(rarity)
    return CONFIG.FILTER_RARITY[rarity] == true
end

-- Atualiza ESP de todas as frutas
function refreshAllESP()
    for fruitObj, data in pairs(State.ActiveFruits) do
        if not fruitObj.Parent then
            removeESPForFruit(fruitObj)
        else
            local visible = State.ESPEnabled and passesRarityFilter(data.Rarity)
            if data.Highlight then
                data.Highlight.Enabled = visible and State.HighlightEnabled
            end
            if data.Billboard then
                data.Billboard.Enabled = visible and State.TextEnabled
            end
            -- Atualiza item na lista
            local listItem = State.FruitListItems[fruitObj]
            if listItem and listItem.Button then
                listItem.Button.Visible = visible and (
                    State.SearchFilter == "" 
                    or data.Name:lower():find(State.SearchFilter, 1, true)
                )
            end
        end
    end
end

-- Scan de frutas no workspace
local function scanFruits()
    for _, obj in ipairs(Workspace:GetDescendants()) do
        if State.ActiveFruits[obj] then continue end
        
        if isFruitObject(obj) then
            local handle = getFruitHandle(obj)
            if not handle then continue end
            
            -- Evita duplicar (se for model + handle filho)
            local parentModel = obj:IsA("BasePart") and obj.Parent or obj
            if parentModel and State.ActiveFruits[parentModel] then continue end
            
            local fruitName, data = getFruitInfo(obj.Name)
            
            -- Cria ESP
            local espData = createESPForFruit(obj, fruitName, data.Color, data.Rarity)
            if espData then
                State.ActiveFruits[obj] = espData
                espData.Distance = 0
                
                -- Cria item na lista
                if passesRarityFilter(data.Rarity) then
                    local listItem = createFruitListItem(obj, fruitName, data.Rarity, data.Color)
                    State.FruitListItems[obj] = listItem
                end
            end
        end
    end
end

-- ═══════════════════════════════════════════════════════════════
-- 🔄 SEÇÃO 10: LOOP PRINCIPAL (Atualização)
-- ═══════════════════════════════════════════════════════════════

local lastUpdate = 0
local lastScan = 0
local SCAN_INTERVAL = 1.5  -- scan completo a cada 1.5s

RunService.RenderStepped:Connect(function()
    local now = tick()
    
    -- Scan periódico para novas frutas
    if now - lastScan > SCAN_INTERVAL then
        lastScan = now
        task.spawn(scanFruits)
    end
    
    -- Atualização de distâncias e remoção
    if now - lastUpdate < CONFIG.ESP_UPDATE_RATE then return end
    lastUpdate = now
    
    local char = LocalPlayer.Character
    if not char then return end
    local root = char:FindFirstChild("HumanoidRootPart")
    if not root then return end
    local myPos = root.Position
    
    for fruitObj, data in pairs(State.ActiveFruits) do
        -- Remove se foi destruído
        if not fruitObj.Parent then
            removeESPForFruit(fruitObj)
            continue
        end
        
        -- Atualiza distância
        local handle = data.Handle
        if handle and handle.Parent then
            local dist = (handle.Position - myPos).Magnitude
            data.Distance = dist
            
            -- Atualiza label do Billboard
            if data.Label then
                data.Label.Text = string.format("%s\n[%s]  %dm", 
                    data.Name, data.Rarity, math.floor(dist))
            end
            
            -- Atualiza label da lista
            local listItem = State.FruitListItems[fruitObj]
            if listItem and listItem.DistanceLabel then
                listItem.DistanceLabel.Text = math.floor(dist) .. "m"
            end
            
            -- Atualiza distância da fruta selecionada
            if State.SelectedFruit and State.SelectedFruit.Model == fruitObj then
                SelectedDist.Text = "Raridade: " .. data.Rarity 
                    .. "  •  Distância: " .. math.floor(dist) .. "m"
            end
            
            -- Esconde se muito longe
            if data.Billboard then
                data.Billboard.Enabled = State.ESPEnabled and State.TextEnabled 
                    and dist <= CONFIG.MAX_ESP_DISTANCE
                    and passesRarityFilter(data.Rarity)
            end
        else
            removeESPForFruit(fruitObj)
        end
    end
end)

-- ═══════════════════════════════════════════════════════════════
-- ⚡ SEÇÃO 11: SISTEMA DE TELEPORTE
-- ═══════════════════════════════════════════════════════════════

local function teleportToFruit(targetData)
    if not targetData then return end
    
    -- Cooldown
    if tick() - State.LastTeleport < CONFIG.TP_COOLDOWN then return end
    State.LastTeleport = tick()
    
    local char = LocalPlayer.Character
    if not char then return end
    local root = char:FindFirstChild("HumanoidRootPart")
    if not root then return end
    
    local handle = targetData.Handle or (targetData.Model and getFruitHandle(targetData.Model))
    if not handle or not handle.Parent then
        SelectedDist.Text = "⚠️ Fruta não encontrada!"
        SelectedDist.TextColor3 = CONFIG.DANGER_COLOR
        return
    end
    
    -- Teleporta
    local targetPos = handle.Position + Vector3.new(0, CONFIG.TP_OFFSET_Y, 0)
    
    -- Feedback visual
    SelectedDist.Text = "⚡ Teleportando..."
    SelectedDist.TextColor3 = CONFIG.WARNING_COLOR
    
    -- Movimento suave
    local startPos = root.Position
    local distance = (targetPos - startPos).Magnitude
    
    if distance > 500 then
        -- Teleporte instantâneo para longas distâncias
        root.CFrame = CFrame.new(targetPos)
    else
        -- Animação suave para curtas
        local tween = TweenService:Create(root, 
            TweenInfo.new(math.min(distance / 100, 0.5), Enum.EasingStyle.Quad),
            {CFrame = CFrame.new(targetPos)})
        tween:Play()
        tween.Completed:Wait()
    end
    
    task.wait(0.1)
    SelectedDist.Text = "✅ Teleportado!"
    SelectedDist.TextColor3 = CONFIG.SUCCESS_COLOR
    
    task.delay(2, function()
        if State.SelectedFruit then
            SelectedDist.Text = "Raridade: " .. State.SelectedFruit.Rarity
            SelectedDist.TextColor3 = State.SelectedFruit.Color or CONFIG.TEXT_COLOR
        end
    end)
end

TeleportBtn.MouseButton1Click:Connect(function()
    if State.SelectedFruit then
        teleportToFruit(State.SelectedFruit)
    else
        SelectedDist.Text = "⚠️ Selecione uma fruta primeiro!"
        SelectedDist.TextColor3 = CONFIG.DANGER_COLOR
    end
end)

-- Tecla de atalho para teleporte (PC)
UserInputService.InputBegan:Connect(function(input, gpe)
    if gpe then return end
    if input.KeyCode == Enum.KeyCode.T then
        if State.SelectedFruit then
            teleportToFruit(State.SelectedFruit)
        end
    end
end)

-- ═══════════════════════════════════════════════════════════════
-- 🎬 SEÇÃO 12: ANIMAÇÃO DE ENTRADA
-- ═══════════════════════════════════════════════════════════════

Panel.Size = UDim2.new(0, 0, 0, 0)
Panel.Visible = true
TweenService:Create(Panel, TweenInfo.new(0.5, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
    Size = UDim2.new(0, 480, 0, 380),
}):Play()

-- Scan inicial
task.spawn(scanFruits)

-- ═══════════════════════════════════════════════════════════════
-- 🔔 SEÇÃO 13: DETECÇÃO DE MUDANÇAS NO WORKSPACE
-- ═══════════════════════════════════════════════════════════════

-- Detecta quando uma fruta é adicionada
Workspace.DescendantAdded:Connect(function(obj)
    if isFruitObject(obj) then
        task.wait(0.2) -- pequeno delay para garantir que está totalmente carregada
        if not State.ActiveFruits[obj] then
            scanFruits()
        end
    end
end)

-- Detecta quando uma fruta é removida
Workspace.DescendantRemoving:Connect(function(obj)
    if State.ActiveFruits[obj] then
        removeESPForFruit(obj)
    end
    -- Se for filho de uma fruta existente
    local parent = obj.Parent
    if parent and State.ActiveFruits[parent] then
        removeESPForFruit(parent)
    end
end)

-- Respawn do personagem
LocalPlayer.CharacterAdded:Connect(function()
    task.wait(2)
    scanFruits()
end)

-- ═══════════════════════════════════════════════════════════════
-- ✅ SCRIPT CARREGADO
-- ═══════════════════════════════════════════════════════════════

print("═══════════════════════════════════════")
print("🍎 FRUIT HUB " .. CONFIG.UI_VERSION .. " carregado!")
print("📁 Local: StarterPlayerScripts")
print("⌨️  Atalho: [T] para teleportar")
print("═══════════════════════════════════════")
