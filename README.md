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


设想一下，你正在调试某个问题，刚刚烧录了一个镜像到flash，但这时正好有个不得不参
加的会议，等开完会了，你能明确的记得flash上的镜像是你修改了某个模块的哪行代码而
编译出的？或者有没有可能，在你开会期间，flash被别人烧录了？

当然在openbmc镜像中，有个/etc/os-release文件，能够看到source tree上的最后一个
commit id, 往往能够起到区分镜像版本的作用，可如果你通过`devtool modify`修改的某
个模块的代码，这个commit id是无法反映出任何关于这个模块的改动的

这时你可能会说，没关系的，我用sdk workflow，嗯，这很好，不过此时你可能还需要一
个 `git-obmc-slr`工具


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
