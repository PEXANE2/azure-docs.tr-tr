---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 07/08/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: d848b92da5d4181832adff8499b3531d020c30c9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78155442"
---
Kısa ömürlü işletim sistemi diskleri yerel sanal makine (VM) depolama alanında oluşturulur ve uzak Azure Depolama'ya kaydedilmez. Kısa ömürlü işletim sistemi diskleri, uygulamaların tek tek VM hatalarına toleranslı olduğu, ancak VM dağıtım süresinden veya tek tek VM örneklerini yeniden görüntülemeden daha fazla etkilendiği durumsuz iş yükleri için iyi çalışır. Geçici işletim sistemi diski ile işletim sistemi diskine daha düşük okuma/yazma gecikmesi ve daha hızlı VM yeniden görüntü elde edeyim. 
 
Geçici disklerin temel özellikleri şunlardır: 
- Stateless uygulamaları için idealdir.
- Hem Marketplace hem de özel görüntülerle kullanılabilirler.
- VM'leri hızlı bir şekilde sıfırlama veya yeniden görüntülenebilme ve örnekleri özgün önyükleme durumuna göre ölçeklendirme yeteneği.  
- Geçici diske benzer şekilde daha düşük gecikme süre. 
- Kısa ömürlü işletim sistemi diskleri ücretsizdir, işletim sistemi diski için depolama maliyeti olmaz.
- Tüm Azure bölgelerinde kullanılabilirler. 
- Kısa ömürlü işletim sistemi diski [Paylaşılan Resim Galerisi](/azure/virtual-machines/linux/shared-image-galleries)tarafından desteklenir. 
 

 
Kalıcı ve kısa ömürlü işletim sistemi diskleri arasındaki temel farklar:

|                             | Kalıcı İşletim Sistemi Diski                          | Kısa Ömürlü İşletim Sistemi Diski                              |    |
|-----------------------------|---------------------------------------------|------------------------------------------------|
| İşletim sistemi diski için boyut sınırı      | 2 TiB                                                                                        | Hangisi daha küçükse, VM boyutu veya 2TiB için önbellek boyutu. **GiB'deki önbellek boyutu**için Bkz. [DS](../articles/virtual-machines/linux/sizes-general.md), [ES](../articles/virtual-machines/linux/sizes-memory.md), [M](../articles/virtual-machines/linux/sizes-memory.md), [FS](../articles/virtual-machines/linux/sizes-compute.md)ve [GS](/azure/virtual-machines/linux/sizes-previous-gen#gs-series)              |
| VM boyutları desteklendi          | Tümü                                                                                          | DSv1, DSv2, DSv3, Esv3, Fs, FsV2, GS, M                                               |
| Disk türü desteği           | Yönetilen ve yönetilmeyen işletim sistemi diski                                                                | Yalnızca yönetilen işletim sistemi diski                                                               |
| Bölge desteği              | Tüm bölgeler                                                                                  | Tüm bölgeler                              |
| Veri kalıcılığı            | İşletim sistemi diskine yazılan işletim sistemi disk verileri Azure Depolama'da depolanır                                  | İşletim sistemi diskine yazılan veriler yerel VM depolamasına depolanır ve Azure Depolama'da kalıcı olarak kullanılmaz. |
| Stop-allocated durumu      | VM'ler ve ölçek kümesi örnekleri durdurulabilir ve ayrılan durum dan yeniden başlatılabilir | VM'ler ve ölçek kümesi örnekleri durdurulamaz                                  |
| Özel işletim sistemi disk desteği | Evet                                                                                          | Hayır                                                                                 |
| İşletim sistemi disk yeniden boyutlandırma              | VM oluşturma sırasında ve VM'den sonra desteklenir                                | Yalnızca VM oluşturma sırasında desteklenen                                                  |
| Yeni bir VM boyutuna yeniden boyutlandırma   | İşletim sistemi disk verileri korunur                                                                    | Os diskindeki veriler silinir, işletim sistemi yeniden                                      |

## <a name="size-requirements"></a>Boyut gereksinimleri

VM ve örnek görüntüleri VM önbelleğine kadar dağıtabilirsiniz. Örneğin, pazardaki Standart Windows Server görüntüleri yaklaşık 127 GiB'dir, bu da 127 GiB'den daha büyük bir önbelleğe sahip bir VM boyutuna ihtiyacınız olduğu anlamına gelir. Bu durumda, [Standard_DS2_v2](~/articles/virtual-machines/dv2-dsv2-series.md) yeterince büyük değil 86 GiB, bir önbellek boyutu vardır. Standard_DS3_v2 172 GiB, yeterince büyük bir önbellek boyutu vardır. Bu durumda, Standard_DS3_v2, DSv2 serisinde bu görüntüyle kullanabileceğiniz en küçük boyutdur. Market ve Windows Server görüntülerinde 30 `[smallsize]` GiB civarında olan temel Linux görüntüleri, mevcut VM boyutlarının çoğunu kullanabilir.

