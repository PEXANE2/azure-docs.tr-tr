---
title: Azure Monitör'de etkinlik günlüğü uyarıları
description: Etkinlik günlüğünde belirli olaylar meydana geldiğinde SMS, webhook, SMS, e-posta ve daha fazlası aracılığıyla bilgilendirilin.
ms.subservice: alerts
ms.topic: conceptual
ms.date: 09/17/2018
ms.openlocfilehash: 26ecfdb33b92c91010af63ec14089dd148d6bad0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77669022"
---
# <a name="alerts-on-activity-log"></a>Etkinlik günlüğündeki uyarılar

## <a name="overview"></a>Genel Bakış

Etkinlik günlüğü uyarıları, uyarıda belirtilen koşullarla eşleşen yeni bir [etkinlik günlüğü olayı](activity-log-schema.md) oluştuğunda etkinleştirilen uyarılardır. [Azure etkinlik günlüğüne](platform-logs-overview.md)kaydedilen olayların sırası ve hacmine bağlı olarak uyarı kuralı çalışacaktır. Etkinlik günlüğü uyarı kuralları Azure kaynaklarıdır, bu nedenle Azure Kaynak Yöneticisi şablonu kullanılarak oluşturulabilirler. Ayrıca Azure portalında oluşturulabilir, güncellenebilir veya silinebilir. Bu makalede, etkinlik günlüğü uyarıları arkasındaki kavramları tanıtır. Etkinlik günlüğü uyarı kurallarının oluşturulması veya kullanımı hakkında daha fazla bilgi için [bkz.](alerts-activity-log.md)

> [!NOTE]
> Etkinlik günlüğü Uyarı kategorisindeki olaylar için uyarılar **oluşturulamaz.**

Genellikle, bildirimleri almak için etkinlik günlüğü uyarıları oluşturursunuz:

* Azure aboneliğinizdeki kaynaklarda genellikle belirli kaynak gruplarına veya kaynaklara yönelik belirli işlemler gerçekleşir. Örneğin, myProductionResourceGroup'taki herhangi bir sanal makine silindiğinde bilgilendirilmek isteyebilirsiniz. Veya aboneliğinizdeki bir kullanıcıya yeni roller atanmışsa haberdar olmak isteyebilirsiniz.
* Bir hizmet durumu olayı oluşur. Servis durumu olayları, aboneliğinizdeki kaynaklar için geçerli olan olayların ve bakım olaylarının bildirimini içerir.

Etkinlik günlüğünde uyarı kurallarının oluşturulabileceği koşulları anlamak için basit bir benzetme, [Azure portalındaki Etkinlik günlüğü](activity-log-view.md#azure-portal)aracılığıyla olayları keşfetmek veya filtrelemektir. Azure Monitor - Etkinlik günlüğünde, etkinlik **günlüğü oluştur düğmesini** kullanarak bir kişi gerekli olayı filtreleyebilir veya bulabilir ve ardından bir uyarı oluşturabilir.

Her iki durumda da, bir etkinlik günlüğü uyarısı yalnızca uyarının oluşturulduğu abonelikteki olayları izler.

Bir etkinlik günlüğü olayı için JSON nesnesindeki herhangi bir üst düzey özelliği temel alan bir etkinlik günlüğü uyarısı yapılandırabilirsiniz. Daha fazla bilgi için [Etkinlik Günlüğündeki Kategoriler'e](activity-log-view.md#categories-in-the-activity-log)bakın. Hizmet durumu olayları hakkında daha fazla bilgi edinmek [için, hizmet bildirimlerinde etkinlik günlüğü uyarıları alın'a](alerts-activity-log-service-notifications.md)bakın. 

Etkinlik günlüğü uyarılarının birkaç ortak seçeneği vardır:

- **Kategori**: İdari, Hizmet Sağlığı, Otomatik Ölçek, Güvenlik, Politika ve Tavsiye. 
- **Kapsam**: Etkinlik günlüğündeki uyarının tanımlandığı tek tek kaynak veya kaynak kümesi. Etkinlik günlüğü uyarısının kapsamı çeşitli düzeylerde tanımlanabilir:
    - Kaynak Düzeyi: Örneğin, belirli bir sanal makine için
    - Kaynak Grubu Düzeyi: Örneğin, belirli bir kaynak grubundaki tüm sanal makineler
    - Abonelik Düzeyi: Örneğin, bir abonelikteki tüm sanal makineler (veya) bir abonelikteki tüm kaynaklar
- **Kaynak grubu**: Varsayılan olarak, uyarı kuralı Kapsam'ta tanımlanan hedefle aynı kaynak grubuna kaydedilir. Kullanıcı, uyarı kuralının depolandığı Kaynak Grubu'nu da tanımlayabilir.
- **Kaynak türü**: Kaynak Yöneticisi, uyarının hedefi için ad alanını tanımlamıştır.
- **İşlem adı**: Rol Tabanlı Erişim Denetimi için kullanılan [Azure Kaynak Yöneticisi işlem](../../role-based-access-control/resource-provider-operations.md) adı. Azure Kaynak Yöneticisi'ne kayıtlı olmayan işlemler bir etkinlik günlüğü uyarısı kuralında kullanılamaz.
- **Düzey**: Olayın önem düzeyi (Verbose, Informational, Warning, Error veya Critical).
- **Durum**: Genellikle Başlatılan, Başarısız Olan veya Başarılı Olan olayın durumu.
- **Başlatan olay**: "arayan" olarak da bilinir. E-posta adresi veya işlemi gerçekleştiren kullanıcının Azure Etkin Dizin tanımlayıcısı.

> [!NOTE]
> Bir abonelikte 100'e kadar uyarı kuralı kapsam etkinliği için de oluşturulabilir: tek bir kaynak, kaynak grubundaki tüm kaynaklar (veya) tüm abonelik düzeyinde.

Bir etkinlik günlüğü uyarısı etkinleştirildiğinde, eylemler veya bildirimler oluşturmak için bir eylem grubu kullanır. Eylem grubu, e-posta adresleri, webhook URL'leri veya SMS telefon numaraları gibi yeniden kullanılabilir bildirim alıcıları kümesidir. Alıcılar, bildirim kanallarınızı merkezileştirmek ve gruplandırmak için birden çok uyarıdan başvurulabilir. Etkinlik günlüğü uyarınızı tanımladığınızda, iki seçeneğiniz vardır. Şunları yapabilirsiniz:

* Etkinlik günlüğü uyarınızda varolan bir eylem grubu kullanın.
* Yeni bir eylem grubu oluşturun.

Eylem grupları hakkında daha fazla bilgi edinmek için Azure [portalında eylem grupları oluştur ve yönet'](action-groups.md)e bakın.


## <a name="next-steps"></a>Sonraki adımlar

- [Uyarılara genel bakış](alerts-overview.md)alın.
- Etkinlik [günlüğü uyarıları oluşturma ve değiştirme](alerts-activity-log.md)hakkında bilgi edinin.
- Etkinlik [günlüğü uyarısı webhook şema](activity-log-alerts-webhook.md)gözden geçirin.
- Hizmet [durumu bildirimleri](service-notifications.md)hakkında bilgi edinin.
