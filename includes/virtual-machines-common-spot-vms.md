---
title: dosya dahil etme
description: dosya dahil etme
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 06/26/2020
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 8ee5973afb9312688178abd9a186c5319032c493
ms.sourcegitcommit: 374e47efb65f0ae510ad6c24a82e8abb5b57029e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2020
ms.locfileid: "85506063"
---
Spot VM 'Lerin kullanılması, önemli bir maliyet tasarruflarından kullanılmamış kapasitemizin avantajlarından yararlanmanızı sağlar. Azure 'un kapasiteyi her zaman yapması gerektiğinde, Azure altyapısı spot VM 'Leri çıkarır. Bu nedenle, spot VM 'Ler toplu işleme işleri, geliştirme/test ortamları, büyük işlem iş yükleri ve daha fazlası gibi kesintileri işleyebilen iş yükleri için mükemmeldir.

Kullanılabilir kapasite miktarı boyut, bölge, günün saati ve daha fazlası temelinde farklılık gösterebilir. Spot VM 'Leri dağıttığınızda, kullanılabilir kapasite varsa Azure VM 'Leri ayırır, ancak bu VM 'Ler için SLA yoktur. Bir spot VM, yüksek oranda kullanılabilirlik garantisi sunar. Azure 'un kapasiteyi yeniden yapması gerektiğinde, Azure altyapısı, nokta VM 'Leri 30 saniye olarak çıkaracaktır. 


## <a name="eviction-policy"></a>Çıkarma ilkesi

VM 'Ler, belirlenen kapasiteye veya en yüksek fiyata göre çıkartılabilir. Bir spot VM oluştururken, çıkarma ilkesini *serbest bırakma* (varsayılan) veya *silme*işlemleri için ayarlayabilirsiniz. 

*Serbest bırakma* ILKESI, VM 'nizi durdurulmuş serbest bırakılmış duruma, daha sonra yeniden dağıtmanıza olanak tanır. Ancak, ayırmanın başarılı olacağını garanti etmez. Serbest bırakılmış VM 'Ler kotaıza göre sayılır ve temel diskler için depolama maliyetleri ücretlendirilecektir. 

SANAL makinenizin çıkarıldıktan sonra silinmesini isterseniz, çıkarma ilkesini *silme*olarak ayarlayabilirsiniz. Çıkarılan VM 'Ler temel disklerle birlikte silinir, bu nedenle depolama alanı için ücretlendirilmeye devam edersiniz. 

> [!NOTE]
>
> Portal Şu anda `Delete` çıkarma seçeneği olarak desteklememektedir, yalnızca `Delete` POWERSHELL, CLI ve şablonları kullanarak ayarlayabilirsiniz.

[Azure zamanlanan olaylar](../articles/virtual-machines/linux/scheduled-events.md)aracılığıyla VM içi bildirimler almayı tercih edebilirsiniz. Bu, VM 'leriniz çıkarıldıktan sonra herhangi bir işi tamamlamak ve çıkarma öncesi görevleri gerçekleştirmek için 30 saniyelik bir işlem yapmanız durumunda size bildirir. 


| Seçenek | Sonuç |
|--------|---------|
| En yüksek fiyat >= geçerli fiyat olarak ayarlanır. | Kapasite ve kota kullanılabiliyorsa VM dağıtılır. |
| En yüksek fiyat geçerli fiyata < şekilde ayarlanır. | VM dağıtılmadı. En yüksek fiyatın >= geçerli fiyat olması gerektiğini belirten bir hata iletisi alırsınız. |
| En yüksek fiyat >= geçerli fiyat ise bir Durdur/serbest bırakma VM yeniden başlatılıyor | Kapasite ve kota varsa, sanal makine dağıtılır. |
| En yüksek fiyat geçerli fiyat <, bir Durdur/serbest bırakma VM 'si yeniden başlatılıyor | En yüksek fiyatın >= geçerli fiyat olması gerektiğini belirten bir hata iletisi alırsınız. | 
| VM 'nin fiyatı geçti ve artık maksimum fiyata >. | VM çıkarıldı. Gerçek çıkarmadan önce bir 30 saniye bildirimi alırsınız. | 
| Çıkarmaya başladıktan sonra, VM 'nin fiyatı en fazla fiyata < durumuna geçer. | VM otomatik olarak yeniden başlatılmaz. VM 'yi kendiniz yeniden başlatabilir ve geçerli fiyattan ücretlendirilir. |
| En fazla fiyat ayarı`-1` | VM, fiyatlandırma nedenleriyle çıkarılmayacak. En yüksek fiyat, standart VM 'Lerin fiyatına kadar geçerli fiyat olacaktır. Standart fiyat üzerinden hiçbir şekilde ücretlendirilmezsiniz.| 
| Maksimum fiyatı değiştirme | En yüksek fiyatı değiştirmek için VM 'yi serbest getirmeniz gerekir. VM 'yi serbest bırakın, yeni bir en yüksek fiyat ayarlayın ve ardından VM 'yi güncelleştirin. |


