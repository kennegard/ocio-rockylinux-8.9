# OpenColorIO v2.3.2 Installation Guide

Welcome to the OpenColorIO v2.3.2 distribution! This guide will help you download, install, and set up OpenColorIO on your system, ensuring all dependencies are correctly linked and configured.

## Table of Contents

1. [Introduction](#introduction)
2. [System Requirements](#system-requirements)
3. [Download and Extract](#download-and-extract)
4. [Installation Steps](#installation-steps)
5. [Setting Up the Environment](#setting-up-the-environment)
6. [Verifying the Installation](#verifying-the-installation)
7. [Troubleshooting](#troubleshooting)

## Introduction

OpenColorIO (OCIO) is a robust color management system used in motion picture production. This guide provides a simple way to get started with the pre-compiled OpenColorIO v2.3.2.

## System Requirements

- **Operating System**: Linux (tested on Rocky Linux 8.9)

## Download and Extract

1. Download the pre-compiled OpenColorIO v2.3.2 from the [releases page](https://github.com/kennegard/ocio-rockylinux-8.9/releases).

2. Extract the downloaded file to your desired location. Replace `/desired/installation/path` with your chosen directory.

   ```bash
   https://github.com/kennegard/ocio-rockylinux-8.9/releases/download/2.3.2/OpenColorIO-2.3.2-dist.tar.gz
   tar -xzvf OpenColorIO-2.3.2-dist.tar.gz -C /desired/installation/path
   ```

   This will create a `dist` directory under the specified path.

## Installation Steps

Assuming the extraction path is `/opt/OpenColorIO`, follow these steps:

1. Verify the installation:

   ```bash
   ls /opt/OpenColorIO/dist
   ```

2. Check that the binaries and libraries are in place:

   ```bash
   ls /opt/OpenColorIO/dist/bin
   ls /opt/OpenColorIO/dist/lib64
   ```

## Setting Up the Environment

To use OpenColorIO, you need to configure your environment to include its binaries and libraries.

### Temporary Setup

1. Add the following to your terminal session to temporarily set the environment variables:

   ```bash
   export OCIO_ROOT=/opt/OpenColorIO/dist
   export PATH=$OCIO_ROOT/bin:$PATH
   export LD_LIBRARY_PATH=$OCIO_ROOT/lib64:$LD_LIBRARY_PATH
   ```

### Permanent Setup

2. To make these changes permanent, add the lines above to your `.bashrc` or `.bash_profile`:

   ```bash
   echo 'export OCIO_ROOT=/opt/OpenColorIO/dist' >> ~/.bashrc
   echo 'export PATH=$OCIO_ROOT/bin:$PATH' >> ~/.bashrc
   echo 'export LD_LIBRARY_PATH=$OCIO_ROOT/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
   ```

   Then source your `.bashrc`:

   ```bash
   source ~/.bashrc
   ```

## Verifying the Installation

Run the following command to ensure OpenColorIO is properly installed and configured:

```bash
ociocheck --version
```

This command should return the version of OpenColorIO and confirm it’s functioning correctly.

## Troubleshooting

If you encounter issues, follow these steps to resolve common problems:

### Missing Library

If you see an error about `libImath-3_1.so.29` not being found, check if it’s installed:

```bash
sudo find /usr -name "libImath-3_1.so.29"
```

If it’s found in `/usr/local/lib64`, ensure this path is in your `LD_LIBRARY_PATH`:

```bash
export LD_LIBRARY_PATH=/usr/local/lib64:$LD_LIBRARY_PATH
```

To make this change permanent and system-wide:

1. Add `/usr/local/lib64` to the library configuration file:

   ```bash
   echo "/usr/local/lib64" | sudo tee /etc/ld.so.conf.d/local_lib64.conf
   ```

2. Update the linker cache:

   ```bash
   sudo ldconfig
   ```

After this, rerun `ociocheck`:

```bash
ociocheck
```

This should resolve the linking issue and allow `ociocheck` to find `libImath-3_1.so.29`.

### Checking Dependencies

You can always check if all dependencies are correctly linked using `ldd`:

```bash
ldd /opt/OpenColorIO/dist/bin/ociocheck
```

This will list all the shared libraries required by `ociocheck` and their paths. Ensure `libImath-3_1.so.29` is correctly listed.

## Conclusion

By following these steps, you should have OpenColorIO v2.3.2 installed and running on your system. For further information and advanced usage, refer to the [OpenColorIO documentation](https://opencolorio.readthedocs.io/en/latest/).

If you encounter any issues not covered here, feel free to open an issue on the [GitHub repository](https://github.com/kennegard/ocio-rockylinux-8.9/issues).