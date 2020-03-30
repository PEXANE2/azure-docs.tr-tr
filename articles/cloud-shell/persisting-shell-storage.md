---
title: Azure Bulut Uyp'ta dosyaları devam etti | Microsoft Dokümanlar
description: Azure Bulut Su ağılının dosyaları nasıl devam ettigini günüz.
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
ms.date: 02/24/2020
ms.author: damaerte
ms.openlocfilehash: d4d59642f638e7b1221c35a4bb281923571d5066
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80297596"
---
# <a name="persist-files-in-azure-cloud-shell"></a>Azure Bulut Uyp'ta dosyaları devam etti
Bulut Kabuğu, oturumlar arasında dosyaları sürdürmek için Azure Dosya depolamasını kullanır. İlk başlangıçta Bulut Shell, oturumlar arasında kalıcı dosyalar için yeni veya varolan bir dosya paylaşımını ilişkilendirmenizi ister.

> [!NOTE]
> Bash ve PowerShell aynı dosya paylaşımını paylaşır. Cloud Shell'de otomatik montajla yalnızca bir dosya paylaşımı ilişkilendirilebilir.

> [!NOTE]
> Bulut kabuğu depolama hesapları için Azure depolama güvenlik duvarı desteklenmez.

## <a name="create-new-storage"></a>Yeni depolama alanı oluşturma

Temel ayarları kullandığınızda ve yalnızca bir abonelik seçtiğinizde, Cloud Shell size en yakın desteklenen bölgede sizin adınıza üç kaynak oluşturur:
* Kaynak grubu: `cloud-shell-storage-<region>`
* Depolama hesabı:`cs<uniqueGuid>`
* Dosya paylaşımı:`cs-<user>-<domain>-com-<uniqueGuid>`

![Abonelik ayarı](media/persisting-shell-storage/basic-storage.png)

Dosya paylaşımı dizininizdeki `clouddrive` `$Home` gibi bağlanır. Bu tek seferlik bir eylemdir ve dosya paylaşımı sonraki oturumlarda otomatik olarak bağlanır. 

Dosya paylaşımı, dizininizdeki `$Home` verileri otomatik olarak devam eden sizin için oluşturulan 5 GB'lık bir görüntü de içerir. Bu hem Bash hem de PowerShell için geçerlidir.

## <a name="use-existing-resources"></a>Varolan kaynakları kullanma

Gelişmiş seçeneği kullanarak, varolan kaynakları ilişkilendirebilirsiniz. Bulut Bulutbölgesi seçerken, aynı bölgede birlikte bulunan bir destek depolama hesabı seçmeniz gerekir. Örneğin, atanan bölgeniz Batı ABD ise, Batı ABD'de de bulunan bir dosya paylaşımını ilişkilendirmeniz gerekir.

Depolama kurulum istemi göründüğünde, ek seçenekleri görüntülemek için **gelişmiş ayarları göster'i** seçin. Yerel olarak yedekli depolama (LRS), coğrafi yedekli depolama (GRS) ve bölge yedekli depolama (ZRS) hesapları için doldurulan depolama seçenekleri filtresi. 

> [!NOTE]
> DESTEK dosya paylaşımınız için ek esneklik için GRS veya ZRS depolama hesaplarının kullanılması önerilir. Hangi fazlalık türü hedeflerinize ve fiyat tercihinize bağlıdır. [Azure Depolama hesapları için çoğaltma seçenekleri hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/storage/common/storage-redundancy)

![Kaynak grup ayarı](media/persisting-shell-storage/advanced-storage.png)

## <a name="securing-storage-access"></a>Depolama erişimini güvence altına alma
Güvenlik için, her kullanıcı kendi depolama hesabını sağlamalıdır.  Rol tabanlı erişim denetimi (RBAC) için, kullanıcıların depolama hesabı düzeyinde katılımcı erişimine veya üzerinde olması gerekir.

Bulut Shell, belirli bir abonelik içinde bir depolama hesabında Azure Dosya Paylaşımı kullanır. Devralınan izinler sayesinde, aboneliğin yeterli erişim haklarına sahip kullanıcılar tüm depolama hesaplarına ve abonelikte bulunan dosya paylaşımlarına erişebilecektir.

Kullanıcılar, izinleri depolama hesabında veya abonelik düzeyinde ayarlayarak dosyalarına erişimi kilitlemelidir.

## <a name="supported-storage-regions"></a>Desteklenen depolama bölgeleri
Geçerli bölgenizi bulmak için `env` Bash'te çalışabilir `ACC_LOCATION`ve değişkeni `$env:ACC_LOCATION`veya PowerShell run'ı bulabilirsiniz. Dosya paylaşımları, dizininizde `$Home` kalıcı lık elde etmek için oluşturulan 5 GB'lık bir görüntü alır.

