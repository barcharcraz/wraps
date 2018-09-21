project('glfw', 'c', version: '3.2.1')

cc = meson.get_compiler('c')


glfw_core_src = files(
    'src/context.c',
    'src/init.c',
    'src/input.c',
    'src/monitor.c',
    'src/vulkan.c',
    'src/window.c'
)
glfw_platform_src = []
glfw_deps = []
glfw_deps += cc.find_library('m', required: false)
glfw_deps += cc.find_library('dl', required: false)
glfw_deps += dependency('threads')
if get_option('windowing_backend') == 'x11'
    x11_dep = dependency('x11')
    xrandr_dep = dependency('xrandr')
    xinerama_dep = dependency('xinerama')
    xkb_dep = dependency('xkbcommon')
    xcursor_dep = dependency('xcursor')
    glfw_deps += [x11_dep, xrandr_dep, xinerama_dep, xkb_dep, xcursor_dep]
    glfw_platform_src += files(
        'src/x11_init.c',
        'src/x11_monitor.c',
        'src/x11_window.c',
        'src/xkb_unicode.c',
        'src/linux_joystick.c',
        'src/posix_time.c',
        'src/posix_tls.c',
        'src/glx_context.c',
        'src/egl_context.c'
    )

else
   error('backend not supported')
endif

conf_data = configuration_data()
if get_option('windowing_backend') == 'x11'
    conf_data.set('_GLFW_X11', 1)
endif
conf_file = configure_file(configuration: conf_data, output: 'glfw_config.h')


glfw_lib = library('glfw', glfw_core_src + glfw_platform_src,
                include_directories: include_directories('src'),
                dependencies: glfw_deps,
                c_args: ['-D_GLFW_USE_CONFIG_H'])

glfw_dep = declare_dependency(
    include_directories: include_directories('src'),
    link_with: glfw_lib)