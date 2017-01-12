+++
date = "2017-01-04T14:32:50Z"
title = "proxychains wrong configuration causes curl/wget weird problem"
slug = "proxychians_curl_weird_problem"
+++

## Weird thing after install proxychains
Today I installed `proxychains` to accelerate my network. The `proxychains` use socks5 proxy (`shadowsocks`). After several minutes trying it was not working as expect, I shutdown `shadowsocks` client (`sslocal`) and back to work.

Then weird thing happened. I couldn’t curl to localhost port. Reason was connection refused. But indeed port was listening.

I could ping to other site but I couldn’t curl/wget from it. The error notice was:
> curl: (6) Could not resolve host: www.baidu.com

After I remove `proxychains` by `apt-get remove`, everything back to normal.

## Guess

`proxychains` will automatically inject into some command. Even you don’t use `proxychains` to run command. So if you turn down socks5 proxy, `proxychains` which continue using proxy will return nothing.

## Find out the cause: `LD_PRELOAD`
How can `proxychains` change behavior of so many clients? The reason is simple: `LD_PRELOAD`

> If you set `LD_PRELOAD` to the path of a shared object, that file will be loaded before any other library (including the C runtime, libc.so). So to run ls with your special malloc() implementation, do this:
>     $ LD_PRELOAD=/path/to/my/malloc.so /bin/ls

reference: [stackoverflow.com](http://stackoverflow.com/questions/426230/what-is-the-ld-preload-trick)

`Proxychains` uses that trick to redirect tcp/udp request to proxy. I guess the normal way to install it will change back `LD_PRELOAD` after `proxychains` command takes effect. But I write it in `.profile` so everything goes wrong. 
