project(
  'skippy-xd', 'c',
  version: files('version.txt'),
)

skippy_sources = [
  'src/clientwin.c',
  'src/config.c',
  'src/dlist.c',
  'src/focus.c',
  'src/img-xlib.c',
  'src/img.c',
  'src/layout.c',
  'src/mainwin.c',
  'src/skippy.c',
  'src/tooltip.c',
  'src/wm.c',
]

# dependencies
cc = meson.get_compiler('c')
m_dep = cc.find_library('m')

x11_dep = dependency('x11')
xcomposite_dep = dependency('xcomposite')
xdamage_dep = dependency('xdamage')
xext_dep = dependency('xext')
xfixes_dep = dependency('xfixes')
xft_dep = dependency('xft')
xrender_dep = dependency('xrender')

# optional
giflib_dep = cc.find_library('libgif', required: get_option('skippy_gif'))
if giflib_dep.found()
  add_project_arguments('-DCFG_GIFLIB', language: 'c')
  add_project_dependencies(giflib_dep, language: 'c')
  skippy_sources += ['src/img-gif.c']
endif

libjpeg_dep = dependency('libjpeg', required: get_option('skippy_jpeg'))
if libjpeg_dep.found()
  add_project_arguments('-DCFG_JPEG', language: 'c')
  add_project_dependencies(libjpeg_dep, language: 'c')
  skippy_sources += ['src/img-jpeg.c']
endif

libpng_dep = dependency('libpng', required: get_option('skippy_png'))
zlib_dep = dependency('zlib', required: get_option('skippy_png'))
if libpng_dep.found() and zlib_dep.found()
  add_project_arguments('-DCFG_LIBPNG', language: 'c')
  add_project_dependencies([libpng_dep, zlib_dep], language: 'c')
  skippy_sources += ['src/img-png.c']
endif

xinerama_dep = dependency('xinerama', required: get_option('skippy_xinerama'))
if xinerama_dep.found()
  add_project_arguments('-DCFG_XINERAMA', language: 'c')
  add_project_dependencies(xinerama_dep, language: 'c')
endif

executable(
  meson.project_name(),
  sources: skippy_sources,
  dependencies: [
    m_dep,
    x11_dep,
    xcomposite_dep,
    xdamage_dep,
    xext_dep,
    xfixes_dep,
    xft_dep,
    xrender_dep,
  ],
  c_args: [ '-DSKIPPYXD_VERSION="' + meson.project_version() + '"' ],
  install: true,
)

install_data(
  sources: ['skippy-xd.sample.rc'],
  rename: ['skippy-xd.rc'],
  install_dir: get_option('sysconfdir') / 'xdg',
)
