# Build Vaa3D on Mac

## Step1

Download qt6.  You can choose to download using the online installer. 

https://download.qt.io/archive/online_installers/. 

## Step2

Use qt creator to open the v3d_qt6.pro. This file is in v3d_external/v3d_main/v3d.

## Step3

Change the address of the generated project according to your own needs and build it. If successful, the app will be generated.

# Build plugins on Mac

If you just want to build a specific plugin, you can open the .pro file of the plugin with qtcreator and click on build. If you want to build all the plugins in vaa3d_tools. You can run build_plugins.sh to do the build. The script file is in the vaa3d_tools/released_plugins folder. 

##### (Note that due to the specific nature of the mac, you may be required to make allowable security settings before this script can be run.)

# Packing Vaa3D on Mac

## Step1

Move teem and TIFF dynamic libraries to v3d_qt6.app/content/MacOS directory, and change the address of the dynamic library. You need to use the following instructions in the packaging folder directory.

**$PATH is v3d_ external address**

```
install_name_tool -change "$PATH/v3d_external/v3d_main/common_lib/src_packages/teem/bin/libteem.1.dylib" "@executable_path/libteem.1.dylib" v3d_qt6.app/Contents/MacOS/v3d_qt6

install_name_tool -change "$PATH/v3d_external/v3d_main/common_lib/src_packages/teem/bin/libtiff.5.dylib" "@executable_path/libtiff.5.dylib" v3d_qt6.app/Contents/MacOS/v3d_qt6

```

## Step2

Use macdeployqt to package the v3d_app. You need to use the following instructions in the packaging folder directory.

```
 macdeployqt ./v3d_qt6.app
```

## Step3

You can use Apple's own disk utility to pack it into dmg format.