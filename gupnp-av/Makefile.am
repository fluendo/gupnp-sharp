RAW_API=gupnp-av-api.raw
API=gupnp-av-api.xml
METADATA=gupnp-av.metadata

ASSEMBLY_NAME= gupnp-av-sharp
ASSEMBLY= $(ASSEMBLY_NAME).dll

if WINBUILD
CODEGEN=gapi_codegen.exe
PARSER=gapi-parser.exe
FIXUP=gapi-fixup.exe
else
CODEGEN=gapi2-codegen
PARSER=gapi2-parser
FIXUP=gapi2-fixup
endif

sources = 

build_sources = $(addprefix $(srcdir)/, $(sources)) AssemblyInfo.cs

customs = 

MAINTAINERCLEANFILES = Makefile.in

CLEANFILES = gupnp-av-sharp.dll generated-stamp generated/*.cs $(API)

DISTCLEANFILES= AssemblyInfo.cs

noinst_DATA = gupnp-av-sharp.dll

all: generated-stamp gupnp-av-sharp.dll

$(RAW_API): $(customs)
	$(PARSER) $(top_srcdir)/sources/gupnp-av-sharp.sources

$(API): $(srcdir)/$(RAW_API) $(srcdir)/$(METADATA)
	cp $(srcdir)/$(RAW_API) $(API)
	chmod u+w $(API)
	$(FIXUP) --api=$(srcdir)/$(API) --metadata=$(srcdir)/$(METADATA)

generated-stamp: $(API)
	 $(CODEGEN) --generate $(srcdir)/$(API) $(GLIB_SHARP_CFLAGS) -I:$(top_builddir)/gssdp/gssdp-api.xml -I:$(top_builddir)/gupnp/gupnp-api.xml --outdir=generated --customdir=$(srcdir) --assembly-name=gupnp-av-sharp && touch generated-stamp

gupnp-av-sharp.dll: $(build_sources) generated-stamp
	$(CSC) -out:gupnp-av-sharp.dll -unsafe -target:library $(GLIB_SHARP_LIBS) -r:$(top_builddir)/gssdp/gssdp-sharp.dll -r:$(top_builddir)/gupnp/gupnp-sharp.dll \
	$(build_sources) $(GENERATED_SOURCES)

install-data-local:
	echo "$(GACUTIL) /i $(ASSEMBLY) /f /package gupnp-av-sharp-1.0 /root $(DESTDIR)$(libdir)";  \
        $(GACUTIL) /i $(ASSEMBLY) /f /package gupnp-av-sharp-1.0 /root $(DESTDIR)$(libdir) || exit 1;

uninstall-local:
	echo "$(GACUTIL) /u $(ASSEMBLY_NAME) /package gupnp-av-sharp-1.0 /root $(DESTDIR)$(libdir)"; \
        $(GACUTIL) /u $(ASSEMBLY_NAME) /package gupnp-av-sharp-1.0 /root $(DESTDIR)$(libdir) || exit 1;

EXTRA_DIST = $(RAW_API) $(sources) $(customs) $(METADATA) AssemblyInfo.cs.in gupnp-av-sharp.dll.config.in
