---
title: Azure Spot VM'leri kullanan bir ölçek kümesi oluşturma
description: Maliyetlerden tasarruf etmek için Spot VM'leri kullanan Azure sanal makine ölçeği kümelerini nasıl oluşturabilirsiniz öğrenin.
author: cynthn
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.topic: article
ms.date: 03/25/2020
ms.author: cynthn
ms.openlocfilehash: a7bd22032a554c83a2ea2323ffdb3ae52dfe4faf
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80545931"
---
# <a name="azure-spot-vms-for-virtual-machine-scale-sets"></a>Sanal makine ölçek setleri için Azure Spot VM'ler 

Azure Spot'u ölçek kümelerinde kullanmak, önemli bir maliyet tasarrufunda kullanılmayan kapasitemizden yararlanmanızı sağlar. Azure'un kapasiteye geri ihtiyacı olduğu herhangi bir zamanda, Azure altyapısı Spot örneklerini boşaltacaktır. Bu nedenle, Spot örnekleri toplu iş işleri, geliştirme/test ortamları, büyük bilgi işlem iş yükleri ve daha fazlası gibi kesintileri işleyebilir iş yükleri için harikadır.

Kullanılabilir kapasite miktarı boyuta, bölgeye, günün saatine ve daha fazlana bağlı olarak değişebilir. Spot örneklerini ölçek kümelerine dağıtırken, Azure örneği yalnızca kapasite varsa ayırır, ancak bu durumlar için SLA yoktur. Spot ölçek kümesi tek bir hata etki alanında dağıtılır ve yüksek kullanılabilirlik garantisi sunmaz.


## <a name="pricing"></a>Fiyatlandırma

Spot örnekleri için fiyatlandırma, bölge ve SKU'ya göre değişkendir. Daha fazla bilgi için [Linux](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/) ve [Windows](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/windows/)için fiyatlandırmaya bakın. 


Değişken fiyatlandırma ile, en fazla 5 ondalık basamak kullanarak, ABD doları (USD) cinsinden maksimum fiyat belirleme seçeneğiniz bulunmaktadır. Örneğin, değeri `0.98765`saatte 0,98765 USD maksimum fiyat olacaktır. Maksimum fiyatı olarak `-1`ayarlarsanız, örnek fiyata göre tahliye olmaz. Örneğin fiyatı, kapasite ve kota mevcut olduğu sürece Spot'un geçerli fiyatı veya standart bir örneğin fiyatı olacaktır, ki bu daha azdır.

## <a name="eviction-policy"></a>Tahliye politikası

Spot ölçek kümeleri oluştururken, çıkarma ilkesini *Deallocate* (varsayılan) veya *Sil*olarak ayarlayabilirsiniz. 

*Deallocate* ilkesi, tahliye edilen örnekleri, tahliye edilen örnekleri yeniden dağıtmanıza olanak tanıyan, devre dışı bırakılan duruma taşır. Ancak, tahsisat başarılı olacağını garanti yoktur. Ayrılan VM'ler ölçek belirleme örneği kotanıza göre sayılır ve temel diskleriniz için ücretlendirilirsiniz. 

Spot ölçeğinizdeki örneklerinizin tahliye edildiklerinde silinmesini istiyorsanız, tahliye ilkesini *silmek*üzere ayarlayabilirsiniz. Silme ilkesi nin silinmeye ayarlı olmasıyla, ölçek kümesi örnek sayısı özelliğini artırarak yeni VM'ler oluşturabilirsiniz. Tahliye edilen VM'ler altta yatan diskleriyle birlikte silinir ve bu nedenle depolama alanı için ücretlendirilmezsiniz. Ayrıca, çıkarılan VM'leri otomatik olarak denemek ve telafi etmek için ölçek kümelerinin otomatik ölçeklendirme özelliğini de kullanabilirsiniz, ancak ayırmanın başarılı olacağının garantisi yoktur. Disklerinizin maliyetini önlemek ve kota sınırlarını vurmak için çıkarma ilkesini silmek üzere ayarlarken yalnızca Spot ölçek kümelerinde otomatik ölçek özelliğini kullanmanız önerilir. 

Kullanıcılar [Azure Zamanlanmış Etkinlikler](../virtual-machines/linux/scheduled-events.md)aracılığıyla VM içi bildirimleri almayı kabul edebilirler. Bu, VM'leriniz tahliye ediliyorsa sizi bilgilendirecektir ve tahliyeden önce herhangi bir işi bitirmek ve kapatma görevlerini gerçekleştirmek için 30 saniyeniz olacak. 


## <a name="deploying-spot-vms-in-scale-sets"></a>Spot VM'leri ölçek kümelerinde dağıtma

