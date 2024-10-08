---
title: 树莓派接收MTI数据
img: https://cdn.pixabay.com/photo/2022/03/28/18/41/spring-7098130_960_720.jpg
excerpt: 在树莓派中安装ubuntu系统和ros系统来采集MTI的数据，在windows下利用MTI的官方软件来接收数据
top: false
toc: true
tocOpen: true
onlyTitle: false
comments: true
share: true
copyright: true
donate: false
categories: 计算机
tags: [树莓派,MTI,Ubuntu,Ros]
mathjax: true
date: 2022-04-18 20:41:54
---

# 树莓派接收传感器MTI-G-710数据

## 一、树莓派系统安装

### 1.系统选择

官方系统下载：https://www.raspberrypi.org/software/operating-systems/

官方OS中分别有桌面和一些常用软件的版本，只有桌面的版本，没有桌面的Lite版本，所以只要安装第二个只带桌面版就可以了，树莓派的官方OS自带ros系统，安装方式与其他系统不一样，比较麻烦，所以我们安装第三方系统。

Ubuntu的第三方系统：https://ubuntu.com/download/raspberry-pi

Ubuntu mate的第三方系统:https://ubuntu-mate.org/download/

Ubuntu的老版本系统：https://old-releases.ubuntu.com/releases/18.04.4/

由于我们的树莓派版本是3B+，在Ubuntu系统中，建议安装18.04的版本，ros系统安装Melodic版本，与MTI的SDK文档要求的ros系统版本一致。为了减轻树莓派负担，从Ubuntu的老版本系统选择`ubuntu-18.04.4-preinstalled-server-arm64+raspi3.img.xz`文件下载，解压。选择arm64的版本原因：arm64可以修改内部的网络配置文件，armhf的不行。

### 2.系统安装

由于下载的好的系统文件是img文件，不同于iso文件，需要特别的软件写入读卡器，软件名为Win32DiskImager，可以上网搜索安装下来使用，还有格式化系统所需要的配套软件SD Card Formatter。打开system-boot盘，找到network-config文件，修改后面的文件。

```
wifis:
  wlan0:
    dhcp4: true
    dhcp6: true
    optional: true
    access-points:
      "填上你的WIFI名称":
        password: "WIFI密码"
```

Ubuntu系统的默认用户是ubuntu，密码是ubuntu。树莓派官方系统的默认用户pi，密码raspberry。在文件盘符新建文件ssh无后缀，用手机开热点让树莓派连接，查看到IP后，之后再用远程软件登陆（putty或者Xshell）。树莓派连接不上wifi，就断开电源，重新启动一下。

对Ubuntu系统换源（把默认的国外下载源换成国内下载源），用下面命令打开文件：

```
$ sudo nano /etc/apt/sources.list
```

arm64与armhf的源不一样，注意区分，下面是arm64的国内清华源：

```
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic main multiverse restricted universe
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-security main multiverse restricted universe
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-updates main multiverse restricted universe
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-backports main multiverse restricted universe
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic main multiverse restricted universe
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-security main multiverse restricted universe
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-updates main multiverse restricted universe
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-backports main multiverse restricted universe
```
把`sources.list`里面存在的文件用#号注释掉，添加上面的文件。

`ctrl+o`写入，回车， `ctrl+x`离开。

> 源文件内容描述

- ubuntu-ports:表示arm版本的源

- bionic:是ubuntu18.04的发行版本

- security：仅修复漏洞，并且尽可能少的改变软件包的行为。低风险。

- backports：backports的团队则认为最好的更新策略是security 策略加上新版本的软件（包括候选版本的）。但不会由Ubuntu security team审查和更新。

- updates：修复严重但不影响系统安全运行的漏洞，这类补丁在经过QA人员记录和验证后才提供，和security那类一样低风险。

- proposed：update类的测试部分，仅建议提供测试和反馈的人进行安装。
- main：完全的自由软件。
- muitiverse：非自由软件，完全不提供支持和补丁。
- restricted：不完全的自由软件。
- universe：ubuntu官方不提供支持与补丁，全靠社区支持。



## 二、xsens官方驱动使用

### 1.官方软件下载和介绍

Linux xsens驱动地址：https://content.xsens.com/download-mt-software-suite

电脑选择linux版本下载好之后得到文件`MT_Software_Suite_linux-x64_2020.0.2.tar.gz`，解压后，可以得到文件`mtsdk_linux-x64_2020.0.2.sh`，利用命令从本地上传到树莓派服务器。

树莓派安装lrzsz

```
$ sudo apt install lrzsz
```

输入rz上传文件到服务器当前目录

```
$ rz
```

