可以，你现在这些字体已经在 `~/Documents/Fonts` 里了。我们直接把它整理成一个**你自己的字体库**，然后安装到当前 Kubuntu 用户环境，不需要 `sudo`，也不会污染系统目录。

你现在这批其实已经相当不错了：

* 思源宋体
* 霞鹜文楷 / 文楷 Mono
* 霞鹜新晰黑 / 新致宋
* 思源宋体
* MiSans
* OPPO Sans
* 阿里巴巴普惠体
* 微软雅黑
* 宋体 / 新宋体
* 黑体
* 楷体 / SimKai
* 仿宋
* 等线

## 直接执行这一套

先进入你现在的目录：

```bash
cd ~/Documents/Fonts
```

创建一个专门的用户字体目录：

```bash
mkdir -p ~/.local/share/fonts/NeoChinese
```

然后把你下载的字体全部复制进去：

```bash
find ~/Documents/Fonts -maxdepth 1 -type f \
  \( -iname "*.ttf" -o -iname "*.otf" -o -iname "*.ttc" \) \
  -exec cp -f {} ~/.local/share/fonts/NeoChinese/ \;
```

刷新字体缓存：

```bash
fc-cache -f -v
```

然后检查：

```bash
fc-list :lang=zh-cn family | sort -u
```

你应该能看到类似：

```text
Alibaba PuHuiTi
KaiTi
LXGW Neo XiHei
LXGW Neo XiHei Plus
LXGW Neo ZhiSong
LXGW Neo ZhiSong Plus
LXGW WenKai
LXGW WenKai Mono
MiSans
OPPO Sans
SimHei
SimKai
SimSun
Source Han Serif SC
Microsoft YaHei
新宋体
方正仿宋简体
等线
```

---

## 我建议顺手把 WOFF2 清掉

你现在有：

```text
SourceHanSerifSC-VF.otf
SourceHanSerifSC-VF.otf.woff2
SourceHanSerifSC-VF.ttf
SourceHanSerifSC-VF.ttf.woff2
```

其中：

```text
.woff2
```

主要是给网页使用的，不是桌面系统字体需要的格式。

所以你的**桌面字体库**里可以只保留：

```text
SourceHanSerifSC-VF.otf
SourceHanSerifSC-VF.ttf
```

执行：

```bash
rm -f ~/.local/share/fonts/NeoChinese/*.woff2
fc-cache -f -v
```

这样干净很多。

---

# 再做一个非常重要的检查

看看系统到底识别了多少字体：

```bash
fc-list :lang=zh-cn family | sort -u | wc -l
```

然后：

```bash
fc-list :lang=zh-cn family | sort -u
```

如果你想确认某几个字体是否正常：

```bash
fc-match "微软雅黑"
fc-match "宋体"
fc-match "黑体"
fc-match "楷体"
fc-match "仿宋"
fc-match "等线"
```

例如：

```text
微软雅黑: "Microsoft YaHei"
宋体: "SimSun"
黑体: "SimHei"
楷体: "KaiTi"
```

如果能这样匹配，**LibreOffice/WPS 打开 Windows 文档的时候就舒服很多了。**

---

# 你的字体库现在建议这样保留

我建议：

```text
~/Documents/Fonts/
```

作为你的**原始字体仓库**。

而：

```text
~/.local/share/fonts/NeoChinese/
```

作为 KDE 实际使用的字体目录。

也就是说：

```text
下载的原始字体
       │
       ▼
~/Documents/Fonts
       │
       │ cp
       ▼
~/.local/share/fonts/NeoChinese
       │
       ▼
     fc-cache
       │
       ▼
Kubuntu / KDE / Chrome / LibreOffice
```

这样以后你再往 `~/Documents/Fonts` 扔字体，只需要重新执行：

```bash
find ~/Documents/Fonts -maxdepth 1 -type f \
  \( -iname "*.ttf" -o -iname "*.otf" -o -iname "*.ttc" \) \
  -exec cp -f {} ~/.local/share/fonts/NeoChinese/ \;

fc-cache -f
```

就可以了。

---

### 还有一个小细节

你这里有：

```text
LXGWWenKaiMono-Regular.ttf
```

这个**千万别拿去当普通 KDE 中文字体**，它是等宽字体。

它特别适合：

```text
Konsole
VS Code
Vim
Neovim
代码编辑器
```

而你的：

```text
LXGWWenKai-Regular.ttf
LXGWWenKai-Medium.ttf
```

才适合：

```text
浏览器
Markdown
阅读
文档
```

等你把这些装好之后，**下一步我建议直接帮你把 Kubuntu 的 KDE、Konsole、Chrome、LibreOffice 四套字体分别配置好**。这样就不是单纯“字体装进去了”，而是整个 Kubuntu 的中文显示真正统一起来。

