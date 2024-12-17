---
title: linux下直接复制文件内容到剪切板
date: 2017-11-23T17:12:00
tags: ['Linux']
categories:
---


首先安装`xsel`.



```

xsel --input --clipboard	#copy to clipboard

xsel --output --clipboard	# get from clipboard

```



如果是简单的用`xsel --clipboard`会自动判别是要输出还是输入.



```

# 将剪切板中的内容输出到文件

echo $(xsel --clipboard) >> a.txt



# 将文件的内容复制到剪切板

cat a.txt | xsel --clipboard



```
    