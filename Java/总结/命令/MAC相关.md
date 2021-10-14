## 复制文件

```markdown
sudo cp -r ~/mytomcat.tar /Volumes/OutData/Movies
```
## 查看进程

```markdown
查看进程
lsof -i tcp:6379
关闭进程
kill -9 
```
## 重置launchpad

```markdown
defaults write com.apple.dock ResetLaunchPad -bool true; killall Dock
```
## 更改hosts

```markdown
sudo vim /etc/hosts
```
## 设置环境变量

```markdown
vi ./.bash_profile
```
## 重置launch

```markdown
defaults write com.apple.dock ResetLaunchPad -bool true; killall Dock
```