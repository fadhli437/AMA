ocal p, uis, rs, cam, lp, vi = game:GetService("Players"), game:GetService("UserInputService"), game:GetService("RunService"), workspace.CurrentCamera, game.Players.LocalPlayer, game:GetService("VirtualInputManager")
local screenGui = Instance.new("ScreenGui", lp:WaitForChild("PlayerGui"))
screenGui.Name = "AmbatronHub"

local frame = Instance.new("Frame", screenGui)
frame.Size, frame.Position = UDim2.new(0, 200, 0, 220), UDim2.new(0, 20, 0, 20)
frame.BackgroundColor3, frame.BorderSizePixel, frame.Active, frame.Draggable = Color3.fromRGB(40, 40, 40), 0, true, true

local uilist = Instance.new("UIListLayout", frame)
uilist.Padding, uilist.FillDirection, uilist.HorizontalAlignment, uilist.VerticalAlignment = UDim.new(0, 6), Enum.FillDirection.Vertical, Enum.HorizontalAlignment.Center, Enum.VerticalAlignment.Top

local function createButton(text)
    local btn = Instance.new("TextButton")
    btn.Size, btn.Text, btn.BackgroundColor3, btn.TextColor3, btn.Font, btn.TextSize, btn.BorderSizePixel, btn.AutoButtonColor = UDim2.new(0, 180, 0, 30), text, Color3.fromRGB(60, 60, 60), Color3.new(1, 1, 1), Enum.Font.Gotham, 14, 0, true
    return btn
end

-- ESP
local espBtn, espEnabled = createButton("Toggle ESP"), false
espBtn.Parent = frame

local function createESP(target)
    local text = Drawing.new("Text")
    text.Visible, text.Center, text.Outline, text.Font, text.Size, text.Color, text.Text = false, true, true, 2, 14, Color3.fromRGB(0, 255, 0), target.Name
    rs.RenderStepped:Connect(function()
        if target and target.Parent then
            local part = target:FindFirstChild("HumanoidRootPart") or target:FindFirstChild("PrimaryPart")
            if part then
                local pos, onScreen = cam:WorldToViewportPoint(part.Position)
                text.Position, text.Visible = Vector2.new(pos.X, pos.Y), onScreen
            else
                text.Visible = false
            end
        else
            text.Visible = false
        end
    end)
    return text
end

local function addESPToObjects()
    for _, object in pairs(workspace:GetChildren()) do
        if object:FindFirstChild("Humanoid") or object:IsA("Model") then createESP(object) end
    end
end

espBtn.MouseButton1Click:Connect(function()
    espEnabled = not espEnabled
    if espEnabled then addESPToObjects() end
end)

-- Aimbot
local aimbotBtn, aimbotEnabled = createButton("Toggle Aimbot (Q)"), false
aimbotBtn.Parent = frame

local function getClosestTarget()
    local closest, shortest = nil, 100
    for _, p in ipairs(p:GetPlayers()) do
        if p ~= lp and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
            local pos, onScreen = cam:WorldToViewportPoint(p.Character.HumanoidRootPart.Position)
            if onScreen then
                local mouse = uis:GetMouseLocation()
                local dist = (Vector2.new(pos.X, pos.Y) - Vector2.new(mouse.X, mouse.Y)).Magnitude
                if dist < shortest then
                    shortest, closest = dist, p
                end
            end
        end
    end
    return closest
end

uis.InputBegan:Connect(function(input) 
    if input.KeyCode == Enum.KeyCode.Q then
        aimbotEnabled = not aimbotEnabled
    end
end)

rs.RenderStepped:Connect(function()
    if aimbotEnabled then
        local target = getClosestTarget()
        if target and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
            cam.CFrame = CFrame.new(cam.CFrame.Position, target.Character.HumanoidRootPart.Position)
        end
    end
end)

-- Fly
local flyBtn, flying, bg, bv = createButton("Toggle Fly (F)"), false
flyBtn.Parent = frame

local function startFly()
    local hrp = lp.Character and lp.Character:FindFirstChild("HumanoidRootPart")
    if hrp then
        bg = Instance.new("BodyGyro", hrp)
        bg.P, bg.maxTorque, bg.CFrame = 9e4, Vector3.new(9e9, 9e9, 9e9), hrp.CFrame
        bv = Instance.new("BodyVelocity", hrp)
        bv.velocity, bv.maxForce = Vector3.zero, Vector3.new(9e9, 9e9, 9e9)
    end
end

uis.InputBegan:Connect(function(i)
    if i.KeyCode == Enum.KeyCode.F then
        flying = not flying
        if flying then startFly() else if bg then bg:Destroy() end if bv then bv:Destroy() end end
    end
end)

rs.RenderStepped:Connect(function()
    if flying and lp.Character then
        local hrp = lp.Character:FindFirstChild("HumanoidRootPart")
        if hrp and bv and bg then
            local move = Vector3.new()
            local cam = workspace.CurrentCamera
            if uis:IsKeyDown(Enum.KeyCode.W) then move += cam.CFrame.LookVector end
            if uis:IsKeyDown(Enum.KeyCode.S) then move -= cam.CFrame.LookVector end
            if uis:IsKeyDown(Enum.KeyCode.A) then move -= cam.CFrame.RightVector end
            if uis:IsKeyDown(Enum.KeyCode.D) then move += cam.CFrame.RightVector end
            if uis:IsKeyDown(Enum.KeyCode.Space) then move += Vector3.new(0, 1, 0) end
            if uis:IsKeyDown(Enum.KeyCode.LeftControl) then move -= Vector3.new(0, 1, 0) end
            bv.velocity = move.Unit * 50
            bg.CFrame = cam.CFrame
        end
    end
end)

-- Fast Run
local fastRunBtn, fastRunning = createButton("Toggle Fast Run (G)"), false
fastRunBtn.Parent = frame

uis.InputBegan:Connect(function(i)
    if i.KeyCode == Enum.KeyCode.G then fastRunning = not fastRunning end
end)

rs.RenderStepped:Connect(function()
    lp.Character.Humanoid.WalkSpeed = fastRunning and 100 or 16
end)

-- No Clip
local noClipBtn, noClipEnabled = createButton("Toggle No Clip (H)"), false
noClipBtn.Parent = frame

uis.InputBegan:Connect(function(i)
    if i.KeyCode == Enum.KeyCode.H then noClipEnabled = not noClipEnabled end
end)

rs.RenderStepped:Connect(function()
    if noClipEnabled and lp.Character then
        lp.Character.Humanoid:ChangeState(Enum.HumanoidStateType.Physics)
    else
        lp.Character.Humanoid:ChangeState(Enum.HumanoidStateType.GettingUp)
    end
end)

-- Auto Clicker
local clickerBtn, autoClickEnabled, delayPerClick = createButton("Toggle Auto Clicker (E)"), false, 0.1
clickerBtn.Parent = frame

uis.InputBegan:Connect(function(input) 
    if input.KeyCode == Enum.KeyCode.E then autoClickEnabled = not autoClickEnabled end
end)

task.spawn(function()
    while true do
        if autoClickEnabled then
            local mouse = uis:GetMouseLocation()
            vi:SendMouseButtonEvent(mouse.X, mouse.Y, 0, true, game, 0)
            vi:SendMouseButtonEvent(mouse.X, mouse.Y, 0, false, game, 0)
        end
        task.wait(delayPerClick)
    end
end)
