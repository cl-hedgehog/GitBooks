# Mac_Talk


### 1.如何让Finder显示隐藏文件和文件夹

1. 打开「终端」应用程序。
2. 输入如下命令：

  > defaults write com.apple.finder AppleShowAllFiles -boolean true ; killall Finder

3. 按下「Return」键确认。

  现在你将会在 Finder 窗口中看到那些隐藏的文件和文件夹了。
  如果你想再次隐藏原本的隐藏文件和文件夹的话，将上述命令替换成
  >defaults write com.apple.finder AppleShowAllFiles -boolean false ; killall Finder








