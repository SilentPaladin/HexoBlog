---
title: "About Me"
date: 2024-10-06 13:38:47
onlyTitle: true
---
# 个人介绍

博主: SilentPaladin | 性别: 男 | 邮箱: CumerZhang@Gmail.com

## 我的书籍

{% sitegroup %}
    {% site Example, url=http://www.baidu.com, screenshot=https://pic4.zhimg.com/v2-7fcb0d73e1d90788ccf136e22ba7b1bd_r.jpg, avatar=https://pic4.zhimg.com/80/v2-45eb5749949e7f90a5c788f9bc5721ef_1440w.jpg, description=这是描述 %}
{% endsitegroup %}

## 我的社交账号

<div style="display: flex;">
    {% btn center cool-2, Example, https://github.com, fa-solid fa-user %}
</div>  

## 我的技能

<div style="display: grid;
  grid-template-columns: 50% 50%;
  grid-template-rows: 30px 30px;
  grid-column-gap: 20px">
{% progress 70 danger Python %}
{% progress 70 info C++ %}
{% progress 70 success Java %}
{% progress 70 warning C %}
{% progress 70 primary C# %}
{% progress 70 primary JavaScript %}
{% progress 30 primary SQL %}
{% progress 70 primary MATLAB %}
{% progress 70 primary Rust %}
{% progress 50 primary .NET %}
{% progress 50 primary Qt %}
{% progress 10 primary Django %}
{% progress 50 primary NumPy %}
{% progress 50 primary Pandas %}
{% progress 50 primary PyTorch %}
</div>

## 其他

{% linkgroup %}
    {% link Example, http://www.baidu.com, https://pica.zhimg.com/80/v2-970dd5538f106dd6be064c4eafc01c36_1440w.webp %}
{% endlinkgroup %}

## 我的游戏

{% gallery %}
    ![王者荣耀](https://pic2.zhimg.com/v2-abb2c12e9fbe8dda1993f7cd5d149159_b.jpg)
{% endgallery %}

## 我的相册

{% swiper %}
    ![Example](https://pic3.zhimg.com/80/v2-7cfc909ebe8d83683909846edd6b5232_1440w.webp "http://www.baidu.com")
{% endswiper %}
