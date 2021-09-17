---
title: Try get Hercules work with Simulink Continue
date: 2021-01-17 02:37:30
tags: Exo Skeleton, FreeRTOS
categories:
- [Exo Skeleton]
---

#### To figure out

- [x] Why it doesn't work previously
  - Only Digital Read and write through pin GIOB_4 and GIOB_6 is supported![image-20201205162257701](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/image-20201205162257701.png)
  - For CAN communication, it's not enough
    ![image-20201205162523032](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/image-20201205162523032.png)
  - ==Wonder how to generate similar block for CAN bus==

- [ ] How builder/launcher/etc methods work

  USB -> Emulation & Serial port for communication with Simulink
  Builder using builder, specify the toolchain

  - Connectivity configuration

  In order to make the Simulink communicate with target, need to read/write with rtiostream.

  

### Pre-Requisite

- [ ] Cortex-R4F MCUs
  ![image-20201205150154490](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/image-20201205150154490.png)



## Changes

### Builder

#### Compare `TI_HERCULES_RM57Lx_toolchain.m` and `HerculesCan\Debug\makefile` (`makefile`, first part of the builder)

- [ ] Linker

  ```makefile
  # Makefile
  HerculusCan.out: $(OBJS) $(CMD_SRCS) $(GEN_CMDS)
  	@echo 'Building target: "$@"'
  	@echo 'Invoking: ARM Linker'
  	"C:/ti/ccs1011/ccs/tools/compiler/ti-cgt-arm_20.2.1.LTS/bin/armcl" -mv7R5 --code_state=32 --float_support=VFPv3D16 -me -g --diag_warning=225 --diag_wrap=off --display_error_number --enum_type=packed --abi=eabi -z -m"HerculusCan.map" --heap_size=0x800 --stack_size=0x800 -i"C:/ti/ccs1011/ccs/tools/compiler/ti-cgt-arm_20.2.1.LTS/lib" -i"C:/ti/ccs1011/ccs/tools/compiler/ti-cgt-arm_20.2.1.LTS/include" --reread_libs --diag_wrap=off --display_error_number --warn_sections --xml_link_info="HerculusCan_linkInfo.xml" --rom_model -o "HerculusCan.out" $(ORDERED_OBJS)
  	@echo 'Finished building target: "$@"'
  	@echo ' '
  ```

  ```matlab
  linkerOpts = { ...
      '-z ' ...
  	'-m"TI_HerculesPilSerial.map" ' ...
  	'-i"$(TI_LIB)" ' ...
  	'-i"$(TI_INCLUDE)" ' ...
  	'--reread_libs ' ...
  	'--warn_sections ' ...
  	'--display_error_number ' ...
  	'--diag_wrap=off ' ...
  	'--rom_model ' ...
  	'--stack=0 ' ...
  	'"$(TARGET_SOURCE)\halcogen\source\sys_link.cmd"'};
  ```

  - [x] `-mv7R4` to `-mv7R5`  
  - [ ] `ProjectName.map` change the project name
  - [x] `linker.Libraries = { '"$(TI_LIB)\rtsv7R4_T_le_v3D16_eabi.lib"' };`, confirmed the .lib exists in the `ti-cgt-arm-20.2.1.LTS/lib`.

#### Change `TargetApplicationFramework` (second part of the builder)

Main thing is adding HalCoGen sources `addHalCoGenSources` and `addRTIOStreamSources`. The `TargetApplicationFramework` provides code to initialize the target before running the main function. It provides code like set the clock frequency, wait states, interrupts, etc. Also, set up the C runtime environment, which HalCoGen will generate. 

- [x] Replace files and folders under `halcogen` with the latest configuration generated from HalCoGen, make sure the SCI2 is configured. Following is the reason for using SCI2. 

  > Before the board is shipped, the XDS100V2 port1 is configured as JTAG, and port2 is configured as SCI. The CPLD on the board is also programmed to route the JTAG signals to the MCU.

- [x] Update project main function to include correct header. e.g. `gio.h` to `HL_gio.h`

### Launcher

Look at `Launcher.m`, it uses `loadti` which comes with Code Composer as a debug server scripting. 

- [x] Change `tconf`, make it point to new `RM57L8xx.ccxml`. This file can be got from Code Composer `targetConfigs`. 
  ![image-20201205232557405](https://raw.githubusercontent.com/bifeitang/blog-img-hosting-yang/master/article_imgs/image-20201205232557405.png)

  One can test the usability of this file by launch and try connect the the target. 

### Communicator

Mathworks provide `libmwrtiostreamserial.dll` on host side. On the target side, it got the `RTIOStream` implemented in the sources folder.



- [ ] Platform-in-the-loop, Matlab generate binary file => USB to the board. 
  Binary file, stl, open command and run the bin file
- [ ] Embedded version, digital write and read