输入sz下载文件到本地

```
$ sz ./****.txt
```

把文件下载到用户目录下后安装mti的sdk

```
$ sudo ./mtsdk_linux-x64_2020.0.2.sh
```

可能会提示确实某某文件，安装文件之后再运行，如果点运行报错，可能是没有运行权限，可以增加权限或者使用bash/sh命令

```
$ sudo bash ./mtsdk_linux-x64_2020.0.2.sh
```

默认的安装位置`[/usr/local/xsens]`，可以选择更改到用户目录下`/home/ubuntu/xsens`



### 2.驱动文件使用介绍

驱动文件就是`mtsdk_linux-x64_2020.0.2.sh`文件，由于官方软件无法在树莓派中适配，所以我们要在官方驱动中使用现成的API，具体可以参考这个文档：https://base.xsens.com/knowledgebase/s/article/Interfacing-MTi-devices-with-the-Raspberry-Pi

官方问答区：https://base.xsens.com/knowledgebase/s/

我们进入到`xsens`文件目录下，可以看到文件

- doc 	 
- lib	
- python	
- examples	
- MTSDK.README	
- `xsens_ros_mti_driver`	
- include
- public        
- Xsens Technologies MT SS License Agreement.rtf

我们进入`example`文件夹中，再进入`mtsdk`文件夹中，可以看到文件

- awindamonitor_cpp	
- Makefile	
- xda_public_cpp	
- embedded_examples	
- xda_cpp	
- xda_python

然后进入到`xda_public_cpp`文件夹中，可以看到文件

- example_mti_parse_logfile.cpp	
- Makefile	
- xspublic	
- example_mti_receive_data.cpp   
- xda_public_cpp.sln

`xspublic`文件包含很多需要使用的前置文件，`example_mti_receive_data.cpp`是接收数据的，可以自动检测是否已经连接MTI传感器， 收集数据产生`logfile.mtb`文件，`example_mti_parse_logfile.cpp`可以把数据文件`logfile.mtb`数据提取转化为`exportfile.txt`文件，每一行为一次收集的数据。

在使用前需要对cpp文件进行编译，第一次可能时间会长一点，在当前目录下，打开终端输入，必须安装ros系统才能编译

```
$ sudo make
```

产生一系列`.o`文件和无后缀文件，那些无后缀就是可执行文件，在终端输入

```
$ sudo ./example_mti_receive_data
```

开始接收数据并会输出到终端中，如果无法检测到MTI传感器，在终端输入命令查到MTI的串口位置并提高串口权限

```
$ dmesg | grep tty
[    0.570241] printk: console [tty0] enabled
[    2.799819] 00:05: ttyS0 at I/O 0x3f8 (irq = 4, base_baud = 115200) is a 16550A
[ 5373.377442] usb 2-2.1: xsens_mt converter now attached to ttyUSB0
$ sudo chmod 777 /dev/ttyUSB0
```

可以看到`xsens_mt`的位置在 `ttyUSB0`，所以需要提升`ttyUSB0`的权限

产生`logfile.mtb`文件文件后，在终端输入

```
$ sudo ./example_mti_parse_logfile
```

`产生exportfile.txt`文件



### 3.程序更改部分

跳转到用户目录下的`xda_public_cpp`文件夹中，打开`example_mti_receive_data.cpp`文件，可以看到

```c++
	// Important for Public XDA!
	// Call this function if you want to record a mtb file:
	device->readEmtsAndDeviceConfiguration();

	XsOutputConfigurationArray configArray;
	configArray.push_back(XsOutputConfiguration(XDI_PacketCounter, 0));
	configArray.push_back(XsOutputConfiguration(XDI_SampleTimeFine, 0));
	if (device->deviceId().isImu())
	{
		configArray.push_back(XsOutputConfiguration(XDI_Acceleration, 0));
		configArray.push_back(XsOutputConfiguration(XDI_RateOfTurn, 0));
		configArray.push_back(XsOutputConfiguration(XDI_MagneticField, 0));
	}
	else if (device->deviceId().isVru() || device->deviceId().isAhrs())
	{
		configArray.push_back(XsOutputConfiguration(XDI_Quaternion, 0));
	}
	else if (device->deviceId().isGnss())
	{
		configArray.push_back(XsOutputConfiguration(XDI_Quaternion, 0));
		configArray.push_back(XsOutputConfiguration(XDI_LatLon, 0));
		configArray.push_back(XsOutputConfiguration(XDI_AltitudeEllipsoid, 0));
		configArray.push_back(XsOutputConfiguration(XDI_VelocityXYZ, 0));
	}
	else
	{
		return handleError("Unknown device while configuring. Aborting.");
	}
```

