# LinuxのNetworkManger『nmcli』でネットワーク設定を管理するための覚書

## NetworkManagerの状態を表示したい
```bash
$ nmcli general status
STATE     CONNECTIVITY  WIFI-HW  WIFI  WWAN-HW  WWAN 
接続済み  完全          有効     有効  有効     有効
```

## ホスト名を確認したい
```bash
$ nmcli general hostname
parrot-virtualbox
```

## ホスト名を編集したい
```bash
$ nmcli general hostname {new_hostname}
```

## ネットワークを有効または無効にしたい
```bash
$ nmcli networking {on | off}
```

## ネットワークの状態を確認したい
```bash
$ nmcli networking connectivity check
# full or noneが表示される
```

## wifiの状態を表示したい
```bash
$ nmcli radio wifi
# enabled or disabledが表示される
```

## wifiの接続を有効または無効にしたい
```bash
$ nmcli radio wifi {on | off}
```

## モバイルブロードバンド接続(4GLTE,5Gなど)の状態を表示したい
```bash
$ nmcli radio wwan
# enabled or disabledが表示される
```

## モバイルブロードバンド接続(4GLTE,5Gなど)を有効または無効にしたい
```bash
$ nmcli radio wwan {on | off}
# enabled or disabledが表示される
```

## 全ての無線接続を一括で有効または無効にしたい
```bash
$ nmcli radio all {on | off}
```

## 現在の接続情報を表示したい
```bash
# 全ての接続を表示
$ nmcli connection show

NAME        UUID                                  TYPE      DEVICE 
有線接続 1  7e709e70-ba3a-3a93-9ba6-1e084c932b58  ethernet  enp0s3

# activeな接続のみ表示
$ nmcli connection show --active
```

## 指定した接続を有効または無効にしたい
```bash
$ nmcli connection {up | down} {UUID}
```

## 指定した接続の設定を編集したい
```bash
# 複数の値から成るプロパティーの場合は、
# '+' 記号を使用すると、アイテムを追加
# '-' 記号を使用すると、アイテムを削除
# どちらも指定されない場合はプロパティ全体を上書きする
$ nmcli connection modify {NAME} {parameter}
```

## 静的IPを設定したい
```bash
$ nmcli connection modify {interface-name} ipv4.method manual
$ nmcli connection modify {interface-name} ipv4.addresses 192.168.1.1/24
```

## 指定した接続の設定をインタラクティブに編集したい
```bash
$ nmcli connection edit {UUID}

# プロファイル一覧を表示
nmcli> print
===============================================================================
                      接続プロファイルの詳細 (有線接続 1)
===============================================================================
connection.id:                          有線接続 1
connection.uuid:                        7e709e70-ba3a-3a93-9ba6-1e084c932b58
connection.stable-id:                   --
connection.type:                        802-3-ethernet
connection.interface-name:              enp0s3
connection.autoconnect:                 はい
connection.autoconnect-priority:        -999
connection.autoconnect-retries:         -1 (default)
connection.multi-connect:               0 (default)
connection.auth-retries:                -1
connection.timestamp:                   1659321253
connection.read-only:                   いいえ
connection.permissions:                 --
connection.zone:                        --
connection.master:                      --
connection.slave-type:                  --
connection.autoconnect-slaves:          -1 (default)
connection.secondaries:                 --
connection.gateway-ping-timeout:        0
connection.metered:                     不明
connection.lldp:                        default
connection.mdns:                        -1 (default)
connection.llmnr:                       -1 (default)
connection.wait-device-timeout:         -1
-------------------------------------------------------------------------------
802-3-ethernet.port:                    --
802-3-ethernet.speed:                   0
802-3-ethernet.duplex:                  --
802-3-ethernet.auto-negotiate:          いいえ
802-3-ethernet.mac-address:             --
802-3-ethernet.cloned-mac-address:      --
802-3-ethernet.generate-mac-address-mask:--
802-3-ethernet.mac-address-blacklist:   --
802-3-ethernet.mtu:                     自動
802-3-ethernet.s390-subchannels:        --
802-3-ethernet.s390-nettype:            --
802-3-ethernet.s390-options:            --
802-3-ethernet.wake-on-lan:             default
802-3-ethernet.wake-on-lan-password:    --
-------------------------------------------------------------------------------
ipv4.method:                            auto
ipv4.dns:                               --
ipv4.dns-search:                        --
ipv4.dns-options:                       --
ipv4.dns-priority:                      0
ipv4.addresses:                         --
ipv4.gateway:                           --
ipv4.routes:                            --
ipv4.route-metric:                      -1
ipv4.route-table:                       0 (unspec)
ipv4.routing-rules:                     --
ipv4.ignore-auto-routes:                いいえ
ipv4.ignore-auto-dns:                   いいえ
ipv4.dhcp-client-id:                    --
ipv4.dhcp-iaid:                         --
ipv4.dhcp-timeout:                      0 (default)
ipv4.dhcp-send-hostname:                はい
ipv4.dhcp-hostname:                     --
ipv4.dhcp-fqdn:                         --
ipv4.dhcp-hostname-flags:               0x0 (none)
ipv4.never-default:                     いいえ
ipv4.may-fail:                          はい
ipv4.dad-timeout:                       -1 (default)
ipv4.dhcp-vendor-class-identifier:      --
ipv4.dhcp-reject-servers:               --
-------------------------------------------------------------------------------
ipv6.method:                            auto
ipv6.dns:                               --
ipv6.dns-search:                        --
ipv6.dns-options:                       --
ipv6.dns-priority:                      0
ipv6.addresses:                         --
ipv6.gateway:                           --
ipv6.routes:                            --
ipv6.route-metric:                      -1
ipv6.route-table:                       0 (unspec)
ipv6.routing-rules:                     --
ipv6.ignore-auto-routes:                いいえ
ipv6.ignore-auto-dns:                   いいえ
ipv6.never-default:                     いいえ
ipv6.may-fail:                          はい
ipv6.ip6-privacy:                       -1 (unknown)
ipv6.addr-gen-mode:                     stable-privacy
ipv6.ra-timeout:                        0 (default)
ipv6.dhcp-duid:                         --
ipv6.dhcp-iaid:                         --
ipv6.dhcp-timeout:                      0 (default)
ipv6.dhcp-send-hostname:                はい
ipv6.dhcp-hostname:                     --
ipv6.dhcp-hostname-flags:               0x0 (none)
ipv6.token:                             --
-------------------------------------------------------------------------------
proxy.method:                           none
proxy.browser-only:                     いいえ
proxy.pac-url:                          0
proxy.pac-script:                       --
-------------------------------------------------------------------------------

nmcli> set {profile name} {value}
nmcli> save
nmcli> quit
```

