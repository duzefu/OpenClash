include $(TOPDIR)/rules.mk

PKG_NAME:=luci-app-openclash
PKG_VERSION:=0.47.088
PKG_MAINTAINER:=vernesong <https://github.com/vernesong/OpenClash>

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)/config
	config PACKAGE_kmod-inet-diag
	default y if PACKAGE_$(PKG_NAME)

	config PACKAGE_luci-compat
	default y if PACKAGE_$(PKG_NAME)

	config PACKAGE_kmod-nft-tproxy
	default y if PACKAGE_firewall4

	config PACKAGE_kmod-ipt-nat
	default y if ! PACKAGE_firewall4

	config PACKAGE_iptables-mod-tproxy
	default y if ! PACKAGE_firewall4

	config PACKAGE_iptables-mod-extra
	default y if ! PACKAGE_firewall4

	config PACKAGE_dnsmasq_full_ipset
	default y if ! PACKAGE_firewall4

	config PACKAGE_dnsmasq_full_nftset
	default y if PACKAGE_firewall4

	config PACKAGE_ipset
	default y if ! PACKAGE_firewall4

	config PACKAGE_luci-app-openclash_include_core
		bool "Include ClashMeta core in package"
		default y
		help
		  Download and include the ClashMeta (mihomo) core binary
		  during IPK compilation. If disabled, the core will be
		  downloaded at runtime on the device.
endef

define Package/$(PKG_NAME)
	CATEGORY:=LuCI
	SUBMENU:=3. Applications
	TITLE:=LuCI support for clash
	PKGARCH:=all
	DEPENDS:=+dnsmasq-full +bash +curl +ca-bundle +ip-full \
	+ruby +ruby-yaml +kmod-tun +unzip
	MAINTAINER:=vernesong
endef

define Package/$(PKG_NAME)/description
    A LuCI support for clash
endef

