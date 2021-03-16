---
title: Hızlı başlangıç-Azure Iletişim hizmetlerinden bir telefon numarası alın
description: Azure portal kullanarak Iletişim Hizmetleri telefon numarası satın almayı öğrenin.
author: prakulka
manager: nmurav
services: azure-communication-services
ms.author: prakulka
ms.date: 03/10/2021
ms.topic: quickstart
ms.service: azure-communication-services
ms.custom: references_regions
ms.openlocfilehash: 390c804692029d7cdee8f78325b441592879582b
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2021
ms.locfileid: "103488593"
---
# <a name="quickstart-get-a-phone-number-using-the-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak telefon numarası alın

[!INCLUDE [Regional Availability Notice](../../includes/regional-availability-include.md)]

Telefon numarası satın almak için Azure portal kullanarak Azure Iletişim Hizmetleri ile çalışmaya başlayın.

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Etkin bir Iletişim Hizmetleri kaynağı.](../create-communication-resource.md)

## <a name="get-a-phone-number"></a>Telefon numarası alın

Numaraları sağlamaya başlamak için [Azure Portal](https://portal.azure.com)iletişim hizmetleri kaynağına gidin.

:::image type="content" source="../media/manage-phone-azure-portal-start.png" alt-text="Iletişim Hizmetleri kaynağının ana sayfasını gösteren ekran görüntüsü.":::

### <a name="getting-new-phone-numbers"></a>Yeni telefon numaraları alma

Kaynak menüsündeki **telefon numaraları** dikey penceresine gidin.

:::image type="content" source="../media/manage-phone-azure-portal-phone-page.png" alt-text="Iletişim Hizmetleri kaynağının telefon sayfasını gösteren ekran görüntüsü.":::

Sihirbazı başlatmak için **Al** düğmesine basın. **Telefon numaraları** dikey penceresindeki sihirbaz, senaryonuza en uygun telefon numarasını seçmenize yardımcı olacak bir dizi soru boyunca size yol gösterir.

İlk olarak, telefon numarasını sağlamak istediğiniz **ülkeyi/bölgeyi** seçmeniz gerekir. Ülke/bölge ' yi seçtikten sonra, gereksinimlerinizi en iyi şekilde karşılayacak **kullanım durumunu** seçmeniz gerekir.

:::image type="content" source="../media/manage-phone-azure-portal-get-numbers.png" alt-text="Telefon numaralarını al görünümünü gösteren ekran görüntüsü.":::

### <a name="select-your-phone-number-features"></a>Telefon numaranız özelliklerinizi seçin

Telefon numaranızı yapılandırmak iki adımda bölünmüştür:

1. [Sayı türünün](../../concepts/telephony-sms/plan-solution.md#phone-number-types-in-azure-communication-services) seçimi
2. [Sayı özelliklerinin](../../concepts/telephony-sms/plan-solution.md#phone-number-features-in-azure-communication-services) seçimi

İki telefon numarası türünden seçim yapabilirsiniz: **coğrafi** ve **ücretsiz.** Bir sayı türü seçtiğinizde, özelliği seçebilirsiniz.

Bizim örneğimizde, **giden çağrı** ve **gelen ve giden SMS** özellikleriyle **ücretsiz bir sayı** türü seçtik.

:::image type="content" source="../media/manage-phone-azure-portal-select-plans.png" alt-text="Özellik seç görünümünü gösteren ekran görüntüsü.":::

Buradan, sağlamak istediğiniz telefon numaralarını özelleştirmek için sayfanın alt kısmındaki **Sonraki: sayılar** düğmesine tıklayın.

### <a name="customizing-phone-numbers"></a>Telefon numaralarını özelleştirme

**Numaralar** sayfasında, sağlamak istediğiniz telefon numaralarını özelleştirecek.

:::image type="content" source="../media/manage-phone-azure-portal-select-numbers-start.png" alt-text="Sayılar seçim sayfasını gösteren ekran görüntüsü.":::

> [!NOTE]
> Bu hızlı **Başlangıç, ücretsiz numara türü** özelleştirme akışını gösterir. **Coğrafi** numara türünü seçtiyseniz deneyim biraz farklı olabilir, ancak nihai sonuç aynı olur.

Kullanılabilir alan kodları listesinden **alan kodunu** seçin ve sağlamak istediğiniz miktarı girin ve ardından seçilen gereksinimlerinizi karşılayan sayıları bulmak için **Ara** ' yı tıklatın. İhtiyaçlarınızı karşılayan telefon numaraları, aylık maliyetleriyle birlikte gösterilir.

:::image type="content" source="../media/manage-phone-azure-portal-found-numbers.png" alt-text="Ayrılmış sayılarla sayı seçim sayfasını gösteren ekran görüntüsü.":::

> [!NOTE]
> Kullanılabilirlik, seçtiğiniz sayı türüne, konuma ve özelliklere bağlıdır.
> Sayılar, işlemin süresi dolmadan kısa bir süre için ayrılır. İşlemin süresi dolarsa, numaraları yeniden seçmeniz gerekir.

Satın alma özetini görüntülemek ve siparişinizi yerleştirmek için sayfanın altındaki **İleri: Özet** düğmesine tıklayın.

### <a name="place-order"></a>Siparişi yerleştir

Özet sayfası, telefon numaralarını sağlamak için numara türünü, özellikleri, telefon numaralarını ve toplam aylık maliyeti gözden geçirir.

> [!NOTE]
> Gösterilen fiyatlar, seçilen telefon numarasının size kira maliyetini kapsayan **aylık yinelenen ücretlerdir** . Bu görünümde yer almadığınızda, çağrılar yaptığınızda veya alırken tahakkuk eden **Kullandıkça Öde maliyetlerinden** de yer verilir. Fiyat listelerine [buradan ulaşabilirsiniz](../../concepts/pricing.md). Bu ücretler, sayı türüne ve adlı hedeflere bağlıdır. Örneğin, Seattle bölgesel numarasından New York 'daki bölgesel bir sayıya yapılan çağrı için dakika başına fiyatı ve bir UK cep telefonu numarasına aynı sayıdan yapılan bir çağrı farklı olabilir.

Son olarak, onaylamak için sayfanın alt kısmındaki **siparişi yerleştir** ' e tıklayın.

:::image type="content" source="../media/manage-phone-azure-portal-get-numbers-summary.png" alt-text="Özet sayfasını gösteren ve sayı türü, özellikler, telefon numarası ve toplam aylık maliyetin gösterildiği ekran görüntüsü.":::

## <a name="find-your-phone-numbers-on-the-azure-portal"></a>Telefon numaralarınızı Azure portal bulun

[Azure Portal](https://portal.azure.com)Azure iletişim kaynağına gidin:

:::image type="content" source="../media/manage-phone-azure-portal-start.png" alt-text="Iletişim Hizmetleri kaynağının ana sayfasını gösteren ekran görüntüsü.":::

Telefon numaralarınızı yönetmek için menüdeki telefon numaraları dikey penceresini seçin.

:::image type="content" source="../media/manage-phone-azure-portal-phones.png" alt-text="Iletişim Hizmetleri kaynağının telefon numarası sayfasını gösteren ekran görüntüsü.":::

> [!NOTE]
> Sağlanan sayıların bu sayfada gösterilmesi birkaç dakika sürebilir.


### <a name="customizing-phone-numbers"></a>Telefon numaralarını özelleştirme

**Numaralar** sayfasında, yapılandırmak için bir telefon numarası seçebilirsiniz.

:::image type="content" source="../media/manage-phone-azure-portal-capability-update.png" alt-text="Güncelleştirme özellikleri sayfasını gösteren ekran görüntüsü.":::

Kullanılabilir seçeneklerden özellikleri seçin, sonra Seçiminizi uygulamak için **Onayla** ' ya tıklayın.

## <a name="troubleshooting"></a>Sorun giderme

Yaygın sorular ve sorunlar:

- Satın alma telefonu yalnızca ABD 'de desteklenmektedir. Telefon numaralarını satın almak için şunları doğrulayın:
  - İlişkili Azure aboneliği faturalandırma adresi Birleşik Devletler bulunmaktadır. Şu anda bir kaynağı başka bir aboneliğe taşıyamazsınız.
  - Iletişim Hizmetleri kaynağınız Birleşik Devletler veri konumunda sağlanır. Kaynağı şu anda başka bir veri konumuna taşıyamazsınız.

- Telefon numarası bırakıldığında, telefon numarası serbest bırakılır veya faturalandırma döngüsünün sonuna kadar yeniden kullanılamaz.

- Bir Iletişim Hizmetleri kaynağı silindiğinde, bu kaynakla ilişkilendirilmiş telefon numaraları otomatik olarak aynı anda serbest bırakılır.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta şunları öğrenirsiniz:

> [!div class="checklist"]
> * Telefon numarası satın alın
> * Telefon numaranızı yönetin
> * Telefon numarası yayınlama

> [!div class="nextstepaction"]
> [SMS gönder](../telephony-sms/send.md) 
>  [Çağırma ile çalışmaya başlayın](../voice-video-calling/getting-started-with-calling.md)
