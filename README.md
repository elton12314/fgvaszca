local UILibrary = {}

function UILibrary:CreateWindow(title, size)
    local ScreenGui = Instance.new("ScreenGui")
    local MainFrame = Instance.new("Frame")
    local TitleLabel = Instance.new("TextLabel")
    local CloseButton = Instance.new("TextButton")
    local TabContainer = Instance.new("Frame")
    local Tabs = {}

    -- Configuração inicial
    if syn and syn.protect_gui then
        syn.protect_gui(ScreenGui)
        ScreenGui.Parent = game:GetService("CoreGui")
    elseif gethui then
        ScreenGui.Parent = gethui()
    elseif game:GetService("CoreGui"):FindFirstChildOfClass("ScreenGui") then
        ScreenGui.Parent = game:GetService("CoreGui")
    else
        ScreenGui.Parent = game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui")
    end

    -- Janela principal
    MainFrame.Parent = ScreenGui
    MainFrame.Size = size or UDim2.new(0, 500, 0, 400)
    MainFrame.Position = UDim2.new(0.5, -250, 0.5, -200)
    MainFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    MainFrame.BorderSizePixel = 0
    MainFrame.Active = true
    MainFrame.Draggable = true
    MainFrame.ClipsDescendants = true

    -- Título da janela
    TitleLabel.Parent = MainFrame
    TitleLabel.Size = UDim2.new(1, 0, 0, 40)
    TitleLabel.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
    TitleLabel.Text = title or "Meu Menu"
    TitleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    TitleLabel.Font = Enum.Font.GothamBold
    TitleLabel.TextSize = 20
    TitleLabel.TextXAlignment = Enum.TextXAlignment.Center  -- Centraliza o texto horizontalmente
    TitleLabel.TextYAlignment = Enum.TextYAlignment.Center  -- Centraliza o texto verticalmente

    -- Botão de fechar
    CloseButton.Parent = MainFrame
    CloseButton.Size = UDim2.new(0, 40, 0, 40)
    CloseButton.Position = UDim2.new(1, -40, 0, 0)
    CloseButton.BackgroundColor3 = Color3.fromRGB(255, 75, 75)
    CloseButton.Text = "X"
    CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    CloseButton.Font = Enum.Font.SourceSans
    CloseButton.TextSize = 20
    CloseButton.MouseButton1Click:Connect(function()
        ScreenGui:Destroy()
    end)

    -- Container de abas
    TabContainer.Parent = MainFrame
    TabContainer.Size = UDim2.new(1, 0, 0, 50)
    TabContainer.Position = UDim2.new(0, 0, 0, 40)
    TabContainer.BackgroundColor3 = Color3.fromRGB(60, 60, 60)

    -- Função para criar abas
    local function createTab(name)
        local TabButton = Instance.new("TextButton")
        local TabFrame = Instance.new("Frame")

        TabButton.Parent = TabContainer
        TabButton.Size = UDim2.new(0, 120, 1, 0)
        TabButton.Text = name
        TabButton.TextColor3 = Color3.fromRGB(255, 255, 255)
        TabButton.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
        TabButton.Font = Enum.Font.Gotham
        TabButton.TextSize = 16

        TabFrame.Parent = MainFrame
        TabFrame.Size = UDim2.new(1, 0, 1, -90)
        TabFrame.Position = UDim2.new(0, 0, 0, 90)
        TabFrame.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
        TabFrame.Visible = false

        -- Alterar a visibilidade ao clicar nas abas
        TabButton.MouseButton1Click:Connect(function()
            for _, tab in pairs(Tabs) do
                tab.Frame.Visible = false
            end
            TabFrame.Visible = true
        end)

        -- Guardar as abas para alternar entre elas
        table.insert(Tabs, {Button = TabButton, Frame = TabFrame})

        return TabFrame
    end

    local Window = {
        ScreenGui = ScreenGui,
        MainFrame = MainFrame,
        CreateTab = createTab
    }

    return Window
end

return UILibrary
