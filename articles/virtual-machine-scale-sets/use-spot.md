---
title: Azure spot VM 'Leri kullanan bir ölçek kümesi oluşturma (Önizleme)
description: Maliyetlerden tasarruf etmek için spot VM 'Ler kullanan Azure sanal makine ölçek kümeleri oluşturmayı öğrenin.
services: virtual-machine-scale-sets
author: cynthn
manager: gwallace
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.topic: article
ms.date: 10/23/2019
ms.author: cynthn
ms.openlocfilehash: 68315b1b0d290b107fe2d28a9e3b49be009b78b8
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74782248"
---
# <a name="preview-azure-spot-vms-for-virtual-machine-scale-sets"></a>Önizleme: sanal makine ölçek kümeleri için Azure spot VM 'Ler 

Ölçek kümelerinde Azure spot kullanımı, önemli bir maliyet tasarruflarından kullanılmamış kapasitemizin avantajlarından yararlanmanızı sağlar. Azure 'un kapasiteyi her zaman yapması gerektiğinde, Azure altyapısı spot örnekleri çıkarır. Bu nedenle, toplu işleme işleri, geliştirme/test ortamları, büyük işlem iş yükleri ve daha fazlası gibi kesintileri işleyebilen iş yükleri için spot örnekler idealdir.

Kullanılabilir kapasite miktarı boyut, bölge, günün saati ve daha fazlası temelinde farklılık gösterebilir. Ölçek kümelerine spot örnekleri dağıttığınızda Azure, örneği yalnızca kullanılabilir kapasite varsa ayırır, ancak bu örnekler için SLA yoktur. Bir spot ölçek kümesi, tek bir hata etki alanında dağıtılır ve yüksek oranda kullanılabilirlik garantisi sunar.

> [!IMPORTANT]
> Nokta örnekleri şu anda genel önizlemededir.
> Bu önizleme sürümü üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>
> Genel önizlemenin erken bölümü için, spot örnekleri sabit bir fiyata sahip olur, bu nedenle fiyat tabanlı çıkarmalar olmayacaktır.

## <a name="pricing"></a>Fiyatlandırma

Spot örnekleri için fiyatlandırma, bölgeye ve SKU 'ya göre değişkendir. Daha fazla bilgi için bkz. [Linux](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/) ve [Windows](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/windows/)fiyatlandırması. 


Değişken fiyatlandırmayla, en fazla 5 ondalık basamak kullanarak ABD Doları (USD) cinsinden maksimum fiyat ayarlama seçeneğiniz vardır. Örneğin değer `0.98765`, saat başına $0,98765 ABD Doları olan en yüksek fiyat olacaktır. En büyük fiyatı `-1`olarak ayarlarsanız, örnek fiyata göre çıkarılmaz. Örneğin fiyatı, kapasite ve kota kullanılabilir olduğu sürece daha az olan bir standart örnek için geçerli fiyat veya fiyat fiyatı olacaktır.

## <a name="eviction-policy"></a>Çıkarma ilkesi

Spot ölçek kümeleri oluştururken, çıkarma ilkesini *serbest bırakma* (varsayılan) veya *silme*işlemleri için ayarlayabilirsiniz. 

*Serbest bırakma* ilkesi, çıkarılan örnekleri yeniden dağıtmanıza izin vererek, çıkarılan örneklerinizi durdurulmuş serbest bırakılmış duruma kaydırır. Ancak, ayırmanın başarılı olacağını garanti etmez. Serbest bırakılmış VM 'Ler, ölçek kümesi örneği kotanıza göre sayılır ve temel disklerinizin ücreti alınır. 

Çıkarma kümesini, çıkarıldıklarında silinmek üzere bir yere isterseniz, çıkarma ilkesini *silme*olarak ayarlayabilirsiniz. Çıkarma ilkesi silinmek üzere ayarlandığında, ölçek kümesi örnek sayısı özelliğini artırarak yeni VM 'Ler oluşturabilirsiniz. Çıkarılan VM 'Ler, temel disklerle birlikte silinir ve bu nedenle depolama alanı için ücretlendirilmeyecektir. Ayrıca, çıkarılan VM 'Leri otomatik olarak denemek ve dengelemek için ölçek kümelerinin otomatik ölçeklendirme özelliğini de kullanabilirsiniz; ancak, ayırmanın başarılı olacağını garanti etmez. Disk maliyetinden ve kota limitlerine ulaşmaktan kaçınmak için çıkarma ilkesini silme olarak belirlediğinizde, yalnızca spot ölçek kümelerinde otomatik ölçeklendirme özelliğini kullanmanız önerilir. 

