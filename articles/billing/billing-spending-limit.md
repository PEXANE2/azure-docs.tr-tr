---
title: Azure harcama limiti | Microsoft Docs
description: Bu makalede, Azure harcama limitinin nasıl çalıştığı ve nasıl kaldırıldığı açıklanmaktadır.
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
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70114444"
---
# <a name="azure-spending-limit"></a>Azure harcama limiti

Azure’daki harcama limiti, harcamanızın kredi tutarınızın limitini aşmasını engeller. Azure deneme sürümüne veya birden çok aya yayılan kredi içeren tekliflere kaydolan tüm yeni müşterilerin harcama limiti, varsayılan olarak açık olacaktır. Harcama limiti 0 ABD dolarıdır ve değiştirilemez. Örneğin, harcama limitini 100 ABD doları olarak değiştiremezsiniz. Ancak harcama limitini kaldırabilirsiniz. Bu nedenle, ya hiç limit yoktur ya da çoğu harcamaları yapmanızı engelleyen sıfır limitine sahip olursunuz. Harcama limiti, taahhüt planları ve kullandıkça öde fiyatlandırmasına tabi planlar gibi bazı planlar kapsamındaki abonelikler için kullanılamaz. [Tüm Azure tekliflerini ve harcama limiti kullanılabilirliğini gösteren listeye](https://azure.microsoft.com/support/legal/offer-details/) göz atın.

## <a name="reaching-a-spending-limit"></a>Harcama limitine ulaşma

Kullanımınız için, Azure aboneliğinize dahil olan aylık tutarları tüketen bir ücret ödemeniz gerekirse dağıttığınız hizmetler, ilgili faturalama döneminin kalan bölümünde devre dışı bırakılır.

Örneğin, aboneliğinize dahil edilen tüm kredileri harcadığınızda, dağıttığınız Azure kaynakları üretimden kaldırılır ve Azure sanal makineleriniz durdurulup serbest bırakılır. Depolama hesaplarınızdaki veriler salt okunur olarak kullanılabilir.

Bir sonraki faturalama döneminin başlangıcında abonelik teklifiniz birkaç aya yayılan krediler içeriyorsa, aboneliğiniz otomatik olarak yeniden etkinleştirilir. Daha sonra Azure kaynaklarınızı yeniden dağıtabilir ve depolama hesaplarınıza ve veritabanlarınıza tam erişim elde edebilirsiniz.

Aboneliğiniz için harcama limitine ulaştığınızda Azure e-posta bildirimleri gönderir. Harcama limitine ulaşan abonelikler hakkında bildirimler görmek için [Hesap Merkezi](https://account.windowsazure.com/Subscriptions)’nde oturum açın.

Ücretsiz deneme aboneliğiniz varsa ve harcama limitine ulaşırsanız, harcama limitini kaldırmak ve aboneliği otomatik olarak etkinleştirmek için [kullandıkça öde](billing-upgrade-azure-subscription.md) fiyatlandırmasına tabi bir plana yükseltme yapabilirsiniz.

<a id="remove"></a>

## <a name="remove-the-spending-limit-in-account-center"></a>Hesap Merkezi’nde harcama limitini kaldırma

Azure aboneliğinizle ilişkili geçerli bir ödeme yöntemi olduğu sürece, harcama limitini istediğiniz zaman kaldırabilirsiniz. Birden çok aya yayılan kredi içeren tekliflerde, bir sonraki faturalama döneminizin başlangıcında da harcama limitini etkinleştirebilirsiniz.

Harcama limitinizi kaldırmak için şu adımları izleyin:

1. [Hesap Merkezi](https://account.windowsazure.com/Subscriptions)’nde oturum açın.
1. Bir abonelik seçin. Harcama limitine ulaşıldığı için abonelik devre dışı bırakılmışsa şu bildirime tıklayın: **Abonelik Harcama Limitine ulaştı ve ücret yazılmasını önlemek için devre dışı bırakıldı.** Aksi takdirde **ABONELİK DURUMU** alanında **Harcama limitini kaldır**’a tıklayın.
1. Size uygun bir seçenek belirleyin.

![Harcama limitini kaldırma seçeneğini belirleme](./media/billing-spending-limit/remove-spending-limit.PNG)

| Seçenek | Etki |
| --- | --- |
| Harcama limitini süresiz olarak kaldır | Harcama limiti kaldırılır ve bir sonraki faturalama döneminin başlangıcında otomatik olarak yeniden açılmaz. |
| Geçerli fatura dönemi için harcama limitini kaldır | Harcama limiti kaldırılır ve bir sonraki fatura döneminin başlangıcında otomatik olarak yeniden açılır. |

## <a name="why-you-might-want-to-remove-the-spending-limit"></a>Harcama limitini neden kaldırmak isteyebilirsiniz?

Harcama limiti, belirli üçüncü taraf ve Microsoft hizmetlerini dağıtmanızı veya kullanmanızı engelleyebilir. Aboneliğinizdeki harcama limitini kaldırmanızı gerektiren senaryolar aşağıda belirtilmiştir.

-  Oracle gibi birinci taraf görüntülerini ve Azure DevOps Services gibi hizmetleri dağıtmayı planlıyorsunuz. Bu durumda, harcama limitiniz neredeyse anında aşılacak ve bunun sonucunda aboneliğiniz devre dışı bırakılacaktır.
- Kesintiye uğramasını istemediğiniz hizmetleriniz var.
- Sanal IP adresleri gibi ayarlar içeren, kaybetmek istemediğiniz hizmet ve kaynaklarınız var. Hizmetler ve kaynaklar serbest bırakıldığında bu ayarlar kaybolur.

## <a name="turn-on-the-spending-limit-after-removing"></a>Kaldırdıktan sonra harcama limitini açma

Bu özellik yalnızca harcama limiti süresiz olarak kaldırıldığında kullanılabilir. Bir sonraki faturalama döneminin başlangıcında otomatik olarak açmak için bu özelliği değiştirin.

1. [Hesap Merkezi](https://account.windowsazure.com/Subscriptions)’nde oturum açın.
1. Harcama limiti seçeneğini değiştirmek için sarı başlığa tıklayın.
1. **Sonraki faturalama döneminde (\<faturalama döneminin başlangıç tarihi\>) harcama limitini aç** seçeneğini belirleyin

## <a name="custom-spending-limit"></a>Özel harcama limiti

Özel harcama limitleri kullanılamaz.

## <a name="a-spending-limit-doesnt-prevent-all-charges"></a>Harcama limiti tüm ücretleri engellemez

[Azure Market’te yayımlanan bazı dış hizmetler](billing-understand-your-azure-marketplace-charges.md), abonelik kredilerinizle kullanılamaz ve harcama limitiniz ayarlanmış olduğunda bile ayrıca ücretlere neden olabilir. Visual Studio lisansları, Azure Active Directory Premium, destek planları ve üçüncü taraf markalı hizmetlerin çoğu örnek olarak gösterilebilir. Yeni bir dış hizmet sağladığınızda, hizmetlerin ayrı olarak faturalandığını bildiren bir uyarı gösterilir:

![Market satın alma uyarısı](./media/billing-understand-your-azure-marketplace-charges/marketplace-warning.PNG)

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar
- [Kullandıkça öde](billing-upgrade-azure-subscription.md) fiyatlandırmasına tabi bir plana yükseltin.
