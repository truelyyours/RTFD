_SCADA (Supervisory Control and Data Acquisition)_ systems are widely used in industrial control applications to monitor and control complex processes. One of the most popular communication protocols in SCADA systems is _Modbus_, a simple and widely adopted serial communication protocol. Modbus enables communication with Programmable Logic Controllers (PLCs) and other automation devices, allowing operators to read and write data. It supports several communication modes, including serial and Ethernet, and is compatible with various devices and systems.

Simulators are typically designed for simplicity, enabling a master-slave pairing-a key feature of Modbus communication. This pairing ensures efficient and reliable data exchange between devices in an industrial control system. Communication between systems is often carried out through the Modbus/TCP protocol, which is widely used by active PLCs in SCADA environments.
# Getting IP Address

An IP address is a unique identifier assigned to every device that connects to a network. It allows devices to communicate with each other over the internet or other networks.

To display information about network interfaces, run the following command:
`ip addr`

The output will display information about network interfaces, including the IP address, subnet mask, and default gateway.
### Modbus Slave

A _Modbus slave_ is a device that responds to commands from a Modbus master. It has an address that the master uses to identify and communicate with the device. The slave contains a set of registers, which can be read or written by the Modbus master, depending on the communication configuration.

_Diagslave_ is a free, open-source Modbus slave simulator and diagnostic tool. It allows users to simulate Modbus slave devices and test the behavior of the Modbus master. Diagslave is useful for troubleshooting and provides detailed communication data between master and slave devices.

To change the working directory to the Modbus slave device simulator, use the following command:
`cd /modbus/diagslave/x86_64-linux-gnu`

To display the help menu for Diagslave, enter:
`./diagslave --help`
# Starting Modbus Slave

The slave ID uniquely identifies the Modbus device on the network and is used by the Modbus master to address individual devices.

To start a Modbus TCP slave with the address 123 using the diagslave tool, run the following command.

sudo ./diagslave -m tcp -a 123

The _-m_ option specifies the Modbus protocol for communication, the _-a_ option sets the Modbus address or slave ID, and the slave process is initiated on TCP port 123, where it listens for incoming Modbus TCP messages from the master.

### Modbus Master

A Modbus master is a device that initiates commands and requests to Modbus slaves, sending requests to read or write data and waiting for responses.

A Modbus master simulator, like Modpoll, is a command-line tool that simulates a Modbus master and allows communication with slave devices to perform read and write operations.
To change to the Modbus master simulator directory, run:
`cd /modbus/modpoll/x86_64-linux-gnu`

To display the help menu for Modpoll, use:
`./modpoll --help`
# Polling Slave Device

Polling a Modbus slave device refers to the process in which a Modbus master sends a request to retrieve data or update a value.

To retrieve 5 values from a Modbus slave device using the Modbus TCP protocol, run the following command:
`./modpoll -m tcp -a 123 -r 100 -c 5 -1 `192.168.1.102

The _-r_ option specifies the starting register address, the _-c_ option sets the number of data points to read, and the _-1_ option defines the output format and file name.
# Read Request

To analyze requests on the Modbus slave simulator, switch back to the _first terminal_ and stop Diagslave using the _Ctrl+C_ keyboard shortcut.

Upon successful execution of the _readHoldingRegisters_ request, the request details, along with the master's IP address and the number of references, will be visible in the slave's output.

_readHoldingRegisters_ is a Modbus function code used to request a range of holding registers from a Modbus slave. Holding registers are memory locations that can be read or written by the Modbus master.

_Coils_ are data registers used to control discrete outputs, such as turning devices on/off, setting alarms, or controlling other outputs.

To start a Modbus TCP slave with the new address 124 using the Diagslave tool, run the following command:

sudo ./diagslave -m tcp -a 124 -c 10

The _-c_ option specifies the number of coils to simulate or respond to on the Modbus slave.

### Writing Registers

Writing registers in a Modbus master involves sending commands to slave devices to update data in their registers.

Return to the second terminal where the Modbus master simulator is running.

To write the value 333 into register 100 of the slave device, use the following command:

./modpoll -m tcp -a 124 -r 100 -1 192.168.1.102 333
