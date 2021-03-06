Let's say you want to write a new OpenVR driver called "myhmd". To do that you will need to following the steps below.

1. Add a new directory called "<steam install dir>/SteamApps/common/openvr/drivers/myhmd"
2. Add a new DLL (or dylib or so) to "drivers/myhmd/bin/win32/driver_myhmd.dll"
3. Implement the [driver factory function](https://github.com/ValveSoftware/openvr/wiki/Driver-Factory-Fuction) in that DLL.
4. Add an implementation of [`vr::IClientTrackedDeviceProvider`](https://github.com/ValveSoftware/openvr/wiki/IClientTrackedDeviceProvider_Overview) to the DLL and return it from the factory. This interface is used in vrclient.dll for HMD presence checks and various other client-side operations.
5. Add an implementation of [`vr::IServerTrackedDeviceProvider`](https://github.com/ValveSoftware/openvr/wiki/IServerTrackedDeviceProvider_Overview) and have that return implementations of [`vr::ITrackedDeviceServerDriver`](https://github.com/ValveSoftware/openvr/wiki/vr::ITrackedDeviceServerDriver-Overview) for each tracked device.

If you want your new driver to work from 64 bit apps, you will also need a 64-bit DLL in "drivers/myhmd/bin/win64/driver_myhmd.dll" that implements all the same things. Same thing for other platforms. 

**WARNING:** The driver interface is still under active development and vrserver does not guarantee backward compatibility with old versions of the interface. At this point don't ship driver binaries and expect them to continue to work going forward.