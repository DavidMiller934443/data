# linebreak

> 更改换行符格式 （ `\n` ）
>
> `\r` = `cr` ： 在 `x` 之前的 `mac os` 中用作换行符
>
> `\n` = `lf` ：在 `unix/linux/mac os x` 中用作换行符
>
> `\r\n` = `cr + lf` ：在 `windows` 中用作换行符

查看使用的是哪种换行符

```shell
┌──(root㉿kali)-[~]
└─# cat -e test.txt
```

```
test^M^MHello, World!
```

```
test$
$
Hello, World!
```

```
test^M$
^M$
Hello, World!
```

> 换行符为 `^M` 时为 `\r` 
>
> 换行符为 `$` 时为 `\n` 
>
> 换行符为 `^M$` 时为 `\r\n` 

使用脚本转换

```shell
┌──(root㉿kali)-[~]
└─# python3 linebreak.py -h
usage: linebreak.py [-r] [-n] [-rn] filename

将文件中的换行符转换为指定格式。

positional arguments:
  filename    要转换的文件名

options:
  -n          将换行符转换为 lf 格式（\n）
  -r          将换行符转换为 cr 格式（\r）
  -rn         将换行符转换为 cr + lf 格式（\r\n）
  -h, --help  显示帮助信息
```

```python
import argparse
import sys

parser = argparse.ArgumentParser(description='将文件中的换行符转换为指定格式。', add_help=False, usage='%(prog)s [-r] [-n] [-rn] filename')
parser.add_argument('filename', help='要转换的文件名')
parser.add_argument('-n', action='store_true', help='将换行符转换为 lf 格式（\\n）')
parser.add_argument('-r', action='store_true', help='将换行符转换为 cr 格式（\\r）')
parser.add_argument('-rn', action='store_true', help='将换行符转换为 cr + lf 格式（\\r\\n）')
parser.add_argument('-h', '--help', action='help', help='显示帮助信息')
args = parser.parse_args()

if not args.n and not args.r and not args.rn:
    print('错误：必须指定换行符格式以进行转换。')
    sys.exit(1)

if args.n:
    new_line_ending = '\n'
    new_line_ending_desc = 'lf 格式'
elif args.r:
    new_line_ending = '\r'
    new_line_ending_desc = 'cr 格式'
else:
    new_line_ending = '\r\n'
    new_line_ending_desc = 'cr + lf 格式'

try:
    with open(args.filename, 'r') as file:
        file_content = file.read()

    file_content = file_content.replace('\r\n', '\n').replace('\r', '\n').replace('\n', new_line_ending)

    with open(args.filename, 'w') as file:
        file.write(file_content)

    print(f"文件 '{args.filename}' 中的换行符已转换为 {new_line_ending_desc}。")
except FileNotFoundError:
    print(f"文件 '{args.filename}' 未找到。")
except Exception as e:
    print(f"发生错误：{e}")

```

