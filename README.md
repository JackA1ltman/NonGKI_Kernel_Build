# Non-GKI Kernel with KSU and SUSFS
![GitHub branch check runs](https://img.shields.io/github/check-runs/JackA1ltman/NonGKI_Kernel_Build/main)![GitHub Downloads (all assets, latest release)](https://img.shields.io/github/downloads/JackA1ltman/NonGKI_Kernel_Build/latest/total)  
[支持列表](Supported_Devices.md) | 中文文档 | [English](README_EN.md)  

**Non-GKI**：我们常说的Non-GKI包括了GKI1.0（内核版本4.19-5.4）（5.4为QGKI）和真正Non-GKI（内核版本≤4.14）  

由于Non-GKI内核存在严重的碎片化，不仅仅体现在内核无法通用，更是存在编译环境参差不齐，包括但不限于系统版本，GCC版本，Clang版本等等，因此决定开始自动化编译Non-GKI内核项目  
本项目欢迎Fork后自行编辑使用，也欢迎增加修改后提交合并，或者成为合作伙伴  

# 使用例
## Profiles/设备代号_ROM名称.env
总共由以下内容组成：  
**Profiles/polaris_pe_a13.env** - 用来表明在Action环境中具体配置文件位置  

**LG G8 ThinQ** - 设备全称，格式：设备品牌_型号_地区  
**alphalm** - 设备代号  

**aarch64-linux-gnu-** - 通常用于指明所用编译器/备用编译器  
**ld.lld LLVM=1 LLVM_IAS=1** - 通常用于编译器所需的自定义参数  

**https://github.com/anht3889/kernel_lge_sm8150_new** - 内核源码所在之处  
**KERNEL_BRANCH** - 内核源码所需分支  

**CLANG_SOURCE** - Clang所在之处，但支持git、tar.gz、tar.xz  
**CLANG_BRANCH** - Clang所需分支，但前提是git  

**false** - 若你的内核需要GCC，但不需要自定义GCC，可通过选项启用系统提供的GNU-GCC，true或false  
**GCC_XX_SOURCE** - GCC所在之处，但支持git、tar.gz、zip  
**GCC_XX_BRANCH** - GCC所需分支，但前提是git  

**DEFCONFIG_SOURCE** - 若有自定义DEFCONFIG文件需求可提供具体文件所在地址  
**sm8150_defconfig** - 不管是否自定义，都需要提供用于编译的必要DEFCONFIG文件，通常格式为：设备_defconfig、vendor/设备_defconfig  

**https://raw.githubusercontent.com/KernelSU-Next/KernelSU-Next/refs/heads/next/kernel/setup.sh** - 你可以自行设定KernelSU的来源，通常情况下是setup.sh。但有需求可启用手动安装的方式，此时则为git  
**NEXT** - 提供KernelSU的所属分支  
**KernelSU-Next** - 部分KernelSU分支存在不同的名称，所以你需要填写正确名称，默认为KernelSU  

**false** - 是否在编译时启用SUSFS，true或false  
**false** - 是否启用SUSFS错误修补，一般用于内核修补时产生错误后，二次补充修补  

**AK3_SOURCE** - Anykernel3所在之处，若需要的话，仅支持git  
**AK3_BRANCH** - Anykernel3所需分支  

**BOOT_SOURCE** - 若你已经启用MKBOOTIMG的方式，要填写原始干净内核的地址，仅限img格式  

**LXC_ENABLE** - (实验性⚠)启用自动化内核LXC/Docker支持，true或false  

**HAVE_NO_DTBO** - (实验性⚠)若你的内核没有提供dtbo.img，且你的设备属于A/B分区且存在dtbo分区，则可启用本选项(true)，默认为false  

**ROM_TEXT** - 用于编译成功后用于上传文件标题，声明内核可用的ROM  

## .github/workflow/build_kernel_设备简称_型号_ROM_Android版本.yml
我们编写了env和用于编译的yml的例本，接下来是对yml例本的解析  
这里仅指出大概可供修改的地方，具体可按需求修改，我们不建议过度修改步骤和顺序  
这是我们提供的示例文件：**codename_rom_template.env**和**build_kernel_template.yml**  

- **env:** - 设置必要修改的变量，独立于Profiles
  - **PACK_METHOD** - 打包方式，分为MKBOOTIMG，和[Anykernel3](https://github.com/osm0sis/AnyKernel3)，默认为Anykernel3
  - **KERNELSU_METHOD** - 嵌入KernelSU的方式：
    - 通常情况下使用**shell**方式即可
    - 但如果你提供了非setup.sh的方式，或者该方式报错，请将其修改**manual**，manual虽然是手动安装，但实际上并不需要维护者修改任何内容
    - 若你的内核已经存在KernelSU，但你想要替换，可使用**only**，仅执行git不执行修补
  - **PATCHES_SOURCE** - 使用susfs不可避免需要手动修补，这是用来填写你存放patch的github项目地址，当然如果你不采用susfs，则不需要填写，可参考我的用于Patch的git项目
  - **PATCHES_BRANCH** - patch项目所需的分支，一般为main
  - **HOOK_METHOD** - 我们提供了两种方式用于KernelSU手动修补：
    - **normal**代表最常见的修补方式，一般不会出问题
    - [vfs](https://github.com/backslashxx/KernelSU/issues/5)是最新的最小化修补方式，似乎会提高隐藏，但是在低版本clang下可能会有ISO编译规范问题，且对于版本≤4.9的内核的支持存在问题，仅更高版本内核建议启用
  - **PROFILE_NAME** - 填写成你修改好的env环境变量文件的名称，例如codename_rom_template.env
  - **KERNELSU_SUS_PATCH** - 如果你的KernelSU不属于KernelSU-Next，并且也没有针对SuSFS的修补分支，可以启用该项目（true），但我们不建议这么做，因为分支KernelSU的魔改情况严重，手动修补已经不能顺应现在的时代了

- **runs-on: ubuntu-XX.XX** 
  - 不同内核所需系统不同，默认为22.04，我们预先提供了两套包安装选项（适配22.04和24.04），我们通过检测系统版本进行决定包安装

- **Set Compile Environment**
  - 这里分为无GCC和有GCC，Clang也有区分判定，请继续往下看
  - 若无GCC，则会自动选择仅Clang，而通常情况下，仅Clang可用于使用antman进行管理的Clang，这些步骤我们都已经可以自动识别，因此不需要修改yml来实现
  - 若有GCC，则需填写GCC 64位和32位的版本，对于GCC我们建议git形式，但同时支持tar.gz和zip
  - 根据本人的使用情况，我们对于Clang支持为git、tar.gz、tar.xz、zip以及上述提到的antman管理软件

- **Set Pack Method and KernelSU and SUSFS**
  - 我们默认提供Anykernel3和MKBOOTIMG两种打包方式，其中AK3可以自动检测内核源码中是否存在，若不存在则调用env提供的SOURCE和BRANCH，对于AK3仅提供git方式，MKBOOTIMG由我们默认提供，一般不需要自行获取
  - **Anykernel3** 需要提供对应项目的地址和分支，且仅支持git方式，或者使用我们提供的默认方式，一般不会出错
  - **MKBOOTIMG** 需要提供干净的原始内核镜像文件，我们建议使用Github raw地址

- **Extra Kernel Options** 
  - 有些内核编译时需要提供更多设置项
  - 通常为针对defconfig文件的补充项，但的确，有些完善的内核其实并不需要额外的设置项，不需要就把该模块中所有内容注释掉即可跳过

- **Added mkdtboimg to kernel (Experiment)** 
  - 如你所见，很多内核，或者说大部分内核都不需要该功能，有些内核例如nameless虽然没有dtbo,但实际上他的确不需要。而且仅限A/B分区的设备，且存在危险性，例如加上dtbo反而无法启动设备等等，三思而后行

- **Setup LXC (Experiment)** 
  - 自动部署LXC，但许多内核并不支持该方式，可用于Fork后自行尝试，在本人的官方编译中应该不会选择支持LXC
  
- **Patch Kernel**
  - 分为两个部分，主要的SUSFS修补和补充修补（Patch Kernel of SUSFS 和 Fixed Kernel Patch）
  - 一切基于env中SUSFS_ENABLE和env.SUSFS_FIXED为true，但不一定都为true
  - SUSFS修补大概率会产生问题，因此通常情况下需要补充修补
  - 补充修补需要执行你重新制作的patch补丁（步骤为：Fixed Kernel Patch）
  
最后提醒⚠️：非上述提示的步骤理论上不需要你做任何修改，我已经尽可能实现多情况判定