可以看到根据传感器的型号来取什么值并不能取到全部的值，改为下面可以采取各个值，

```c++
  // Important for Public XDA!
	// Call this function if you want to record a mtb file:
	device->readEmtsAndDeviceConfiguration();

	XsOutputConfigurationArray configArray;
	//TimeStamp
	configArray.push_back(XsOutputConfiguration(XDI_PacketCounter, 0));
	configArray.push_back(XsOutputConfiguration(XDI_SampleTimeFine, 0));
  configArray.push_back(XsOutputConfiguration(XDI_SampleTimeCoarse, 0));
	configArray.push_back(XsOutputConfiguration(XDI_UtcTime, 0));
	//Quaternion
	configArray.push_back(XsOutputConfiguration(XDI_Quaternion, 0));
	configArray.push_back(XsOutputConfiguration(XDI_RotationMatrix, 0));
	configArray.push_back(XsOutputConfiguration(XDI_EulerAngles, 0));
	//Inertial Data
	configArray.push_back(XsOutputConfiguration(XDI_Acceleration, 0));
	configArray.push_back(XsOutputConfiguration(XDI_FreeAcceleration, 0));
	configArray.push_back(XsOutputConfiguration(XDI_RateOfTurn, 0));
	//Magnetic Field
	configArray.push_back(XsOutputConfiguration(XDI_MagneticField, 0));
	//Temperature
	configArray.push_back(XsOutputConfiguration(XDI_Temperature, 0));
	//Pressure
	configArray.push_back(XsOutputConfiguration(XDI_BaroPressure, 0));
	//High-Rate Data
	configArray.push_back(XsOutputConfiguration(XDI_AccelerationHR, 0));
	configArray.push_back(XsOutputConfiguration(XDI_RateOfTurnHR, 0));
	//Status
	configArray.push_back(XsOutputConfiguration(XDI_StatusWord, 0));
	configArray.push_back(XsOutputConfiguration(XDI_StatusByte, 0));
	//Position and Velocity
	configArray.push_back(XsOutputConfiguration(XDI_LatLon, 0));
	configArray.push_back(XsOutputConfiguration(XDI_AltitudeEllipsoid, 0));
	configArray.push_back(XsOutputConfiguration(XDI_VelocityXYZ, 0));
	//GNSS Data
	configArray.push_back(XsOutputConfiguration(XDI_GnssPvtData, 0));
	configArray.push_back(XsOutputConfiguration(XDI_GnssSatInfo, 0));
```

根据定义，可以看到各个参数为枚举类型，枚举类定义文档如下：

