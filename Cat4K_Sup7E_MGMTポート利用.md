# WS-X45-SUP7-E= のマネージメントポートを利用する方法

## 背景
WS-X45-SUP7-E= 上にあるMGMTポートを利用する際に必要になる設定です。  
発売当初は、MGMTポートは利用できませんでしたが、最近のVersionでは利用できるようになっています。  
実際MGMTポート経由のOS転送が可能であることは確認しています。

## 設定方法とその例

- interface FastEthernet 1(MGMTポート)へのIPアドレス設定

Switch#`configure terminal`  
Enter configuration commands, one per line.  End with CNTL/Z.  
Switch(config)#  
Switch(config)#`interface FastEthernet 1`  
Switch(config-if)#`ip address 10.0.0.1 255.255.255.0`  
Switch(config-if)#`no shutdown`  
Switch(config-if)#`exit`  
Switch(config)#  
Switch(config)#`ip route vrf mgmtVrf 0.0.0.0 0.0.0.0 10.0.0.254`  
Switch(config)#  
Switch(config)#`end`  
Switch#  

- FTP/TFTPのsource interface設定

(注意:この設定を入れないと、MGMTポート経由のtftp/ftpによるファイル操作ができません。)

[tftp] Switch(config)#`ip tftp source-interface FastEthernet1`  
[ftp]  Switch(config)#`ip ftp source-interface FastEthernet1`  

## 設定後の通信確認

MGMTポートは、**VRF:mgmtVrf**にデフォルトで属しているため、Pingをするときは、**vrfオプション**を付けてPingをして下さい。  

Switch#`ping vrf mgmtVrf 10.0.0.XX`  

Ping疎通が確認できたら、tftp/ftpで、ファイル操作してください。  

## 参照先Link

[Installation and Configuration Note for the Catalyst 4500 E-Series Supervisor Engine 7-E](https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst4500/hardware/configuration/notes/OL_23144.html#wp153015)