Kısa ömürlü diskler, VM boyutunun Premium depolama alanını desteklemesini de gerektirir. Boyutları genellikle (ama her zaman `s` değil) DSv2 ve EsV3 gibi, adında bir var. Daha fazla bilgi için, hangi boyutların Premium depolama alanını desteklediğiyle ilgili ayrıntılar için [Azure VM boyutlarına](../articles/virtual-machines/linux/sizes.md) bakın.

## <a name="powershell"></a>PowerShell

PowerShell VM dağıtımı için geçici bir disk kullanmak için VM yapılandırmanızda [Set-AzVMOSDisk'i](/powershell/module/az.compute/set-azvmosdisk) kullanın. `-DiffDiskSetting` Ayarlayın `Local` ve `-Caching` . `ReadOnly`     

```powershell
Set-AzVMOSDisk -DiffDiskSetting Local -Caching ReadOnly
```

Ölçek ayarlamadağıtımları için yapılandırmanızda [Set-AzVmssStorageProfile](/powershell/module/az.compute/set-azvmssstorageprofile) cmdlet'i kullanın. `-DiffDiskSetting` Ayarlayın `Local` ve `-Caching` . `ReadOnly`


```powershell
Set-AzVmssStorageProfile -DiffDiskSetting Local -OsDiskCaching ReadOnly
```

## <a name="cli"></a>CLI

CLI VM dağıtımı için geçici bir disk kullanmak `--ephemeral-os-disk` için `true` az [vm'deki](/cli/azure/vm#az-vm-create) `--os-disk-caching` parametreyi `ReadOnly`ve parametreyi .

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image UbuntuLTS \
  --ephemeral-os-disk true \
  --os-disk-caching ReadOnly \
  --admin-username azureuser \
  --generate-ssh-keys
```

Ölçek kümeleri için [az-vmss-oluşturmak](/cli/azure/vmss#az-vmss-create) için aynı `--os-disk-caching` `ReadOnly` `--ephemeral-os-disk true` parametreyi kullanırsınız ve parametreyi .

## <a name="portal"></a>Portal   

Azure portalında, **Diskler** sekmesinin **Gelişmiş** bölümünü açarak VM dağıtırken geçici diskler kullanmayı seçebilirsiniz. **Kullanım için geçici işletim sistemi diski** **evet**seçin.

![Kısa ömürlü bir işletim sistemi diski kullanmayı seçmek için radyo düğmesini gösteren ekran görüntüsü](./media/virtual-machines-common-ephemeral/ephemeral-portal.png)

Kısa ömürlü bir disk kullanma seçeneği soluksa, işletim sistemi görüntüsünden daha büyük önbellek boyutuna sahip olmayan veya Premium depolama alanını desteklemeyen bir VM boyutu seçmiş olabilirsiniz. **Temel bilgiler** sayfasına geri dön ve başka bir VM boyutu seçmeyi deneyin.

Portalı kullanarak geçici işletim sistemi diskleri içeren ölçek kümeleri de oluşturabilirsiniz. Sadece yeterince büyük önbellek boyutuna sahip bir VM boyutu seçtiğinizden emin olun ve sonra kullanım kısa ömürlü **işletim sistemi diski** **Evet**seçin .

![Ölçek kümeniz için geçici bir işletim sistemi diski kullanmayı seçmek için radyo düğmesini gösteren ekran görüntüsü](./media/virtual-machines-common-ephemeral/scale-set.png)

## <a name="scale-set-template-deployment"></a>Ölçek kümesi şablon dağıtımı  
Kısa ömürlü bir işletim sistemi diski kullanan bir ölçek `diffDiskSettings` kümesi oluşturma `Microsoft.Compute/virtualMachineScaleSets/virtualMachineProfile` işlemi, özelliği şablondaki kaynak türüne eklemektir. Ayrıca, önbelleğe alma ilkesi `ReadOnly` geçici işletim sistemi diski için ayarlanmalıdır. 


```json
{ 
  "type": "Microsoft.Compute/virtualMachineScaleSets", 
  "name": "myScaleSet", 
  "location": "East US 2", 
  "apiVersion": "2018-06-01", 
  "sku": { 
    "name": "Standard_DS2_v2", 
    "capacity": "2" 
  }, 
  "properties": { 
    "upgradePolicy": { 
      "mode": "Automatic" 
    }, 
    "virtualMachineProfile": { 
       "storageProfile": { 
        "osDisk": { 
          "diffDiskSettings": { 
                "option": "Local" 
          }, 
          "caching": "ReadOnly", 
          "createOption": "FromImage" 
        }, 
        "imageReference":  { 
          "publisher": "Canonical", 
          "offer": "UbuntuServer", 
          "sku": "16.04-LTS", 
          "version": "latest" 
        } 
      }, 
      "osProfile": { 
        "computerNamePrefix": "myvmss", 
        "adminUsername": "azureuser", 
        "adminPassword": "P@ssw0rd!" 
      } 
    } 
  } 
}  
```

## <a name="vm-template-deployment"></a>VM şablon dağıtımı 
Şablon kullanarak geçici işletim sistemi diski olan bir VM dağıtabilirsiniz. Geçici işletim sistemi diskleri kullanan bir VM oluşturma işlemi, `diffDiskSettings` özelliği şablondaki Microsoft.Compute/virtualMachines kaynak türüne eklemektir. Ayrıca, önbelleğe alma ilkesi `ReadOnly` geçici işletim sistemi diski için ayarlanmalıdır. 

```json
{ 
  "type": "Microsoft.Compute/virtualMachines", 
  "name": "myVirtualMachine", 
  "location": "East US 2", 
  "apiVersion": "2018-06-01", 
  "properties": { 
       "storageProfile": { 
            "osDisk": { 
              "diffDiskSettings": { 
                "option": "Local" 
              }, 
              "caching": "ReadOnly", 
              "createOption": "FromImage" 
            }, 
            "imageReference": { 
                "publisher": "MicrosoftWindowsServer", 
                "offer": "WindowsServer", 
                "sku": "2016-Datacenter-smalldisk", 
                "version": "latest" 
            }, 
            "hardwareProfile": { 
                 "vmSize": "Standard_DS2_v2" 
             } 
      }, 
      "osProfile": { 
        "computerNamePrefix": "myvirtualmachine", 
        "adminUsername": "azureuser", 
        "adminPassword": "P@ssw0rd!" 
      } 
    } 
 } 
```


## <a name="reimage-a-vm-using-rest"></a>REST kullanarak bir VM'yi yeniden görüntüle
Aşağıda açıklandığı gibi REST API'yi kullanarak ve Azure Portalı üzerinden VM'nin Genel Bakış bölmesine giderek geçici işletim sistemi ile sanal makine örneğini yeniden görüntüleyebilirsiniz. Ölçek kümeleri için, yeniden görüntüleme zaten Powershell, CLI ve portal üzerinden kullanılabilir.

```
POST https://management.azure.com/subscriptions/{sub-
id}/resourceGroups/{rgName}/providers/Microsoft.Compute/VirtualMachines/{vmName}/reimage?a pi-version=2018-06-01" 
```
 
## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**S: Yerel işletim sistemi disklerinin boyutu nedir?**

C: Os diskine tüm okuma/yazmaların Sanal Makine ile aynı düğümde yerel olacağı VM önbellek boyutuna kadar platform ve özel görüntüleri destekliyoruz. 

**S: Kısa ömürlü işletim sistemi diski yeniden boyutlandırılabilir mi?**

C: Hayır, kısa ömürlü işletim sistemi diski sağlandıktan sonra işletim sistemi diski yeniden boyutlandırılamaz. 

**S: Yönetilen Diskleri Geçici VM'ye ekleyebilir miyim?**

C: Evet, kısa ömürlü işletim sistemi diski kullanan bir VM'ye yönetilen bir veri diski ekleyebilirsiniz. 

**S: Tüm VM boyutları geçici işletim sistemi diskleri için desteklenecek mi?**

C: Hayır, B serisi, N serisi ve H serisi boyutları dışında tüm Premium Depolama VM boyutları (DS, ES, FS, GS ve M) desteklenir.  
 
**S: Geçici işletim sistemi diski varolan VM'lere ve ölçek kümelerine uygulanabilir mi?**

C: Hayır, geçici işletim sistemi diski yalnızca VM ve ölçek kümesi oluşturma sırasında kullanılabilir. 

**S: Geçici ve normal işletim sistemi disklerini bir ölçek kümesinde karıştırabilir misiniz?**

C: Hayır, aynı ölçek kümesi içinde geçici ve kalıcı işletim sistemi disk örneklerinin bir karışımı olamaz. 

**S: Geçici işletim sistemi diski Powershell veya CLI kullanılarak oluşturulabilir mi?**

C: Evet, REST, Templates, PowerShell ve CLI'yi kullanarak Geçici Işletim Sistemi Diski ile VM'ler oluşturabilirsiniz.

**S: Hangi özellikler geçici işletim sistemi diski ile desteklenmez?**

C: Geçici diskler desteklemez:
- VM görüntülerini yakalama
- Disk anlık görüntüleri 
- Azure Disk Şifrelemesi 
- Azure Backup
- Azure Site Recovery  
- İşletim Sistemi Disk Değişimi 
