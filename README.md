<!--
    Copyright 2013-2014 The GLFW-RS Developers. For a full listing of the authors,
    refer to the AUTHORS file at the top-level directory of this distribution.

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

        http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
-->

# glfw-rs

GLFW bindings and wrapper for The Rust Programming Language.

## Example

~~~rust
extern crate glfw;

use glfw::{Action, Context, Key};

fn main() {
    let mut glfw = glfw::init(glfw::FAIL_ON_ERRORS).unwrap();

    let (mut window, events) = glfw.create_window(300, 300, "Hello this is window", glfw::WindowMode::Windowed)
        .expect("Failed to create GLFW window.");

    window.set_key_polling(true);
    window.make_current();

    while !window.should_close() {
        glfw.poll_events();
        for (_, event) in glfw::flush_messages(&events) {
            handle_window_event(&mut window, event);
        }
    }
}

fn handle_window_event(window: &mut glfw::Window, event: glfw::WindowEvent) {
    match event {
        glfw::WindowEvent::Key(Key::Escape, _, Action::Press, _) => {
            window.set_should_close(true)
        }
        _ => {}
    }
}
~~~

## Using glfw-rs

### Prerequisites

Make sure you have [compiled and installed GLFW 3.x](http://www.glfw.org/docs/latest/compile.html).
You might be able to find it on your package manager, for example on OS X:
`brew install --static glfw3` (you may need to run `brew tap homebrew/versions`).
If not you can download and build the library
[from the source](http://www.glfw.org/docs/latest/compile.html) supplied on the
GLFW website. Note that if you compile GLFW with CMake on Linux, you will have
to supply the `-DCMAKE_C_FLAGS=-fPIC` argument. You may install GLFW to your
`PATH`, otherwise you will have to specify the directory containing the library
binaries when you call `make` or `make lib`:

~~~
GLFW_LIB_DIR=path/to/glfw/lib/directory make
~~~

### Including glfw-rs in your project

Add this to your `Cargo.toml`:

~~~toml
[dependencies.glfw]
git = "https://github.com/bjz/glfw-rs.git"
~~~

#### On Windows

By default, `glfw-rs` will try to compile the `glfw` library. If you want to link to your custom
build of `glfw` or if the build doesn't work (which is probably the case on Windows), you can
disable this:

~~~toml
[dependencies.glfw]
git = "https://github.com/bjz/glfw-rs.git"
default-features = false
~~~

### A note about Travis CI

You may encounter the following error when attempting to build your project on Travis:

~~~
CMake Error at CMakeLists.txt:3 (cmake_minimum_required):
  CMake 2.8.9 or higher is required.  You are running version 2.8.7
~~~

This is because Travis CI _still_ hasn't updated their CMake version almost a year since the issue
was reported (See travis-ci/travis-ci#2030). Because of this, you will need to [add the following
build commands](https://github.com/travis-ci/travis-ci/issues/2030#issuecomment-49210009) to your
`.travis.yml`:

~~~yml
before_install:
  # install a newer cmake since at this time Travis only has version 2.8.7
  - yes | sudo add-apt-repository ppa:kalakris/cmake
  - sudo apt-get update -qq
install: sudo apt-get install cmake
~~~

## Support

Contact `bjz` on irc.mozilla.org [#rust](http://mibbit.com/?server=irc.mozilla.org&channel=%23rust)
and [#rust-gamedev](http://mibbit.com/?server=irc.mozilla.org&channel=%23rust-gamedev),
or [post an issue](https://github.com/bjz/glfw-rs/issues/new) on Github.

## glfw-rs in use

- [sebcrozet/kiss3d](https://github.com/sebcrozet/kiss3d)
- [Jeaye/q3](https://github.com/Jeaye/q3)
- [cyndis/rsmc](https://github.com/cyndis/rsmc/)
- [ozkriff/zoc](https://github.com/ozkriff/zoc)
