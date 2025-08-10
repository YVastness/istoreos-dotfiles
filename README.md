# 初始化 iStoreOS

## 修改登录 IP 地址

1. 登录 iStoreOS:

* WebUI:  
在浏览器中输入 iStoreOS 的默认 IP 地址(通常是 `192.168.100.1`)，然后使用用户名 `root` 和密码 `admin` 登录。
  
* SSH:  
使用 SSH 客户端连接到 iStoreOS 的 IP 地址，用户名也是 `root`，密码是 `admin`。
  
2. 找到网络配置文件:
* 网络配置文件通常位于 `/etc/config/network`。

3. 编辑配置文件:
* 使用文本编辑器(例如 `vi` 或 `nano`) 打开配置文件。
* 找到 LAN 接口的配置段落。
* 修改 `option ipaddr` 的值为新的 IP 地址。
* 修改 `option netmask` 的值为新的子网掩码。
* 如果需要，修改 `option gateway` 的值为新的网关地址。
* 保存并关闭文件。

4. 重启网络服务:
* 在 WebUI 中，找到“系统”->“重启”并点击重启。
* 在 SSH 中，输入 `service network restart` 命令并回车。

5. 验证:
* 在浏览器中输入新的 IP 地址，应该能够访问 iStoreOS 的WebUI。
* 在终端中输入 ping 新的 IP 地址，应该能够 ping 通。

## 给系统扩容



https://github.com/user-attachments/assets/ca5ff105-64df-4dde-b3a9-2fe1844696c1

## 安装 openclash

```bash
  # 可以上 GitHub 找最新版本下载
  curl -L -o /tmp/openclash.ipk https://github.com/vernesong/OpenClash/releases/download/v0.46.137/luci-app-openclash_0.46.137_all.ipk
  opkg install /tmp/openclash.ipk
# 安装内核
  curl -L -o /tmp/mihomo.gz https://github.com/MetaCubeX/mihomo/releases/download/v1.19.12/mihomo-linux-amd64-compatible-v1.19.12.gz
  mkdir -p /etc/openclash/core/
  gunzip -c /tmp/mihomo.gz > /etc/openclash/core/clash_meta
  
```

## 切换 shell 为 bash

在 OpenWrt 上通过 SSH 登录后默认使用的是 BusyBox 提供的 ash，如果你希望改为 bash 并切换到它，可以使用以下方法：

修改 /etc/passwd 将默认 shell 改为 bash  
1.	安装 Bash（如尚未安装）：

```bash
opkg update
opkg install bash
```
2. 确保 `/etc/shells` 中包含 Bash 路径（可选但推荐）：  
```bash
echo "/bin/bash" >> /etc/shells
```
3.	编辑 `/etc/passwd` 将对应用户的 shell 修改成 `/bin/bash`，例如：  
```bash
root:x:0:0:root:/root:/bin/bash
```

这样登录时就会默认进入 Bash。 ￼

## 安装软件

```bash
  opkg install git git-http make golang less
  # 安装 cargo
  curl https://sh.rustup.rs -sSf | sh

  cargo install starship --locked
  cargo install eza
  cargo install du-dust
  cargo install --locked bat
  cargo install --locked yazi-fm yazi-cli

  go install github.com/jesseduffield/lazydocker@latest

  # 下载安装 chezmoi
  git clone https://github.com/twpayne/chezmoi.git
  cd chezmoi
  make install-from-git-working-copy
  mv ~/go/bin/chezmoi /bin
  rm -rf ../chezmoi

  # 安装 fzf
  git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
  ~/.fzf/install

  # 安装 neovim
  cd /tmp
  git clone https://github.com/neovim/neovim
  cd neovim && make CMAKE_BUILD_TYPE=RelWithDebInfo # 如果报错，可以重复执行 make CMAKE_BUILD_TYPE=RelWithDebInfo。
  make install
  cp /usr/local/bin/nvim /usr/bin
  git clone https://github.com/LazyVim/starter ~/.config/nvim # 安装 LazyVim
```
