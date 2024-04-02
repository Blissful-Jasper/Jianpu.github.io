---
layout: post
title: "Windows | install salem and geopandas"
author: Jianpu
categories: Python
tags: [python]
image: python.jpg
date: 2024-04-01 14:19
comments: true
---


# 前言
---
![plot_wrf_t2.png](https://s2.loli.net/2024/04/02/4IijsHqDRBNPmKU.png)
---
近日，闲来无事想要测试 `salem` 和 `geopandas` 结合的相关掩膜。很久之前我已经在` Windows` 上记录过相关的安装教程，但是最近发现原来安装之后还会产生很多环境依赖的问题。经过两天的测试，终于解决了环境依赖的问题。

发现主要依赖冲突的地方在于 `salem` 的安装。根据官网的介绍，salem 需要依赖于 `rasterio、shapely、geopandas` 这三个库，而 `rasterio` 和 `geopandas` 又依赖于 `GDAL`。之前我安装 `geopandas` 时是通过 `pip` 安装源文件的，所以导致 `geopandas` 和` salem` 中的` GDAL` 和 `rasterio `的环境产生了冲突，导致虽然明面上安装成功了，但是实际运行时会出现 `rasterio` 无法导入、`GDAL` 无法查找到路径等问题。

此外，在安装 `salem `过程中还需要安装 `netCDF4`，进行 `nc `文件的读取。而单独安装 `salem `过程中也会导致额外安装的 `xarray `出现问题，无法正常读取 `nc `文件。

此外，由于安装` geopandas `过程需要依赖投影 `pyproj` 这个库，而 `cartopy` 这个库对于不同的 `Python` 版本所需要的 `pyproj `也是不一样的，所以会导致` geopandas `安装的 `pyproj` 与 `cartopy` 不适配，导致无法成功进行投影绘图。


![Required dependencies](https://files.mdnice.com/user/57394/1911b72a-c082-4cb1-9f29-078962885ca7.png)


![salem Required dependencies](https://files.mdnice.com/user/57394/6d75ccce-1618-4846-a82d-6ce8c33684a2.png)


综上，通过单一的 `pip `指令安装 `salem` 和 `geopandas` 无法解决环境依赖问题，所以这里将采用 conda 进行安装，依次解决不同库之间的环境依赖问题。

# 解决方法

## 环境准备

- anaconda 环境


- 创建一个虚拟环境

```
conda create -n py311 python=3.11
```

在`3.8`和`3.11`版本的`python`测试成功，这里以`python=3.11`版本进行演示

- windows系统




进入虚拟环境

```
conda activate py311
```

salem官网中介绍的依赖环境如下所示：

```
Python 3+

numpy (of course)

scipy: for its interpolation tools, among other things

pyproj: for map projections transformations

netCDF4: to read most geoscientific files

pandas: working with labeled data

xarray : pandas in N-dimensions

joblib: for it’s Memory class
```


这里将所需要的依赖环境以及常用的库一下全部进行安装，额外安装的包括：`xarray、wrf-python、cartopy、geopandas、rasterio、cmaps`,可以根据需要进行额外添加。以上的库保证了基本的nc文件读取以及可视化不会出现问题。

安装命令采用`forge`安装：

```bash
conda install conda-forge::gdal rasterio shapely geopandas scipy pyproj netCDF4 xarray joblib  salem pillow matplotlib scikit-image cartopy wrf-python cmaps
```



![安装过程](https://files.mdnice.com/user/57394/9c2eb8db-76fb-4a7c-821e-3d4578ad4d76.png)



等待安装结束后，运行相关代码，成功搞定！！！

以下附上所有安装库的对应版本，方便一键进行安装。

```bash
conda env create -f environment.yml
```
---

```bash
name: py311
channels:
  - conda-forge
  - defaults
dependencies:
  - affine=2.3.0=pyhd3eb1b0_0
  - alabaster=0.7.12=pyhd3eb1b0_0
  - aom=3.7.1=h63175ca_0
  - arrow=1.2.3=py311haa95532_1
  - astroid=2.14.2=py311haa95532_0
  - asttokens=2.0.5=pyhd3eb1b0_0
  - atomicwrites=1.4.0=py_0
  - attrs=23.1.0=py311haa95532_0
  - autopep8=2.0.4=pyhd3eb1b0_0
  - aws-c-auth=0.7.16=h7613915_8
  - aws-c-cal=0.6.10=hf6fcf4e_2
  - aws-c-common=0.9.14=hcfcfb64_0
  - aws-c-compression=0.2.18=hf6fcf4e_2
  - aws-c-event-stream=0.4.2=h3df98b0_6
  - aws-c-http=0.8.1=h4e3df0f_7
  - aws-c-io=0.14.6=hf0b8b6f_2
  - aws-c-mqtt=0.10.3=h96fac68_2
  - aws-c-s3=0.5.4=h08df315_0
  - aws-c-sdkutils=0.1.15=hf6fcf4e_2
  - aws-checksums=0.1.18=hf6fcf4e_2
  - aws-crt-cpp=0.26.4=hbe739fa_2
  - aws-sdk-cpp=1.11.267=hfaf0dd0_4
  - azure-core-cpp=1.11.1=h249a519_1
  - azure-storage-blobs-cpp=12.10.0=h91493d7_1
  - azure-storage-common-cpp=12.5.0=h91493d7_4
  - babel=2.11.0=py311haa95532_0
  - bcrypt=3.2.0=py311h2bbff1b_1
  - beautifulsoup4=4.12.2=py311haa95532_0
  - binaryornot=0.4.4=pyhd3eb1b0_1
  - black=23.11.0=py311haa95532_0
  - blas=1.0=mkl
  - bleach=4.1.0=pyhd3eb1b0_0
  - blosc=1.21.5=hdccc3a2_0
  - bottleneck=1.3.7=py311hd7041d2_0
  - branca=0.6.0=py311haa95532_0
  - brotli=1.0.9=ha925a31_2
  - brotli-python=1.0.9=py311hd77b12b_7
  - bzip2=1.0.8=h2bbff1b_5
  - c-ares=1.28.1=hcfcfb64_0
  - c-blosc2=2.14.0=h183a6f4_0
  - ca-certificates=2024.3.11=haa95532_0
  - cairo=1.18.0=h1fef639_0
  - cartopy=0.22.0=py311h786e728_0
  - certifi=2024.2.2=py311haa95532_0
  - cffi=1.16.0=py311h2bbff1b_0
  - cfitsio=4.4.0=h9b0cee5_0
  - cftime=1.6.2=py311h5bb9823_0
  - chardet=4.0.0=py311haa95532_1003
  - charls=2.4.2=h1537add_0
  - charset-normalizer=2.0.4=pyhd3eb1b0_0
  - click=8.1.7=py311haa95532_0
  - click-plugins=1.1.1=pyhd3eb1b0_0
  - cligj=0.7.2=pyhd3eb1b0_0
  - cloudpickle=2.2.1=py311haa95532_0
  - cmaps=2.0.1=pyhd8ed1ab_0
  - colorama=0.4.6=py311haa95532_0
  - comm=0.2.1=py311haa95532_0
  - contourpy=1.2.0=py311h59b6b97_0
  - cookiecutter=2.6.0=py311haa95532_0
  - cryptography=42.0.5=py311h89fc84f_0
  - cycler=0.11.0=pyhd3eb1b0_0
  - dav1d=1.2.1=h2bbff1b_0
  - debugpy=1.6.7=py311hd77b12b_0
  - decorator=5.1.1=pyhd3eb1b0_0
  - defusedxml=0.7.1=pyhd3eb1b0_0
  - diff-match-patch=20200713=pyhd3eb1b0_0
  - dill=0.3.7=py311haa95532_0
  - docstring-to-markdown=0.11=py311haa95532_0
  - docutils=0.18.1=py311haa95532_3
  - executing=0.8.3=pyhd3eb1b0_0
  - expat=2.6.2=h63175ca_0
  - flake8=7.0.0=py311haa95532_0
  - fmt=10.2.1=h181d51b_0
  - folium=0.14.0=py311haa95532_0
  - font-ttf-dejavu-sans-mono=2.37=hd3eb1b0_0
  - font-ttf-inconsolata=2.001=hcb22688_0
  - font-ttf-source-code-pro=2.030=hd3eb1b0_0
  - font-ttf-ubuntu=0.83=h8b1ccd4_0
  - fontconfig=2.14.2=hbde0cde_0
  - fonts-anaconda=1=h8fa9717_0
  - fonts-conda-ecosystem=1=hd3eb1b0_0
  - fonttools=4.25.0=pyhd3eb1b0_0
  - freetype=2.12.1=ha860e81_0
  - freexl=2.0.0=hd7a5696_0
  - geopandas=0.14.2=py311haa95532_0
  - geopandas-base=0.14.2=py311haa95532_0
  - geos=3.12.1=h1537add_0
  - geotiff=1.7.1=hbf5ca3a_15
  - gettext=0.21.1=h5728263_0
  - giflib=5.2.1=h8cc25b3_3
  - glib=2.80.0=h39d0aa6_1
  - glib-tools=2.80.0=h0a98069_1
  - gst-plugins-base=1.22.9=h001b923_1
  - gstreamer=1.22.9=hb4038d2_1
  - hdf4=4.2.15=h5557f11_7
  - hdf5=1.14.3=nompi_h73e8ff5_100
  - icc_rt=2022.1.0=h6049295_2
  - icu=73.2=h63175ca_0
  - idna=3.4=py311haa95532_0
  - imagecodecs=2024.1.1=py311hd83dc81_3
  - imageio=2.33.1=py311haa95532_0
  - imagesize=1.4.1=py311haa95532_0
  - importlib-metadata=7.0.1=py311haa95532_0
  - importlib_metadata=7.0.1=hd3eb1b0_0
  - inflection=0.5.1=py311haa95532_0
  - intel-openmp=2023.1.0=h59b6b97_46320
  - intervaltree=3.1.0=pyhd3eb1b0_0
  - ipykernel=6.28.0=py311haa95532_0
  - ipython=8.20.0=py311haa95532_0
  - isort=5.9.3=pyhd3eb1b0_0
  - jaraco.classes=3.2.1=pyhd3eb1b0_0
  - jedi=0.18.1=py311haa95532_1
  - jellyfish=1.0.1=py311h36a85e1_0
  - jinja2=3.1.3=py311haa95532_0
  - joblib=1.2.0=py311haa95532_0
  - jsonschema=4.19.2=py311haa95532_0
  - jsonschema-specifications=2023.7.1=py311haa95532_0
  - jupyter_client=8.6.0=py311haa95532_0
  - jupyter_core=5.5.0=py311haa95532_0
  - jupyterlab_pygments=0.2.2=py311haa95532_0
  - jxrlib=1.1=he774522_2
  - kealib=1.5.3=hd248416_0
  - keyring=24.3.1=py311haa95532_0
  - kiwisolver=1.4.4=py311hd77b12b_0
  - krb5=1.21.2=heb0366b_0
  - lazy-object-proxy=1.6.0=py311h2bbff1b_0
  - lazy_loader=0.3=py311haa95532_0
  - lcms2=2.16=h67d730c_0
  - lerc=4.0.0=h63175ca_0
  - libabseil=20240116.1=cxx17_h63175ca_2
  - libaec=1.1.3=h63175ca_0
  - libarchive=3.7.2=h313118b_1
  - libavif=1.0.1=h7a9aacb_3
  - libbrotlicommon=1.1.0=hcfcfb64_1
  - libbrotlidec=1.1.0=hcfcfb64_1
  - libbrotlienc=1.1.0=hcfcfb64_1
  - libclang13=18.1.2=default_hf64faad_1
  - libcrc32c=1.1.2=hd77b12b_0
  - libcurl=8.7.1=hd5e4a3a_0
  - libdeflate=1.20=hcfcfb64_0
  - libexpat=2.6.2=h63175ca_0
  - libffi=3.4.4=hd77b12b_0
  - libgdal=3.8.4=hf83a0e2_5
  - libglib=2.80.0=h39d0aa6_1
  - libgoogle-cloud=2.22.0=h9cad5c0_1
  - libgoogle-cloud-storage=2.22.0=hb581fae_1
  - libgrpc=1.62.1=h5273850_0
  - libiconv=1.17=hcfcfb64_2
  - libjpeg-turbo=3.0.0=hcfcfb64_1
  - libkml=1.3.0=h63940dd_7
  - libnetcdf=4.9.2=nompi_h07c049d_113
  - libogg=1.3.5=h2bbff1b_1
  - libpng=1.6.43=h19919ed_0
  - libpq=16.2=hdb24f17_1
  - libprotobuf=4.25.3=h503648d_0
  - libre2-11=2023.09.01=hf8d8778_2
  - librttopo=1.1.0=h94c4f80_15
  - libsodium=1.0.18=h62dcd97_0
  - libspatialindex=1.9.3=h6c2663c_0
  - libspatialite=5.1.0=hf2f0abc_4
  - libsqlite=3.45.2=hcfcfb64_0
  - libssh2=1.11.0=h7dfc565_0
  - libtiff=4.6.0=hddb2be6_3
  - libvorbis=1.3.7=he774522_0
  - libwebp-base=1.3.2=h2bbff1b_0
  - libxcb=1.15=hcd874cb_0
  - libxml2=2.12.6=hc3477c8_1
  - libzip=1.10.1=h1d365fa_3
  - libzlib=1.2.13=hcfcfb64_5
  - libzopfli=1.0.3=ha925a31_0
  - lz4-c=1.9.4=h2bbff1b_0
  - lzo=2.10=he774522_2
  - m2w64-gcc-libgfortran=5.3.0=6
  - m2w64-gcc-libs=5.3.0=7
  - m2w64-gcc-libs-core=5.3.0=7
  - m2w64-gmp=6.1.0=2
  - m2w64-libwinpthread-git=5.0.0.4634.697f757=2
  - mapclassify=2.5.0=py311haa95532_0
  - markdown-it-py=2.2.0=py311haa95532_1
  - markupsafe=2.1.3=py311h2bbff1b_0
  - matplotlib=3.8.0=py311haa95532_0
  - matplotlib-base=3.8.0=py311hf62ec03_0
  - matplotlib-inline=0.1.6=py311haa95532_0
  - mccabe=0.7.0=pyhd3eb1b0_0
  - mdurl=0.1.0=py311haa95532_0
  - minizip=4.0.3=hb68bac4_0
  - mistune=2.0.4=py311haa95532_0
  - mkl=2023.1.0=h6b88ed4_46358
  - mkl-service=2.4.0=py311h2bbff1b_1
  - mkl_fft=1.3.8=py311h2bbff1b_0
  - mkl_random=1.2.4=py311h59b6b97_0
  - more-itertools=10.1.0=py311haa95532_0
  - msys2-conda-epoch=20160418=1
  - munkres=1.1.4=py_0
  - mypy_extensions=1.0.0=py311haa95532_0
  - nbclient=0.8.0=py311haa95532_0
  - nbconvert=7.10.0=py311haa95532_0
  - nbformat=5.9.2=py311haa95532_0
  - nest-asyncio=1.6.0=py311haa95532_0
  - netcdf4=1.6.5=nompi_py311he019f65_100
  - networkx=3.1=py311haa95532_0
  - numexpr=2.8.7=py311h1fcbade_0
  - numpy=1.26.4=py311hdab7c0b_0
  - numpy-base=1.26.4=py311hd01c5d8_0
  - numpydoc=1.5.0=py311haa95532_0
  - openjpeg=2.5.2=h3d672ee_0
  - openssl=3.2.1=hcfcfb64_1
  - packaging=23.2=py311haa95532_0
  - pandas=2.2.1=py311hea22821_0
  - pandocfilters=1.5.0=pyhd3eb1b0_0
  - paramiko=2.8.1=pyhd3eb1b0_0
  - parso=0.8.3=pyhd3eb1b0_0
  - pathspec=0.10.3=py311haa95532_0
  - pcre2=10.43=h17e33f8_0
  - pexpect=4.8.0=pyhd3eb1b0_3
  - pickleshare=0.7.5=pyhd3eb1b0_1003
  - pip=23.3.1=py311haa95532_0
  - pixman=0.43.4=h63175ca_0
  - platformdirs=3.10.0=py311haa95532_0
  - pluggy=1.0.0=py311haa95532_1
  - ply=3.11=py311haa95532_0
  - poppler=24.03.0=hc2f3c52_0
  - poppler-data=0.4.11=haa95532_1
  - postgresql=16.2=h94c9ec1_1
  - proj=9.3.1=ha107b6e_0
  - prompt-toolkit=3.0.43=py311haa95532_0
  - prompt_toolkit=3.0.43=hd3eb1b0_0
  - psutil=5.9.0=py311h2bbff1b_0
  - pthread-stubs=0.3=h3c9f919_1
  - ptyprocess=0.7.0=pyhd3eb1b0_2
  - pure_eval=0.2.2=pyhd3eb1b0_0
  - pycodestyle=2.11.1=py311haa95532_0
  - pycparser=2.21=pyhd3eb1b0_0
  - pydocstyle=6.3.0=py311haa95532_0
  - pyflakes=3.2.0=py311haa95532_0
  - pygments=2.15.1=py311haa95532_1
  - pylint=2.16.2=py311haa95532_0
  - pylint-venv=3.0.3=py311haa95532_0
  - pyls-spyder=0.4.0=pyhd3eb1b0_0
  - pynacl=1.5.0=py311h8cc25b3_0
  - pyparsing=3.0.9=py311haa95532_0
  - pyqt=5.15.10=py311hd77b12b_0
  - pyqt5-sip=12.13.0=py311h2bbff1b_0
  - pyqtwebengine=5.15.10=py311hd77b12b_0
  - pyshp=2.3.1=py311haa95532_0
  - pysocks=1.7.1=py311haa95532_0
  - python=3.11.8=he1021f5_0
  - python-dateutil=2.8.2=pyhd3eb1b0_0
  - python-fastjsonschema=2.16.2=py311haa95532_0
  - python-lsp-black=2.0.0=py311haa95532_0
  - python-lsp-jsonrpc=1.1.2=pyhd3eb1b0_0
  - python-lsp-server=1.10.0=py311haa95532_0
  - python-slugify=5.0.2=pyhd3eb1b0_0
  - python-tzdata=2023.3=pyhd3eb1b0_0
  - python_abi=3.11=2_cp311
  - pytoolconfig=1.2.6=py311haa95532_0
  - pytz=2023.3.post1=py311haa95532_0
  - pywin32=305=py311h2bbff1b_0
  - pywin32-ctypes=0.2.2=py311haa95532_0
  - pyyaml=6.0.1=py311h2bbff1b_0
  - pyzmq=25.1.2=py311hd77b12b_0
  - qdarkstyle=3.2.3=pyhd3eb1b0_0
  - qstylizer=0.2.2=py311haa95532_0
  - qt-main=5.15.8=h9e85ed6_20
  - qt-webengine=5.15.9=h5bd16bc_7
  - qtawesome=1.2.2=py311haa95532_0
  - qtconsole=5.5.1=py311haa95532_0
  - qtpy=2.4.1=py311haa95532_0
  - rav1e=0.6.6=h975169c_2
  - re2=2023.09.01=hd3b24a8_2
  - referencing=0.30.2=py311haa95532_0
  - requests=2.31.0=py311haa95532_1
  - rich=13.3.5=py311haa95532_0
  - rope=1.12.0=py311haa95532_0
  - rpds-py=0.10.6=py311h062c2fa_0
  - rtree=1.0.1=py311h2eaa2aa_0
  - scikit-image=0.22.0=py311hb4ba03d_0
  - scikit-learn=1.2.2=py311hd77b12b_1
  - scipy=1.12.0=py311h9f229c6_0
  - setuptools=68.2.2=py311haa95532_0
  - sip=6.7.12=py311hd77b12b_0
  - six=1.16.0=pyhd3eb1b0_1
  - snappy=1.1.10=h6c2663c_1
  - snowballstemmer=2.2.0=pyhd3eb1b0_0
  - snuggs=1.4.7=pyhd3eb1b0_0
  - sortedcontainers=2.4.0=pyhd3eb1b0_0
  - soupsieve=2.5=py311haa95532_0
  - spdlog=1.12.0=h64d2f7d_2
  - sphinx=5.0.2=py311haa95532_0
  - sphinxcontrib-applehelp=1.0.2=pyhd3eb1b0_0
  - sphinxcontrib-devhelp=1.0.2=pyhd3eb1b0_0
  - sphinxcontrib-htmlhelp=2.0.0=pyhd3eb1b0_0
  - sphinxcontrib-jsmath=1.0.1=pyhd3eb1b0_0
  - sphinxcontrib-qthelp=1.0.3=pyhd3eb1b0_0
  - sphinxcontrib-serializinghtml=1.1.5=pyhd3eb1b0_0
  - spyder=5.5.1=py311haa95532_0
  - spyder-kernels=2.5.0=py311haa95532_0
  - sqlite=3.41.2=h2bbff1b_0
  - stack_data=0.2.0=pyhd3eb1b0_0
  - svt-av1=1.7.0=h63175ca_0
  - tbb=2021.8.0=h59b6b97_0
  - text-unidecode=1.3=pyhd3eb1b0_0
  - textdistance=4.2.1=pyhd3eb1b0_0
  - threadpoolctl=2.2.0=pyh0d69192_0
  - three-merge=0.1.1=pyhd3eb1b0_0
  - tifffile=2023.4.12=py311haa95532_0
  - tiledb=2.21.1=h25b666a_1
  - tinycss2=1.2.1=py311haa95532_0
  - tk=8.6.13=h5226925_1
  - tomli=2.0.1=py311haa95532_0
  - tomlkit=0.11.1=py311haa95532_0
  - tornado=6.3.3=py311h2bbff1b_0
  - traitlets=5.7.1=py311haa95532_0
  - tzdata=2024a=h04d1e81_0
  - ucrt=10.0.20348.0=haa95532_0
  - ujson=5.4.0=py311hd77b12b_0
  - unidecode=1.2.0=pyhd3eb1b0_0
  - uriparser=0.9.7=h2bbff1b_0
  - urllib3=2.1.0=py311haa95532_1
  - vc=14.2=h21ff451_1
  - vc14_runtime=14.38.33130=h82b7239_18
  - vs2015_runtime=14.38.33130=hcb4865c_18
  - watchdog=2.1.6=py311haa95532_0
  - wcwidth=0.2.5=pyhd3eb1b0_0
  - webencodings=0.5.1=py311haa95532_1
  - whatthepatch=1.0.2=py311haa95532_0
  - wheel=0.41.2=py311haa95532_0
  - win_inet_pton=1.1.0=py311haa95532_0
  - wrapt=1.14.1=py311h2bbff1b_0
  - wrf-python=1.3.4.1=py311hc52a59b_3
  - xarray=2023.6.0=py311haa95532_0
  - xerces-c=3.2.5=h63175ca_0
  - xorg-libxau=1.0.11=hcd874cb_0
  - xorg-libxdmcp=1.1.3=hcd874cb_0
  - xyzservices=2022.9.0=py311haa95532_1
  - xz=5.4.6=h8cc25b3_0
  - yaml=0.2.5=he774522_0
  - yapf=0.40.2=py311haa95532_0
  - zeromq=4.3.5=hd77b12b_0
  - zfp=1.0.1=h63175ca_0
  - zipp=3.17.0=py311haa95532_0
  - zlib=1.2.13=hcfcfb64_5
  - zlib-ng=2.0.7=h2bbff1b_0
  - zstd=1.5.5=hd43e919_0
  - pip:
      - descartes==1.1.0
      - fiona==1.8.21
      - gdal==3.4.3
      - global-land-mask==1.0.0
      - munch==4.0.0
      - pillow==9.1.1
      - pooch==1.8.1
      - pyogrio==0.7.2
      - pyproj==3.3.1
      - rasterio==1.2.10
      - regionmask==0.12.1
      - salem==0.3.5
      - shapely==1.8.2
prefix: D:\anaconda\ANACONDA\envs\py311

```
封面来源：
> https://salem.readthedocs.io/en/stable/_images/plot_wrf_t2.png