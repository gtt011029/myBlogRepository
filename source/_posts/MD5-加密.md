---
title: MD5 加密
abbrlink: 35634
date: 2019-11-02 14:50:20
tags:
---







最近做东西，遇到加密问题，之前没有用过，就简单的查了点资料

为了数据安全经常会去使用到加密技术，这边的加密一般是针对用户名和密码的加密，本地保或传输到服务器时，密码都有可能被获取到，直接操作密码肯定是不安全的，一般是进行MD5加密再保存或传输（这边注意一下，MD5加密是不可逆的，加密后无法解密，想要解密只能用穷举法）

<!--more-->

MD5其他的用法：

防止被篡改，可以对一些数据先计算MD5，创数后再计算MD5进行对比，如果数据被篡改了，MD5肯定就变了

```
/**
     * 获取MD5字符串
     */
    public static String getMD5(String content) {
        try {
            MessageDigest digest = MessageDigest.getInstance("MD5");
            digest.update(content.getBytes());
            return getHashString(digest);
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
        }
        return null;
    }
    
/**
     * 将字节数组转换成十六进制字符串
     *
     * @param digest
     * @return
     */
    private static String getHashString(MessageDigest digest) {
        StringBuilder builder = new StringBuilder();
        for (byte b : digest.digest()) {
            builder.append(Integer.toHexString((b >> 4) & 0xf));
            builder.append(Integer.toHexString(b & 0xf));
        }
        return builder.toString();
    }
```





MD5加盐：

获取到MD5加密数据后，虽然无法解密获取密码，但是因为用户的密码一般都是比较短的，如果把可能的密码先MD5处理，保存到数据库，然后再一个一个跟你的MD5结果匹配（穷举法），如果相同那么就获取到了密码；

所以有种比较好的方法，再用户密码的基础上再加上一些复杂的字符串再计算MD5，那反推出原始密码就变得非常困难了。加上的这段长字符，我们称为盐(Salt)，通过这种方式加密结果，我们称之为加盐。

这边呢，加盐有两种方法，一是加固定的字符串，二是也可以生成随机数作为盐加进去，这样的安全性能高点

```
/**
     * 设置一个SALT增加复杂度
     */
    private final static String SALT = "qrqhhufadfhsdqwer";
/**
     * 获取加盐的MD5字符串
     */
    public static String getMD5WithSalt(String content) {
        return getMD5(getMD5(content) + SALT);
    }
```



MD5到底算不算加密：

有的人说其不算加密：

1、MD5本质上说是一种hash算法（又叫散列算法），是一类把任意数据转换为定长数据的算法的统称

2、加密呢，指的是对数据进行转换后，数据变成了另一种格式，并且除了拿到解密方法的人，没人能把数据转换回来，由此可能加密后是可以解密的。而MD5是不可逆的是无法解密的，所以有人认为MD5不算加密

当然，这边MD5是不是加密，还是看个人的看法与理解