```c++
//AUTO namespace xstypes {
/*!	\enum XsDataIdentifier
	\brief Defines the data identifiers

	The list of standard data identifiers is shown below.
	The last positions in the data identifier depends on the configuration of the data.
	For example 0x4020 is 3D acceleration in single precision float format,
	where 0x4022 is 3D acceleration in fixed point 16.32 format.

	Refer to the Low Level Communication Protocol for more details.
*/
enum XsDataIdentifier
{
	XDI_None					= 0x0000,	//!< Empty datatype
	XDI_TypeMask				= 0xFE00,	//!< Mask for checking the group which a dataidentifier belongs to, Eg. XDI_TimestampGroup or XDI_OrientationGroup
	XDI_FullTypeMask			= 0xFFF0,	//!< Mask to get the type of data, without the data format
	XDI_FullMask				= 0xFFFF,	//!< Complete mask to get entire data identifier
	XDI_FormatMask				= 0x01FF,	//!< Mask for getting the data id without checking the group
	XDI_DataFormatMask			= 0x000F,	//!< Mask for extracting just the data format /sa XDI_SubFormat

	XDI_SubFormatMask			= 0x0003,	//!< Determines, float, fp12.20, fp16.32, double output... (where applicable)
	XDI_SubFormatFloat			= 0x0000,	//!< Floating point format
	XDI_SubFormatFp1220			= 0x0001,	//!< Fixed point 12.20
	XDI_SubFormatFp1632			= 0x0002,	//!< Fixed point 16.32
	XDI_SubFormatDouble			= 0x0003,	//!< Double format

	XDI_SubFormatLeft			= 0x0000,	//!< Left side data (ie XDI_GloveData for the left hand)
	XDI_SubFormatRight			= 0x0001,	//!< Right side data (ie XDI_GloveData for the right hand)

	XDI_TemperatureGroup		= 0x0800,	//!< Group for temperature outputs
	XDI_Temperature				= 0x0810,	//!< Temperature

	XDI_TimestampGroup			= 0x1000,	//!< Group for time stamp related outputs
	XDI_UtcTime					= 0x1010,	//!< Utc time from the GNSS receiver
	XDI_PacketCounter			= 0x1020,	//!< Packet counter, increments every packet
	XDI_Itow					= 0x1030,	//!< Itow. Time Of Week from the GNSS receiver
	XDI_GnssAge					= 0x1040,	//!< Gnss age from the GNSS receiver
	XDI_PressureAge				= 0x1050,	//!< Age of a pressure sample, in packet counts
	XDI_SampleTimeFine			= 0x1060,	//!< Sample Time Fine
	XDI_SampleTimeCoarse		= 0x1070,	//!< Sample Time Coarse
	XDI_FrameRange				= 0x1080,	//!< Reserved \internal add for MTw (if needed)
	XDI_PacketCounter8			= 0x1090,	//!< 8 bit packet counter, wraps at 256
	XDI_SampleTime64			= 0x10A0,	//!< 64 bit sample time

	XDI_OrientationGroup		= 0x2000,	//!< Group for orientation related outputs
	XDI_CoordSysMask			= 0x000C,	//!< Mask for the coordinate system part of the orientation data identifier
	XDI_CoordSysEnu				= 0x0000,	//!< East North Up orientation output
	XDI_CoordSysNed				= 0x0004,	//!< North East Down orientation output
	XDI_CoordSysNwu				= 0x0008,	//!< North West Up orientation output
	XDI_Quaternion				= 0x2010,	//!< Orientation in quaternion format
	XDI_RotationMatrix			= 0x2020,	//!< Orientation in rotation matrix format
	XDI_EulerAngles				= 0x2030,	//!< Orientation in euler angles format

	XDI_PressureGroup			= 0x3000,	//!< Group for pressure related outputs
	XDI_BaroPressure			= 0x3010,	//!< Pressure output recorded from the barometer

	XDI_AccelerationGroup		= 0x4000,	//!< Group for acceleration related outputs
	XDI_DeltaV					= 0x4010,	//!< DeltaV SDI data output
	XDI_Acceleration			= 0x4020,	//!< Acceleration output in m/s2
	XDI_FreeAcceleration		= 0x4030,	//!< Free acceleration output in m/s2
	XDI_AccelerationHR			= 0x4040,	//!< AccelerationHR output

	XDI_IndicationGroup			= 0x4800,	//!< 0100.1000 -> bit reverse = 0001.0010 -> type 18
	XDI_TriggerIn1				= 0x4810,	//!< Trigger in 1 indication
	XDI_TriggerIn2				= 0x4820,	//!< Trigger in 2 indication
	XDI_TriggerIn3				= 0x4830,	//!< Trigger in 3 indication

	XDI_PositionGroup			= 0x5000,	//!< Group for position related outputs
	XDI_AltitudeMsl				= 0x5010,	//!< Altitude at Mean Sea Level
	XDI_AltitudeEllipsoid		= 0x5020,	//!< Altitude at ellipsoid
	XDI_PositionEcef			= 0x5030,	//!< Position in earth-centered, earth-fixed format
	XDI_LatLon					= 0x5040,	//!< Position in latitude, longitude

	XDI_GnssGroup				= 0x7000,	//!< Group for Gnss related outputs
	XDI_GnssPvtData				= 0x7010,	//!< Gnss position, velocity and time data
	XDI_GnssSatInfo				= 0x7020,	//!< Gnss satellite information
	XDI_GnssPvtPulse			= 0x7030,	//!< Time of the PVT timepulse (4Hz upsampled from the 1PPS)

	XDI_AngularVelocityGroup	= 0x8000,	//!< Group for angular velocity related outputs
	XDI_RateOfTurn				= 0x8020,	//!< Rate of turn data in rad/sec
	XDI_DeltaQ					= 0x8030,	//!< DeltaQ SDI data
	XDI_RateOfTurnHR			= 0x8040,	//!< Rate of turn HR data

	XDI_RawSensorGroup			= 0xA000,	//!< Group for raw sensor data related outputs
	XDI_RawUnsigned				= 0x0000,	//!< Tracker produces unsigned raw values, usually fixed behavior
	XDI_RawSigned				= 0x0001,	//!< Tracker produces signed raw values, usually fixed behavior
	XDI_RawAccGyrMagTemp		= 0xA010,	//!< Raw acceleration, gyroscope, magnetometer and temperature data
	XDI_RawGyroTemp				= 0xA020,	//!< Raw gyroscope and temperature data
	XDI_RawAcc					= 0xA030,	//!< Raw acceleration data
	XDI_RawGyr					= 0xA040,	//!< Raw gyroscope data
	XDI_RawMag					= 0xA050,	//!< Raw magnetometer data
	XDI_RawDeltaQ				= 0xA060,	//!< Raw deltaQ SDI data
	XDI_RawDeltaV				= 0xA070,	//!< Raw deltaV SDI data
	XDI_RawBlob					= 0xA080,	//!< Raw blob data

	XDI_AnalogInGroup			= 0xB000,	//!< Group for analog in related outputs
	XDI_AnalogIn1				= 0xB010,	//!< Data containing adc data from analog in 1 line (if present)
	XDI_AnalogIn2				= 0xB020,	//!< Data containing adc data from analog in 2 line (if present)

	XDI_MagneticGroup			= 0xC000,	//!< Group for magnetometer related outputs
	XDI_MagneticField			= 0xC020,	//!< Magnetic field data in a.u.
	XDI_MagneticFieldCorrected	= 0xC030,	//!< Corrected Magnetic field data in a.u. (ICC result)

	XDI_SnapshotGroup			= 0xC800,	//!< Group for snapshot related outputs
	XDI_RetransmissionMask		= 0x0001,	//!< Mask for the retransmission bit in the snapshot data
	XDI_RetransmissionFlag		= 0x0001,	//!< Bit indicating if the snapshot if from a retransmission
	XDI_AwindaSnapshot			= 0xC810,	//!< Awinda type snapshot
	XDI_FullSnapshot			= 0xC820,	//!< Full snapshot
	XDI_GloveSnapshotLeft		= 0xC830, 	//!< Glove Snapshot for Left Hand
	XDI_GloveSnapshotRight		= 0xC840, 	//!< Glove Snapshot for Right Hand

	// The numbers of the GloveData items should match the GloveSnapshot items, but in the CA range
	XDI_GloveDataGroup			= 0xCA00,	//!< Group for usable glove data
	XDI_GloveDataLeft			= 0xCA30, 	//!< Glove Data for Left Hand
	XDI_GloveDataRight			= 0xCA40, 	//!< Glove Data for Right Hand

	XDI_VelocityGroup			= 0xD000,	//!< Group for velocity related outputs
	XDI_VelocityXYZ				= 0xD010,	//!< Velocity in XYZ coordinate frame

	XDI_StatusGroup				= 0xE000,	//!< Group for status related outputs
	XDI_StatusByte				= 0xE010,	//!< Status byte
	XDI_StatusWord				= 0xE020,	//!< Status word
	XDI_Rssi					= 0xE040,	//!< Rssi information
	XDI_DeviceId				= 0xE080,	//!< DeviceId output
	XDI_LocationId				= 0xE090,	//!< LocationId output
};
/*! @} */
```

