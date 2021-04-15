# 2. 安装
预编译的二进制文件可用于稳定版本和当前开发版本。立体管线也能从源码进行编译，但不建议这样做。
## 2.1 预编译二进制文件(Linux and macOS)
简单地下载适合你操作系统的发行版，解压，运行bin文件夹下的可执行文件。没有其他必要的"安装"步骤也不需要管理员权限。  
- [Stable Releases](https://github.com/NeoGeographyToolkit/StereoPipeline/releases)  
- [Development Build](http://byss.arc.nasa.gov/stereopipeline/daily_build/)  

要将 ASP 可执行子目录永久添加到 PATH，你可以将以下命令行添加到你的 shell 配置中(例如，``~/.bashrc``)，将``/path/to/StereoPipeline/bin``替换为你文件系统上的位置：
```
export PATH=${PATH}:/path/to/StereoPipeline/bin
```
### 2.1.1 行星影像  
如果你想处理 NASA 航天器获取的星球影像，你可能得先安装 **ISIS** 。立体管线主程序的操作并不需要完整的 ISIS 安装(只有 ISIS 的数据文件夹是必需的)，但是在使用立体管线程序处理行星数据时，一些预处理步骤是很有必要的。 如果你只想处理全球的地面数字图像，直接跳到[数字地球用户快速入门]()章节。  

在运行立体管线之前，你需要安装``ISIS``，先对非地面影像执行预处理操作(辐射校正、星历处理等)。就像我们的二进制文件一样，你也可以按原样使用 ISIS 二进制文件。  

如果你需要重新编译，你可以按照以下说明用[源码构建ASP]()(但我们不推荐这样)。如果 ISIS 的当前版本比立体管线编译所需要的ISIS版本(在 ASP发行说明中已列出)要新，请放心，我们正在推出一个新版本。但是，由于立体管线在内部建立了自己的 ISIS 库自包含版本，因此您应该能够将 ISIS 的较新版本与ASP的最新版本一起使用。这是 ISIS 开发人员在数据格式和相机模型没有重大改变的前提下得出的结论。至少，如果安装失败，你可以安装一个 ISIS 的旧版本。按照以下 ISIS 安装说明，创建一个新的``conda``环境(不是当前 ISIS)，在你运行``conda install isis``之前，先运行``conda search isis``来找出所有适合安装的ISIS版本。举个例子，如果你想安装 ISIS 4.4.0 ,可以在``conda search isis``中列出，运行``conda install isis=4.4.0``(安装特定版本的 ISIS)然后根据 ISIS 安装说明的其余部分进行操作。  

最后，运行立体管线可执行文件仅需你下载ISIS辅助数据并且适当设置``ISISDATA``的环境变量即可。这通常是通过启动 ISIS 的``conda``环境为用户所执行的。  
### 2.1.2 ISIS用户快速入门
1. 获取立体管线：https://github.com/NeoGeographyToolkit/StereoPipeline/releases  

2. 获取 ISIS 二进制文件及安装：https://github.com/USGS-Astrogeology/ISIS3#installation  

3. 获取 ISIS 数据，详细见：https://github.com/USGS-Astrogeology/ISIS3#the-isis-data-area  

4. 解压：  
```
tar xzvf StereoPipeline-<VERSION>-<ARCH>-<OS>.tar.gz
```  

5. 添加立体管线路径(可选)：  
    - bash: ``export PATH="</path/to/StereoPipeline>/bin:${PATH}"``  
    - csh: ``setenv PATH "</path/to/StereoPipeline>/bin:${PATH}"``  

6. 试一试：参看[第3节]()的例子。  

### 2.1.3 数字地球用户快速入门

1. 获取立体管线：https://github.com/NeoGeographyToolkit/StereoPipeline/releases  

2. 解压：  
```
tar xzvf StereoPipeline-<VERSION>-<ARCH>-<OS>.tar.gz
```  

3. 试一试：地球影像处理在[第4章]()数据处理教程中有说明。  

### 2.1.4 航空和历史影像快速入门  

通过以上方式获取软件，处理无精确相机姿态信息的影像在[第9章]()有描述

## 2.2 常见错误  
下面是你可能会看到的一些错误及其含义。将它们视为问题的模板。实际上，错误消息可能会略有不同。  
```
**I/O ERROR** Unable to open [$ISISDATA/<Some/Path/Here>]. 
Stereo step 0: Preprocessing failed
```

你需要设置 ISIS 环境或者手动设置正确的`ISISDATA`路径。  
```
bash: stereo: command not found
```  

你需要将已部署的立体管线安装的`bin`文件夹添加到`PATH`环境变量中。  

## 2.3 conda获取预编译的ASP  
获取conda:  
```
https://docs.conda.io/en/latest/miniconda.html
```  

让它能够执行，运行：  
```
./Miniconda3-latest-Linux-x86_64.sh
```

在Linux上，以及在OSX上的适当版本。建议使用：  
```
$HOME/miniconda3 
``` 
作为安装目录文件。   

创建一个 ASP 环境：  
```
conda create -n asp python=3.6
conda activate asp
```  

添加相关的 channels:  
```
conda config --env --add channels    
conda config --env --add channels usgs-astrogeology
conda config --env --add channels nasa-ames-stereo-pipeline 
```

不要跳过以上三个中任意一个，即使你认为你已经部署有某些channels。

运行：
```
conda config --show channels
```
来确保channels的顺序：
```
- nasa-ames-stereo-pipeline
- usgs-astrogeology
- conda-forge
- defaluts
```

可能你已经在全局的`~/.condarc`文件中包含了某些channels，并且你很想运行最终的 add channels 命令，但是如果你不熟悉 conda 的 channel 管理，这可能会导致意想不到的结果。请仔细检查`--show channels`命令的输出顺序，如果它不像上面那样，你可以编辑`$CONDA_PREFIX/.condarc`文件，或者完全删除它，然后完全按照上面所示，运行三个`conda config --ebv -add channels`命令中的每一个。  

我们不推荐使用`--orepend channels`参数，那样会添加`nasa-ames-stereo-pipeline`到你默认的`~/.condarc`文件中，造成你所有 conda 环境中都有，这是你不想要的。  

使用`conda`命令安装 ASP：  
```
conda install stereo-pipeline==2.7.0
```

检查`stereo`命令是否可以找到：  
```
which stereo
```

conda 获取的精确依赖库可能存在一些变化性。作为记录，可以在立体管线 Github 仓库的`conda/`子目录中以一组 .yaml 文件形式找到此发行版的完整环境。所以，可以按以下方式安装：
```
conda env create -f asp_2.7.0_linux_env.yaml
```

或者：
```
conda env create -f asp_2.7.0_osx_env.yaml
```

这取决于你的平台。然后像之前那样调用它：
```
conda activate asp
```

最后，如果你要处理行星数据，你需要在新的`asp` conda 环境中完成 ISIS 的安装步骤。新的`asp`环境已经安装有基本的 ISIS 软件，但是你必须运行设置 ISIS 环境变量的脚本并且安装恰当的 ISIS 数据文件(如果你还具有单独的 ISIS conda 环境，你可以用启动脚本，将`asp` conda 环境的`$ISISDATA`环境变量指向你现有的数据区域)。更多关于 ISIS 安装剩余部分的信息，请参照[installation instructions at their repo](https://github.com/USGS-Astrogeology/ISIS3).  

## 2.4 源码构建ASP
首先需要下载所有带有 conda 的 ASP 依赖项，并将其作为预编译的二进制文件，然后从 Github 上拉取 **VisionWorkbench** 和 **Stereo Pipeline** 的源码，在本地构建。仅建议非常牛*的用户这样做。  

如上所述，具有 ASP 依赖项的环境位于 Stereo Pipeline 仓库的 `conda` 目录中。下载完那些后，你可以在 Linux 上运行： 
```
conda env create -f asp_deps_2.7.0_linux_env.yaml
```

或者在Mac上：
```
conda env create -f asp_deps_2.7.0_osx_env.yaml
```

这就会创建一个`asp_deps`环境，然后激活它：
```
conda activate asp_deps
```

由 conda 创建的某些 .la 文件指向其他不可用的 .la 文件。鉴于此，那些文件应该被编辑并且把
```
/path/to/libmylibrary.la
```
替换为：
```
-L/path/to -lmylibrary
```  

可以用以下命令完成：
```
cd ~/miniconda3/envs/asp_deps/lib
mkdir -p  backup
cp -fv  *.la backup # back these up
perl -pi -e "s#(/[^\s]*?lib)/lib([^\s]+).la#-L\$1 -l\$2#g" *.la
```  

Linux 环境也需要包含 C 和 C++ 编译器。Mac 上编译器提供给 conda 的并不会正确构建 ASP ，因此建议使用 Apple 提供的 clang 和 clang ++。  

接着，创建一个工作目录：
```
buildDir=$HOME/build_asp
mkdir -p $buildDir
```  

在 Linux 上构建 VisionWorkbench 和 Stereo Pipeline:  
```
cd $buildDir
~/miniconda3/envs/asp_deps/bin/git clone \
    git@github.com:visionworkbench/visionworkbench.git
cd visionworkbench
git checkout 2.7.0 # check out the desired commit
mkdir -p build
cd build
~/miniconda3/envs/asp_deps/bin/cmake ..                                                 \
  -DASP_DEPS_DIR=$HOME/miniconda3/envs/asp_deps                                         \
  -DCMAKE_VERBOSE_MAKEFILE=ON                                                           \
  -DCMAKE_INSTALL_PREFIX=$buildDir/install                                              \
  -DCMAKE_C_COMPILER=$HOME/miniconda3/envs/asp_deps/bin/x86_64-conda_cos6-linux-gnu-gcc \
  -DCMAKE_CXX_COMPILER=$HOME/miniconda3/envs/asp_deps/bin/x86_64-conda_cos6-linux-gnu-g++
make -j10
make install

cd $buildDir
~/miniconda3/envs/asp_deps/bin/git clone \
git@github.com:NeoGeographyToolkit/StereoPipeline.git
cd StereoPipeline
git checkout 2.7.0 # check out the desired commit
mkdir -p build
cd build
~/miniconda3/envs/asp_deps/bin/cmake ..                                                 \
  -DASP_DEPS_DIR=$HOME/miniconda3/envs/asp_deps                                         \
  -DCMAKE_VERBOSE_MAKEFILE=ON                                                           \
  -DCMAKE_INSTALL_PREFIX=$buildDir/install                                              \
  -DVISIONWORKBENCH_INSTALL_DIR=$buildDir/install                                       \
  -DCMAKE_C_COMPILER=$HOME/miniconda3/envs/asp_deps/bin/x86_64-conda_cos6-linux-gnu-gcc \
  -DCMAKE_CXX_COMPILER=$HOME/miniconda3/envs/asp_deps/bin/x86_64-conda_cos6-linux-gnu-g++
make -j10
make install
```  

在 OSX 上构建 Visionbench 和 ASP(就像上面一样，但是省略了编译器):  
```
cd $buildDir
~/miniconda3/envs/asp_deps/bin/git clone \
  git@github.com:visionworkbench/visionworkbench.git
cd visionworkbench
git checkout 2.7.0 # check out the desired commit
mkdir -p build
cd build
~/miniconda3/envs/asp_deps/bin/cmake ..                                                 \
  -DASP_DEPS_DIR=$HOME/miniconda3/envs/asp_deps                                         \
  -DCMAKE_VERBOSE_MAKEFILE=ON                                                           \
  -DCMAKE_INSTALL_PREFIX=$buildDir/install
make -j10
make install

cd $buildDir
~/miniconda3/envs/asp_deps/bin/git clone \
  git@github.com:NeoGeographyToolkit/StereoPipeline.git
cd StereoPipeline
git checkout 2.7.0 # check out the desired commit
mkdir -p build
cd build
~/miniconda3/envs/asp_deps/bin/cmake ..                                                 \
  -DASP_DEPS_DIR=$HOME/miniconda3/envs/asp_deps                                         \
  -DCMAKE_VERBOSE_MAKEFILE=ON                                                           \
  -DVISIONWORKBENCH_INSTALL_DIR=$buildDir/install                                       \
  -DCMAKE_INSTALL_PREFIX=$buildDir/install
make -j10
make install
```  

## 2.5 构建文档  
ASP文档以ReStructured Text编码，并使用Sphinx-Doc系统（https://www.sphinx-doc.org）和sphinxcontrib-bibtex（https://sphinxcontrib-bibtex.readthedocs.io）构建。这些软件包已经是*asp_deps*环境的一部分，但是可以单独下载。  

要构建 PDF(不是 HTML)，还需要完整的 LaTex 发行版，该发行版目前无法使用 conda 进行安装，并且得针对你的系统进行安装。  

`docs`目录包含文档根目录。运行`make html`和`make latexpdf`会在 _build 子文件夹下创建当前版本的 HTML 和 PDF 文档。特别地， PDF 文档会出现在:
```
./_build/latex/asp_book.pdf
```  

## 2.6  conda 构建ASP和其依赖项  
这是在[第20节]()讨论的一个高级主题。  

## 2.7 设置优化 
最后，ASP 的最后一件事是设置 Vision Workbench 的渲染和日志设置。这个步骤是可选的，但为了获得最佳性能，应在此处进行一些考虑。  

Vision Workbench 对于 Stereo Pipeline来说是一个多线程影像处理库。通过编辑在 home 文件夹中隐藏的`.vwrc`文件，可以配置 Vision Workbench 处理数据的设置。下面是一个例子：  
```
# This is an example VW configuration file. Save this file to
# ~/.vwrc to adjust the VW log settings, even if the program is
#already running.

# General settings
[general]
default_num_threads = 16
write_pool_size = 40
system_cache_size = 1024000000 # ~ 1 GB

# The following integers are associated with the log levels
# throughout the Vision Workbench.  Use these in the log rules
# below.
#
#    ErrorMessage = 0
#    WarningMessage = 10
#    InfoMessage = 20
#    DebugMessage = 30
#    VerboseDebugMessage = 40
#    EveryMessage = 100
#
# You can create a new log file or adjust the settings
# for the console log:
#   logfile <filename>
#       - or -
#   logfile console

# Once you have created a logfile (or selected the console), you
# can add log rules using the following syntax. (Note that you
# can use wildcard characters '*' to catch all log_levels for a
# given log_namespace, or vice versa.)

# <log_level> <log_namespace>

# Below are examples of using the log settings.

# Turn on various logging levels for several subsystems, with
# the output going to the console (standard output).
[logfile console]
# Turn on error and warning messages for the thread subsystem.
10 = thread
# Turn on error, warning, and info messages for the
# asp subsystem.
20 = asp
# Turn on error, warning, info, and debug messages for the
# stereo subsystem.
30 = stereo
# Turn on every single message for the cache subsystem (this will
# be extremely verbose and is not recommended).
# 100 = cache
# Turn off all progress bars to the console (not recommended).
# 0 = *.progress

# Turn on logging of error and warning messages to a file for the
# stereo subsystem. Warning: This file will be always appended
# to, so it should be deleted periodically.
# [logfile /tmp/vw_log.txt]
# 10 = stereo
```  

上面的示例中可以实现很多可能的操作。下面让我们介绍最重要的选项以及用户在选择值时应注意的事项。  

### 2.7.1 性能设置  

`default_num_threds`(默认值为2)  
设置可用于渲染的最大线程数。当 stereo 的 `subpixel_rfne`在运行时，并且已经将`defalut_num_threads`设置为8时，你可能会注意到这里有10个线程正在运行。这并不是一个错误，你可以看到8个线程被用于渲染，1个线程用于保存`main()`函数的执行，最后有1个可选线程用作文件驱动程序的接口。  

通常设置参数的最好方式是和你系统处理器数目相等。如果系统支持超线程，确保算术中包含逻辑处理器的数目。为栅格化添加更多线程提高了立体管线对内存的要求。如果你系统内存有限，最好是降低`defalut_num_threads`选项。  

`write_pool_size`(默认值为21)  
`write_pool_size`选项代表等待写入磁盘的磁贴的最大等待池大小。大多数文件格式不允许随意乱码写入磁贴。但是，大多数方法将是成排的磁贴被乱序写入，而一行中的磁贴则必须按顺序写入。由于之前的约束，磁贴在栅格化后可能会花些时间在"写入池"等待才能将其写入磁盘。如果“写入池”满了，那么只能栅格化按顺序排列的磁贴。这使得立体管线表现的就像它只用了一个单一的处理器。  

增加`write_pool_size`使立体管线更能够使用系统中的所有处理核。将该值设置的太大可能意味着过多使用内存，因为在等待写入时必须将更多的图像部分保留在内存中。此数目应大于线程数目，可能为20。  

`system_cache_size`(默认值为805306368)  
从硬盘驱动访问文件可能非常慢。如果应用程序需要对输入文件进行多次传递，则尤其糟糕。为了提高性能， Vision Workbench 通常会将输入文件存储在内存中，以便快速访问。该文件存储称为“系统缓存”，其最大大小由`system_cache_size`决定，默认值为768MB。  

这个值设置的太高可能会导致你的应用崩溃。通常建议将这个值设置在系统最大可用内存的1/4左右。这个属性的单位是字节。  

这些值的建议是基于 ASP 中区块匹配算法的使用。当使用内存密集型算法(例如 SGM)时，你可能希望降低其中一些值(例如高速缓存大小)，以留出更多内存供算法使用。  

### 2.7.2 日志设定  
立体管线在控制台中显示的信息按详细程序分为几个命名空间。上面显示的`.vwrc`文件中提供了一个自定义的立体管线输出的示例。  

立体管线中的几个工具，包括`stereo`，自动将控制台中显示的信息附加到当前输出目录中的日志文件中。这些日志还包含一些系统和设置的数据，这些数据可能有助于解决工具问题。  

也可以指定所有工具都附加到全局日志文件中，如`.vwrc`文件中所示。