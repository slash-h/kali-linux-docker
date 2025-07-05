
This Dockerfile and the container is created by - https://blog.meister-security.de/kali-linux-mit-xfce-im-docker-container/

Steps:
1. Download the Dockerfile (update it if required)
2. Create Image, set user and password
   ```
	docker image build --platform linux/amd64 \
        -t custom/kali-linux \
        --build-arg DESKTOP_ENVIRONMENT=xfce \
        --build-arg REMOTE_ACCESS=rdp \
        --build-arg KALI_PACKAGE=core \
        --build-arg RDP_PORT=3389 \
        --build-arg VNC_PORT=5908 \
        --build-arg VNC_DISPLAY=$XVNC_DISPLAY \
        --build-arg SSH_PORT=20022 \
        --build-arg BUILD_ENV=amd64 \
        --build-arg HOSTDIR \
        --build-arg CONTAINERDIR \
        --build-arg UNAME=user \
        --build-arg UPASS=theuserpass \
        .
   ```
3. Create container
   ```
	docker create   --name kali-linux \
                --network bridge \
                --platform linux/amd64 \
                -p 3389:3389 \
                -p 5908:5908 \
                -p 20022:20022 \
                -t \
                -v ./kali/home/user:/opt \
                custom/kali-linux
   ```
4. Start the container
   ```
	docker start kali-linux
   ```
Once the container is up and running, connect to it using a RDP program (e.g. freeRDP). Here is macOS, use the Windows App (remote desktop).
A new connection is created in the Windows App giving hostname as 'localhost'
Upon trying to connect to the new connection (localhost), system asks for user and password. Enter the user and password set during creation of the image above.


After successfully connecting to the remote system, you are already in Kali Linux system. If an error regarding 'notifications plugin' (xfce4-panel) is shown, then click on the remove button and reinstall the panel using below commands -

        sudo apt install --reinstall xfce4-panel
        sudo apt install xfce4-goodies
 
Note that the argument `KALI_PACKAGE=core` can be omitted in the image build command. If the `KALI_PACKAGE` argument is omitted then the `kali-linux-default` package will be installed.
In the above CREATE command, it is instructed to install the `core` package of kali-linux. Therefore there might be some packages (utilites / tools / commands / applications) missing in the system, however, every package can be easily installed when required using `sudo apt install <package_name>`.