根据自己需要采集的数据来设定参数



## 三、ros系统的安装与使用

### 1.环境配置

可以根据官网的教程安装ros系统http://wiki.ros.org/melodic/Installation/Ubuntu

>  配置资源表

```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

> 设置密钥

```
sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
```

>  安装ros基础版没有桌面工具

```
sudo apt update
sudo apt install ros-melodic-ros-base
```

>  环境配置

```
echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

> 初始化ros

```
sudo apt install python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential
sudo rosdep init
rosdep update
```

>  运行ros

```
roscore
```

------

常见错误：

> ERROR: cannot download default sources list from:
>
> https://raw.githubusercontent.com/ros/rosdistro/master/rosdep/sources.list.d/20-default.list
>
> Website may be down.

网速慢，修改hosts，提前设置好IP

```
$ sudo nano /etc/hosts
```

添加以下内容保存即可

```
185.199.108.133 raw.githubusercontent.com
185.199.109.133 raw.githubusercontent.com
185.199.110.133 raw.githubusercontent.com
185.199.111.133 raw.githubusercontent.com
```

> ERROR: default sources list file already exists:
>
> /etc/ros/rosdep/sources.list.d/20-default.list
>
> Please delete if you wish to re-initialize

表示已经存在了文件，删除文件后，重新运行`sudo rosdep init`

```
$ sudo rm /etc/ros/rosdep/sources.list.d/20-default.list
```

------

`rosdep update` 超时，最经常发生的问题

方法一：使用手机热点，多试几次，移动，联通网都试一试

方法二：使用延长加载时间

在下面的文件中，有`DEFAULT_TIME_OUT=15.0`，改为180，再试一次

```
/usr/lib/python2.7/dist-packages/rosdep2/gbpdistro_support.py
/usr/lib/python2.7/dist-packages/rosdep2/sources_list.py 
/usr/lib/python2.7/dist-packages/rosdep2/rep3.py
```

