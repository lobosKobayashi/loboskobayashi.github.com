---
layout: post
category : Wikipedia
tagline: "Supporting tagline"
tags : [pysf, machine_learning]
---
{% include JB/setup %}

##■■ rpy2.py モジュールのインストール
D:\my\sf\blog\linearPrediction\SVM>pip install rpy2
Downloading/unpacking rpy2

私の三時間のロスを、他の方が繰り返さないだけの価値があるだろう

##■■ pip install rpy2 の顛末

pip install rpy2

      Running setup.py egg_info for package rpy2

        Error: Tried to guess R's HOME but no R command in the PATH.
        Complete output from command python setup.py egg_info:
        running egg_info

    writing pip-egg-info\rpy2.egg-info\PKG-INFO

    writing top-level names to pip-egg-info\rpy2.egg-info\top_level.txt

    writing dependency_links to pip-egg-info\rpy2.egg-info\dependency_links.txt

    warning: manifest_maker: standard file '-c' not found



    Error: Tried to guess R s HOME but no R command in the PATH.

pip install rpy2

      Running setup.py egg_info for package rpy2

        Error: Tried to guess R s HOME but no R command in the PATH.
        Complete output from command python setup.py egg_info:
        running egg_info

R をインストールし PATH に "D:\lng\R31\bin" を追加した

    .
    .
    .
        snipped
    creating build\temp.win32-2.7\Release\rpy\rinterface

    C:\MinGW32-xy\bin\gcc.exe -mno-cygwin -mdll -O -Wall -DWin32=1 -I.\rpy\rinterfac
    e -IC:\Python27\include -IC:\Python27\PC -ID:/lng/R31/include/i386 -c .\rpy\rint
    erface\_rinterface.c -o build\temp.win32-2.7\Release\.\rpy\rinterface\_rinterfac
    e.o

    In file included from .\rpy\rinterface\_rinterface.c:58:0:

    .\rpy\rinterface\/_rinterface.h:8:15: fatal error: R.h: No such file or director
    y

    compilation terminated.

    error: command 'gcc' failed with exit status 1

    ----------------------------------------
    Command C:\Python27\python.exe -c "import setuptools;__file__='D:\\my\\sf\\blog\
    \linearPrediction\\SVM\\build\\rpy2\\setup.py';exec(compile(open(__file__).read(
    ).replace('\r\n', '\n'), __file__, 'exec'))" install --single-version-externally
    -managed --record c:\users\kk\appdata\local\temp\pip-is_htr-record\install-recor
    d.txt failed with error code 1
    Storing complete log in C:\Users\kk\AppData\Roaming\pip\pip.log
    
    .\rpy\rinterface\/_rinterface.h:8:15: fatal error: R.h: No such file or director

    _rinterface.h:8:15: fatal error: R.h: No such file or director
    http://stackoverflow.com/questions/24130310/rpy2-installation-error-on-windows-8-anaconda

fatal error: R.h: No such file or director

[stackoverflow](http://stackoverflow.com/questions/24130310/rpy2-installation-error-on-windows-8-anaconda) にある

    Dr. Gohlke's binary is probably the easiest solution. But you need to change the Python installation path in your registry for this method to work. The relevant key is in HKEY_LOCAL_MACHINE\SOFTWARE\Python\PythonCore\2.7\InstallPath. Change it so that the anaconda Python is the default python installation.

    Windows Registry Editor Version 5.00

    [HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Python\PythonCore\2.7\InstallPath]
    @="C:\\Python27\\"


教訓 Google に頼りすぎるな。インストールでのエラー･メッセージを読んで、自分でエラー原因を推測するぐらいをしてから、インターネット検索に移れ。
