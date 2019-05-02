
opusfile_lib = library('opusfile',
    'info.c',
    'internal.c',
    'opusfile.c',
    'stream.c',

    c_args : opusfile_cflags,
    build_by_default : not meson.is_subproject(),
    include_directories : opusfile_incdirs,
    dependencies : [m_dep, ogg_dep, opus_dep],
)

opusfile_dep = declare_dependency(
    include_directories : opusfile_incdirs,
    link_with : opusfile_lib,
    dependencies : [ogg_dep, opus_dep],
    version : meson.project_version(),
)