Bulut Shell makineleri aşağıdaki bölgelerde bulunur:

|Alan|Bölge|
|---|---|
|Kuzey ve Güney Amerika|Doğu ABD, Güney Orta ABD, Batı ABD|
|Avrupa|Kuzey Avrupa, Batı Avrupa|
|Asya Pasifik|Hindistan Orta, Güneydoğu Asya|

Müşteriler, verilerinin belirli bir bölgede depolanması şartı yoksa birincil bir bölge seçmelidir. Böyle bir gereksinimleri varsa, ikincil bir depolama bölgesi kullanılmalıdır.

### <a name="secondary-storage-regions"></a>İkincil depolama bölgeleri
İkincil bir depolama bölgesi kullanılıyorsa, ilişkili Azure depolama hesabı, bunları monte ettiğiniz Bulut Bulut suması makinesi olarak farklı bir bölgede bulunur. Örneğin, Jane depolama hesabını ikinci bir bölge olan Kanada Doğu'da bulunacak şekilde ayarlayabilir, ancak monte ettiği makine hala birincil bir bölgede bulunuyor. Onun verileri Kanada'da bulunur, ancak Amerika Birleşik Devletleri'nde işlenir.

> [!NOTE]
> İkincil bir bölge kullanılırsa, Cloud Shell için dosya erişimi ve başlangıç süresi daha yavaş olabilir.

Bir kullanıcı, `(Get-CloudDrive | Get-AzStorageAccount).Location` Dosya Paylaşımı'nın konumunu görmek için PowerShell'de çalıştırılabilir.

## <a name="restrict-resource-creation-with-an-azure-resource-policy"></a>Azure kaynak ilkesiyle kaynak oluşturmayı kısıtlama
Cloud Shell'de oluşturduğunuz depolama `ms-resource-usage:azure-cloud-shell`hesapları . Kullanıcıların Bulut Bulut'ta depolama hesapları oluşturmasına izin vermek istiyorsanız, bu belirli etiket tarafından tetiklenen etiketler için bir [Azure kaynak ilkesi](../azure-policy/json-samples.md) oluşturun.

## <a name="how-cloud-shell-storage-works"></a>Cloud Shell depolama nasıl çalışır? 
Bulut Kabuğu, dosyaları aşağıdaki yöntemlerin her ikisi yle de devam eder: 
* Dizininizin içindeki tüm `$Home` içeriği devam etmek için dizininizin bir disk görüntüsünü oluşturma. Disk görüntüsü, belirtilen dosya paylaşımınıza `acc_<User>.img` `fileshare.storage.windows.net/fileshare/.cloudconsole/acc_<User>.img`şu şekilde kaydedilir ve değişiklikleri otomatik olarak eşitler. 
* Doğrudan dosya paylaşımı `clouddrive` etkileşimi `$Home` için dizininizde olduğu gibi belirtilen dosya paylaşımınızı montaj. `/Home/<User>/clouddrive`' ye `fileshare.storage.windows.net/fileshare`eşlenir.
 
> [!NOTE]
> Dizininizdeki `$Home` SSH anahtarları gibi tüm dosyalar, monte edilmiş dosya paylaşımınızda depolanan kullanıcı diski resminizde kalıcıdır. Dizin ve monte edilmiş dosya `$Home` paylaşımında bilgileri kalıcı olarak uyguladığınızda en iyi uygulamaları uygulayın.

## <a name="clouddrive-commands"></a>clouddrive komutları

### <a name="use-the-clouddrive-command"></a>Komutu `clouddrive` kullanma
Bulut Kabuğu'nda, Cloud Shell'e monte edilen dosya paylaşımını el ile güncelleştirmenizi sağlayan " adlı " komutu `clouddrive`çalıştırabilirsiniz.

!["clouddrive" komutunu çalıştırma](media/persisting-shell-storage/clouddrive-h.png)

### <a name="list-clouddrive"></a>Liste`clouddrive`
Hangi dosya paylaşımının , `clouddrive`olarak `df` monte edildiğini bulmak için komutu çalıştırın. 

clouddrive'a giden dosya yolu, depolama hesabı adınızı ve dosya paylaşımınızı URL'de gösterir. Örneğin, `//storageaccountname.file.core.windows.net/filesharename`

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

### <a name="mount-a-new-clouddrive"></a>Yeni bir bulut sürücüsüne binin

