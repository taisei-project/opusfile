project('opusfile', 'c', version : '0.11', license : 'bsd-style')

m_dep = meson.get_compiler('c').find_library('m', required : false)
ogg_dep = dependency('ogg', fallback : ['ogg', 'libogg_dep'])
opus_dep = dependency('opus', fallback : ['opus', 'libopus_dep'])

opusfile_cflags = []

cc = meson.get_compiler('c')

if cc.has_function('lrintf', dependencies : m_dep)
    opusfile_cflags += ['-DOP_HAVE_LRINTF']
endif

opusfile_incdirs = include_directories('include')

subdir('src')
