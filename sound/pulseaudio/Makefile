#
# Copyright (C) 2011 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=pulseaudio
PKG_VERSION:=1.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://freedesktop.org/software/pulseaudio/releases/
PKG_MD5SUM:=e5749151dbbaa7e928793f020bf1208b

PKG_BUILD_DIR=$(BUILD_DIR)/$(PKG_NAME)-$(BUILD_VARIANT)/$(PKG_NAME)-$(PKG_VERSION)
PKG_BUILD_DEPENDS:=intltool/host

PKG_FIXUP = libtool
PKG_INSTALL = 1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

define Package/pulseaudio/Default
  SECTION:=sound
  CATEGORY:=Sound
  SUBMENU:=pulseaudio
  URL:=http://www.pulseaudio.org
endef

define Package/libpulse/Default
  $(call Package/pulseaudio/Default)
  DEPENDS:=+libspeexdsp +libsndfile +libltdl +libpthread \
	+librt +alsa-lib +libjson $(ICONV_DEPENDS) $(INTL_DEPENDS) \
	@!UCLIBC_VERSION_0_9_30_1 @!UCLIBC_VERSION_0_9_30_2 \
	@!UCLIBC_VERSION_0_9_30_3
  TITLE:=library
endef

define Package/pulseaudio/Default/description
  PulseAudio (formerly Polypaudio) is a cross-platform, networked sound server.
endef

define Package/libpulse
  $(call Package/libpulse/Default)
  VARIANT:=nodbus
endef

define Package/libpulse/description
  $(call Package/pulseaudio/Default/description)
  This package contains the libraries.
endef

define Package/libpulse-dbus
  $(call Package/libpulse/Default)
  DEPENDS+=+dbus
  TITLE+= (compiled against dbus)
  VARIANT:=dbus
endef

define Package/libpulse-dbus/description
  $(call Package/pulseaudio/Default/description)
  This package contains the libraries compiled against dbus.
endef

define Package/pulseaudio-daemon
  $(call Package/pulseaudio/Default)
  TITLE:=sound daemon
  DEPENDS:=+(!PACKAGE_libpulse-dbus):libpulse
endef

define Package/pulseaudio-daemon/description
  $(call Package/pulseaudio/Default/description)
  This package contains the daemon.
endef

define Package/pulseaudio-daemon/conffiles
/etc/pulse/client.conf
/etc/pulse/daemon.conf
/etc/pulse/default.pa
/etc/pulse/system.pa
endef

define Package/pulseaudio-profiles
  $(call Package/pulseaudio/Default)
  DEPENDS:=+pulseaudio-daemon
  TITLE:=profiles
endef

define Package/pulseaudio-profiles/description
  PulseAudio profiles
endef

define Package/pulseaudio-tools
  $(call Package/pulseaudio/Default)
  DEPENDS:=+(!PACKAGE_libpulse-dbus):libpulse
  TITLE:=tools
endef

define Package/pulseaudio-tools/description
  PulseAudio tools:
  pacat, pacmd, pactl, pamon, paplay, parec, parecord, pasuspender
endef

CONFIGURE_ARGS += \
	--with-system-user=root \
	--with-system-group=root \
	--with-access-group=root \
	--with-database=simple \
	--without-caps \
	--disable-hal \
	--disable-tcpwrap \
	--disable-nls \
	--disable-manpages \
	--disable-oss-output \
	--disable-oss-wrapper \
	--disable-samplerate \
	--disable-per-user-esound-socket \
	--disable-solaris \
	--disable-glib2 \
	--disable-jack \
	--disable-asyncns \
	--disable-avahi \
	--disable-gconf \
	--disable-bluez \
	--disable-dbus \
	--disable-lirc \
	--disable-alsa \
	--disable-udev \
	--disable-openssl \

# enabling dbus links the libararies against dbus
ifeq ($(BUILD_VARIANT), dbus)
CONFIGURE_ARGS += --enable-dbus
endif

# add additional features if module is enabled
ifneq ($(CONFIG_PACKAGE_pulseaudio-mod-alsa-card),)
  CONFIGURE_ARGS+= --enable-alsa
