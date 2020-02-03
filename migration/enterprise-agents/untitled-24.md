# Missing dependencies for Enterprise Agent on Redhat Enterprise Linux RHEL 7 installation

By default, the linux package installation of the Enterprise Agent on [supported version of Redhat Enterprise Linux Operating System](https://success.thousandeyes.com/PublicArticlePage?articleIdParam=kA0E0000000CmnoKAC_Supported-Enterprise-Agent-operating-systems) will fail with missing dependency packages errors.  
The install\_thousandeyes.sh script will display the error:

```text
Installing the ThousandEyes AgentFAILURE: Failed installing the ThousandEyes Agent
```

 The following step by step guide will enable the repository holding the Enterprise Agent dependency packages, before attempting to run the Enterprise Agent installation script.

* [Step 1: Register and subscribe]()
* [Step 2: Enable additional repository]()
* [Step 3: Install ThousandEyes Enterprise Agent]()
* [Troubleshooting the installation]()
  * [Default ThousandEyes linux package installation log]()
  * [Are you subscribed to Redhat? Is your subscription valid?]()
  * [Verify repositories]()
  * [Clean your subscription registration]()
  * [Verify dependencies]()
  * [Redhat Knowledgebase reference]()

All the commands below should be run as root. To reduce the content repetition, all "sudo" command prefixes have been removed and the whole guide assumes that you are running each command in a root shell.

To reach the root shell, use the following command:

```text
$ sudo -s
```

The **expected output** is marked with **bold characters** on every sample command output below.

## Step 1: Register and subscribe

An active subscription with Redhat is required. Register and automatically subscribe in one step

```text
# subscription-manager register --username <username> --auto-attach
```

```text
Registering to: subscription.rhsm.redhat.com:443/subscription
Password: <enter your password>
The system has been registered with ID: 1234ab56-a78b-9012-2356-a7bc8901d234
The registered system name is: hostname.yourcompany.com
Installed Product Current Status:
Product Name: Red Hat Enterprise Linux Server
Status: Subscribed
```

## Step 2: Enable additional repository

```text
# subscription-manager repos --enable=rhel-7-server-optional-rpms
```

```text
Repository 'rhel-7-server-optional-rpms' is enabled for this system.
```

## Step 3: Install ThousandEyes Enterprise Agent

Complete the linux package installation by following this article:  
[Enterprise Agent deployment using Linux Package method](https://success.thousandeyes.com/PublicArticlePage?articleIdParam=kA0E0000000CmnZKAS)

## Troubleshooting the installation

### Default ThousandEyes linux package installation log

 Look for errors in the default installation log.

```text
# cat /tmp/install_thousandeyes*.log
```

Sample output errors:

```text
...
yum -y -q install te-agent
Error: Package: te-agent-1.58.2-1.x86_64 (thousandeyes)
           Requires: libldns.so.1()(64bit)
Error: Package: te-agent-1.58.2-1.x86_64 (thousandeyes)
           Requires: libcares.so.2()(64bit)
Error: Package: te-agent-1.58.2-1.x86_64 (thousandeyes)
           Requires: libaprutil-1.so.0()(64bit)
Error: Package: te-agent-1.58.2-1.x86_64 (thousandeyes)
           Requires: libapr-1.so.0()(64bit)
 You could try using --skip-broken to work around the problem
 You could try running: rpm -Va --nofiles --nodigest
```

or

```text
...
yum -y -q install te-browserbot
Error: Package: te-browserbot-1.84.2-1.x86_64 (thousandeyes)
           Requires: xorg-x11-server-Xvfb
 You could try using --skip-broken to work around the problem
 You could try running: rpm -Va --nofiles --nodigest
```

### Are you subscribed to Redhat? Is your subscription valid?

```text
# subscription-manager list
```

```text
+-------------------------------------------+
Installed Product Status
+-------------------------------------------+
Product Name: Red Hat Enterprise Linux Server
Product ID: 69
Version: 7.5
Arch: x86_64
Status: Subscribed
Status Details:
Starts: 06/29/2018
Ends: 06/29/2029
```

### Verify repositories

```text
# yum repolist enabled
```

```text
Loaded plugins: product-id, search-disabled-repos, subscription-manager
repo id repo name status
!rhel-7-server-optional-rpms/7Server/x86_64 Red Hat Enterprise Linux 7 Server - Optional (RPMs) 15,178 - required, manually enabled at Step 2
!rhel-7-server-rpms/7Server/x86_64 Red Hat Enterprise Linux 7 Server (RPMs) 20,668 - required, enabled by default
!rhel-7-server-rt-rpms/7Server/x86_64 Red Hat Enterprise Linux for Real Time (RHEL 7 Server) (RPMs) 309 - enabled by default
```

### Clean your subscription registration

```text
# subscription-manager remove --all
```

```text
1 local certificate has been deleted.
1 subscription removed at the server.
```

```text
# subscription-manager unregister
```

```text
Unregistering from: subscription.rhsm.redhat.com:443/subscription
System has been unregistered.
```

```text
# subscription-manager clean
```

```text
All local data removed
```

### Determine the installed dependencies and corresponding repositories

#### Determine the ID of the yum transaction history

```text
# yum history
```

```text
Loaded plugins: product-id, search-disabled-repos, subscription-manager
ID     | Login user               | Date and time    | Action(s)      | Altered
-------------------------------------------------------------------------------
     3 | root <root>              | 2019-03-22 12:31 | Install        |  123 EE
     2 | root <root>              | 2019-03-22 12:05 | Install        |    7 EE
     1 | System <unset>           | 2019-03-22 11:49 | Install        |  349   
history list
```

#### Dependencies for te-agent

Use the ID determined above with yum history. The ID value may be different on your system.  
Observe for lines with the following format:  
Dep-Install package\_name     @repository

```text
# yum history info 2
```

```text
Loaded plugins: product-id, search-disabled-repos, subscription-manager
Transaction ID : 2
Begin time     : Fri Mar 22 12:05:42 2019
Begin rpmdb    : 349:7ca0d7469815b183923749f849905184af03318c
End time       :            12:05:45 2019 (3 seconds)
End rpmdb      : 356:bbb6720e8cc5bb39393e4e40cfe7fec54488eec8
User           : root <root>
Return-Code    : Success
Command Line   : -y -q install te-agent
Transaction performed with:
    Installed     rpm-4.11.3-35.el7.x86_64                  @anaconda/7.6
    Installed     subscription-manager-1.21.10-2.el7.x86_64 @anaconda/7.6
    Installed     yum-3.4.3-161.el7.noarch                  @anaconda/7.6
    Installed     yum-metadata-parser-1.1.4-10.el7.x86_64   @anaconda/7.6
Packages Altered:
    Dep-Install apr-1.4.8-3.el7_4.1.x86_64     @rhel-7-server-rpms
    Dep-Install apr-util-1.5.2-6.el7.x86_64    @rhel-7-server-rpms
    Dep-Install c-ares-1.10.0-3.el7.x86_64     @rhel-7-server-rpms
    Dep-Install ldns-1.6.16-10.el7.x86_64      @rhel-7-server-rpms
    Dep-Install libosip2-3.5.0-1.el6.rf.x86_64 @thousandeyes
    Dep-Install libpcap-14:1.5.3-11.el7.x86_64 @rhel-7-server-rpms
    Install     te-agent-1.58.2-1.x86_64       @thousandeyes
Scriptlet output:
   1 Created symlink from /etc/systemd/system/multi-user.target.wants/te-agent.service to /usr/lib/systemd/system/te-agent.service.
history info
```

You may also find the [te-agent dependencies](https://yum.thousandeyes.com/RHEL/7/te-agent-dependencies.txt) list for RHEL 7 in the ThousandEyes official repository, available as a text file.

#### Dependencies for te-browserbot

Use the ID determined above with yum history. The ID value may be different on your system.  
Observe for lines with the following format:  
Dep-Install package\_name     @repository

```text
# yum history info 3
```

```text
Loaded plugins: product-id, search-disabled-repos, subscription-manager
Transaction ID : 3
Begin time     : Fri Mar 22 12:31:34 2019
Begin rpmdb    : 356:bbb6720e8cc5bb39393e4e40cfe7fec54488eec8
End time       :            12:32:04 2019 (30 seconds)
End rpmdb      : 479:f4b148a769eace9e301c46ad24028a567c12a67f
User           : root <root>
Return-Code    : Success
Command Line   : -y -q install te-browserbot
Transaction performed with:
    Installed     rpm-4.11.3-35.el7.x86_64                  @anaconda/7.6
    Installed     subscription-manager-1.21.10-2.el7.x86_64 @anaconda/7.6
    Installed     yum-3.4.3-161.el7.noarch                  @anaconda/7.6
    Installed     yum-metadata-parser-1.1.4-10.el7.x86_64   @anaconda/7.6
Packages Altered:
    Dep-Install GConf2-3.2.6-8.el7.x86_64                                     @rhel-7-server-rpms
    Dep-Install adwaita-cursor-theme-3.28.0-1.el7.noarch                      @rhel-7-server-rpms
    Dep-Install adwaita-icon-theme-3.28.0-1.el7.noarch                        @rhel-7-server-rpms
    Dep-Install at-3.1.13-24.el7.x86_64                                       @rhel-7-server-rpms
    Dep-Install at-spi2-atk-2.26.2-1.el7.x86_64                               @rhel-7-server-rpms
    Dep-Install at-spi2-core-2.28.0-1.el7.x86_64                              @rhel-7-server-rpms
    Dep-Install atk-2.28.1-1.el7.x86_64                                       @rhel-7-server-rpms
    Dep-Install avahi-libs-0.6.31-19.el7.x86_64                               @rhel-7-server-rpms
    Dep-Install bc-1.06.95-13.el7.x86_64                                      @rhel-7-server-rpms
    Dep-Install cairo-1.15.12-3.el7.x86_64                                    @rhel-7-server-rpms
    Dep-Install cairo-gobject-1.15.12-3.el7.x86_64                            @rhel-7-server-rpms
    Dep-Install colord-libs-1.3.4-1.el7.x86_64                                @rhel-7-server-rpms
    Dep-Install copy-jdk-configs-3.3-10.el7_5.noarch                          @rhel-7-server-rpms
    Dep-Install cups-client-1:1.6.3-35.el7.x86_64                             @rhel-7-server-rpms
    Dep-Install cups-libs-1:1.6.3-35.el7.x86_64                               @rhel-7-server-rpms
    Dep-Install dconf-0.28.0-4.el7.x86_64                                     @rhel-7-server-rpms
    Dep-Install dejavu-fonts-common-2.33-6.el7.noarch                         @rhel-7-server-rpms
    Dep-Install dejavu-sans-fonts-2.33-6.el7.noarch                           @rhel-7-server-rpms
    Dep-Install ed-1.9-4.el7.x86_64                                           @rhel-7-server-rpms
    Dep-Install flac-libs-1.3.0-5.el7_1.x86_64                                @rhel-7-server-rpms
    Dep-Install fontconfig-2.13.0-4.3.el7.x86_64                              @rhel-7-server-rpms
    Dep-Install fontpackages-filesystem-1.44-8.el7.noarch                     @rhel-7-server-rpms
    Dep-Install fribidi-1.0.2-1.el7.x86_64                                    @rhel-7-server-rpms
    Dep-Install gdk-pixbuf2-2.36.12-3.el7.x86_64                              @rhel-7-server-rpms
    Dep-Install giflib-4.1.6-9.el7.x86_64                                     @rhel-7-server-rpms
    Dep-Install glib-networking-2.56.1-1.el7.x86_64                           @rhel-7-server-rpms
    Dep-Install gnutls-3.3.29-9.el7_6.x86_64                                  @rhel-7-server-rpms
    Dep-Install graphite2-1.3.10-1.el7_3.x86_64                               @rhel-7-server-rpms
    Dep-Install gsettings-desktop-schemas-3.28.0-2.el7.x86_64                 @rhel-7-server-rpms
    Dep-Install gsm-1.0.13-11.el7.x86_64                                      @rhel-7-server-rpms
    Dep-Install gtk-update-icon-cache-3.22.30-3.el7.x86_64                    @rhel-7-server-rpms
    Dep-Install gtk2-2.24.31-1.el7.x86_64                                     @rhel-7-server-rpms
    Dep-Install gtk3-3.22.30-3.el7.x86_64                                     @rhel-7-server-rpms
    Dep-Install harfbuzz-1.7.5-2.el7.x86_64                                   @rhel-7-server-rpms
    Dep-Install hicolor-icon-theme-0.12-7.el7.noarch                          @rhel-7-server-rpms
    Dep-Install jasper-libs-1.900.1-33.el7.x86_64                             @rhel-7-server-rpms
    Dep-Install java-1.7.0-openjdk-1:1.7.0.211-2.6.17.1.el7_6.x86_64          @rhel-7-server-rpms
    Dep-Install java-1.7.0-openjdk-headless-1:1.7.0.211-2.6.17.1.el7_6.x86_64 @rhel-7-server-rpms
    Dep-Install javapackages-tools-3.4.1-11.el7.noarch                        @rhel-7-server-rpms
    Dep-Install jbigkit-libs-2.0-11.el7.x86_64                                @rhel-7-server-rpms
    Dep-Install json-glib-1.4.2-2.el7.x86_64                                  @rhel-7-server-rpms
    Dep-Install lcms2-2.6-3.el7.x86_64                                        @rhel-7-server-rpms
    Dep-Install libICE-1.0.9-9.el7.x86_64                                     @rhel-7-server-rpms
    Dep-Install libSM-1.2.2-2.el7.x86_64                                      @rhel-7-server-rpms
    Dep-Install libX11-1.6.5-2.el7.x86_64                                     @rhel-7-server-rpms
    Dep-Install libX11-common-1.6.5-2.el7.noarch                              @rhel-7-server-rpms
    Dep-Install libXScrnSaver-1.2.2-6.1.el7.x86_64                            @rhel-7-server-rpms
    Dep-Install libXau-1.0.8-2.1.el7.x86_64                                   @rhel-7-server-rpms
    Dep-Install libXcomposite-0.4.4-4.1.el7.x86_64                            @rhel-7-server-rpms
    Dep-Install libXcursor-1.1.15-1.el7.x86_64                                @rhel-7-server-rpms
    Dep-Install libXdamage-1.1.4-4.1.el7.x86_64                               @rhel-7-server-rpms
    Dep-Install libXdmcp-1.1.2-6.el7.x86_64                                   @rhel-7-server-rpms
    Dep-Install libXext-1.3.3-3.el7.x86_64                                    @rhel-7-server-rpms
    Dep-Install libXfixes-5.0.3-1.el7.x86_64                                  @rhel-7-server-rpms
    Dep-Install libXfont2-2.0.3-1.el7.x86_64                                  @rhel-7-server-rpms
    Dep-Install libXft-2.3.2-2.el7.x86_64                                     @rhel-7-server-rpms
    Dep-Install libXi-1.7.9-1.el7.x86_64                                      @rhel-7-server-rpms
    Dep-Install libXinerama-1.1.3-2.1.el7.x86_64                              @rhel-7-server-rpms
    Dep-Install libXmu-1.1.2-2.el7.x86_64                                     @rhel-7-server-rpms
    Dep-Install libXrandr-1.5.1-2.el7.x86_64                                  @rhel-7-server-rpms
    Dep-Install libXrender-0.9.10-1.el7.x86_64                                @rhel-7-server-rpms
    Dep-Install libXt-1.1.5-3.el7.x86_64                                      @rhel-7-server-rpms
    Dep-Install libXtst-1.2.3-1.el7.x86_64                                    @rhel-7-server-rpms
    Dep-Install libXxf86vm-1.1.4-1.el7.x86_64                                 @rhel-7-server-rpms
    Dep-Install libasyncns-0.8-7.el7.x86_64                                   @rhel-7-server-rpms
    Dep-Install libepoxy-1.5.2-1.el7.x86_64                                   @rhel-7-server-rpms
    Dep-Install liberation-fonts-common-1:1.07.2-16.el7.noarch                @rhel-7-server-rpms
    Dep-Install liberation-sans-fonts-1:1.07.2-16.el7.noarch                  @rhel-7-server-rpms
    Dep-Install libfontenc-1.1.3-3.el7.x86_64                                 @rhel-7-server-rpms
    Dep-Install libglvnd-1:1.0.1-0.8.git5baa1e5.el7.x86_64                    @rhel-7-server-rpms
    Dep-Install libglvnd-egl-1:1.0.1-0.8.git5baa1e5.el7.x86_64                @rhel-7-server-rpms
    Dep-Install libglvnd-glx-1:1.0.1-0.8.git5baa1e5.el7.x86_64                @rhel-7-server-rpms
    Dep-Install libgusb-0.2.9-1.el7.x86_64                                    @rhel-7-server-rpms
    Dep-Install libjpeg-turbo-1.2.90-6.el7.x86_64                             @rhel-7-server-rpms
    Dep-Install libmodman-2.0.1-8.el7.x86_64                                  @rhel-7-server-rpms
    Dep-Install libogg-2:1.3.0-7.el7.x86_64                                   @rhel-7-server-rpms
    Dep-Install libproxy-0.4.11-11.el7.x86_64                                 @rhel-7-server-rpms
    Dep-Install libsndfile-1.0.25-10.el7.x86_64                               @rhel-7-server-rpms
    Dep-Install libsoup-2.62.2-2.el7.x86_64                                   @rhel-7-server-rpms
    Dep-Install libthai-0.1.14-9.el7.x86_64                                   @rhel-7-server-rpms
    Dep-Install libtiff-4.0.3-27.el7_3.x86_64                                 @rhel-7-server-rpms
    Dep-Install libusbx-1.0.21-1.el7.x86_64                                   @rhel-7-server-rpms
    Dep-Install libvorbis-1:1.3.3-8.el7.1.x86_64                              @rhel-7-server-rpms
    Dep-Install libwayland-client-1.15.0-1.el7.x86_64                         @rhel-7-server-rpms
    Dep-Install libwayland-cursor-1.15.0-1.el7.x86_64                         @rhel-7-server-rpms
    Dep-Install libwayland-egl-1.15.0-1.el7.x86_64                            @rhel-7-server-rpms
    Dep-Install libwayland-server-1.15.0-1.el7.x86_64                         @rhel-7-server-rpms
    Dep-Install libxcb-1.13-1.el7.x86_64                                      @rhel-7-server-rpms
    Dep-Install libxkbcommon-0.7.1-1.el7.x86_64                               @rhel-7-server-rpms
    Dep-Install libxkbfile-1.0.9-3.el7.x86_64                                 @rhel-7-server-rpms
    Dep-Install libxshmfence-1.2-1.el7.x86_64                                 @rhel-7-server-rpms
    Dep-Install m4-1.4.16-10.el7.x86_64                                       @rhel-7-server-rpms
    Dep-Install mailx-12.5-19.el7.x86_64                                      @rhel-7-server-rpms
    Dep-Install mesa-libEGL-18.0.5-4.el7_6.x86_64                             @rhel-7-server-rpms
    Dep-Install mesa-libGL-18.0.5-4.el7_6.x86_64                              @rhel-7-server-rpms
    Dep-Install mesa-libgbm-18.0.5-4.el7_6.x86_64                             @rhel-7-server-rpms
    Dep-Install mesa-libglapi-18.0.5-4.el7_6.x86_64                           @rhel-7-server-rpms
    Dep-Install nettle-2.7.1-8.el7.x86_64                                     @rhel-7-server-rpms
    Dep-Install pango-1.42.4-1.el7.x86_64                                     @rhel-7-server-rpms
    Dep-Install patch-2.7.1-10.el7_5.x86_64                                   @rhel-7-server-rpms
    Dep-Install pcsc-lite-libs-1.8.8-8.el7.x86_64                             @rhel-7-server-rpms
    Dep-Install pixman-0.34.0-1.el7.x86_64                                    @rhel-7-server-rpms
    Dep-Install psmisc-22.20-15.el7.x86_64                                    @rhel-7-server-rpms
    Dep-Install pulseaudio-libs-10.0-5.el7.x86_64                             @rhel-7-server-rpms
    Dep-Install python-javapackages-3.4.1-11.el7.noarch                       @rhel-7-server-rpms
    Dep-Install redhat-lsb-core-4.1-27.el7.x86_64                             @rhel-7-server-rpms
    Dep-Install redhat-lsb-submod-security-4.1-27.el7.x86_64                  @rhel-7-server-rpms
    Dep-Install rest-0.8.1-2.el7.x86_64                                       @rhel-7-server-rpms
    Dep-Install spax-1.5.2-13.el7.x86_64                                      @rhel-7-server-rpms
    Install     te-browserbot-1.84.2-1.x86_64                                 @thousandeyes
    Dep-Install te-chromium-68.0.3440.83-1~centos7.x86_64                     @thousandeyes
    Dep-Install time-1.7-45.el7.x86_64                                        @rhel-7-server-rpms
    Dep-Install trousers-0.3.14-2.el7.x86_64                                  @rhel-7-server-rpms
    Dep-Install ttmkfdir-3.0.9-42.el7.x86_64                                  @rhel-7-server-rpms
    Dep-Install tzdata-java-2018i-1.el7.noarch                                @rhel-7-server-rpms
    Dep-Install wget-1.14-18.el7.x86_64                                       @rhel-7-server-rpms
    Dep-Install xkeyboard-config-2.24-1.el7.noarch                            @rhel-7-server-rpms
    Dep-Install xorg-x11-font-utils-1:7.5-21.el7.x86_64                       @rhel-7-server-rpms
    Dep-Install xorg-x11-fonts-Type1-7.5-9.el7.noarch                         @rhel-7-server-rpms
    Dep-Install xorg-x11-server-Xvfb-1.20.1-5.3.el7_6.x86_64                  @rhel-7-server-optional-rpms
    Dep-Install xorg-x11-server-common-1.20.1-5.3.el7_6.x86_64                @rhel-7-server-rpms
    Dep-Install xorg-x11-xauth-1:1.0.9-1.el7.x86_64                           @rhel-7-server-rpms
    Dep-Install xorg-x11-xkb-utils-7.7-14.el7.x86_64                          @rhel-7-server-rpms
Scriptlet output:
   1 Created symlink from /etc/systemd/system/multi-user.target.wants/te-browserbot.service to /usr/lib/systemd/system/te-browserbot.service.
history info
```

You may also find the [te-browserbot dependencies](https://yum.thousandeyes.com/RHEL/7/) list for RHEL 7 in the ThousandEyes official repository, available as a txt file.

### Redhat Knowledgebase reference

[How to register and subscribe a system to the Red Hat Customer Portal using Red Hat Subscription-Manager](https://access.redhat.com/solutions/253273)