#### <a name="prerequisites-for-manual-mounting"></a>Manuel montaj için ön koşullar
Komutu kullanarak Bulut Kabuğu ile ilişkili dosya paylaşımını `clouddrive mount` güncelleştirebilirsiniz.

Varolan bir dosya paylaşımını bağlarsanız, depolama hesapları nın seçtiğiniz Bulut Bulut bölgesinde bulunması gerekir. Çalıştırarak `env` ve kontrol ederek `ACC_LOCATION`konumu alın.

#### <a name="the-clouddrive-mount-command"></a>Komut `clouddrive mount`

> [!NOTE]
> Yeni bir dosya paylaşımı düzenliyorsanız, dizininiz `$Home` için yeni bir kullanıcı resmi oluşturulur. Önceki `$Home` resminiz önceki dosya paylaşımınızda tutulur.

Komutu `clouddrive mount` aşağıdaki parametrelerle çalıştırın:

```
clouddrive mount -s mySubscription -g myRG -n storageAccountName -f fileShareName
```

Daha fazla ayrıntı `clouddrive mount -h`görüntülemek için, burada gösterildiği gibi çalıştırın:

!['clouddrive mount' komutunu çalıştırma](media/persisting-shell-storage/mount-h.png)

### <a name="unmount-clouddrive"></a>Unmount clouddrive
Bulut Shell'e monte edilmiş bir dosya paylaşımını istediğiniz zaman atlayabilirsiniz. Cloud Shell'in kullanılmak üzere monte edilmiş bir dosya paylaşımı nı gerektirmesi nedeniyle, bir sonraki oturumda başka bir dosya paylaşımı oluşturmanız ve takmanız istenir.

1. `clouddrive unmount` öğesini çalıştırın.
2. İstemleri onaylayın ve onaylayın.

El ile silmediğiniz sürece dosya paylaşımınız var olmaya devam edecektir. Cloud Shell artık sonraki oturumlarda bu dosya paylaşımını aramaz. Daha fazla ayrıntı `clouddrive unmount -h`görüntülemek için, burada gösterildiği gibi çalıştırın:

!['clouddrive unmount' komutunu çalıştırma](media/persisting-shell-storage/unmount-h.png)

> [!WARNING]
> Bu komutu çalıştırmak hiçbir kaynağı silmese de, Bulut Kabuğu'na eşlenen bir kaynak grubunu, depolama hesabını `$Home` veya dosya paylaşımını el ile silmek dizin diski görselinizi ve dosya paylaşımınızdaki dosyaları siler. Bu eylem geri alınamaz.
## <a name="powershell-specific-commands"></a>PowerShell'e özel komutlar

### <a name="list-clouddrive-azure-file-shares"></a>Azure `clouddrive` dosya paylaşımlarını listele
Cmdlet, `Get-CloudDrive` Bulut `clouddrive` Kabuğu'nda şu anda monte edilen Azure dosya paylaşım bilgilerini alır. <br>
![Get-CloudDrive'ı Çalıştırma](media/persisting-shell-storage-powershell/Get-Clouddrive.png)

### <a name="unmount-clouddrive"></a>Unmount`clouddrive`
Bulut Shell'e monte edilmiş bir Azure dosya paylaşımını istediğiniz zaman atlayabilirsiniz. Azure dosya paylaşımı kaldırıldıysa, bir sonraki oturumda yeni bir Azure dosya paylaşımı oluşturmanız ve takmanız istenir.

Cmdlet, `Dismount-CloudDrive` geçerli depolama hesabından bir Azure dosya paylaşımını açar. Geçerli oturumu `clouddrive` sonlandırmak. Kullanıcıdan bir sonraki oturumda yeni bir Azure dosya paylaşımı oluşturması ve takması istenir.
![Sökme-CloudDrive'ı Çalıştırma](media/persisting-shell-storage-powershell/Dismount-Clouddrive.png)

[!INCLUDE [PersistingStorage-endblock](../../includes/cloud-shell-persisting-shell-storage-endblock.md)]

Not: Bir dosyada bir işlev tanımlamanız ve PowerShell cmdlets'ten aramanız gerekiyorsa, nokta işleci nin eklenmesi gerekir. Örneğin: . .\MyFunctions.ps1

## <a name="next-steps"></a>Sonraki adımlar
[Bulut Kabuğu Hızlı Başlat](quickstart.md) <br>
[Microsoft Azure Dosyaları depolama hakkında bilgi edinin](https://docs.microsoft.com/azure/storage/storage-introduction) <br>
[Depolama etiketleri hakkında bilgi edinin](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) <br>
