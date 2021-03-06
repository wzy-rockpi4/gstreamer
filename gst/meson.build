gst_sources = [
  'gst.c',
  'gstobject.c',
  'gstallocator.c',
  'gstbin.c',
  'gstbuffer.c',
  'gstbufferlist.c',
  'gstbufferpool.c',
  'gstbus.c',
  'gstcaps.c',
  'gstcapsfeatures.c',
  'gstchildproxy.c',
  'gstclock.c',
  'gstclock-linreg.c',
  'gstcontext.c',
  'gstcontrolbinding.c',
  'gstcontrolsource.c',
  'gstdatetime.c',
  'gstdebugutils.c',
  'gstdevice.c',
  'gstdeviceprovider.c',
  'gstdeviceproviderfactory.c',
  'gstelement.c',
  'gstelementfactory.c',
  'gsterror.c',
  'gstevent.c',
  'gstformat.c',
  'gstghostpad.c',
  'gstdevicemonitor.c',
  'gstinfo.c',
  'gstiterator.c',
  'gstatomicqueue.c',
  'gstmessage.c',
  'gstmeta.c',
  'gstmemory.c',
  'gstminiobject.c',
  'gstpad.c',
  'gstpadtemplate.c',
  'gstparamspecs.c',
  'gstpipeline.c',
  'gstplugin.c',
  'gstpluginfeature.c',
  'gstpluginloader.c',
  'gstpoll.c',
  'gstpreset.c',
  'gstprotection.c',
  'gstquark.c',
  'gstquery.c',
  'gstregistry.c',
  'gstregistrychunks.c',
  'gstsample.c',
  'gstsegment.c',
  'gststreamcollection.c',
  'gststreams.c',
  'gststructure.c',
  'gstsystemclock.c',
  'gsttaglist.c',
  'gsttagsetter.c',
  'gsttask.c',
  'gsttaskpool.c',
  'gsttoc.c',
  'gsttocsetter.c',
  'gsttracer.c',
  'gsttracerfactory.c',
  'gsttracerrecord.c',
  'gsttracerutils.c',
  'gsttypefind.c',
  'gsttypefindfactory.c',
  'gsturi.c',
  'gstutils.c',
  'gstvalue.c',
  'gstparse.c',
]

gst_headers = [
  'gst.h',
  'glib-compat.h',
  'gstobject.h',
  'gstallocator.h',
  'gstbin.h',
  'gstbuffer.h',
  'gstbufferlist.h',
  'gstbufferpool.h',
  'gstbus.h',
  'gstcaps.h',
  'gstcapsfeatures.h',
  'gstchildproxy.h',
  'gstclock.h',
  'gstcompat.h',
  'gstcontext.h',
  'gstcontrolbinding.h',
  'gstcontrolsource.h',
  'gstdatetime.h',
  'gstdebugutils.h',
  'gstelement.h',
  'gstelementmetadata.h',
  'gstdevice.h',
  'gstdeviceprovider.h',
  'gstdeviceproviderfactory.h',
  'gstelementfactory.h',
  'gsterror.h',
  'gstevent.h',
  'gstformat.h',
  'gstghostpad.h',
  'gstdevicemonitor.h',
  'gstinfo.h',
  'gstiterator.h',
  'gstatomicqueue.h',
  'gstmacros.h',
  'gstmessage.h',
  'gstmeta.h',
  'gstmemory.h',
  'gstminiobject.h',
  'gstpad.h',
  'gstpadtemplate.h',
  'gstparamspecs.h',
  'gstpipeline.h',
  'gstplugin.h',
  'gstpluginfeature.h',
  'gstpoll.h',
  'gstpreset.h',
  'gstprotection.h',
  'gstquery.h',
  'gstsample.h',
  'gstsegment.h',
  'gststreamcollection.h',
  'gststreams.h',
  'gststructure.h',
  'gstsystemclock.h',
  'gsttaglist.h',
  'gsttagsetter.h',
  'gsttask.h',
  'gsttaskpool.h',
  'gsttoc.h',
  'gsttocsetter.h',
  'gsttracer.h',
  'gsttracerfactory.h',
  'gsttracerrecord.h',
  'gsttypefind.h',
  'gsttypefindfactory.h',
  'gsturi.h',
  'gstutils.h',
  'gstvalue.h',
  'gstregistry.h',
  'gstparse.h',
  'math-compat.h',
]
install_headers(gst_headers, subdir : 'gstreamer-1.0/gst')

