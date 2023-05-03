# gfw-pac

科学上网。通过 gfwlist 和中国 IP 地址生成 PAC(Proxy auto-config) 文件。对存在于 gfwlist 的域名和解析出的 IP 在国外的域名使用代理。

## 特性

- 速度快，优先按域名匹配，再按解析后的 IP 匹配
- 可自定义需要代理的域名
- 可自定义本地开发 TLD 域名，例如 .test
- 可自定义不需要代理的域名
- 如果访问的域名不在列表里，但是 IP 在国外，也返回代理服务器

## 用法

编辑 `gfw.pac` 的第一行换成自己的代理服务器直接使用，或者手工运行 `gfw-pac.py` 生成自己的 pac 文件。

## gfw-pac.py 使用说明

```shell
usage: gfw-pac.py [-h] [-i GFWLIST] -f PAC -p PROXY [--user-rule USER_RULE] [--direct-rule DIRECT_RULE] [--localtld-rule LOCALTLD_RULE] [--ip-file IP_FILE]

options:
  -h, --help            show this help message and exit
  -i GFWLIST, --input GFWLIST
                        path to gfwlist
  -f PAC, --file PAC    path to output pac
  -p PROXY, --proxy PROXY
                        the proxy parameter in the pac file, for example, "SOCKS5 127.0.0.1:1080;"
  --user-rule USER_RULE
                        user rule file, which will be appended to gfwlist
  --direct-rule DIRECT_RULE
                        user rule file, contains domains not bypass proxy
  --localtld-rule LOCALTLD_RULE
                        local TLD rule file, contains TLDs with a leading dot not bypass proxy
  --ip-file IP_FILE     delegated-apnic-latest from apnic.net
```

参数说明：

```
-h 显示帮助
-i 指定本地 gfwlist 文件，若不指定则自动下载
-f (必须)输出的 pac 文件
-p (必须)指定代理服务器
--user-rule 自定义使用代理的域名文件，文件里每行一个域名
--direct-rule 自定义不使用代理的域名文件，文件里每行一个域名
--localtld-rule 自定义不使用代理的顶级域，文件里每行一个域名，必须带前导圆点（例如 .test）
--ip-file 指定本地的从 apnic 下载的 IP 分配文件。若不指定则自动从 apnic 下载
```

举例：

```shell
python3 gfw-pac.py \
        -i gfwlist.txt \
        -f gfw.pac \
        -p "PROXY 127.0.0.1:10809; SOCKS5 127.0.0.1:10808; DIRECT" \
        --user-rule custom-domains.txt \
        --direct-rule direct-domains.txt \
        --localtld-rule local-tlds.txt \
        --ip-file delegated-apnic-latest.txt
```

## 疑难解答

若自动下载 APNIC 的 IP 分配文件很慢，可自行用科学办法下载 <http://ftp.apnic.net/apnic/stats/apnic/delegated-apnic-latest> 后，用 `--ip-file` 参数指定下载好的文件。同理 gfwlist 从 <https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt> 下载后用 `-i` 参数指定。
