### LinkMapParser
#### 背景
LinkMapParser是一个可以帮助开发者分析Xcode生成的link map文件的工具。Xcode生成的link map文件中记录了iOS程序包的各个组成部分，包括所有的类、方法及变量等等。通过分析这个文件，可以知道程序包中各个模块的体积大小，以用于后续优化。

#### 使用方法
##### 1. 安装Python环境
LinkMapParser是一个Python脚本，运行该脚本需要开发者的机器有Python的运行环境，安装Python的方法可以查阅相关资料。Python版本为2.7。

##### 2. 生成link map文件
Xcode默认是不生成link map文件的。生成link map文件需修改项目中的Build Settings，选择Target的Build Settings，修改Write Link Map File为Yes，修改Path to Link Map File为你需要的地址，然后编译程序，即可在该地址生成相应的link map文件。

##### 3. 运行工具
该工具支持分析一个link map文件和比较两个link map文件，运行的命令分别为：

###### 分析一个link map文件

```
python parselinkmap.py $map_link_file_path
```
输出结果类似于：

```
================================================================================
        demoData/TestCleanPackage-LinkMap-normal-x86_64.txt各模块体积汇总
================================================================================
Creating Result File : demoData/BaseLinkMapResult.txt
AppDelegate.o                                     0.01M
ViewController.o                                  0.00M
TestCleanPackage.app.xcent                        0.00M
UnUsedClass.o                                     0.00M
main.o                                            0.00M
libobjc.tbd                                       0.00M
linker synthesized                                0.00M
Foundation.tbd                                    0.00M
UIKit.tbd                                         0.00M
总体积:                                           0.01M
```

demo中只有一个Bundle，可以看出各个class文件在安装包中所占大小，如AppDelegate占用0.01M。

比较两个link map文件

```
python parselinkmap.py $base_map_link_file_path $target_map_link_file_path
```

LinkMapParser会分析两个map link文件，然后比较各个模块的体积是否有变化，最后列出体积变大的模块。

输出结果类似于：

```
================================================================================
                     demoData/BaseLinkMap.txt各模块体积汇总
================================================================================
Creating Result File : demoData/BaseLinkMapResult.txt
AppDelegate.o                                     0.01M
ViewController.o                                  0.00M
TestCleanPackage.app.xcent                        0.00M
UnUsedClass.o                                     0.00M
main.o                                            0.00M
libobjc.tbd                                       0.00M
linker synthesized                                0.00M
Foundation.tbd                                    0.00M
UIKit.tbd                                         0.00M
总体积:                                           0.01M






================================================================================
                    demoData/TargetLinkMap.txt各模块体积汇总
================================================================================
Creating Result File : demoData/TargetLinkMapResult.txt
AppDelegate.o                                     0.64M
ViewController.o                                  0.00M
TestCleanPackage.app.xcent                        0.00M
UnUsedClass.o                                     0.00M
main.o                                            0.00M
libobjc.tbd                                       0.00M
linker synthesized                                0.00M
Foundation.tbd                                    0.00M
UIKit.tbd                                         0.00M
总体积:                                           0.64M






================================================================================
                                    比较结果
================================================================================
模块名称                                          基线大小  目标大小  是否新模块
AppDelegate.o                                     0.01M     0.64M
```