方法三：使用代理

再文件夹`/usr/lib/python2.7/dist-packages/rosdep2/sources_list.py`中，找到下面函数并修改`url`的赋值语句

```python
def download_rosdep_data(url):
  
  try:
    url="https://ghproxy.com/"+url
```

然后在下面文件夹中找到类似这样的全局变量，修改为下面这样

```python
DEFAULT_INDEX_URL = 'https://ghproxy.com/https://raw.githubusercontent.com/ros/rosdistro/master/index-v4.yaml'
```

```
/usr/lib/python2.7/dist-packages/rosdep2/gbpdistro_support.py
/usr/lib/python2.7/dist-packages/rosdep2/sources_list.py 
/usr/lib/python2.7/dist-packages/rosdep2/rep3.py
```

再试一次，应该就可以了

方法四：下载文件到本地

在本地下载好后，修改各个文件，加载目标文件，详情见：https://blog.csdn.net/sinat_25923849/article/details/107976434

///////////////////////////////////////////////////////////////////////////////////////////////////////

​													下面部分，作为研究查看

///////////////////////////////////////////////////////////////////////////////////////////////////////

### 2.连接MTI

我们的MTI型号：`MTi-G-710-2A8G4`，查看xsens_driver可支持ros版本，详情见官网：http://wiki.ros.org/xsens_driver

可以看到xsens_driver只支持melodic和 kinetic的ros系统，ros系统支持的系统见官网：http://wiki.ros.org/ROS/Installation

所以我们选择melodic的版本，要下载xsens_driver需要看到下面的Source对应的链接：https://github.com/ethz-asl/ethzasl_xsens_driver

除了 这个驱动之外，还有一个驱动就在我们下载官方的软件包中，安装完mtsdk后，在xsens文件夹下，可以看到`xsens_ros_mti_driver`

> xsens_driver驱动安装

```
sudo apt-get install ros-melodic-xsens-driver
roscore
rosstack profile
rospack profile
```

> 修改访问MTI设备的权限，ttyUSB0换成你的设备号

```
sudo chmod 777 /dev/ttyUSB0
```

> 启动MTI

```
roslaunch xsens_driver xsens_driver.launch
rostopic list
rostopic echo /imu/data
```

出现数据说明OK了，连接成功

------

> **Tips：**
>
> 所有关于节点的操作都要在运行`xsens_driver.launch`的前提下
>
> ubuntu 查看USB对应的串口:
>
> ```
> dmesg | grep tty
> ```
>
> 第三方插件安装命令
>
> ```
> sudo apt-get install ros-melodic-**
> ```
>
> 可视化查看节点之间的关系
>
> ```
> rqt_graph
> ```
>
> 查看驱动节点的信息
>
> ```
> rosnode info /xsens_driver
> ```
>
> 话题列表
>
> ```
> rostopic list
> ```

### 3.xsens_driver节点简述

MTI数据传输是通过`xsens_driver`这个节点以话题订阅的方式进行的，`xsens_driver`节点不停发布数据所以要想获取数据必须要订阅话题

我们先查看`xsens_driver`节点有用的信息

```
$ rosnode info /xsens_driver

Node [/xsens_driver]
Publications: 
 * /imu/data [sensor_msgs/Imu]
 * /imu_data_str [std_msgs/String]
 * /rosout [rosgraph_msgs/Log]
 * /time_reference [sensor_msgs/TimeReference]

Subscriptions: None

Services: 
 * /xsens_driver/get_loggers
 * /xsens_driver/set_logger_level
```

可以看到它有有4个话题发布，没有订阅，两个服务，通过不同时间不同时候话题发布的也不同。实际话题发布比较少，是因为只有在有要发布的数据时才会公布主题，没有数据就不会发布。接下来，我们可以查看其中一个的数据结构，上面的`/rosout`是ros 系统自带的，不属于它自己的。

> 查看驱动`/xsens_driver`节点发布`/imu/data`话题的消息[sensor_msgs/Imu]结构：

```
$ rosmsg show sensor_msgs/Imu

std_msgs/Header header
  uint32 seq
  time stamp
  string frame_id
geometry_msgs/Quaternion orientation
  float64 x
  float64 y
  float64 z
  float64 w
float64[9] orientation_covariance
geometry_msgs/Vector3 angular_velocity
  float64 x
  float64 y
  float64 z
float64[9] angular_velocity_covariance
geometry_msgs/Vector3 linear_acceleration
  float64 x
  float64 y
  float64 z
float64[9] linear_acceleration_covariance
```

实际上可以通过查看`xsens_driver`驱动的ros官网查看所有可以产生的话题，列出以下表格：

