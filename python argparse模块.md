### python argparse 模块
> 编写用户友好的命令行界面

```
import argparse
# 1、创建解析器, prefix_chars:设置前缀字符、allow_abbrev:设置是否识别长缩写、conflict_handler:处理参数覆盖的情况、add_help:设置帮助功能
parse = argparse.ArgumentParser(description='Process some integers.')  

# 2、添加参数
parser.add_argument('integers', metavar='N', type=int, nargs='+', help='an integer for the accumulator')
parser.add_argument('--sum', dest='accumulate', action='store_const', const=sum, default=max, help='sum the integers (default: find the max)')

# 3、解析参数
args = parser.parse_args()
```

即可在命令行中使用 --sum 传递可选参数，以及位置参数

----
#### **参数解析
parse.add_argument()
`add_argument(name or flags...[, action][, nargs][, const][, default][, type][, choices][, required][, help][, metavar][, dest])`
- name or flag:位置参数或可选参数的名称或列表，如 -f, --func 表示可选, func 表示位置参数(位置参数不支持列表)
- action:
  - store:默认，存储参数的值
  - store_const, const=True:存储关键字参数const指定的值(只能给可选参数设置，位置参数不可设置；设置后，命令行中可选参数不接受传值)
  - store_true, store_false:用于存储可选参数的默认值(当命令行中使用该可选参数时，为store的默认值，否则为store默认值取反)
  - append:存储列表，将参数值放到列表中(对可选参数有效)
  - append_const:存储一个列表，并将const关键字参数指定的值附加到列表中(可选参数不接受传值)。
  - count:关键字出现的次数
  - version:打印版本信息
- nargs:单个参数对应传值的个数
  - N:N个命令行参数收集到一个列表中
  - ?:
    - 可选参数:从命令行选取一个参数，如果该命令未传参，则使用const的值，如果未使用该命令，则使用default的值
    - 位置参数:如果不存在命令行参数，则使用默认值
  - *:存在所有命令行参数都收集到列表中，对多个位置参数通常没有多大意义
  - +:等同于*,但要求至少传一个参数
  - argparse.REMAINDER:剩余的命令行参数都收集到一个列表中
- const:给action为store_const,append_const以及nargs=?时使用
- default:默认值
- type:类型
- choices:从一组受限制的值中选择命令行参数值，支持type做类型转换匹配
- required:设置可选参数是否必传
- help:帮助信息
- metavar:
- dest:可选参数对应的属性名称
