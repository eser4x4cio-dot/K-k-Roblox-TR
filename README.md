local player = game.Players.LocalPlayer
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

-- [AYARLAR]
local settings = {
    infJump = false,
    speedBoost = false,
    noclip = false,
    esp = false,
    anchor = false,
    speedVal = 340,
    fovVal = 80
}

local Binds = {
    infJump = Enum.KeyCode.J,
    speedBoost = Enum.KeyCode.B,
    noclip = Enum.KeyCode.M,
    esp = Enum.KeyCode.K,
    anchor = Enum.KeyCode.L
}

local UpdateFunctions = {}

-- [ESP SİSTEMİ]
local ESP_Folder = Instance.new("Folder", game.CoreGui)
ESP_Folder.Name = "KucukTR_ESP_Final"

local function createESP(targetPlayer)
    if targetPlayer == player then return end
    local function applyESP()
        local char = targetPlayer.Character or targetPlayer.CharacterAdded:Wait()
        local hrp = char:WaitForChild("HumanoidRootPart", 5)
        if not hrp then return end
        local box = Instance.new("BoxHandleAdornment", ESP_Folder)
        box.Name = targetPlayer.Name; box.AlwaysOnTop = true; box.ZIndex = 10; box.Transparency = 0.5
        box.Size = Vector3.new(4, 6, 1); box.Color3 = Color3.fromRGB(191, 0, 255)
        local bill = Instance.new("BillboardGui", ESP_Folder)
        bill.AlwaysOnTop = true; bill.Size = UDim2.new(0, 100, 0, 40); bill.StudsOffset = Vector3.new(0, 3, 0); bill.Adornee = hrp
        local nameLabel = Instance.new("TextLabel", bill)
        nameLabel.Size = UDim2.new(1, 0, 0.3, 0); nameLabel.BackgroundTransparency = 1; nameLabel.TextColor3 = Color3.new(1, 1, 1); nameLabel.Font = Enum.Font.GothamBold; nameLabel.TextSize = 8
        local distLabel = Instance.new("TextLabel", bill)
        distLabel.Position = UDim2.new(0, 0, 0.3, 0); distLabel.Size = UDim2.new(1, 0, 0.3, 0); distLabel.BackgroundTransparency = 1; distLabel.TextColor3 = Color3.fromRGB(191, 0, 255); distLabel.Font = Enum.Font.GothamBold; distLabel.TextSize = 8
        local hpLabel = Instance.new("TextLabel", bill)
        hpLabel.Position = UDim2.new(0, 0, 0.6, 0); hpLabel.Size = UDim2.new(1, 0, 0.3, 0); hpLabel.BackgroundTransparency = 1; hpLabel.TextColor3 = Color3.fromRGB(255, 0, 0); hpLabel.Font = Enum.Font.GothamBold; hpLabel.TextSize = 8
        local conn; conn = RunService.RenderStepped:Connect(function()
            if not settings.esp or not targetPlayer.Parent or not char.Parent then box:Destroy(); bill:Destroy(); conn:Disconnect(); return end
            local c_hrp = char:FindFirstChild("HumanoidRootPart"); local hum = char:FindFirstChild("Humanoid")
            if c_hrp and hum then box.Adornee = c_hrp; local dist = math.floor((player.Character.HumanoidRootPart.Position - c_hrp.Position).Magnitude)
                nameLabel.Text = targetPlayer.Name; distLabel.Text = dist .. "M"; hpLabel.Text = "%" .. math.floor(hum.Health)
            else box.Adornee = nil end
        end)
    end
    targetPlayer.CharacterAdded:Connect(applyESP); if targetPlayer.Character then applyESP() end
end

local function toggleESP() ESP_Folder:ClearAllChildren(); if settings.esp then for _, p in pairs(game.Players:GetPlayers()) do createESP(p) end end end
game.Players.PlayerAdded:Connect(createESP)

