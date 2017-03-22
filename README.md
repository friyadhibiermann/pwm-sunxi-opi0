# pwm-sunxi-opi0

Loadable Kernel Module to support PWM on Allwinner H3 / H2+ SoC (sun8i / sun8iw7p1). Used in Orange Pi Zero.

Provides access to PWM configuration parameters from userspace. Only exposes PWM0 (PWM1 will be added once H2+ SoC documentation is available). PWM0 is on PA5, which is exposed as UART0_RX (middle pin on the UART header). Remap in FEX.

Once loaded, the following sysfs structure is created:

<pre>
-- pwm-sunxi-opi0
      |
      +----pwm0
               |
               +---run
               +---prescale
               +---entire_cycles
               +---active_cycles
               +---freqperiod
               +---polarity
	</pre>

	
  Only supports PWM0. It's exposed via PA6, which is an Rx pin in the UART header (middle pin).
  
  * run (read / write)
  
        **Allowed values: 0, 1**
	
  To enable / disable PWM0:
  <pre>
  echo 1 > /sys/class/pwm-sunxi-opi0/pwm0/run
  echo 0 > /sys/class/pwm-sunxi-opi0/pwm0/run`
  </pre>
  
  * prescale (read / write)
  
  Divide 24MHz PWM clock by a specified prescaler (in hex, see table below)
  <pre>
  PRESCALE_DIV120  = 0x00,  /* Divide 24mhz clock by 120 */
  PRESCALE_DIV180  = 0x01,
  PRESCALE_DIV240  = 0x02,
  PRESCALE_DIV360  = 0x03,
  PRESCALE_DIV480  = 0x04,
  PRESCALE_INVx05  = 0x05, // Invalid prescaler setting
  PRESCALE_INVx06  = 0x06,
  PRESCALE_INVx07  = 0x07,
  PRESCALE_DIV12k  = 0x08,
  PRESCALE_DIV24k  = 0x09,
  PRESCALE_DIV36k  = 0x0a,
  PRESCALE_DIV48k  = 0x0b,
  PRESCALE_DIV72k  = 0x0c,
  PRESCALE_INVx0d  = 0x0d,
  PRESCALE_INVx0e  = 0x0e,
  PRESCALE_DIV_NO  = 0x0f
  </pre>
  
  * entire_cycles (read / write)
  
      **Allowed values 0..65534**
  
      Specify number of ticks in a complete PWM cycle
  
   * active_cycles (r/w)
   
      **Allowed values 0..65535**
  
      Specify number of active ticks in a PWM cycle
  
  * freqperiod (read only)
    
    Show a calculated frequency of the PWM cycle (accounting for PWM clock frequency and period)
    
  * polarity (read / write)
      
      **Allowed values: 0, 1**
      
      Specify polarity of the duty cycle (positive / negative)
      
