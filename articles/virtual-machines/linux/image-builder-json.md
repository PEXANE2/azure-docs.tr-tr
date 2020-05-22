---
title: Azure görüntü Oluşturucu şablonu oluşturma (Önizleme)
description: Azure Image Builder ile kullanmak üzere şablon oluşturmayı öğrenin.
author: danis
ms.author: danis
ms.date: 03/24/2020
ms.topic: article
ms.service: virtual-machines-linux
ms.subservice: imaging
ms.reviewer: cynthn
ms.openlocfilehash: f567114613f484f0765a6e007c3f0ba97480a968
ms.sourcegitcommit: a9784a3fd208f19c8814fe22da9e70fcf1da9c93
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83779341"
---
# <a name="preview-create-an-azure-image-builder-template"></a>Önizleme: Azure görüntü Oluşturucu şablonu oluşturma 

Azure Image Builder, görüntü Oluşturucu hizmetine bilgi geçirmek için bir. JSON dosyası kullanır. Bu makalede, JSON dosyasının bölümlerine gidecektir, bu sayede kendinizinkini oluşturabilirsiniz. Tam. JSON dosyaları örneklerini görmek için bkz. [Azure görüntü Oluşturucu GitHub](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts).

Bu, temel şablon biçimidir:

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



## <a name="type-and-api-version"></a>Tür ve API sürümü

, `type` Olması gereken kaynak türüdür `"Microsoft.VirtualMachineImages/imageTemplates"` . `apiVersion`API değiştiğinde zaman içinde değişir, ancak `"2019-05-01-preview"` Önizleme için olmalıdır.

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
## <a name="vmprofile"></a>vmProfile
Varsayılan olarak, görüntü Oluşturucu bir "Standard_D1_v2" derleme VM 'si kullanır, bunu geçersiz kılabilirsiniz; Örneğin, bir GPU VM 'si için bir görüntüyü özelleştirmek istiyorsanız, bir GPU VM boyutu gerekir. Bu isteğe bağlıdır.

```json
 {
    "vmSize": "Standard_D1_v2"
 },
```

## <a name="osdisksizegb"></a>osDiskSizeGB

Varsayılan olarak, görüntü Oluşturucu görüntünün boyutunu değiştirmez, kaynak görüntüden boyutu kullanır. İşletim sistemi diskinin boyutunu (Win ve Linux) artırabilirsiniz, bu isteğe bağlıdır ve 0 değeri kaynak görüntüyle aynı boyutu bırakır. 

```json
 {
    "osDiskSizeGB": 100
 },
```

## <a name="vnetconfig"></a>vnetConfig
VNET özellikleri belirtmezseniz, görüntü Oluşturucu kendi sanal ağ, genel IP ve NSG 'yi oluşturur. Genel IP, hizmet için derleme VM ile iletişim kurmak üzere kullanılır, ancak genel bir IP istemiyorsanız veya görüntü oluşturucunun yapılandırma sunucuları (DSC, Chef, Pupevcil hayvan, anormal), dosya paylaşımları vb. gibi var olan VNET kaynaklarınıza erişimi olmasını istiyorsanız, VNET belirtebilirsiniz. Daha fazla bilgi için, [ağ belgelerini](https://github.com/danielsollondon/azvmimagebuilder/blob/master/aibNetworking.md#networking-with-azure-vm-image-builder)gözden geçirin, bu isteğe bağlıdır.

```json
    "vnetConfig": {
        "name": "<vnetName>",
        "subnetName": "<subnetName>",
        "resourceGroupName": "<vnetRgName>"
    }
```
## <a name="tags"></a>Etiketler

Bunlar, oluşturulan görüntü için belirtebileceğiniz anahtar/değer çiftleridir.

## <a name="depends-on-optional"></a>Bağımlı (isteğe bağlı)

Bu isteğe bağlı bölüm, devam etmeden önce bağımlılıkların tamamlandığından emin olmak için kullanılabilir. 

