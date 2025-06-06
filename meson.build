project('py4godot', 'cpp',
    meson_version: '>=1.6.0',)

cc = meson.get_compiler('cpp')
message('compiler:' + cc.get_id())
message('system:' + build_machine.system())
message('cpu_family:' + build_machine.cpu_family())
message('host_platform:' + host_machine.system())
platform_format = ''
machine = host_machine.system()
platform_format = '32'
if host_machine.cpu_family() == 'x86_64'
    platform_format = '64'
endif
if host_machine.cpu_family() == 'aarch64'
    platform_format = '64'
endif

message('platform:' + platform_format)
message('machine:' + machine)

extra_args = []
message(meson.get_compiler('cpp').get_id())

link_args = []
cpp_args = []
if meson.get_compiler('cpp').get_id() == 'gcc'
    add_global_arguments('-DMS_WIN' + platform_format, language: 'cpp')
    cpp_args = ['-D LIBRARY_EXPORTS=1']
    if platform_format == '32'
        link_args = ['-m32']
        cpp_args += ['-m32', '-std=c++17']
    endif
endif
if meson.get_compiler('cpp').get_id() == 'g++'
    add_global_arguments('-DMS_WIN' + platform_format, language: 'cpp')
    cpp_args = ['-D LIBRARY_EXPORTS=1']
    if platform_format == '32'
        link_args = ['-m32']
        cpp_args += ['-m32', '-std=c++17']
    endif
endif
if meson.get_compiler('cpp').get_id() == 'clang'
    cpp_args += ['-std=c++17', '-Wno-address-of-temporary']
endif
if meson.get_compiler('cpp').get_id() == 'msvc'
    add_global_arguments('/bigobj', language: 'cpp')
    cpp_args = ['/DLIBRARY_EXPORTS=1']
endif

message(meson.current_source_dir())

if build_machine.system() == 'windows'
    py = import('python').find_installation('python3')
    dep_py = py.dependency()
    internal_inc = include_directories('py4godot/gdextension-api/', 'py4godot/core', 'py4godot/enums',
    'py4godot/classes', 'py4godot/pluginscript_api', 'py4godot/godot_bindings',
    'py4godot/script_instance', 'py4godot/core/variant4', 'py4godot/cppcore', 'py4godot/cppenums',
    'py4godot/cppclasses', 'py4godot/cpputils', 'py4godot/script_language', 'py4godot/pluginscript_api',
    'py4godot/pluginscript_api/utils')
    glob = run_command('python', files(meson.current_source_dir()+'/meson_scripts/glob_tools.py'), check:false)
    glob_cpp = run_command('python', files(meson.current_source_dir()+'/meson_scripts/glob_tools_cpp.py'), check:false)
endif
if build_machine.system() == 'darwin'
    message('search:')
    message(meson.current_source_dir()+'/python_files/cpython-3.13.3-'+machine+platform_format + '/python')
    py = import('python').find_installation('python3')
    dep_py = py.dependency()
    lib_py = cc.find_library('python3.13', dirs:meson.current_source_dir()+'/python_files/cpython-3.13.3-'+machine+platform_format + '/python/lib')
        internal_inc = include_directories('py4godot','py4godot/gdextension-api/', 'py4godot/core', 'py4godot/enums',
    'py4godot/classes', 'py4godot/pluginscript_api', 'py4godot/godot_bindings',
    'py4godot/script_instance', 'py4godot/core/variant4', 'py4godot/cppcore', 'py4godot/cppenums',
    'py4godot/cppclasses', 'py4godot/cpputils', 'py4godot/script_language', 'py4godot/pluginscript_api',
    'py4godot/pluginscript_api/utils',
    'py4godot/instance_data', 'python_files/cpython-3.13.3-'+machine+platform_format+'/python/include/python3.13')
    glob = run_command('python3', files(meson.current_source_dir()+'/meson_scripts/glob_tools.py'), check:false)
    glob_cpp = run_command('python3', files(meson.current_source_dir()+'/meson_scripts/glob_tools_cpp.py'), check:false)
    dep_py = lib_py
endif
if meson.has_external_property('is_mingw') and build_machine.system() == 'linux'
    message('dirs:')
    py = import('python').find_installation('python3')
    message(meson.current_source_dir()+'/python_files/cpython-3.13.3-'+machine+platform_format + '/python')
    link_args += ['-static-libgcc', '-static-libstdc++'] #TODO: Is this good like this?
    lib_py = cc.find_library('python312', dirs:meson.current_source_dir()+'/python_files/cpython-3.13.3-'+machine+platform_format + '/python')
        internal_inc = include_directories('py4godot','py4godot/gdextension-api/', 'py4godot/core', 'py4godot/enums',
    'py4godot/classes', 'py4godot/pluginscript_api', 'py4godot/godot_bindings',
    'py4godot/script_instance', 'py4godot/core/variant4', 'py4godot/cppcore', 'py4godot/cppenums',
    'py4godot/cppclasses', 'py4godot/cpputils', 'py4godot/script_language', 'py4godot/pluginscript_api',
    'py4godot/pluginscript_api/utils',
    'py4godot/instance_data', 'python_files/cpython-3.13.3-'+machine+platform_format+'/python/include')
    glob = run_command('python3', files(meson.current_source_dir()+'/meson_scripts/glob_tools.py'), check:false)
    glob_cpp = run_command('python3', files(meson.current_source_dir()+'/meson_scripts/glob_tools_cpp.py'), check:false)
    dep_py = lib_py
endif

