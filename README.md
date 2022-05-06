# Making Old Hardware New

Warning Preform at Your Own Risk!!!!

Linksys EA8500 DD-WRT Flash Write-Up

![image](https://user-images.githubusercontent.com/97143257/167186821-95524549-b7ca-4a18-9c32-66bb0bf91deb.png)

Sources:

· https://mrjcd.com/EA8500_DD-WRT/#newbuild

· https://www.s-config.com/linksys-e8500-dd-wrt-router

These two were my main sources of guidance, The rest have the firmware, Cool shops to get tools, and great information for the router itself on its capabilities.

· https://dd-wrt.com/support/router-database/?model=EA8500_1.0 DD-WRT- most current firmware at time of writing

· https://www.adafruit.com Adafruit- Amazing group Makers out of New York lead by Lady Ada

· https://hackerwarehouse.com Hacker Warehouse- Amazing source of hard-to-find tools for the InfoSec community.

· https://fcc.io/ FCC Search Tool- Search Q87-EA8500 in the search bar and you will be taken to the FCC page with the Complete FCC test report (also a great site to check any device for extra documentation.)

· https://www.tomsguide.com/us/linksys-ea8500-router,review-2867.html Tom’s Guide- Original review for the EA8500 from 06/2015

Requirements:

· Linksys EA8500 there are two board versions I found online that I will go over later.

· Ethernet cable

· Philips head screwdriver

· Spudger (small plastic pry tool)

· Multimeter

· USB to TTL module (I used a Bus Pirate)

Hardware Specs:

· Qualcomm IPQ8064 1.4Ghz Dual-Core

· 128 MiB Flash Memory supports serial flashing via internal headers

· 512 MiB DDR 3 RAM

· 1 USB 3.0 ports

· 1 eSATA/USB 2.0 combo port

· Gigabit speed 4x4 MU-MIMO with Beamforming

· 2.4 Ghz b, g, n 5.0 Ghz an+ac

![image](https://user-images.githubusercontent.com/97143257/167187037-402a5193-f143-4530-8ec8-b6a4a7577651.png)

Opening the Router:

To begin I started by removing the rubber feet off the bottom of the router.

![image](https://user-images.githubusercontent.com/97143257/167187167-6927a50d-aca3-4f5b-bdde-fa44c51b193e.png)

I used my spudger to pry the adhesive free, exposing the four Philips head screws holding the clam shell together. After removing the screws, I used the spudger in the seam of the case and slid it along to free the clips holding it together.

![image](https://user-images.githubusercontent.com/97143257/167187263-0820c2fe-d53b-4c3f-85d7-eb5eba0c7381.png)

Here is the first glorious view of the circuitry inside.

![image](https://user-images.githubusercontent.com/97143257/167187346-4688ead9-e9dd-41d1-b058-54c5469c1457.png)
![image](https://user-images.githubusercontent.com/97143257/167187426-0d307c9d-8cd3-4ddf-9974-db5e831eca70.png)
![image](https://user-images.githubusercontent.com/97143257/167187469-2eab3d38-4b96-4f31-828a-1b0197946188.png)

It is also where I found that, I got the REV. 205 (XC) board. This is the spot where your experience may diverge from this walk-thru a bit. The 205(XC) is the older version of the board that kept the serial headers in place as seen above. If you have the newer version 305(XD) You will find that the headers are missing.

![image](https://user-images.githubusercontent.com/97143257/167187535-3353be99-7ed8-4637-b036-5becf214e7f8.png)

Credit: https://www.s-config.com

But according to them all the necessary circuitry is still intact. You will just have to break out a soldering iron and attach a five-pin header to the JM1 slot pictured above. After that you can continue with the walkthrough.

Downloads:

Now that the router is ready, I turned to my computer to set it up for the process. I started by going to https://mrjcd.com/EA8500_DD-WRT/#newbuild and downloading the PuTTy-TFTP.zip file on the page. I used ESET to scan the file which showed as clean, but as with anything you download from the internet use caution. The zip file contains six items. Putty.exe, tftpd32.exe, EA8500-factory-to-ddwrt.img, EUPL-EN.pdf, tftpd32.chm, tftpd32.ini. The two programs are Putty which will allow the PC to interact with the router through the USB to TTL (Serial) connection. The other is tftpd32 this is Trivial File Transfer Protocol to transfer data from the PC to the Router. So, think of Putty being the road and tftpd32 being a car going from one location to another.

Connecting PC to EA8500:

For this I read that the type of USB to TTL matters. Everything that I read said it needs to be the PL2303HX-USB-to-TTL-RS232 version. They are not very expensive on Adafruit https://www.adafruit.com/product/4364 $19.99 at time of writing. I used a tool called Bus Pirate and it worked great. It has more features and built-in macros that enable the serial connection to be used as an interactive shell. Adafruit is currently sold out, but they update stock all the time and it is only $30. https://hackerwarehouse.com/product/bus-pirate/ has some in stock currently, but their offering is $50 but is a bit different with a better wire harness and an acrylic case.

![image](https://user-images.githubusercontent.com/97143257/167187714-11563a88-716e-4af7-b44a-a7e4a5fe6a74.png)

Connecting the micro-USB end to the PC will enable us to view the COM port the PC will use to communicate. Using the Windows search, type Device Manager and open it. Under Ports (COM & LPT) You should see one option. In my case it was COM5. I made note of that for later. Next, I used my multimeter to verify which header pin was ground. I used the continuity setting and set one probe to the grounded screw hole and touched all the pins till I found continuity indicated by the buzzer, and the meter reading zero resistance.

![image](https://user-images.githubusercontent.com/97143257/167187787-1001ce88-c135-46f7-a5b2-263dc4e17c57.png)
![image](https://user-images.githubusercontent.com/97143257/167187832-8f1c4893-9d12-4096-996d-5fba4ea1918a.png)

After I verified that the pin closest to the screw hole was ground. I then moved to connecting the pirate. For this I consulted the wiring harness diagram for the pirate to determine which wire was ground and in the case of this router it uses UART as its Communication protocol, so I needed to find the TX (Transmit) and RX (Receive) wires. As shown in the chart below I have the bottom wire kit so according to it Ground is Brown, TX is Grey, and RX is Black. The clips don’t match the wire colors.

![image](https://user-images.githubusercontent.com/97143257/167187933-2f47958c-f27b-411a-8831-3a81c959a5b1.png)
![image](https://user-images.githubusercontent.com/97143257/167187970-61413c5f-b0ac-4451-98ff-3efa4603d3dc.png)
![image](https://user-images.githubusercontent.com/97143257/167188020-8ff2c331-2689-4bfb-a403-a6cc614e7fed.png)

Lastly, I turned the switch on the router to off, plugged it into power, and ran the Ethernet cable to my PC’s NIC. I then moved on to software tools.

Installation and Configuration of Software Tools:

I created a file on my desktop named EA8500 and extracted the zip file I downloaded earlier there. Next, I installed and configured My network settings, Putty, and tftpd32 using the following configurations.

Network Settings:

I went into my network settings and opened my adapter settings. From there I disabled my WIFI card, so I did not pick up internet on that NIC. I then open the properties of my Ethernet NIC and set my IP address to Static using the “Use following IP address” setting. I set the IP address to 192.168.1.2 and the subnet mask to 255.255.255.0 with no default gateway.

![image](https://user-images.githubusercontent.com/97143257/167188105-21dcf65c-49e9-4073-bc92-5cc732dfb2b5.png)

Putty:

Once Putty was installed, and opened, I had to go in and set up the settings to communicate smoothly with the router. For this I used the Serial option under connection type. Set the Serial line to COM5 from earlier. For the speed, I used the recommended Baud speed of 115200, that is the speed of the bytes being sent and received. I also went into logging and set Session logging to All Session Output. So that I would get a log file created with all the comms between the PC and Router.

![image](https://user-images.githubusercontent.com/97143257/167188180-929dcc1c-9c24-4e82-a20c-41d6109c8b79.png)
![image](https://user-images.githubusercontent.com/97143257/167188227-e566b3b0-f44d-4a1e-b50b-8341e315fe5b.png)

With that I went ahead and clicked open. All my settings and connections were good. And I was greeted with this screen.

![image](https://user-images.githubusercontent.com/97143257/167188302-a001ed6d-a980-4666-9dd1-36821600b02a.png)

Credit: https://mrjcd.com/EA8500_DD-WRT/#newbuild

Next, I had to configure the Bus Pirate in the Putty shell. Using the ? command I was given a list of commands available. With the Bus pirate you do not have a full shell, so you must be careful of your typing because you cannot delete characters. If you miss type, you may have to start over with a reboot of the Pirate and Putty. Using the command m for mode I was given this list below. I chose option 3 for UART then option 9 for 115200 Then for data bits, stop bits, and receive polarity I just used default by hitting enter. The last option for Output type does need to be changed to option 2 indicating I am using the ground wire. That last part is where it is easy to miss type. To use the macros, I used the (0) option, parentheses included. I used live monitor to check it out at first using (2) as seen below. But to complete the flashing I needed to use (1) for transparent bridge so that I could type, and the router shell would recognized that input.

![image](https://user-images.githubusercontent.com/97143257/167188373-4e01078d-d0a6-4e67-973b-78eccefcafcb.png)
![image](https://user-images.githubusercontent.com/97143257/167188415-1d18c7b5-5480-4ec0-8661-8dd72d47abbc.png)

Now with the pirate ready I switched on the router. The first time I booted it up I was too slow as this router has a three second window to get to the IPQ controls before the bootloader begins loading Linksys’s firmware. So, I turned the router off and reset my putty connection and switched it back on, reconfigured the pirate and this time I hit the any key to stop autoboot, after a second the router booted into IPQ mode, and I was greeted with (IPQ) prompt shown below.

![image](https://user-images.githubusercontent.com/97143257/167188475-128f3c5a-f909-481c-8dc8-b823bea856ff.png)

Now that I have IPQ shell I can use the Trivial File Transfer Protocol to send over the new firmware.

TFTPD32:

Setting this up comes with another obligatory warning to do your due diligence to verify the safety of your downloads. The following steps require you to allow the TFTPD32 through your operating system firewall. In my case I was not connected to the internet, so I disabled it temporarily. It must be run as admin as well. Once opened I needed to change a few options included in mrjcd blog. Under the Global tab the only option that needs to be checked is the TFTP Server. Under the TFTP tab You need to have the base directory point to the file created earlier, in my case on my desktop. Under TFTP Security set this to None. TFTP configuration should be set properly by default, if not the Timeout (Seconds) need to be set to 3, the Max Retransmit set to 6, the TFTP port set to 69, and local port pool empty. Under Advanced TFTP Options only three boxes need to be checked, Option Negotiation, Show Progress Bar, and Translate Unix File Names.

![image](https://user-images.githubusercontent.com/97143257/167188541-55e3a2e7-41b4-493b-9b09-b764936aad02.png)
![image](https://user-images.githubusercontent.com/97143257/167188577-c5d6bd36-4958-49db-979d-8fb6d7996fd7.png)
![image](https://user-images.githubusercontent.com/97143257/167188622-9f9371a3-e158-4ea9-bb25-949faedc5d1c.png)

Credit: https://mrjcd.com

The last screen shot above will be what it looks like when it is ready to serve the files to the router keep in mind yours will show where you stored your files.

The Transfer:

Now that the router is ready to accept our file transfers and our TFTP server is ready to serve up the files. We can turn our attention back to the IPQ shell in Putty. We need to tell it to set the first portion of the 128 MiB flash to the DD-WRT factory image, have it recognized the new local IP Address of the router, the IP address of the TFTP Server we setup and run it.

![image](https://user-images.githubusercontent.com/97143257/167188757-1fa430fc-b15a-451f-8cc8-839d7c72789a.png)

Using the commands:

setenv image EA8500-factory-to-ddwrt.img

setenv ipaddr 192.168.1.1

setenv serverip 192.168.1.2

run flashimg

As you can see from my logs this is where I found the firewall issue. If you begin to receive T T T T T T this is indicating that the data is not being transferred, in my case I left the transfer going and switch over to disable my firewall in Realtime, it began transferring properly indicated be the # # # # # #. Once the first partition was flashed to the DD-WRT factory image, we can flash the second partition with the web interface. Using the Commands:

setenv image EA8500-factory-to-ddwrt.img

setenv ipaddr 192.168.1.1

setenv serverip 192.168.1.2

run flashimg2

![image](https://user-images.githubusercontent.com/97143257/167188857-223c44da-81bd-44bd-87ab-b63781333cf2.png)

Now once it is completed and you have an IPQ prompt back, type reset to reboot the router. While the router is rebooting you can close TFTPD32, and reenable your firewall. You can also close Putty, in my case I was pulling the logs of the reboot, so I left it running, but if you are not logging you can close Putty as well. With the Ethernet cable still hooked up to your machine once the router fully reboots. Open a browsers page and navigate to 192.168.1.1 and you will be greeted with the new DD-WRT interface.

![image](https://user-images.githubusercontent.com/97143257/167188952-d20aa6a4-8d80-4eee-9935-625c4c5f829e.png)

After setting the username and password for the new interface, I highly recommend first updating to the most up to date firmware available. To do this you will have to hook the router up to your Wan network to give it internet. Also now is a good time to change your IP address back to the DHCP setting changed earlier. Once the router is booted with network connection go to the Administration Tab and run an update check which that will update the firmware to the most current version. This will unlock all the new features that you can take advantage of including WPA3 Enterprise and other current features that were not possible with the Linksys firmware. If you were following along and made it this far Congratulations you flashed some open-source software on your aging equipment breathing new life into it and keeping it out of a landfill 😊I hope you found this useful.
