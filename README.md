# Reusable Rocketry at Purdue Programming Manifesto

**The Entry Point for Your Programming Career**

<p align="center"><img src="resources/RRaPTransCropped.png" width="300" height="300"></p>

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
- Type “sudo chmod +x **NAME OF FILE**” and hit enter
- Type in your password, then open the file with “./**NAME OF FILE**”

## Basic Programming Standards

Basic programming standards will be followed for the programming of systems. To begin, all functions must begin with the function header template. This is as follows:

```C
/********************************
*
* Function Name:
*
* Programmer’s Name & Email:
*
*********************************/
```

This template will allow us to determine who to credit the work to during code reviews, making review sessions easier.

## Use of Defined Constants

The use of literal constants is not allowed unless when completely necessary. The *only* case where a literal constant is to be used is when the name of the constant would be the number itself. This is due to the fact that microcontroller code should be **as readable as possible** to a user looking at it. Take the following two cases:

```C
//IMPROPER DEFINITIONS
int foo()
{
    u_int32_t registerVal;
    registerVal = 1 << 8;

    return(registerVal);
}
```

```C
//PROPER DEFINITIONS
#define BIT_ON                  1
#define SENSOR_OFFSET           8

int foo()
{
    u_int32_t registerVal;
    registerVal = BIT_ON << SENSOR_OFFSET;

    return(registerVal);
}
```

While the first example seems too simple to define the values of 1 and 8, the second example shows the meaning those values have. Even if the value defined is only used once, it will help improve the readability of the code, speed the code review/merging process, and will help cut down on errors in the long run. ***THIS CONCEPT IS EXTREMELY IMPORTANT FOR CAN IDS!***

## The Use of CAN IDs in Software

While on the topic of defining all values, CAN IDs, as stated above, are very important to use this concept with. There is a master list of CAN IDs found [here](). When using a CAN ID in software, it must match the list of CAN IDs found at the link. If an ID needs to be changed for any reason, or if one needs to be added, you must consult the other members of the software team before doing so. ***CHANGING CAN IDs REQUIRES DOING IT ON EVERY PIECE OF HARDWARE! KEEP CHANGES TO A MINIMUM!*** While this is the case, proper definition of CAN IDs will help aid this process. Here are two examples that show examples of good and bad useage.

```C
//PROPER DEFINITIONS
#define ACC_ROLL_CAN_ID             0x6c5
#define ACC_PITCH_CAN_ID            0x6c7
#define TEMP_MAIN_CAN_ID            0x7d5
#define FORCE_NOSE_CAN_ID           0x7c6

void can_rx(struct canFrame)
{
    switch(canFrame.ID)
    {
        case(ACC_ROLL_CAN_ID):
            rollDataHandler();
            break;

        case(ACC_PITCH_CAN_ID):
            pitchDataHandler();
            break;   
    }

    return;
}
```

```C
//IMPROPER DEFINITIONS
void can_rx(struct canFrame)
{
    switch(canFrame.ID)
    {
        case(0x6c5):
            rollDataHandler();
            break;

        case(0x6c7):
            pitchDataHandler();
            break;   
    }

    return;
}
```

While both statements do the same thing, a person reading the second will have a hard time following what is happening. They must reference the CAN ID list every time an ID occurs to tell what the program is doing. This could lead to errors in the review process, as well as errors in coding. During the review process, files that do not define CAN IDs properly/change CAN IDs without proper notice will be removed during the review process.

## Variable Naming Convention

The use of descriptive variable names is very important for our use. Variables should have names that are more than 15 characters long are more overwhelming than descriptive. We would like to keep names as short as possible while not compromising code readability/understanding. Also, variables assigned to a certain sensor/micro-controller/section of a board should be included in a structure if necessary. In this way, we can easily group data that has a similar characteristic. This can also help us properly scope data while keeping the memory safe.

Take the following examples:

```C
//IMPROPER NAMING USED
int foo()
{
    int x;
    int y;
    int z;
    float value;

    x = HAL_GPIO_Getvalue(&adc1);
    VTaskDelay(100);
    y = HAL_GPIO_Getvalue(&adc2);
    VTaskDelay(100);
    z = HAL_GPIO_Getvalue(&adc1);

    value = (x + y + z) / 3;

    return value;
}
```

```C
//PROPER NAMING USED
#define TEMP_SENSOR_COUNT           3.0

int foo()
{
    int noseTemperature;
    int exhaustTemperature;
    int chamberTemperature;
    float mean;

    noseTemperature = HAL_GPIO_Getvalue(&adc1);
    VTaskDelay(100);
    exhaustTemperature = HAL_GPIO_Getvalue(&adc2);
    VTaskDelay(100);
    chamberTemperature = HAL_GPIO_Getvalue(&adc1);

    mean= (noseTemperature + exhaustTemperature + chamberTemperature) / TEMP_SENSOR_COUNT;

    return mean;
}
```

The first example doesn't show that we are reading temperature data, nor does it show that `adc1` is responsible for the nose and chamber temperatures, while `adc2` is responsible for the exhaust temperature. Good variable naming conventions are fundamental to having successful code, and it is required that all code have meaningful variable names.

## STM32CubeMX Generated Code

Editing `STM32CubeMX` generated code inside of the IDE is not acceptable. If an individual makes changes to the code without changing the values in the software, perfectly good code could become useless due to a slight imperfection with the `Cube` settings. Any changes to `RTOS`, `pinout`, or `clock values` must occur inside `STM32CubeMX` to ensure all changes are properly tracked and committed for ***ALL*** members to use.