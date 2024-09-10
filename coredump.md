在 Linux 上，设置 C 程序生成 core dump 文件可以帮助开发者在程序崩溃时保存程序的内存映像，以便后续调试。以下是如何设置 core dump 的步骤：

### 1. 确认系统是否允许生成 core dump 文件
首先，使用 `ulimit` 命令来检查或设置 core dump 文件的大小限制。

```bash
ulimit -c
```
如果输出为 `0`，则表示系统不允许生成 core dump 文件。

### 2. 允许生成 core dump 文件
通过 `ulimit` 命令设置 core dump 文件的大小为不限，或者指定一个合适的值。

```bash
ulimit -c unlimited
```
这将允许生成大小不限的 core dump 文件。如果只想生成特定大小的 core dump 文件，可以替换 `unlimited` 为具体的数字（单位为字节）。

### 3. 设置 core dump 文件保存路径
默认情况下，core dump 文件通常生成在程序崩溃的工作目录中。可以通过修改 `/proc/sys/kernel/core_pattern` 文件来设置 core dump 文件的保存路径和命名格式。

```bash
echo "/tmp/core_%e_%p_%t" | sudo tee /proc/sys/kernel/core_pattern
```
解释：
- `%e`: 崩溃程序的文件名
- `%p`: 程序的 PID
- `%t`: 崩溃时间的时间戳
- `/tmp`: 指定 core dump 文件保存的目录

### 4. 配置持久化 core dump 设置
要使 core dump 设置在系统重启后依然生效，可以将以下行添加到 `/etc/security/limits.conf` 文件中：
```bash
* soft core unlimited
* hard core unlimited
```

### 5. 调试 core dump 文件
程序生成 core dump 文件后，可以使用调试工具如 `gdb` 来分析这个文件，找出程序崩溃的原因。

使用 `gdb` 调试：
```bash
gdb /path/to/program /path/to/core
```
其中：
- `/path/to/program`: 崩溃程序的可执行文件路径
- `/path/to/core`: 生成的 core dump 文件路径

### 示例：
1. 编写一个简单的 C 程序，故意导致段错误 (Segmentation fault)：
   ```c
   #include <stdio.h>
   
   int main() {
       int *p = NULL;
       *p = 10;  // 产生段错误
       return 0;
   }
   ```

2. 编译并运行程序：
   ```bash
   gcc -o segfault segfault.c
   ./segfault
   ```

3. 如果已配置 core dump，将会在指定路径生成 core dump 文件。使用 `gdb` 调试：
   ```bash
   gdb ./segfault core
   ```

4. 在 `gdb` 中，可以使用 `bt` 命令查看程序崩溃时的堆栈信息：
   ```gdb
   (gdb) bt
   ```

这将帮助定位程序崩溃的具体位置。
