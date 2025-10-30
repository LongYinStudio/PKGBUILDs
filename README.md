# 一些自己维护的AUR

## 流程

1. 首先，前往 [https://aur.archlinux.org](https://aur.archlinux.org) 创建一个账户。确保添加正确的 SSH 密钥。接下来，使用以下命令克隆一个空的 Git 存储库。

```bash
git clone https://aur.archlinux.org/your-repo-name
```

> 完成上述步骤后，创建一个名为 `PKGBUILD` 的文件。一旦文件创建成功，您可以继续进行下一步。

2. 编写一个 `PKGBUILD` 文件

> 只列出了部分配置项，还有很多(如package()，prepare()，provides等等)，详细参考 [https://man.archlinux.org/man/PKGBUILD.5](https://man.archlinux.org/man/PKGBUILD.5)

```ini
# Maintainer: Username <email>

pkgname=<pkgname> # 包名
pkgver=1.0.0 # 版本
pkgrel=1 # 发布号
pkgdesc="Description of your app" # 描述
arch=('x86_64' 'aarch64') # 支持的架构
url="https://github.com/<user>/<project>"
license=() # 许可证
depends=() # 运行时依赖
makedepends=() # 构建时依赖
options=('!strip' '!emptydirs') # 如 !strip（保留调试符号）、!emptydirs（不创建空目录）等。
# install=${pkgname}.install # 脚本
# source_x86_64=("https://github.com/<user>/<project>/releases/download/v$pkgver/appname_"$pkgver"_amd64.deb")
# source_aarch64=("https://github.com/<user>/<project>/releases/download/v$pkgver/appname_"$pkgver"_arm64.deb")
# source=("$pkgname-$pkgver.tar.gz::https://github.com/author/project/archive/v$pkgver.tar.gz")
# sha256sums_x86_64、sha256sums_aarch64
sha256sums=('SKIP') # source里的文件校验（sha256sums\md5sums等等），文件不要用 'SKIP'
```

3. 生成 `.SRCINFO`

> 为了将您的 `repo` 推送到 `aur`，您必须生成一个 `srcinfo` 文件。可以使用以下命令完成此操作。

```bash
makepkg --printsrcinfo >> .SRCINFO
```

4. 测试

测试这个应用程序非常简单。你只需要在与 `pkgbuild` 文件相同的目录中运行 `makepkg -f` 命令，然后看它是否正常工作。

构建并安装: `makepkg -si`

5. 发布

最后，在测试阶段结束后，您可以使用以下命令将应用程序发布到用户存储库。

```bash
git add .
git commit -m "feat: new feature"
git push
```

6. 安装

```bash
yay -S pkgname
或者
paru -S pkgname
```

7. 其他

> 仓库结构

```
PKGBUILDs/
├── package1/
│   ├── PKGBUILD
│   ├── .SRCINFO
│   ├── package1.install
│   └── README.md
├── package2/
│   ├── PKGBUILD
│   ├── .SRCINFO
│   └── any.patch
└── templates/
    ├── PKGBUILD.template
    └── README.template
```

> 环境

```bash
# 安装基础开发工具
sudo pacman -S --needed base-devel git

# 设置 makepkg 配置（可选）多线程编译
echo "MAKEFLAGS=\"-j$(nproc)\"" | sudo tee -a /etc/makepkg.conf
```
