# Ubuntu on ZedBoard

## 環境需求

- ZedBoard
- SD Card (>4GB)
- PC (Ubuntu)

## 建置步驟

1. 下載此專案

    - `git clone https://github.com/zyx1121/zedboard.ubuntu`

2. 插入 SD 卡至電腦並確認裝置名稱

    - `df`

3. 卸載 SD (若確認名稱為 `sdb`)

    - `sudo umount /dev/sdb`

4. 進入 fdisk 重新分配 SD

    - `sudo fdisk /dev/sdb`

    列出所有分區
    - `p`
  
    清除一個分區, 請重複動作直到刪除所有分區
    - `d`, ` `

    建立新分區, 最後一個扇區輸入`+500M`, 其它保持預設
    - `n`, ` `, `+500M`, ` `

    更改分區為 W95 FAT32
    - `t`, `c`

    再建立新分區, 全部保持預設
    - `n`, ` `, ` `, ` `

    設定啟動標記
    - `a`, `1`

    儲存並退出 fdisk
    - `w`

5. 格式化分區

    - `sudo mkfs.vfat -F 32 -n boot /dev/sdb1`

    - `sudo mkfs.ext4 -L root /dev/sdb2`

6. 掛載 SD

    - `sudo mkdir -p /media/user/boot`
      
    - `sudo mount /dev/sdb1 /media/user/boot`
      
    - `sudo mkdir -p /media/user/root`
      
    - `sudo mount /dev/sdb2 /media/user/root`

    **注意：** "user" 要換成你的使用者名字

7. 複製 boot files 到 SD 的 boot 分區

    - `cd zedboard.ubuntu/boot`
      
    - `sudo cp ./* /media/user/boot`
      
    - `sync`

8. 下載 Ubuntu 並解壓到 SD 的 root 分區

    - `wget https://releases.linaro.org/archive/ubuntu/images/developer/15.12/linaro-vivid-developer-20151215-714.tar.gz`
      
    - `sudo tar xpf linaro-vivid-developer-20151215-714.tar.gz --strip-components=1 -C /media/user/root`
      
    - `sync`

9. 設定跳線, 使 ZedBoard 以 SD 卡開機, 並連接 UART USB 至電腦, 大功告成！

    若未如預期進入 Ubuntu 系統請重置 u-boot 環境變數
   
    重新啟動 ZedBoard 並取消 autoboot 進入 u-boot 輸入下列指令
   
    - `env default -a`
      
    - `env save`
      
    - `reset`
