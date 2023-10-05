local isFKeyBeingPressed = false

local function toggleScript()
    isFKeyBeingPressed = not isFKeyBeingPressed
    if isFKeyBeingPressed then
        print("Auto Parry is ON")
    else
        print("Auto Parry is OFF")
    end
end

local gui = Instance.new("ScreenGui")
gui.Parent = game.Players.LocalPlayer.PlayerGui

local frame = Instance.new("Frame")
frame.Parent = gui
frame.Size = UDim2.new(0, 200, 0, 100)
frame.Position = UDim2.new(0.5, -100, 0.5, -50)
frame.BackgroundColor3 = Color3.new(0, 0, 0)
frame.BorderSizePixel = 0

local inputTextBox = Instance.new("TextBox")
inputTextBox.Parent = frame
inputTextBox.Size = UDim2.new(1, 0, 0.8, 0)
inputTextBox.Position = UDim2.new(0, 0, 0, 0)
inputTextBox.BackgroundColor3 = Color3.new(1, 1, 1)
inputTextBox.PlaceholderText = "Enter your code here"

local toggleButton = Instance.new("TextButton")
toggleButton.Parent = frame
toggleButton.Size = UDim2.new(1, 0, 0.2, 0)
toggleButton.Position = UDim2.new(0, 0, 0.8, 0)
toggleButton.BackgroundColor3 = Color3.new(0, 1, 0)
toggleButton.Text = "Toggle Script"
toggleButton.TextColor3 = Color3.new(1, 1, 1)

toggleButton.MouseButton1Click:Connect(toggleScript)

-- ...

local function checkProximityToPlayer(ball, player)
    local predictionTime = calculatePredictionTime(ball, player)
    local realBallAttribute = ball:GetAttribute("realBall")
    local target = ball:GetAttribute("target")
    
    local ballSpeedThreshold = math.max(0.4, 0.6 - ball.Velocity.magnitude * 0.01)

    if predictionTime <= ballSpeedThreshold and realBallAttribute == true and target == player.Name and not isFKeyBeingPressed then
        vim:SendKeyEvent(true, Enum.KeyCode.F, false, nil)
        wait(0.005)
        vim:SendKeyEvent(false, Enum.KeyCode.F, false, nil)
        lastBallPressed = ball
        isFKeyBeingPressed = true
    elseif lastBallPressed == ball and (predictionTime > ballSpeedThreshold or realBallAttribute ~= true or target ~= player.Name) then
        isFKeyBeingPressed = false
    end
end

-- ...
