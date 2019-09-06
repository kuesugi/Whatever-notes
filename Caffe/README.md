## Caffe的各种坑 (Ubuntu 18 / macOS Mojave / Docker)

因为项目的关系, 需要用到**caffe**. 但是很多人, 包括我自己, 在尝试安装后, 从下载到配置 等等, 遇到了一系列的问题. 有的没有解决办法, 有的存在但在你的机器上并不work. 

我在Ubuntu 18 (VMware) 和 mac OSX分别遇到了不同的问题, 在尝试了大概两天过后我放弃了. 最后没有办法 只能通过Docker来实现 (最后成功了 `;D` ). 在这里说说各种坑, 包括上面两个系统以及Docker的, 让大家避免错误, 少走弯路. 如果你觉得有用或有其他问题, 请留言 (Github请在Issues里留言).

- #### Ubuntu 18.04 安装CUDA + cuDNN + Caffe:

	- https://blog.csdn.net/u011021773/article/details/81298666
		- 这篇教程非常详细, 有兴趣的可以参考. 包含了很多错误解决办法

- #### macOSX + Anaconda + Python3.7 安装 Caffe:

	- https://www.piddnad.cn/2018/08/19/Installing_Caffe_on_MacOS_10.14/

	- 作者遇到的错误以及解决办法:

		- ```markdown
			1. make all 时一堆错误，错误出在文件名带 protobuf 的文件
			protobuf 版本问题，需要安装3.6.0以下版本，具体参考前文。
			
			2. make pycaffe 时错误，提示library not found fpr -lboost_python
			将配置文件84行
			 PYTHON_LIBRARIES := boost_python3 python3.7m
			改为
			 PYTHON_LIBRARIES := boost_python37 python3.7m
			```

- #### macOSX 安装 Caffe cpu版:

	- https://blog.csdn.net/u014027643/article/details/84139107

		- 这篇教程在我试图在macOSX里安装caffe给予了很多帮助, 下面是当时我遇到的错误, **作者**给出的解决办法:

		- ```markdown
			1. make runtest 如果出现错误dyld: Library not loaded: @rpath/libhdf5_hl.100.dylib,将添加libhdf5_hl.100.dylib所在路径添加到rpath:
			install_name_tool -add_rpath '/Users/lxy/anaconda3/lib' /Users/lxy/caffe/build/tools/caffe
			2. 继续make runtest，报错dyld: Library not loaded:@rpath/libhdf5_hl.100.dylib,再次添加rpath:
			install_name_tool -add_rpath '/Users/lxy/anaconda3/lib' /Users/lxy/caffe/build/test/test_all.testbin 
			
			// 附: lxy是作者的目录, 请根据你的自行改变
			```

	- https://blog.csdn.net/jixinpu/article/details/82965768

		- 也是一篇好文章, 包括错误及其解决办法: 
			- `Error: Xcode alone is not sufficient on High Sierra`
			- `make: *** [.build_release/src/caffe/proto/caffe.pb.o] Error 1`

	- https://blog.csdn.net/huangynn/article/details/50898661

		- 没太用上, 需要的可以参考一下

	- https://blog.csdn.net/rnZuoZuo/article/details/89277381

		- 包含错误及其解决办法:
			- XCode, protobuf, opencv, leveldb (这个东西的错误让我快吐了), python的编译 等

	- https://jinkey.ai/post/tech/mac-ren-yi-pythonhuan-jing-an-zhuang-caffe-de-zhong-ji-jiao-cheng

		- 没太用上, 但是看上去不错, 需要的可以参考

	- https://github.com/BVLC/caffe/issues/5601

		- CMake报错的解决办法

	- 遇到 `Makefile: recipe for target '.build_release/lib/libcaffe.so.1.0.0-rc' failed` 类似的错误:

		- https://github.com/BVLC/caffe/issues/3671  当中Aaron9477的回答, 大家可以往下翻也可以看到其他的解决办法:
			- I think the reason is you use `make` to compile, which makes caffe's python port only find libraries in this catalog. Maybe you use `cmake `to compile and it could work.
				`make clean`
				`cd caffe-master`
				`mkdir build`
				`cd build`
				`cmake ..`
				`make all -j8`
				I hope I could help you!

	- 遇到`CMake Error at CMakeLists.txt:5 (find_package):
		Could not find a configuration file for package "OpenCV" that is compatible
		with requested version "3".` :

		- https://github.com/Homebrew/homebrew-science/issues/2466
			- zwlu的回答: `brew ln opencv3 --force`

