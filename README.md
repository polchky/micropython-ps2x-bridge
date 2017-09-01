This library was created to read a Playstation Dualshock controller in micropython using an external arduino board. It is divided in two parts:
* The arduino code reads the gamepad data, using the [Arduino-PS2X](https://github.com/madsci1016/Arduino-PS2X) library. You need to install it on the arduino board. Please refer to [Bill Porter's excellent guide](http://www.billporter.info/2010/06/05/playstation-2-controller-arduino-library-v1-0/) on how to connect a dualshock controller to an arduino board. Pay attention to the troubleshooting guide.
* The micropython library receives the gamepad data over serial communication with the arduino board.
## Usage
Upload the arduino code on a microcontroller, connect it both to the dualshock controller and to the micropython board. Then:
```python
import ps2x_controller
controller = ps2x_controller.Controller()

# single update
controller.update()
print("R1 button state: " + str(controller.value('PSB_R1')))
print("left x-axis joystick value: " + str(controller.value('PSS_LX')))

# continuous updates
while(True):
	controller.update()
	if(controller.clicked('PSB_TRIANGLE')):
		print("triangle button just pressed!")


```
By default, the controller uses uart's 2nd bus (tested on an ESP32 board), please refer to your board's schematics to find the RX and TX pins (16 & 17 resp. on the ESP32).
The controller update needs to be called manually. This is because a pressed button is "clicked" only in the first update during which it is pressed.
The update() function returns True if the update was successful, False otherwise.
