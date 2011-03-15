RAW_API=gssdp-api.raw
API=gssdp-api.xml
METADATA=gssdp.metadata

ASSEMBLY_NAME= gssdp-sharp
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

customs = Client.custom ResourceBrowser.custom ResourceGroup.custom 

MAINTAINERCLEANFILES = Makefile.in

CLEANFILES = gssdp-sharp.dll generated-stamp generated/*.cs $(API)

DISTCLEANFILES= AssemblyInfo.cs

noinst_DATA = gssdp-sharp.dll

all: generated-stamp gssdp-sharp.dll

$(RAW_API): $(customs)
	$(PARSER) $(top_srcdir)/sources/gssdp-sharp.sources

$(API): $(srcdir)/$(RAW_API) $(srcdir)/$(METADATA)
	cp $(srcdir)/$(RAW_API) $(API)
	chmod u+w $(API)
	$(FIXUP) --api=$(srcdir)/$(API) --metadata=$(srcdir)/$(METADATA)

generated-stamp: $(API)
	 $(CODEGEN) --generate $(srcdir)/$(API) $(GTK_SHARP_CFLAGS) --outdir=generated --customdir=$(srcdir) --assembly-name=gssdp-sharp && touch generated-stamp

gssdp-sharp.dll: $(build_sources) generated-stamp
	$(CSC) -out:gssdp-sharp.dll -unsafe -target:library $(GLIB_SHARP_LIBS) \
	$(build_sources) $(GENERATED_SOURCES)

install-data-local:
	echo "$(GACUTIL) /i $(ASSEMBLY) /f /package gssdp-sharp-1.0 /root $(DESTDIR)$(libdir)";  \
        $(GACUTIL) /i $(ASSEMBLY) /f /package gssdp-sharp-1.0 /root $(DESTDIR)$(libdir) || exit 1;

uninstall-local:
	echo "$(GACUTIL) /u $(ASSEMBLY_NAME) /package gssdp-sharp-1.0 /root $(DESTDIR)$(libdir)"; \
        $(GACUTIL) /u $(ASSEMBLY_NAME) /package gssdp-sharp-1.0 /root $(DESTDIR)$(libdir) || exit 1;

EXTRA_DIST = $(RAW_API) $(sources) $(customs) $(METADATA) AssemblyInfo.cs.in gssdp-sharp.dll.config.in