Spot VM'leri ölçek kümelerine dağıtmak için, yeni *Öncelik* bayrağını *Spot'a*ayarlayabilirsiniz. Ölçek setinizdeki tüm VM'ler Spot olarak ayarlanır. Spot VM'lerle bir ölçek kümesi oluşturmak için aşağıdaki yöntemlerden birini kullanın:
- [Azure portalı](#portal)
- [Azure CLI](#azure-cli)
- [Azure PowerShell](#powershell)
- [Azure Resource Manager şablonları](#resource-manager-templates)

## <a name="portal"></a>Portal

Spot VM'leri kullanan bir ölçek kümesi oluşturma [işlemi, başlarken makalede](quick-create-portal.md)ayrıntılı olarak belirtildiği gibi. Bir ölçek kümesi dağıtırken, Spot bayrağını ve çıkarma ilkesini ayarlamayı ![seçebilirsiniz: Spot VM'lerle bir ölçek kümesi oluşturma](media/virtual-machine-scale-sets-use-spot/vmss-spot-portal-max-price.png)


## <a name="azure-cli"></a>Azure CLI

Spot VM'ler ile bir ölçek kümesi oluşturma [işlemi, başlarken makalede](quick-create-cli.md)ayrıntılı olarak belirtilmiştir. Sadece '--Öncelikli Nokta'yı ekleyin `--max-price`ve ekleyin. Bu örnekte, `-1` örneğin `--max-price` fiyata göre tahliye edilmeyişemelerini kullanırız.

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

Spot VM'ler ile bir ölçek kümesi oluşturma [işlemi, başlarken makalede](quick-create-powershell.md)ayrıntılı olarak belirtilmiştir.
Sadece '-Öncelikli Spot' ekleyin `-max-price` ve [Yeni-AzVmssConfig](/powershell/module/az.compute/new-azvmssconfig)bir kaynağı .

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

Spot VM'leri kullanan bir ölçek kümesi oluşturma [işlemi, Linux](quick-create-template-linux.md) veya [Windows](quick-create-template-windows.md)için başlangıç makalesinde ayrıntılı olarak belirtildiği gibi. 

Spot şablon dağıtımları`"apiVersion": "2019-03-01"` için kullanın veya daha sonra. `priority`Şablonunuzdaki `evictionPolicy` `"virtualMachineProfile":` `billingProfile` bölüme , ve özellikleri ekleyin: 

```json
                "priority": "Spot",
                "evictionPolicy": "Deallocate",
                "billingProfile": {
                    "maxPrice": -1
                }
```

Örneği tahliye edildikten sonra silmek için `evictionPolicy` parametreyi `Delete`' ' olarak değiştirin

## <a name="faq"></a>SSS

**S:** Oluşturulduktan sonra, spot örneği standart örnekle aynı mıdır?

**A:** Evet, spot VM'ler için SLA olmaması ve istedikleri zaman tahliye edilebilemeleri dışında.


**S:** Tahliye edildiğinde ama yine de kapasiteye ihtiyaç duyduğunuzda ne yapmalısınız?

**A:** Hemen kapasiteye ihtiyacınız varsa Spot VM yerine standart VM kullanmanızı öneririz.


**S:** Spot için kota nasıl yönetilir?

**A:** Spot örnekleri ve standart örnekler ayrı kota havuzları olacaktır. Spot kota, VM'ler ve ölçek ayarlı örnekler arasında paylaşılır. Daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits).


**S:** Spot için ek kota talep edebilir miyim?

**A:** Evet, [standart kota istek süreci](https://docs.microsoft.com/azure/azure-portal/supportability/per-vm-quota-requests)aracılığıyla Spot VM'ler için kotanızı artırma isteği gönderebileceksiniz.


**S:** Varolan ölçek kümelerini Spot ölçek kümelerine dönüştürebilir miyim?

**A:** Hayır, `Spot` bayrağı ayarlamak yalnızca oluşturma zamanında desteklenir.


**S:** Düşük öncelikli `low` ölçek kümeleri için kullanıyorsam, bunun `Spot` yerine kullanmaya başlamam gerekir mi?

**A:** Şimdilik, hem `low` `Spot` de çalışacak, ancak kullanmaya `Spot`geçiş başlamalıdır.


**S:** Hem normal VM'ler hem de Spot VM'ler içeren bir ölçek kümesi oluşturabilir miyim?

**A:** Hayır, ölçek kümesi birden fazla öncelik türünü destekleyemez.


**S:**  Spot ölçek kümeleriyle otomatik ölçek kullanabilir miyim?

**A:** Evet, Spot ölçek kümenizde otomatik ölçeklendirme kuralları ayarlayabilirsiniz. VM'leriniz boşaltılırsa, otomatik ölçek yeni Spot VM'ler oluşturmayı deneyebilir. Unutmayın, bu kapasite olsa garanti edilmez. 


**S:**  Otomatik ölçeklendirme her iki tahliye ilkeleriyle (anlaşma yeri ve silme) çalışır mı?

**A:** Otomatik ölçek kullanırken tahliye ilkenizi silmek üzere ayarlamanız önerilir. Bunun nedeni, ayrılan örneklerin ölçek kümesindeki kapasite sayınıza göre sayılmasıdır. Otomatik ölçeklendirme kullanırken, büyük olasılıkla tahsis edilmiş, tahliye örnekleri nedeniyle hedef örnek sayımınızı hızlı bir şekilde vuracaktır. 


**S:** Spot VM'leri hangi kanallar destekler?

**A:** Spot VM kullanılabilirliği için aşağıdaki tabloya bakın.

<a name="channel"></a>

| Azure Kanalları               | Azure Spot VMs Kullanılabilirliği       |
|------------------------------|-----------------------------------|
| Kurumsal Anlaşma         | Evet                               |
| Kullandıkça Öde                | Evet                               |
| Bulut Hizmet Sağlayıcısı (CSP) | [Eşinizle iletişim kurun](https://docs.microsoft.com/partner-center/azure-plan-get-started) |
| Avantajlar                     | Kullanılamaz                     |
| Sponsor                    | Kullanılamaz                     |
| Ücretsiz Deneme                   | Kullanılamaz                     |


**S:** Soruları nereye gönderebilirim?

**A:** Sorunuzu Q&`azure-spot` [A'da](https://docs.microsoft.com/answers/topics/azure-spot.html)yayınlayabilir ve etiketleyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Fiyatlandırma ayrıntıları için [sanal makine ölçeği ayar fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/) göz atın.
