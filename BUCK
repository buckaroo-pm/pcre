load('//:subdir_glob.bzl', 'subdir_glob')

macos_flags = [
  '-DHAVE_DIRENT_H=1',
  '-DHAVE_SYS_STAT_H=1',
  '-DHAVE_SYS_TYPES_H=1',
  '-DHAVE_UNISTD_H=1',
  '-DHAVE_STDINT_H=1',
  '-DHAVE_INTTYPES_H=1',
  '-DHAVE_BCOPY=1',
  '-DHAVE_MEMMOVE=1',
  '-DHAVE_STRERROR=1',
  '-DHAVE_STRTOLL=1',
  '-DHAVE_STRTOQ=1',
  '-DPCRE_STATIC=1',
  '-DSUPPORT_PCRE8=1',
  '-DNEWLINE=10',
  '-DPOSIX_MALLOC_THRESHOLD=10',
  '-DLINK_SIZE=2',
  '-DPARENS_NEST_LIMIT=250',
  '-DMATCH_LIMIT=10000000',
  '-DMATCH_LIMIT_RECURSION=MATCH_LIMIT',
  '-DPCREGREP_BUFSIZE=20480',
  '-DMAX_NAME_SIZE=32',
  '-DMAX_NAME_COUNT=10000',
]

linux_flags = [
  '-DHAVE_DIRENT_H=1',
  '-DHAVE_SYS_STAT_H=1',
  '-DHAVE_SYS_TYPES_H=1',
  '-DHAVE_UNISTD_H=1',
  '-DHAVE_STDINT_H=1',
  '-DHAVE_INTTYPES_H=1',
  '-DHAVE_BCOPY=1',
  '-DHAVE_MEMMOVE=1',
  '-DHAVE_STRERROR=1',
  '-DHAVE_STRTOLL=1',
  '-DHAVE_STRTOQ=1',
  '-DPCRE_STATIC=1',
  '-DSUPPORT_PCRE8=1',
  '-DNEWLINE=10',
  '-DPOSIX_MALLOC_THRESHOLD=10',
  '-DLINK_SIZE=2',
  '-DPARENS_NEST_LIMIT=250',
  '-DMATCH_LIMIT=10000000',
  '-DMATCH_LIMIT_RECURSION=MATCH_LIMIT',
  '-DPCREGREP_BUFSIZE=20480',
  '-DMAX_NAME_SIZE=32',
  '-DMAX_NAME_COUNT=10000',
]

cxx_binary(
  name = 'dftables',
  headers = dict(subdir_glob([
    ('', '*.h'),
    ('', 'pcre_maketables.c'),
  ]).items() + [
    ('pcre.h', 'pcre.h.generic'),
  ]),
  srcs = [
    'dftables.c',
  ],
  platform_preprocessor_flags = [
    ('macos.*', macos_flags),
    ('linux.*', linux_flags),
  ],
)

genrule(
  name = 'pcre_chartables',
  out = 'pcre_chartables.c',
  cmd = '$(exe :dftables) $OUT',
)

cxx_library(
  name = 'pcre',
  header_namespace = '',
  exported_headers = dict(subdir_glob([
    ('', '*.h'),
  ]).items() + [
    ('pcre.h', 'pcre.h.generic'),
  ]),
  headers = {
    'config.h': 'config.h.generic',
  },
  srcs = glob([
    'pcre_*.c',
  ], exclude = glob([
    'pcre_*_test.c',
  ])) + [
    ':pcre_chartables',
  ],
  platform_preprocessor_flags = [
    ('macos.*', macos_flags),
    ('linux.*', linux_flags),
  ],
  visibility = [
    'PUBLIC',
  ],
)

cxx_binary(
  name = 'demo',
  srcs = [
    'pcredemo.c',
  ],
  deps = [
    ':pcre',
  ],
)

cxx_binary(
  name = 'test',
  header_namespace = '',
  headers = [
    'pcre_tables.c',
    'pcre_ucd.c',
  ],
  srcs = [
    'pcretest.c',
  ],
  platform_preprocessor_flags = [
    ('macos.*', macos_flags),
    ('linux.*', linux_flags),
  ],
  linker_flags = [
    '-lreadline',
  ],
  deps = [
    ':pcre',
  ],
)
