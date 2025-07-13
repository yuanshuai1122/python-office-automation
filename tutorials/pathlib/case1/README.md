背景需求
公司每个月整理文档，要求为每个部门预先生成一个月的每日子目录，用于后续每日归档文档。
目录结构如下：
```yaml
./归档文档/2025年07月/
├── 行政部/
│   ├── 2025-07-01/
│   ├── 2025-07-02/
│   ├── ...
├── 财务部/
│   ├── 2025-07-01/
│   ├── ...

```
每个子目录里放一个空的 README.txt，用于提醒文员归档文件放这里。

最终效果
```yaml
./归档文档/2025年07月/行政部/2025-07-01/README.txt
./归档文档/2025年07月/行政部/2025-07-02/README.txt
...
./归档文档/2025年07月/财务部/2025-07-01/README.txt
...

```
实现代码（基础版）
```python
from pathlib import Path
from datetime import datetime, timedelta

# 配置年月
year = 2025
month = 7
month_str = f"{year}年{month:02d}月"
root_dir = Path(f"./归档文档/{month_str}")
departments = ['行政部', '财务部', '研发部', '市场部']

# 生成该月的日期列表
start_date = datetime(year, month, 1)
days_in_month = 31 if month == 7 else 30  # 简化一下逻辑
dates = [(start_date + timedelta(days=i)).strftime("%Y-%m-%d") for i in range(days_in_month)]

# 执行创建
for dept in departments:
    for date_str in dates:
        target_dir = root_dir / dept / date_str
        readme_file = target_dir / "README.txt"
        target_dir.mkdir(parents=True, exist_ok=True)
        if not readme_file.exists():
            readme_file.write_text(f"请在此目录归档 {dept} 于 {date_str} 的文档。")
            print(f"✅ 创建：{readme_file}")

```

面向对象写法
```python
from pathlib import Path
from datetime import datetime, timedelta


class ArchiveDirectoryInitializer:
    def __init__(self, base_dir: Path, year: int, month: int, departments: list[str]):
        self.base_dir = base_dir
        self.year = year
        self.month = month
        self.departments = departments
        self.month_str = f"{year}年{month:02d}月"
        self.root_dir = self.base_dir / self.month_str
        self.dates = self._generate_dates()

    def _generate_dates(self) -> list[str]:
        """生成该月所有日期字符串列表"""
        start_date = datetime(self.year, self.month, 1)
        next_month = self.month + 1 if self.month < 12 else 1
        next_month_year = self.year + 1 if self.month == 12 else self.year
        end_date = datetime(next_month_year, next_month, 1)
        days = (end_date - start_date).days
        return [(start_date + timedelta(days=i)).strftime("%Y-%m-%d") for i in range(days)]

    def _create_readme(self, target_dir: Path, date_str: str, dept: str):
        """在目标目录下生成 README.txt"""
        readme_file = target_dir / "README.txt"
        if not readme_file.exists():
            readme_file.write_text(f"请在此目录归档 {dept} 于 {date_str} 的文档。")
            print(f"✅ 创建：{readme_file}")
        else:
            print(f"⚠️ 已存在：{readme_file}")

    def create_structure(self):
        """生成目录与 README"""
        for dept in self.departments:
            for date_str in self.dates:
                target_dir = self.root_dir / dept / date_str
                target_dir.mkdir(parents=True, exist_ok=True)
                self._create_readme(target_dir, date_str, dept)


# 使用示例
if __name__ == "__main__":
    initializer = ArchiveDirectoryInitializer(
        base_dir=Path("./归档文档"),
        year=2025,
        month=7,
        departments=["行政部", "财务部", "研发部", "市场部"]
    )
    initializer.create_structure()

```