if not get_option('disable_registry')
  gst_registry = ['gstregistrybinary.c']
else
  gst_registry = []
endif

configure_file(input : 'gstconfig.h.in',
  output : 'gstconfig.h',
  install_dir : 'include/gstreamer-1.0/gst',
  configuration : cdata)
configure_file(input : 'gstversion.h.in',
  output : 'gstversion.h',
  install_dir : 'include/gstreamer-1.0/gst',
  configuration : cdata)

mkenums = find_program('build_mkenum.py')
glib_mkenums = find_program('glib-mkenums')

gstenum_h = custom_target('gstenum_h',
  output : 'gstenumtypes.h',
  input : gst_headers,
  install : true,
  install_dir : 'include/gstreamer-1.0/gst',
  command : [mkenums, glib_mkenums, '@OUTPUT@', '@INPUT@'])

gstenum_c = custom_target('gstenum_c',
  output : 'gstenumtypes.c',
  input : gst_headers,
  depends : [gstenum_h],
  command : [mkenums, glib_mkenums, '@OUTPUT@', '@INPUT@'])

subdir('parse')
subdir('printf')

libgst_c_args = gst_c_args + [
  '-D_GNU_SOURCE',
  '-DGST_EXPORTS',
  '-DG_LOG_DOMAIN=g_log_domain_gstreamer',
  '-DGST_DISABLE_DEPRECATED',
]

# Make it possible to build both static and shared versions
# at the same time. By default use shared for unit tests etc.
# This choice is arbitrary.
if libtype != 'shared'
  libgst_static = static_library('gstreamer-1.0', gst_sources,
    gstenum_h, gstenum_c, grammar, parser, gst_registry,
    c_args : [libgst_c_args],
    include_directories : [configinc,
      # HACK, change include paths in .y and .l in final version.
      include_directories('parse')],
    install : true,
    link_with : printf_lib,
    dependencies : [gobject_dep, gmodule_dep, glib_dep, mathlib] + platform_deps,
  )
  libgst = libgst_static
endif

# Make sure that subproject building gir files work
gst_incdirs = [configinc]
gst_gen_sources = [gstenum_h]
if libtype != 'static'
  libgst_shared = shared_library('gstreamer-1.0', gst_sources,
    gstenum_h, gstenum_c, grammar, parser, gst_registry,
    version : libversion,
    soversion : soversion,
    c_args : libgst_c_args,
    include_directories : [configinc,
      # HACK, change include paths in .y and .l in final version.
      include_directories('parse')],
    link_with : printf_lib,
    install : true,
    dependencies : [gobject_dep, gmodule_dep, glib_dep, mathlib] + platform_deps,
    vs_module_defs: vs_module_defs_dir + 'libgstreamer.def',
  )
  libgst = libgst_shared
  if build_gir
    gst_gir_extra_args = gir_init_section + [ '--c-include=gst/gst.h' ]
    if meson.is_subproject()
      # FIXME: There must be a better way to do this
      # Need to pass the include path to find gst/gst.h and gst/gstenumtypes.h (built)
      gst_gir_extra_args += ['--cflags-begin',
	  '-I' + meson.current_source_dir() + '/..',
	  '-I' + meson.current_build_dir() + '/..',
	  '--cflags-end']
    endif

    gst_incdirs += [configinc]
    gst_gen_sources += [gnome.generate_gir(libgst_shared,
      sources : gst_sources + gst_headers,
      namespace : 'Gst',
      nsversion : apiversion,
      identifier_prefix : 'Gst',
      symbol_prefix : 'gst',
      export_packages : 'gstreamer-1.0',
      includes : ['GLib-2.0', 'GObject-2.0', 'GModule-2.0' ],
      install : true,
      extra_args : gst_gir_extra_args,
    )]
  endif
endif

gst_dep = declare_dependency(link_with : libgst,
  include_directories : gst_incdirs,
  dependencies : [glib_dep, gobject_dep, gmodule_dep],
  # Everything that uses libgst needs this built to compile
  sources : gst_gen_sources,
)
