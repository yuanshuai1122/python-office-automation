# Python pathlib 库教程：从入门到精通

`pathlib` 是 Python 标准库的一部分，用于以面向对象的方式处理文件和目录路径。它比传统的 `os.path` 更简洁、更直观，适合现代 Python 开发。本教程将带你逐步掌握 `pathlib` 的常用功能，通过实际代码示例让你快速上手。

## 1. 什么是 `pathlib`？

`pathlib` 提供了一个 `Path` 类，用于表示文件系统中的路径。它支持跨平台操作（Windows、Linux、macOS），自动处理不同操作系统的路径分隔符（如 `/` 和 `\`）。通过 `pathlib`，你可以轻松地创建、操作和查询文件路径。

### 导入 `pathlib`

```python
from pathlib import Path
```

## 2. 创建 Path 对象

`Path` 是 `pathlib` 的核心类，可以通过多种方式创建路径对象。

### 示例：创建路径

```python
# 当前工作目录
p = Path()

# 指定路径
p = Path("documents/data.txt")

# 相对路径
p = Path("folder/subfolder")

# 绝对路径
p = Path("/home/user/documents")

# 当前用户主目录
p = Path.home()

# 当前工作目录
p = Path.cwd()

print(p)  # 输出路径，例如：/home/user/documents 或 documents\data.txt（Windows）
```

**说明**：

- `Path()` 默认表示当前工作目录。
- `Path.home()` 返回用户主目录（如 `~/.` 或 `C:\Users\Username`）。
- `Path.cwd()` 返回当前工作目录。

## 3. 路径拼接

使用 `/` 操作符可以轻松拼接路径，`pathlib` 会自动处理分隔符。

### 示例：拼接路径

```python
# 拼接路径
base = Path("documents")
file_path = base / "data" / "file.txt"

print(file_path)  # 输出：documents/data/file.txt
```

**说明**：

- `/` 操作符可以连接 `Path` 对象或字符串，跨平台兼容。
- 结果是一个新的 `Path` 对象。

## 4. 常用路径属性

`Path` 对象提供了许多属性，用于获取路径的各个部分。

### 示例：获取路径信息

```python
p = Path("documents/data/file.txt")

print(p.name)       # 输出：file.txt（文件名）
print(p.suffix)     # 输出：.txt（扩展名）
print(p.stem)       # 输出：file（文件名不含扩展名）
print(p.parent)     # 输出：documents/data（父目录）
print(p.parts)      # 输出：('documents', 'data', 'file.txt')（路径分段）
print(p.is_absolute())  # 输出：False（是否绝对路径）
```

**说明**：

- `.name`：文件名（包括扩展名）。
- `.suffix`：文件扩展名。
- `.stem部分`.stem`：文件名（不含扩展名）。
- `.parent`：父目录。
- `.parts`：路径的各个部分。
- `.is_absolute()`：检查是否为绝对路径。

## 5. 检查路径状态

`pathlib` 提供方法来检查路径是否存在、是文件还是目录等。

### 示例：检查路径

```python
p = Path("documents/data/file.txt")

print(p.exists())       # 输出：True 或 False（路径是否存在）
print(p.is_file())      # 输出：True 或 False（是否为文件）
print(p.is_dir())       # 输出：True 或 False（是否为目录）
print(p.resolve())      # 输出绝对路径，解析符号链接
```

**说明**：

- `.exists()`：检查路径是否存在。
- `.is_file()`：检查是否为文件。
- `.is_dir()`：检查是否为目录。
- `.resolve()`：返回绝对路径，并解析符号链接。

## 6. 创建和操作文件/目录

`pathlib` 提供了便捷的方法来创建、删除和操作文件或目录。

### 示例：创建和删除

```python
# 创建目录
p = Path("new_folder")
p.mkdir(exist_ok=True)  # 如果目录已存在，不会报错

# 创建文件
file = Path("new_folder/test.txt")
file.touch()  # 创建空文件

# 写入文件
file.write_text("Hello, pathlib!")

# 读取文件
content = file.read_text()
print(content)  # 输出：Hello, pathlib!

# 删除文件
file.unlink(missing_ok=True)  # 如果文件不存在，不会报错

# 删除目录（必须为空）
p.rmdir()
```

**说明**：

- `.mkdir(exist_ok=True)`：创建目录，`exist_ok=True` 防止重复创建报错。
- `.touch()`：创建空文件或更新文件时间戳。
- `.write_text()`：写入文本到文件。
- `.read_text()`：读取文件内容。
- `.unlink(missing_ok=True)`：删除文件，`missing_ok=True` 防止文件不存在报错。
- `.rmdir()`：删除空目录。

## 7. 遍历目录

`pathlib` 提供了强大的目录遍历功能，支持递归查找文件。

### 示例：遍历目录

```python
# 列出目录中的所有文件和子目录
p = Path("documents")
for item in p.iterdir():
    print(item)  # 输出每个文件或目录的路径

# 递归查找所有 .txt 文件
for txt_file in p.glob("**/*.txt"):
    print(txt_file)  # 输出所有 .txt 文件的路径
```

