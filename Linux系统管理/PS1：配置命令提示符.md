# Bash Shell 提示符转义序列
<ul><li><code>\a</code>: 发出一个响铃声（警报）。</li><li><code>\d</code>: 显示日期，格式为“Tue May 17”。</li><li><code>\D{format}</code>: 使用 <code>strftime</code> 函数显示格式化日期和时间。例如：<code>\D{%F %T}</code> 会显示为“2024-05-17 13:37:42”。</li><li><code>\e</code>: 转义字符（ASCII 码 27）。</li><li><code>\h</code>: 主机名（仅显示第一个点之前的部分）。</li><li><code>\H</code>: 完整的主机名。</li><li><code>\j</code>: 当前 Shell 会话中正在运行的后台任务数。</li><li><code>\l</code>: 当前终端设备的名称（例如，<code>tty1</code>）。</li><li><code>\n</code>: 换行符。</li><li><code>\r</code>: 回车符。</li><li><code>\s</code>: Shell 的名称（即，执行该实例的基本名称，如 <code>bash</code>）。</li><li><code>\t</code>: 当前时间，格式为24小时制 HH:MM:SS。</li><li><code>\T</code>: 当前时间，格式为12小时制 HH:MM:SS。</li><li><code>\@</code>: 当前时间，格式为12小时制带AM/PM。</li><li><code>\A</code>: 当前时间，格式为24小时制 HH:MM。</li><li><code>\u</code>: 当前用户名。</li><li><code>\v</code>: Bash 的版本号（例如，4.4）。</li><li><code>\V</code>: Bash 的版本信息（例如，4.4.12(1)-release）。</li><li><code>\w</code>: 当前工作目录。</li><li><code>\W</code>: 当前工作目录的最后一个目录名。</li><li><code>\!</code>: 当前命令在历史中的编号。</li><li><code>\#</code>: 当前命令的编号（这个数字会递增）。</li><li><code>\$</code>: 如果是普通用户则显示 <code>$</code>，如果是超级用户（root）则显示 <code>#</code>。</li><li><code>\nnn</code>: 使用八进制值 <code>nnn</code> 表示的字符（例如，<code>\007</code> 表示一个响铃声）。</li><li><code>\\</code>: 反斜杠自身。</li><li><code>\[</code>: 表示非打印字符的开始（用于嵌入终端控制序列）。</li><li><code>\]</code>: 表示非打印字符的结束。</li></ul>

# 示例
- 基本
```bash
PS1='\u@\h:\w\$ '
```
- 包含时间
```bash
PS1='[\t] \u@\h:\w\$ '
```
- 包含日期时间
```bash
PS1='\d \t \u@\h:\w\$ '
```
- 自用
```bash
export PS1='\e[1;32m\u\e[m\e[1;33m@\e[m\e[1;35m\H\e[m\e[4m:`pwd`\e[m\n\$ '
```