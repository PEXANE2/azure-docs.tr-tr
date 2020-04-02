---
title: include dosyası
description: include dosyası
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/23/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: d56964b7415e4ca5903950cd46c02b3c27f62d5e
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80547400"
---
Spot VM'leri kullanmak, önemli bir maliyet tasarrufu nda kullanılmayan kapasitemizden yararlanmanızı sağlar. Azure'un kapasiteye geri ihtiyacı olduğu herhangi bir zamanda, Azure altyapısı Spot VM'leri boşaltacaktır. Bu nedenle, Spot VM'ler toplu iş işleri, geliştirme/test ortamları, büyük bilgi işlem iş yükleri ve daha fazlası gibi kesintileri işleyebilir iş yükleri için idealdir.

Kullanılabilir kapasite miktarı boyuta, bölgeye, günün saatine ve daha fazlana bağlı olarak değişebilir. Spot VM'leri dağıtırken, Azure kapasite varsa VM'leri ayırır, ancak bu VM'ler için SLA yoktur. Spot VM yüksek kullanılabilirlik garantisi sunmaz. Azure'un kapasiteye geri ihtiyacı olduğu herhangi bir zamanda, Azure altyapısı Spot VM'leri 30 saniye önceden haber verebecektir. 


## <a name="eviction-policy"></a>Tahliye politikası

VM'ler kapasiteye veya belirlediğiniz maksimum fiyata göre tahliye edilebilir. Sanal makineler için tahliye ilkesi, tahliye edilen VM'lerinizi durdurulan duruma taşıyan ve tahliye edilen VM'leri daha sonra yeniden dağıtmanıza olanak tanıyan *Deallocate* olarak ayarlanır. Ancak, Spot VM'lerin yeniden tahsisi, mevcut Spot kapasitesine bağlı olacaktır. Ayrılan VM'ler spot vCPU kotanıza göre sayılacak ve temel diskleriniz için ücretlendirilirsiniz. 

Kullanıcılar [Azure Zamanlanmış Etkinlikler](../articles/virtual-machines/linux/scheduled-events.md)aracılığıyla VM içi bildirimleri almayı kabul edebilirler. Bu, VM'leriniz tahliye ediliyorsa sizi bilgilendirecektir ve tahliyeden önce herhangi bir işi bitirmek ve kapatma görevlerini gerçekleştirmek için 30 saniyeniz olacak. 


| Seçenek | Sonuç |
|--------|---------|
| Maksimum fiyat >= geçerli fiyat olarak ayarlanır. | Kapasite ve kota varsa VM dağıtılır. |
| Maksimum fiyat geçerli fiyatı < olarak ayarlanır. | VM dağıtılmadı. Maksimum fiyatın >= geçerli fiyat olması gerektiğine dair bir hata iletisi alırsınız. |
| Maksimum fiyat >= geçerli fiyat ise bir stop/deallocate VM'yi yeniden başlatma | Kapasite ve kota varsa, VM dağıtılır. |
| Maksimum fiyat geçerli fiyat < ise bir stop/deallocate VM'yi yeniden başlatma | Maksimum fiyatın >= geçerli fiyat olması gerektiğine dair bir hata iletisi alırsınız. | 
| VM için fiyat yükseldi ve şimdi maksimum fiyat >. | VM tahliye edildi. Gerçek tahliyeden önce 30'lar bildirimi alırsınız. | 
| Tahliye den sonra VM için fiyat max fiyat < geri gider. | VM otomatik olarak yeniden başlatılacaktır. VM'yi kendiniz yeniden başlatabilirsiniz ve geçerli fiyatüzerinden ücretlendirilir. |
| Maksimum fiyat ayarlanırsa`-1` | VM fiyatlandırma nedenleriyle tahliye edilmez. Maksimum fiyat, standart VM'ler için fiyata kadar geçerli fiyat olacaktır. Hiçbir zaman standart fiyatın üzerinde ücretlendirilmezsiniz.| 
| Maksimum fiyatı değiştirme | Maksimum fiyatı değiştirmek için VM'yi bulmanız gerekir. Deallocate VM, t yeni bir max fiyat ayarlamak, sonra VM güncelleyin. |

## <a name="limitations"></a>Sınırlamalar

Aşağıdaki VM boyutları Spot VM'ler için desteklenmez:
 - B serisi
 - Herhangi bir boyutta promosyon sürümleri (Dv2, NV, NC, H promo boyutları gibi)

Spot VM'ler şu anda kısa ömürlü işletim sistemi diskleri kullanamaz.

Spot VM'ler Microsoft Azure China 21Vianet dışında herhangi bir bölgeye dağıtılabilir.

## <a name="pricing"></a>Fiyatlandırma

Spot VM'ler için fiyatlandırma, bölgeye ve SKU'ya göre değişkendir. Daha fazla bilgi için [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) ve [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/)için VM fiyatlandırması için bkz. 


Değişken fiyatlandırma ile, en fazla 5 ondalık basamak kullanarak, ABD doları (USD) cinsinden maksimum fiyat belirleme seçeneğiniz bulunmaktadır. Örneğin, değeri `0.98765`saatte 0,98765 USD maksimum fiyat olacaktır. Maksimum fiyatı olarak `-1`ayarlarsanız, VM fiyata göre tahliye olmaz. VM için fiyat spot veya standart bir VM için fiyat olacak, hangi hiç daha az, sürece kapasite ve kota mevcuttur.


##  <a name="frequently-asked-questions"></a>Sık sorulan sorular

**S:** Oluşturulduktan sonra, Spot VM normal standart VM ile aynı mıdır?

**A:** Evet, spot VM'ler için SLA olmaması ve istedikleri zaman tahliye edilebilemeleri dışında.


**S:** Tahliye edildiğinde ama yine de kapasiteye ihtiyaç duyduğunuzda ne yapmalısınız?

**A:** Hemen kapasiteye ihtiyacınız varsa Spot VM yerine standart VM kullanmanızı öneririz.


**S:** Spot VM'ler için kota nasıl yönetilir?

**A:** Spot VM'lerde ayrı bir kota havuzu olacaktır. Spot kota, VM'ler ve ölçek ayarlı örnekler arasında paylaşılır. Daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits).


**S:** Spot için ek kota talep edebilir miyim?

**A:** Evet, [standart kota istek süreci](https://docs.microsoft.com/azure/azure-portal/supportability/per-vm-quota-requests)aracılığıyla Spot VM'ler için kotanızı artırma isteği gönderebileceksiniz.


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



