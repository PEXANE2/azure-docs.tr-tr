---
title: Red Hat rezervasyon planı indirimleri - Azure
description: Red Hat planı indirimlerinin sanal makinelerde Red Hat yazılımına nasıl uygulandığını öğrenin.
author: yashesvi
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 06/11/2020
ms.author: banders
ms.openlocfilehash: 46a1fd9be35abb19d920e2a3bd34f88c557f40b1
ms.sourcegitcommit: c4ad4ba9c9aaed81dfab9ca2cc744930abd91298
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84735124"
---
# <a name="understand-how-the-red-hat-linux-enterprise-software-reservation-plan-discount-is-applied-for-azure"></a>Red Hat Linux Enterprise yazılım rezervasyon planı indiriminin Azure için nasıl uygulandığını anlayın

Red Hat Linux planı satın almanızın ardından indirim, rezervasyonla eşleşen, dağıtılan Red Hat sanal makinelerine otomatik olarak uygulanır. Red Hat Linux planı, Red Hat yazılımını bir Azure sanal makinesinde çalıştırma maliyetini kapsar.

Doğru Red Hat Linux planını satın almak için hangi Red Hat sanal makinelerini çalıştırdığınızı ve bu sanal makinelerdeki vCPU sayısını anlamanız gerekir. Kullanım bilgilerini içeren CSV dosyanızdan, hangi planın satın alınacağını belirlemenize yardımcı olması için aşağıdaki bölümleri kullanın.

## <a name="discount-applies-to-different-vm-sizes"></a>Farklı sanal makine boyutları için indirim uygulanır

Ayrılmış Sanal Makine Örnekleri gibi Red Hat planı satın alımları da örnek boyutu esnekliği sunar. Başka bir deyişle, farklı bir vCPU sayısıyla bir sanal makine dağıttığınızda bile indiriminiz geçerli olur. Yazılım planı içindeki farklı sanal makine boyutları için indirim geçerlidir.

İndirim tutarı, aşağıdaki tablolarda listelenen orana bağlıdır. Bu oran, söz konusu gruptaki her VM boyutunun göreli ayak izini karşılaştırır. Bu oran, sanal makine vCPU’larına bağlıdır. Kaç tane sanal makine örneğinin Red Hat Linux planı indirimi aldığını hesaplamak için oran değerini kullanın.

Örneğin, 1 veya 4 vCPU içeren bir sanal makine için Red Hat Linux Enterprise Server planı satın alırsanız, bu rezervasyon için oran 1 olur. İndirim, şunlar için Red Hat yazılımı maliyetini kapsar:

- 1 veya 4 vCPU ile dağıtılan 1 sanal makine,
- ya da 5 veya daha fazla vCPU bulunan bir sanal makinenin Red Hat Enterprise Linux maliyetlerinin 0,46'sı veya yaklaşık %46'sı.

### <a name="red-hat-enterprise-linux"></a>Red Hat Enterprise Linux

Azure portalı market adları:
- Red Hat Enterprise Linux 6.7
- Red Hat Enterprise Linux 6.8
- Red Hat Enterprise Linux 6.9
- Red Hat Enterprise Linux 6.10
- Red Hat Enterprise Linux 7
- Red Hat Enterprise Linux 7.2
- Red Hat Enterprise Linux 7.3
- Red Hat Enterprise Linux 7.4
- Red Hat Enterprise Linux 7.5
- Red Hat Enterprise Linux 7.6
- Red Hat Enterprise Linux 8.2

[Planın uygulandığı Red Hat Enterprise Linux ölçümlerini denetleyin](https://isfratio.blob.core.windows.net/isfratio/RHELRatios.csv)

## <a name="next-steps"></a>Sonraki adımlar

Rezervasyonlar hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure rezervasyonları nedir?](save-compute-costs-reservations.md)
- [Azure rezervasyonları ile Red Hat yazılım planları için ön ödeme yapma](../../virtual-machines/linux/prepay-rhel-software-charges.md)
- [Azure Ayrılmış VM Örnekleri ile Sanal Makinelere ön ödeme yapma](../../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure rezervasyonlarını yönetme](manage-reserved-vm-instance.md)
- [Kullandıkça Öde aboneliğiniz için rezervasyon kullanımını anlama](understand-reserved-instance-usage.md)
- [Kurumsal kaydınız için rezervasyon kullanımını anlama](understand-reserved-instance-usage-ea.md)

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
