build PixelOS-16.1 for nuwa  

Build Logs  

1.  
error: build/soong/fsgen/Android.bp:32:1: module "custom_nuwa_generated_init_boot_image" variant "android_arm64_armv8-2a-dotprod_kryo"  
 (created by module "soong_filesystem_creator" variant "android_common"): header_version: must be set  
17:29:25 soong bootstrap failed with: exit status 1  

fix:  
device/xiaomi/sm8550-common/BoardConfigCommon.mk  
add # Init Boot  
BOARD_INIT_BOOT_HEADER_VERSION := 4  
BOARD_MKBOOTIMG_INIT_ARGS += --header_version $(BOARD_INIT_BOOT_HEADER_VERSION)  

-----------------------------------------------------------------------------------------------------------------------  

2.  
error: vendor/xiaomi/nuwa/Android.bp:9068:1: "camera.xiaomi" depends on undefined module "libui-v34".  
Or did you mean ["libriscv64" "libui" "libutils-v32" "libutils-v33"]?  

fix:  
we can keep it by add in vendor/xiaomi/nuwa/Android.bp  
soong_namespace {  
    imports: [  
        "hardware/lineage/compat",  
    ],  
}  

-----------------------------------------------------------------------------------------------------------------------  

3.  
error: vendor/qcom/opensource/commonsys-intf/display/aidl/composer3/Android.bp:1:1: module "vendor.qti.hardware.display.composer3-V5-rust" variant   "android_arm64_armv8-2a-dotprod_kryo_rlib_rlib-std" (created by module "vendor.qti.hardware.display.composer3_interface"): Dependency path:  
           via tag rust.dependencyTag: { name:rlibTag library:true procMacro:false dynamic:false}  
    -> android.hardware.graphics.composer3-V4-rust{os:android,arch:arm64_armv8-2a-dotprod_kryo,rust_libraries:rlib,rust_stdlinkage:rlib-std}  
           via tag rust.dependencyTag: { name:rlibTag library:true procMacro:false dynamic:false}  
    -> android.hardware.graphics.common-V7-rust{os:android,arch:arm64_armv8-2a-dotprod_kryo,rust_libraries:rlib,rust_stdlinkage:rlib-std}  
           via tag struct { blueprint.DependencyTag }: {DependencyTag:<nil>}  
    -> android.hardware.graphics.common-V7-rust-source{}  
error: vendor/qcom/opensource/commonsys-intf/display/aidl/composer3/Android.bp:1:1: module "vendor.qti.hardware.display.composer3-V5-rust" variant   "android_arm64_armv8-2a-dotprod_kryo_rlib_rlib-std" (created by module "vendor.qti.hardware.display.composer3_interface"): Dependency path:  
           via tag rust.dependencyTag: { name:rlibTag library:true procMacro:false dynamic:false}  
    -> android.hardware.graphics.common-V6-rust{os:android,arch:arm64_armv8-2a-dotprod_kryo,rust_libraries:rlib,rust_stdlinkage:rlib-std}  
           via tag rust.dependencyTag: { name:source library:false procMacro:false dynamic:false}  
    -> android.hardware.graphics.common-V6-rust{os:android,arch:arm64_armv8-2a-dotprod_kryo,rust_libraries:source}  
           via tag struct { blueprint.DependencyTag }: {DependencyTag:<nil>}  
    -> android.hardware.graphics.common-V6-rust-source{}  
error: vendor/qcom/opensource/commonsys-intf/display/aidl/composer3/Android.bp:1:1: module "vendor.qti.hardware.display.composer3-V5-rust" variant   "android_arm64_armv8-2a-dotprod_kryo_rlib_rlib-std" (created by module "vendor.qti.hardware.display.composer3_interface"): Dependency path:  
           via tag rust.dependencyTag: { name:rlibTag library:true procMacro:false dynamic:false}  
    -> android.hardware.graphics.common-V6-rust{os:android,arch:arm64_armv8-2a-dotprod_kryo,rust_libraries:rlib,rust_stdlinkage:rlib-std}  
           via tag struct { blueprint.DependencyTag }: {DependencyTag:<nil>}  
    -> android.hardware.graphics.common-V6-rust-source{}  
17:21:52 soong bootstrap failed with: exit status 1  

fix:  
vendor/qcom/opensource/commonsys-intf/display/aidl/composer3/Android.bp  
line 17  
change "android.hardware.graphics.common-V6" to "android.hardware.graphics.common-V7"  
line 38  
change "android.hardware.graphics.common-V6" to "android.hardware.graphics.common-V7"  

-----------------------------------------------------------------------------------------------------------------------  

4.  
error: vendor/xiaomi/nuwa/Android.bp:9924:1: "libcamxcommonutils" depends on undefined module "libprocessgroup_shim".  
Module "libcamxcommonutils" is defined in namespace "vendor/xiaomi/nuwa" which can read these 7 namespaces: ["vendor/xiaomi/nuwa" "device/xiaomi/  sm8550-common" "hardware/qcom-caf/sm8550" "hardware/xiaomi" "vendor/qcom/opensource/commonsys-intf/display" "vendor/xiaomi/sm8550-common" "."]  
Module "libprocessgroup_shim" can be found in these namespaces: ["hardware/lineage/compat"]  
Or did you mean ["libprocessgroup_util"]?  

fix:  
soong_namespace {  
    imports: [   
        "hardware/lineage/compat",  
    ],  
}  

-----------------------------------------------------------------------------------------------------------------------  

