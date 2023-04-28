---
title: "(Quick !!) CAN /CAN-FD Debugging Guide" # Title of the blog post.
date: 2022-04-28T16:28:18+05:30 # Date of post creation.
description: "Debugging of CAN (Controller Area Network) and CAN-FD (Flexible Data-rate) in a system is a critical aspect of ensuring smooth and reliable communication between the electronic components. One common issue that can arise during debugging is signal integrity, which can cause errors in the transmission and reception of data. It is also important to check the configuration and settings of the CAN/CAN-FD controllers, such as the bitrate and sample point, to ensure they are properly set up. Additionally, analyzing the bus traffic and using debugging tools, such as oscilloscopes and logic analyzers, can help identify and isolate issues. By carefully debugging the CAN/CAN-FD system, you can ensure optimal performance and avoid potential errors or malfunctions." # Description used for search engine.
featured: true # Sets if post is a featured post, making appear on the home page side bar.
draft: false # Sets whether to render this page. Draft of true will not be rendered.
toc: false # Controls if a table of contents should be generated for first-level links automatically.
# menu: main
usePageBundles: false # Set to true to group assets like images in the same folder as this post.
featureImage: "/images/CAN_and_Debugging_Guide/2023-04-28-17-10-05.png" # Sets featured image on blog post.
featureImageAlt: 'CAN and Debugging Guide on the DB9 connector' # Alternative text for featured image.
featureImageCap: 'CAN and Debugging Guide on the DB9 connector' # Caption (optional).
thumbnail: "/images/CAN_and_Debugging_Guide/2023-04-28-16-52-53.png" # Sets thumbnail image appearing inside card on homepage.
shareImage: "/images/path/share.png" # Designate a separate image for social media sharing.
codeMaxLines: 10 # Override global value for how many lines within a code block before auto-collapsing.
codeLineNumbers: true # Override global value for showing of line numbers within code block.
figurePositionShow: true # Override global value for showing the figure label.
categories:
  - Technology
tags:
  - CAN
  - CAN-FD
# comment: false # Disable comment if false.
---
* INTRODUCTION
  * **Project Overview** : Overview on Debugging of CAN / CAN-FD BUS  
      `Debugging of CAN (Controller Area Network) and CAN-FD (Flexible Data-rate) in a system is a critical aspect of ensuring smooth and reliable communication between the electronic components. One common issue that can arise during debugging is signal integrity, which can cause errors in the transmission and reception of data. It is also important to check the configuration and settings of the CAN/CAN-FD controllers, such as the bitrate and sample point, to ensure they are properly set up. Additionally, analyzing the bus traffic and using debugging tools, such as oscilloscopes and logic analyzers, can help identify and isolate issues. By carefully debugging the CAN/CAN-FD system, you can ensure optimal performance and avoid potential errors or malfunctions.`
  <!-- * **Project Deliverables or application** : -->
  * **Detailed description of components** :  
    * tested on STM32H7XX
    * Code generated through CubeMx (see reference for more detail)
  * **Safety Requirement** :
    * **Debugging of CAN BUS** :
      * CAN BUS need to be transmitted with the impedance of of 120 Ohm. At the end of the system.
      * CAN bus connection need to be done in daisy chain mechanism (it is recommended).
      * Go with the twisted pair wire or shielded twisted pair wire.
      * Connector recommended is **Deutsche Plug Connector**
      * **CAN-H** (voltage level) : 2.5v to 3.5v
      * **CAN-L** (voltage level) : 1.5v to 2.5v
      * While on multimeter on a working connection the voltage of (depending on the bandwidth or bus load):
        * CAN H slightly **above** 2.5v
        * CAN H slightly **below** 2.5v
      * Termination register is important to avoid back propagation of signal.
      * The stub or extension wire need to be shorter than the main wire to avoid noise in the signal.
      * The wire resistance between a working CAN circuit need to be 60 ohm(recommended), it can be 40 ohm (not recommended).
      * Some device have the termination resistance build in which is software configurable or can be tested when the device is power on.
      * If the `CAN-H` and `CAN-L` are connected reverse then the voltage in CAN-L will be higher than CAN-H.
      * The proffered way of debugging a CAN circuit is to check each CAN device one by one.
      * If any of the CAN Signal is grounded than the overall voltage will drop regardless of CAN-H and CAN-L.
      * The impedance between CAN wire and ground should in M ohm (open-circuit).
    * visit this site for the selection of the time quanta : [http://www.bittiming.can-wiki.info/](http://www.bittiming.can-wiki.info/)
    * And for FD-CAN use this software: https://www.peak-system.com/fileadmin/media/files/BitRateCalculationTool.zip
    * Calculation of time quanta
      ```bash
      APB1 CLK(PCLK1) = 25 Mhz
      CAN Pre-scaler = 1
      Duration of 1 time quanta (1TQ) = PCK1 / CAN_pre-scaler
                                      = 0.04 micro seconds
      ```
    * Always check for the clock frequency of the APB1 bus and configure your system according to that.
    * **TODO** : Try this config (untested) 
       ![](2021-08-27-16-29-51.png)
<p align="center">
<img align="center" width="600" height="600" class="special-img-class" src="/images/CAN_and_Debugging_Guide/2023-04-28-16-34-22.png" alt =" CAN Points in Vehicle ECU" title ="CAN Points in Vehicle ECU" >
</p>

<!-- * PROJECT ORGANIZATION
  * software or hardware tools required
  * Hardware Interfaces or software interface
* SYSTEM ARCHITECTURAL DESIGN (if any)
  * chosen system architecture
  * System Interface Description
  * Discussion of Alternative Designs
* TEST PLAN ( if any)
  * Features to be Tested
  * Features not to be Tested
  * Testing Tools and Environment Setup
* TEST CASES (if any)
  * Case-n :
  * Purpose
  * Inputs
  * Expected Outputs -->
* **REFERENCE AND ADDITIONAL MATERIAL** :  
  * See this youtube video for troubleshooting guide : [Link](https://www.youtube.com/watch?v=ulcKnrPmJqM)  
  * Bosch ecu EDC17C53 Connection : [Link](https://www.evc.de/en/product/bsl/showEcu.asp?title=EDC17C53)   
  * CAN-FD documentation : [Link](can_fd_spec.pdf)  
  * Explanation of CAN-FD calculation : [Link](https://www.fatalerrors.org/a/stm32g474-canfd-use-case-explanation.html)  
  * Official documentation guide : [Guide](/images/CAN_and_Debugging_Guide/slla270.pdf)  
  
  <iframe src="/images/CAN_and_Debugging_Guide/slla270.pdf" width="100%" height="500px"></iframe>

  * Bosch reference of CAN-FD with example code: [Link](/images/CAN_and_Debugging_Guide/can_fd_spec.pdf)  
  <iframe src="/images/CAN_and_Debugging_Guide/can_fd_spec.pdf" width="100%" height="500px"></iframe>


### This documentation format is based on IEEE standard visit : [Link](http://www.cs.loyola.edu/~binkley/482/src/ieee-documentation-template.pdf)  

<!-- **Note** Please follow the general format for the documentation.   -->