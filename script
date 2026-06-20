local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local FrameCorner = Instance.new("UICorner")
local FrameStroke = Instance.new("UIStroke")
local GlowLine = Instance.new("Frame")
local Title = Instance.new("TextLabel")
local ButtonContainer = Instance.new("Frame")
local UIListLayout = Instance.new("UIListLayout")
local FOVBtn = Instance.new("TextButton")
local XrayBtn = Instance.new("TextButton")
local ESPBtn = Instance.new("TextButton")
local BrightBtn = Instance.new("TextButton")
local InfoLabel = Instance.new("TextLabel")

local TweenService = game:GetService("TweenService")
local uis = game:GetService("UserInputService")
local camera = workspace.CurrentCamera
local players = game:GetService("Players")
local lighting = game:GetService("Lighting")

ScreenGui.Parent = game:GetService("CoreGui")
ScreenGui.ResetOnSpawn = false

MainFrame.Name = "ZawusVisuals"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(12, 10, 18)
MainFrame.Position = UDim2.new(0.05, 0, 0.3, 0)
MainFrame.Size = UDim2.new(0, 240, 0, 305)
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.ClipsDescendants = true

FrameCorner.CornerRadius = UDim.new(0, 14)
FrameCorner.Parent = MainFrame

FrameStroke.Color = Color3.fromRGB(35, 25, 50)
FrameStroke.Thickness = 1.5
FrameStroke.Parent = MainFrame

GlowLine.Name = "GlowLine"
GlowLine.Parent = MainFrame
GlowLine.BackgroundColor3 = Color3.fromRGB(180, 0, 255)
GlowLine.Position = UDim2.new(0, 0, 0, 0)
GlowLine.Size = UDim2.new(1, 0, 0, 3)
GlowLine.BorderSizePixel = 0

Title.Parent = MainFrame
Title.Size = UDim2.new(1, 0, 0, 45)
Title.BackgroundTransparency = 1
Title.Text = ""
Title.TextColor3 = Color3.fromRGB(200, 150, 255)
Title.TextSize = 19
Title.Font = Enum.Font.Code

task.spawn(function()
    local fullText = "ZAWUS VISUALS"
    for i = 1, #fullText do
        Title.Text = string.sub(fullText, 1, i) .. "_"
        task.wait(0.2)
    end
    Title.Text = fullText
end)

ButtonContainer.Parent = MainFrame
ButtonContainer.BackgroundTransparency = 1
ButtonContainer.Position = UDim2.new(0, 12, 0, 55)
ButtonContainer.Size = UDim2.new(1, -24, 1, -95)

UIListLayout.Parent = ButtonContainer
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.Padding = UDim.new(0, 8)

local function styleButton(btn, text)
    btn.Parent = ButtonContainer
    btn.Size = UDim2.new(1, 0, 0, 38)
    btn.BackgroundColor3 = Color3.fromRGB(22, 18, 32)
    btn.Text = text
    btn.TextColor3 = Color3.fromRGB(160, 150, 175)
    btn.TextSize = 12
    btn.Font = Enum.Font.Code
    
    local btnCorner = Instance.new("UICorner")
    btnCorner.CornerRadius = UDim.new(0, 8)
    btnCorner.Parent = btn
    
    local btnStroke = Instance.new("UIStroke")
    btnStroke.Color = Color3.fromRGB(40, 32, 55)
    btnStroke.Thickness = 1
    btnStroke.Parent = btn
    
    return btnStroke
end

local fovStroke = styleButton(FOVBtn, "FOV 120    [ KEY: ' ]")
local xrayStroke = styleButton(XrayBtn, "X-RAY      [ KEY: ] ]")
local espStroke = styleButton(ESPBtn, "PLAYER ESP [ KEY: ; ]")
local brightStroke = styleButton(BrightBtn, "FULLBRIGHT [ KEY: [ ]")

InfoLabel.Parent = MainFrame
InfoLabel.Size = UDim2.new(1, 0, 0, 30)
InfoLabel.Position = UDim2.new(0, 0, 1, -33)
InfoLabel.BackgroundTransparency = 1
InfoLabel.Text = "KEYBINDS: [G] Hide UI | [Alt] Kill UI"
InfoLabel.TextColor3 = Color3.fromRGB(140, 120, 160)
InfoLabel.TextSize = 11
InfoLabel.Font = Enum.Font.Code

local function toggleVisuals(btn, stroke, active)
    if active then
        TweenService:Create(btn, TweenInfo.new(0.25), {BackgroundColor3 = Color3.fromRGB(45, 10, 70)}):Play()
        TweenService:Create(btn, TweenInfo.new(0.25), {TextColor3 = Color3.fromRGB(210, 100, 255)}):Play()
        TweenService:Create(stroke, TweenInfo.new(0.25), {Color = Color3.fromRGB(180, 0, 255)}):Play()
    else
        TweenService:Create(btn, TweenInfo.new(0.25), {BackgroundColor3 = Color3.fromRGB(22, 18, 32)}):Play()
        TweenService:Create(btn, TweenInfo.new(0.25), {TextColor3 = Color3.fromRGB(160, 150, 175)}):Play()
        TweenService:Create(stroke, TweenInfo.new(0.25), {Color = Color3.fromRGB(40, 32, 55)}):Play()
    end