5. 
error: hardware/lineage/compat/Android.bp:417:1: module "libui-v34" variant "android_vendor_arm64_armv8-2a-dotprod_kryo_static": Dependency path:  
           via tag cc.libraryDependencyTag: { Kind:sharedLibraryDependency Order:normalLibraryDependency wholeStatic:false reexportFlags:false   explicitlyVersioned:false explicitlyImpl:false dataLib:false ndk:false staticUnwinder:false makeSuffix: skipApexAllowedDependenciesCheck:false excludeInApex:false excludeInNonApex:false unexportedSymbols:false}  
    -> android.hardware.graphics.allocator-V2-ndk{os:android,image:vendor,arch:arm64_armv8-2a-dotprod_kryo,link:shared}  
           via tag cc.dependencyTag: { name:reuse objects}  
    -> android.hardware.graphics.allocator-V2-ndk{os:android,image:vendor,arch:arm64_armv8-2a-dotprod_kryo,link:static}  
           via tag cc.libraryDependencyTag: { Kind:sharedLibraryDependency Order:normalLibraryDependency wholeStatic:false reexportFlags:true  explicitlyVersioned:false explicitlyImpl:false dataLib:false ndk:false staticUnwinder:false makeSuffix: skipApexAllowedDependenciesCheck:false   excludeInApex:false excludeInNonApex:false unexportedSymbols:false}  
    -> android.hardware.graphics.common-V7-ndk{os:android,image:vendor,arch:arm64_armv8-2a-dotprod_kryo,link:shared}  
           via tag cc.dependencyTag: { name:gen header export}  
    -> android.hardware.graphics.common-V7-ndk-source{}  

fix:  
hardware/lineage/compat/Android.bp:417:1: "libui-v34"  
change "android.hardware.graphics.common-V6-ndk" to "android.hardware.graphics.common-V7-ndk"  

-----------------------------------------------------------------------------------------------------------------------  

6.  
FAILED: out/soong/.intermediates/system/sepolicy/vendor_sepolicy.cil/android_common/nuwa/vendor_sepolicy.cil  
out/host/linux-x86/bin/version_policy -b out/soong/.intermediates/system/sepolicy/pub_policy.cil/android_common/nuwa/pub_policy.cil -n 202504 -o out/soong/.intermediates/system/sepolicy/vendor_sepolicy.cil/  android_common/nuwa/vendor_sepolicy.cil -t out/soong/.intermediates/system/sepolicy/vendor_sepolicy.cil.raw/android_common/nuwa/vendor_sepolicy.cil.raw && out/host/linux-x86/bin/build_sepolicy filter_out -f out/  soong/.intermediates/system/sepolicy/plat_pub_versioned.cil/android_common/nuwa/plat_pub_versioned.cil -t out/soong/.intermediates/system/sepolicy/vendor_sepolicy.cil/android_common/nuwa/vendor_sepolicy.cil && out/  host/linux-x86/bin/secilc -v -m -M true -G -N -c 30 out/soong/.intermediates/system/sepolicy/plat_sepolicy.cil/android_common/plat_sepolicy.cil out/soong/.intermediates/system/sepolicy/system_ext_sepolicy.cil/  android_common/nuwa/system_ext_sepolicy.cil out/soong/.intermediates/system/sepolicy/product_sepolicy.cil/android_common/nuwa/product_sepolicy.cil out/soong/.intermediates/system/sepolicy/plat_pub_versioned.cil/  android_common/nuwa/plat_pub_versioned.cil out/soong/.intermediates/system/sepolicy/plat_mapping_file/android_common/202504.cil out/soong/.intermediates/system/sepolicy/vendor_sepolicy.cil/android_common/nuwa/  vendor_sepolicy.cil -o /dev/null -f /dev/null # hash of input list: cd4a62464ae2c591e1ab59abefe400127f511200fdc155917459d6f17a51ad6e  
Found conflicting genfscon rules  
  at out/soong/.intermediates/system/sepolicy/plat_sepolicy.cil/android_common/plat_sepolicy.cil:158  
  at out/soong/.intermediates/system/sepolicy/vendor_sepolicy.cil/android_common/nuwa/vendor_sepolicy.cil:3  
Problems processing genfscon rules  
Failed post db handling  
Post process failed  
Failed to compile cildb: -1  

fix:  
device\qcom\sepolicy_vndr\sm8550\generic\vendor\common/genfs_contexts  
line66: # genfscon proc /sys/kernel/firmware_config             u:object_r:vendor_proc_firmware_cfg:s0  

7.  
error: vendor/xiaomi/sm8550-common/Android.bp:15167:1: "libdlbpreg" depends on undefined module "libstagefright_foundation-v33".  
Module "libdlbpreg" is defined in namespace "vendor/xiaomi/sm8550-common" which can read these 8 namespaces: ["vendor/xiaomi/sm8550-common" "device/xiaomi/sm8550-common" "hardware/qcom-caf/wlan" "hardware/qcom-caf/sm8550" "hardware/xiaomi" "vendor/qcom/opensource/commonsys-intf/display" "vendor/qcom/opensource/dataservices" "."]  
Module "libstagefright_foundation-v33" can be found in these namespaces: ["hardware/lineage/compat"]  
Or did you mean ["libstagefright_foundation"]?

-----------------------------------------------------------------------------------------------------------------------  

fix:  
fix:  
soong_namespace {  
    imports: [   
        "hardware/lineage/compat",  
    ],  
} 







