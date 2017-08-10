# 测试

1. 测试xargs的基本功能
  命令：ls *.txt | xargs wc -l
  预期返回：
    10253 a.js
    10253 b.js
    10253 c.js
    10253 d.js
    10253 e.js
    51265 total

2. 测试功能参数-n
  命令：echo {0..8} | xargs -n 2 echo
  预期返回：
  0 1
  2 3
  4 5
  6 7
  8

3. 测试参数接收
  命令：echo {0..8} | xargs -n 2 -P 5 -d echo
  预期返回：
  OPTIONS:
  {:max_args=>"2", :max_procs=>"5", :dev=>true}

  RAW:
  0 1 2 3 4 5 6 7 8


  LENGTH
  raw: 18 | process_by_new_line: 1 | process_by_space: 9 | process_by_tab: 1

  PRE PRECESS RESOURCE:
  ["0", "1", "2", "3", "4", "5", "6", "7", "8"]

  COMMANDS:
  ["echo"]

  EXECUTABLE COMMAND:
  echo 0 1 2 3 4 5 6 7 8

  RESULT:
  0 1
  2 3
  4 5
  6 7
  8

4. 测试对不完整参数的处理
  命令：echo {0..8} | xargs -n echo
  预期返回：OptionParser::InvalidOption
  实际返回：程序错误

5. 测试对未定义参数的处理
  命令：echo {0..8} | xargs -x echo
  预期返回：OptionParser::InvalidArgument
  实际返回：
  WARNING: ["-x"] options invalid
  0 1 2 3 4 5 6 7 8

# 时间分配

1. 研究xargs文档：30min左右
2. 规划代码：45min左右
3. 编写、调试代码：3h30min左右
4. 测试：60min
5. 编写文档：40min

# 已知bug
1. 对于不完整参数的处理（测试4）
  目前无法很好地处理不完整参数。读取参数时，程序会将紧跟其后的一个字符元素判定为其参数，在参数合法的情况下没有问题，参数不完整时会出现误读，导致错误。

2. 对于未定义参数的处理（测试5）
  出现未定义参数时，应当停止当前程序，由于不能很好的抓取未定义参数，因此目前只是做打印警告处理。

# 可优化的地方
1. 对于STDIN内容的处理，应该还有更好的方式。
2. 对于参数ARGV还可以有更系统的处理方式，目前的实现只是为允许x命令自带参数而针对OptionParser所做的Hack。
