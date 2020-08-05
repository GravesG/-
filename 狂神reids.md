> incr   decr

设置步长

INCRBY   [KEY]    10      // 每次增加10

DECRBY   [KEY]    10      // 每次减少10



> 获取部分字符串

getrange   key  0 3     截取字符串【0，3】



> 替换指定位置字符串

setrange key  1 xx    把第二个字符替换成xx



setex（set with expire）  设置过期时间

setnx （set if not exist） 不存在设置（在分布式锁中常常使用

）