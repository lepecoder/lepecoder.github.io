---
title: Linux交换Esc和Caps
date: 2019-04-10T18:37:00
tags:
categories:
---

使用过 `.xmodmap`,重启后就失效，添加到`rc.local`也不管用，后来通过在`xorg`里配置成功。

更改xorg里的键盘配置，增加`Option "XkbOptions" "caps:swapescape"`
我用的archlinux在`/etc/X11/xorg.conf.d/00-keyboard.conf `，有些可能在 /etc/X11/xorg.conf
一个可能的键盘字段类似下面这样

```
Section "InputClass"
        Identifier "system-keyboard"
        MatchIsKeyboard "on"
	Option "XkbOptions" "caps:swapescape"
        Option "XkbLayout" "cn"
        Option "XkbModel" "pc105"
EndSection
```

类似的命令可以在`/usr/share/X11/xkb/rules/xorg.lst `里找到，比如

```
  caps:internal        Caps Lock uses internal capitalization; Shift "pauses" Caps Lock
  caps:internal_nocancel Caps Lock uses internal capitalization; Shift does not affect Caps Lock
  caps:shift           Caps Lock acts as Shift with locking; Shift "pauses" Caps Lock
  caps:shift_nocancel  Caps Lock acts as Shift with locking; Shift does not affect Caps Lock
  caps:capslock        Caps Lock toggles normal capitalization of alphabetic characters
  caps:shiftlock       Caps Lock toggles ShiftLock (affects all keys)
  caps:swapescape      Swap ESC and Caps Lock
  caps:escape          Make Caps Lock an additional Esc
  caps:backspace       Make Caps Lock an additional Backspace
  caps:super           Make Caps Lock an additional Super
  caps:hyper           Make Caps Lock an additional Hyper
  caps:menu            Make Caps Lock an additional Menu key
  caps:numlock         Make Caps Lock an additional Num Lock
```
    