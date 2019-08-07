---
title: Dosyaları Azure Cloud Shell Sürdür | Microsoft Docs
description: Azure Cloud Shell dosyalarının sürekli olarak devam edip etmediğini gösteren yönergeler.
services: azure
documentationcenter: ''
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/04/2018
ms.author: damaerte
ms.openlocfilehash: f60125123d019cbfa93bfc1b06da7ac90b54e311
ms.sourcegitcommit: d060947aae93728169b035fd54beef044dbe9480
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2019
ms.locfileid: "68742032"
---
[!INCLUDE [PersistingStorage-introblock](../../includes/cloud-shell-persisting-shell-storage-introblock.md)]

## <a name="how-cloud-shell-storage-works"></a>Cloud Shell depolama nasıl kullanılır 
Aşağıdaki yöntemlerin her ikisinde de dosyaları Cloud Shell devam ettirir: 
* Dizin içindeki tüm içerikleri kalıcı hale `$Home` getirmek için dizininizin disk görüntüsünü oluşturma. Disk görüntüsü, belirtilen dosya paylaşımınızda olduğu gibi `acc_<User>.img` `fileshare.storage.windows.net/fileshare/.cloudconsole/acc_<User>.img`kaydedilir ve değişiklikleri otomatik olarak eşitler. 
* Doğrudan dosya paylaşma etkileşimi için belirtilen `clouddrive` dosya paylaşımınızı `$Home` dizininize bağlama. `/Home/<User>/clouddrive`öğesine `fileshare.storage.windows.net/fileshare`eşlenir.
 
> [!NOTE]
> Dizininizdeki `$Home` SSH anahtarları gibi tüm dosyalar, bağlı dosya paylaşımınızda saklanan Kullanıcı diski yansımanızda kalıcıdır. `$Home` Dizininizde ve bağlı dosya paylaşımında bilgileri kalıcı hale getirebilmeniz için en iyi yöntemleri uygulayın.

## <a name="bash-specific-commands"></a>Bash 'e özgü komutlar

### <a name="use-the-clouddrive-command"></a>`clouddrive` Komutunu kullanın
Bash Cloud Shell ile, Cloud Shell bağlı dosya paylaşımının el ile güncelleştirilmesini `clouddrive`sağlayan adlı bir komutu çalıştırabilirsiniz.
!["CloudDrive" komutu çalıştırılıyor](media/persisting-shell-storage/clouddrive-h.png)

### <a name="mount-a-new-clouddrive"></a>Yeni bir CloudDrive takın

#### <a name="prerequisites-for-manual-mounting"></a>El ile bağlama önkoşulları
`clouddrive mount` Komutunu kullanarak Cloud Shell ilişkili dosya paylaşımının güncelleştirilmesini sağlayabilirsiniz.

Var olan bir dosya paylaşımından bağlama yaparsanız, depolama hesaplarının Select Cloud Shell bölgesinde bulunması gerekir. Bash 'ten çalıştırarak `env` ve öğesini `ACC_LOCATION`denetleyerek konumu alın.

#### <a name="the-clouddrive-mount-command"></a>`clouddrive mount` Komutu

> [!NOTE]
> Yeni bir dosya paylaşma oluşturuyorsanız, `$Home` dizininiz için yeni bir Kullanıcı görüntüsü oluşturulur. Önceki `$Home` resminiz önceki dosya paylaşımınızda tutulur.

`clouddrive mount` Komutunu aşağıdaki parametrelerle çalıştırın:

```
clouddrive mount -s mySubscription -g myRG -n storageAccountName -f fileShareName
```

Daha fazla ayrıntı görüntülemek için, `clouddrive mount -h`aşağıda gösterildiği gibi çalıştırın:

![' CloudDrive Mount 'komutunu çalıştırma](media/persisting-shell-storage/mount-h.png)

### <a name="unmount-clouddrive"></a>CloudDrive bağlantısını kaldır
Cloud Shell bağlanan bir dosya paylaşımının bağlantısını dilediğiniz zaman kaldırabilirsiniz. Cloud Shell bağlı bir dosya paylaşımının kullanılmasını gerektirdiğinden, bir sonraki oturumda başka bir dosya paylaşma oluşturmanız ve bağlamanız istenir.

