---
title: some forms of encryption
tags: [encryption]
categories: outline
---

<div align="center">
https://blog.csdn.net/u013565368/article/details/53081195
https://www.cnblogs.com/zyw-205520/p/5585014.html
https://blog.storagecraft.com/5-common-encryption-algorithms/
https://github.com/xuzongbao/encryption
</div>

<!--more-->

# md5

信息摘要算法, 不可逆, 抗修改

但是用户密码不可直接使用 md5 加密后存储, 因为还是可能被暴力破解, 如事先生成一个 pwd -> md5(pwd) 映射字典, 破解一个 md5 时, 只要将待破解值拿到字典中和 字典值进行比对, 匹配上后返回对应的 key 即可破解.

所以一般会将密码拼接上一个随机串(加盐), 然后 md5 后存储. BCryptPasswordEncoder 就是这样, 不过更优雅, 她将 随机盐拼接到 加密后的 md5 密码后, 无需特地存储 盐

# rsa