## <a name="limitations"></a>Sınırlamalar

Aşağıdaki VM boyutları, spot VM 'Ler için desteklenmez:
 - B serisi
 - Her boyuttaki promosyon sürümleri (dv2, NV, NC, H promosyon boyutları gibi)

Spot sanal makineler, Microsoft Azure Çin 21Vianet dışında herhangi bir bölgeye dağıtılabilir.

Bazı abonelik kanalları desteklenmez:

<a name="channel"></a>

| Azure kanalları               | Azure spot VM kullanılabilirliği       |
|------------------------------|-----------------------------------|
| Kurumsal Anlaşma         | Yes                               |
| Kullandıkça Öde                | Yes                               |
| Bulut hizmeti sağlayıcısı (CSP) | [İş ortağınızla iletişime geçin](https://docs.microsoft.com/partner-center/azure-plan-get-started) |
| Yararları                     | Kullanılamaz                     |
| Sponsorlu                    | Yes                               |
| Ücretsiz Deneme                   | Kullanılamaz                     |



## <a name="pricing"></a>Fiyatlandırma

Nokta VM 'Leri için fiyatlandırma, bölge ve SKU temel alınarak değişkendir. Daha fazla bilgi için bkz. [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) ve [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/)için VM fiyatlandırması. 


Değişken fiyatlandırmayla, en fazla 5 ondalık basamak kullanarak ABD Doları (USD) cinsinden maksimum fiyat ayarlama seçeneğiniz vardır. Örneğin, değer, `0.98765` saat başına $0,98765 ABD Doları olan en yüksek fiyat olacaktır. En yüksek fiyatı olacak şekilde ayarlarsanız `-1` , VM fiyata göre çıkarılmaz. Kapasite ve kota kullanılabilir olduğu sürece, sanal makine fiyatı, nokta için geçerli fiyat veya standart bir sanal makine fiyatı olacaktır.


##  <a name="frequently-asked-questions"></a>Sık sorulan sorular

**S:** Oluşturulduktan sonra, normal standart VM ile aynı bir spot VM mi?

Y **:** Evet, spot VM 'Ler için SLA olmaması ve herhangi bir zamanda çıkartılanlar haricinde.


**S:** Ne yapmalı, ancak yine de kapasiteye ihtiyaç duydunuz?

Y **:** Kapasiteye ihtiyacınız varsa, nokta VM 'Leri yerine standart VM 'Ler kullanmanızı öneririz.


**S:** Spot VM 'Ler için kota nasıl yönetilir?

Y **:** Spot VM 'Lerin ayrı bir kota havuzu olacaktır. Spot kota, VM 'Ler ve ölçek kümesi örnekleri arasında paylaşılır. Daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits).


**S:** Nokta için ek kota isteyebilir miyim?

Y **:** Evet, [Standart kota isteği işlemi](https://docs.microsoft.com/azure/azure-portal/supportability/per-vm-quota-requests)aracılığıyla spot VM 'lerle ilgili kotayı artırmak için isteği gönderebileceksiniz.


**S:** Sorularınızı nereden gönderebilirim?

Y **:** Soru- `azure-spot` [cevap A&](https://docs.microsoft.com/answers/topics/azure-spot.html), sorunuzu gönderebilirsiniz ve etiketleyebilirsiniz. 



