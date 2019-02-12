# Reusable Rocketry at Purdue Programming Manifesto
**The Entry Point for Your Programming Career**

![Logo](resources/RRaPTrans.png)

## Basic Information
RRaP plans to use the `STM32` family of microcontrollers to control multiple facets of electronics technology. There are two main reasons for this. For one thing, ECE classes (specifically CompE) at Purdue University will use the `STM32F4 Discovery boards` for programming knowledge. By using similar microcontrollers for development, we increase the chances of an individual in the program knowing how to code for our system. Also, the `STM32` family is industry standard. Many automotive manufacturers and businesses use them for day to day electronics. These chips are much faster than arduino `ATMega` based systems as clock speeds can reach 100 Mhz. Therefore, these chips are the best possible option for rocketry design. For data communications, the `CAN` bus will be used. The `CAN` bus is easy to implement, easy to control, has been industry standard since 2003, and is very efficient at data transfer. For this reason, the `CAN` bus will be used in conjunction with the `STM32` family to design PCB boards for use in rocketry systems.

## Required Software

There are two pieces of required software for STM programming. 

**STM32CubeMX**: “Cube”, as it is referred to, is an easy to use software generation tool that allows the user to generate code that will setup and run the `STM32` chips. It allows for visual customization of GPIO input, GPIO output, ADC, I2C, and CAN pins, along with many other types. It also allows configuration of clock speeds and RTOS setup. 

Press [here](https://www.st.com/en/development-tools/stm32cubemx.html) to download the software. Keep in mind, this will require a login. 

The manual for the software can be found [here](https://www.st.com/content/ccc/resource/technical/document/user_manual/10/c5/1a/43/3a/70/43/7d/DM00104712.pdf/files/DM00104712.pdf/jcr:content/translations/en.DM00104712.pdf). This document should be referenced for general information, as well as troubleshooting.

---

**System Workbench**: System workbench is build on top of the Eclipse IDE. While it is not the best, it does all makefile generation for building in the background and includes the ST-LINK drivers, GDB, and other required files for debugging and uploading .elf files to the boards. In the future, we hope to implement these features as a subset of Visual Studio Code, but for now, we must use this. 

Press [here](https://www.st.com/en/development-tools/sw4stm32.html) to download the software. Keep in mind, this will require a login.

For Mac users, double clicking on the setup will not work. You must follow these instructions:

- Make sure Java is installed and updated
- Right click the setup/control click the setup and select show “package contents”
- Go into “Contents”, then “MacOs”
- Double click the unix executable file. It might also be a .jar file.
- If this doesn’t work, use the terminal to navigate to the file
- Type “sudo chmod +x **NAME OF FILE***” and hit enter
- Type in your password, then open the file with “./**NAME OF FILE**”


If this doesn’t work, try this:
- Open terminal
- Use cd **DIRECTORY** to navigate to the setup file
- Type “sudo chmod +x **NAME OF FILE***” and hit enter
- Type in your password, then open the file with “./**NAME OF FILE**”

## Basic Programming Standards
	
Basic programming standards will be followed for the programming of systems. To begin, all functions must begin with the function header template. This is as follows:

```C
/********************************
* 
* Function Name:
*
* Return Type:
*
* Parameters:
* 1.
* 2.
* 3.
*
* Programmer’s Name & Email:
*
*********************************/
```

This template will allow us to determine who to credit the work to during code reviews, making review sessions easier.

