# auto-wallk
free
-- KODE UTAMA AUTO WALK ANDA DIMASUKKAN DI SINI
local Player = game.Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait() 
local Humanoid = Character:WaitForChild("Humanoid")

local AutoWalkSpeed = 10 
local isAutoWalking = false

local function toggleAutoWalk()
    isAutoWalking = not isAutoWalking
    if isAutoWalking then
        print("Auto Walk Diaktifkan! Tekan 'T' lagi untuk berhenti.")
    else
        Humanoid:Move(Vector3.new(0, 0, 0))
        print("Auto Walk Dinonaktifkan!")
    end
end

game:GetService("UserInputService").InputBegan:Connect(function(input, gameProcessedEvent)
    if input.KeyCode == Enum.KeyCode.T and not gameProcessedEvent then
        toggleAutoWalk()
    end
end)

game:GetService("RunService").Heartbeat:Connect(function()
    if isAutoWalking and Character.Parent and Humanoid.Health > 0 then
        local movementVector = Vector3.new(0, 0, -AutoWalkSpeed)
        Humanoid:Move(movementVector, false) 
    end
end)

Humanoid.Died:Connect(function()
    isAutoWalking = false
    local hrp = Character:FindFirstChild("HumanoidRootPart")
    if hrp then
        local existingFlyVelocity = hrp:FindFirstChild("FlyVelocity")
        if existingFlyVelocity then
            existingFlyVelocity:Destroy()
        end
    end
end)
