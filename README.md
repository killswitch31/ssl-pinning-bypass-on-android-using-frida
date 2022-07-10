# ssl-pinning-bypass-using-frida

You need a rooted android device to do this.

1. sudo apt-get install android-tools-adb 
 

2. Run the following commands:

  python -m pip install Frida 
  python -m pip install objection 
  python -m pip install frida-tools 

              Or 

  pip install Frida
  pip install objection 
  pip install frida-tools 

3. If  running on windows, make sure to include dirs containing frida.exe in PATH variables for system env.

4. To find out the arch version of the device, run following command: 

    adb shell getprop ro.product.cpu.abi 

 

5. Go to this link and download frida-server for android for your arch. And then unzip it and rename the unzipped file as “frida-server”. 
    
    https://github.com/frida/frida/releases/ 

6. Follow this guide to set proxy in burp for android device (https://support.portswigger.net/customer/portal/articles/1841101-configuring-an-android-device-to-work-with-burp ): 

    a. export CA cert from BurpSuite and name it "cacert.cer"
    
    b. export CA cert again from BurpSuite and name it "cacert.der"
    
    c. Email the "cacert.cer" file to a gmail address.
    
    d. On your android device, open the email in Gmail app and opne the "cacert.cer" attachment.
    
    e. Enter you passcode, if prompted, and add the cert to the trusted root CA on device. Name it anything you want. 


7. Run the following commands: 

    adb push frida-server /data/local/tmp 

    adb shell chmod 755 /data/local/tmp/frida-server 

8. Then in a different terminal window run "adb shell" and do the following (do not close the terminal after done. Just minimize it) 

    commands:  

      Su 

      Cd /data/local/tmp/ 

      ./frida-server & 

    Just minimize the terminal. 

 

9. You must have exported the burp cert in step 6(b). 

    Run the following command to push the burp cert to device: 

    adb push cacert.der /data/local/tmp/cert-der.crt 

 

10. Run the following command while app is open on the rooted android device: 

    Command: frida-ps –U 

    Note down the package name from the list of running processes.
 

11. If no errors in previous step, then run the following command: 

    frida --codeshare akabe1/frida-multiple-unpinning –U –f <package-name>

    Type “%resume” when the prompt appears. 

12. Don’t forget to configure wifi settings to use burp as proxy. 


SSLPinning BYPASSED!! 

Enjoy!! 

 

 

frida --codeshare akabe1/frida-multiple-unpinning –U –f <package-name> 
