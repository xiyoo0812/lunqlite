# lunqlite
unqlite的lua封装库！

# 依赖
- [lua](https://github.com/xiyoo0812/lua.git)5.2以上
- [luakit](https://github.com/xiyoo0812/luakit.git)一个luabind库
- 项目路径如下<br>
  |--proj <br>
  &emsp;|--lua <br>
  &emsp;|--lunqlite <br>
  &emsp;|--luakit


# 编译
- msvc: 准备好lua依赖库并放到指定位置，将proj文件加到sln后编译。
- linux: 准备好lua依赖库并放到指定位置，执行make -f lmdb.mak

# 注意事项
- mimalloc: 参考[quanta](https://github.com/xiyoo0812/quanta.git)使用，不用则在工程文件中注释

# 用法
```lua
-- unqlite_test.lua
local log_debug     = logger.debug
local jsoncodec     = json.jsoncodec

local driver = unqlite.create()
local jcodec = jsoncodec()

driver.set_codec(jcodec)
driver.open("./unqlite/xxx.db")

local a = driver.put("abc1", {a=123})
local b = driver.put("abc2", "234")
local c = driver.put("abc3", "235")
local d = driver.put("abc4", "236")
log_debug("put: {}-{}-{}-{}", a, b, c, d)

for i = 1, 6 do
    local da,rc = driver.get("abc" .. i)
    log_debug("get-{}: {}-{}", i, da,rc)
end

local r, k, v = driver.cursor_first()
while v do
    log_debug("cursor1: {}={}->{}", k, v, r)
    r, k, v = driver.cursor_next()
end

r, k, v = driver.cursor_seek("abc2")
while v do
    log_debug("cursor2: {}={}->{}", k, v, r)
    r, k, v = driver.cursor_next()
end

r, k, v = driver.cursor_last()
while v do
    log_debug("cursor3: {}={}->{}", k, v, r)
    r, k, v = driver.cursor_prev()
end

a = driver.put("abc4", {a=234})
b = driver.put("abc5", "235")
log_debug("put: {}-{}", a, b)
for i = 1, 6 do
    local da,rc = driver.get("abc" .. i)
    log_debug("get-{}: {}-{}", i, da,rc)
end

b = driver.del("abc4")
a = driver.del("abc5")
log_debug("del: {}-{}", a, b)
for i = 1, 6 do
    local da,rc = driver.get("abc" .. i)
    log_debug("del_get-{}: {}-{}", i, da,rc)
end

```
