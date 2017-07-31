#简介
加密文件系统（比如eCryptfs）通过将加密服务集成到文件系统这一层面来解决上面的问题。本指南旨在通过介绍ecryptfs，快速上手一种ubuntu下的文件或目录加密方式。  

**step1**, 安装ecryptfs:  
`sudo apt-get install ecryptfs-utils`

**step2**, 新建一个用于存放`加密后的目录或文件`的目录，这里为了演示，我在~/Templates建立了一个目录test :  
`cd ~/Templates`  
`mkdir test`

**step3**, 新建一个目录
`mkdir testDocument`

**step4**, 输入命令
`sudo mount -t ecryptfs test testDocument`
然后按提示一路走下去
```
Passphrase:
Select cipher:
 1) aes: blocksize = 16; min keysize = 16; max keysize = 32
 2) blowfish: blocksize = 8; min keysize = 16; max keysize = 56
 3) des3_ede: blocksize = 8; min keysize = 24; max keysize = 24
 4) twofish: blocksize = 16; min keysize = 16; max keysize = 32
 5) cast6: blocksize = 16; min keysize = 16; max keysize = 32
 6) cast5: blocksize = 8; min keysize = 5; max keysize = 16
Selection [aes]:
Select key bytes:
 1) 16
 2) 32
 3) 24
Selection [16]:
Enable plaintext passthrough (y/n) [n]:
Enable filename encryption (y/n) [n]: y  
Filename Encryption Key (FNEK) Signature [cbd6dc63028e5602]:
Attempting to mount with the following options:
  ecryptfs_unlink_sigs
  ecryptfs_fnek_sig=cbd6dc63028e5602
  ecryptfs_key_bytes=16
  ecryptfs_cipher=aes
  ecryptfs_sig=cbd6dc63028e5602
WARNING: Based on the contents of [/root/.ecryptfs/sig-cache.txt],
it looks like you have never mounted with this key
before. This could mean that you have typed your
passphrase wrong.

Would you like to proceed with the mount (yes/no)? : yes
Would you like to append sig [cbd6dc63028e5602] to
[/root/.ecryptfs/sig-cache.txt]
in order to avoid this warning in the future (yes/no)? : yes
Successfully appended new sig to user sig cache file
Mounted eCryptfs
```
记住输入的**Passphrase**和`ecryptfs_sig=cbd6dc63028e5602`后面的字符串。

**step5**, 输入命令：  
`echo "hello, world">./testDocument/hel`  
输入`ls ./testDocument`得到结果`hel`,输入`cat ./testDocument/hel`得到结果`hello, world`，可见在testDocument目录下创建了一个包含字符串"hello, world"的文件。

step6, 输入命令:  
`ls test`  
显示了一堆乱码，

在testDocument目录里面打添加或修改后打文件或目录都会被加密然后加入到test目录里面。由于之前选择的参数`Enable filename encryption (y/n) [n]: y`，也会加密文件名。由此可见，只需像编辑普通目录一样编辑testDocument目录即可。

**step6**, 当编辑完成目录testDocument后，testDocument目录里打内容会被加密然后加入到test目录。此时输入命令：
`ls testDocument`
`sudo umount testDocument`  
`ls testDocument`
只在第一个命令输入后输出了`hel`，第二条命令解除了testDocument目录对test目录的解密挂载

此时，完成了加密过程。  

**step7**,
再执行**step4**,即可重新将test目录挂载到testDocument目录下，此过程自动解密。  
解密后，输入命令`ls testDocument`输出原本在`test`目录下文件的文件名（解密后的）

------------
[参考文档:Ecryptfs企业级加密文件系统](http://wiki.ubuntu.org.cn/Ecryptfs%E4%BC%81%E4%B8%9A%E7%BA%A7%E5%8A%A0%E5%AF%86%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F) 
