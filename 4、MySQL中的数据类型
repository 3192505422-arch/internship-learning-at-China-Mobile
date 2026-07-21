一、整数类型
tinyint(m)：1个字节（-128-127）
smallint(m)：2个字节（-32768-32767）
mediumint(m)：3个字节（-8388608-8388607）
int(m)：4个字节（-2147483648-2147483647）
bigint(m)：8个字节（+—9.22*10的18次方）
*数值类型中的长度m是指显示长度，并不表示存储长度，只有字段指定zerofill时有用

二、浮点类型
float（m，d）：单精度浮点型 8位精度（4字节）m总个数，d小数位
double（m，d）：双精度浮点型 16位精度（8字节）m总个数，d小数位

三、字符类型
char（n）：固定长度，最多255个字符，适合用在身份证号码、手机号码等定长，速度最快
tinytext：可变长度，最多255个字符
varchar（n）：可变长度，最多65535个字符，适合用在长度可变的属性
text：可变长度，最多65535个字符，当不知道属性的最大长度时，适合用text，速度最慢
mediumtext：可变长度，最多2的24次方-1个字符
longtext：可变长度，最多2的32次方-1个字符
使用建议：经常变化的字段用varchar，知道固定长度的用char，尽量用varchar，超过255字符的只能用varchar或者text，能用varchar的地方不用text。

四、日期类型
date：日期YYYY-MM-DD
time：时间HH:MM:SS
datetime：日期时间YYYY-MM-DD HH:MM:SS
timestamp；时间戳YYYYMMDD HHMMSS

五、二进制数据（BLOB）
1、BLOB和TEXT存储方式不同，TEXT以文本方式存储，英文存储区分大小写，而Blob是以二进制方式存储，不分大小写。
2、BLOB存储的数据只能整体读出。
3、TEXT可以指定字符集，BLOB不用指定字符集。