-- [GUI TASARIMI - BAŞLIK DÜZELTİLDİ]
local screenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui")); screenGui.ResetOnSpawn = false
local frame = Instance.new("Frame", screenGui)
frame.Size = UDim2.new(0, 220, 0, 340)
frame.Position = UDim2.new(0.5, -110, 0.5, -170)
frame.BackgroundColor3 = Color3.fromRGB(8, 8, 15)
frame.BorderSizePixel = 0; frame.Active = true; frame.Draggable = true

local corner = Instance.new("UICorner", frame); corner.CornerRadius = UDim.new(0, 10)
local stroke = Instance.new("UIStroke", frame); stroke.Thickness = 3; stroke.Color = Color3.fromRGB(255, 0, 255)

-- BAŞLIK VE TÜRK BAYRAKLARI 🇹🇷
local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0, 35)
title.BackgroundTransparency = 1
title.Text = "🇹🇷 KÜÇÜK ROBLOX TR 🇹🇷" -- İsim düzeltildi ve bayraklar eklendi
title.Font = Enum.Font.GothamBold
title.TextSize = 13 -- Bayraklar sığsın diye hafif küçültüldü
title.TextColor3 = Color3.new(1,1,1)

RunService.RenderStepped:Connect(function()
    local hue = tick() % 3 / 3
    title.TextColor3 = Color3.fromHSV(hue, 0.7, 1); stroke.Color = Color3.fromHSV(hue, 1, 1)
end)

local content = Instance.new("Frame", frame)
content.Size = UDim2.new(0.9, 0, 0.82, 0); content.Position = UDim2.new(0.05, 0, 0.12, 0); content.BackgroundTransparency = 1
local layout = Instance.new("UIListLayout", content); layout.Padding = UDim.new(0, 5); layout.SortOrder = Enum.SortOrder.LayoutOrder

-- [TOGGLE]
local function createToggle(text, varName)
    local container = Instance.new("Frame", content); container.Size = UDim2.new(1, 0, 0, 24); container.BackgroundTransparency = 1
    local bindBtn = Instance.new("TextButton", container); bindBtn.Size = UDim2.new(0, 24, 0, 20); bindBtn.BackgroundColor3 = Color3.fromRGB(150, 0, 255); bindBtn.Text = Binds[varName].Name; bindBtn.TextColor3 = Color3.new(1, 1, 1); bindBtn.Font = Enum.Font.GothamBold; bindBtn.TextSize = 8; Instance.new("UICorner", bindBtn)
    local label = Instance.new("TextLabel", container); label.Size = UDim2.new(0.5, 0, 1, 0); label.Position = UDim2.new(0.15, 0, 0, 0); label.BackgroundTransparency = 1; label.Text = text; label.TextColor3 = Color3.new(1,1,1); label.Font = Enum.Font.GothamBold; label.TextSize = 10; label.TextXAlignment = Enum.TextXAlignment.Left
    local switchBg = Instance.new("TextButton", container); switchBg.Size = UDim2.new(0, 32, 0, 16); switchBg.Position = UDim2.new(0.82, 0, 0.15, 0); switchBg.BackgroundColor3 = Color3.fromRGB(30, 30, 30); switchBg.Text = ""; Instance.new("UICorner", switchBg).CornerRadius = UDim.new(1, 0)
    local circle = Instance.new("Frame", switchBg); circle.Size = UDim2.new(0, 10, 0, 10); circle.Position = UDim2.new(0.1, 0, 0.18, 0); circle.BackgroundColor3 = Color3.new(1, 1, 1); Instance.new("UICorner", circle).CornerRadius = UDim.new(1, 0)
    local function updateUI()
        local active = settings[varName]
        circle:TweenPosition(active and UDim2.new(0.58, 0, 0.18, 0) or UDim2.new(0.1, 0, 0.18, 0), "Out", "Quad", 0.2, true)
        switchBg.BackgroundColor3 = active and Color3.fromRGB(255, 0, 255) or Color3.fromRGB(30, 30, 30)
    end
    UpdateFunctions[varName] = updateUI
    switchBg.MouseButton1Click:Connect(function() settings[varName] = not settings[varName]; if varName == "esp" then toggleESP() end; updateUI() end); updateUI()
end