endif
ifneq ($(CONFIG_PACKAGE_pulseaudio-mod-alsa-sink),)
  CONFIGURE_ARGS+= --enable-alsa
endif
ifneq ($(CONFIG_PACKAGE_pulseaudio-mod-alsa-source),)
  CONFIGURE_ARGS+= --enable-alsa
endif
ifneq ($(CONFIG_PACKAGE_pulseaudio-mod-bluetooth-device),)
  CONFIGURE_ARGS+= --enable-bluez
endif
ifneq ($(CONFIG_PACKAGE_pulseaudio-mod-bluetooth-discover),)
  CONFIGURE_ARGS+= --enable-bluez
endif
ifneq ($(CONFIG_PACKAGE_pulseaudio-mod-bluetooth-proximity),)
  CONFIGURE_ARGS+= --enable-bluez
endif
ifneq ($(CONFIG_PACKAGE_pulseaudio-mod-lirc),)
  CONFIGURE_ARGS+= --enable-lirc
endif
ifneq ($(CONFIG_PACKAGE_pulseaudio-mod-oss),)
  CONFIGURE_ARGS+= --enable-oss-output --enable-oss-wrapper
endif
ifneq ($(CONFIG_PACKAGE_pulseaudio-mod-raop-discover),)
  CONFIGURE_ARGS+= --enable-avahi --enable-openssl
endif
ifneq ($(CONFIG_PACKAGE_pulseaudio-mod-raop-sink),)
  CONFIGURE_ARGS+= --enable-openssl
endif
ifneq ($(CONFIG_PACKAGE_pulseaudio-mod-udev-detect),)
  CONFIGURE_ARGS+= --enable-udev
endif
ifneq ($(CONFIG_PACKAGE_pulseaudio-mod-zeroconf-discover),)
  CONFIGURE_ARGS+= --enable-avahi
endif
ifneq ($(CONFIG_PACKAGE_pulseaudio-mod-zeroconf-publish),)
  CONFIGURE_ARGS+= --enable-avahi
endif


CONFIGURE_VARS += \
	PKG_CONFIG_LIBDIR="$(STAGING_DIR)/usr/lib/pkgconfig"

#SUPP_LIBS:=-L$(INTL_PREFIX)/lib -L$(ICONV_PREFIX)/lib
TARGET_CFLAGS += -std=gnu99

