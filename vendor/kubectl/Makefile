# Package details
export VENDOR = kubernetes
export PACKAGE_REPO_NAME = kubernetes
export DOWNLOAD_URL = https://storage.googleapis.com/kubernetes-release/release/v$(PACKAGE_VERSION)/bin/$(OS)/$(ARCH)/$(PACKAGE_NAME)
export APK_BUILD_TEMPLATE = APKBUILD.github-binary
export APKBUILD_DEPENDS += dpkg
export APKBUILD_INSTALL_SCRIPTS = $(PACKAGE_NAME).post-install $(PACKAGE_NAME).post-deinstall
export MAJOR_VERSION = latest
export INSTALL_DIR = /usr/share/${PACKAGE_NAME}/${MAJOR_VERSION}/bin

include ../../tasks/Makefile.vendor_includes

install:
	$(call download_binary)

test:
	$(PACKAGE_EXE) version --client | grep -F $(PACKAGE_VERSION)
