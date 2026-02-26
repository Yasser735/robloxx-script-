--[[
    VIOLENCE DISTRICT - WINDUI EDITION
    Menggunakan script dari TexRBLX sebagai basis
    Fitur asli 100% dipertahankan, hanya UI yang diganti dengan WindUI
]]

-- Load WindUI Library
local WindUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/Footagesus/WindUI/main/dist/main.lua"))()

-- ==================== FITUR ASLI (100% MURNI) ====================
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Workspace = game:GetService("Workspace")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local Lighting = game:GetService("Lighting")
local TeleportService = game:GetService("TeleportService")
local VirtualUser = game:GetService("VirtualUser")
local HttpService = game:GetService("HttpService")
local TweenService = game:GetService("TweenService")
local CoreGui = game:GetService("CoreGui")
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()
local Camera = Workspace.CurrentCamera

-- Anti Afk
LocalPlayer.Idled:Connect(function()
    VirtualUser:CaptureController()
    VirtualUser:ClickButton2(Vector2.new())
end)

-- Variables Asli
local SpeedEnabled = false
local SpeedValue = 16
local JumpEnabled = false
local JumpValue = 50
local FlyEnabled = false
local NoclipEnabled = false
local InfJumpEnabled = false
local ESPEnabled = false
local AimbotEnabled = false
local SilentAimEnabled = false
local TriggerbotEnabled = false
local TriggerDelay = 0.1
local AimPart = "Head"
local FOV = 90
local TeamCheck = false
local VisibleCheck = true
local ESPColor = Color3.fromRGB(255, 50, 50)
local FullbrightEnabled = false
local TimeValue = 12
local GravityValue = 196.2
local FOVValue = 90
local TargetPlayer = nil
local UIEnabled = true

-- Fly (Fitur Asli)
local flying = false
local bodyGyro, bodyVelocity

function EnableFly()
    if not LocalPlayer.Character or not LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then return end
    flying = true
    local hrp = LocalPlayer.Character.HumanoidRootPart
    bodyGyro = Instance.new("BodyGyro")
    bodyGyro.Parent = hrp
    bodyGyro.MaxTorque = Vector3.new(9e9, 9e9, 9e9)
    bodyGyro.P = 9e9
    bodyGyro.CFrame = hrp.CFrame
    bodyVelocity = Instance.new("BodyVelocity")
    bodyVelocity.Parent = hrp
    bodyVelocity.MaxForce = Vector3.new(9e9, 9e9, 9e9)
    bodyVelocity.Velocity = Vector3.new(0, 0, 0)
    
    spawn(function()
        while flying do
            wait()
            local move = Vector3.new(0, 0, 0)
            if UserInputService:IsKeyDown(Enum.KeyCode.W) then
                move = move + Camera.CFrame.LookVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.S) then
                move = move - Camera.CFrame.LookVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.A) then
                move = move - Camera.CFrame.RightVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.D) then
                move = move + Camera.CFrame.RightVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.Space) then
                move = move + Vector3.new(0, 1, 0)
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.LeftControl) then
                move = move - Vector3.new(0, 1, 0)
            end
            bodyVelocity.Velocity = move * (SpeedValue * 2)
            bodyGyro.CFrame = CFrame.new(hrp.Position, hrp.Position + Camera.CFrame.LookVector)
        end
    end)
end

function DisableFly()
    flying = false
    if bodyGyro then bodyGyro:Destroy() end
    if bodyVelocity then bodyVelocity:Destroy() end
end

