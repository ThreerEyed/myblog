---
title: 我喜欢的声音
date: 2020-05-22 11:59:16
top_img: https://cdn.jsdelivr.net/gh/ThreerEyed/CDN/background/queen.jpg
---

{% aplayerlist %}
{
    "narrow": false,                          // Optional, narrow style
    "autoplay": true,                         // Optional, autoplay song(s), not supported by mobile browsers
    "mode": "random",                         // Optional, play mode, can be `random` `single` `circulation`(loop) `order`(no loop), default: `circulation`
    "showlrc": 3,                             // Optional, show lrc, can be 1, 2, 3
    "mutex": true,                            // Optional, pause other players when this player playing
    "theme": "#e6d0b2",	                      // Optional, theme color, default: #b7daff
    "preload": "metadata",                    // Optional, the way to load music, can be 'none' 'metadata' 'auto', default: 'auto'
    "listmaxheight": "513px",                 // Optional, max height of play list
    "music": [
        {
            "title": "CoCo",
            "author": "Jeff Williams",
            "url": "caffeine.mp3",
            "pic": "caffeine.jpeg",
            "lrc": "caffeine.txt"
        },
        {
            "title": "アイロニ",
            "author": "鹿乃",
            "url": "irony.mp3",
            "pic": "irony.jpg"
        }
    ]
}
{% endaplayerlist %}

{% meting "2243708362" "netease" "playlist" "autoplay" "mutex:true" "listmaxheight:340px" "preload:none" "theme:#ad7a86"%}

{% meting "5030248493" "netease" "playlist" "autoplay" "mutex:true" "listmaxheight:400px" "preload:none" "theme:#ad7a86"%}

