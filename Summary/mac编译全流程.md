# Build Vaa3D on Mac

## Step1



## Step2



## Step3

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