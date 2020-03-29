---
title: MySQL'i Azure'da OpenSUSE VM'ye yükleme
description: MySQL'i Azure'daki OpenSUSE Linux Sanal makinesine yüklemeyi öğrenin.
services: virtual-machines-linux
author: cynthn
ms.service: virtual-machines-linux
ms.subservice: workloads
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 07/11/2018
ms.author: cynthn
ms.openlocfilehash: 0d3f0a61da3654c31c99cfac43c86b081876f700
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78944586"
---
# <a name="install-mysql-on-a-virtual-machine-running-opensuse-linux-in-azure"></a>MySQL'i Azure'da OpenSUSE Linux çalıştıran sanal bir makineye yükleyin

[MySQL](https://www.mysql.com) popüler, açık kaynak kodlu bir SQL veritabanıdır. Bu öğretici, OpenSUSE Linux çalıştıran bir sanal makine oluşturmak ve sonra MySQL yüklemek nasıl gösterir.


CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme]( /cli/azure/install-azure-cli).

## <a name="create-a-virtual-machine-running-opensuse-linux"></a>OpenSUSE Linux çalıştıran sanal bir makine oluşturun

İlk olarak, bir kaynak grubu oluşturun. Bu örnekte, kaynak grubuna *mySQSUSEResourceGroup* adı verilir ve *Doğu ABD* bölgesinde oluşturulur.

```azurecli-interactive
az group create --name mySQLSUSEResourceGroup --location eastus
```

VM'yi oluşturun. Bu örnekte, VM *myVM* olarak adlandırılır ve VM boyutu *Standard_D2s_v3,* ancak iş yükünüz için en uygun olduğunu düşündüğünüz [VM boyutunu](sizes.md) seçmelisiniz.

```azurecli-interactive
az vm create --resource-group mySQLSUSEResourceGroup \
   --name myVM \
   --image openSUSE-Leap \
   --size Standard_D2s_v3 \
   --generate-ssh-keys
```

MySQL için bağlantı noktası 3306 üzerinden trafiğe izin vermek için ağ güvenlik grubuna bir kural eklemeniz gerekir.

```azurecli-interactive
az vm open-port --port 3306 --resource-group mySQLSUSEResourceGroup --name myVM
```

## <a name="connect-to-the-vm"></a>VM’ye bağlanma

VM'ye bağlanmak için SSH'yi kullanırsınız. Bu örnekte, VM'nin genel IP adresi *10.111.112.113'tür.* VM'yi oluşturduğunuzda çıktıda IP adresini görebilirsiniz.

```azurecli-interactive  
ssh 10.111.112.113
```

 
## <a name="update-the-vm"></a>VM'yi güncelleştirin
 
VM'ye bağlandıktan sonra sistem güncelleştirmelerini ve düzeltme emamalarını yükleyin. 
   
```bash
sudo zypper update
```

VM'nizi güncellemek için istemleri izleyin.

## <a name="install-mysql"></a>MySQL'i Yükleyin 


MySQL'i VM'de SSH üzerinden yükleyin. İstemleri uygun şekilde yanıtlayın.

```bash
sudo zypper install mysql
```
 
MySQL'i sistem önyükleme yaparken başlatacak şekilde ayarlayın. 

```bash
sudo systemctl enable mysql
```
MySQL'in etkin olduğunu doğrulayın.

```bash
systemctl is-enabled mysql
```

Bu döndürmeli: etkin.

Sunucuyu yeniden başlatın.

```bash
sudo reboot
```


## <a name="mysql-password"></a>MySQL şifresi

Yüklemeden sonra, MySQL kök parolası varsayılan olarak boştur. MySQL'i güvence altına almak için **mysql\_secure\_installation** komut dosyasını çalıştırın. Komut dosyası, MySQL kök parolasını değiştirmenizi, anonim kullanıcı hesaplarını kaldırmanızı, uzaktan kök oturum açmayı devre dışı kaldırmanızı, test veritabanlarını kaldırmanızı ve ayrıcalıklar tablosunu yeniden yüklemenizi ister. 

Sunucu yeniden başlatıldıktan sonra, vm tekrar ssh.

```azurecli-interactive  
ssh 10.111.112.113
```



```bash
mysql_secure_installation
```

## <a name="sign-in-to-mysql"></a>MySQL'de oturum açın

Artık oturum açabilir ve MySQL komut istemini girebilirsiniz.

```bash  
mysql -u root -p
```
Bu, veritabanıyla etkileşimde bulunmanız için SQL deyimleri verebileceğiniz MySQL komut istemine geçer.

Şimdi, yeni bir MySQL kullanıcısı oluşturun.

```sql
CREATE USER 'mysqluser'@'localhost' IDENTIFIED BY 'password';
```
   
Yarı kolon (;) satırın sonunda komutu sona erdirmek için çok önemlidir.


## <a name="create-a-database"></a>Veritabanı oluşturma


Bir veritabanı oluşturun `mysqluser` ve kullanıcıya izin ver.

```sql
CREATE DATABASE testdatabase;
GRANT ALL ON testdatabase.* TO 'mysqluser'@'localhost' IDENTIFIED BY 'password';
```
   
Veritabanı kullanıcı adları ve parolaları yalnızca veritabanına bağlanan komut dosyaları tarafından kullanılır.  Veritabanı kullanıcı hesabı adları, sistemdeki gerçek kullanıcı hesaplarını mutlaka temsil etmez.

Oturum açmayı başka bir bilgisayardan etkinleştirin. Bu örnekte, oturum açmaizni vermek için bilgisayarın IP adresi *10.112.113.114'tür.*

```sql
GRANT ALL ON testdatabase.* TO 'mysqluser'@'10.112.113.114' IDENTIFIED BY 'password';
```
   
MySQL veritabanı yönetim yardımcı programı çıkmak için, yazın:

```    
quit
```


## <a name="next-steps"></a>Sonraki adımlar
MySQL hakkında ayrıntılı bilgi için [MySQL Belgeleri'ne](https://dev.mysql.com/doc)bakın.




