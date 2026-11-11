local tool = script.Parent
local event = tool:WaitForChild("ShootEvent")

event.OnServerEvent:Connect(function(player, origin, direction)
    if tool.Parent ~= player.Character then return end

    if typeof(origin) ~= "Vector3"
        or typeof(direction) ~= "Vector3" then
        return
    end

    if direction.Magnitude < 0.99
        or direction.Magnitude > 1.01 then
        return
    end

    local params = RaycastParams.new()
    params.FilterType = Enum.RaycastFilterType.Exclude
    params.FilterDescendantsInstances = {player.Character}

    local result = workspace:Raycast(
        origin,
        direction.Unit * 500,
        params
    )

    if result then
        local model = result.Instance:FindFirstAncestorOfClass("Model")
        local humanoid = model and model:FindFirstChildOfClass("Humanoid")

        if humanoid and model ~= player.Character then
            humanoid:TakeDamage(20)
        end
    end
end)