|                     话题                     |                    数据                    |
| :------------------------------------------: | :----------------------------------------: |
|         `imu/data (sensor_msgs/Imu)`         |            方位角速度和线加速度            |
|        `fix (sensor_msgs/NavSatFix)`         | 经度、纬度和海拔（来自GPS，仅适用于MTi-G） |
|   `velocity (geometry_msgs/TwistStamped)`    |        速度信息（仅来自GPS的线性）         |
|    `imu/mag (sensor_msgs/MagneticField)`     |                  磁场信息                  |
|   `temperature (sensor_msgs/Temperature)`    |                 传感器温度                 |
|    `pressure (sensor_msgs/FluidPressure)`    |                  压力信息                  |
|        `analog_in1 (std_msgs/UInt16)`        |               第一个模拟输入               |
|        `analog_in2 (std_msgs/UInt16)`        |               第二个模拟输入               |
|     `ecef (geometry_msgs/PointStamped)`      |      ECEF位置（仅适用于带GPS的设备）       |
| `time_reference (sensor_msgs/TimeReference)` |             来自设备的时间信息             |
|       `imu_data_str (std_msgs/String)`       |      从IMU解析的所有数据的字符串版本       |

所以，我们只要用程序订阅`/xsens_driver`节点发出的数据并存储就行了。



### 4.用c++程序来订阅话题数据示例

首先要创建工作空间(`worksapce`)是存放工程相关文件的文件夹，分为以下四个文件夹

- `src`：代码空间
- `build`：编译空间
- `devel`：开发空间
- `install`：安装空间

> 创建`catkin_ws`工作空间

```
$ mkdir -p ~/catkin_ws/src
$ cd ~/catkin_ws/src
$ catkin_init_worksapce
```

> 编译工作空间

```
$ cd ~/catkin_ws
$ catkin_make
```

> 设置环境变量

```
$ source devel/setup.bash
```

> 检查环境变量设置是否正确

```
$ echo $ROS_PACKAGE_PATH
```

> 创建`test_pkgstd_msgs`功能包，并添加 `rospy`和`roscpp`依赖，一个针对python编程，一个针对c++编程

```
$ cd ~/catkin_ws/src
$ catkin_create_pkg test_pkgstd_msgs rospy roscpp
```

> 编译功能包

```
$ cd ~/catkin_ws
$ catkin_make
$ source ~/catkin_ws/devel/setup.bash
```

*注意：同一个工作空间下，不允许同名的功能包；不同工作空间下，允许同名的功能包*；

------

所有的程序都要写在功能包里面，要先创建空的功能包，再编译空的功能包，再打开功能包可以看到有`src`文件夹，在`src`文件夹里面写程序

以海龟仿真器为例子，通过订阅`/turtle1/pose`话题来接收位置信息

```c++
// 订阅/turtle/pose话题，消息类型为turtlesim::Pose

#include <ros/ros.h>
#include "turtlesim/Pose.h"

//接收订阅消息后，会进入消息回调函数，在数据进来时会类似中断，执行完成后才执行下一个数据，不会嵌套
void poseCallback(const turtlesim::Pose::ConstPtr& msg){
    //将接受到的消息打印出来
    ROS_INFO("Turtle Pose: x:%0.6f, y:%0.6f",msg->x,msg->y);
}
int main(int argc,char **argv){
    //初始化Ros节点,pose_subscriber是该程序文件的名字
    ros::init(argc,argv,"pose_subscriber");
    
    //创建节点句柄
    ros::NodeHandle n;
    
    //创建一个Subscriber,订阅名为/turtle/pose的topic，注册回调函数poseCallback,10为话题订阅的队列长度就是缓冲区
    ros::Subscriber pose_sub = n.subscriber("/turtle/pose",10,poseCallback);
    
    //循环等待回调函数
    ros::spin();
        
    return 0
}
```

程序写好后，在功能包目录下，打开 `CMakeLists.txt`文本文件，找到如下的文本，添加上面一句话，保存

```
add_executable(pose_subscriber src/pose_subscriber.cpp)
target_link_libraries(pose_subscriber ${catkin_LIBRARIES})

##############
## Install 	##
##############
```

> 编译功能包

```
$ cd ~/catkin_ws
$ catkin_make
```

> 运行程序

```
$ rosrun test_pkgstd_msgs pose_subscriber
```

------



### 5.ros系统其他一些功能简述

#### （1）参数使用

> 参数就类似全局字典，所有节点都可以访问，在运行`xsens_driver.launch`文件下
>
> 查看所有参数

