# Kext Loading Sequence Examples

This Chapter contains a collection of `config.plist` examples to demonstrate the loading sequences of certain kexts and family of kexts. In contrast to Clover, where you just have to add the required kexts to `Clover\kexts\other` and you're done, OpenCore loads Kexts in the exact same order they are listed in the "Kernel > Add" Section of the `config.plist`.  

Basically, Kexts which provide additional functionality other kexts rely on, have to be loaded first. Config 1 contains the loading sequence for the most essential Kexts that are required by almost every Hackintosh to boot. These are:

1. Lilu
2. VirtualSMC (+ Sensor Plugins)
3. Whatevergreen

The rest of the config examples show the loading sequences for `Bluetooth`, `Wifi`, `Keyboards` and `Trackpad` kexts because theses have addition kexts nested inside of them and which have to be loaded in the correct order to work correctly. Not having them in the right order may cause Kernel Panics. So does having a Kext in the list which isn't present in the "OC > Kexts" Folder but is enabled in the config.plist. So it's of utmost importance that the Kexts are loaded in the correct order and that the content of the config.plist reflects what's inside the OC Folder 1:1. The examples provided should provide you a good guideline. 

## Note
The kexts listed in Config 2 and 5 are both for PS2 Controllers (Keyboards, Mice, Trackpads). Don't use both at the same time. We recommend to use `config-2-PS2-Controller`list.
