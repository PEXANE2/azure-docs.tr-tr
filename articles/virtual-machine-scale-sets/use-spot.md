---
title: Azure spot VM 'Leri kullanan bir ölçek kümesi oluşturma
description: Maliyetlerden tasarruf etmek için spot VM 'Ler kullanan Azure sanal makine ölçek kümeleri oluşturmayı öğrenin.
author: cynthn
ms.author: cynthn
ms.topic: how-to
ms.service: virtual-machine-scale-sets
ms.subservice: spot
ms.date: 03/25/2020
ms.reviewer: jagaveer
ms.custom: jagaveer, devx-track-azurecli
ms.openlocfilehash: 2898364811616c16a0c33ea26dcaacace9c2c4ed
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87491808"
---
# <a name="azure-spot-vms-for-virtual-machine-scale-sets"></a>Sanal Makine Ölçek Kümeleri için Azure spot VM 'Ler 

Ölçek kümelerinde Azure spot kullanımı, önemli bir maliyet tasarruflarından kullanılmamış kapasitemizin avantajlarından yararlanmanızı sağlar. Azure 'un kapasiteyi her zaman yapması gerektiğinde, Azure altyapısı spot örnekleri çıkarır. Bu nedenle, toplu işleme işleri, geliştirme/test ortamları, büyük işlem iş yükleri ve daha fazlası gibi kesintileri işleyebilen iş yükleri için spot örnekler idealdir.

Kullanılabilir kapasite miktarı boyut, bölge, günün saati ve daha fazlası temelinde farklılık gösterebilir. Ölçek kümelerine spot örnekleri dağıttığınızda Azure, örneği yalnızca kullanılabilir kapasite varsa ayırır, ancak bu örnekler için SLA yoktur. Bir spot ölçek kümesi, tek bir hata etki alanında dağıtılır ve yüksek oranda kullanılabilirlik garantisi sunar.


## <a name="pricing"></a>Fiyatlandırma

Spot örnekleri için fiyatlandırma, bölgeye ve SKU 'ya göre değişkendir. Daha fazla bilgi için bkz. [Linux](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/) ve [Windows](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/windows/)fiyatlandırması. 


Değişken fiyatlandırmayla, en fazla 5 ondalık basamak kullanarak ABD Doları (USD) cinsinden maksimum fiyat ayarlama seçeneğiniz vardır. Örneğin, değer, `0.98765` saat başına $0,98765 ABD Doları olan en yüksek fiyat olacaktır. En yüksek fiyatı olacak şekilde ayarlarsanız `-1` , örnek fiyata göre çıkarılmaz. Örneğin fiyatı, kapasite ve kota kullanılabilir olduğu sürece daha az olan bir standart örnek için geçerli fiyat veya fiyat fiyatı olacaktır.

## <a name="eviction-policy"></a>Çıkarma ilkesi

Spot ölçek kümeleri oluştururken, çıkarma ilkesini *serbest bırakma* (varsayılan) veya *silme*işlemleri için ayarlayabilirsiniz. 

*Serbest bırakma* ilkesi, çıkarılan örnekleri yeniden dağıtmanıza izin vererek, çıkarılan örneklerinizi durdurulmuş serbest bırakılmış duruma kaydırır. Ancak, ayırmanın başarılı olacağını garanti etmez. Serbest bırakılmış VM 'Ler, ölçek kümesi örneği kotanıza göre sayılır ve temel disklerinizin ücreti alınır. 

Çıkarma kümesini, çıkarıldıklarında silinmek üzere bir yere isterseniz, çıkarma ilkesini *silme*olarak ayarlayabilirsiniz. Çıkarma ilkesi silinmek üzere ayarlandığında, ölçek kümesi örnek sayısı özelliğini artırarak yeni VM 'Ler oluşturabilirsiniz. Çıkarılan VM 'Ler, temel disklerle birlikte silinir ve bu nedenle depolama alanı için ücretlendirilmeyecektir. Ayrıca, çıkarılan VM 'Leri otomatik olarak denemek ve dengelemek için ölçek kümelerinin otomatik ölçeklendirme özelliğini de kullanabilirsiniz; ancak, ayırmanın başarılı olacağını garanti etmez. Disk maliyetinden ve kota limitlerine ulaşmaktan kaçınmak için çıkarma ilkesini silme olarak belirlediğinizde, yalnızca spot ölçek kümelerinde otomatik ölçeklendirme özelliğini kullanmanız önerilir. 

Kullanıcılar [Azure zamanlanan olaylar](../virtual-machines/linux/scheduled-events.md)aracılığıyla VM içi bildirimler almayı kabul edebilir. Bu, VM 'leriniz çıkarıldıktan sonra herhangi bir işi tamamlamak ve çıkarma öncesi görevleri gerçekleştirmek için 30 saniyelik bir işlem yapmanız durumunda size bildirir. 


