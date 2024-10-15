https://github.com/SusilRamarao/Ubuntu-Arduino-Kernel-Driver-Communication 

My Youtube video : [Writing Linux Kernel Driver To Communicate With Arduino (youtube.com)](https://www.youtube.com/watch?v=MYD0Eqn61QA)

***Overall flow:***
We need to set an serial interface. The Kernel driver should initiate a character device file, read and then send it to the serial port.

***Arduino code:***
`#define LED 13`
`void setup() {`
  `pinMode(LED, OUTPUT);`
  `Serial.begin(9600);  // Start serial communication at 9600 baud rate`
`}`

`void loop() {`
  
  `if (Serial.available()) {         // Check if there is serial data available`
    `char command = Serial.read();   // Read the command`
    `if (command == '1') {           // Command to turn the LED ON`
      `digitalWrite(LED, HIGH);   // Turn the LED on`
    `}` 
    `else if (command == '0') {      // Command to turn the LED OFF`
      `digitalWrite(LED, LOW);    // Turn the LED off`
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

`#define SERIAL_DEVICE "/dev/ttyACM0"  // Please find the appropriate driver via arduino ide.

`static struct file *serial_file = NULL;`

`static int serial_open(void) {`

    `// Open the serial port`
    `serial_file = filp_open(SERIAL_DEVICE, O_RDWR | O_NOCTTY, 0);`

    `if (IS_ERR(serial_file)) {`
        `printk(KERN_ALERT "Failed to open the serial device\n");`
        `return PTR_ERR(serial_file);`
    `}`
    `return 0;`
`}`

`static void serial_close(void) {`
    `if (serial_file) {`
        `filp_close(serial_file, NULL);`
    `}`
`}`

`static ssize_t serial_write(const char *data, size_t size) {`
    `mm_segment_t oldfs;`
    `ssize_t ret;`

    `if (!serial_file) {`
        `printk(KERN_ALERT "Serial device not opened\n");`
        `return -EIO;`
    `}`

    `// Write the command to the serial device`
    `ret = vfs_write(serial_file, data, size, &serial_file->f_pos);`

    `return ret;`
`}`

`static ssize_t blink_led(const char *buffer, size_t len) {`
    `char command = buffer[0];`
    
    `if (command == '1') {`
        `serial_write("1", 1);  // Send '1' to Arduino (LED ON)`
    `} else if (command == '0') {`
        `serial_write("0", 1);  // Send '0' to Arduino (LED OFF)`
    `}`

    `return len;`
`}`

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
`MODULE_AUTHOR("Your Name");`
`MODULE_DESCRIPTION("A simple Linux driver to control an Arduino LED via USB");`

