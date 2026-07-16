# linux-build-gcc

English | [简体中文](./README_zh.md)

---

A workflow for compiling and installing GCC from source on Linux in GitHub Actions.

It automatically caches the installation path to avoid recompiling the same version. Cache path: `$HOME/.local/gcc-<gcc version>`.
Where `<gcc version>` is the GCC version number, e.g. `16.1.0`. Valid version numbers are listed at https://ftp.gnu.org/gnu/gcc/ (only versions `10.1.0` and later are supported).

This workflow is suitable for newer GCC versions. Whenever possible, it is recommended to install GCC via a package manager such as apt.

## Usage

In your GitHub Actions workflow, use `uses` to include this workflow and pass the desired GCC version (optional).

For example:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: ZheFeng7110/linux-build-gcc@v1
        with:
          gcc-version: '16.1.0'  # (Optional) Specify the GCC version

    # ...
```

The above approach has a drawback: if a subsequent step fails, the cache will be discarded. Therefore, it is recommended to separate cache storage and cache restoration into two different jobs to prevent the cache from being dropped:

```yaml
jobs:
  cache:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: ZheFeng7110/linux-build-gcc@v1
        with:
          gcc-version: '16.1.0'

  build:
    runs-on: ubuntu-latest
    needs: cache
    steps:
      - uses: actions/checkout@v4

      - uses: ZheFeng7110/linux-build-gcc/restore-cache@v1
        with:
          gcc-version: '16.1.0'
```

This way, even if the build job fails, the cache from the cache job will not be discarded.

## License

[Unlicense](./UNLICENSE)
