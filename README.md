-- Servicios
local Players = game:GetService("Players")

-- Variables
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

-- Especifica la ubicación del modelo en el workspace
local modelPath = "Workspace.NombreDelModelo" -- Reemplaza con la ruta correcta

local partName = "NombreDeLaParte" -- Reemplaza con el nombre de la parte dentro del modelo

-- Función para teletransportar al jugador
local function teletransportarJugador()
    -- Buscar el modelo en la ubicación especificada
    local model = game:GetService("DataModel"):FindFirstChild(modelPath)
    
    if not model then
        warn("Modelo no encontrado en la ubicación:", modelPath)
        return
    end

    -- Buscar la parte dentro del modelo
    local part = model:FindFirstChild(partName)
    if not part then
        warn("Parte no encontrada:", partName)
        return
    end

    -- Obtener la posición de la parte
    local nuevaPosicion = part.Position + Vector3.new(0, 2, 0) -- Ajusta la altura según sea necesario

    -- Teletransportar al jugador
    if character and character:FindFirstChild("HumanoidRootPart") then
        character:MoveTo(nuevaPosicion)
        print("Jugador teletransportado a:", nuevaPosicion)
    else
        warn("HumanoidRootPart no encontrado en el personaje")
    end
end

-- Conectar la función a un evento o acción (ejemplo: al presionar una tecla)
local userInputService = game:GetService("UserInputService")
userInputService.InputBegan:Connect(function(input, gameProcessedEvent)
    if gameProcessedEvent then return end -- Ignorar si el juego ya procesó el evento

    if input.KeyCode == Enum.KeyCode.T then -- Presiona la tecla 'T' para teletransportarte
        teletransportarJugador()
    end
end)

print("Script de teletransporte local activado.")