## デバイスの状態を確認したい
```bash
$ nmcli device status

DEVICE  TYPE      STATE     CONNECTION 
enp0s3  ethernet  接続済み  有線接続 1 
lo      loopback  管理無し  --
```

## 指定したデバイスの情報を確認したい
```bash
$ nmcli device show {DEVICE}

GENERAL.DEVICE:                         enp0s3
GENERAL.TYPE:                           ethernet
GENERAL.HWADDR:                         08:00:27:F9:8C:93
GENERAL.MTU:                            1500
GENERAL.STATE:                          100 (接続済み)
GENERAL.CONNECTION:                     有線接続 1
GENERAL.CON-PATH:                       /org/freedesktop/NetworkManager/ActiveConnection/3
WIRED-PROPERTIES.CARRIER:               オン
IP4.ADDRESS[1]:                         10.0.2.15/24
IP4.GATEWAY:                            10.0.2.2
IP4.ROUTE[1]:                           dst = 0.0.0.0/0, nh = 10.0.2.2, mt = 100
IP4.ROUTE[2]:                           dst = 10.0.2.0/24, nh = 0.0.0.0, mt = 100
IP4.DNS[1]:                             192.168.177.21
IP4.DNS[2]:                             192.168.193.21
IP4.DNS[3]:                             192.168.50.1
IP4.DOMAIN[1]:                          test.local
IP6.ADDRESS[1]:                         fe80::7a8b:e58b:f189:9b1b/64
IP6.GATEWAY:                            --
IP6.ROUTE[1]:                           dst = fe80::/64, nh = ::, mt = 100
IP6.ROUTE[2]:                           dst = ff00::/8, nh = ::, mt = 256, table=255
```

## デバイスの設定を変更したい
```bash
$ nmcli device modify {DEVICE} {parameter}
```

## 指定したデバイスを接続または切断したい
```bash
$ nmcli device {connect | disconnect} {DEVICE}
```

## デバイスを削除したい
```bash
$ nmcli device delete {DEVICE}
```

## デバイスの状態を継続監視したい
```bash
$ nmcli device monitor {DEVICE}

# デバイスが接続、切断されると標準出力に以下のような出力がされる
enp0s3: 停止中
enp0s3: 切断済み
enp0s3: 接続 '有線接続 1' を使用中
enp0s3: 接続中 (準備)
enp0s3: 接続中 (設定中)
enp0s3: 接続中 (IP 設定を取得中)
enp0s3: 接続中 (IP の接続性チェック)
enp0s3: 接続中 (セカンダリー接続を開始)
enp0s3: 接続済み
```

## wifiのアクセスポイントを表示したい
```bash
$ nmcli device wifi list
```

## wifiのアクセスポイントを再検索したい
```bash
$ nmcli device wifi rescan
```

## wifiのアクセスポイントに接続したい
```bash
$ nmcli device wifi connect {SSID}
```

## wifiのホットスポットを作成したい
```bash
$ nmcli device wifi hotspot
```

