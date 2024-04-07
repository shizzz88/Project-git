# Petalinux create project
  1) Go to the installed petalinux folder where the file 'settings.sh' is located and write the following command in order the set the environment varialble to petalinux:

       ``
      source settings.sh
      ``

  2) create an empty project by writing the following command:

     ``
     petalinux-create -t project --template zynqMP --name <project-name>
     ``

3) configure the petalinux hw settings by using the following command:

    ``
    petalinux-config --get-hw-description <path to the .xsa file>
    ``

![PMOD Pinouts](KV260_PMOD_Pintouts.png] 


# Petaliunx GPIOs

GPIOs in petalinux can be found in the following location:

``
/sys/class/gpio
``
you need to export the GPIO number in order to use it:

``
echo **GPIO-number** > /sys/class/gpio/export
``

To change the dircection to output, use the following command:

``
echo out > /sys/class/gpio/gpio-number/direction
``

To write the value on the output pin, use the following command:

``
echo 1 > /sys/class/gpio/gpio-number/value
``

To read the GPIO value:

``
cat /sys/class/gpio/gpio-number/value
``


## References

Petalinux GPIOS: http://en.ica123.com/implementing-linux-gpio-input-and-output/

Petalinux UIOs: https://www.hackster.io/Roy_Messinger/gpio-and-petalinux-embedded-linux-yocto-based-c71773