-- Noclip (Fitur Asli)
local noclipConnection
function SetNoclip(state)
    if noclipConnection then
        noclipConnection:Disconnect()
        noclipConnection = nil
    end
    if state then
        noclipConnection = RunService.Stepped:Connect(function()
            if LocalPlayer.Character then
                for _, part in ipairs(LocalPlayer.Character:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = false
                    end
                end
            end
        end)
    end
end

-- Infinite Jump (Fitur Asli)
local infJumpConnection
function SetInfJump(state)
    if infJumpConnection then
        infJumpConnection:Disconnect()
        infJumpConnection = nil
    end
    if state then
        infJumpConnection = UserInputService.JumpRequest:Connect(function()
            if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
                LocalPlayer.Character.Humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
            end
        end)
    end
end

-- ESP (Fitur Asli)
local ESP = {}

function EnableESP()
    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= LocalPlayer then
            local esp = {}
            esp.Player = player
            
            local box = Drawing.new("Square")
            box.Visible = false
            box.Color = ESPColor
            box.Thickness = 1
            box.Filled = false
            
            local nameLabel = Drawing.new("Text")
            nameLabel.Visible = false
            nameLabel.Color = ESPColor
            nameLabel.Center = true
            nameLabel.Size = 16
            
            local healthBar = Drawing.new("Square")
            healthBar.Visible = false
            healthBar.Color = Color3.fromRGB(0, 255, 0)
            healthBar.Filled = true
            
            local healthBg = Drawing.new("Square")
            healthBg.Visible = false
            healthBg.Color = Color3.fromRGB(50, 50, 50)
            healthBg.Filled = true
            
            function esp.Update()
                if not player.Character or not player.Character:FindFirstChild("HumanoidRootPart") or not player.Character:FindFirstChild("Humanoid") then
                    return
                end
                local hrp = player.Character.HumanoidRootPart
                local humanoid = player.Character.Humanoid
                local pos, onScreen = Camera:WorldToViewportPoint(hrp.Position)
                
                if not onScreen then
                    box.Visible = false
                    nameLabel.Visible = false
                    healthBar.Visible = false
                    healthBg.Visible = false
                    return
                end
                
                local scale = 1 / (pos.Z * 0.001) * 2
                local width = 50 * scale
                local height = 100 * scale
                
                box.Visible = true
                box.Position = Vector2.new(pos.X - width/2, pos.Y - height/2)
                box.Size = Vector2.new(width, height)
                
                nameLabel.Visible = true
                nameLabel.Position = Vector2.new(pos.X, pos.Y - height/2 - 20)
                nameLabel.Text = player.Name
                
                healthBg.Visible = true
                healthBg.Position = Vector2.new(pos.X - width/2 - 5, pos.Y - height/2)
                healthBg.Size = Vector2.new(3, height)
                
                local healthPercent = humanoid.Health / humanoid.MaxHealth
                local healthHeight = height * healthPercent
                healthBar.Visible = true
                healthBar.Position = Vector2.new(pos.X - width/2 - 5, pos.Y + height/2 - healthHeight)
                healthBar.Size = Vector2.new(3, healthHeight)
                healthBar.Color = Color3.fromRGB(255 * (1 - healthPercent), 255 * healthPercent, 0)
            end
            
            ESP[player] = esp
        end
    end
end

function DisableESP()
    for _, esp in pairs(ESP) do
        if esp.box then esp.box:Remove() end
        if esp.nameLabel then esp.nameLabel:Remove() end
        if esp.healthBar then esp.healthBar:Remove() end
        if esp.healthBg then esp.healthBg:Remove() end
    end
    ESP = {}
end

-- Aimbot (Fitur Asli)
function getClosestPlayer()
    local closest = nil
    local shortest = FOV
    local center = Vector2.new(Camera.ViewportSize.X/2, Camera.ViewportSize.Y/2)
    
    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") and player.Character:FindFirstChild("Humanoid") and player.Character.Humanoid.Health > 0 then
            if TeamCheck and player.Team == LocalPlayer.Team then
                continue
            end
            local pos = Camera:WorldToViewportPoint(player.Character[AimPart].Position)
            if pos.Z > 0 then
                local screenPos = Vector2.new(pos.X, pos.Y)
                local dist = (screenPos - center).Magnitude
                if dist < shortest then
                    if VisibleCheck then
                        local ray = Ray.new(Camera.CFrame.Position, (player.Character[AimPart].Position - Camera.CFrame.Position).Unit * 1000)
                        local hit, _ = Workspace:FindPartOnRayWithIgnoreList(ray, {LocalPlayer.Character, Camera})
                        if hit and hit:IsDescendantOf(player.Character) then
                            closest = player
                            shortest = dist
                        end
                    else
                        closest = player
                        shortest = dist
                    end
                end
            end
        end
    end
    return closest
end

-- ==================== WINDUI GUI ====================
local Window = WindUI:CreateWindow({
    Title = "Violence District",
    SubTitle = "Original Features",
    Folder = "ViolenceDistrict",
    Icon = "solar:skull-bold-duotone",
    Size = UDim2.fromOffset(500, 450),
    ToggleKey = Enum.KeyCode.RightShift,
    Topbar = {
        Height = 44,
        ButtonsType = "Mac",
    },
})

-- Tab Aimbot
local AimbotTab = Window:Tab({ Title = "Aimbot", Icon = "solar:target-bold" })
local AimbotSection = AimbotTab:Section({ Title = "Aimbot Settings" })

AimbotSection:Toggle({
    Title = "Enable Aimbot",
    Default = AimbotEnabled,
    Callback = function(v) AimbotEnabled = v end
})

AimbotSection:Toggle({
    Title = "Silent Aim",
    Default = SilentAimEnabled,
    Callback = function(v) SilentAimEnabled = v end
})

AimbotSection:Toggle({
    Title = "Triggerbot",
    Default = TriggerbotEnabled,
    Callback = function(v) TriggerbotEnabled = v end
})

AimbotSection:Slider({
    Title = "Trigger Delay",
    Min = 0,
    Max = 1,
    Default = TriggerDelay,
    Decimals = 2,
    Callback = function(v) TriggerDelay = v end
})

AimbotSection:Dropdown({
    Title = "Aim Part",
    Values = { "Head", "Torso", "HumanoidRootPart" },
    Default = AimPart,
    Callback = function(v) AimPart = v end
})

AimbotSection:Slider({
    Title = "FOV",
    Min = 10,
    Max = 360,
    Default = FOV,
    Callback = function(v) FOV = v end
})

AimbotSection:Toggle({
    Title = "Team Check",
    Default = TeamCheck,
    Callback = function(v) TeamCheck = v end
})

AimbotSection:Toggle({
    Title = "Visible Check",
    Default = VisibleCheck,
    Callback = function(v) VisibleCheck = v end
})

-- Tab ESP
local ESPTab = Window:Tab({ Title = "ESP", Icon = "solar:eye-bold" })
local ESPSection = ESPTab:Section({ Title = "ESP Settings" })

ESPSection:Toggle({
    Title = "Enable ESP",
    Default = ESPEnabled,
    Callback = function(v)
        ESPEnabled = v
        if v then EnableESP() else DisableESP() end
    end
})

ESPSection:ColorPicker({
    Title = "ESP Color",
    Default = ESPColor,
    Callback = function(c) ESPColor = c end
})

-- Tab Movement
local MovementTab = Window:Tab({ Title = "Movement", Icon = "solar:running-bold" })
local MovementSection = MovementTab:Section({ Title = "Movement Settings" })

MovementSection:Toggle({
    Title = "Speed Hack",
    Default = SpeedEnabled,
    Callback = function(v)
        SpeedEnabled = v
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
            LocalPlayer.Character.Humanoid.WalkSpeed = v and SpeedValue or 16
        end
    end
})

MovementSection:Slider({
    Title = "Speed Value",
    Min = 16,
    Max = 500,
    Default = SpeedValue,
    Callback = function(v)
        SpeedValue = v
        if SpeedEnabled and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
            LocalPlayer.Character.Humanoid.WalkSpeed = v
        end
    end
})

MovementSection:Toggle({
    Title = "Fly",
    Default = FlyEnabled,
    Callback = function(v)
        FlyEnabled = v
        if v then EnableFly() else DisableFly() end
    end
})

MovementSection:Toggle({
    Title = "Noclip",
    Default = NoclipEnabled,
    Callback = function(v)
        NoclipEnabled = v
        SetNoclip(v)
    end
})

MovementSection:Toggle({
    Title = "Infinite Jump",
    Default = InfJumpEnabled,
    Callback = function(v)
        InfJumpEnabled = v
        SetInfJump(v)
    end
})

-- Tab Visuals
local VisualsTab = Window:Tab({ Title = "Visuals", Icon = "solar:palette-bold" })
local VisualsSection = VisualsTab:Section({ Title = "Visual Settings" })

VisualsSection:Toggle({
    Title = "Fullbright",
    Default = FullbrightEnabled,
    Callback = function(v)
        FullbrightEnabled = v
        if v then
            Lighting.Brightness = 5
            Lighting.FogEnd = 1e10
            Lighting.GlobalShadows = false
            Lighting.Ambient = Color3.new(1, 1, 1)
        else
            Lighting.Brightness = 1
            Lighting.FogEnd = 1000
            Lighting.GlobalShadows = true
            Lighting.Ambient = Color3.new(0, 0, 0)
        end
    end
})

VisualsSection:Slider({
    Title = "Time",
    Min = 0,
    Max = 24,
    Default = TimeValue,
    Callback = function(v)
        TimeValue = v
        Lighting.ClockTime = v
    end
})

VisualsSection:Slider({
    Title = "Gravity",
    Min = 0,
    Max = 500,
    Default = GravityValue,
    Callback = function(v)
        GravityValue = v
        Workspace.Gravity = v
    end
})

-- Tab World
local WorldTab = Window:Tab({ Title = "World", Icon = "solar:globe-bold" })
local WorldSection = WorldTab:Section({ Title = "World Control" })

WorldSection:Button({
    Title = "Teleport to Mouse",
    Callback = function()
        if Mouse.Hit then
            LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(Mouse.Hit.p + Vector3.new(0, 5, 0))
        end
    end
})

-- Tab Misc
local MiscTab = Window:Tab({ Title = "Misc", Icon = "solar:widget-bold" })
local MiscSection = MiscTab:Section({ Title = "Miscellaneous" })

MiscSection:Button({
    Title = "Rejoin Server",
    Callback = function()
        TeleportService:Teleport(game.PlaceId, LocalPlayer)
    end
})

-- ==================== MAIN LOOP ASLI ====================
RunService.RenderStepped:Connect(function()
    -- Aimbot (Fitur Asli)
    if AimbotEnabled then
        local target = getClosestPlayer()
        if target then
            local part = target.Character[AimPart]
            if part then
                local targetPos = part.Position
                local camPos = Camera.CFrame.p
                local dir = (targetPos - camPos).unit
                Camera.CFrame = CFrame.new(camPos, camPos + dir)
            end
        end
    end
    
    -- Triggerbot (Fitur Asli)
    if TriggerbotEnabled and Mouse.Target then
        local player = Players:GetPlayerFromCharacter(Mouse.Target.Parent)
        if player and player ~= LocalPlayer then
            if TeamCheck and player.Team == LocalPlayer.Team then return end
            wait(TriggerDelay)
            mouse1press()
            wait(0.05)
            mouse1release()
        end
    end
    
    -- ESP (Fitur Asli)
    if ESPEnabled then
        for _, esp in pairs(ESP) do
            if esp.Update then
                esp.Update()
            end
        end
    end
end)

-- Character respawn handler (Fitur Asli)
LocalPlayer.CharacterAdded:Connect(function(char)
    wait(1)
    if SpeedEnabled and char and char:FindFirstChild("Humanoid") then
        char.Humanoid.WalkSpeed = SpeedValue
    end
    if FlyEnabled then
        EnableFly()
    end
    if NoclipEnabled then
        SetNoclip(true)
    end
end)

-- Notifikasi sukses
WindUI:Notify({
    Title = "Violence District",
    Content = "Original features loaded with WindUI",
    Duration = 3,
})

print("Violence District - WindUI Edition Loaded")q
