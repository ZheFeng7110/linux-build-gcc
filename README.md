# linux-build-gcc

用于 Github Actions 中 Linux 系统从源代码编译并安装 GCC 的工作流。

会自动缓存安装路径，从而避免相同版本重复编译。缓存路径：`/usr/local/gcc-<gcc version>/bin`
其中，`<gcc version>` 为 GCC 的版本号，例如 `16.1.0`，有效的版本号见 https://ftp.gnu.org/gnu/gcc/
中列出来的版本（仅支持 `10.1.0` 及以后的版本）。

该工作流适用于较新的 gcc。条件允许的情况下，建议优先使用如 apt 之类的包管理器安装 gcc。

## 使用方法

在 Github Actions 的工作流中，使用 `uses` 引入该工作流，并传入所需的 GCC 版本号（可选）。

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: ZheFeng7110/linux-build-gcc@v1
        with:
          gcc-version: '16.1.0'  # （可选）指定所需的 GCC 版本

    # ...
```

## 许可证

[Unlicense](./UNLICENSE)
