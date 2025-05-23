# get-cmake

- [The **get-cmake** action installs as fast as possible your desired versions of CMake and Ninja](#the-get-cmake-action-installs-as-fast-as-possible-your-desired-versions-of-cmake-and-ninja)
  - [Quickstart](#quickstart)
    - [If you want to use  **latest stable** you can use this one-liner:](#if-you-want-to-use--latest-stable-you-can-use-this-one-liner)
    - [If you want to **pin** the workflow to **specific range of versions** of CMake and Ninja:](#if-you-want-to-pin-the-workflow-to-specific-range-of-versions-of-cmake-and-ninja)
  - [Action reference: all input/output parameters](#action-reference-all-inputoutput-parameters)
- [Developers information](#developers-information)
  - [Prerequisites](#prerequisites)
  - [Build and lint](#build-and-lint)
  - [Generate the catalog of CMake releases](#generate-the-catalog-of-cmake-releases)
  - [Packaging](#packaging)
  - [Testing](#testing)
- [License](#license)

<br>

# [The **get-cmake** action installs as fast as possible your desired versions of CMake and Ninja](https://github.com/marketplace/actions/get-cmake)
The action restores from local or cloud based cache both CMake and Ninja. If a `cache-miss` occurs, it downloads and caches the tools right away.

Works for `x64` and `arm64` hosts on Linux, macOS and Windows.

The desired version can be specified using [semantic versioning ranges](https://docs.npmjs.com/about-semantic-versioning), and also use `install` or `installrc` special tokens to install resp. the [latest stable](./.latest_cmake_version) or [release candidate](./.latestrc_cmake_version).

There are two kind of caches:
- The cloud based [GitHub cache](https://www.npmjs.com/package/@actions/cache). Enabled by default, it can be disabled using the input `useCloudCache:false`. 
- The local self-hosted runner cache, stored locally using [tool-cache](https://www.npmjs.com/package/@actions/tool-cache). Disabled by default, it can enabled with the input `useLocalCache:true`. 


Steps of `get-cmake`:
  1. If a `cache-hit` occurs (either local or cloud cache), CMake and Ninja are restored from the cache.
     1. if both local and cloud are enabled, the local cache check goes first.
  2. If a `cache-miss` occurs, i.e. none of the enabled caches hit:
     1. the action downloads and installs the desired versions of CMake and Ninja.
     2. the action stores CMake and Ninja for the enabled caches:
        1. if enabled, on the [cloud based GitHub cache](https://www.npmjs.com/package/@actions/cache). This is beneficial for the next run of the workflow especially on _GitHub-hosted runners_.
        2. if enabled, on the local GitHub runner cache. This is beneficial for the next run of the workflow on the same _self-hosted runner_.
        
        _Note:_ when there is a `cache-hit`, nothing will be stored in any of the caches.
  3. Adds to the `PATH` environment variable the binary directories for CMake and Ninja.
  
<br>

## Quickstart
### If you want to use  **latest stable** you can use this one-liner:
```yaml
  # Option 1: using v4 tag, the most recent CMake and ninja are installed.
    - uses: step-security/get-cmake@v4  # <--= Just this one-liner suffices.
```
The local and cloud cache can be enabled or disabled, for example:
```yaml
    # Suited for self-hosted GH runners where the local cache wins over the cloud.
    - uses: step-security/get-cmake@v4  
      with:
        useLocalCache: true         # <--= Use the local cache (default is 'false').
        useCloudCache: false        # <--= Ditch the cloud cache (default is 'true').
```
And there is a second option:
```yaml
  # Option 2: specify 'latest' or 'latestrc' in the input version arguments:
    - name: Get latest CMake and Ninja
      uses: step-security/get-cmake@v4
      with:
        cmakeVersion: latestrc      # <--= optional, use the latest release candidate (notice the 'rc' suffix).
        ninjaVersion: latest        # <--= optional, use the latest release (non candidate).
```

<br>

### If you want to **pin** the workflow to **specific range of versions** of CMake and Ninja:
```yaml
  # Option 1: specify in a input parameter the desired version using ranges.
  - uses: step-security/get-cmake@v4
    with:
      cmakeVersion: "~3.25.0"  # <--= optional, use most recent 3.25.x version
      ninjaVersion: "^1.11.1"  # <--= optional, use most recent 1.x version

  # or using a specific version (no range)
  - uses: step-security/get-cmake@v4
    with:
      cmakeVersion: 3.25.2     # <--= optional, stick to exactly 3.25.2 version
      ninjaVersion: 1.11.1     # <--= optional, stick to exactly 1.11.1 version
```
or there is another option:
```yaml
  # Option 2: or you can use the Git 'tag' to select the version, and you can have a one-liner statement,
  # but note that you can only use one of the existing tags, create a PR to add the tag you need!
  - name: Get specific version CMake, v4.0.0
    uses: step-security/get-cmake@v4.0.0     # <- this one-liner is all you need.
```
<br>

## Action reference: all input/output parameters
Please read [action.yml](./action.yml).

<br>

# Developers information

## Prerequisites
[gulp 4](https://www.npmjs.com/package/gulp4) globally installed.

<br>

## Build and lint
Build with `tsc` running:

 > npm run build

Launch `lint` by:

 > npm run lint

<br>

## Generate the catalog of CMake releases
To generate the catalog of CMake releases, run a special test with this command:

 > npm run generate-catalog

Then embed the new catalog by packaging the action.

<br>

## Packaging
To build, lint validate and package the extension (and embed the release catalog) for release purpose, run:

  > npm run pack

<br>

## Testing
To build, pack and run all tests:
 
 > npm run test

 To run all tests:
 
 > npx jest

 or

 > npx jest -- -t "&lt;regex to match the describe clause&gt;"

<br>

# License
All the content in this repository is licensed under the [MIT License](LICENSE).

Copyright (c) 2020-2021-2022-2023-2024 Luca Cappa
Copyright (c) 2025 Step Security