end

local fovActive = false
local function toggleFOV()
    fovActive = not fovActive
    camera.FieldOfView = fovActive and 120 or 70
    toggleVisuals(FOVBtn, fovStroke, fovActive)
end
FOVBtn.MouseButton1Click:Connect(toggleFOV)

local xrayActive = false
local function toggleXray()
    xrayActive = not xrayActive
    toggleVisuals(XrayBtn, xrayStroke, xrayActive)
    
    for _, obj in pairs(workspace:GetDescendants()) do
        if obj:IsA("BasePart") and not obj.Parent:FindFirstChildOfClass("Humanoid") then
            if xrayActive then
                if not obj:GetAttribute("OldTrans") then
                    obj:SetAttribute("OldTrans", obj.Transparency)
                end
                obj.Transparency = 0.6
            else
                local old = obj:GetAttribute("OldTrans")
                obj.Transparency = old or 0
            end
        end
    end
end
XrayBtn.MouseButton1Click:Connect(toggleXray)

local espActive = false
local function applyESP(player)
    if player == players.LocalPlayer then return end
    
    local function addHighlight(char)
        if char:FindFirstChild("ESPHighlight") then return end
        local highlight = Instance.new("Highlight")
        highlight.Name = "ESPHighlight"
        highlight.Parent = char
        highlight.FillColor = Color3.fromRGB(180, 0, 255)
        highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
        highlight.FillOpacity = 0.4
        highlight.Enabled = espActive
    end
    
    if player.Character then addHighlight(player.Character) end
    player.CharacterAdded:Connect(addHighlight)
end

for _, p in pairs(players:GetPlayers()) do applyESP(p) end
players.PlayerAdded:Connect(applyESP)

local function toggleESP()
    espActive = not espActive
    toggleVisuals(ESPBtn, espStroke, espActive)
    
    for _, p in pairs(players:GetPlayers()) do
        if p.Character and p.Character:FindFirstChild("ESPHighlight") then
            p.Character.ESPHighlight.Enabled = espActive
        end
    end
end
ESPBtn.MouseButton1Click:Connect(toggleESP)

local brightActive = false
local origAmbient = lighting.Ambient
local origOutdoorAmbient = lighting.OutdoorAmbient

local function toggleBright()
    brightActive = not brightActive
    toggleVisuals(BrightBtn, brightStroke, brightActive)
    
    if brightActive then
        lighting.Ambient = Color3.fromRGB(255, 255, 255)
        lighting.OutdoorAmbient = Color3.fromRGB(255, 255, 255)
        lighting.Brightness = 2
    else
        lighting.Ambient = origAmbient
        lighting.OutdoorAmbient = origOutdoorAmbient
        lighting.Brightness = 1
    end
end
BrightBtn.MouseButton1Click:Connect(toggleBright)

local isVisible = true
local isTweening = false
local originalSize = UDim2.new(0, 240, 0, 305)

local function toggleMenuAnimation()
    if isTweening then return end
    isTweening = true
    
    if isVisible then
        local tweenFrame = TweenService:Create(MainFrame, TweenInfo.new(0.3, Enum.EasingStyle.Quart, Enum.EasingDirection.In), {
            Size = UDim2.new(0, 240, 0, 0),
            BackgroundTransparency = 1
        })
        tweenFrame:Play()
        tweenFrame.Completed:Connect(function()
            MainFrame.Visible = false
            isVisible = false
            isTweening = false
        end)
    else
        MainFrame.Visible = true
        local tweenFrame = TweenService:Create(MainFrame, TweenInfo.new(0.4, Enum.EasingStyle.Quart, Enum.EasingDirection.Out), {
            Size = originalSize,
            BackgroundTransparency = 0
        })
        tweenFrame:Play()
        tweenFrame.Completed:Connect(function()
            isVisible = true
            isTweening = false
        end)
    end
end

uis.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    
    if input.KeyCode == Enum.KeyCode.LeftAlt or input.KeyCode == Enum.KeyCode.RightAlt then
        local fadeOut = TweenService:Create(MainFrame, TweenInfo.new(0.25, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {
            Size = UDim2.new(0, 240, 0, 0),
            BackgroundTransparency = 1
        })
        fadeOut:Play()
        fadeOut.Completed:Connect(function()
            ScreenGui:Destroy()
        end)
    elseif input.KeyCode == Enum.KeyCode.G then
        toggleMenuAnimation()
    elseif input.KeyCode == Enum.KeyCode.LeftBracket then
        toggleBright()
    elseif input.KeyCode == Enum.KeyCode.Semicolon then
        toggleESP()
    elseif input.KeyCode == Enum.KeyCode.Quote then
        toggleFOV()
    elseif input.KeyCode == Enum.KeyCode.RightBracket then
        toggleXray()
    end
end)
