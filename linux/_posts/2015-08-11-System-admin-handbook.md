## killall -- kill processes by name
SYNOPSIS: `killall [-SIGNAL] [procname ...]`
默认发送 TERM 信号到当前用户(with a real UID identical to the caller of killall)的所有符合 procname 的进程。
super-user 允许 kill 任意进程。
-SIGNAL 参数，SIG 前缀是可选的，如 `killall -KILL nginx` 等于 `killall -SIGKILL nginx`
`killall -l` 可列出所有支持的信号。

