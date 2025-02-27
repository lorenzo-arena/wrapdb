project('taglib', 'cpp',
  version : '1.12',
  meson_version : '>=0.47.0',
  default_options : ['warning_level=0']
)

cpp = meson.get_compiler('cpp')

add_project_arguments('-DHAVE_CONFIG_H', language : 'cpp')
if get_option('default_library') != 'static'
  add_project_arguments('-DMAKE_TAGLIB_LIB', language : 'cpp')
else
  add_project_arguments('-DTAGLIB_STATIC', language : 'cpp')
endif

cdata = configuration_data()
if cpp.compiles('int main() { int i, x = 0; i = __sync_add_and_fetch(&x,1); return x; }', name : 'atomic builtins')
  cdata.set('HAVE_GCC_ATOMIC', 1)
elif cpp.compiles('#include <libkern/OSAtomic.h> int main() { int i, x = 0; i = OSAtomicIncrement32Barrier(&x); return x; }', name : 'macOS atomic')
  cdata.set('HAVE_MAC_ATOMIC', 1)
elif cpp.compiles('#include <windows.h> int main() { LONG i, x = 0; i = InterlockedIncrement(&x); return x; }', name : 'Windows atomic')
  cdata.set('HAVE_WIN_ATOMIC', 1)
endif

if cpp.compiles('int main() { __builtin_bswap64(0); return 0; }', name : 'gcc builtin byteswap')
  cdata.set('HAVE_GCC_BYTESWAP', 1)
elif cpp.compiles('#include <byteswap.h> int main() { __bswap_64(0); return 0; }', name : 'glibc byteswap')
  cdata.set('HAVE_GLIBC_BYTESWAP', 1)
elif cpp.compiles('#include <stdlib.h> int main() { _byteswap_uint64(0); return 0; }', name : 'MSC byteswap')
  cdata.set('HAVE_MSC_BYTESWAP', 1)
elif cpp.compiles('#include <libkern/OSByteOrder.h> int main() { OSSwapInt64(0); return 0; }', name : 'macOS byteswap')
  cdata.set('HAVE_MAC_BYTESWAP', 1)
elif cpp.links('#include <sys/endian.h> int main() { swap64(0); return 0; }', name : 'OpenBSD byteswap')
  cdata.compiles('HAVE_OPENBSD_BYTESWAP', 1)
endif

if cpp.compiles('#include <cstdio> #include <cstdarg> int main() { char buf[20]; va_list args; vsnprintf(buf, 20, \"%d\", args); return 0; }', name : 'vsnprintf')
  cdata.set('HAVE_VSNPRINTF', 1)
elif cpp.compiles('#include <cstdio> #include <cstdarg> int main() { char buf[20]; va_list args; vsprintf_s(buf, \"%d\", args); return 0; }', name : 'vsprintf_s')
  cdata.set('HAVE_VSPRINTF_S', 1)
endif

zlib_dep = dependency('zlib', required: get_option('zlib'))
cdata.set('HAVE_ZLIB', zlib_dep.found())

cfile = configure_file(
  input: 'config.h.cmake',
  output: 'config.h',
  format: 'cmake@',
  configuration : cdata,
)

tcdata = configuration_data()
tcfile = configure_file(
  output : 'taglib_config.h',
  configuration : tcdata,
)

