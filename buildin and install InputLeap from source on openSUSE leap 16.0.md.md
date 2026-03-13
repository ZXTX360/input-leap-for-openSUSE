This guide details how to build and install
InputLeap from source on openSUSE Leap 16.0.
Building InputLeap on openSUSE Leap 16.0
1. Prerequisites
Open a terminal and install the necessary build tools and development libraries. Leap 16.0 uses a modern stack (GCC 15+, Qt 6), so ensure these specific packages are present:
bash

sudo zypper install cmake gcc-c++ git rpm-build \
libX11-devel libXtst-devel libXext-devel libXinerama-devel \
libXrandr-devel libXi-devel libICE-devel libSM-devel \
avahi-compat-mDNSResponder-devel qt6-base-devel \
qt6-linguist-devel openssl-devel libcurl-devel

Use code with caution.
2. Obtain the Source Code
To ensure all sub-components (like internal libraries) are included, it is highly recommended to clone the repository using Git rather than downloading a ZIP file.
bash

cd ~/Downloads
git clone --recursive https://github.com
cd input-leap

Use code with caution.
Note: If you already have a source folder and it is missing files, run git submodule update --init --recursive inside the folder.
3. Configure the Build
Create a dedicated build directory to keep the source tree clean. We will disable unit tests to speed up the process and avoid issues with missing test dependencies.
bash

mkdir build
cd build
cmake -DINPUTLEAP_BUILD_TESTS=OFF ..

Use code with caution.
4. Compile and Install
Once cmake finishes without errors, compile the application using all available CPU cores:
bash

# Compile
make -j$(nproc)

# Install to system
sudo make install

Use code with caution.
5. Verification
Verify that the binary is correctly placed in your system path:
bash

which input-leap

Use code with caution.
To launch the application:
bash

input-leap

Use code with caution.
Troubleshooting Leap 16.0 Specifics

    Wayland Support: Leap 16.0 defaults to Wayland. InputLeap works best on X11. If the cursor cannot move between screens, log out and select "Plasma (X11)" or "GNOME on Xorg" at the login screen.
    Firewall: You must open port 24800 to allow connections:
    bash

    sudo firewall-cmd --add-port=24800/tcp --permanent
    sudo firewall-cmd --reload

    Use code with caution.

Missing Files: If you see errors regarding gtest-all.cc, ensure you used the -DINPUTLEAP_BUILD_TESTS=OFF flag during the cmake step.
