---
title: Yazılım planları için ön ödeme - Azure Rezervasyonları
description: Kullanabileceğiniz kadar öde maliyetleriniz üzerinden tasarruf etmek için yazılım planları için nasıl ön ödeme yapabileceğinizi öğrenin.
author: bandersmsft
manager: yashesvi
ms.service: virtual-machines-linux
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/06/2020
ms.author: banders
ms.openlocfilehash: 3e05920e495dd4aa14be6c849590a37a2bafa33f
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80757239"
---
# <a name="prepay-for-azure-software-plans"></a>Azure yazılım planları için ön ödeme yapma

Azure'da SUSE ve RedHat yazılım kullanımınız için ön ödeme yaptığınızda, kullandıkça öde maliyetleriniz üzerinden tasarruf edebilirsiniz. İndirimler sanal makine kullanımında değil, yalnızca SUSE ve RedHat sayaçları için geçerlidir. Ek tasarruf için sanal makineler için rezervasyonları ayrı ayrı satın alabilirsiniz.

Azure portalından SUSE ve RedHat yazılım planlarını satın alabilirsiniz. Bir plan satın almak için:

- En az bir Enterprise veya tek tek abonelik için sahip rolünün olması gerekir.
- Kurumsal abonelikler için, [EA portal](https://ea.azure.com/)’da **Ayrılmış Örnek Ekle** seçeneği etkinleştirilmelidir. Ayar devre dışı bırakılmışsa, abonelik için bir EA Yöneticisi olmalısınız.
- Bulut Çözüm Sağlayıcısı (CSP) programı için, yönetici aracılar veya satış temsilcileri yazılım planlarını satın alabilir.

## <a name="buy-a-software-plan"></a>Yazılım planı satın alma

1. Azure portalında oturum açın ve [Rezervasyonlar'a](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade)gidin.
2. **Ekle'yi** tıklatın ve ardından satın almak istediğiniz yazılım planını seçin.
Gerekli alanları doldurun. Satın aldığınız özellikleriyle eşleşen herhangi bir SUSE Linux VM veya RedHat VM indirim alır. İndirimi alan dağıtımların gerçek sayısı, seçilen kapsam ve miktara bağlıdır.
3. Bir abonelik seçin. Planın parasını ödemek için kullanılır.
Abonelik ödeme yöntemi, rezervasyon için peşin maliyetler tahsil edilir. Abonelik türü bir Kurumsal Sözleşme (teklif numaraları: MS-AZR-0017P veya MS-AZR-0148P) veya gidilecek kadar öde fiyatlandırması (teklif numaraları: MS-AZR-0003P veya MS-AZR-0023P) ile bireysel anlaşma olmalıdır.
    - Kurumsal abonelik için ücretler kaydın maddi işlem bakiyesinden düşülür ve fazla kullanım olarak ücretlendirilir.
    - Gider olarak öde fiyatlandırmasına sahip tek bir abonelik için, ücretler aboneliğin kredi kartına veya fatura ödeme yöntemine faturalandırılır.
4. Bir kapsam seçin. Kapsam, bir aboneliği veya birden çok aboneliği (paylaşılan kapsam) kapsayabilir.
    - Tek abonelik - Plan iskontosu abonelikteki eşleşen kullanıma uygulanır.
    - Paylaşılan - Plan iskontosu, faturalandırma bağlamınızdaki herhangi bir abonelikteki eşleşen örneklere uygulanır. Kurumsal müşteriler için faturalandırma bağlamı kayıttır ve kayıttaki tüm abonelikleri içerir. Gittiniz gibi öde fiyatlandırma müşterileri ile bireysel plan için, faturalandırma bağlamı, hesap yöneticisi tarafından oluşturulan gittikçe öde fiyatlandırma abonelikleri içeren tüm bireysel planlardır.
    - Tek kaynak grubu - Rezervasyon iskontosu yalnızca seçili kaynak grubundaki eşleşen kaynaklara uygulanır.
5. VM boyutunu ve görüntü türünü seçmek için bir ürün seçin. İndirim yalnızca seçili VM boyutu için geçerlidir.
6. Bir veya üç yıllık bir dönem seçin.
7. Faturalandırma indirimini alabilecek ön ödemeli VM örneklerinin sayısı olan bir miktar seçin.
8. Ürünü sepete ekleyin, gözden geçirin ve satın alın.

Rezervasyon indirimi, ön ödeme yaptığınız yazılım sayacına otomatik olarak uygulanır. VM işlem ücretleri plan kapsamında değildir. VM rezervasyonlarını ayrı olarak satın alabilirsiniz.

## <a name="discount-applies-to-different-suse-vm-sizes"></a>İndirim farklı SUSE VM boyutları için geçerlidir

Ayrılmış VM örnekleri gibi, SUSE Linux planları örnek boyutu esnekliği sunar. İndiriminiz, satın aldığınız SUSE planından farklı boyutta bir VM dağıtsanız bile geçerlidir. Daha fazla bilgi için [bkz.](../../cost-management-billing/reservations/understand-suse-reservation-charges.md)

## <a name="redhat-plan-discount"></a>RedHat planı indirimi

Planlar sadece Red Hat Enterprise Linux sanal makineleri için kullanılabilir. İndirim RedHat Enterprise Linux SAP HANA VMs veya RedHat Enterprise Linux SAP Business Apps VM'ler için geçerli değildir.

RedHat planı indirimleri yalnızca satın alma sırasında seçtiğiniz VM boyutu için geçerlidir. RHEL planları satın alındıktan sonra iade edilemez veya değiştirilmez.


## <a name="cancellation-and-exchanges-not-allowed"></a>İptal ve değişime izin verilmiyor

Satın aldığınız bir SUSE veya RedHat planını iptal edemez veya değiştiremezsiniz. Kullanımınızı denetleyerek doğru planı satın aldığınızdan emin olun. Ne satın alınacağı konusunda yardım almak için [bkz.](../../cost-management-billing/reservations/understand-suse-reservation-charges.md)

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa veya yardıma ihtiyacınız varsa, [bir destek isteği oluşturun.](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)

## <a name="next-steps"></a>Sonraki adımlar

Rezervasyonu nasıl yöneteceklerini öğrenmek için Azure [rezervasyonlarını yönet'e](../../cost-management-billing/reservations/manage-reserved-vm-instance.md)bakın.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure Rezervasyonları nedir?](../../cost-management-billing/reservations/save-compute-costs-reservations.md)
- [Azure’da Rezervasyonları Yönetme](../../cost-management-billing/reservations/manage-reserved-vm-instance.md)
- [SUSE rezervasyon indiriminin nasıl uygulandığını anlama](../../cost-management-billing/reservations/understand-suse-reservation-charges.md)
- [Kullandıkça Öde aboneliğiniz için rezervasyon kullanımını anlama](../../cost-management-billing/reservations/understand-reserved-instance-usage.md)
- [Kurumsal kaydınız için rezervasyon kullanımını anlama](../../cost-management-billing/reservations/understand-reserved-instance-usage-ea.md)
