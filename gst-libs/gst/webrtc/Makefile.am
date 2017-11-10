lib_LTLIBRARIES = libgstwebrtc-@GST_API_VERSION@.la

glib_enum_headers = dtlstransport.h icetransport.h rtptransceiver.h webrtc_fwd.h
glib_enum_define = GST_WEBRTC
glib_gen_prefix = gst_webrtc
glib_gen_basename = webrtc

built_sources = webrtc-enumtypes.c
built_headers = webrtc-enumtypes.h
BUILT_SOURCES = $(built_sources) $(built_headers)
CLEANFILES = $(BUILT_SOURCES)

libgstwebrtc_@GST_API_VERSION@_la_SOURCES = \
	dtlstransport.c \
	icetransport.c \
	rtcsessiondescription.c \
	rtpreceiver.c \
	rtpsender.c \
	rtptransceiver.c

nodist_libgstwebrtc_@GST_API_VERSION@_la_SOURCES = $(built_sources)

libgstwebrtc_@GST_API_VERSION@includedir = $(includedir)/gstreamer-@GST_API_VERSION@/gst/webrtc
libgstwebrtc_@GST_API_VERSION@include_HEADERS = \
	dtlstransport.h \
	icetransport.h \
	rtcsessiondescription.h \
	rtpreceiver.h \
	rtpsender.h \
	rtptransceiver.h \
	webrtc_fwd.h \
	webrtc.h

nodist_libgstwebrtc_@GST_API_VERSION@include_HEADERS = $(built_headers)

libgstwebrtc_@GST_API_VERSION@_la_CFLAGS = \
	-I$(top_builddir)/gst-libs \
	-I$(top_srcdir)/gst-libs \
	$(GST_PLUGINS_BASE_CFLAGS) \
	$(GST_BASE_CFLAGS) \
	$(GST_CFLAGS) \
	$(GST_SDP_CFLAGS)
libgstwebrtc_@GST_API_VERSION@_la_LIBADD = \
	$(GST_PLUGINS_BASE_LIBS) \
	$(GST_BASE_LIBS) \
	$(GST_LIBS) \
	$(GST_SDP_LIBS)
libgstwebrtc_@GST_API_VERSION@_la_LDFLAGS = \
	$(GST_LIB_LDFLAGS) \
	$(GST_ALL_LDFLAGS) \
	$(GST_LT_LDFLAGS)

include $(top_srcdir)/common/gst-glib-gen.mak

if HAVE_INTROSPECTION
BUILT_GIRSOURCES = GstWebRTC-@GST_API_VERSION@.gir

gir_headers=$(patsubst %,$(srcdir)/%, $(libgstwebrtc_@GST_API_VERSION@include_HEADERS))
gir_sources=$(patsubst %,$(srcdir)/%, $(libgstwebrtc_@GST_API_VERSION@_la_SOURCES))

GstWebRTC-@GST_API_VERSION@.gir: $(INTROSPECTION_SCANNER) libgstwebrtc-@GST_API_VERSION@.la
	$(AM_V_GEN)PKG_CONFIG_PATH="$(GST_PKG_CONFIG_PATH)" GI_SCANNER_DISABLE_CACHE=yes \
		GST_PLUGIN_SYSTEM_PATH_1_0="" GST_PLUGIN_PATH_1_0="" GST_REGISTRY_UPDATE=no \
		CPPFLAGS="$(CPPFLAGS)" CFLAGS="$(CFLAGS)" LDFLAGS="$(LDFLAGS)" CC="$(CC)" PKG_CONFIG="$(PKG_CONFIG)" DLLTOOL="$(DLLTOOL)" \
		$(INTROSPECTION_SCANNER) -v --namespace GstWebRTC \
		--nsversion=@GST_API_VERSION@ \
		--identifier-prefix=Gst \
		--symbol-prefix=gst \
		--warn-all \
		--c-include "gst/webrtc/webrtc.h" \
		-I$(top_builddir)/gst-libs \
		-I$(top_srcdir)/gst-libs \
		--add-include-path=`PKG_CONFIG_PATH="$(GST_PKG_CONFIG_PATH)" $(PKG_CONFIG) --variable=girdir gstreamer-@GST_API_VERSION@` \
		--add-include-path=`PKG_CONFIG_PATH="$(GST_PKG_CONFIG_PATH)" $(PKG_CONFIG) --variable=girdir gstreamer-base-@GST_API_VERSION@` \
		--add-include-path=`PKG_CONFIG_PATH="$(GST_PKG_CONFIG_PATH)" $(PKG_CONFIG) --variable=girdir gstreamer-sdp-@GST_API_VERSION@` \
		--library=libgstwebrtc-@GST_API_VERSION@.la \
		--include=Gst-@GST_API_VERSION@ \
		--include=GstBase-@GST_API_VERSION@ \
		--include=GstSdp-@GST_API_VERSION@ \
		--libtool="$(top_builddir)/libtool" \
		--pkg gstreamer-@GST_API_VERSION@ \
		--pkg gstreamer-base-@GST_API_VERSION@ \
		--pkg gstreamer-sdp-@GST_API_VERSION@ \
		--pkg-export gstreamer-webrtc-@GST_API_VERSION@ \
		--add-init-section="$(INTROSPECTION_INIT)" \
		-DGST_USE_UNSTABLE_API \
		--output $@ \
		$(gir_headers) \
		$(gir_sources)

# INTROSPECTION_GIRDIR/INTROSPECTION_TYPELIBDIR aren't the right place to
# install anything - we need to install inside our prefix.
girdir = $(datadir)/gir-1.0
gir_DATA = $(BUILT_GIRSOURCES)

typelibsdir = $(libdir)/girepository-1.0/

typelibs_DATA = $(BUILT_GIRSOURCES:.gir=.typelib)

%.typelib: %.gir $(INTROSPECTION_COMPILER)
	$(AM_V_GEN)PKG_CONFIG_PATH="$(GST_PKG_CONFIG_PATH)" \
		$(INTROSPECTION_COMPILER) \
		--includedir=$(srcdir) \
		--includedir=$(builddir) \
		--includedir=`PKG_CONFIG_PATH="$(GST_PKG_CONFIG_PATH)" $(PKG_CONFIG) --variable=girdir gstreamer-@GST_API_VERSION@` \
		--includedir=`PKG_CONFIG_PATH="$(GST_PKG_CONFIG_PATH)" $(PKG_CONFIG) --variable=girdir gstreamer-base-@GST_API_VERSION@` \
		--includedir=`PKG_CONFIG_PATH="$(GST_PKG_CONFIG_PATH)" $(PKG_CONFIG) --variable=girdir gstreamer-sdp-@GST_API_VERSION@` \
		$(INTROSPECTION_COMPILER_OPTS) $< -o $(@F)

CLEANFILES += $(BUILT_GIRSOURCES) $(typelibs_DATA)
endif