Kullanıcılar [Azure zamanlanan olaylar](../virtual-machines/linux/scheduled-events.md)aracılığıyla VM içi bildirimler almayı kabul edebilir. Bu, VM 'leriniz çıkarıldıktan sonra herhangi bir işi tamamlamak ve çıkarma öncesi görevleri gerçekleştirmek için 30 saniyelik bir işlem yapmanız durumunda size bildirir. 


## <a name="deploying-spot-vms-in-scale-sets"></a>Ölçek kümelerinde spot VM 'Leri dağıtma

Ölçek kümeleri üzerinde spot VM 'Leri dağıtmak için yeni *Öncelik* bayrağını *nokta*olarak ayarlayabilirsiniz. Ölçek kümesindeki tüm sanal makineler, spot olarak ayarlanacak. Spot VM 'Ler içeren bir ölçek kümesi oluşturmak için aşağıdaki yöntemlerden birini kullanın:
- [Azure portalda](#portal)
- [Azure CLI](#azure-cli)
- [Azure PowerShell](#powershell)
- [Azure Resource Manager şablonları](#resource-manager-templates)

## <a name="portal"></a>Portal

Spot VM 'Ler kullanan bir ölçek kümesi oluşturma işlemi Başlarken [makalesinde](quick-create-portal.md)ayrıntılıdır. Bir ölçek kümesi dağıttığınızda, nokta bayrağını ve çıkarma ilkesini ayarlamayı seçebilirsiniz: ![spot VM 'lerle ölçek kümesi oluşturma](media/virtual-machine-scale-sets-use-spot/vmss-spot-portal-max-price.png)


## <a name="azure-cli"></a>Azure CLI

Spot VM 'Ler içeren bir ölçek kümesi oluşturma işlemi Başlarken [makalesinde](quick-create-cli.md)ayrıntılıdır. Yalnızca '--Priority noktası ' ekleyin ve `--max-price`ekleyin. Bu örnekte, örneğin fiyata göre çıkarılmaması için `--max-price` `-1` kullanırız.

```azurecli
az vmss create \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --admin-username azureuser \
    --generate-ssh-keys \
    --priority Spot \
    --max-price -1 
```

## <a name="powershell"></a>PowerShell

Spot VM 'Ler içeren bir ölçek kümesi oluşturma işlemi Başlarken [makalesinde](quick-create-powershell.md)ayrıntılıdır.
'-Priority noktası ' eklemeniz yeterlidir ve [New-AzVmssConfig](/powershell/module/az.compute/new-azvmssconfig)' e bir `-max-price` sağlayın.

```powershell
$vmssConfig = New-AzVmssConfig `
    -Location "East US 2" `
    -SkuCapacity 2 `
    -SkuName "Standard_DS2" `
    -UpgradePolicyMode Automatic `
    -Priority "Spot" `
    --max-price -1
```

## <a name="resource-manager-templates"></a>Resource Manager şablonları

Spot VM 'Ler kullanan bir ölçek kümesi oluşturma işlemi, [Linux](quick-create-template-linux.md) veya [Windows](quick-create-template-windows.md)için Başlarken makalesinde ayrıntılı olarak aynıdır. Şablonunuzda *Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachineProfile* kaynak türüne ' Priority ' özelliğini ekleyin ve değer olarak *spot* değerini belirtin. *2019-03-01* API sürümünü veya üstünü kullandığınızdan emin olun. 

Çıkarma ilkesini silmeye ayarlamak için, ' Çıkarlationpolicy ' parametresini ekleyin ve *Silinecek*şekilde ayarlayın.

Aşağıdaki örnek, *Orta Batı ABD*'de *MyScaleSet* adlı bir Linux spot ölçek kümesi oluşturur ve bu durum, çıkarma sırasında ölçek kümesindeki VM 'leri *siler* :

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "myScaleSet",
  "location": "East US 2",
  "apiVersion": "2019-03-01",
  "sku": {
    "name": "Standard_DS2_v2",
    "capacity": "2"
  },
  "properties": {
    "upgradePolicy": {
      "mode": "Automatic"
    },
    "virtualMachineProfile": {
       "priority": "Spot",
       "evictionPolicy": "delete",
       "storageProfile": {
        "osDisk": {
          "caching": "ReadWrite",
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
## <a name="faq"></a>SSS

**S:** Oluşturulduktan sonra, standart örnekle aynı bir spot örneğidir mi?

Y **:** Evet, spot VM 'Ler için SLA olmaması ve herhangi bir zamanda çıkartılanlar haricinde.


**S:** Ne yapmalı, ancak yine de kapasiteye ihtiyaç duydunuz?

Y **:** Kapasiteye ihtiyacınız varsa, nokta VM 'Leri yerine standart VM 'Ler kullanmanızı öneririz.


**S:** Kota, Spot için nasıl yönetilir?

Y **:** Spot örnekleri ve standart örneklerin ayrı kota havuzları olacaktır. Spot kota, VM 'Ler ve ölçek kümesi örnekleri arasında paylaşılır. Daha fazla bilgi için bkz. [Azure aboneliği ile hizmet limitleri, kotalar ve kısıtlamalar](https://docs.microsoft.com/azure/azure-subscription-service-limits).


**S:** Nokta için ek kota isteyebilir miyim?

Y **:** Evet, [Standart kota isteği işlemi](https://docs.microsoft.com/azure/azure-supportability/per-vm-quota-requests)aracılığıyla spot VM 'lerle ilgili kotayı artırmak için isteği gönderebileceksiniz.


**S:** Varolan ölçek kümelerini spot ölçek kümelerine dönüştürebilir miyim?

Y **:** Hayır, `Spot` bayrağını ayarlamak yalnızca oluşturma sırasında desteklenir.


**S:** Düşük öncelikli ölçek kümeleri için `low` kullanıyorum, bunun yerine `Spot` kullanmaya başlamam gerekir mi?

Y **:** Şimdilik `low` ve `Spot` her ikisi de çalışır, ancak `Spot`kullanarak geçiş yapmanız gerekir.


**S:** Hem normal VM 'Ler hem de spot VM 'Ler içeren bir ölçek kümesi oluşturabilir miyim?

Y **:** Hayır, ölçek kümesi birden fazla öncelik türünü desteklemez.


**S:**  Ölçek ölçeği kümeleriyle otomatik ölçeklendirmeyi kullanabilir miyim?

Y **:** Evet, spot ölçek kümesinde otomatik ölçeklendirme kuralları ayarlayabilirsiniz. VM 'niz çıkarıldıysanız, otomatik ölçeklendirme yeni spot VM 'Ler oluşturmayı deneyebilir. Bu kapasitenin da garanti edilmediği unutulmamalıdır. 


**S:**  Otomatik ölçeklendirme, çıkarma ilkeleriyle (serbest bırakma ve silme) çalışır mi?

Y **:** Otomatik ölçeklendirme kullanırken çıkarma ilkenizi silinmek üzere ayarlamanız önerilir. Bunun nedeni, serbest bırakılmış örneklerin ölçek kümesindeki kapasite saymanız ile sayılmasıyla kaynaklanır. Otomatik ölçeklendirme kullanırken, serbest bırakılmış, çıkarılan örnekler nedeniyle hedef örnek sayınıza büyük olasılıkla ulaşmanız gerekir. 


**S:** Hangi kanallar spot VM 'Leri destekliyor?

Y **:** Nokta VM kullanılabilirliği için aşağıdaki tabloya bakın.

<a name="channel"></a>

| Azure kanalları               | Azure spot VM kullanılabilirliği       |
|------------------------------|-----------------------------------|
| Kurumsal Sözleşme         | Yes                               |
| Kullandıkça Öde                | Yes                               |
| Bulut hizmeti sağlayıcısı (CSP) | [İş ortağınızla iletişime geçin](https://docs.microsoft.com/partner-center/azure-plan-get-started) |
| Avantajlar                     | Kullanılamıyor                     |
| Sponsorlu                    | Kullanılamıyor                     |
| Ücretsiz Deneme                   | Kullanılamıyor                     |


**S:** Sorularınızı nereden gönderebilirim?

Y **:** Soru- [cevap A &](https://docs.microsoft.com/answers/topics/azure-spot.html)`azure-spot` sorunuzu gönderebilir ve etiketleyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
Artık spot VM 'Ler içeren bir ölçek kümesi oluşturduğumuza göre, [Otomatik ölçeklendirme şablonumuzu spot kullanarak](https://github.com/Azure/vm-scale-sets/tree/master/preview/lowpri)dağıtmaya çalışın.

Fiyatlandırma ayrıntıları için [sanal makine ölçek kümesi fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/) göz atın.