- #### Mac安装Docker:

	- https://yeasy.gitbooks.io/docker_practice/install/mac.html
		- 详细 + 简单

- #### Docker安装caffe cpu-only版:

	- https://medium.com/@rosstopherkeen/using-caffe-on-macos-os-x-3f923cd499e
		- 这里的docker image (映像) 来自于bvlc (我本人选择的)
	- https://blog.csdn.net/elaine_bao/article/details/53117676
		- 这里的docker image 来自elezar 
	- **我的选择是通过教程2来安装教程1里的image**

- #### Ubuntu / macOSX 安装 dlib:

	- https://www.pyimagesearch.com/2017/03/27/how-to-install-dlib/
		- 其中的Step 2是可以跳过的 (Step #2: Access your Python virtual environment (optional))

- #### Ubuntu 安装 PyQt4:

	- 由于有GUI程序, 我在我的**docker**中安装了**PyQt4**, 以下是教程
		- https://blog.csdn.net/xingchengmeng/article/details/53939539

- #### 遇到缺少 QtWebKit 的问题:

	1. 提取QtWebKit.so (简单方法/亲测有效): **t1m0thyj** 的两个回答 https://github.com/ninja-ide/ninja-ide/issues/1985

	2. 使用PySide (个人没尝试): https://stackoverflow.com/questions/2922711/importerror-no-module-named-qtwebkit

- #### 缺少 `shape_predictor_68_face_landmarks.dat` :

	- Dyex719的回答: https://github.com/cmusatyalab/openface/issues/76

- #### Docker 中出现 `cannot connect to X server`:

	- 该方法不知道是否适用于其他系统. 本人: macOSX Mojave + Docker

	- 在mac的terminal里输入`socat TCP-LISTEN:6000,reuseaddr,fork UNIX-CLIENT:\"$DISPLAY\"`

		- 可以是`6001`, 如果运行上面命令显示`6000`被占用的话

	- 安装Xquartz: `brew install xquartz`. 这一步很重要: **把mac账户log in然后log out. 具体操作是mac界面左上角最下面的一项**

	- 在terminal另一个窗口输入`open -a Xquartz`打开该程序. 这时候你应该会看到一个白色的窗口弹出

	- 点击最上面一栏的XQquartz的偏好设置/Preferences, 最后一项Security, 打开Allow connections...

	- 在白色窗口内输入`ifconfig en0`, inet 后的ip地址记下

	- 输入 `xhost +` 看到access deny类似的字样 (原句忘记了) 即可

	- 接下来就可以运行docker了. 在你的docker运行命令在插入`-e DISPLAY=你的IP:0`. 在你的docker中就可以运行你的GUI程序了

	- 如果上述步骤不work, 请移步参考: 

		- https://cntnr.io/running-guis-with-docker-on-mac-os-x-a14df6a76efc

		- https://medium.com/@sahputra/running-qt-application-using-docker-on-macos-x-ad2e9d34532a

			

- #### 推荐 Face recognition 项目:

	- https://github.com/ageitgey/face_recognition/blob/master/README_Simplified_Chinese.md

- #### 发现了一个新奇的东西: Floydhub 的 dockerfile (包含众多流行的机器学习 深度学习的库)

	- https://github.com/floydhub/dl-docker

`祝宁成功`