define Build/InstallDev
	$(INSTALL_DIR) \
		$(1)/usr/lib/pkgconfig \
		$(1)/usr/include/pulse \
		$(1)/usr/lib
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/include/pulse/* \
		$(1)/usr/include/pulse
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/pkgconfig/*.pc \
		$(1)/usr/lib/pkgconfig
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/*.so* \
		$(1)/usr/lib/
endef

define Package/libpulse/install
	$(INSTALL_DIR) \
		$(1)/usr/lib

	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/*.so* \
		$(1)/usr/lib/
endef

define Package/libpulse-dbus/install
	$(INSTALL_DIR) \
		$(1)/usr/lib \
		$(1)/etc/dbus-1/system.d

	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/*.so* \
		$(1)/usr/lib/

	$(INSTALL_CONF) \
		$(PKG_INSTALL_DIR)/etc/dbus-1/system.d/pulseaudio-system.conf \
		$(1)/etc/dbus-1/system.d/pulseaudio-system.conf

	$(SED) \
		's/user="pulse"/user="root"/' \
		$(1)/etc/dbus-1/system.d/pulseaudio-system.conf
endef

define Package/pulseaudio-daemon/install
	$(INSTALL_DIR) \
		$(1)/etc/pulse \
		$(1)/etc/init.d \
		$(1)/usr/bin

	$(INSTALL_BIN) \
		$(PKG_INSTALL_DIR)/usr/bin/pulseaudio \
		$(1)/usr/bin/pulseaudio

	$(INSTALL_BIN) \
		./files/pulseaudio.init \
		$(1)/etc/init.d/pulseaudio

	$(INSTALL_CONF) \
		$(PKG_INSTALL_DIR)/etc/pulse/* \
		$(1)/etc/pulse
endef

define Package/pulseaudio-modules-common/install
	$(INSTALL_DIR) \
		$(1)/usr/lib/pulse-$(PKG_VERSION)/modules \

	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/pulse-$(PKG_VERSION)/modules/lib*.so \
		$(1)/usr/lib/

	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/pulse-$(PKG_VERSION)/modules/module*.so \
		$(1)/usr/lib/pulse-$(PKG_VERSION)/modules/

endef

define Package/pulseaudio-profiles/install
	$(INSTALL_DIR) \
		$(1)/usr/share/pulseaudio/alsa-mixer/paths \
		$(1)/usr/share/pulseaudio/alsa-mixer/profile-sets

	$(INSTALL_CONF) \
		$(PKG_INSTALL_DIR)/usr/share/pulseaudio/alsa-mixer/paths/* \
		$(1)/usr/share/pulseaudio/alsa-mixer/paths

	$(INSTALL_CONF) \
		$(PKG_INSTALL_DIR)/usr/share/pulseaudio/alsa-mixer/profile-sets/* \
		$(1)/usr/share/pulseaudio/alsa-mixer/profile-sets
endef

define Package/pulseaudio-tools/install
	$(INSTALL_DIR) \
		$(1)/usr/bin

	$(INSTALL_BIN) \
		$(PKG_INSTALL_DIR)/usr/bin/pa* \
		$(1)/usr/bin/
endef

# 1: plugin name
# 2: plugin title/description
# 3: modules
# 4: libs
# 5: extra dependency
# 5: configure flags
define BuildPlugin
  PKG_CONFIG_DEPENDS+= CONFIG_PACKAGE_pulseaudio-mod-$(1)

  define Package/pulseaudio-mod-$(1)
  $$(call Package/pulseaudio/Default)
    TITLE:=$(2)
    DEPENDS:= pulseaudio-daemon $(5)
  endef

  define Package/pulseaudio-mod-$(1)/install
	$(INSTALL_DIR) $$(1)/usr/lib/pulse-$(PKG_VERSION)/modules
	for m in $(3); do \
		$(CP) \
		    $(PKG_INSTALL_DIR)/usr/lib/pulse-$(PKG_VERSION)/modules/module-$$$$$$$${m}.so \
			$$(1)/usr/lib/pulse-$(PKG_VERSION)/modules/ ; \
	done
	for l in $(4); do \
		$(CP) \
		    $(PKG_INSTALL_DIR)/usr/lib/pulse-$(PKG_VERSION)/modules/lib$$$$$$$${l}.so \
			$$(1)/usr/lib/pulse-$(PKG_VERSION)/modules/ ; \
	done
  endef

  $$(eval $$(call BuildPackage,pulseaudio-mod-$(1)))
endef

$(eval $(call BuildPackage,libpulse))
$(eval $(call BuildPackage,libpulse-dbus))
$(eval $(call BuildPackage,pulseaudio-daemon))
$(eval $(call BuildPackage,pulseaudio-tools))
$(eval $(call BuildPackage,pulseaudio-profiles))

# modules:
#$(eval $(call BuildPlugin,NAME,DESCRIPTION,MODULES,LIBS,DEPENDENCIES))
$(eval $(call BuildPlugin,alsa-card,ALSA Card,alsa-card,alsa-util,+alsa-lib))
$(eval $(call BuildPlugin,alsa-sink,ALSA Sink,alsa-sink,alsa-util,+alsa-lib))
$(eval $(call BuildPlugin,alsa-source,ALSA Source,alsa-source,alsa-util,+alsa-lib))
$(eval $(call BuildPlugin,always-sink,Always keeps at least one sink loaded,always-sink,,))
$(eval $(call BuildPlugin,augment-properties,Information on stream properties,augment-properties,,))
$(eval $(call BuildPlugin,bluetooth-device,Bluetooth audio sink and source,bluetooth-device,,+bluez-libs))
$(eval $(call BuildPlugin,bluetooth-discover,Detect bluetooth audio devices,bluetooth-discover,,+bluez-libs))
$(eval $(call BuildPlugin,bluetooth-proximity,Bluetooth Proximity Volume Control,bluetooth-proximity,,+bluez-libs))
$(eval $(call BuildPlugin,card-restore,Automatically restore profile of cards,card-restore,,))
$(eval $(call BuildPlugin,cli-protocol-tcp,CLI protocol over TCP,cli-protocol-tcp,protocol-cli cli,))
$(eval $(call BuildPlugin,cli-protocol-unix,CLI protocol over UDP,cli-protocol-unix,protocol-cli cli,))
$(eval $(call BuildPlugin,cli,Command line interface,cli,cli,))
$(eval $(call BuildPlugin,combine,Compatibility module,combine,,))
$(eval $(call BuildPlugin,combine-sink,Combine multiple sinks to one,combine-sink,,))
$(eval $(call BuildPlugin,console-kit,Create client for ConsoleKit sessions,console-kit,,))
$(eval $(call BuildPlugin,cork-music-on-phone,Mute music on phone calls,cork-music-on-phone,,))
$(eval $(call BuildPlugin,dbus-protocol,D-Bus interface,dbus-protocol,,))
$(eval $(call BuildPlugin,default-device-restore,Restore default sink and source,default-device-restore,,))
$(eval $(call BuildPlugin,detect,Detect audio hardware,detect,,))
$(eval $(call BuildPlugin,device-manager,Keep track of devices and prioritise,device-manager,protocol-native,))
$(eval $(call BuildPlugin,device-restore,Automatically restore state of devices,device-restore,protocol-native,))
$(eval $(call BuildPlugin,echo-cancel,Echo Cancellation,echo-cancel,,))
$(eval $(call BuildPlugin,equalizer-sink,General Purpose Equalizer,equalizer-sink,,+fftw3f))
$(eval $(call BuildPlugin,esound-compat-spawnfd,ESOUND compatibility module,esound-compat-spawnfd,,))
$(eval $(call BuildPlugin,esound-compat-spawnpid,ESOUND compatibility module,esound-compat-spawnpid,,))
$(eval $(call BuildPlugin,esound-protocol-tcp,ESOUND protocol over TCP,esound-protocol-tcp,protocol-esound,))
$(eval $(call BuildPlugin,esound-protocol-unix,ESOUND protocol over UDP,esound-protocol-unix,protocol-esound,))
$(eval $(call BuildPlugin,esound-sink,ESOUND Sink,esound-sink,,))
$(eval $(call BuildPlugin,filter-apply,Load filter sinks automatically when needed,filter-apply,,))
$(eval $(call BuildPlugin,filter-heuristics,Detect when filters are desirable,filter-heuristics,,))
$(eval $(call BuildPlugin,gconf,GConf Adapter,gconf,,+libgconf2))
$(eval $(call BuildPlugin,http-protocol-tcp,HTTP protocol over TCP,http-protocol-tcp,protocol-http,))
$(eval $(call BuildPlugin,http-protocol-unix,HTTP protocol over UDP,http-protocol-unix,protocol-http,))
$(eval $(call BuildPlugin,intended-roles,Set device of streams based on roles,intended-roles,,))
#$(eval $(call BuildPlugin,lirc,LIRC volume control,lirc,,+lirc-lib))
$(eval $(call BuildPlugin,loopback,Loopback from source to sink,loopback,,))
$(eval $(call BuildPlugin,match,Playback stream expression matching module,match,,))
$(eval $(call BuildPlugin,mmkbd-evdev,Multimedia keyboard support via Linux evdev,mmkbd-evdev,,))
$(eval $(call BuildPlugin,native-protocol-fd,Native protocol over file descriptor,native-protocol-fd,protocol-native,))
$(eval $(call BuildPlugin,native-protocol-tcp,Native protocol over TCP,native-protocol-tcp,protocol-native,))
$(eval $(call BuildPlugin,native-protocol-unix,Native protocol over UDP,native-protocol-unix,protocol-native,))
$(eval $(call BuildPlugin,null-sink,Clocked NULL sink,null-sink,,))
$(eval $(call BuildPlugin,null-source,Clocked NULL source,null-source,,))
$(eval $(call BuildPlugin,oss,OSS Sink/Source,oss,,))
$(eval $(call BuildPlugin,pipe-sink,UNIX pipe sink,pipe-sink,,))
$(eval $(call BuildPlugin,pipe-source,UNIX pipe source,pipe-source,,))
$(eval $(call BuildPlugin,position-event-sounds,Position sounds on widget position,position-event-sounds,,))
$(eval $(call BuildPlugin,raop-discover,ZeroConf RAOP devices discovery,raop-discover,avahi-wrap,+libavahi-client))
$(eval $(call BuildPlugin,raop-sink,RAOP Sink,raop-sink,raop rtp,+libopenssl))
$(eval $(call BuildPlugin,remap-sink,Virtual channel remapping sink,remap-sink,,))
$(eval $(call BuildPlugin,rescue-streams,Move streams to default sink/source,rescue-streams,,))
$(eval $(call BuildPlugin,rtp-recv,Receive data from network via RTP/SAP/SDP,rtp-recv,,))
$(eval $(call BuildPlugin,rtp-send,Send data over network via RTP/SAP/SDP,rtp-send,rtp,))
$(eval $(call BuildPlugin,rygel-media-server,UPnP MediaServer Plugin for Rygel,rygel-media-server,protocol-http,))
$(eval $(call BuildPlugin,simple-protocol-tcp,Simple protocol over TCP,simple-protocol-tcp,protocol-simple,))
$(eval $(call BuildPlugin,simple-protocol-unix,Simple protocol over UDP,simple-protocol-unix,protocol-simple,))
$(eval $(call BuildPlugin,sine,Sine wave generator,sine,,))
$(eval $(call BuildPlugin,sine-source,Sine wave generator source,sine-source,,))
$(eval $(call BuildPlugin,stream-restore,Automatically restore stream state,stream-restore,protocol-native,))
$(eval $(call BuildPlugin,suspend-on-idle,Suspend sink/source if idle for too long,suspend-on-idle,,))
$(eval $(call BuildPlugin,switch-on-connect,Switch to newly connected source/sink,switch-on-connect,,))
$(eval $(call BuildPlugin,tunnel-sink,Tunnel module for sinks,tunnel-sink,,))
$(eval $(call BuildPlugin,tunnel-source,Tunnel module for sources,tunnel-source,,))
$(eval $(call BuildPlugin,udev-detect,Detect available audio hardware,udev-detect,,+udev))
$(eval $(call BuildPlugin,virtual-sink,Virtual sink,virtual-sink,,))
$(eval $(call BuildPlugin,virtual-source,Virtual source,virtual-source,,))
$(eval $(call BuildPlugin,volume-restore,Compatibility module,volume-restore,,))
$(eval $(call BuildPlugin,zeroconf-discover,ZeroConf Service Discovery,zeroconf-discover,avahi-wrap,+libavahi-client))
$(eval $(call BuildPlugin,zeroconf-publish,ZeroConf Service Publisher,zeroconf-publish,avahi-wrap protocol-native,+libavahi-client))

# unsupported
#$(eval $(call BuildPlugin,hal-detect-compat,Compatibility module,hal-detect-compat,,))
#$(eval $(call BuildPlugin,hal-detect,Detect available audio hardware and load matching drivers,hal-detect,,))
#$(eval $(call BuildPlugin,jackdbus-detect,Adds JACK sink/source ports when JACK is started,jackdbus-detect,,))
#$(eval $(call BuildPlugin,jack-sink,JACK Sink,jack-sink,,))
#$(eval $(call BuildPlugin,jack-source,JACK Source,jack-source,,))
#$(eval $(call BuildPlugin,ladspa-sink,Virtual LADSPA sink,ladspa-sink,,))
#$(eval $(call BuildPlugin,x11-bell,X11 bell interceptor,x11-bell,,))
#$(eval $(call BuildPlugin,x11-cork-request,Synthesize X11 media key events when cork/uncork is requested,x11-cork-request,,))
#$(eval $(call BuildPlugin,x11-publish,X11 credential publisher,x11-publish,,))
#$(eval $(call BuildPlugin,x11-xsmp,X11 session management,x11-xsmp,,))
