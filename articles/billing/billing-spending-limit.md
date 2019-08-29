---
title: Azure harcama limiti | Microsoft Docs
description: Bu makalede bir Azure harcama limitinin nasıl çalıştığı ve nasıl kaldırılacağı açıklanır.
author: bandersmsft
manager: amberb
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/28/2019
ms.author: banders
ms.openlocfilehash: 1324b60de05805cd409975358ff78e5b2b27eef1
ms.sourcegitcommit: 8e1fb03a9c3ad0fc3fd4d6c111598aa74e0b9bd4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70114444"
---
# <a name="azure-spending-limit"></a>Azure harcama limiti

Azure 'daki harcama limiti, kredi miktarınız üzerinden harcamayı önler. Birden çok aya ait krediler içeren bir Azure deneme veya tekliflerine kaydolan tüm yeni müşterilerin harcama limiti varsayılan olarak açıktır. Harcama limiti $0 ' dir ve değiştirilemez. Örneğin, harcama limitini $100 olarak değiştiremezsiniz. Bununla birlikte, harcama limitini de kaldırabilirsiniz. Bu nedenle, herhangi bir sınırı yoktur ya da en fazla harcama çeşitinizden kurtulabilmeniz gereken bir sınırı sıfır. Harcama limiti, taahhüt planları ve kullandıkça öde fiyatlandırmasına sahip planlar kapsamındaki abonelikler için kullanılabilir değildir. [Azure tekliflerinin tam listesine ve harcama limitinin kullanılabilirliğine](https://azure.microsoft.com/support/legal/offer-details/)bakın.

## <a name="reaching-a-spending-limit"></a>Harcama sınırına ulaşma

Kullanımınız, Azure aboneliğinize dahil edilen aylık miktarları karşılayan ücretlerle sonuçlanırsa, dağıttığınız hizmetler bu fatura döneminin geri kalanı için devre dışı bırakılır.

Örneğin, aboneliğinize dahil edilen tüm kredileri harcadığınızda, dağıttığınız Azure kaynakları üretimden kaldırılır ve Azure sanal makineleriniz durdurulur ve serbest bırakılır. Depolama hesaplarınızdaki veriler salt okunurdur.

Bir sonraki fatura döneminin başlangıcında, abonelik teklifiniz birden fazla aya ait krediler içeriyorsa, aboneliğiniz otomatik olarak yeniden etkinleştirilir. Daha sonra Azure kaynaklarınızı yeniden dağıtabilirsiniz ve depolama hesaplarınız ile veritabanlarınıza tam erişim sağlayabilirsiniz.

Aboneliğiniz için harcama sınırına ulaştığınızda Azure e-posta bildirimleri gönderir. Harcama limitine ulaşan Aboneliklerle ilgili bildirimleri görmek için [Hesap merkezinde](https://account.windowsazure.com/Subscriptions) oturum açın.

Ücretsiz bir deneme aboneliğiniz varsa ve harcama limitine ulaşırsanız, harcama limitini kaldırmak ve aboneliği otomatik olarak etkinleştirmek için [Kullandıkça Öde](billing-upgrade-azure-subscription.md) fiyatlandırmasıyla bir plana yükseltebilirsiniz.

<a id="remove"></a>

## <a name="remove-the-spending-limit-in-account-center"></a>Hesap Merkezi 'nde harcama limitini kaldırma

Azure aboneliğinizle ilişkili geçerli bir ödeme yöntemi olduğu sürece harcama limitini dilediğiniz zaman kaldırabilirsiniz. Birden çok aya kredi içeren teklifler için, bir sonraki fatura döneminizin başlangıcında harcama limitini de etkinleştirebilirsiniz.

Harcama limitini kaldırmak için şu adımları izleyin:

1. [Hesap Merkezi](https://account.windowsazure.com/Subscriptions)' nde oturum açın.
1. Bir abonelik seçin. Harcama sınırına ulaşıldığından abonelik devre dışı bırakıldıysa, bildirime tıklayın: **Abonelik harcama sınırına ulaştı ve ücretleri engellemek için devre dışı bırakıldı.** Aksi takdirde, **ABONELIK durumu** alanında **harcama limitini kaldır** ' a tıklayın.
1. Size uygun bir seçenek belirleyin.

![Harcama limitini Kaldır seçeneği seçiliyor](./media/billing-spending-limit/remove-spending-limit.PNG)

| Seçenek | Etki |
| --- | --- |
| Harcama sınırını süresiz olarak kaldır | Harcama limiti kaldırılır ve bir sonraki fatura döneminin başlangıcında otomatik olarak yeniden açılmaz. |
| Geçerli fatura dönemi için harcama limitini kaldırma | Harcama limiti kaldırılır ve bir sonraki fatura döneminin başlangıcında otomatik olarak yeniden açılır. |

## <a name="why-you-might-want-to-remove-the-spending-limit"></a>Harcama limitini neden kaldırmak isteyebilirsiniz?

Harcama limiti, bazı üçüncü taraf ve Microsoft hizmetlerini dağıtmanızı veya kullanmanızı engelleyebilir. Aboneliğinizde harcama limitini kaldırmanız gereken durumlar aşağıda verilmiştir.

-  Oracle ve Azure DevOps Services gibi hizmetler gibi ilk taraf görüntülerini dağıtmayı planlarsınız. Bu durum, harcama limitinizi neredeyse hemen aşmanıza ve aboneliğinizin devre dışı bırakılmasına neden olur.
- Kesintiye uğramaması istemediğiniz hizmetleriniz var.
- Sanal IP adresleri gibi ayarlar içeren, kaybetmek istemediğiniz hizmet ve kaynaklarınız var. Bu ayarlar, hizmetler ve kaynaklar yeniden ayrıldığında kaybedilir.

## <a name="turn-on-the-spending-limit-after-removing"></a>Kaldırdıktan sonra harcama limitini aç

Bu özellik yalnızca harcama limiti süresiz olarak kaldırıldığında kullanılabilir. Bir sonraki fatura döneminin başlangıcında otomatik olarak açmak üzere değiştirin.

1. [Hesap Merkezi](https://account.windowsazure.com/Subscriptions)' nde oturum açın.
1. Harcama limiti seçeneğini değiştirmek için sarı başlık ' a tıklayın.
1. **Faturalama \<dönemininsonrakifaturadönemibaşlangıçtarihindeharcamalimitiniaç'ıseçin.\>**

## <a name="custom-spending-limit"></a>Özel harcama limiti

Özel harcama limitleri kullanılamıyor.

## <a name="a-spending-limit-doesnt-prevent-all-charges"></a>Harcama limiti tüm ücretleri engellemez

[Azure Marketi 'nde yayınlanan bazı dış hizmetler](billing-understand-your-azure-marketplace-charges.md) , abonelik kredilerinizle birlikte kullanılamaz ve harcama limitiniz ayarlanmış olsa bile ayrı ücretler doğurmaya tabi olabilir. Visual Studio lisansları, Azure Active Directory Premium, destek planları ve çoğu üçüncü taraf markalı Hizmetleri örnek olarak verilebilir. Yeni bir dış hizmet sağladığınızda, hizmetlerin ayrı olarak faturalandırıldığını bildiren bir uyarı gösterilir:

![Market satın alma uyarısı](./media/billing-understand-your-azure-marketplace-charges/marketplace-warning.PNG)

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız varsa, [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar
- [Kullandıkça Öde](billing-upgrade-azure-subscription.md) fiyatlandırmasıyla bir plana yükseltin.
