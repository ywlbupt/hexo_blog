title: Git子模块的维护
date: 2016-01-12 01:01:01
tags: 
    - git
    - TBD
description: git submodule的建立于维护
---

*   添加Existing git子模块

        git submodule add https://github.com/ywlbupt/prj_note.wiki.git NOTE
*   在父仓库中更新子模块
    具体需要进去子文件夹中pull

        cd NOTE
        git pull

    或者是在父仓库中执行以下命令 

        git submodule foreach git pull

*   删除子模块
    git 并不支持直接删除Submodule需要手动删除对应的文件

        git rm NOTE

*   在clone父模块的时候初始化子模块采用递归参数 --recursive

        git clone https://github.com/ywlbupt/Repository_Name.git --recursive

*   子模块pull后，需要在父模块中执行以下命令，

        git submodule update

    更复杂一些，如果你的 submodule 又依赖了 submodule，那么很可能你需要在 git pull 和 git submodule update 之后，再分别到每个 submodule 中再执行一次 git submodule update，这里可以使用 git submodule foreach 命令来实现：` git submodule foreach git submodule update`
