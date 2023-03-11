# Transfer
<a title="Release" target="_blank" href="https://github.com/Mikubill/transfer/releases"><img src="https://img.shields.io/github/release/Mikubill/transfer.svg?style=flat-square&hash=c7"></a>
<a title="Go Report Card" target="_blank" href="https://goreportcard.com/report/github.com/Mikubill/transfer"><img src="https://goreportcard.com/badge/github.com/Mikubill/transfer?style=flat-square"></a>

🍭集合多个 API 的大文件传输工具

## install
直接下载二进制
```shell script
# Stable Release
curl -sL https://git.io/file-transfer | sh 
```
或者直接下压缩包(linux amd64)
```shell script
wget https://gh.ddlc.top/https://github.com/Mikubill/transfer/releases/download/v0.4.17/transfer_0.4.17_linux_amd64.tar.gz
tar -zxvf transfer_0.4.17_linux_amd64.tar.gz
rm transfer_0.4.17_linux_amd64.tar.gz
```

scoop
```shell script
scoop install transfer
```

## support

文件上传范例

```bash
./transfer <backend> <your-file-path>

./transfer wet /home/user/file.bin
```

目前支持的文件传输服务：

|  Name  | Command | Site  | Limit |
|  ----  | ----  | ----  |  ----  |
| ⭐WeTransfer | `wet` | https://wetransfer.com/ | 2GB |
| CatBox | `cat` | https://catbox.moe/ | 200MB |
| Fileio | `fio` | https://file.io/ | 100MB | 
| Wenshushu | `wss` | https://wenshushu.cn/ | 2GB |
| Transfer.sh | `trs` | https://transfer.sh/ | slow |
| LitterBox | `lit` | https://litterbox.catbox.moe/ | 1GB |
| Null | `null` | https://0x0.st/ | 512M |
| Infura (ipfs) | `inf` | https://infura.io/ | 128M |
| Quickfile | `qf` | https://quickfile.cn | 512M |
| DownloadGG | `gg` | https://download.gg/ | - |

|  Name   | Command | Site  | 
| CowTransfer | `cow` | https://www.cowtransfer.com/ | 

[登陆上传相关说明](#login)

## picbed support

图床上传范例

```bash
./transfer image <your-image-path> -b <backend>

./transfer image /home/user/image.png -b tg
```

目前支持的图床：

|  Name  | Command | Site  | 
|  ----  | ----  |  ----  |  
| CCUpload | `-b cc` | https://upload.cc/ | 
| Telegraph | `-b tg` | https://telegra.ph/ | 
| Prntscr | `-b pr` | https://prnt.sc/ | 

支持部分 chevereto 搭建的图床服务（beta，仅公开上传）：

|  Name  | Command | Site  | 
|  ----  | ----  |  ----  | 
| ImgLoc | `-b ch -d imgloc.com` | https://imgloc.com/ | 
| ImgTu | `-b ch -d imgtu.com` | https://imgtu.com/ | 
| ImgTg | `-b ch -d imgtg.com` | https://imgtg.com/ | 
| ZPhotos | `-b ch -d z.photos` | https://z.photos/ | 

以下图床为实验性支持：

|  Name  | Command | Site  | 
|  ----  | ----  |  ----  | 
| ImgTP | `-b itp` | https://imgtp.com/ | 
| ImgURL | `-b iu` | https://imgurl.com/ | 
| ImgKr | `-b ikr` | https://imgkr.com/ | 
| ImgBox | `-b box` | https://imgbox.com/ | 

## usage 

上传完下载操作会自动识别支持的链接，不需要指定服务名称。

```shell script
# download file
./transfer https://.../
```



### login 

部分 backend 支持登陆环境下上传，使用时只需要提供对应的 cookie 即可。

CowTransfer

```shell script
# login to upload
./transfer cow --cookie="remember-mev2=...;" -a "<cow-auth-token>" file
```

AirPortal

```shell script
# login to upload
./transfer arp -t <your-token> -u <your-username> file
```

TmpLink 
```shell script
# login to upload
./transfer tmp -t <your-token> file
```

Lanzous

蓝奏云可以只使用 `phpdisk_info` 项作为 cookie 上传文件，但可能无法进行文件管理（如删除等）。如需要上传到指定目录或进行文件管理操作需要在 cookie 中指定 `folder_id_c` 的值，如：

```shell script
# login to upload (without path)
./transfer lzs --cookie='phpdisk_info=...' file

# login to upload (with path)
./transfer lzs --cookie='phpdisk_info=...; folder_id_c=...;' file
```

### image

transfer 也支持上传图片至图床，默认自动使用阿里图床上传，也可以通过 `-b, --backend` 指定图床。

```text

Upload a image to imageBed.
Default backend is ali.backend, you can modify it by -b flag.

Backend support:
  alibaba(ali), baidu(bd), ccupload(cc), juejin(jj),
  netease(nt), prntscr(pr), smms(sm), sogou(sg),
  toutiao(tt), xiaomi(xm), vimcn(vm), suning(sn)

Example:
  # simply upload
  transfer image your-image

  # specify backend to upload
  transfer image -b sn your-image

Note: Image bed backend may have strict size or format limit.

Usage:
  transfer image [flags]

Flags:
  -b, --backend string   Set upload/download backend
  -h, --help             help for image

Global Flags:
      --encrypt              encrypt stream when upload
      --encrypt-key string   specify the encrypt key
      --keep                 keep program active when process finish
  -v, --verbose              enable verbose mode to debug
      --version              show version and exit
```

### encrypt & decrypt

和前面 upload 使用的是同样的加密，只是在本地进行。也可以使用前面下载的加密后文件在此解密。可以通过不同参数指定密钥和输出文件名

关于加密的说明：目前只能选择 AES-CBC 的加密方式，分块大小策略为 min(1m, fileSize)

```shell script 
# encrypt
transfer encrypt your-file

# encrypt using specified key
transfer encrypt -k abc your-file

# decrypt using specified key
transfer decrypt -k abc your-file

# specify path
transfer encrypt -o output your-file
```

### hash 

hash 功能使用 sha1, crc32, md5, sha256 对文件进行校验，可以用来检验文件一致性。

```shell script 
➜  ./transfer hash main.go
size: 68
path: /../transfer/main.go

crc32: a51da8f5
md5: aa091bb918ab85b1dc44cb771b1663d1
sha1: a8e25d41330c545da8bcbeade9aebdb1b4a13ab7
sha256: ab4dd3cdd79b5e2a88fcb3fcd45dfcffc935c913adfa888f3fb50b324638e958
```
