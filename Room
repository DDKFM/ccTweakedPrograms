--==================================================================================================
--Program to make a turtle dig out a room of dimensions X*Y*Z extending forwards, up, and right from the turtle
--Author: Black_Humor
--Usage: place the turtle at the bottom left corner of the room you want to dig, 
--then type room <length> <height> <width> 

--Note: Avoid doing long even width rooms; the algorithm I'm using is kind of bad for those. Will optimise
--eventually.
--==================================================================================================


--get variables
local inputs = { ... }
if #inputs ~= 3 then
	print("Usage: <length> <height> <width>")
	return
end

local length = tonumber(inputs[1])
local height = tonumber(inputs[2])
local width = tonumber(inputs[3])
if length <= 0 then
	print("ERROR: Length must be greater than 0!")
	return
end
if height <= 0 then
	print("ERROR: Width must be greater than 0!")
	return
end
if width <= 0 then
	print("ERROR: Height must be greater than 0!")
	return
end

function tryDig()
	while turtle.detect() do
		turtle.dig()
		sleep(0.1)
	end
	return turtle.forward()
end

function tryFuel(amount)
	while turtle.getFuelLevel() <= amount do
		if turtle.refuel() then
			print("Refuelling...")
		else
			print("Not enough fuel!")
			break
		end
	end
end
--declare relevant functions
function digLine(X)	
	tryFuel(X)
	dug_X = 1
	while dug_X < X do
		if tryDig() then
		--incrementing here because don't want to incr for each piece of gravel dug
			dug_X = dug_X + 1
			if dug_X >= X then
				break
			end
		end
		
		--print("[dug_X = ", dug_X, ", X = ", X, "]")
	end
end

function digSlice(X, Z)
	print ("Digging slice of length ", X, " and width ", Z)
	
	tryFuel(X*Z)
	dug_Z = 0
	while dug_Z < Z do
		print("Digging line ", dug_Z + 1, "/", Z)
		digLine(X)
		dug_Z = dug_Z + 1
		if dug_Z >= Z then
			break --don't want to do all this other stuff; just end here
		end
		
		if (dug_Z % 2) == 1 then 
			turtle.turnRight()
		else
			turtle.turnLeft()
		end
		
		tryDig()
		
		if (dug_Z % 2) == 1 then 
			turtle.turnRight()
		else
			turtle.turnLeft()
		--print("[dug_Z = ", dug_Z, ", Z = ", Z, "]")
		end
	end
end

function digRoom(X, Y, Z)
	tryFuel(X*Y*Z)
	dug_Y = 0
	while dug_Y < Y do --this condition should actually break the loop almost never
		print("Digging line ", dug_Y + 1, "/", Y)
		digSlice(X, Z)
		--check the condition
		dug_Y = dug_Y + 1
		if dug_Y >= Y then
			break
		end
		
		--figure out which end of the room the turtle is at
		turtle.turnRight()
		if (Z % 2) == 1 then
			turtle.turnRight()			
		end
		
		--now, go up
		while turtle.detectUp() do
			turtle.digUp()
		end
		turtle.up()
		
		if (Z % 2) == 0 then -- if perpendicular, need to reverse x and z
			digSlice(Z, X)
		else
			digSlice(X, Z)
		end
		
		dug_Y = dug_Y + 1
		if dug_Y >= Y then
			break
		end
		
		turtle.turnRight()
		if (Z % 2) == 1 then
			turtle.turnRight()			
		end
		
		--now, go up
		while turtle.detectUp() do
			turtle.digUp()
		end
		turtle.up()
		
		--print("[dug_Y = ", dug_Y, ", Y = ", Y, "]")
	end
end
--main loop
digRoom(length, height, width)
