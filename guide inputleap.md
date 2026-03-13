
To finalize your setup on
openSUSE Leap 16.0, you can create a desktop entry for the application menu and set it to launch automatically when you log in.
1. Create a Desktop Shortcut (.desktop file)
Since you built from source, you might need to manually create the entry so it appears in your KDE Plasma or GNOME menu.

    Create the file:
    bash

    nano ~/.local/share/applications/input-leap.desktop

    Use code with caution.

Paste the following content:
ini

[Desktop Entry]
Name=InputLeap
Comment=Share mouse and keyboard between computers
Exec=/usr/local/bin/input-leap
Icon=input-leap
Terminal=false
Type=Application
Categories=Network;Utility;
Keywords=keyboard;mouse;sharing;

Use code with caution.
(Note: If which input-leap returned /usr/bin/input-leap instead, update the Exec line accordingly.)
Save and exit (Ctrl+O, Enter, Ctrl+X). Your menu should update instantly.

2. Set Up Autostart (Two Ways)
Method A: The GUI Way (Recommended for Plasma/GNOME)

    Open System Settings.
    Navigate to Startup and Shutdown > Autostart.
    Click + Add > Add Application.
    Search for InputLeap and select it.

Method B: The Manual Way (Works on all Desktop Environments)
Simply copy the desktop file you just created into the autostart folder:
bash

mkdir -p ~/.config/autostart
cp ~/.local/share/applications/input-leap.desktop ~/.config/autostart/

Use code with caution.
3. Running Without the GUI (Headless Client)
If you are setting up a Client (the machine being controlled) and don't want the window to pop up every time, change the Exec line in your Autostart file to:
bash

Exec=/usr/local/bin/input-leapc -f --name [Your_Client_Name] [Server_IP_Address]

Use code with caution.
Replace [Your_Client_Name] with the name you gave this PC in the server settings.
4. Firewall Reminder
InputLeap will fail to connect if the port is closed. Run this once:
bash

sudo firewall-cmd --add-port=24800/tcp --permanent
sudo firewall-cmd --reload

Use code with caution.
Do you need help finding the Server IP or naming your Client to get them talking to each other?

help finding the Server IP or naming your Client to get them talking to each other
To get your computers talking, you must ensure the
Screen Name on the Client matches the configuration on the Server and that the Client knows the Server's Local IP Address. 
1. Find the Server IP Address 
On your Server machine (the one with the keyboard and mouse), find its local network address: 

    Terminal Method: Run hostname -I or ip addr show. Look for the inet address (e.g., 192.168.1.15).
    GUI Method: The InputLeap main window usually displays the server's IP address(es) at the top of the interface. 

2. Identify and Name the Client
The Client and Server must use the exact same "Screen Name" (case-sensitive). 

    On the Client: Open InputLeap. Look for the field labeled Screen name (often found just above the Server IP field). Note this name exactly.
        Tip: You can change this name by going to Settings (or pressing F4).
    On the Server:
        Click Configure Server.
        In the grid, click and drag the monitor icon from the top-right into a slot relative to your main screen.
        Double-click the new monitor icon and change its Screen name to match the Client's name exactly. 

3. Connect the Client
On your Client machine:

    Ensure "Client" mode is selected.
    Uncheck "Auto config" if you want to enter the IP manually.
    Enter the Server IP Address found in Step 1 into the "Server IP" field.
    Click Start. 

Connection Troubleshooting

    Firewall: If it fails to connect, ensure port 24800 is open on the server.
    SSL/Crypto: If you see certificate errors, you may need to go to Settings and ensure "Enable SSL" is either checked or unchecked identically on both machines.
    Unrecognized Name: If the server log says unrecognised client name, double-check that the name in the Server's grid matches the Client's screen name exactly, including capitalization. 

    https://github.com/input-leap/input-leap