```
$ rosparam list

/rosdistro
/roslaunch/uris/host_pi_virtual_machine__32831
/rosversion
/run_id
/xsens_driver/angular_velocity_covariance_diagonal
/xsens_driver/baudrate
/xsens_driver/device
/xsens_driver/frame_id
/xsens_driver/frame_local
/xsens_driver/initial_wait
/xsens_driver/linear_acceleration_covariance_diagonal
/xsens_driver/no_rotation_duration
/xsens_driver/orientation_covariance_diagonal
/xsens_driver/timeout
```

> 显示某个参数的值

```
$ rosparam get /xsens_driver/baudrate
0
```

> 设置某个参数的值

```
$ rosparam set /xsens_driver/baudrate 115200
```

> 保存参数到当前目录下文件

```
$ rosparam dump xsens_driver_param
```

> 打开`xsens_driver_param`文件

```

```

*可以在上面修改数值，就可以统一修改参数*

> 从文件读取参数

```
$ rosparam load xsens_driver_param
```

> 删除参数

```
$ rosparam delete /xsens_driver/baudrate
```

------

以小海龟仿真程序为例，用c++程序读取和设置参数

> 创建功能包

```
$ cd ~/catkin_ws/src
$ catkin_create_pkg learning_parameter roscs
```

> 配置 `CMakeLists.txt`文本文件的编译规则

```
add_executable(parameter_config src/parameter_config.cpp)
target_link_libraries(parameter_config ${catkin_LIBRARIES})

##############
## Install 	##
##############
```

> 编译运行

```
$ cd ~/catkin_ws
$ catkin_make
$ source devel/setup.bash
$ roscore
$ rosrun turtlesim turtlesim_node
$ rosrun learning_parameter parameter_config
```

------



#### （2）launch文件使用简述

当订阅多个话题时或者需要其他操作，那么需要运行多个程序而且每次都要启动ros系统，所以我们可以把这些操作全部封装在`.launch`文件中，之后运行程序就和驱动程序一样运行`.launch`文件就行了。

> launch文件部分语法：xml

```xml
<launch>
    //启动节点
    <node pkg="package-name" type="executable-name" name="node-name"/>
</launch>
```

- `pkg`：节点所在的功能包名称
- `type`：节点可执行文件名称
- `name`：节点运行时名称
- `output`、 `respawn`、`ns`、`args`、`required`

```xml
<launch>
    //设置ros系统运行中的参数，存储在服务器中
    <param name="output_frame" value="odom"/>
    //加载参数文件的多个参数
    <rosparam file="param.yaml" command="load" ns="params"/>
    //launch文件内部的局部变量
    <arg name="arg-name" default="arg-value"/>
</launch>
```

- name：参数名
- value：参数值

```xml
<launch>
    //包含其他launch文件，file为launch文件路径
    <include file="$(dirname)/other.launch"/>
</launch>
```

还有等等其他标签，可以在里面设置参数，而且默认启动ros，不用手动设置了，详情见：http://wiki.ros.org/roslaunch

要想使用launch文件需要先在工作空间中建立功能包

```
$ catkin_create_pkg learning_launch
```

在功能包建立launch文件夹存储launch文件，在添加新的launch文件时要对工作空间重新编译，然后运行`example.launch`文件

```
roslaunch learning_launch example.launch
```



## 四、总结

### 1.连接树莓派

可以直接用手机开热点，修改wifi名或者密码到上面，也可以修改network-config文件，更改wifi，可能连接不上，多试几次给树莓派断电。在手机上可以直接查看到树莓派IP，其他地方的话使用Advanced IP Scanner软件查看树莓派IP，得到IP地址后，利用putty或者Xshell连接树莓派。



### 2.跳转到可执行文件的目录

在putty或者Xshell终端上使用命令，跳转到目录里面

```
cd xsens/examples/mtsdk/xda_public_cpp
```



### 3.数据采集

输入命令，开始采集

```
sudo ./example_mti_receive_data
```

该cpp文件被修改过，需要输入采集时间（单位：s）。如果不需要那么多数据，可以修改`example_mti_receive_data.cpp`文件，然后注释掉某些命令。



### 4.数据解析

源文件有解析数据的，但是不好用，下载到本地（Windows电脑上），利用命令

```
sz logfile.mtb
```

下载Windows电脑的MTI的软件，官网地址：https://content.xsens.com/download-mt-software-suite，选择Windows版本的。

找到`MT Manager`文件夹打开，运行`mtmanager64.exe`程序，打开桌面端。

打开程序后，选择菜单栏的File->Open File，选择`logfile.mtb`文件，加载数据。

菜单栏File->Export导出数据。