if not meson.has_external_property('is_mingw') and build_machine.system() == 'linux'
    lib_py = cc.find_library('python3.13', dirs:meson.current_source_dir()+'/python_files/cpython-3.13.3-'+machine+platform_format+'/python/lib')
        internal_inc = include_directories('py4godot','py4godot/gdextension-api/', 'py4godot/core', 'py4godot/enums',
    'py4godot/classes', 'py4godot/pluginscript_api', 'py4godot/godot_bindings',
    'py4godot/script_instance', 'py4godot/core/variant4', 'py4godot/cppcore', 'py4godot/cppenums',
    'py4godot/cppclasses', 'py4godot/cpputils', 'py4godot/script_language', 'py4godot/pluginscript_api',
    'py4godot/pluginscript_api/utils','python_files/cpython-3.13.3-'+machine+platform_format+'/python/include/python3.13',
    'py4godot/instance_data')
    py = import('python').find_installation('python3')
    glob = run_command('python3', files(meson.current_source_dir()+'/meson_scripts/glob_tools.py'), check:false)
    glob_cpp = run_command('python3', files(meson.current_source_dir()+'/meson_scripts/glob_tools_cpp.py'), check:false)
    dep_py = lib_py
endif

if build_machine.system() == 'linux'
endif

godot_program = find_program('godot', required:false)
sources = glob.stdout().strip().split('\n')

cppsources = glob_cpp.stdout().strip().split('\n')
main_lib = shared_library('main',cppsources,
dependencies:[dep_py], include_directories:internal_inc,name_prefix:'',c_args:extra_args,
 link_args : link_args,
 cpp_args:cpp_args,
 install_rpath: '$ORIGIN/../lib',
  build_rpath : '$ORIGIN/../lib')

pythonscript_lib = shared_library('pythonscript',
                        'py4godot/godot_bindings/pythonscript.cpp',
                        dependencies:[dep_py],
                        include_directories:internal_inc,name_prefix:'',
                         c_args:extra_args, link_args:link_args,link_with:main_lib,
                        cpp_args:cpp_args,
                        install_rpath: '$ORIGIN/../lib',
                        build_rpath : '$ORIGIN/../lib')

foreach source : sources
    converted_source = source.replace('\\','#')
    linux_converted = converted_source.replace('/', '#')
    shared_library(linux_converted, [source+'.cpp'],
    dependencies:[dep_py], include_directories:internal_inc,name_prefix:'',c_args:extra_args,
    link_args : link_args,
    link_with : main_lib,
    cpp_args:cpp_args, install_rpath: '$ORIGIN/../lib',
    build_rpath : '$ORIGIN/../lib')
endforeach
if godot_program.found()
    #test('test_vector3', godot_program, args : ['--path', 'tests/core/vector3', '--headless'], workdir:meson.global_source_root(), timeout:1000, is_parallel:false)
    #test('test_vector2', godot_program, args : ['--path', 'tests/core/vector2', '--headless'], workdir:meson.global_source_root(),  timeout:1000, is_parallel:false)
    #test('test_rect2', godot_program, args : ['--path', 'tests/core/rect2', '--headless'], workdir:meson.global_source_root(), timeout:1000, is_parallel:false)
    test('test_aabb', py, args:['test_file.py', 'tests/core/aabb', godot_program],  workdir:meson.global_source_root(), timeout:1000, is_parallel:false)
    test('test_color', py, args:['test_file.py', 'tests/core/color', godot_program],  workdir:meson.global_source_root(), timeout:1000, is_parallel:false)
    #test('test_basis', godot_program, args : ['--path', 'tests/core/basis', '--headless'], workdir:meson.global_source_root(), timeout:1000, is_parallel:false)
    #test('test_array', godot_program, args : ['--path', 'tests/core/array', '--headless'], workdir:meson.global_source_root(), timeout:1000, is_parallel:false)
    test('test_color', godot_program, args : ['--path', 'tests/core/color', '--headless'], workdir:meson.global_source_root(), timeout:1000, is_parallel:false)
    test('test_packed_arrays', godot_program, args : ['--path', 'tests/core/packed_arrays', '--headless'], workdir:meson.global_source_root(), timeout:1000, is_parallel:false)
    #test('test_dictionary', godot_program, args : ['--path', 'tests/core/dictionary', '--headless'], workdir:meson.global_source_root(), timeout:1000, is_parallel:false)
    #test('test_node_path', godot_program, args : ['--path', 'tests/core/node_path', '--headless'], workdir:meson.global_source_root(), timeout:1000, is_parallel:false)
    #test('test_rid', godot_program, args : ['--path', 'tests/core/rid', '--headless'], workdir:meson.global_source_root(), timeout:1000, is_parallel:false)
    #test('test_quat', godot_program, args : ['--path', 'tests/core/quat', '--headless'], workdir:meson.global_source_root(), timeout:1000, is_parallel:false)
    #test('test_transform', godot_program, args : ['--path', 'tests/core/transform', '--headless'], workdir:meson.global_source_root(), timeout:1000, is_parallel:false)
    #test('test_transform2d', godot_program, args : ['--path', 'tests/core/transform2d', '--headless'], workdir:meson.global_source_root(), timeout:1000, is_parallel:false)
    test('test_signals', py, args:['test_file.py', 'tests/binding/signals', godot_program], workdir:meson.global_source_root(), timeout:100, is_parallel:false)
    test('test_singletons', py, args:['test_file.py', 'tests/binding/singletons', godot_program], workdir:meson.global_source_root(), timeout:100, is_parallel:false)
    test('test_numpy', py, args:['tests/libraries/numpy/test_file.py', 'tests/libraries/numpy', godot_program], workdir:meson.global_source_root(), timeout:100, is_parallel:false)
endif