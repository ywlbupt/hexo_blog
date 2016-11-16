title: ubuntu-cmd~update-hold设置某软件不更新
date: 2016-11-15 00:31:21
category: [Ubuntu, Cmd]
tags:
description:
---
[Toc]

## Holding

There are four ways of holding back packages: with dpkg, apt, aptitude or dselect.

### dpkg

Put a package on hold:
```bash
echo "package hold" | sudo dpkg --set-selections
```

Remove the hold:
```bash
echo "package install" | sudo dpkg --set-selections
```

Display the status of your packages:
```bash
dpkg --get-selections
```

Display the status of a single package:
```bash
dpkg --get-selections | grep "package"
```

### apt

我用这个命令hold住vim 8.0的源代码安装

Hold a package:
```bash
sudo apt-mark hold package_name
```

Remove the hold:
```
sudo apt-mark unhold package_name
```

#### sudo apt-get upgrade

通过执行`sudo apt-get upgrade`
会有如下提示：

```bash
The following packages have been kept back:
  vim
  0 upgraded, 0 newly installed, 0 to remove and 1 not upgraded.
```


### aptitude

Hold a package:

```bash
sudo aptitude hold package_name
```

Remove the hold:
```bash
sudo aptitude unhold package_name
```
