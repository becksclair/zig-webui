<div align="center">

# WebUI Zig

<!-- [build-status]: https://img.shields.io/github/actions/workflow/status/webui-dev/go-webui/ci.yml?branch=main&style=for-the-badge&logo=V&labelColor=414868&logoColor=C0CAF5 -->

[last-commit]: https://img.shields.io/github/last-commit/webui-dev/zig-webui?style=for-the-badge&logo=github&logoColor=C0CAF5&labelColor=414868
<!-- [release-version]: https://img.shields.io/github/v/tag/webui-dev/go-webui?style=for-the-badge&logo=webtrees&logoColor=C0CAF5&labelColor=414868&color=7664C6 -->
[license]: https://img.shields.io/github/license/webui-dev/zig-webui?style=for-the-badge&logo=opensourcehardware&label=License&logoColor=C0CAF5&labelColor=414868&color=8c73cc

<!-- [![][build-status]](https://github.com/webui-dev/go-webui/actions?query=branch%3Amain) -->

[![][last-commit]](https://github.com/webui-dev/zig-webui/pulse)
<!-- [![][release-version]](https://github.com/webui-dev/go-webui/releases/latest) -->
[![][license]](https://github.com/webui-dev/zig-webui/blob/main/LICENSE)

> WebUI is not a web-server solution or a framework, but it allows you to use any web browser as a GUI, with your preferred language in the backend and HTML5 in the frontend. All in a lightweight portable lib.

![Screenshot](https://github.com/webui-dev/webui/assets/34311583/57992ef1-4f7f-4d60-8045-7b07df4088c6)

</div>

## Features

- Parent library written in pure C
- Lightweight ~200 Kb & Small memory footprint
- Fast binary communication protocol between WebUI and the browser (_Instead of JSON_)
- Multi-platform & Multi-Browser
- Using private profile for safety

## Examples

There are several examples for newbies, they are in the `examples` directory.

You can use `zig build --help` to view all buildable examples.

Like `zig build run_minimal`, this will build and run the `minimal` example.

## Installation

### Zig `0.11`

1. Add this to `build.zig.zon`

```zig
.@"zig-webui" = .{
        // It is recommended to replace the following branch with commit id
        .url = "https://github.com/webui-dev/zig-webui/archive/main.tar.gz",
        .hash = <hash value>,
    },
```

This tells zig to fetch zig-webui from a tarball provided by GitHub. Make sure to replace the COMMIT part with an actual commit SHA in long form, like `219faa2a5cd5a268a865a1100e92805df4b84610`. Every time you want to update zig-webui you'll have to update this commit.

2. Config `build.zig`

Add this:

```zig
const zig_webui = b.dependency("zig-webui", .{
    .target = target,
    .optimize = optimize,
    .enable_tls = false, // whether enable tls support
    .is_static = true, // whether static link
});

// add module
exe.addModule("webui", zig_webui.module("webui"));

// link library
exe.linkLibrary(zig_webui.artifact("webui"));
```

### Zig `nightly`

> To be honest, I don’t recommend using the nightly version because the API of the build system is not yet stable, which means that there may be problems with not being able to build after nightly is updated.

1. Add to `build.zig.zon`

```sh
# It is recommended to replace the following branch with commit id
zig fetch --save https://github.com/webui-dev/zig-webui/archive/main.tar.gz
```

2. Config `build.zig`

Add this:

```zig
const zig_webui = b.dependency("zig-webui", .{
    .target = target,
    .optimize = optimize,
    .enable_tls = false, // whether enable tls support
    .is_static = true, // whether static link
});

// add module
exe.root_module.addImport("webui", zig_webui.module("webui"));
// note: see this issue for the API changes above,
// https://github.com/ziglang/zig/pull/18160

// link library
exe.linkLibrary(zig_webui.artifact("webui"));
```

> It is not recommended to dynamically link libraries under Windows, which may cause some symbol duplication problems.
> see this issue: https://github.com/ziglang/zig/issues/15107

## UI & The Web Technologies

[Borislav Stanimirov](https://ibob.bg/) discusses using HTML5 in the web browser as GUI at the [C++ Conference 2019 (_YouTube_)](https://www.youtube.com/watch?v=bbbcZd4cuxg).

<!-- <div align="center">
  <a href="https://www.youtube.com/watch?v=bbbcZd4cuxg"><img src="https://img.youtube.com/vi/bbbcZd4cuxg/0.jpg" alt="Embrace Modern Technology: Using HTML 5 for GUI in C++ - Borislav Stanimirov - CppCon 2019"></a>
</div> -->

<div align="center">

![CPPCon](https://github.com/webui-dev/webui/assets/34311583/4e830caa-4ca0-44ff-825f-7cd6d94083c8)

</div>

Web application UI design is not just about how a product looks but how it works. Using web technologies in your UI makes your product modern and professional, And a well-designed web application will help you make a solid first impression on potential customers. Great web application design also assists you in nurturing leads and increasing conversions. In addition, it makes navigating and using your web app easier for your users.

### Why Use Web Browsers?

Today's web browsers have everything a modern UI needs. Web browsers are very sophisticated and optimized. Therefore, using it as a GUI will be an excellent choice. While old legacy GUI lib is complex and outdated, a WebView-based app is still an option. However, a WebView needs a huge SDK to build and many dependencies to run, and it can only provide some features like a real web browser. That is why WebUI uses real web browsers to give you full features of comprehensive web technologies while keeping your software lightweight and portable.

### How Does it Work?

<div align="center">

![Diagram](https://github.com/ttytm/webui/assets/34311583/dbde3573-3161-421e-925c-392a39f45ab3)

</div>

Think of WebUI like a WebView controller, but instead of embedding the WebView controller in your program, which makes the final program big in size, and non-portable as it needs the WebView runtimes. Instead, by using WebUI, you use a tiny static/dynamic library to run any installed web browser and use it as GUI, which makes your program small, fast, and portable. **All it needs is a web browser**.

### Runtime Dependencies Comparison

|                                 | WebView           | Qt                         | WebUI               |
| ------------------------------- | ----------------- | -------------------------- | ------------------- |
| Runtime Dependencies on Windows | _WebView2_        | _QtCore, QtGui, QtWidgets_ | **_A Web Browser_** |
| Runtime Dependencies on Linux   | _GTK3, WebKitGTK_ | _QtCore, QtGui, QtWidgets_ | **_A Web Browser_** |
| Runtime Dependencies on macOS   | _Cocoa, WebKit_   | _QtCore, QtGui, QtWidgets_ | **_A Web Browser_** |

## Supported Web Browsers

| Browser         | Windows         | macOS         | Linux           |
| --------------- | --------------- | ------------- | --------------- |
| Mozilla Firefox | ✔️              | ✔️            | ✔️              |
| Google Chrome   | ✔️              | ✔️            | ✔️              |
| Microsoft Edge  | ✔️              | ✔️            | ✔️              |
| Chromium        | ✔️              | ✔️            | ✔️              |
| Yandex          | ✔️              | ✔️            | ✔️              |
| Brave           | ✔️              | ✔️            | ✔️              |
| Vivaldi         | ✔️              | ✔️            | ✔️              |
| Epic            | ✔️              | ✔️            | _not available_ |
| Apple Safari    | _not available_ | _coming soon_ | _not available_ |
| Opera           | _coming soon_   | _coming soon_ | _coming soon_   |

### License

> Licensed under the MIT License.
