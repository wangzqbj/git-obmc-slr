# git-obmc-slr

Imagine this scenario: you're debugging an issue and have just flashed an image
to the device. However, an important meeting comes up that you must attend.
After the meeting, will you still be able to remember which lines of code you
modified in a specific module before compiling the image? Or is it possible
that someone else re-flashed the device while you were in the meeting?

In OpenBMC images, there is an /etc/os-release file that shows the commit ID of
the last change on the source tree, which can often help differentiate between
image versions. But if you've modified a module using devtool modify, the
commit ID won't reflect any changes you made to that module.

At this point, you might say, "No problem, I'm using the SDK workflow." That's
great! In that case, you’ll likely also need a tool like git-obmc-slr.

If you're using the sdk workflow, your development process likely involves
making some changes in a repo, then committing the code to a local or remote
git server. To integrate these changes into the Bitbake build system, you also
need to update the SRCREV of the repo. If it's your first time debugging the
repo, you may also need to modify the SRCURI. The typical process is as follows:

```
cd ${_WORKSPACE_}/$repo
do some changes
git commit

cd ${_WORKSPACE_}/$repo
modify SRCREV/SRCURI for repo
git commit

bitbake obmc-phosphor-image
```

In this workflow, you can obtain the last commit ID of the source tree from the
/etc/os-release file. Using this commit, you can trace back to the SRCREV
commit ID of the repo, thus confirming which version is currently being
debugged.

However, during debugging, you might make numerous modifications to the repo,
requiring multiple updates to the SRCREV. This process can become tedious, and
you might be tempted to set SRCREV to AUTOREV as a shortcut. But if you do this,
you will no longer be able to track the repo's commit ID from the source tree,
meaning you won’t be able to determine the version of the repo from the
/etc/os-release file. After flashing the firmware, if your work is interrupted,
you might forget which version of the repo was flashed.

Therefore, you may need a tool like git-obmc-slr (sync local repo) to help you
update the SRCREV without resorting to the shortcut of using AUTOREV.

设想一下，你正在调试某个问题，刚刚烧录了一个镜像到flash，但这时正好有个不得不参
加的会议，等开完会了，你能明确的记得flash上的镜像是你修改了某个模块的哪行代码而
编译出的？或者有没有可能，在你开会期间，flash被别人烧录了？

当然在openbmc镜像中，有个/etc/os-release文件，能够看到source tree上的最后一个
commit id, 往往能够起到区分镜像版本的作用，可如果你通过`devtool modify`修改的某
个模块的代码，这个commit id是无法反映出任何关于这个模块的改动的

这时你可能会说，没关系的，我用sdk workflow，嗯，这很好，不过此时你可能还需要一
个 `git-obmc-slr`工具

如果你使用的是`sdk workflow`的话，你的开发流程可能是先在repo下面进行一些修改，
然后将代码提交到本地或者远程的git server，为了使修改能集成到bitbake编译系统，还
需要修改一下repo对应的SRCREV，如果是第一次对repo的调试，还需要修改SRCURI, 流程
如下:

```
cd ${_WORKSPACE_}/$repo
do some changes
git commit

cd ${_WORKSPACE_}/$repo
modify SRCREV/SRCURI for repo
git commit

bitbake obmc-phosphor-image
```

这种流程下，可以通过/etc/os-release文件获取到source tree上最后的commit id, 通过
这个commit，能够查到 repo SRCREV对应的commit id, 进而能够确认当前所用的调试版本

当然，调试期间，你可能会对repo做相当多次的修改，需要修改很多次的SRCREV，此过程
相当无聊，很可能为了偷懒你会将SRCREV修改成"AUTOREV", 这样的话，你就无法再通过
source tree的commit id查到repo的commit id了，也就是说无法通过/etc/os-release文
件得知repo的修改版本，这样当你刷写了flash之后，工作被打断，很可能就会忘记烧录到
flash的repo版本是哪个

所以，你可能需要一个工具 git-obmc-slr(stands for sync local repo)，来帮助你更新
repo的SRCREV, 而不是“偷懒”使用“AUTOREV"


Note: Most of the code copied from https://github.com/openbmc/openbmc-tools/blob/master/openbmc-autobump/openbmc-autobump.py

## Installation

```
cp git-obmc-slr ~/.local/bin
```

## Usage

```
export _WORKSPACE_="/path/to/workspace"
cd ${_WORKSPACE_}/$repo
do some changes
git add/commit
git obmc-slr

cd ${_WORKSPACE_}/openbmc
git log # check the commit id
```

[![asciicast](https://asciinema.org/a/wfAw9yfBu9X1HGhBShvEyMuu3.svg)](https://asciinema.org/a/wfAw9yfBu9X1HGhBShvEyMuu3)