-- [SLIDER]
local function createSlider(text, min, max, varName)
    local container = Instance.new("Frame", content); container.Size = UDim2.new(1, 0, 0, 35); container.BackgroundTransparency = 1
    local label = Instance.new("TextLabel", container); label.Size = UDim2.new(1, 0, 0, 12); label.BackgroundTransparency = 1; label.Text = text; label.TextColor3 = Color3.new(0.8,0.8,0.8); label.Font = Enum.Font.GothamBold; label.TextSize = 9; label.TextXAlignment = Enum.TextXAlignment.Left
    local valBox = Instance.new("TextLabel", container); valBox.Size = UDim2.new(0, 35, 0, 12); valBox.Position = UDim2.new(0.8, 0, 0, 0); valBox.BackgroundTransparency = 1; valBox.TextColor3 = Color3.new(1,0,1); valBox.Text = tostring(settings[varName]); valBox.Font = Enum.Font.GothamBold; valBox.TextSize = 9
    local sliderBtn = Instance.new("TextButton", container); sliderBtn.Size = UDim2.new(1, 0, 0, 4); sliderBtn.Position = UDim2.new(0, 0, 0.6, 0); sliderBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 45); sliderBtn.Text = ""; Instance.new("UICorner", sliderBtn)
    local fill = Instance.new("Frame", sliderBtn); fill.Size = UDim2.new((settings[varName] - min) / (max - min), 0, 1, 0); fill.BackgroundColor3 = Color3.fromRGB(255, 0, 255); Instance.new("UICorner", fill)
    local dragging = false; local function moveSlider()
        local diff = math.clamp((UIS:GetMouseLocation().X - sliderBtn.AbsolutePosition.X) / sliderBtn.AbsoluteSize.X, 0, 1)
        fill.Size = UDim2.new(diff, 0, 1, 0); local value = math.floor(min + (diff * (max - min))); settings[varName] = value; valBox.Text = tostring(value)
    end
    sliderBtn.InputBegan:Connect(function(input) if input.UserInputType == Enum.UserInputType.MouseButton1 then dragging = true moveSlider() end end)
    UIS.InputEnded:Connect(function(input) if input.UserInputType == Enum.UserInputType.MouseButton1 then dragging = false end end)
    UIS.InputChanged:Connect(function(input) if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then moveSlider() end end)
end

createToggle("Hız", "speedBoost")
createToggle("Noclip", "noclip")
createToggle("ESP", "esp")
createToggle("Inf Jump", "infJump")
createToggle("Anchor", "anchor")
createSlider("Hız Ayarı", 16, 1000, "speedVal")
createSlider("FOV Ayarı", 70, 120, "fovVal")

RunService.RenderStepped:Connect(function()
    if workspace.CurrentCamera then workspace.CurrentCamera.FieldOfView = settings.fovVal end
    local char = player.Character; local hrp = char and char:FindFirstChild("HumanoidRootPart")
    if hrp then hrp.Anchored = settings.anchor
        if settings.noclip then for _, v in pairs(char:GetDescendants()) do if v:IsA("BasePart") then v.CanCollide = false end end end
        if settings.speedBoost and not settings.anchor then
            local hum = char:FindFirstChildOfClass("Humanoid")
            if hum and hum.MoveDirection.Magnitude > 0 then hrp.AssemblyLinearVelocity = Vector3.new(hum.MoveDirection.X * settings.speedVal, hrp.AssemblyLinearVelocity.Y, hum.MoveDirection.Z * settings.speedVal) end
        end
    end
end)

UIS.InputBegan:Connect(function(input, gp)
    if gp then return end; if input.KeyCode == Enum.KeyCode.G then frame.Visible = not frame.Visible end
    for var, key in pairs(Binds) do if input.KeyCode == key then settings[var] = not settings[var]; if var == "esp" then toggleESP() end; if UpdateFunctions[var] then UpdateFunctions[var]() end end end
    if input.KeyCode == Enum.KeyCode.Space and settings.infJump then
        local hrp = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
        if hrp then hrp.AssemblyLinearVelocity = Vector3.new(hrp.AssemblyLinearVelocity.X, 50, hrp.AssemblyLinearVelocity.Z) end
    end
end)
