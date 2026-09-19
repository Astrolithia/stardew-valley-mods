# Stardew Valley Mods

这个仓库用于同步这套 SMAPI mods。

> 此仓库是 **公开仓库**。朋友无需被邀请；只要配置 GitHub SSH key（或把下面 URL 改成 HTTPS）即可克隆。

## 给朋友：macOS 安装

先完全退出 Stardew Valley，然后在终端粘贴以下命令。它会寻找 Steam 安装的 Stardew Valley `Mods` 目录；找到后，将本仓库克隆进独立的 `stardew-valley-mods` 子目录，不会覆盖朋友已有的 mods。

```bash
set -e
REPO='git@github.com:Astrolithia/stardew-valley-mods.git'
CANDIDATES=(
  "$HOME/Library/Application Support/Steam/steamapps/common/Stardew Valley/Contents/MacOS/Mods"
  "/Volumes"/*/SteamLibrary/steamapps/common/Stardew\ Valley/Contents/MacOS/Mods
)

MODS_DIR=''
for path in "${CANDIDATES[@]}"; do
  if [ -d "$path" ]; then
    MODS_DIR="$path"
    break
  fi
done

if [ -z "$MODS_DIR" ]; then
  echo '没有找到 Mods 目录。请在 Steam 中定位 Stardew Valley 安装目录后重试。' >&2
  exit 1
fi

TARGET="$MODS_DIR/stardew-valley-mods"
if [ -e "$TARGET" ]; then
  echo "目标已存在：$TARGET" >&2
  echo '如需更新，请运行下方的 git pull 命令。' >&2
  exit 1
fi

git clone "$REPO" "$TARGET"
echo "完成：$TARGET"
```

如果 Steam 不在默认位置，可先在 Finder 中右键 Stardew Valley → **显示包内容**，再进入 `Contents/MacOS/Mods`；把上面 `MODS_DIR` 改为该路径后执行 `git clone`。

## 后续更新

每次更新前退出游戏，然后执行：

```bash
cd "$(find /Volumes "$HOME/Library/Application Support/Steam" -type d -path '*/Stardew Valley/Contents/MacOS/Mods/stardew-valley-mods' -print -quit 2>/dev/null)"
git pull --ff-only
```

如果朋友在这个目录中改了文件，`git pull --ff-only` 会停止而不会覆盖其改动；先备份或提交那些改动后再更新。

## 注意

- 朋友已有同名 mod 时可能冲突。请只保留其中一个版本。
- 请在游戏关闭时执行 clone 或 pull。
- 这个仓库当前只包含应共享的 mod 文件；本机日志、备份和 Finder 元数据已被忽略。
