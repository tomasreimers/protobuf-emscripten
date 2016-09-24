# Protobufs + Emscripten

## About

This repository contains edits to the Google Protobuf library (https://github.com/google/protobuf)
that allow it to be compiled using Emscripten (https://github.com/kripken/emscripten) so that
you can link against it in Emscripten-compiled projects that require protobufs.

The changes introduced mostly just shim thread primatives, and are introduced in the most unobtrusive way
so that on version bumps to the protobufs library this repository can simply be rebased.  

This was heavily inspired by https://github.com/invokr/protobuf-emscripten , I created a separate repo to
pin protobufs at 3.x, clarify the readme, and include the full commit history to make rebasing easier.

## Compiling

Download this repository and run

```
$ ./autogen.sh
$ emconfigure ./configure --with-protoc=protoc
$ emmake make
```

*Note: You need to pass the `--with-protoc` flag pointing to a pre-compiled protoc binary. This is because during compilation, protobufs run the protoc they just compiled (in order to build protobufs for testcases); however, since we compiled protoc with emscripten to output LLVM-IR (instead of a valid, executable binary), the compiled protoc will be unable to run on the host machine. To get around this, we must pass a precompiled protoc.*

This will create a file in `./src/.libs/libprotobuf.a` that you can link against with `emcc`.

## Linking

Once you've run the compile steps above you can link against the generated file using
`emcc my_file.cc -o my_file.js ../path/to/protobuf-emscripten/src/.libs/libprotobuf.a`

## Author

Tomas Reimers, September 2016
