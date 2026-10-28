project('synctex', 'c')

# Keep aligned with synctex_parser_api_level.txt, not the parser release version.
version = '2.0.0'

pkg = import('pkgconfig')

headers = files(
  'synctex_parser.h',
  'synctex_parser_advanced.h',
  'synctex_parser_utils.h',
  'synctex_version.h',
)

sources = files(
  'synctex_parser.c',
  'synctex_parser_utils.c',
)

main = files(
  'synctex_main.c',
)

zdep = dependency('zlib', version: '>=1.2.8')

synctex_inc = include_directories('.')
synctex_lib = library('synctex',
  sources,
  install: true,
  dependencies: [ zdep ],
  include_directories: [ synctex_inc ],
)

synctex_dep = declare_dependency(
  include_directories: [ synctex_inc ],
  link_with: synctex_lib,
)

# Preserve the legacy flat include layout as well as namespaced consumers.
install_headers(headers)
install_headers(headers, subdir: 'synctex')

pkg.generate(
  name: 'synctex',
  version: version,
  libraries: synctex_lib,
  subdirs: '.',
  description: 'SyncTeX parser library',
  url: 'https://github.com/jlaurens/synctex',
  install_dir: join_paths(get_option('libdir'), 'pkgconfig'),
)

synctex_src = include_directories('.')
synctex_exe = executable('synctex',
  main,
  include_directories: [ synctex_inc, synctex_src ],
  install: true,
  link_with: [ synctex_lib ],
  dependencies: [ zdep ],
  c_args: [ '-D__SYNCTEX_WORK__' ]
)

test('first test',
  synctex_exe,
  args: [
    'edit',
    '-o',
    '1:100:100:synctex test files/1/edit query/1.pdf'
  ],
  workdir: meson.current_source_dir(),
)
