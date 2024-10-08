---
title: 解析edge收藏栏
tags:
  - Python
  - 自制
swiper: false
top: false
toc: true
tocOpen: true
onlyTitle: false
comments: true
share: true
copyright: true
donate: false
mathjax: true
imgTop: true
img: https://cdn.pixabay.com/photo/2022/06/11/09/20/snake-7256057_1280.jpg
excerpt: 使用python解析edge导出的收藏栏，提取链接和icon并写入到md文件中
categories: 计算机
date: 2024-10-08 03:20:09
---
# 解析edge收藏栏

```python
import re
from bs4 import BeautifulSoup

def find_icon(links):
    icons = []
    for link in links:
        linka= link.find_all('a')
        if len(linka) > 0:
            for link_a in linka:
                href = "https://www.baidu.com" if link_a.get('href') is None else link_a.get('href')
                top_level_domain = re.search(r'(https?://[^/]+\.[^/]+)', href).group(0)
                text = "无" if link_a.get_text() == "" or link_a.get_text() is None else link_a.get_text()
                icon = top_level_domain + "/favicon.ico"
                icons.append((text, href, icon))
    return icons

def write_to_file(icons, strings):
    lines_to_append = []
    lines_to_append.append(f"## {strings}")
    lines_to_append.append('{% btns circle grid4 %}')
    for site in icons:
        lines_to_append.append(f"    {{% cell {site[0]}, {site[1]}, {site[2]} %}}")
    lines_to_append.append('{% endbtns %}')
    return lines_to_append

def parse_bookmark_file(file_path):
    with open(file_path, 'r', encoding='utf-8') as file:
        content = file.read()
    soup = BeautifulSoup(content, 'lxml')
    link1 = soup.find('dl')
    link2 = link1.find_all('dl', recursive=False)
    link3 = link1.find_all('dt', recursive=False)
    link21 = link2[0].find_all('dl', recursive=False)
    资源 = find_icon(link21[0])
    影音 = find_icon(link21[1])
    游戏 = find_icon(link21[2])
    代码 = find_icon(link21[3])
    待看 = find_icon(link2[1])
    漫画 = find_icon(link2[2])
    其他 = find_icon(link3)
    lines_to_append = []
    lines_to_append += write_to_file(资源, "资源")
    lines_to_append += " "
    lines_to_append += write_to_file(影音, "影音")
    lines_to_append += " "
    lines_to_append += write_to_file(游戏, "游戏")
    lines_to_append += " "
    lines_to_append += write_to_file(代码, "代码")
    lines_to_append += " "
    lines_to_append += write_to_file(待看, "待看")
    lines_to_append += " "
    lines_to_append += write_to_file(漫画, "漫画")
    lines_to_append += " "
    lines_to_append += write_to_file(其他, "其他")
    lines_to_append += " "
    return lines_to_append

def main():
    file_path = "***.html"  # 替换为你收藏夹路径
    lines_to_append = parse_bookmark_file(file_path)
    file_path = "***.md" # 替换为要写入的文件路径
    with open(file_path, "a", encoding="utf-8") as file:
        for line in lines_to_append:
            file.write(line + "\n")

if __name__ == "__main__":
    main()
    print("完成")
```
