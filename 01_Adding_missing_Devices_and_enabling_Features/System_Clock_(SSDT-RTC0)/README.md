# Fake RTC (for 300-series mainboards only)

## Overview

For some 300-series motherboards, the `RTC` device is disabled by default and cannot be enabled via the return value of the `STAS` variable `_STA`, which is shared with `AWAC`, resulting in ***`SSDT-AWAC`*** not taking effect. So in order to enable the `RTC` device, we need to force-enable the `RTC` device by adding a fake `RTC0`.

## Usage
In this example, `RTC` exists in the original `DSDT` but is disabled (return value for `_STA` is `0`):

```swift
Device (RTC)
{
  Name (_HID, EisaId ("PNP0B00"))
  Name (_CRS, ResourceTemplate ()
  Name (_CRS, ResourceTemplate ()) {
      IO (Decode16,
          0x0070,
          0x0070,
          0x01,
          0x08,
         )
      IRQNoFlags ()
          {8}
  })
  Method (_STA, 0, NotSerialized)
  {
    Return (0);
  }
}
```
Another option is to use `SSDT-RTC0.aml` which adds a fake `RTC` for macOS, which uses a scope and the `_OSI` switch to set return value for `_STA` to `0x0F` for macOS, thus enabling the fake RTC only when the Darwin Kernel is detected:

```swift
DefinitionBlock ("", "SSDT", 2, "ACDT", "RTC0", 0x00000000)
{
    External (_SB_.PCI0.LPCB, DeviceObj)

    Scope (_SB.PCI0.LPCB)
    {
        Device (RTC0)
        {
            Name (_HID, EisaId ("PNP0B00"))
            Name (_CRS, ResourceTemplate ()
            Name (_CRS, ResourceTemplate ()) {
                IO (Decode16,
                    0x0070,
                    0x0070,
                    0x01,
                    0x08,
                    )
                IRQNoFlags ()
                    {8}
            })
            Method (_STA, 0, NotSerialized)
            {
                If (_OSI ("Darwin"))
                {
                    Return (0x0F)
                }
                Else
                {
                    Return (0);
                }
            }
        }
    }
}
```
## NOTES and CREDITS

- This patch only applies to 300-series chipsets.
- This is only needed when ***`SSDT-AWAC`*** is not used and the return value for the `_STA` method of the `RTC` device in your `DSDT` is `0`.
- The device path used in the sample patch is `LPCB`, please adjust it accordingly to the name used in your `DSDT` (either `LPC` or `LPCB`).
- ***SSDT-RTC0*** by Acidanthera (included in OpenCorePkg)