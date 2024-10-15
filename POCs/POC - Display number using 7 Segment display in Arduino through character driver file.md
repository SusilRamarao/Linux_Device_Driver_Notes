
My Youtube video : [Writing Linux Kernel Driver To Communicate With Arduino 7 Segment Display (youtube.com)](https://www.youtube.com/watch?v=6YoYSlV4WAs)

***Overall flow:***
We need to set an serial interface. The Kernel driver should initiate a character device file, read and then send it to the serial port.

***Arduino code:***
i`nt a=7; //definition digital 7  pins as pin to control 'a' section(segment)`
`int b=6; //definition digital 6  pins as pin to control 'b' section(segment)`
`int c=4; //definition digital 4  pins as pin to control 'c' section(segment)`
`int d=11;//definition digital 11 pins as pin to control 'd' section(segment)`
`int e=10;//definition digital 10 pins as pin to control 'e' section(segment)`
`int f=8; //definition digital 8  pins as pin to control 'f' section(segment)`
`int g=9; //definition digital 9  pins as pin to control 'g' section(segment)`
`int dp=5;//definition digital 5  pins as pin to control 'dp' section(segment)`
`void digital_0(void) //Segment display digital 0`
`{`
  `digitalWrite(a,HIGH);`
  `digitalWrite(b,HIGH);`
  `digitalWrite(c,HIGH);`
  `digitalWrite(d,HIGH);`
  `digitalWrite(e,HIGH);`
  `digitalWrite(f,HIGH);`
  `digitalWrite(g, LOW);`
  `digitalWrite(dp,LOW);`
`}`
`void digital_1(void) //Segment display digital 1`
`{`
  `digitalWrite(a,LOW);`
  `digitalWrite(b,HIGH);`
  `digitalWrite(c,HIGH);`
  `digitalWrite(d,LOW);`
  `digitalWrite(e,LOW);`
  `digitalWrite(f,LOW);`
  `digitalWrite(g,LOW);`
  `digitalWrite(dp,LOW);`
`}`
`void digital_2(void) //Segment display digital 2`
`{`
  `digitalWrite(a,HIGH);`
  `digitalWrite(b,HIGH);`
  `digitalWrite(c,LOW);`
  `digitalWrite(d,HIGH);`
  `digitalWrite(e,HIGH);`
  `digitalWrite(f,LOW);`
  `digitalWrite(g,HIGH);`
  `digitalWrite(dp,LOW);`
`}`
`void digital_3(void) //Segment display digital 3`
`{`
  `digitalWrite(a,HIGH);`
  `digitalWrite(b,HIGH);`
  `digitalWrite(c,HIGH);`
  `digitalWrite(d,HIGH);`
  `digitalWrite(e,LOW);`
  `digitalWrite(f,LOW);`
  `digitalWrite(g,HIGH);`
  `digitalWrite(dp,LOW);`
`}`
`void digital_4(void) //Segment display digital 4`
`{`
  `digitalWrite(a,LOW);`
  `digitalWrite(b,HIGH);`
  `digitalWrite(c,HIGH);`
  `digitalWrite(d,LOW);`
  `digitalWrite(e,LOW);`
  `digitalWrite(f,HIGH);`
  `digitalWrite(g,HIGH);`
  `digitalWrite(dp,LOW);`
`}`
`void digital_5(void) //Segment display digital 5`
`{`
  `digitalWrite(a,HIGH);`
  `digitalWrite(b,LOW);`
  `digitalWrite(c,HIGH);`
  `digitalWrite(d,HIGH);`
  `digitalWrite(e,LOW);`
  `digitalWrite(f,HIGH);`
  `digitalWrite(g,HIGH);`
  `digitalWrite(dp,LOW);`
`}`
`void digital_6(void) //Segment display digital 6`
`{`
  `digitalWrite(a,HIGH);`
  `digitalWrite(b,LOW);`  
  `digitalWrite(c,HIGH);`
  `digitalWrite(d,HIGH);`
  `digitalWrite(e,HIGH);`
  `digitalWrite(f,HIGH);`
  `digitalWrite(g,HIGH);`
  `digitalWrite(dp,LOW);`
`}`
`void digital_7(void) //Segment display digital 7`
`{`
  `digitalWrite(a,HIGH);`
  `digitalWrite(b,HIGH);`  
  `digitalWrite(c,HIGH);`  
  `digitalWrite(d,LOW);` 
  `digitalWrite(e,LOW);`
  `digitalWrite(f,LOW);`
  `digitalWrite(g,LOW);`
  `digitalWrite(dp,LOW);`
`}`
`void digital_8(void) //Segment display digital 8`
`{`
  `digitalWrite(a,HIGH);`
  `digitalWrite(b,HIGH);`
  `digitalWrite(c,HIGH);`
  `digitalWrite(d,HIGH);`
  `digitalWrite(e,HIGH);`
  `digitalWrite(f,HIGH);`
  `digitalWrite(g,HIGH);`
  `digitalWrite(dp,LOW);`
`}`
`void digital_9(void) //Segment display digital 9`
`{`
  `digitalWrite(a,HIGH);`
  `digitalWrite(b,HIGH);`
  `digitalWrite(c,HIGH);`
  `digitalWrite(d,HIGH);`
  `digitalWrite(e,LOW);`
  `digitalWrite(f,HIGH);`
  `digitalWrite(g,HIGH);`
  `digitalWrite(dp,LOW);`
`}`
`void setup()`
`{`
  `int i;             //Define variables`
  
  `Serial.begin(9600);  // Start serial communication at 9600 baud rate`

  `for(i=4;i<=11;i++)`
  `pinMode(i,OUTPUT); //Set up 4 ~ 11 pins to output mode`
`}`
`void loop()`
`{`
  `if (Serial.available()) {         // Check if there is serial data available`
    `char command = Serial.read();   // Read the command`
    `switch (command) {`
      `case '0':`
        `digital_0(); //Segment display digital 0`
        `break;`
      `case '1':`
        `digital_1(); //Segment display digital 0`
        `break;`
      `case '2':`
        `digital_2(); //Segment display digital 0`
        `break;`
      `case '3':`
        `digital_3(); //Segment display digital 0`
        `break;`
      `case '4':`
        `digital_4(); //Segment display digital 0`
        `break;`
      `case '5':`
        `digital_5(); //Segment display digital 0`
        `break;`
      `case '6':`
        `digital_6(); //Segment display digital 0`
        `break;`
      `case '7':`
        `digital_7(); //Segment display digital 0`
        `break;`
      `case '8':`
        `digital_8(); //Segment display digital 0`
        `break;`
      `case '9':`
        `digital_9(); //Segment display digital 0`
        `break;`
    `}`
  `}`

`}`


Then we need to write a kernel driver which reads the device file and based on the input, we write to serial port. As mentioned above if we write device char driver as 1 then we send "1" to the serial port, which then should glow the led in arduino.

***Kernel device driver code:***

`#include <linux/module.h>`
`#include <linux/fs.h>`
`#include <linux/uaccess.h>`
`#include <linux/tty.h>`
`#include <linux/slab.h>`

`#define SERIAL_DEVICE "/dev/ttyACM0"  // Path to the Arduino device file (changes based on your system)`

`static struct file *serial_file = NULL;`

`//It initiates a serial connection to the serial device ttyACM0`
`static int serial_open(void) {`

    `// Open the serial port`
    `serial_file = filp_open(SERIAL_DEVICE, O_RDWR | O_NOCTTY, 0);`

    `if (IS_ERR(serial_file)) {`
        `printk(KERN_ALERT "Failed to open the serial device\n");`
        `return PTR_ERR(serial_file);`
    `}`
    `return 0;`
`}`

`//It closes the serial connection at the end.`
`static void serial_close(void) {`
    `if (serial_file) {`
        `filp_close(serial_file, NULL);`
    `}`
`}`

`//serial_write is used to send a data to the serial interface port device, which was initiated.`  
`static ssize_t serial_write(const char *data, size_t size) {`
    
    `ssize_t ret;`

    `if (!serial_file) {`
        `printk(KERN_ALERT "Serial device not opened\n");`
        `return -EIO;`
    `}`

    `// Write the command to the serial device`
    `ret = vfs_write(serial_file, data, size, &serial_file->f_pos);`

    `return ret;`
`}`

`//simple function which calls serial write whenever the device file has a change.`
`static ssize_t blink_led(const char *buffer, size_t len) {`
    `char command = buffer[0];`
    
    `serial_write(command, 1);`
    `/*`
    `if (command == '1') {`
        `serial_write("1", 1);  // Send '1' to Arduino (LED ON)`
    `} else if (command == '0') {`
        `serial_write("0", 1);  // Send '0' to Arduino (LED OFF)`
    `}*/`

    `return len;`
`}`

`//Below two are initialization and exit functions called.`
`static int __init led_driver_init(void) {`
    `printk(KERN_INFO "LED Driver Initialized\n");`

    `if (serial_open()) {`
        `printk(KERN_ALERT "Failed to open serial device\n");`
        `return -1;`
    `}`

    `return 0;`
`}`

`static void __exit led_driver_exit(void) {`
    `serial_close();`
    `printk(KERN_INFO "LED Driver Exited\n");`
`}`

`module_init(led_driver_init);`
`module_exit(led_driver_exit);`

`MODULE_LICENSE("GPL");`
`MODULE_AUTHOR("Susil Ramarao");`
`MODULE_DESCRIPTION("A simple Linux driver to control an Arduino 7-segment display");`

