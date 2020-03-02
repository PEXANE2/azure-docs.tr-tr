---
title: Azure Kurumsal Anlaşma faturanızı gözden geçirme
description: Azure Kurumsal Anlaşmaları’nın kullanımını ve faturasını okuma ve anlama hakkında bilgi edinin.
author: bandersmsft
ms.reviewer: amberb
tags: billing
ms.service: cost-management-billing
ms.topic: tutorial
ms.date: 02/24/2020
ms.author: banders
ms.openlocfilehash: 017f617bff14eb0c031c40240ca6c1ac62c9c941
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77598230"
---
# <a name="understand-your-azure-enterprise-agreement-bill"></a>Azure Kurumsal Anlaşma faturanızı anlama

Kurumsal Anlaşması olan Azure müşterileri, kuruluşun kredisini aştığında veya kredi kapsamında olmayan hizmetleri kullandığında bir fatura alır.

Kuruluşunuzun kredisi, parasal taahhüdünüzü kapsar. Parasal taahhüt, kuruluşunuzun Azure hizmetleri kullanımı için peşin ödediği tutardır. Microsoft hesap yöneticinizle veya kurumsal bayinizle iletişim kurarak Kurumsal Anlaşmanıza parasal taahhüt fonları ekleyebilirsiniz.

Bu öğretici yalnızca Azure Kurumsal Anlaşma sahibi olan Azure müşterileri için geçerlidir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Faturalanmış ücretleri gözden geçirme
> * Hizmet fazla kullanım ücretlerini gözden geçirme
> * Market faturasını inceleme

## <a name="prerequisites"></a>Ön koşullar

