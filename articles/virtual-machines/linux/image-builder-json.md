---
title: Azure Resim Oluşturucu şablonu oluşturma (önizleme)
description: Azure Image Builder ile kullanılacak bir şablonu nasıl oluşturabilirsiniz öğrenin.
author: danis
ms.author: danis
ms.date: 03/24/2020
ms.topic: article
ms.service: virtual-machines-linux
ms.subservice: imaging
manager: gwallace
ms.openlocfilehash: e1f1bc09406c34836c13deb805fa399ab4751d41
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80246798"
---
# <a name="preview-create-an-azure-image-builder-template"></a>Önizleme: Azure Resim Oluşturucu şablonu oluşturma 

Azure Image Builder, Bilgileri Image Builder hizmetine aktarmak için bir .json dosyası kullanır. Bu makalede, json dosyasının bölümleri üzerinden gidecek, böylece kendi oluşturabilirsiniz. Tam .json dosyalarının örneklerini görmek için [Azure Image Builder GitHub'a](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts)bakın.

Bu temel şablon biçimidir:

```json
 { 
    "type": "Microsoft.VirtualMachineImages/imageTemplates", 
    "apiVersion": "2019-05-01-preview", 
    "location": "<region>", 
    "tags": {
        "<name": "<value>",
        "<name>": "<value>"
             }
    "identity":{},           
    "dependsOn": [], 
    "properties": { 
        "buildTimeoutInMinutes": <minutes>, 
        "vmProfile": 
            {
            "vmSize": "<vmSize>",
            "osDiskSizeGB": <sizeInGB>,
            "vnetConfig": {
                "name": "<vnetName>",
                "subnetName": "<subnetName>",
                "resourceGroupName": "<vnetRgName>"
            },
        "source": {}, 
        "customize": {}, 
        "distribute": {} 
      } 
 } 
```



## <a name="type-and-api-version"></a>Türü ve API sürümü

Kaynak `type` türü, olması `"Microsoft.VirtualMachineImages/imageTemplates"`gereken. API `apiVersion` değiştikçe zaman içinde değişecektir, `"2019-05-01-preview"` ancak önizleme için olmalıdır.

```json
    "type": "Microsoft.VirtualMachineImages/imageTemplates",
    "apiVersion": "2019-05-01-preview",
```

## <a name="location"></a>Konum

Konum, özel görüntünün oluşturulacağı bölgedir. Image Builder önizlemesi için aşağıdaki bölgeler desteklenir:

- Doğu ABD
- Doğu ABD 2
- Orta Batı ABD
- Batı ABD
- Batı ABD 2
- Kuzey Avrupa
- Batı Avrupa


```json
    "location": "<region>",
```
## <a name="vmprofile"></a>vmProfil
Varsayılan olarak Görüntü Oluşturucu bir "Standard_D1_v2" oluşturmak VM kullanır, bu geçersiz kılar, örneğin, bir GPU VM için görüntü özelleştirmek istiyorsanız, bir GPU VM boyutu gerekir. Bu isteğe bağlıdır.

```json
 {
    "vmSize": "Standard_D1_v2"
 },
```

## <a name="osdisksizegb"></a>osDiskSizeGB

Varsayılan olarak, Görüntü Oluşturucu görüntünün boyutunu değiştirmez, kaynak görüntüboyutu kullanır. İşletim sistemi diskinin (Win ve Linux) boyutunu artırabilirsiniz, bu isteğe bağlıdır ve 0 değeri kaynak görüntüyle aynı boyutta bırakmak anlamına gelir. 

```json
 {
    "osDiskSizeGB": 100
 },
```

## <a name="vnetconfig"></a>vnetConfig
Herhangi bir VNET özelliği belirtmezseniz, Image Builder kendi VNET, Public IP ve NSG'sini oluşturur. Public IP, hizmetin yapı VM ile iletişim kurabilmesi için kullanılır, ancak bir Genel IP istemiyorsanız veya Image Builder'ın yapılandırma sunucuları (DSC, Chef, Puppet, Ansible), dosya paylaşımları vb. gibi mevcut VNET kaynaklarınıza erişebilmesini istemiyorsanız kullanılır. , sonra bir VNET belirtebilirsiniz. Daha fazla bilgi için ağ belgelerini gözden [geçirin,](https://github.com/danielsollondon/azvmimagebuilder/blob/master/aibNetworking.md#networking-with-azure-vm-image-builder)bu isteğe bağlıdır.

```json
    "vnetConfig": {
        "name": "<vnetName>",
        "subnetName": "<subnetName>",
        "resourceGroupName": "<vnetRgName>"
    }
```
## <a name="tags"></a>Etiketler

Bunlar, oluşturulan görüntü için belirtebileceğiniz anahtar/değer çiftleridir.

## <a name="depends-on-optional"></a>Bağlıdır (isteğe bağlı)

Bu isteğe bağlı bölüm, devam etmeden önce bağımlılıkların tamamlandığından emin olmak için kullanılabilir. 

```json
    "dependsOn": [],
```

Daha fazla bilgi için kaynak [bağımlılıklarını tanımla'ya](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-define-dependencies#dependson)bakın.

## <a name="identity"></a>Kimlik
Varsayılan olarak, Image Builder komut dosyalarını kullanmayı veya GitHub ve Azure depolama alanı gibi birden çok konumdan dosya kopyalamayı destekler. Bunları kullanmak için herkese açık olmaları gerekir.

Kimlik Azure depolama hesabında en az 'Depolama Blob Veri Okuyucusu' verildiği sürece, Görüntü Oluşturucu'nun Azure Depolama'ya erişmesine izin vermek için sizin adınız tarafından tanımlanan Azure Kullanıcı Tarafından Atanmış Yönetilen Kimlik'i de kullanabilirsiniz. Bu, depolama lekelerini dışarıdan erişilebilir hale getirmeniz veya SAS Belirteçleri'ni kurmanız gerekolmadığı anlamına gelir.


```json
    "identity": {
    "type": "UserAssigned",
          "userAssignedIdentities": {
            "<imgBuilderId>": {}
        }
        },
```

Tam bir örnek için bkz: [Azure Depolama'daki dosyalara erişmek için Azure Kullanıcı Tarafından Atanmış Yönetilen Kimlik Kullan.](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts/7_Creating_Custom_Image_using_MSI_to_Access_Storage)

Kullanıcı tarafından atanan kimlik için Görüntü Oluşturucu desteği: • Yalnızca tek bir kimliği destekler • Özel alan adlarını desteklemez

Daha fazla bilgi edinmek için Azure [kaynakları için yönetilen kimlikler nedir?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)
Bu özelliği dağıtma hakkında daha fazla bilgi için, [Azure CLI'yi kullanarak Azure VM'de Azure kaynakları için yönetilen kimlikleri yapılandırma](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm#user-assigned-managed-identity)'ya bakın.

## <a name="properties-source"></a>Özellikleri: kaynak

Bu `source` bölüm, Image Builder tarafından kullanılacak kaynak görüntü hakkında bilgi içerir.

API, görüntü oluşturmanın kaynağını tanımlayan bir 'SourceType' gerektirir, şu anda üç tür vardır:
- PlatformImage - kaynak görüntü bir Pazar görüntüsü olduğunu belirtti.
- ManagedImage - normal yönetilen bir görüntüden başlarken bunu kullanın.
- SharedImageVersion - Bu kaynak olarak Paylaşılan Resim Galerisi'nde bir resim sürümü kullanırken kullanılır.

### <a name="iso-source"></a>ISO kaynağı
Şimdi [RHEL Bring Your Own Abonelik görüntüleri](https://docs.microsoft.com/azure/virtual-machines/workloads/redhat/byos)olduğu gibi, görüntü oluşturucu bu işlevselliği küçümseyen, aşağıdaki zaman çizelgeleri gözden geçirin:
    * 31 Mart 2020 - RHEL ISO kaynaklarına sahip Görüntü Şablonları artık kaynak sağlayıcısı tarafından kabul edilecektir.
    * 30 Nisan 2020- RHEL ISO kaynaklarını içeren Görüntü Şablonları artık işlenmeyecektir.

### <a name="platformimage-source"></a>PlatformImage kaynağı 
Azure Image Builder, Windows Server ve istemciyi destekler ve Linux Azure Marketi görüntülerini tam liste için [buraya](https://docs.microsoft.com/azure/virtual-machines/windows/image-builder-overview#os-support) bakın. 

```json
        "source": {
            "type": "PlatformImage",
                "publisher": "Canonical",
                "offer": "UbuntuServer",
                "sku": "18.04-LTS",
                "version": "latest"
        },
```


Buradaki özellikler, AZ CLI kullanarak VM's oluşturmak için kullanılan özellikleri almak için aşağıdakileri çalıştırın: 
 
```azurecli-interactive
az vm image list -l westus -f UbuntuServer -p Canonical --output table –-all 
```

Sürümde 'en son' kullanabilirsiniz, sürüm görüntü oluşturma gerçekleştiğinde değerlendirilir, şablon gönderildiğinde değil. Bu işlevselliği Paylaşılan Resim Galerisi hedefiyle kullanırsanız, şablonu yeniden göndermeyi önleyebilir ve resim oluşturmayı aralıklarla yeniden çalıştırabilirsiniz, böylece resimleriniz en son resimlerden yeniden oluşturulur.

### <a name="managedimage-source"></a>ManagedImage kaynağı

Kaynak görüntüyü genelleştirilmiş bir VHD veya VM'nin varolan yönetilen görüntüsü olarak ayarlar. Kaynak yönetilen görüntü desteklenen bir işletim sistemi olmalıdır ve Azure Image Builder şablonunuzla aynı bölgede olmalıdır. 

```json
        "source": { 
            "type": "ManagedImage", 
                "imageId": "/subscriptions/<subscriptionId>/resourceGroups/{destinationResourceGroupName}/providers/Microsoft.Compute/images/<imageName>"
        }
```

Yönetilen `imageId` görüntünün ResourceId olmalıdır. Kullanılabilir `az image list` resimleri listelemek için kullanın.


### <a name="sharedimageversion-source"></a>SharedImageVersion kaynağı
Kaynak resmi Paylaşılan Resim Galerisi'nde varolan bir resim sürümünü ayarlar. Görüntü sürümü desteklenen bir işletim sistemi olmalıdır ve görüntü Azure Image Builder şablonunuzla aynı bölgeye çoğaltılmalıdır. 

```json
        "source": { 
            "type": "SharedImageVersion", 
            "imageVersionID": "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/p  roviders/Microsoft.Compute/galleries/<sharedImageGalleryName>/images/<imageDefinitionName/versions/<imageVersion>" 
   } 
```

Görüntü `imageVersionId` sürümünün ResourceId olmalıdır. Resim sürümlerini listelemek için [az sig resim-sürüm listesini](/cli/azure/sig/image-version#az-sig-image-version-list) kullanın.

## <a name="properties-buildtimeoutinminutes"></a>Özellikler: buildTimeoutInMinutes

Varsayılan olarak, Görüntü Oluşturucu su 240 dakika çalışır. Bundan sonra, görüntü oluşturma tamamlandı olsun ya da olmasın, zaman dışarı ve durdurmak. Zaman anına isabet edilirse, buna benzer bir hata görürsünüz:

```text
[ERROR] Failed while waiting for packerizer: Timeout waiting for microservice to
[ERROR] complete: 'context deadline exceeded'
```

Bir buildTimeoutInMinutes değeri belirtmezseniz veya 0 olarak ayarlarsanız, bu varsayılan değeri kullanır. Değeri en fazla 960 min 'e (16 saat) kadar artırabilir veya azaltabilirsiniz. Windows için, bunu 60 dakikanın altına ayarlamanızı önermiyoruz. Zaman aşımını bulursanız, özelleştirme adımının kullanıcı girişi gibi bir şey bekleyip beklemediğinizi görmek için [günlükleri](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#collecting-and-reviewing-aib-image-build-logs)gözden geçirin. 

Özelleştirmelerin tamamlanması için daha fazla zamana ihtiyacınız olduğunu düşünüyorsanız, bunu biraz genel merkezle ihtiyacınız olduğunu düşündüğünüz şeye ayarlayın. Ancak, bir hata görmeden önce zaman anına gelmesini beklemek zorunda kaldığınız için çok yüksek ayaryapmayın. 


## <a name="properties-customize"></a>Özellikler: özelleştirmek

Image Builder birden çok 'özelleştirici' destekler. Özelleştiriciler, görüntünüzün çalıştırılması veya sunucuların yeniden başlatılması gibi resminizi özelleştirmek için kullanılan işlevlerdir. 

`customize`Kullanırken: 
- Birden çok özelleştirici kullanabilirsiniz, ancak `name`benzersiz bir olması gerekir.
- Özelleştiriciler şablonda belirtilen sırada çalıştırın.
- Bir özelleştirmebaşarısız olursa, tüm özelleştirme bileşeni başarısız olur ve bir hatayı geri bildirir.
- Bir şablonda kullanmadan önce komut dosyasını iyice test etmemeniz önerilir. Komut dosyasını kendi VM'nizde hata ayıklamanız daha kolay olacaktır.
- Hassas verileri komut dosyalarına koymayın. 
- [MSI](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts/7_Creating_Custom_Image_using_MSI_to_Access_Storage)kullanmıyorsanız komut dosyası konumlarının herkese açık olması gerekir.

```json
        "customize": [
            {
                "type": "Shell",
                "name": "<name>",
                "scriptUri": "<path to script>",
                "sha256Checksum": "<sha256 checksum>"
            },
            {
                "type": "Shell",
                "name": "<name>",
                "inline": [
                    "<command to run inline>",
                ]
            }

        ],
```     

 
Özelleştirme bölümü bir dizidir. Azure Image Builder, özelleştiriciler arasında sırayla çalışır. Herhangi bir özelleştirici herhangi bir hata yapı işlemi başarısız olur. 
 
 
### <a name="shell-customizer"></a>Kabuk özelleyici

Kabuk özelleştirici, kabuk komut dosyalarının çalıştırılmasını destekler, bunlar IB'nin bunlara erişmesi için herkese açık olmalıdır.

```json
    "customize": [ 
        { 
            "type": "Shell", 
            "name": "<name>", 
            "scriptUri": "<link to script>",
            "sha256Checksum": "<sha256 checksum>"       
        }, 
    ], 
        "customize": [ 
        { 
            "type": "Shell", 
            "name": "<name>", 
            "inline": "<commands to run>"
        }, 
    ], 
```

İşletim Sistemi Desteği: Linux 
 
Özellikleri özelleştirin:

- **türü** – Kabuk 
- **ad** - özelleştirmeyi izlemek için ad 
- **scriptUri** - URI dosyanın konumuna 
- **inline** - kabuk komutları dizisi, virgüllerle ayrılmıştır.
- **sha256Checksum** - Dosyanın sha256 checksum değeri, bu yerel oluşturmak ve sonra Image Builder checksum ve doğrulamak.
    * Mac/Linux pistinde bir terminal kullanarak sha256Checksum'u oluşturmak için:`sha256sum <fileName>`


Komutların süper kullanıcı ayrıcalıklarıyla çalışması için önceden belirlenmiş `sudo`olmaları gerekir.

> [!NOTE]
> Kabuk özelleyicisini RHEL ISO kaynağıyla çalıştırırken, herhangi bir özelleştirme oluşmadan önce ilk özelleştirme kabuk işlemenizin bir Red Hat yetkilendirme sunucusuna kaydolduğundan emin olmanız gerekir. Özelleştirme tamamlandıktan sonra, komut dosyası nın yetkilendirme sunucusuna kaydını silmesi gerekir.

### <a name="windows-restart-customizer"></a>Windows yeniden başlat özelleyici 
Yeniden Başlat özelleyici, bir Windows VM'yi yeniden başlatmanızı ve yeniden çevrimiçi olmasını beklemenizi sağlar, bu da yeniden başlatma gerektiren yazılımları yüklemenize olanak tanır.  

```json 
     "customize": [ 

            {
                "type": "WindowsRestart",
                "restartCommand": "shutdown /r /f /t 0", 
                "restartCheckCommand": "echo Azure-Image-Builder-Restarted-the-VM  > c:\\buildArtifacts\\azureImageBuilderRestart.txt",
                "restartTimeout": "5m"
            }
  
        ],
```

İşletim Sistemi Desteği: Windows
 
Özellikleri özelleştirin:
- **Türü**: WindowsRestart
- **restartCommand** - Komut yeniden başlatma (isteğe bağlı) yürütmek için. Varsayılan değer: `'shutdown /r /f /t 0 /c \"packer restart\"'`.
- **restartCheckCommand** – Komut yeniden başlatma başarılı olup olmadığını kontrol etmek için (isteğe bağlı). 
- **yeniden başlatmaTimeout** - Büyüklük ve birim dizesi olarak belirtilen zaman dışarıyı yeniden başlatın. Örneğin, `5m` (5 dakika) `2h` veya (2 saat). Varsayılan değer: '5m'

### <a name="linux-restart"></a>Linux yeniden başlatma  
Linux Yeniden Başlatma özelleyicisi yoktur, ancak sürücüleri veya yeniden başlatma gerektiren bileşenler yüklüyorsanız, bunları yükleyebilir ve Shell özelleyicisini kullanarak yeniden başlatabilirsiniz, yapı VM'ye 20 dakika ssh zaman acısı vardır.

### <a name="powershell-customizer"></a>PowerShell özelleştirici 
Kabuk özelleyici powershell komutları ve satır başı çalışan destekler, komut ib onlara erişmek için kamuya erişilebilir olmalıdır.

```json 
     "customize": [
        { 
             "type": "PowerShell",
             "name":   "<name>",  
             "scriptUri": "<path to script>",
             "runElevated": "<true false>",
             "sha256Checksum": "<sha256 checksum>" 
        },  
        { 
             "type": "PowerShell", 
             "name": "<name>", 
             "inline": "<PowerShell syntax to run>", 
             "validExitCodes": "<exit code>",
             "runElevated": "<true or false>" 
         } 
    ], 
```

İşletim sistemi desteği: Windows ve Linux

Özellikleri özelleştirin:

- **türü** – PowerShell.
- **scriptUri** - URI PowerShell komut dosyası dosyasının konumuna. 
- **inline** – Satır altı komutları çalıştırılacak, virgülle ayrılacak.
- **validExitCodes** – Komut dosyası/satır lı komuttan döndürülebilen isteğe bağlı, geçerli kodlar, bu komut/satır komutunun bildirilen hatasını önler.
- **runElevated** – İsteğe bağlı, boolean, yüksek izinlerle komutları ve komutları çalıştırmak için destek.
- **sha256Checksum** - Dosyanın sha256 checksum değeri, bu yerel oluşturmak ve sonra Image Builder checksum ve doğrulamak.
    * Windows [Get-Hash](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/get-filehash?view=powershell-6) bir PowerShell kullanarak, sha256Checksum oluşturmak için


### <a name="file-customizer"></a>Dosya özelleyici

Dosya özelleştirici, görüntü oluşturucuya github veya Azure depolama sundan dosya indirmeimkanı sağlar. Yapı yapılarına dayanan bir görüntü yapı ardışık hattınız varsa, dosya özelleyicisini yapı paylaşımından karşıdan yükleyip yapı yı görüntüye taşıyabilirsiniz.  

```json
     "customize": [ 
         { 
            "type": "File", 
             "name": "<name>", 
             "sourceUri": "<source location>",
             "destination": "<destination>",
             "sha256Checksum": "<sha256 checksum>"
         }
     ]
```

İşletim sistemi desteği: Linux ve Windows 

Dosya özelleştirici özellikleri:

- **sourceUri** - erişilebilir bir depolama bitiş noktası, bu GitHub veya Azure depolama olabilir. Tüm bir dizin değil, yalnızca bir dosya indirebilirsiniz. Bir dizin indirmeniz gerekiyorsa, sıkıştırılmış bir dosya kullanın ve Shell veya PowerShell özelleştiricilerini kullanarak dosyayı açın. 
- **hedef** – bu tam hedef yolu ve dosya adıdır. Başvurulan herhangi bir yol ve alt dizinler mevcut olmalı, bunları önceden ayarlamak için Shell veya PowerShell özelleştiricilerini kullanmalıdır. Yolu oluşturmak için komut dosyası özelleştiricilerini kullanabilirsiniz. 

Bu, Windows dizinleri ve Linux yolları tarafından desteklenir, ancak bazı farklılıklar vardır: 
- Linux OS ' s - Görüntü oluşturucu yazabilirsiniz tek yol / tmp olduğunu.
- Windows – Yol kısıtlaması yok, ancak yol var olmalıdır.
 
 
Dosyayı karşıdan yüklemeye çalışan veya belirli bir dizine koymaya çalışan bir hata varsa, özelleştirme adımı başarısız olur ve bu işlem.log'da olur.

> [!NOTE]
> Dosya özelleştirici sadece küçük dosya indirme, < 20MB için uygundur. Daha büyük dosya yüklemeleri için bir komut dosyası veya satır satır komutu, Linux `wget` veya, `curl`Windows gibi dosyaları indirmek için kullanım kodu `Invoke-WebRequest`kullanın.

Dosya özellemindeki dosyalar [MSI](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts/7_Creating_Custom_Image_using_MSI_to_Access_Storage)kullanılarak Azure Depolama'dan indirilebilir.

### <a name="windows-update-customizer"></a>Windows Update Özelleştirici
Bu özelleştirici, Packer topluluğu tarafından sürdürülen açık kaynak kodlu bir proje olan Packer için [topluluk Windows Update Provisioner](https://packer.io/docs/provisioners/community-supported.html) üzerine kurulmuştur. Microsoft, Görüntü Oluşturucu hizmetiyle geçici çözümü sınayıp doğrular ve bu hizmetle ilgili sorunları araştırmayı destekler ve sorunları gidermek için çalışır, ancak açık kaynak projesi Microsoft tarafından resmi olarak desteklenmez. Windows Update Provisioner ile ilgili ayrıntılı belgeler ve yardım için lütfen proje deposuna bakın.
 
     "customize": [
            {
                "type": "WindowsUpdate",
                "searchCriteria": "IsInstalled=0",
                "filters": [
                    "exclude:$_.Title -like '*Preview*'",
                    "include:$true"
                            ],
                "updateLimit": 20
            }
               ], 
İşletim sistemi desteği: Windows

Özellikleri özelleştirin:
- **yazın** – WindowsUpdate.
- **searchCriteria** - İsteğe bağlı olarak, hangi güncelleştirme türünün yüklendiği (Önerilen, Önemli vb.), BrowseOnly=0 ve IsInstalled=0 (Önerilen) varsayılan dır.
- **filtreler** – İsteğe bağlı, güncellemeleri eklemek veya hariç tutmak için bir filtre belirtmenize olanak tanır.
- **updateLimit** – İsteğe bağlı olarak, kaç güncelleştirmenin yüklenebileceğini tanımlar, varsayılan 1000.
 
 

### <a name="generalize"></a>Genelleştir 
Varsayılan olarak, Azure Image Builder görüntüyü 'genelleştirmek' için her görüntü özelleştirme aşamasının sonunda 'deprovision' kodu da çalıştıracaktır. Genelleme, görüntünün birden çok VM oluşturmak için yeniden kullanılabileceğini niçin ayarlandığı bir işlemdir. Azure Görüntü Oluşturucu, Windows VM'leri için Sysprep kullanır. Azure Image Builder, Linux için 'waagent -deprovision' çalıştırın. 

Görüntü Oluşturucu kullanıcılarının genelleme yapması komutları her durum için uygun olmayabilir, bu nedenle Azure Image Builder gerekirse bu komutu özelleştirmenize olanak tanır. 

Varolan özelleştirmeyi geçiriyorsanız ve farklı Sysprep/waagent komutları kullanıyorsanız, Görüntü Oluşturucu genel komutlarını kullanabilirsiniz ve VM oluşturma başarısız olursa, kendi Sysprep veya waagent komutlarınızı kullanın.

Azure Image Builder bir Windows özel görüntüsünü başarıyla oluşturursa ve ondan bir VM oluşturursanız, VM oluşturmanın başarısız olduğunu veya başarıyla tamamlanmadığını fark ederseniz, Windows Server Sysprep belgelerini gözden geçirmeniz veya bir destek isteği oluşturmanız gerekir. Windows Server Sysprep Müşteri Hizmetleri Destek ekibi, sorun giderme ve doğru Sysprep kullanımı hakkında tavsiyelerde bulunabilir.


#### <a name="default-sysprep-command"></a>Varsayılan Sysprep komutu
```powershell
echo '>>> Waiting for GA to start ...'
while ((Get-Service RdAgent).Status -ne 'Running') { Start-Sleep -s 5 }
while ((Get-Service WindowsAzureTelemetryService).Status -ne 'Running') { Start-Sleep -s 5 }
while ((Get-Service WindowsAzureGuestAgent).Status -ne 'Running') { Start-Sleep -s 5 }
echo '>>> Sysprepping VM ...'
if( Test-Path $Env:SystemRoot\\windows\\system32\\Sysprep\\unattend.xml ){ rm $Env:SystemRoot\\windows\\system32\\Sysprep\\unattend.xml -Force} & $Env:SystemRoot\\System32\\Sysprep\\Sysprep.exe /oobe /generalize /quiet /quit
while($true) { $imageState = Get-ItemProperty HKLM:\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Setup\\State | Select ImageState; if($imageState.ImageState -ne 'IMAGE_STATE_GENERALIZE_RESEAL_TO_OOBE') { Write-Output $imageState.ImageState; Start-Sleep -s 5  } else { break } }
```
#### <a name="default-linux-deprovision-command"></a>Varsayılan Linux deprovision komutu

```bash
/usr/sbin/waagent -force -deprovision+user && export HISTSIZE=0 && sync
```

#### <a name="overriding-the-commands"></a>Komutları Geçersiz Kılma
Komutları geçersiz kılmak için, tam dosya adı içeren komut dosyalarını oluşturmak için PowerShell veya Shell komut dosyası hükümlerini kullanın ve bunları doğru dizinlere koyun:

* Windows: c:\DeprovisioningScript.ps1
* Linux: /tmp/DeprovisioningScript.sh

Görüntü Oluşturucu bu komutları okuyacak, bu AIB günlükleri, 'özelleştirme.log' yazılır. Günlüklerin nasıl toplandığıyla ilgili [sorun giderme](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#collecting-and-reviewing-aib-logs) sorununa bakın.
 
## <a name="properties-distribute"></a>Özellikler: dağıtmak

Azure Image Builder üç dağıtım hedefini destekler: 

- **managedImage** - yönetilen görüntü.
- **sharedImage** - Paylaşılan Resim Galerisi.
- **VHD** - Bir depolama hesabında VHD.

Aynı yapılandırmada hedef türlerin her ikisine de bir görüntü dağıtabilirsiniz, [lütfen örneklere](https://github.com/danielsollondon/azvmimagebuilder/blob/7f3d8c01eb3bf960d8b6df20ecd5c244988d13b6/armTemplates/azplatform_image_deploy_sigmdi.json#L80)bakın.

Dağıtmak için birden fazla hedefiniz olduğundan, Image Builder' ı sorgulayarak erişilebilen her dağıtım `runOutputName`hedefi için bir durum tutar.  Bu `runOutputName` dağıtım hakkında bilgi için dağıtım sonrası sorgulayabileceğiniz bir nesnedir. Örneğin, VHD'nin konumunu veya görüntü sürümünün çoğaltıldığı bölgeleri veya oluşturulan SIG Image sürümünü sorgulayabilirsiniz. Bu her dağıtım hedefinin bir özelliğidir. Her `runOutputName` dağıtım hedefine özgü olmalıdır. Aşağıda bir örnek, bu paylaşılan resim galerisi dağıtım sorgulayan:

```bash
subscriptionID=<subcriptionID>
imageResourceGroup=<resourceGroup of image template>
runOutputName=<runOutputName>

az resource show \
        --ids "/subscriptions/$subscriptionID/resourcegroups/$imageResourceGroup/providers/Microsoft.VirtualMachineImages/imageTemplates/ImageTemplateLinuxRHEL77/runOutputs/$runOutputName"  \
        --api-version=2019-05-01-preview
```

Çıktı:
```json
{
  "id": "/subscriptions/xxxxxx/resourcegroups/rheltest/providers/Microsoft.VirtualMachineImages/imageTemplates/ImageTemplateLinuxRHEL77/runOutputs/rhel77",
  "identity": null,
  "kind": null,
  "location": null,
  "managedBy": null,
  "name": "rhel77",
  "plan": null,
  "properties": {
    "artifactId": "/subscriptions/xxxxxx/resourceGroups/aibDevOpsImg/providers/Microsoft.Compute/galleries/devOpsSIG/images/rhel/versions/0.24105.52755",
    "provisioningState": "Succeeded"
  },
  "resourceGroup": "rheltest",
  "sku": null,
  "tags": null,
  "type": "Microsoft.VirtualMachineImages/imageTemplates/runOutputs"
}
```

### <a name="distribute-managedimage"></a>Distribute: managedImage

Görüntü çıktısı yönetilen bir görüntü kaynağı olacaktır.

```json
"distribute": [
        {
"type":"managedImage",
       "imageId": "<resource ID>",
       "location": "<region>",
       "runOutputName": "<name>",
       "artifactTags": {
            "<name": "<value>",
             "<name>": "<value>"
               }
         }]
```
 
Özellikleri dağıtın:
- **türü** – managedImage 
- **imageId** – Hedef görüntünün kaynak kimliği, beklenen\<biçim: /subscriptions/\<subscriptionId>/resourceGroups/ destinationResourceGroupName>/providers/Microsoft.Compute/images/imageNamename\<>
- **konum** - yönetilen görüntünün konumu.  
- **runOutputName** – dağıtımı tanımlamak için benzersiz bir ad.  
- **artifactTags** - İsteğe bağlı kullanıcı belirtilen anahtar değer çifti etiketleri.
 
 
> [!NOTE]
> Hedef kaynak grubu olmalıdır.
> Görüntünün farklı bir bölgeye dağıtılmasını istiyorsanız, dağıtım süresini artırır. 

### <a name="distribute-sharedimage"></a>Distribute: sharedImage 
Azure Paylaşılan Resim Galerisi, görüntü bölgesi çoğaltma, sürüm leme ve özel görüntülerin paylaşılmasına olanak tanıyan yeni bir Görüntü Yönetimi hizmetidir. Azure Image Builder bu hizmetle dağıtımı destekler, böylece görüntüleri Paylaşılan Resim Galerileri tarafından desteklenen bölgelere dağıtabilirsiniz. 
 
Paylaşılan Resim Galerisi şu şekildedir: 
 
- Galeri - Birden çok paylaşılan resim için kapsayıcı. Bir bölgede bir galeri dağıtılır.
- Görüntü tanımları - görüntüler için kavramsal bir gruplama. 
- Resim sürümleri - Bu bir VM veya ölçek kümesi dağıtmak için kullanılan bir görüntü türüdür. Görüntü sürümleri, VM'lerin dağıtılması gereken diğer bölgelere çoğaltılabilir.
 
Resim Galerisi'ne dağıtabilmek için önce bir galeri ve resim tanımı oluşturmanız gerekir, [bkz.](shared-images.md) 

```json
{
    "type": "sharedImage",
    "galleryImageId": "<resource ID>",
    "runOutputName": "<name>",
    "artifactTags": {
        "<name>": "<value>",
        "<name>": "<value>"
    },
    "replicationRegions": [
        "<region where the gallery is deployed>",
        "<region>"
    ]
}
``` 

Paylaşılan resim galerileri için özellikleri dağıtın:

- **türü** - sharedImage  
- **galleryImageId** – Paylaşılan resim galerisinin kimliği. Biçimi:\</subscriptions/ subscriptionId\<>/resourceGroups/ resourceGroupName>/providers/Microsoft.Compute/galleries/\<sharedImageGalleryName\<>/images/ imageGalleryName>.
- **runOutputName** – dağıtımı tanımlamak için benzersiz bir ad.  
- **artifactTags** - İsteğe bağlı kullanıcı belirtilen anahtar değer çifti etiketleri.
- **çoğaltmaBölgeler** - Çoğaltma için bölgelerin dizi. Bölgelerden biri Galeri'nin dağıtıldığı bölge olmalıdır.
 
> [!NOTE]
> Azure Image Builder'ı galeriye farklı bir bölgede kullanabilirsiniz, ancak Azure Image Builder hizmetinin görüntüyü veri merkezleri arasında aktarması gerekir ve bu daha uzun sürer. Image Builder, tek renkli bir tamsayıya dayanarak görüntüyü otomatik olarak sürümler, şu anda belirtemezsiniz. 

### <a name="distribute-vhd"></a>Dağıt: VHD  
Bir VHD çıktı yapabilirsiniz. Daha sonra VHD'yi kopyalayabilir ve Azure MarketPlace'de yayımlamak veya Azure Yığını ile kullanmak için kullanabilirsiniz.  

```json
{ 
    "type": "VHD",
    "runOutputName": "<VHD name>",
    "tags": {
        "<name": "<value>",
        "<name>": "<value>"
    }
}
```
 
İşletim Sistemi Desteği: Windows ve Linux

VHD parametrelerini dağıtın:

- **türü** - VHD.
- **runOutputName** – dağıtımı tanımlamak için benzersiz bir ad.  
- **etiketler** - İsteğe bağlı kullanıcı belirtilen anahtar değeri çifti etiketleri.
 
Azure Image Builder, kullanıcının bir depolama hesabı konumu belirtmesine izin `runOutputs` vermez, ancak konumu almak için durumun sorgulanabilir.  

```azurecli-interactive
az resource show \
   --ids "/subscriptions/$subscriptionId/resourcegroups/<imageResourceGroup>/providers/Microsoft.VirtualMachineImages/imageTemplates/<imageTemplateName>/runOutputs/<runOutputName>"  | grep artifactUri 
```

> [!NOTE]
> VHD oluşturulduktan sonra, mümkün olan en kısa sürede farklı bir konuma kopyalayın. VHD, görüntü şablonu Azure Image Builder hizmetine gönderildiğinde oluşturulan geçici kaynak grubunda bir depolama hesabında depolanır. Resim şablonu silerseniz, VHD'yi kaybedersiniz. 
 
## <a name="next-steps"></a>Sonraki adımlar

[Azure Image Builder GitHub'da](https://github.com/danielsollondon/azvmimagebuilder)farklı senaryolar için örnek .json dosyaları vardır.
 