1. `clouddrive unmount` öğesini çalıştırın.
2. İstemleri kabul edin ve onaylayın.

Dosya paylaşımınız, el ile silmediğiniz takdirde mevcut olmaya devam edecektir. Cloud Shell artık sonraki oturumlarda bu dosya paylaşımının aranmayacak. Daha fazla ayrıntı görüntülemek için, `clouddrive unmount -h`aşağıda gösterildiği gibi çalıştırın:

![' CloudDrive unmount'komutunu çalıştırma](media/persisting-shell-storage/unmount-h.png)

> [!WARNING]
> Bu komutu çalıştırmak hiçbir kaynağı silmez, bir kaynak grubu, depolama hesabı veya Cloud Shell eşlenen dosya paylaşımının el ile silinmesi, `$Home` Dizin disk görüntünüzü ve dosya paylaşımınızda bulunan dosyaları siler. Bu eylem geri alınamaz.

### <a name="list-clouddrive"></a>Listele`clouddrive`
Hangi dosya paylaşımının bağlı `clouddrive`olduğunu saptamak için `df` komutunu çalıştırın. 

CloudDrive dosya yolu, URL 'deki depolama hesabı adınızı ve dosya paylaşımınızı gösterir. Örneğin, `//storageaccountname.file.core.windows.net/filesharename`

```
justin@Azure:~$ df
Filesystem                                          1K-blocks   Used  Available Use% Mounted on
overlay                                             29711408 5577940   24117084  19% /
tmpfs                                                 986716       0     986716   0% /dev
tmpfs                                                 986716       0     986716   0% /sys/fs/cgroup
/dev/sda1                                           29711408 5577940   24117084  19% /etc/hosts
shm                                                    65536       0      65536   0% /dev/shm
//mystoragename.file.core.windows.net/fileshareName 5368709120    64 5368709056   1% /home/justin/clouddrive
justin@Azure:~$
```
## <a name="powershell-specific-commands"></a>PowerShell 'e özgü komutlar

### <a name="list-clouddrive-azure-file-shares"></a>Azure `clouddrive` dosya paylaşımlarını listeleyin
Cmdlet 'i, `clouddrive` Cloud Shell tarafından şu anda bağlı olan Azure dosya paylaşma bilgilerini alır. `Get-CloudDrive` <br>
![Get-CloudDrive çalıştırılıyor](media/persisting-shell-storage-powershell/Get-Clouddrive.png)

### <a name="unmount-clouddrive"></a>Madı`clouddrive`
Cloud Shell bağlanan bir Azure dosya paylaşımının bağlantısını dilediğiniz zaman kaldırabilirsiniz. Azure dosya paylaşımında kaldırılan bir sonraki oturumda yeni bir Azure dosya paylaşma oluşturmanız ve bağlamanız istenir.

`Dismount-CloudDrive` Cmdlet 'i geçerli depolama hesabından bir Azure dosya paylaşımının takar. Kaldırma, `clouddrive` geçerli oturumu sonlandırır. Kullanıcıdan bir sonraki oturum sırasında yeni bir Azure dosya paylaşımının oluşturulması ve bağlanması istenir.
![Dismount-CloudDrive çalıştırılıyor](media/persisting-shell-storage-powershell/Dismount-Clouddrive.png)

[!INCLUDE [PersistingStorage-endblock](../../includes/cloud-shell-persisting-shell-storage-endblock.md)]

Not: Bir dosyada bir işlev tanımlamanız ve PowerShell cmdlet 'lerinden çağırmak gerekiyorsa, nokta operatörü dahil olmalıdır. Örneğin:. .\MyFunctions.ps1

## <a name="next-steps"></a>Sonraki adımlar
[Bash Cloud Shell hızlı başlangıç](quickstart.md) <br>
[PowerShell Cloud Shell hızlı başlangıç](quickstart-powershell.md) <br>
[Microsoft Azure dosyaları depolama hakkında bilgi edinin](https://docs.microsoft.com/azure/storage/storage-introduction) <br>
[Depolama etiketleri hakkında bilgi edinin](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) <br>