Faturanızdaki ücretleri gözden geçirip doğrulamak için Kurum Yöneticisi olmanız gerekir. Daha fazla bilgi için bkz. [Azure’daki Azure Kurumsal Anlaşma yönetici rollerini anlama](../manage/understand-ea-roles.md). Kuruluşunuz için kimin Kurum Yöneticisi olduğunu bilmiyorsanız [desteğe başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="review-invoiced-charges-for-most-customers"></a>Çoğu müşteri için faturalanmış ücretleri gözden geçirme

Bu bölüm, Avustralya, Japonya veya Singapur’daki Azure müşterileri için geçerli değildir.

Faturalama döneminizde aşağıdakilerden herhangi biri gerçekleştiğinde bir Azure faturası alırsınız:

- **Hizmet fazla kullanımı**: Kuruluşunuzun kullanım ücretleri, kredi bakiyenizi aştığında.
- **Ayrı olarak faturalandırılan ücretler**: Kuruluşunuzun kullandığı hizmetler, kredi kapsamında olmadığında. Kredi bakiyeniz ne olursa olsun aşağıdaki hizmetler için faturalandırılırsınız:
    - Canonical
    - Citrix XenApp Essentials
    - Citrix XenDesktop
    - Kayıtlı Kullanıcı
    - Openlogic
    - Uzaktan Erişim Hakları XenApp Essentials Kayıtlı Kullanıcı
    - Ubuntu Advantage
    - Visual Studio Enterprise (Aylık)
    - Visual Studio Enterprise (Yıllık)
    - Visual Studio Professional (Aylık)
    - Visual Studio Professional (Yıllık)
- **Market ücretleri**: Azure Market satın alma işlemleri ve kullanımı, kuruluşunuzun kredi kapsamında yer almaz. Bu nedenle, kredi bakiyeniz ne olursa olsun Pazar ücretleri için faturalandırılırsınız. Enterprise Portal’da bir Kurumsal Yönetici, Pazar satın alma işlemlerini etkinleştirebilir ve devre dışı bırakabilir.

Faturanızda önce Azure kullanım ücretleri ve bunlarla ilişkili maliyetler, ardından da market ücretleri gösterilir. Kredi bakiyeniz varsa, bu bakiye Azure kullanımınıza uygulanır ve faturanızda maliyeti olmayan Azure kullanımı ve market kullanımı en son gösterilir.

Enterprise Portal’da **Raporlar** > **Kullanım Özeti** bölümünde gösterilen birleşik toplam tutarı, Azure faturanızla karşılaştırın. **Kullanım Özeti**’ndeki tutarlara vergiler dahil değildir.

[Azure EA portalında](https://ea.azure.com) oturum açın. Ardından **Raporlar**’ı seçin. Sekmenin sağ üst köşesinde**M** görünümünü **C** olarak değiştirin ve fatura dönemine göre ayarlayın.  

![Kullanım özetinde M + C seçeneğini gösteren ekran görüntüsü.](./media/review-enterprise-agreement-bill/ea-portal-usage-sumary-cm-option.png)

**Toplam Kullanım** ve **Azure Market** birleşik tutarı, faturanızdaki **Toplam Genişletilmiş Tutar** ile aynı olmalıdır. Ücretlerinizle ilgili daha fazla ayrıntı almak için **Kullanım Bilgilerini İndirme** bölümüne gidin.  

![Kullanımı İndir sekmesini gösteren ekran görüntüsü](./media/review-enterprise-agreement-bill/ea-portal-download-usage.png)

## <a name="review-invoiced-charges-for-other-customers"></a>Diğer müşteriler için faturalanmış ücretleri gözden geçirme

Bu bölüm yalnızca Avustralya, Japonya veya Singapur’daki Azure müşterileri için geçerlidir.

Aşağıdaki olaylardan herhangi biri gerçekleştiğinde bir veya birden fazla Azure faturası alırsınız:

- **Hizmet fazla kullanımı**: Kuruluşunuzun kullanım ücretleri, kredi bakiyenizi aştığında.
- **Ayrı olarak faturalandırılan ücretler**: Kuruluşunuzun kullandığı hizmetler, kredi kapsamında olmadığında. Aşağıdaki hizmetler için faturalandırılırsınız:
    - Canonical
    - Citrix XenApp Essentials
    - Citrix XenDesktop
    - Kayıtlı Kullanıcı
    - Openlogic
    - Uzaktan Erişim Hakları XenApp Essentials Kayıtlı Kullanıcı
    - Ubuntu Advantage
    - Visual Studio Enterprise (Aylık)
    - Visual Studio Enterprise (Yıllık)
    - Visual Studio Professional (Aylık)
    - Visual Studio Professional (Yıllık)
- **Market ücretleri**: Azure Market satın alma işlemleri ve kullanımı, kuruluşunuzun kredi kapsamında yer almaz ve ayrı olarak faturalandırılır. Enterprise Portal’da bir Kurumsal Yönetici, Pazar satın alma işlemlerini etkinleştirebilir ve devre dışı bırakabilir.

Hizmet fazla kullanımı için ödenmesi gereken ücretleriniz ve faturalama döneminde ayrı olarak faturalandırılan ücretleriniz varsa tek bir fatura alırsınız. Bu, her iki masraf türünü de içerir. Pazar ücretleri her zaman ayrı olarak faturalandırılır.

## <a name="review-service-overage-charges-for-other-customers"></a>Diğer müşteriler için hizmet fazla kullanım ücretlerini gözden geçirme

Bu bölüm yalnızca Avustralya, Japonya veya Singapur’daysanız geçerlidir.

Enterprise Portal’da **Raporlar** > **Kullanım Özeti** bölümündeki toplam kullanım tutarınızı, hizmet fazla kullanım faturanızla karşılaştırın. Hizmet fazla kullanım faturası, kuruluşunuzun kredisini aşan kullanımı ve/veya kredi kapsamında olmayan hizmetleri içerir. **Kullanım Özeti**’ndeki tutarlara vergiler dahil değildir.

[Azure EA portalında](https://ea.azure.com) oturum açın ve **Raporlar**'ı seçin. Sekmenin sağ üst köşesinde**M** görünümünü **C** olarak değiştirin ve fatura dönemine göre ayarlayın.  

![Kullanım özetinde M + C seçeneğini gösteren ekran görüntüsü.](./media/review-enterprise-agreement-bill/ea-portal-usage-sumary-cm-option.png)

**Toplam Kullanım** tutarı, hizmet fazla kullanım faturanızdaki **Toplam Genişletilmiş Tutar** ile aynı olmalıdır. Ücretlerinizle ilgili daha fazla bilgi edinmek için **Kullanımı İndir** > **Gelişmiş Rapor İndirme** bölümüne gidin. Vergiler, rezervasyon ücretleri veya market ücretleri rapora dahil değildir.  

![Kullanımı indir sekmesinde Gelişmiş rapor İndirme’yi gösteren ekran görüntüsü.](./media/review-enterprise-agreement-bill/ea-portal-download-usage-advanced.png)

Aşağıdaki tabloda, Enterprise Portal’daki **Kullanım Özeti**’nde ve faturada gösterilen terimler ve açıklamalar listelenmektedir:

|Fatura terimi|Kullanım Özeti terimi|Açıklama|
|---|---|---|
|Toplam Genişletilmiş Tutar|Toplam Kullanım|Kredi uygulanmadan önceki belirli dönem için toplam vergi öncesi kullanım ücreti.|
|Taahhüt Kullanımı|Taahhüt Kullanımı|Bu belirli dönemde uygulanan kredi.|
|Toplam Satış|Toplam Fazla Kullanım|Kredi tutarınızı aşan toplam kullanım ücreti. Bu tutara vergi dahil değildir.|
|Vergi Tutarı|Uygulanamaz|Belirli bir dönem için toplam satış tutarına uygulanan vergi.|
|Vergi Tutarı|Uygulanamaz|Kredi uygulandıktan ve vergi eklendikten sonra fatura için ödenmesi gereken tutar.|

### <a name="review-marketplace-invoice"></a>Market faturasını inceleme

Bu bölüm yalnızca Avustralya, Japonya veya Singapur’daysanız geçerlidir.

Enterprise Portal’daki **Raporlar** > **Kullanım Özeti** bölümündeki Azure Market toplamınızı market faturanızla karşılaştırın. Market faturasında yalnızca Azure Market satın alma işlemleri ve kullanımı bulunur. **Kullanım Özeti**’ndeki tutarlara vergiler dahil değildir.

[Enterprise Portal](https://ea.azure.com)'da oturum açın ve **Raporlar**'ı seçin. Sekmenin sağ üst köşesinde**M** görünümünü **C** olarak değiştirin ve fatura dönemine göre ayarlayın.  

![Kullanım özetinde M + C seçeneğini gösteren ekran görüntüsü.](./media/review-enterprise-agreement-bill/ea-portal-usage-sumary-cm-option.png)  

**Azure Market** toplamı, market faturanızdaki **Toplam Satış** ile aynı olmalıdır. Kullanım temelli ücretlerinizle ilgili daha fazla bilgi edinmek için**Kullanımı İndir** bölümüne gidin. **Market Ücretleri** bölümünde **İndir**’i seçin. Market fiyatı, yayımcı tarafından belirlenen bir vergiyi içerir. Müşteriler, yayımcıdan işlemle ilgili verginin tahsil edilmesine ilişkin ayrı bir fatura almaz.

![Market ücretleri bölümünde indirme seçeneğini gösteren ekran görüntüsü.](./media/review-enterprise-agreement-bill/ea-portal-download-usage-marketplace.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Faturalanmış ücretleri gözden geçirme
> * Hizmet fazla kullanım ücretlerini gözden geçirme
> * Market faturasını inceleme

Azure EA portalı hakkında daha fazla bilgi edinmek için bir sonraki makaleye geçin.

> [!div class="nextstepaction"]
> [Azure EA portalı ile çalışmaya başlama](../manage/ea-portal-get-started.md)