## <a name="deploying-spot-vms-in-scale-sets"></a>Ölçek kümelerinde spot VM 'Leri dağıtma

Ölçek kümeleri üzerinde spot VM 'Leri dağıtmak için yeni *Öncelik* bayrağını *nokta*olarak ayarlayabilirsiniz. Ölçek kümesindeki tüm sanal makineler, spot olarak ayarlanacak. Spot VM 'Ler içeren bir ölçek kümesi oluşturmak için aşağıdaki yöntemlerden birini kullanın:
- [Azure Portal](#portal)
- [Azure CLI](#azure-cli)
- [Azure PowerShell](#powershell)
- [Azure Resource Manager şablonları](#resource-manager-templates)

## <a name="portal"></a>Portal

Spot VM 'Ler kullanan bir ölçek kümesi oluşturma işlemi Başlarken [makalesinde](quick-create-portal.md)ayrıntılıdır. Bir ölçek kümesi dağıttığınızda, spot bayrağını ve çıkarma ilkesini ayarlamayı seçebilirsiniz: ![ spot VM 'ler ile ölçek kümesi oluşturma](media/virtual-machine-scale-sets-use-spot/vmss-spot-portal-max-price.png)


## <a name="azure-cli"></a>Azure CLI

Spot VM 'Ler içeren bir ölçek kümesi oluşturma işlemi Başlarken [makalesinde](quick-create-cli.md)ayrıntılıdır. '--Priority noktası ' ve Ekle ' yi eklemeniz yeterlidir `--max-price` . Bu örnekte, `-1` Örneğin `--max-price` fiyata göre çıkarılamadığı için kullanırız.

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
' Öncelikli nokta ' ekleyin ve `-max-price` [New-AzVmssConfig](/powershell/module/az.compute/new-azvmssconfig)için bir sağlayın.

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

Spot VM 'Ler kullanan bir ölçek kümesi oluşturma işlemi, [Linux](quick-create-template-linux.md) veya [Windows](quick-create-template-windows.md)için Başlarken makalesinde ayrıntılı olarak aynıdır. 

Spot şablon dağıtımları için `"apiVersion": "2019-03-01"` veya üstünü kullanın. `priority` `evictionPolicy` `billingProfile` Şablonunuzda bölümüne ve özelliklerini ekleyin `"virtualMachineProfile":` : 

```json
                "priority": "Spot",
                "evictionPolicy": "Deallocate",
                "billingProfile": {
                    "maxPrice": -1
                }
```

Çıkarıldıktan sonra örneği silmek için `evictionPolicy` parametresini olarak değiştirin `Delete` .

## <a name="faq"></a>SSS

**S:** Oluşturulduktan sonra, standart örnekle aynı bir spot örneğidir mi?

Y **:** Evet, spot VM 'Ler için SLA olmaması ve herhangi bir zamanda çıkartılanlar haricinde.


**S:** Ne yapmalı, ancak yine de kapasiteye ihtiyaç duydunuz?

Y **:** Kapasiteye ihtiyacınız varsa, nokta VM 'Leri yerine standart VM 'Ler kullanmanızı öneririz.


**S:** Kota, Spot için nasıl yönetilir?

Y **:** Spot örnekleri ve standart örneklerin ayrı kota havuzları olacaktır. Spot kota, VM 'Ler ve ölçek kümesi örnekleri arasında paylaşılır. Daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md).


**S:** Nokta için ek kota isteyebilir miyim?

Y **:** Evet, [Standart kota isteği işlemi](../azure-portal/supportability/per-vm-quota-requests.md)aracılığıyla spot VM 'lerle ilgili kotayı artırmak için isteği gönderebileceksiniz.


**S:** Varolan ölçek kümelerini spot ölçek kümelerine dönüştürebilir miyim?

Y **:** Hayır, bayrak ayarı `Spot` yalnızca oluşturma sırasında desteklenir.


**S:** `low`Düşük öncelikli ölçek kümeleri için kullanılıyorsa, bunun yerine kullanmaya başlamam gerekir `Spot` mi?

Y **:** Şimdilik her ikisi de `low` `Spot` çalışır, ancak kullanmaya geçiş başlatmanız gerekir `Spot` .


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
| Kurumsal Anlaşma         | Yes                               |
| Kullandıkça Öde                | Yes                               |
| Bulut hizmeti sağlayıcısı (CSP) | [İş ortağınızla iletişime geçin](/partner-center/azure-plan-get-started) |
| Yararları                     | Kullanılamaz                     |
| Sponsorlu                    | Yes                               |
| Ücretsiz Deneme                   | Kullanılamaz                     |


**S:** Sorularınızı nereden gönderebilirim?

Y **:** Soru- `azure-spot` [cevap A&](/answers/topics/azure-spot.html), sorunuzu gönderebilirsiniz ve etiketleyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Fiyatlandırma ayrıntıları için [sanal makine ölçek kümesi fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/) göz atın.