define Build/Prepare
	$(CP) $(CURDIR)/root $(PKG_BUILD_DIR)
	$(CP) $(CURDIR)/luasrc $(PKG_BUILD_DIR)
	$(foreach po,$(wildcard ${CURDIR}/po/zh-cn/*.po), \
		po2lmo $(po) $(PKG_BUILD_DIR)/$(patsubst %.po,%.lmo,$(notdir $(po)));)
	chmod 0755 $(PKG_BUILD_DIR)/root/etc/init.d/openclash
	chmod -R 0755 $(PKG_BUILD_DIR)/root/usr/share/openclash/
	mkdir -p $(PKG_BUILD_DIR)/root/etc/openclash/config
	mkdir -p $(PKG_BUILD_DIR)/root/etc/openclash/rule_provider
	mkdir -p $(PKG_BUILD_DIR)/root/etc/openclash/proxy_provider
	mkdir -p $(PKG_BUILD_DIR)/root/etc/openclash/core
	mkdir -p $(PKG_BUILD_DIR)/root/usr/share/openclash/backup/overwrite
	cp -f "$(PKG_BUILD_DIR)/root/etc/config/openclash" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash" >/dev/null 2>&1
	cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_rules.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_rules.list" >/dev/null 2>&1
	cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_rules_2.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_rules_2.list" >/dev/null 2>&1
	cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_hosts.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_hosts.list" >/dev/null 2>&1
	cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_fake_filter.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_fake_filter.list" >/dev/null 2>&1
	cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_domain_dns.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_domain_dns.list" >/dev/null 2>&1
	cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_domain_dns_policy.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_domain_dns_policy.list" >/dev/null 2>&1
	cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_proxy_server_dns_policy.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_proxy_server_dns_policy.list" >/dev/null 2>&1
	cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_fallback_filter.yaml" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_fallback_filter.yaml" >/dev/null 2>&1
	cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_sniffer.yaml" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_sniffer.yaml" >/dev/null 2>&1
	cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_localnetwork_ipv4.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_localnetwork_ipv4.list" >/dev/null 2>&1
	cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_localnetwork_ipv6.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_localnetwork_ipv6.list" >/dev/null 2>&1
	cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_chnroute_pass.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_chnroute_pass.list" >/dev/null 2>&1
	cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_chnroute6_pass.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_chnroute6_pass.list" >/dev/null 2>&1
	cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_firewall_rules.sh" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_firewall_rules.sh" >/dev/null 2>&1
	cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_overwrite.sh" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_overwrite.sh" >/dev/null 2>&1
	cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/overwrite/default" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/overwrite/default" >/dev/null 2>&1
	exit 0
endef

define Build/Configure
endef

# Map OpenWrt ARCH to ClashMeta core platform name
define GetClashArch
$(strip $(if $(findstring x86_64,$(ARCH)),linux-amd64-v1,\
  $(if $(findstring aarch64,$(ARCH)),linux-arm64,\
    $(if $(findstring arm,$(ARCH)),linux-armv7,\
      $(if $(findstring mips64,$(ARCH)),linux-mips64,\
        $(if $(findstring mipsel,$(ARCH)),linux-mipsle-softfloat,\
          $(if $(findstring mips,$(ARCH)),linux-mips-softfloat,\
            $(if $(findstring i386,$(ARCH)),linux-386,\
              $(if $(findstring loongarch64,$(ARCH)),linux-loong64,\
0))))))))))
endef

define Build/Compile
ifeq ($(CONFIG_PACKAGE_luci-app-openclash_include_core),y)
	$(eval CLASH_ARCH:=$(call GetClashArch))
	@if [ "$(CLASH_ARCH)" = "0" ]; then \
		echo "WARNING: Unsupported architecture $(ARCH) for ClashMeta core, skipping core download"; \
	else \
		echo "Downloading ClashMeta core for $(CLASH_ARCH)..."; \
		curl -SLk --connect-timeout 30 --retry 3 "https://raw.githubusercontent.com/vernesong/OpenClash/core/master/meta/clash-$(CLASH_ARCH).tar.gz" -o "$(PKG_BUILD_DIR)/clash_meta.tar.gz" || { echo "ERROR: Failed to download ClashMeta core"; exit 1; }; \
		gzip -t "$(PKG_BUILD_DIR)/clash_meta.tar.gz" || { echo "ERROR: Downloaded ClashMeta core is corrupted"; exit 1; }; \
		tar zxvfo "$(PKG_BUILD_DIR)/clash_meta.tar.gz" -C "$(PKG_BUILD_DIR)/root/etc/openclash/core/" || exit 1; \
		mv "$(PKG_BUILD_DIR)/root/etc/openclash/core/clash" "$(PKG_BUILD_DIR)/root/etc/openclash/core/clash_meta" || exit 1; \
		chmod 4755 "$(PKG_BUILD_DIR)/root/etc/openclash/core/clash_meta"; \
		"$(PKG_BUILD_DIR)/root/etc/openclash/core/clash_meta" -v >/dev/null 2>&1 || { echo "ERROR: ClashMeta core binary is not executable"; exit 1; }; \
		rm -f "$(PKG_BUILD_DIR)/clash_meta.tar.gz"; \
		echo "ClashMeta core downloaded successfully for $(CLASH_ARCH)"; \
	fi
endif
endef

define Package/$(PKG_NAME)/conffiles
endef

define Package/$(PKG_NAME)/preinst
#!/bin/sh
	if [ -f "/etc/config/openclash" ] && [ ! -f "/tmp/openclash.bak" ]; then
		cp -f "/etc/config/openclash" "/tmp/openclash.bak" >/dev/null 2>&1
		cp -rf "/etc/openclash" "/tmp/openclash" >/dev/null 2>&1
		cp -rf "/usr/share/openclash/ui" "/tmp/openclash_ui" >/dev/null 2>&1
		cp -rf "/www/luci-static/resources/openclash/pac" "/tmp/pac" >/dev/null 2>&1
	fi
	exit 0
endef

define Package/$(PKG_NAME)/postinst
#!/bin/sh
	exit 0
endef

define Package/$(PKG_NAME)/prerm
#!/bin/sh
	[ -n "$(pidof clash)" ] && /etc/init.d/openclash stop 2>/dev/null
	if [ -f "/etc/config/openclash" ] && [ ! -f "/tmp/openclash.bak" ]; then
		cp -f "/etc/config/openclash" "/tmp/openclash.bak" >/dev/null 2>&1
		cp -rf "/etc/openclash" "/tmp/openclash" >/dev/null 2>&1
		cp -rf "/usr/share/openclash/ui" "/tmp/openclash_ui" >/dev/null 2>&1
		cp -rf "/www/luci-static/resources/openclash/pac" "/tmp/pac" >/dev/null 2>&1
	fi
	exit 0
endef

define Package/$(PKG_NAME)/postrm
#!/bin/sh
	DEFAULT_DNSMASQ_CFGID="$$(uci -q show "dhcp.@dnsmasq[0]" | awk 'NR==1 {split($0, conf, /[.=]/); print conf[2]}' 2>/dev/null)"
	if [ -f "/tmp/etc/dnsmasq.conf.$DEFAULT_DNSMASQ_CFGID" ]; then
	   DNSMASQ_CONF_DIR="$$(awk -F '=' '/^conf-dir=/ {print $2}' "/tmp/etc/dnsmasq.conf.$DEFAULT_DNSMASQ_CFGID" 2>/dev/null)"
	else
	   DNSMASQ_CONF_DIR="/tmp/dnsmasq.d"
	fi
	DNSMASQ_CONF_DIR=$${DNSMASQ_CONF_DIR%*/}
	rm -rf /etc/openclash >/dev/null 2>&1
	rm -rf /etc/config/openclash >/dev/null 2>&1
	rm -rf /tmp/openclash.log >/dev/null 2>&1
	rm -rf /tmp/openclash_start.log >/dev/null 2>&1
	rm -rf /tmp/openclash_last_version >/dev/null 2>&1
	rm -rf /tmp/openclash.change >/dev/null 2>&1
	rm -rf /tmp/clash_last_version >/dev/null 2>&1
	rm -rf /usr/share/openclash >/dev/null 2>&1
	rm -rf ${DNSMASQ_CONF_DIR}/dnsmasq_openclash_custom_domain.conf >/dev/null 2>&1
	rm -rf ${DNSMASQ_CONF_DIR}/dnsmasq_openclash_chnroute_pass.conf >/dev/null 2>&1
	rm -rf ${DNSMASQ_CONF_DIR}/dnsmasq_openclash_chnroute6_pass.conf >/dev/null 2>&1
	rm -rf /tmp/etc/openclash >/dev/null 2>&1
	rm -rf /tmp/openclash_announcement >/dev/null 2>&1
	rm -rf /www/luci-static/resources/openclash >/dev/null 2>&1
	sed -i '/OpenClash Append/,/OpenClash Append End/d' "/usr/lib/lua/luci/model/network.lua" >/dev/null 2>&1
	sed -i '/.*kB maximum content size*/c\HTTP_MAX_CONTENT      = 1024*100		-- 100 kB maximum content size' /usr/lib/lua/luci/http.lua >/dev/null 2>&1
	sed -i '/.*kB maximum content size*/c\export let HTTP_MAX_CONTENT = 1024*100;		// 100 kB maximum content size' /usr/share/ucode/luci/http.uc >/dev/null 2>&1
	uci -q delete firewall.openclash
	uci -q commit firewall
	[ -f "/etc/config/ucitrack" ] && {
	uci -q delete ucitrack.@openclash[-1]
	uci -q commit ucitrack
	}
	rm -rf /tmp/luci-*
	exit 0
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/i18n
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/*.*.lmo $(1)/usr/lib/lua/luci/i18n/
	$(CP) $(PKG_BUILD_DIR)/root/* $(1)/
	$(CP) $(PKG_BUILD_DIR)/luasrc/* $(1)/usr/lib/lua/luci/
endef

$(eval $(call BuildPackage,$(PKG_NAME)))
