ARCHS = arm64 arm64e
TARGET = iphone:clang:latest:15.0
THEOS_PACKAGE_SCHEME = rootless
INSTALL_TARGET_PROCESSES = WeChat

include $(THEOS)/makefiles/common.mk

TWEAK_NAME = WeChatAntiRecall
WeChatAntiRecall_FILES = Tweak.xm
WeChatAntiRecall_CFLAGS = -fobjc-arc
WeChatAntiRecall_FRAMEWORKS = Foundation UIKit
WeChatAntiRecall_PLIST = WeChatAntiRecall.plist

include $(THEOS_MAKE_PATH)/tweak.mkPackage: com.local.wechat-antirecall
Name: WeChatAntiRecall-iOS
Version: 0.1.1
Architecture: iphoneos-arm64
Description: Rootless iOS jailbreak WeChat feature scaffold; hooks not yet implemented
Maintainer: Local Developer
Author: Local Developer
Section: Tweaks
Depends: mobilesubstrate{
    Filter = {
        Bundles = (
            "com.tencent.xin"
        );
    };
}#import <Foundation/Foundation.h>
#import <UIKit/UIKit.h>

// Rootless-compatible diagnostic scaffold only.
// Target: iOS 17+, WeChat 8.0.79, Dopamine rootless.
// No private class names, selectors, or offsets are guessed.

%ctor {
    @autoreleasepool {
        NSLog(@"[WeChatAntiRecall-iOS] rootless scaffold loaded; no WeChat methods hooked");
    }
}

// Planned modules:
// 1. One-tap forwarding: reuse the verified native forwarding flow.
// 2. Anti-recall: map the target build's revoke handler before adding a hook.
// 3. Recall history: opt-in, local-only, bounded storage with clear controls.
// 4. Red-packet helper: detect and notify; manual claim entry first.
//
// Do not reuse patch addresses from macOS WeChat builds.
// Keep all chat content local; no telemetry or upload code is included.# WeChatAntiRecall iOS Theos — Rootless scaffold

## Target configuration
- Dopamine 2.x (rootless)
- iOS 17+
- WeChat 8.0.79
- arm64 / arm64e build targets
- Package architecture: iphoneos-arm64

## Status

This is an engineering scaffold, not a feature-complete tweak.
The constructor only logs that the tweak loaded.

## Rootless build

On a Mac with Theos and Xcode command-line tools installed:

    make clean
    make package

THEOS_PACKAGE_SCHEME = rootless asks Theos to use the
rootless install prefix (/var/jb), rootless library paths,
rpaths, and package architecture.

## Injection scope

WeChatAntiRecall.plist limits injection to bundle identifier
com.tencent.xin. Confirm the installed app's actual bundle
identifier on the target device before testing.

## Implementation sequence

1. Build and install the diagnostic package on a test device.
2. Verify the native message action menu and forwarding workflow.
3. Analyze the target build's revoke-event path.
4. Add a local, opt-in recall-history store.
5. Add feature toggles, bounded storage, crash checks,
   and uninstall/rollback instructions.

## Privacy and testing

- Test on a spare device/account.
- Do not reuse macOS patch offsets for iOS.
- Do not guess private selectors or fixed offsets.
- Keep chat records local and opt-in.
- Automated red-packet claiming is not implemented..theos/
.packages/
packages/
.deb
.dylib
*.DS_StoreWeChatAntiRecall/
├── Makefile
├── control
├── Tweak.xm
├── WeChatAntiRecall.plist
├── README.md
└── .gitignoremake clean
make package