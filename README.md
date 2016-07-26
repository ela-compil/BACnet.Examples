### Bacnet.Room.Simulator

This windows form application *simulates* an heating/cooling room controler. See the Readme file in the application directory.

### BasicReadWrite

* Send a `Whois` to all devices on the net
* Get back all the `Iam` responses
* Read `Present_Value` property on the object `ANALOG_INPUT:1` provided by the device 1026
* Write `Present_Value` property on the object `ANALOG_OUTPUT:0` provided by the device 4000

### BasicAdviseCOV

* Send a `Whois` to all devices on the net
* Get back all the `Iam` responses
* Advise to `OBJECT_ANALOG_INPUT:1` provided by the device 1026, for 60 secondes
* Write on the console each notification

### BasicAlarmListener

* Can send reponses to `WhoIs` query : own device id is 2000
* Can send responses to `ReadProperty`/`ReadPropertyMultiple`
* Write on the console each `Alarm` or `Event` received (broadcast or unicast)

### BasicServer

* Send an `Iam` message : own device id is 1234
* Can send reponses to `WhoIs` query
* Offers three objects `OBJECT_DEVICE:1234`, `OBJECT_ANALOG_INPUT:0`, `OBJECT_ANALOG_VALUE:0`
* Only `OBJECT_ANALOG_VALUE:0.PRESENT_VALUE` can be writen
* `OBJECT_ANALOG_INPUT_0.PRESENT_VALUE` changes continously: `PRESENT_VALUE = OBJECT_ANALOG_VALUE_0.PRESENT_VALUE * Sin (w.t)`

### BBMDDemo

* BBMD services on a simple device server with only one Device Object
* Foreign devices accepted
* Tested with
 * Wago 750/830 (vendor Id 222)
 * Newron DoGate (vendor Id 451)
 * Sauter EY-AS521 (vendor Id 80)
* Also running on a Raspberry Pi on Linux/Mono

### AnotherStorageImplementation

Shows another way, much complex than the original one in DeviceStorage.cs file (without the Xml descriptor also) to write a server. Each Bacnetobject is a object in the C# code. So a class must be written for each Bacnet object type, but this give the possibility to have a complex behaviour in objects code. Shows also dynamic creation/destruction of objects by a remote client. Actual objects types are :

* Device
* Structured View
* Analog Input, Analog Value, Analog Output (With Intrinsect reporting)
* Digital Input, Digial Value, Digital Output
* Multistates Input, Multistates Value, Multistates Output (With Intrinsect reporting)
* Characters String
* File
* Trendlog
* Notification Class
* Schedule
* Calendar


Objects persistance could be achieved with serialization

### RaspberrySample

This application is similar to BasicServer, for Raspberry Pi with Mono. DeviceDescriptor.xml should be modify in order to access as your want the GPIO pins :

* `OBJECT_BINARY_INPUT:x` - GPIOX will be configured and used as an input
* `OBJECT_BINARY_OUTPUT:x` - GPIOX will be configured and used as an output
* `PRESENT_VALUE` will be apply to the output at the begining

When adding new objects, take care to add it also in the `PROP_OBJECT_LIST` of the `DEVICE_OBJECT`. In the original DeviceDescriptor.xml file one can found GPIO4 as input and GPIO7, GPIO8 as output. Also `OBJECT_ANALOG_INPUT:0` get the CPU temperature. Mono should be installed (mono complete not mono runtime it's not enough !), and to start the code in sudo mode : `sudo mono ./RaspberrySample.exe`. DeviceDescriptor.xml must be in the application directory

Tested on a Raspberry Pi Model B. Similar code based on AnotherStorageImplementation. Run 24/24 acting also as a BBMD somewhere in France. Ready with small modifications (GPIO) for Intel/Edison, Texas/BeagleBone, and a  lot of Linux plateforms with Mono installed.

### BacnetToDatabase

A sample application that will transfer all present values from a given device, to a SQL database. (SQL CE Local DB.)

### ObjectBrowseSample

In theory each bacnet device should have object of type OBJECT_DEVICE with property PROP_OBJECT_LIST. This property should contain a list of all bacnet objects (ids) of that device. In this sample we detect bacnet device in local network and try to list its object ids by reading this property. 