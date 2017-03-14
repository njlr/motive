include_defs('//BUCKAROO_DEPS')

genrule(
  name = 'schemas',
  out = 'out',
  srcs = glob([
    'schemas/*.fbs',
  ]),
  cmd = 'mkdir -p $OUT && cd $OUT && flatc --cpp $SRCS',
)

genrule(
  name = 'anim_list_generated.h',
  out = 'anim_list_generated.h',
  cmd = 'cp $(location :schemas)/anim_list_generated.h $OUT',
)

genrule(
  name = 'anim_table_generated.h',
  out = 'anim_table_generated.h',
  cmd = 'cp $(location :schemas)/anim_table_generated.h $OUT',
)

genrule(
  name = 'anim_generated.h',
  out = 'anim_generated.h',
  cmd = 'cp $(location :schemas)/anim_generated.h $OUT',
)

genrule(
  name = 'compact_spline_generated.h',
  out = 'compact_spline_generated.h',
  cmd = 'cp $(location :schemas)/compact_spline_generated.h $OUT',
)

genrule(
  name = 'motive_generated.h',
  out = 'motive_generated.h',
  cmd = 'cp $(location :schemas)/motive_generated.h $OUT',
)

genrule(
  name = 'spline_anim_generated.h',
  out = 'spline_anim_generated.h',
  cmd = 'cp $(location :schemas)/spline_anim_generated.h $OUT',
)

cxx_library(
  name = 'motive',
  header_namespace = '',
  exported_headers = subdir_glob([
    ('include', 'motive/**/*.h'),
  ]),
  headers = {
    'anim_list_generated.h': ':anim_list_generated.h',
    'anim_table_generated.h': ':anim_table_generated.h',
    'anim_generated.h': ':anim_generated.h',
    'compact_spline_generated.h': ':compact_spline_generated.h',
    'motive_generated.h': ':motive_generated.h',
    'spline_anim_generated.h': ':spline_anim_generated.h',
  },
  srcs = glob([
    'src/motive/**/*.cpp',
  ]),
  compiler_flags = [
    '-std=c++14',
  ],
  visibility = [
    'PUBLIC',
  ],
  deps = BUCKAROO_DEPS,
)
