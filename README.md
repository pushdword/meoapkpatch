# meoapkpatch

release apk ready to run: https://github.com/pushdword/meoapkpatch/releases/tag/release-main-4.8.0

note: you have to install it via ADB. First enable USB debug and you can install over the network via ./adb connect IP; ./adb install -r file.apk

## decompile

use apktool to decompile
apktool d meo.apk

## apply patch

read the patch file and change yourself or apply it using patch
patch -p1 < meopatch.patch

## build using apktool

apktool b -f -v meo -o meo-patched.apk

## align in 4-byte (required for newer API levels and SDKs)

zipalign -f -p 4 meo-patched.apk meo-patched-zipalign.apk
(exists on android SDK)

## sign using apksigner (jarsign doesn't work with 4-byte alignment)

apksigner sign --verbose --ks meopatch.keystore meo-patched-zipalign.apk
-> input your keystore password to unlock and uses the pki
-> you can create a keystore using keytool (found on JDK bin folder):
-> keytool -genkey -v -keystore meopatch.keystore -alias meopatch -keyalg RSA -keysize 2048 -validity 10000

(exists on android SDK)
meo-patched-zipalign.apk is not signed.

install meo-patched-zipalign.apk via adb (enable USB debug via developer mode - tap 10x build name on settings -, find the IP address, ./adb connect IP, ./adb install -r meo-patched-zipalign.apk, disable USB debug and develop mode)
