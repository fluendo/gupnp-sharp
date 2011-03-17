RAW_API=gupnp-api.raw
API=gupnp-api.xml
METADATA=gupnp.metadata

ASSEMBLY_NAME= gupnp-sharp
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

customs = Context.custom ServiceActionInfo.custom ServiceIntrospection.custom ServiceStateVariableInfo.custom 

MAINTAINERCLEANFILES = Makefile.in

CLEANFILES = gupnp-sharp.dll generated-stamp generated/*.cs $(API)

DISTCLEANFILES= AssemblyInfo.cs

noinst_DATA = gupnp-sharp.dll

all: generated-stamp gupnp-sharp.dll

$(RAW_API): $(customs)
	$(PARSER) $(top_srcdir)/sources/gupnp-sharp.sources

$(API): $(srcdir)/$(RAW_API) $(srcdir)/$(METADATA)
	cp $(srcdir)/$(RAW_API) $(API)
	chmod u+w $(API)
	$(FIXUP) --api=$(srcdir)/$(API) --metadata=$(srcdir)/$(METADATA)

generated-stamp: $(API)
	 $(CODEGEN) --generate $(srcdir)/$(API) $(GLIB_SHARP_CFLAGS) -I:$(top_builddir)/gssdp/gssdp-api.xml --outdir=generated --customdir=$(srcdir) --assembly-name=gupnp-sharp && touch generated-stamp

gupnp-sharp.dll: $(build_sources) generated-stamp
	$(CSC) -out:gupnp-sharp.dll -unsafe -target:library $(GLIB_SHARP_LIBS) -r:$(top_builddir)/gssdp/gssdp-sharp.dll \
	$(build_sources) $(GENERATED_SOURCES)

install-data-local:
	echo "$(GACUTIL) /i $(ASSEMBLY) /f /package gupnp-sharp-1.0 /root $(DESTDIR)$(libdir)";  \
        $(GACUTIL) /i $(ASSEMBLY) /f /package gupnp-sharp-1.0 /root $(DESTDIR)$(libdir) || exit 1;

uninstall-local:
	echo "$(GACUTIL) /u $(ASSEMBLY_NAME) /package gupnp-sharp-1.0 /root $(DESTDIR)$(libdir)"; \
        $(GACUTIL) /u $(ASSEMBLY_NAME) /package gupnp-sharp-1.0 /root $(DESTDIR)$(libdir) || exit 1;

EXTRA_DIST = $(RAW_API) $(sources) $(customs) $(METADATA) AssemblyInfo.cs.in gupnp-sharp.dll.config.in