**说明**：

- `.iterdir()`：列出目录中的直接内容。
- `.glob("**/*.txt")`：递归查找匹配模式的文件（`**` 表示任意深度的子目录）。

## 8. 路径转换和操作

`pathlib` 提供了一些方法来转换路径格式或执行其他操作。

### 示例：路径转换

```python
p = Path("documents/data/file.txt")

# 转换为绝对路径
print(p.absolute())  # 输出绝对路径

# 转换为字符串
print(str(p))  # 输出：documents/data/file.txt

# 转换为 Windows 或 POSIX 风格路径
print(p.as_posix())  # 输出：documents/data/file.txt（使用 /）
print(p.as_uri())    # 输出：file:///...（URI 格式）
```

**说明**：

- `.absolute()`：返回绝对路径（不解析符号链接）。
- `.as_posix()`：返回 POSIX 风格路径（使用 `/`）。
- `.as_uri()`：返回文件路径的 URI 表示。

## 9. 文件元信息

`Path` 对象可以获取文件的元信息，如大小、修改时间等。

### 示例：获取文件信息

```python
p = Path("documents/data/file.txt")

stats = p.stat()
print(stats.st_size)      # 输出文件大小（字节）
print(stats.st_mtime)     # 输出最后修改时间（时间戳）
print(stats.st_mode)      # 输出文件权限模式
```

**说明**：

- `.stat()`：返回文件的状态信息，类似 `os.stat()`。
- `st_size`：文件大小（字节）。
- `st_mtime`：最后修改时间（Unix 时间戳）。
- `st_mode`：文件权限模式。

## 10. 处理符号链接

`pathlib` 支持创建和检查符号链接。

### 示例：符号链接

```python
p = Path("documents/link.txt")
target = Path("documents/data/file.txt")

# 创建符号链接
p.symlink_to(target)

# 检查是否为符号链接
print(p.is_symlink())  # 输出：True

# 解析符号链接
print(p.resolve())  # 输出目标文件的绝对路径
```

**说明**：

- `.symlink_to(target)`：创建指向目标的符号链接。
- `.is_symlink()`：检查是否为符号链接。
- `.resolve()`：解析符号链接到实际路径。

## 11. 高级用法：批量重命名

结合 `glob` 和路径操作，可以轻松实现批量文件操作。

### 示例：批量重命名

```python
# 将所有 .txt 文件重命名为 .bak
for file in Path("documents").glob("**/*.txt"):
    new_name = file.with_suffix(".bak")
    file.rename(new_name)
    print(f"Renamed {file} to {new_name}")
```

**说明**：

- `.with_suffix()`：更改文件扩展名。
- `.rename()`：重命名文件或移动文件到新路径。

## 12. 跨平台兼容性

`pathlib` 自动处理 Windows 和 Unix 系统之间的路径差异（如分隔符 `/` 和 `\`）。无论在哪个平台，代码都保持一致。

### 示例：跨平台路径

```python
p = Path("documents") / "data" / "file.txt"
print(p)  # Windows: documents\data\file.txt
          # Unix: documents/data/file.txt
```

## 13. 错误处理

操作文件系统时，可能会遇到权限错误或文件不存在等情况，建议使用 `try-except` 处理。

### 示例：错误处理

```python
p = Path("non_existent/file.txt")
try:
    content = p.read_text()
except FileNotFoundError:
    print("文件不存在！")
except PermissionError:
    print("权限不足！")
```

**说明**：

- 常见的异常包括 `FileNotFoundError` 和 `PermissionError`。
- 使用 `missing_ok=True` 或 `exist_ok=True` 可以减少显式异常处理。

## 14. 综合示例：文件管理脚本

以下是一个综合示例，展示如何使用 `pathlib` 管理文件。

### 示例：整理目录中的图片

```python
from pathlib import Path
import shutil

# 整理图片到以年份命名的文件夹
source_dir = Path("photos")
dest_dir = Path("organized_photos")

for img in source_dir.glob("**/*.[jp][pn][gf]"):  # 匹配 jpg、png、gif
    year = img.stat().st_mtime.strftime("%Y")  # 获取年份
    new_dir = dest_dir / year
    new_dir.mkdir(parents=True, exist_ok=True)  # 创建年份目录
    new_path = new_dir / img.name
    shutil.move(str(img), str(new_path))  # 移动文件
    print(f"Moved {img} to {new_path}")
```

**说明**：

- 使用 `glob` 查找图片文件。
- 根据修改时间提取年份，创建对应目录。
- 使用 `shutil.move` 移动文件（`pathlib` 不直接提供移动功能）。

## 总结

`pathlib` 提供了一种现代、优雅的方式来处理文件系统路径。它的主要优势包括：

- 跨平台兼容性。
- 面向对象的接口，代码简洁。
- 强大的路径操作和文件管理功能。

通过本教程，你应该已经掌握了 `pathlib` 的核心用法。建议多练习，尝试在实际项目中使用 `pathlib`，逐步替换传统的 `os.path` 操作。