install_headers(
  tcfile,
  'taglib/ape/apefile.h',
  'taglib/ape/apefooter.h',
  'taglib/ape/apeitem.h',
  'taglib/ape/apeproperties.h',
  'taglib/ape/apetag.h',
  'taglib/asf/asfattribute.h',
  'taglib/asf/asffile.h',
  'taglib/asf/asfpicture.h',
  'taglib/asf/asfproperties.h',
  'taglib/asf/asftag.h',
  'taglib/flac/flacmetadatablock.h',
  'taglib/flac/flacfile.h',
  'taglib/flac/flacpicture.h',
  'taglib/flac/flacproperties.h',
  'taglib/it/itfile.h',
  'taglib/it/itproperties.h',
  'taglib/mod/modfile.h',
  'taglib/mod/modfilebase.h',
  'taglib/mod/modproperties.h',
  'taglib/mod/modtag.h',
  'taglib/mp4/mp4atom.h',
  'taglib/mp4/mp4coverart.h',
  'taglib/mp4/mp4item.h',
  'taglib/mp4/mp4properties.h',
  'taglib/mp4/mp4file.h',
  'taglib/mp4/mp4tag.h',
  'taglib/mpc/mpcfile.h',
  'taglib/mpc/mpcproperties.h',
  'taglib/mpeg/id3v1/id3v1genres.h',
  'taglib/mpeg/id3v1/id3v1tag.h',
  'taglib/mpeg/id3v2/frames/attachedpictureframe.h',
  'taglib/mpeg/id3v2/frames/commentsframe.h',
  'taglib/mpeg/id3v2/frames/eventtimingcodesframe.h',
  'taglib/mpeg/id3v2/frames/generalencapsulatedobjectframe.h',
  'taglib/mpeg/id3v2/frames/ownershipframe.h',
  'taglib/mpeg/id3v2/frames/popularimeterframe.h',
  'taglib/mpeg/id3v2/frames/privateframe.h',
  'taglib/mpeg/id3v2/frames/synchronizedlyricsframe.h',
  'taglib/mpeg/id3v2/frames/tableofcontentsframe.h',
  'taglib/mpeg/id3v2/frames/textidentificationframe.h',
  'taglib/mpeg/id3v2/frames/unsynchronizedlyricsframe.h',
  'taglib/mpeg/id3v2/frames/urllinkframe.h',
  'taglib/mpeg/id3v2/frames/chapterframe.h',
  'taglib/mpeg/id3v2/frames/podcastframe.h',
  'taglib/mpeg/id3v2/frames/relativevolumeframe.h',
  'taglib/mpeg/id3v2/frames/uniquefileidentifierframe.h',
  'taglib/mpeg/id3v2/frames/unknownframe.h',
  'taglib/mpeg/id3v2/id3v2footer.h',
  'taglib/mpeg/id3v2/id3v2synchdata.h',
  'taglib/mpeg/id3v2/id3v2.h',
  'taglib/mpeg/id3v2/id3v2extendedheader.h',
  'taglib/mpeg/id3v2/id3v2frame.h',
  'taglib/mpeg/id3v2/id3v2framefactory.h',
  'taglib/mpeg/id3v2/id3v2header.h',
  'taglib/mpeg/id3v2/id3v2tag.h',
  'taglib/mpeg/mpegfile.h',
  'taglib/mpeg/mpegheader.h',
  'taglib/mpeg/mpegproperties.h',
  'taglib/mpeg/xingheader.h',
  'taglib/ogg/flac/oggflacfile.h',
  'taglib/ogg/opus/opusfile.h',
  'taglib/ogg/opus/opusproperties.h',
  'taglib/ogg/speex/speexfile.h',
  'taglib/ogg/speex/speexproperties.h',
  'taglib/ogg/vorbis/vorbisfile.h',
  'taglib/ogg/vorbis/vorbisproperties.h',
  'taglib/ogg/oggfile.h',
  'taglib/ogg/oggpage.h',
  'taglib/ogg/oggpageheader.h',
  'taglib/ogg/xiphcomment.h',
  'taglib/riff/aiff/aifffile.h',
  'taglib/riff/aiff/aiffproperties.h',
  'taglib/riff/rifffile.h',
  'taglib/riff/wav/infotag.h',
  'taglib/riff/wav/wavfile.h',
  'taglib/riff/wav/wavproperties.h',
  'taglib/s3m/s3mfile.h',
  'taglib/s3m/s3mproperties.h',
  'taglib/taglib_export.h',
  'taglib/toolkit/tdebuglistener.h',
  'taglib/toolkit/taglib.h',
  'taglib/toolkit/tbytevector.h',
  'taglib/toolkit/tbytevectorlist.h',
  'taglib/toolkit/tbytevectorstream.h',
  'taglib/toolkit/tfile.h',
  'taglib/toolkit/tfilestream.h',
  'taglib/toolkit/tiostream.h',
  'taglib/toolkit/tlist.h',
  'taglib/toolkit/tlist.tcc',
  'taglib/toolkit/tmap.h',
  'taglib/toolkit/tmap.tcc',
  'taglib/toolkit/tpropertymap.h',
  'taglib/toolkit/trefcounter.h',
  'taglib/toolkit/tstring.h',
  'taglib/toolkit/tstringlist.h',
  'taglib/trueaudio/trueaudiofile.h',
  'taglib/trueaudio/trueaudioproperties.h',
  'taglib/wavpack/wavpackfile.h',
  'taglib/wavpack/wavpackproperties.h',
  'taglib/xm/xmfile.h',
  'taglib/xm/xmproperties.h',
  'taglib/audioproperties.h',
  'taglib/fileref.h',
  'taglib/tag.h',
  subdir : 'taglib',
)

