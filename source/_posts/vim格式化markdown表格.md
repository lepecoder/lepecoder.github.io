---
title: vim格式化markdown表格
date: 2017-11-23T16:59:00
tags: ['vim']
categories: 开发工具
---


## 安装插件


[ https://github.com/dhruvasagar/vim-table-mode ](https://github.com/dhruvasagar/vim-table-mode)



可以查看github页使用git下载,如果使用Vundle插件管理工具的话,也可以直接添加一行`Plugin 'table-mode'`



## 配置



在`.vimrc`里添加如下配置

```

let g:table_mode_corner = '|'

let g:table_mode_border=0

let g:table_mode_fillchar=' '



function! s:isAtStartOfLine(mapping)

  let text_before_cursor = getline('.')[0 : col('.')-1]

  let mapping_pattern = '\V' . escape(a:mapping, '\')

  let comment_pattern = '\V' . escape(substitute(&l:commentstring, '%s.*$', '', ''), '\')

  return (text_before_cursor =~? '^' . ('\v(' . comment_pattern . '\v)?') . '\s*\v' . mapping_pattern . '\v$')

endfunction



inoreabbrev <expr> <bar><bar>

          \ <SID>isAtStartOfLine('\|\|') ?

          \ '<c-o>:TableModeEnable<cr><bar><space><bar><left><left>' : '<bar><bar>'

inoreabbrev <expr> __

          \ <SID>isAtStartOfLine('__') ?

          \ '<c-o>:silent! TableModeDisable<cr>' : '__'

```

在任意空行插入`||`,然后退出插入模式即可启用表格格式化插件.





## 效果

![o_1bvk1182k1g5ks4d1k6o1ffs18ofa.gif](http://ot0ucj3at.bkt.clouddn.com/o_1bvk1182k1g5ks4d1k6o1ffs18ofa.gif)
    