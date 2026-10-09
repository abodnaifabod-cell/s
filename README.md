local player = game.Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

local touchGui = playerGui:FindFirstChild("TouchGui")
if touchGui then
    local touchControlFrame = touchGui:FindFirstChild("TouchControlFrame")
    if touchControlFrame then
        local jumpButton = touchControlFrame:FindFirstChild("JumpButton")
        if jumpButton then
            -- تكبير حجم الزر وجعله واضحاً
            jumpButton.Size = UDim2.new(0, 220, 0, 220)
            
            -- نقل الزر إلى المكان المحدّد في أسفل اليمين بجانب زر القفل
            jumpButton.Position = UDim2.new(1, -240, 1, -260)
        end
    end
end