taglib_includes = [
  '.',
  '3rdparty',
  'taglib',
  'taglib/ape',
  'taglib/asf',
  'taglib/flac',
  'taglib/it',
  'taglib/mod',
  'taglib/mp4',
  'taglib/mpc',
  'taglib/mpeg',
  'taglib/mpeg/id3v1',
  'taglib/mpeg/id3v2',
  'taglib/mpeg/id3v2/frames',
  'taglib/ogg',
  'taglib/ogg/flac',
  'taglib/ogg/opus',
  'taglib/ogg/speex',
  'taglib/ogg/vorbis',
  'taglib/riff',
  'taglib/riff/aiff',
  'taglib/riff/wav',
  'taglib/s3m',
  'taglib/toolkit',
  'taglib/trueaudio',
  'taglib/wavpack',
  'taglib/xm',
]

taglib = library('tag',
  'taglib/audioproperties.cpp',
  'taglib/fileref.cpp',
  'taglib/tag.cpp',
  'taglib/tagunion.cpp',
  'taglib/tagutils.cpp',
  'taglib/ape/apefile.cpp',
  'taglib/ape/apefooter.cpp',
  'taglib/ape/apeitem.cpp',
  'taglib/ape/apeproperties.cpp',
  'taglib/ape/apetag.cpp',
  'taglib/asf/asfattribute.cpp',
  'taglib/asf/asffile.cpp',
  'taglib/asf/asfpicture.cpp',
  'taglib/asf/asfproperties.cpp',
  'taglib/asf/asftag.cpp',
  'taglib/flac/flacfile.cpp',
  'taglib/flac/flacmetadatablock.cpp',
  'taglib/flac/flacpicture.cpp',
  'taglib/flac/flacproperties.cpp',
  'taglib/flac/flacunknownmetadatablock.cpp',
  'taglib/it/itfile.cpp',
  'taglib/it/itproperties.cpp',
  'taglib/mod/modfilebase.cpp',
  'taglib/mod/modfile.cpp',
  'taglib/mod/modproperties.cpp',
  'taglib/mod/modtag.cpp',
  'taglib/mp4/mp4atom.cpp',
  'taglib/mp4/mp4coverart.cpp',
  'taglib/mp4/mp4file.cpp',
  'taglib/mp4/mp4item.cpp',
  'taglib/mp4/mp4properties.cpp',
  'taglib/mp4/mp4tag.cpp',
  'taglib/mpc/mpcfile.cpp',
  'taglib/mpc/mpcproperties.cpp',
  'taglib/mpeg/mpegfile.cpp',
  'taglib/mpeg/mpegheader.cpp',
  'taglib/mpeg/mpegproperties.cpp',
  'taglib/mpeg/xingheader.cpp',
  'taglib/mpeg/id3v1/id3v1genres.cpp',
  'taglib/mpeg/id3v1/id3v1tag.cpp',
  'taglib/mpeg/id3v2/id3v2extendedheader.cpp',
  'taglib/mpeg/id3v2/id3v2footer.cpp',
  'taglib/mpeg/id3v2/id3v2frame.cpp',
  'taglib/mpeg/id3v2/id3v2framefactory.cpp',
  'taglib/mpeg/id3v2/id3v2header.cpp',
  'taglib/mpeg/id3v2/id3v2synchdata.cpp',
  'taglib/mpeg/id3v2/id3v2tag.cpp',
  'taglib/mpeg/id3v2/frames/attachedpictureframe.cpp',
  'taglib/mpeg/id3v2/frames/chapterframe.cpp',
  'taglib/mpeg/id3v2/frames/commentsframe.cpp',
  'taglib/mpeg/id3v2/frames/eventtimingcodesframe.cpp',
  'taglib/mpeg/id3v2/frames/generalencapsulatedobjectframe.cpp',
  'taglib/mpeg/id3v2/frames/ownershipframe.cpp',
  'taglib/mpeg/id3v2/frames/podcastframe.cpp',
  'taglib/mpeg/id3v2/frames/popularimeterframe.cpp',
  'taglib/mpeg/id3v2/frames/privateframe.cpp',
  'taglib/mpeg/id3v2/frames/relativevolumeframe.cpp',
  'taglib/mpeg/id3v2/frames/synchronizedlyricsframe.cpp',
  'taglib/mpeg/id3v2/frames/tableofcontentsframe.cpp',
  'taglib/mpeg/id3v2/frames/textidentificationframe.cpp',
  'taglib/mpeg/id3v2/frames/uniquefileidentifierframe.cpp',
  'taglib/mpeg/id3v2/frames/unknownframe.cpp',
  'taglib/mpeg/id3v2/frames/unsynchronizedlyricsframe.cpp',
  'taglib/mpeg/id3v2/frames/urllinkframe.cpp',
  'taglib/ogg/oggfile.cpp',
  'taglib/ogg/oggpage.cpp',
  'taglib/ogg/oggpageheader.cpp',
  'taglib/ogg/xiphcomment.cpp',
  'taglib/ogg/flac/oggflacfile.cpp',
  'taglib/ogg/opus/opusfile.cpp',
  'taglib/ogg/opus/opusproperties.cpp',
  'taglib/ogg/speex/speexfile.cpp',
  'taglib/ogg/speex/speexproperties.cpp',
  'taglib/ogg/vorbis/vorbisfile.cpp',
  'taglib/ogg/vorbis/vorbisproperties.cpp',
  'taglib/riff/rifffile.cpp',
  'taglib/riff/aiff/aifffile.cpp',
  'taglib/riff/aiff/aiffproperties.cpp',
  'taglib/riff/wav/infotag.cpp',
  'taglib/riff/wav/wavfile.cpp',
  'taglib/riff/wav/wavproperties.cpp',
  'taglib/s3m/s3mfile.cpp',
  'taglib/s3m/s3mproperties.cpp',
  'taglib/toolkit/tbytevector.cpp',
  'taglib/toolkit/tbytevectorlist.cpp',
  'taglib/toolkit/tbytevectorstream.cpp',
  'taglib/toolkit/tdebug.cpp',
  'taglib/toolkit/tdebuglistener.cpp',
  'taglib/toolkit/tfile.cpp',
  'taglib/toolkit/tfilestream.cpp',
  'taglib/toolkit/tiostream.cpp',
  'taglib/toolkit/tpropertymap.cpp',
  'taglib/toolkit/trefcounter.cpp',
  'taglib/toolkit/tstring.cpp',
  'taglib/toolkit/tstringlist.cpp',
  'taglib/toolkit/tzlib.cpp',
  'taglib/trueaudio/trueaudiofile.cpp',
  'taglib/trueaudio/trueaudioproperties.cpp',
  'taglib/wavpack/wavpackfile.cpp',
  'taglib/wavpack/wavpackproperties.cpp',
  'taglib/xm/xmfile.cpp',
  'taglib/xm/xmproperties.cpp',
  include_directories : taglib_includes,
  dependencies: zlib_dep,
  install : true,
  version : '1.18.0',
)

pkg = import('pkgconfig')
pkg.generate(taglib,
  filebase: 'taglib',
  subdirs : 'taglib'
)

taglib_dep = declare_dependency(
  include_directories : taglib_includes,
  link_with : taglib,
)

