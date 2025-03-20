**Only Chinese:**  
| 设备名称 | 设备代号 | 内核/作者/名称 | 系统 | Android | 打包方式 | KernelSU | SuSFS | LXC | VFS Hook | 维护状态 |  
|----------|----------|----------|----------|----------|----------|----------|----------|----------|----------|----------|  
| 一加 8 (CN) | instantnoodle | 4.19/ppajda/XTD | OxygenOS/ColorOS 13.1 | 13 | AnyKernel3 | Magic | ✅ | ❌ | ❌ | Stable |  
| 一加 8 (All) | instantnoodle | 4.19/Nameless/Nameless | Nameless 15 | 15 | AnyKernel3 | Next | ✅ | ❌ | ❌ | Suspend |  
| 小米 MIX2S (CN) | polaris | 4.9/EvoX/EvoX | Evolution X 10.2.1 | 15 | Boot Image | Next | ✅ | ❌ | ❌ | Stable |  
| 红米 K20 Pro (CN) | raphael | 4.14/SOVIET-ANDROID/SOVIET-STAR-OSS | Based-AOSP | 15 | AnyKernel3 | Next | ✅ | ❌ | ❌ | Stable |  
| 红米 Note 4X (CN) | mido | 4.9/RaidenShogunSeggs/(Nothing) | Based-AOSP | 13 | AnyKernel3 | Next | ✅ | ❌ | ❌ | Suspend |  
| 黑鲨4 | penrose | 4.19/DtHnAme/(Nothing) | MIUI/JoyUI 12 | 11 | AnyKernel3 | Next | ❌ | ❌ | ✅ | Stable |  
| 小米 10S | thyme | 4.19/TIMISONG-dev/MagicTime | Based-AOSP | 15 | AnyKernel3 | Next | ✅ | ❌ | ❌ | Stable |  
| 中兴 A201ZT | a201zt | 4.19/官方内核 | 未知 | Null | AnyKernel3 | Next | ❌ | ❌ | ❌ | Suspend |  
| 三星 S20-5G | x1q | 4.19/官方内核 | Oneui 5.1/6.1 | 13/14 | Boot Image | KernelSU | ❌ | ❌ | ❌ | Suspend |  

特别说明：
  - 我们提供的KernelSU分支包括：[Next](https://github.com/KernelSU-Next/KernelSU-Next)、[Magic](https://github.com/backslashxx/KernelSU)、[rsuntk](https://github.com/rsuntk/KernelSU)、[lightsummer233](https://github.com/lightsummer233/KernelSU)、[酷友二创-mksu-skn](https://github.com/ShirkNeko/KernelSU)
  - 打包方式：Anykernel3请在Recovery下刷入，Boot Image请在Recovery/Fastboot下选择刷入Boot分区
  - 部分机型由于内核问题将暂停（Suspend）维护，但仍可通过Action的方式Fork后自行编译
  - All-代表该机型所有地区可用，CN-代表国区机型可用，其他同理
  - 一加8 OxygenOS/ColorOS 13.1 经测试8、8t、8Pro、9r都可用，且该内核类原生设备同样可用（但会有某些Bugs）
  - 红米 Note 4X 通常仅高通可用，联发科设备不支持
  - 黑鲨4 因机型内核缺陷（缺少ANDROID_KABI）无法修补SuSFS，为了提高隐藏性和安全性，因此是首款将常规手动Hook切换至[VFS Hook](https://github.com/backslashxx/KernelSU/issues/5)的设备
  - 一加8 Nameless 15 存在WiFi失效的问题，请谨慎刷入
  - 中兴 Z201ZT 由于源代码并非Git方式获得，因此修改了yml文件中对应的获取方式，由于存在较多未知信息，因此该内核仅研究学习使用，若有需要可自行Fork编译
