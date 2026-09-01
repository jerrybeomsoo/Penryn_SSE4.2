# Penryn_SSE4.2
Proof-of-concept UEFI VMX Hypervisor for running Windows 11 24H2+ on Penryn CPUs without SSE4.2 and POPCNT instructions.

> [!WARNING]
> Provided as-is. The full sourcecode is currently not disclosed to the public.

## Status : <br>
Currently can boot Windows 11 25H2 into the "desktop" on a Intel Core 2 Duo T9900 machine, with CPU core count restricted to 1.<br>
The performance is very sluggish, and several applications does not even run properly. (e.g. Task Manager crashing on launch)<br>
Currently, the debug screen (White and Cyan numbers) is always shown on the display.<br>

## Usage : <br>
0. Prepare an disk by installing the Windows 11 24H2+ on it and connect it to Windows PC.<br>
   Assign a letter to the EFI partition of the disk you prepared, <br>
   and run the following commands on Powershell (with Administrator rights). <br>
   The "NNN" on the first command should be changed to a number of the disk shown on "diskpart" or "diskmgmt.msc". <br>
   
```
Add-PartitionAccessPath -DiskNumber NNN -PartitionNumber 1 -AccessPath "S:\" -ErrorAction SilentlyContinue
$bcd = "S:\EFI\Microsoft\Boot\BCD"
bcdedit /store $bcd /enum "{default}"
bcdedit /store $bcd /set "{default}" numproc 1
bcdedit /store $bcd /set "{default}" recoveryenabled No
bcdedit /store $bcd /set "{default}" bootstatuspolicy IgnoreAllFailures
bcdedit /store $bcd /enum "{default}"
```

1. Flash an USB drive with Clover EFI Bootloader. <br>
   I've used "BootDiskUtility v2.1.023" and "CloverISO-4961.tar.lzma". <br>
   
2. Place "hypervisor.efi" on the USB Drive. <br>
   If the PC supports UEFI, then copy the file to "/EFI/CLOVER/drivers64UEFI" directory. <br>
   If not (legacy boot only), then copy the file to "/EFI/CLOVER/drivers64" directory. <br>

3. Boot the PC with Clover EFI Bootloader, select "Microsoft EFI" entry on the Clover EFI. <br>

## Credits : 
https://github.com/tianocore/edk2
