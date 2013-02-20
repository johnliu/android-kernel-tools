Android Kernel Tools
====================

Tools for building, running, debugging the Android Kernel (linux-goldfish-2.6.29) on Mac OS X.


## Requirements

Create a case sensitive drive:
```sh
hdutil create -size 1.5g -fs "Case-sensitive HFS+" -volname ECE353 android.dmg
```

Clone your repository to the drive.
```sh
open android.dmg
cd /Volumes/ECE353/

# Using svn
svn co svn+ssh://remote.ecf.utoronto.ca/n/svn/ece353s_<grp_#>/svn/trunk android

# Using git-svn
git svn clone -s ssh://remote.ecf.utoronto.ca/n/svn/ece353s_10/svn/ android
```

Install Xcode from the App Store and install Command Line Tools.

Install Homebrew (http://mxcl.github.com/homebrew/)
```sh
ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go)"
```

Install the android ndk and sdk.
```sh
brew install android-sdk
brew install android-ndk
```


## Setup

Clone the script to your favourite directory:
```sh
git clone https://github.com/johnliu/android-kernel-tools.git
```

Enable the script as an executable:
```sh
chmod +x ./setup
```

Install the tools:
```sh
./setup install
```

Other possible use cases:
```sh
./setup uninstall           # uninstall the tools
./setup install --dryrun    # show what files will be installed
./setup uninstall --dryrun  # show what files will be uninstalled
```


## Scripts

`agcc` -- `gcc` for android userspace executables.
```sh
# sample usage:
agcc -o program_name source.c
adb push program_name /data/local/
adb shell /data/local/program_name
```

`agdb` -- `gdb` for android kernel/userspace.
```sh
# run for kernel:
agdb --kernel
agdb -k

# run for userspace:
agdb --user program_name
agdb -u program_name
```

`buildandroid` -- builds the android kernel
```sh
buildandroid
```

`runandroid`, `runkernel`, `runuser` -- runs the android emulator and debugs (for both user and kernel space)
```sh
runandroid -0                   # runs the telnet session continuously
runandroid -0 -v                # runs the telnet session continously with output
runandroid -1                   # builds the android kernel then runs it with a telnet connection
                                # this command also starts an adb shell

runandroid --user program.c     # compiles the userspace program and runs it under agdb
runuser program.c               # same as previous command

runandroid --kernel             # runs the kernel under agdb
runkernel                       # same as previous command
```

### Typical Workflow
1.  Open one terminal window and run `runandroid -0`. This will start attempting to
    connect to telnet.
2.  Open second terminal window and run `runandroid -1`. This will build the android kernel
    and start an adb shell.
3.  Either `runuser program.c` or `runkernel` depending on whether kernel is being tested
    or userspace program is being tested.


## Other Resources

See PeterSlawski/ece353-resources for additional help.
