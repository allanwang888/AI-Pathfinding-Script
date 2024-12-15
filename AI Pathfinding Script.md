


-- Reference to the AI model
local ai = script.Parent -- The script's parent should be the AI model

-- Get the Humanoid and HumanoidRootPart of the AI
local humanoid = ai:FindFirstChild("Humanoid") -- Needed for moving the AI
local rootPart = ai:FindFirstChild("HumanoidRootPart") -- Used for positioning

-- Check if the Humanoid or HumanoidRootPart exists, and stop the script if missing
if not humanoid or not rootPart then
    warn("AI requires a Humanoid and HumanoidRootPart!") -- Warn developers if the setup is incorrect
    return -- Exit the script
end

-- Reference the folder containing the waypoints
local waypointsFolder = game.Workspace:FindFirstChild("Waypoints") -- The folder should be in Workspace
if not waypointsFolder then
    warn("Waypoints folder not found!") -- Warn if the folder is missing
    return -- Exit the script
end

-- Get all waypoint parts inside the folder
local waypoints = waypointsFolder:GetChildren() -- Returns a table of all child objects in the folder

-- Sort the waypoints by name (e.g., Waypoint1, Waypoint2, etc.)
table.sort(waypoints, function(a, b) return a.Name < b.Name end) -- Ensures a consistent path order

-- Initialize the index for the current waypoint
local currentIndex = 1 -- Start with the first waypoint

-- Function to move the AI to the next waypoint
local function moveToNextWaypoint()
    if #waypoints == 0 then return end -- Exit if there are no waypoints

    -- Get the current target waypoint
    local target = waypoints[currentIndex]

    -- Use the Humanoid to move the AI to the waypoint's position
    humanoid:MoveTo(target.Position) 

    -- Wait until the AI reaches the waypoint
    humanoid.MoveToFinished:Wait()

    -- Update to the next waypoint in a loop
    currentIndex = currentIndex + 1 -- Move to the next index
    if currentIndex > #waypoints then -- If the last waypoint is reached,
        currentIndex = 1 -- Loop back to the first waypoint
    end
end

-- Main loop: continuously move the AI along the path
while task.wait() do
    moveToNextWaypoint() -- Call the function to move the AI to the next waypoint
end





