```json
    "dependsOn": [],
```

Daha fazla bilgi için bkz. [Kaynak bağımlılıklarını tanımlama](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-define-dependencies#dependson).

## <a name="identity"></a>Kimlik
Varsayılan olarak, görüntü Oluşturucu betikleri kullanmayı veya GitHub ve Azure depolama gibi birden çok konumdan dosya kopyalamayı destekler. Bunları kullanmak için herkese açık bir şekilde erişilebilir olmaları gerekir.

Azure depolama hesabında, kimliğe en az ' Depolama Blobu veri okuyucu ' değeri verildiği sürece, görüntü oluşturucunun Azure Storage 'a erişmesine izin vermek için, sizin tarafınızdan tanımlanan Azure Kullanıcı tarafından atanan yönetilen bir kimliği de kullanabilirsiniz. Bu, depolama bloblarını dışarıdan erişilebilir hale getirmeniz veya SAS belirteçleri oluşturmanız gerekmediği anlamına gelir.


```json
    "identity": {
    "type": "UserAssigned",
          "userAssignedIdentities": {
            "<imgBuilderId>": {}
        }
        },
```

Tam bir örnek için bkz. Azure [depolama 'da dosyalara erişmek Için Azure Kullanıcı tarafından atanan bir yönetilen kimlik kullanma](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts/7_Creating_Custom_Image_using_MSI_to_Access_Storage).

Kullanıcı tarafından atanan bir kimlik için görüntü Oluşturucu desteği: • yalnızca tek bir kimliği destekler • özel etki alanı adlarını desteklemez

Daha fazla bilgi edinmek için bkz. [Azure kaynakları için Yönetilen kimlikler nelerdir?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).
Bu özelliği dağıtma hakkında daha fazla bilgi için bkz. Azure [CLI kullanarak Azure VM 'de Azure kaynakları için yönetilen kimlikleri yapılandırma](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm#user-assigned-managed-identity).

## <a name="properties-source"></a>Özellikler: kaynak

`source`Bölümü, görüntü Oluşturucu tarafından kullanılacak kaynak görüntüyle ilgili bilgiler içerir.

API, görüntü derlemesi için kaynağı tanımlayan bir ' SourceType ' gerektirir; şu anda üç tür vardır:
- Platformımage-kaynak görüntünün Market görüntüsü olduğunu gösterdi.
- Managedımage-normal yönetilen görüntüden itibaren bunu kullanın.
- Parça sürümü-bu, paylaşılan bir görüntü galerisinde kaynak olarak bir görüntü sürümü kullanırken kullanılır.

### <a name="iso-source"></a>ISO kaynağı
Şu anda [kendi abonelik görüntülerini](https://docs.microsoft.com/azure/virtual-machines/workloads/redhat/byos)getiren bu işlevselliği Image Builder 'dan kullanımdan kaldıracağız. lütfen aşağıdaki zaman çizelgelerini gözden geçirin:
    * 31 Mart 2020-RHEL ISO kaynaklarıyla birlikte görüntü şablonları artık kaynak sağlayıcısı tarafından kabul edilecektir.
    * 30 Nisan 2020-RHEL ISO kaynaklarını içeren görüntü şablonları artık işlenmeyecek.

### <a name="platformimage-source"></a>Platformımage kaynağı 
Azure Image Builder, Windows Server ve Client ve Linux Azure Marketi görüntülerini destekler, tam liste için [buraya](https://docs.microsoft.com/azure/virtual-machines/windows/image-builder-overview#os-support) bakın. 

```json
        "source": {
            "type": "PlatformImage",
                "publisher": "Canonical",
                "offer": "UbuntuServer",
                "sku": "18.04-LTS",
                "version": "latest"
        },
```


Buradaki özellikler, VM 'leri oluşturmak için kullanılan, AZ CLı kullanarak, özellikleri almak için aşağıdaki adımları çalıştırmıştır. 
 
```azurecli-interactive
az vm image list -l westus -f UbuntuServer -p Canonical --output table –-all 
```

Sürümünde ' en son ' seçeneğini kullanabilirsiniz. sürüm, şablon gönderildiğinde değil, görüntü oluşturma işlemi gerçekleştiğinde değerlendirilir. Bu işlevi paylaşılan görüntü Galerisi hedefi ile birlikte kullanıyorsanız, şablonu yeniden göndermeyi ve görüntü oluşturmayı yeniden çalıştırmayı önleyebilirsiniz, böylece görüntüleriniz en son görüntülerden yeniden oluşturulur.

### <a name="managedimage-source"></a>Managedımage kaynağı

Kaynak görüntüyü genelleştirilmiş bir VHD veya VM 'nin mevcut bir yönetilen görüntüsü olarak ayarlar. Kaynak yönetilen görüntü desteklenen bir işletim sistemi olmalıdır ve Azure Image Builder şablonunuz ile aynı bölgede olmalıdır. 

```json
        "source": { 
            "type": "ManagedImage", 
                "imageId": "/subscriptions/<subscriptionId>/resourceGroups/{destinationResourceGroupName}/providers/Microsoft.Compute/images/<imageName>"
        }
```

`imageId`Yönetilen görüntünün RESOURCEID olmalıdır. `az image list`Kullanılabilir görüntüleri listelemek için kullanın.


### <a name="sharedimageversion-source"></a>Parça sürümü kaynağı
Kaynak görüntüyü paylaşılan görüntü galerisinde var olan bir görüntü sürümü olarak ayarlar. Görüntü sürümü desteklenen bir işletim sistemi olmalıdır ve görüntünün Azure Image Builder şablonunuz ile aynı bölgeye çoğaltılması gerekir. 

```json
        "source": { 
            "type": "SharedImageVersion", 
            "imageVersionID": "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/p  roviders/Microsoft.Compute/galleries/<sharedImageGalleryName>/images/<imageDefinitionName/versions/<imageVersion>" 
   } 
```

`imageVersionId`Görüntü sürümünün RESOURCEID olmalıdır. Resim sürümlerini listelemek için [az Sig Image-Version List](/cli/azure/sig/image-version#az-sig-image-version-list) kullanın.

## <a name="properties-buildtimeoutinminutes"></a>Özellikler: Buildtimeoutınminutes

Varsayılan olarak, görüntü Oluşturucu 240 dakika boyunca çalışır. Bundan sonra, görüntü yapısının tamamlanıp tamamlanmayacağı, zaman aşımına uğrayacaktır ve durdurulur. Zaman aşımı isabet alıyorsa şuna benzer bir hata görürsünüz:

```text
[ERROR] Failed while waiting for packerizer: Timeout waiting for microservice to
[ERROR] complete: 'context deadline exceeded'
```

Buildtimeoutınminutes değeri belirtmez veya 0 olarak ayarlarsanız, bu varsayılan değeri kullanır. Değeri en fazla 960dakika (16sa) kadar artırabilir veya azaltabilirsiniz. Windows için, bu ayarı 60 dakika içinde ayarlamayı önermiyoruz. Zaman aşımına uğramış olduğunu fark ederseniz, özelleştirme adımının Kullanıcı girişi gibi bir şeyi beklediğini görmek için [günlükleri](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#collecting-and-reviewing-aib-image-build-logs)gözden geçirin. 

Özelleştirmelerin tamamlaması için daha fazla zaman gerektiğini fark ederseniz, bunu, ihtiyacınız olan ve küçük bir ek yük ile istediğiniz şekilde ayarlayın. Ancak bir hata görüntülemeden önce zaman aşımına uğramamasını beklemeniz gerekebilecek için çok yüksek bir ayarlama yapın. 


## <a name="properties-customize"></a>Özellikler: özelleştirme

Görüntü Oluşturucu çoklu ' özelleştiriciler ' destekler. Özelleştiriciler, resminizi özelleştirmek için kullanılan işlevlerdir (örneğin, betikleri çalıştırma veya sunucuları yeniden başlatma). 

Kullanırken `customize` : 
- Birden çok özelleştiriciler kullanabilirsiniz, ancak benzersiz olmalıdır `name` .
- Özelleştiriciler, şablonda belirtilen sırada yürütülür.
- Bir Özelleştirici başarısız olursa, tüm özelleştirme bileşeni başarısız olur ve bir hatayı geri bildirir.
- Bir şablonda kullanmadan önce betiği iyice sınamanız önemle tavsiye edilir. Kendi sanal makinenizde betik hata ayıklaması daha kolay olacaktır.
- Gizli verileri betiklerine yerleştirmeyin. 
- [MSI](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts/7_Creating_Custom_Image_using_MSI_to_Access_Storage)kullanmadığınız durumlar dışında, betik konumlarının herkese açık bir şekilde erişilebilir olması gerekir.

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

 
Customize bölümü bir dizidir. Azure Image Builder, özelleştiriciler aracılığıyla sıralı sırayla çalışacaktır. Herhangi bir Özelleştirici içindeki herhangi bir hata derleme işlemini başarısız olur. 
 
 
### <a name="shell-customizer"></a>Kabuk Özelleştirici

Kabuk Özelleştirici, kabuk betikleri çalıştırmayı destekler, bu, ıB 'nin bunlara erişebilmesi için herkese açık bir şekilde erişilebilir olması gerekir.

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

İşletim sistemi desteği: Linux 
 
Özellikleri Özelleştir:

- **tür** – Shell 
- **ad** -özelleştirmeyi izlemek için ad 
- **Scripturi** -URI, dosyanın konumuna 
- noktalı virgülle ayrılmış kabuk komutlarının **satır içi** dizisi.
- **sha256Checksum** -dosyanın SHA256 sağlama toplamı değeri, bu yerel olarak oluşturulur ve ardından görüntü Oluşturucu sağlama toplamı ve doğrular.
    * Mac/Linux çalıştıran bir Terminal kullanarak sha256Checksum oluşturmak için:`sha256sum <fileName>`


Komutların süper kullanıcı ayrıcalıklarıyla çalışması için, ön ekine sahip olmaları gerekir `sudo` .

> [!NOTE]
> RHEL ISO kaynağı ile kabuk Özelleştirici çalıştırırken, özelleştirme gerçekleşmeden önce, ilk özelleştirme kabuğunuzun bir Red Hat yetkilendirme sunucusuyla kaydolduğunu güvence altına almanız gerekir. Özelleştirme tamamlandıktan sonra betiğin, yetkilendirme sunucusuyla kaydı yapılmalıdır.

### <a name="windows-restart-customizer"></a>Windows yeniden başlatma Özelleştirici 
Yeniden başlatma Özelleştirici, bir Windows sanal makinesini yeniden başlatmanızı ve yeniden çevrimiçi hale gelmesini bekleyebilir, bu sayede yeniden başlatma gerektiren yazılımları yükleyebilirsiniz.  

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

İşletim sistemi desteği: Windows
 
Özellikleri Özelleştir:
- **Yazın**: windowsrestart
- yeniden başlatma işlemini yürütmek için **restartCommand** komutu (isteğe bağlı). Varsayılan değer: `'shutdown /r /f /t 0 /c \"packer restart\"'`.
- **restartCheckCommand** – yeniden başlatmanın başarılı olup olmadığını denetlemek için komut (isteğe bağlı). 
- **restartTimeout** -yeniden başlatma zaman aşımı, bir büyüklük ve birim dizesi olarak belirtildi. Örneğin, `5m` (5 dakika) veya `2h` (2 saat). Varsayılan değer: ' 5dk '

### <a name="linux-restart"></a>Linux yeniden başlatma  
Linux yeniden başlatma Özelleştirici yoktur, ancak sürücüleri veya yeniden başlatma gerektiren bileşenleri yüklüyorsanız, yükleyebilir ve kabuk Özelleştirici kullanarak bir yeniden başlatma başlatabilirsiniz, derleme VM 'sine yönelik 20 dakikalık bir SSH zaman aşımı vardır.

### <a name="powershell-customizer"></a>PowerShell Özelleştirici 
Kabuk Özelleştirici PowerShell betikleri ve satır içi komutunu çalıştırmayı destekler, ıB 'nin bunlara erişebilmesi için betiklerin herkese açık bir şekilde erişilebilir olması gerekir.

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

Özellikleri Özelleştir:

- **tür** – PowerShell.
- **Scripturi** -URI, PowerShell betik dosyasının konumuna. 
- **satır içi** – çalıştırılacak olan satır içi komutlar, virgülle ayrılır.
- **Validexitcodes** : isteğe bağlı, betiğe/satır içi komuttan döndürülebilecek geçerli kodlar, bu, betik/satır içi komutunun bildirilen başarısızlığından kaçınacaktır.
- **runyükseltici** – isteğe bağlı, Boole, yükseltilmiş izinlerle komutları ve betikleri çalıştırmaya yönelik destek.
- **sha256Checksum** -dosyanın SHA256 sağlama toplamı değeri, bu yerel olarak oluşturulur ve ardından görüntü Oluşturucu sağlama toplamı ve doğrular.
    * Windows [Get-Hash](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/get-filehash?view=powershell-6) üzerinde bir PowerShell kullanarak sha256Checksum oluşturmak için


### <a name="file-customizer"></a>Dosya Özelleştirici

Dosya Özelleştirici, görüntü oluşturucunun bir GitHub veya Azure depolamadan dosya indirmesini sağlar. Yapı yapıtlarına dayanan bir görüntü derleme işlem hattına sahipseniz, derleme paylaşımından dosya Özelleştirici 'yi indirmek için ayarlayabilir ve yapıtları görüntüye taşıyabilirsiniz.  

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

Dosya Özelleştirici özellikleri:

- **sourceURI** -erişilebilir bir depolama uç noktası, GitHub veya Azure depolama olabilir. Bir dizinin tamamını değil yalnızca bir dosyayı indirebilirsiniz. Bir dizin indirmeniz gerekiyorsa, sıkıştırılmış bir dosya kullanın, ardından Shell veya PowerShell özelleştiricileri kullanarak bu dosyayı açın. 
- **hedef** : Bu, tam hedef yolu ve dosya adıdır. Başvurulan tüm yol ve alt dizinler var olmalıdır, bunu önceden ayarlamak için Shell veya PowerShell özelleştiricileri kullanın. Yolu oluşturmak için özelleştiriciler ' i kullanabilirsiniz. 

Bu, Windows dizinleri ve Linux yolları tarafından desteklenir, ancak bazı farklılıklar vardır: 
- Linux işletim sistemi – tek yol Image Builder,/t MP.
- Windows – yol kısıtlaması yok, ancak yol var olmalıdır.
 
 
Dosyayı indirmeye çalışırken bir hata varsa veya belirtilen bir dizine yerleştirirseniz, özelleştirme adımı başarısız olur ve bu, özelleştirme. log ' da olur.

> [!NOTE]
> Dosya Özelleştirici yalnızca küçük dosya indirmeleri için uygundur < 20 MB. Daha büyük dosya indirmeleri için bir betik veya satır içi komutu, Linux veya Windows gibi dosyaları indirmek için kullanılan kodu kullanın `wget` `curl` `Invoke-WebRequest` .

Dosya Özelleştirici içindeki dosyalar, [MSI](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts/7_Creating_Custom_Image_using_MSI_to_Access_Storage)kullanılarak Azure Storage 'dan indirilebilir.

### <a name="windows-update-customizer"></a>Windows Update Özelleştirici
Bu Özelleştirici, Packer topluluğu tarafından tutulan açık kaynaklı bir proje olan Packer için [topluluk Windows Update hazırlayıcı](https://packer.io/docs/provisioners/community-supported.html) üzerine kurulmuştur. Microsoft, görüntü Oluşturucu hizmeti ile birlikte hizmeti sınar ve doğrular ve bunlarla ilgili sorunları araştırarak sorunları gidermeye çalışır, ancak açık kaynak proje Microsoft tarafından resmi olarak desteklenmez. Hakkında ayrıntılı belgeler ve Windows Update hazırlayıcı ile ilgili yardım için lütfen proje deposuna bakın.
 
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

Özellikleri Özelleştir:
- **şunu yazın** : windowsupdate.
- **SearchCriteria** -isteğe bağlı, hangi tür güncelleştirmelerin yüklü olduğunu tanımlar (önerilen, önemli vs.), BrowseOnly = 0 ve IsInstalled = 0 (önerilir) varsayılandır.
- **Filtreler** : isteğe bağlı, güncelleştirmeleri dahil etmek veya hariç tutmak için bir filtre belirtmenize izin verir.
- **Updatelimit** : isteğe bağlı, kaç güncelleştirme yüklenebileceğini tanımlar, varsayılan 1000.
 
 

### <a name="generalize"></a>Genelleştir 
Varsayılan olarak, Azure Image Builder, görüntüyü ' genelleştirmek ' için her görüntü özelleştirme aşamasının sonunda ' deprovision ' kodunu da çalıştıracak. Genelleştirmek, görüntünün ayarlandığı bir işlemdir, bu sayede birden fazla VM oluşturmak için yeniden kullanılabilir. Windows VM 'Leri için Azure Image Builder, Sysprep kullanır. Linux için Azure görüntü Oluşturucu ' waagent-deprovision ' çalıştırır. 

Genelleştirilecek komutlar Image Builder kullanıcıları her durum için uygun olmayabilir, bu nedenle Azure Image Builder gerekirse bu komutu özelleştirmenize olanak tanır. 

Var olan özelleştirmeyi geçiriyorsanız ve farklı Sysprep/waagent komutları kullanıyorsanız, görüntü Oluşturucu genel komutlarını kullanabilir ve VM oluşturma işlemi başarısız olursa, kendi Sysprep veya waagent komutlarınızı kullanın.

Azure Image Builder başarıyla bir Windows özel görüntüsü oluşturursa ve bundan bir VM oluşturursanız, VM oluşturma işleminin başarısız olduğunu veya başarılı bir şekilde tamamlanmayacağını bulun, Windows Server Sysprep belgelerini gözden geçirmeniz veya Windows Server Sysprep müşteri hizmetleri destek ekibi ile bir destek isteği oluşturarak doğru Sysprep kullanımını giderebilen ve önermeniz gerekir.


#### <a name="default-sysprep-command"></a>Varsayılan Sysprep komutu
```powershell
Write-Output '>>> Waiting for GA Service (RdAgent) to start ...'
while ((Get-Service RdAgent).Status -ne 'Running') { Start-Sleep -s 5 }
Write-Output '>>> Waiting for GA Service (WindowsAzureTelemetryService) to start ...'
while ((Get-Service WindowsAzureTelemetryService) -and ((Get-Service WindowsAzureTelemetryService).Status -ne 'Running')) { Start-Sleep -s 5 }
Write-Output '>>> Waiting for GA Service (WindowsAzureGuestAgent) to start ...'
while ((Get-Service WindowsAzureGuestAgent).Status -ne 'Running') { Start-Sleep -s 5 }
Write-Output '>>> Sysprepping VM ...'
if( Test-Path $Env:SystemRoot\system32\Sysprep\unattend.xml ) {
  Remove-Item $Env:SystemRoot\system32\Sysprep\unattend.xml -Force
}
& $Env:SystemRoot\System32\Sysprep\Sysprep.exe /oobe /generalize /quiet /quit
while($true) {
  $imageState = (Get-ItemProperty HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Setup\State).ImageState
  Write-Output $imageState
  if ($imageState -eq 'IMAGE_STATE_GENERALIZE_RESEAL_TO_OOBE') { break }
  Start-Sleep -s 5
}
Write-Output '>>> Sysprep complete ...'
```
#### <a name="default-linux-deprovision-command"></a>Varsayılan Linux sağlamayı kaldırma komutu

```bash
/usr/sbin/waagent -force -deprovision+user && export HISTSIZE=0 && sync
```

#### <a name="overriding-the-commands"></a>Komutları geçersiz kılma
Komutları geçersiz kılmak için PowerShell veya Shell betik hazırlayıcılar ' ni kullanarak tam dosya adına sahip komut dosyalarını oluşturun ve bunları doğru dizinlere yerleştirin:

* Windows: C:\deprovisioningscript.exe. ps1
* Linux:/tmp/deprovisioningscript.exe

Görüntü Oluşturucu bu komutları okuyacak, bunlar ' özelleştirme. log ' AıB günlüklerine yazılır. Bkz. günlüklerin nasıl toplanacağı hakkında [sorun giderme](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#collecting-and-reviewing-aib-logs) .
 
## <a name="properties-distribute"></a>Özellikler: dağıt

Azure görüntü Oluşturucu üç dağıtım hedefini destekler: 

- **Managedımage** tarafından yönetilen görüntü.
- **Sharedimage** -paylaşılan görüntü Galerisi.
- Depolama hesabında **VHD** VHD.

Aynı yapılandırmadaki her iki hedef türüne bir görüntü dağıtabilirsiniz, lütfen [örneklere](https://github.com/danielsollondon/azvmimagebuilder/blob/7f3d8c01eb3bf960d8b6df20ecd5c244988d13b6/armTemplates/azplatform_image_deploy_sigmdi.json#L80)bakın.

' Ye dağıtım yapmak için birden fazla hedefe sahip olabilirsiniz, görüntü Oluşturucu, sorgulanarak erişilebilen her dağıtım hedefi için bir durum tutar `runOutputName` .  , `runOutputName` Bu dağıtım hakkında bilgi için gönderi dağıtımını sorgulayabilir. Örneğin, VHD konumunu veya görüntü sürümünün çoğaltılacağı bölgeleri veya SıG görüntü sürümü oluşturulmasını sorgulayabilirsiniz. Bu, her dağıtım hedefinin bir özelliğidir. `runOutputName`Her dağıtım hedefi için benzersiz olmalıdır. İşte bu, paylaşılan görüntü Galerisi dağıtımını sorgulmıştır:

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

### <a name="distribute-managedimage"></a>Dağıt: Managedımage

Görüntü çıkışı yönetilen bir görüntü kaynağı olacaktır.

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
 
Özellikleri dağıtma:
- **tür** – managedımage 
- **ImageID** – hedef görüntünün kaynak kimliği; beklenen biçim:/subscriptions/ \< SubscriptionID>/ResourceGroups/ \< destinationresourcegroupname>/Providers/Microsoft.COMPUTE/images/ \< ImageName>
- **konum** -yönetilen görüntünün konumu.  
- **Runoutputname** : dağıtımı tanımlamak için benzersiz ad.  
- **Artifacttags** -isteğe bağlı kullanıcı tarafından belirtilen anahtar değer çifti etiketleri.
 
 
> [!NOTE]
> Hedef kaynak grubu mevcut olmalıdır.
> Görüntünün farklı bir bölgeye dağıtılmasını istiyorsanız dağıtım süresini arttıracaktır. 

### <a name="distribute-sharedimage"></a>Dağıt: parça 
Azure Paylaşılan görüntü Galerisi, görüntü bölgesi çoğaltmasının yönetilmesine izin veren, özel görüntülerin sürümü oluşturma ve paylaşımı sağlayan yeni bir görüntü yönetim hizmetidir. Azure görüntü Oluşturucu bu hizmetle dağıtmayı destekler, böylece paylaşılan görüntü galerilerine göre desteklenen bölgelere görüntü dağıtabilirsiniz. 
 
Paylaşılan görüntü Galerisi şu şekilde yapılır: 
 
- Galeri-birden çok paylaşılan görüntü kapsayıcısı. Bir galeri tek bir bölgede dağıtılır.
- Görüntü tanımları-görüntüler için kavramsal gruplama. 
- Görüntü sürümleri-bu, VM veya ölçek kümesi dağıtmak için kullanılan bir görüntü türüdür. Görüntü sürümleri, VM 'Lerin dağıtılması gereken diğer bölgelere çoğaltılabilir.
 
Görüntü galerisine dağıtabilmeniz için önce bir galeri ve görüntü tanımı oluşturmanız gerekir, [paylaşılan görüntüler](shared-images.md)' e bakın. 

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

Paylaşılan görüntü galerilerine yönelik özellikleri dağıtma:

- **tür** -sharedimage  
- **Gallerımageıd** : paylaşılan görüntü galerisinin kimliği. Biçim:/Subscriptions/ \< subscriptionıd>/resourceGroups/ \< resourcegroupname>/Providers/Microsoft.COMPUTE/Galleries/ \< sharedimagegallername>/Images/ \< ımagegallername>.
- **Runoutputname** : dağıtımı tanımlamak için benzersiz ad.  
- **Artifacttags** -isteğe bağlı kullanıcı tarafından belirtilen anahtar değer çifti etiketleri.
- **replicationregion** -çoğaltma için bölgelerin dizisi. Bölgelerden biri, galerinin dağıtıldığı bölge olmalıdır.
 
> [!NOTE]
> Azure Image Builder 'ı galeriye farklı bir bölgede kullanabilirsiniz, ancak Azure Image Builder hizmetinin görüntüyü veri merkezleri arasında aktarması gerekir ve bu daha uzun sürer. Image Builder, bir monoton tamsayıya göre otomatik olarak görüntü sürümüne sahip olur, bunu şu anda belirtemezsiniz. 

### <a name="distribute-vhd"></a>Dağıtma: VHD  
Bir VHD 'ye çıkış yapabilirsiniz. Daha sonra VHD 'yi kopyalayabilir ve Azure Market 'Te yayımlamak için kullanabilir veya Azure Stack kullanabilirsiniz.  

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
 
İşletim sistemi desteği: Windows ve Linux

VHD parametrelerini dağıt:

- **tür** -VHD.
- **Runoutputname** : dağıtımı tanımlamak için benzersiz ad.  
- **Etiketler** -isteğe bağlı kullanıcı tarafından belirtilen anahtar değer çifti etiketleri.
 
Azure görüntü Oluşturucu, kullanıcının bir depolama hesabı konumu belirtilmesine izin vermez, ancak `runOutputs` konumunu almak için durumunu sorgulayabilirsiniz.  

```azurecli-interactive
az resource show \
   --ids "/subscriptions/$subscriptionId/resourcegroups/<imageResourceGroup>/providers/Microsoft.VirtualMachineImages/imageTemplates/<imageTemplateName>/runOutputs/<runOutputName>"  | grep artifactUri 
```

> [!NOTE]
> VHD oluşturulduktan sonra, bunu farklı bir konuma (mümkün olan en kısa sürede) kopyalayın. VHD, görüntü şablonu Azure görüntü Oluşturucu hizmetine gönderildiğinde oluşturulan geçici kaynak grubundaki bir depolama hesabında depolanır. Görüntü şablonunu silerseniz, VHD 'yi kaybedersiniz. 
 
## <a name="next-steps"></a>Sonraki adımlar

[Azure görüntü Oluşturucu GitHub](https://github.com/danielsollondon/azvmimagebuilder)'da farklı senaryolar için Sample. JSON dosyaları vardır.
 
