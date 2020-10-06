---
title: Hızlı başlangıç-Azure Iletişim hizmetlerinden bir telefon numarası alın
description: Azure portal kullanarak Iletişim Hizmetleri telefon numarası satın almayı öğrenin.
author: prakulka
manager: nmurav
services: azure-communication-services
ms.author: prakulka
ms.date: 10/05/2020
ms.topic: quickstart
ms.service: azure-communication-services
ms.custom: references_regions
ms.openlocfilehash: e06c3720e180c1dc4fa2f227fd86d15cbbb0ff33
ms.sourcegitcommit: 6a4687b86b7aabaeb6aacdfa6c2a1229073254de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91756980"
---
# <a name="quickstart-get-a-phone-number-using-the-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak telefon numarası alın

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]

Telefon numarası satın almak için Azure portal kullanarak Azure Iletişim Hizmetleri ile çalışmaya başlayın.

## <a name="prerequisites"></a>Ön koşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Etkin bir Iletişim Hizmetleri kaynağı.](../create-communication-resource.md)

## <a name="get-a-phone-number"></a>Telefon numarası alın

Numaraları sağlamaya başlamak için [Azure Portal](https://portal.azure.com)iletişim hizmetleri kaynağına gidin.

:::image type="content" source="../media/manage-phone-azure-portal-start.png" alt-text="Iletişim Hizmetleri kaynağının ana sayfasını gösteren ekran görüntüsü.":::

### <a name="getting-new-phone-numbers"></a>Yeni telefon numaraları alma

Kaynak menüsündeki telefon numaraları dikey penceresine gidin.

:::image type="content" source="../media/manage-phone-azure-portal-phone-page.png" alt-text="Iletişim Hizmetleri kaynağının ana sayfasını gösteren ekran görüntüsü.":::

`Get`Sihirbazı başlatmak için düğmeye basın. Dikey penceredeki sihirbaz, `Phone numbers` senaryonuza en uygun telefon numarasını seçmenize yardımcı olacak bir dizi soru boyunca size yol gösterir. 

İlk olarak `Country/region` telefon numarasını sağlamak istediğiniz yeri seçmeniz gerekir. Ülke/bölge ' yi seçtikten sonra, `use case` gereksinimlerinize en uygun paketleri seçmeniz gerekir. 

:::image type="content" source="../media/manage-phone-azure-portal-get-numbers.png" alt-text="Iletişim Hizmetleri kaynağının ana sayfasını gösteren ekran görüntüsü.":::

### <a name="select-a-phone-plan"></a>Telefon planı seçin

Telefon planının seçilmesi iki adıma ayrılmıştır: 

1. [Sayı türünün](../../concepts/telephony-sms/plan-solution.md#phone-number-types-in-azure-communication-services) seçimi
2. [Planın](../../concepts/telephony-sms/plan-solution.md#phone-number-plans-in-azure-communication-services) seçimi

Şu anda iki sayı türü sunuyoruz: `Geographic` , ve `Toll-free` . Bir sayı türü seçtiğinizde, aralarından seçim yapabileceğiniz birkaç plan sunulacaktır.

Bizim örneğimizde, `Toll-free` `Outbound calling` ve planlarla bir sayı türü seçtik `Inbound and Outbound SMS` .

:::image type="content" source="../media/manage-phone-azure-portal-select-plans.png" alt-text="Iletişim Hizmetleri kaynağının ana sayfasını gösteren ekran görüntüsü.":::

Buradan, `Next: Numbers` sağlamak istediğiniz telefon numaralarını özelleştirmek için sayfanın altındaki düğmeye tıklayın.

### <a name="customizing-phone-numbers"></a>Telefon numaralarını özelleştirme

`Numbers`Sayfasında, sağlamak istediğiniz telefon numaralarını özelleştirecek.

:::image type="content" source="../media/manage-phone-azure-portal-select-numbers-start.png" alt-text="Iletişim Hizmetleri kaynağının ana sayfasını gösteren ekran görüntüsü.":::

> [!NOTE]
> Bu hızlı başlangıç, `Toll-free` sayı türü özelleştirme akışını gösteriyor. Sayı türünü seçtiyseniz deneyim biraz farklı olabilir `Geographic` , ancak nihai sonuç aynı olur.

`Area code`Kullanılabilir alan kodları listesinden öğesini seçin ve sağlamak istediğiniz miktarı girip, `Search` seçtiğiniz gereksinimleri karşılayan sayıları bulmak için tıklayın. İhtiyaçlarınızı karşılayan telefon numaraları, aylık maliyetleriyle birlikte gösterilir.

:::image type="content" source="../media/manage-phone-azure-portal-found-numbers.png" alt-text="Iletişim Hizmetleri kaynağının ana sayfasını gösteren ekran görüntüsü.":::

> [!NOTE]
> Kullanılabilirlik, seçtiğiniz sayı türüne, konuma ve plana bağlıdır.
> Sayılar, işlemin süresi dolmadan kısa bir süre için ayrılır. İşlemin süresi dolarsa, numaraları yeniden seçmeniz gerekir.

Satın alma özetini görüntülemek ve siparişinizi yerleştirmek için `Next: Summary` sayfanın altındaki düğmeye tıklayın.

### <a name="place-order"></a>Siparişi yerleştir

Özet sayfası, telefon numaralarını sağlamak için numara türünü, özellikleri, telefon numaralarını ve toplam aylık maliyeti gözden geçirir.

> [!NOTE]
> Gösterilen fiyatlar, seçilen telefon numarasının size kira maliyetini kapsayan **aylık yinelenen ücretlerdir** . Bu görünümde yer almadığınızda, çağrılar yaptığınızda veya alırken tahakkuk eden **Kullandıkça Öde maliyetlerinden** de yer verilir. Fiyat listelerine [buradan ulaşabilirsiniz](../../concepts/pricing.md). Bu ücretler, sayı türüne ve adlı hedeflere bağlıdır. Örneğin, Seattle bölgesel numarasından New York 'daki bölgesel bir sayıya yapılan çağrı için dakika başına fiyatı ve bir UK cep telefonu numarasına aynı sayıdan yapılan bir çağrı farklı olabilir.

Son olarak, `Place order` onaylamak için sayfanın alt kısmına tıklayın.

:::image type="content" source="../media/manage-phone-azure-portal-get-numbers-summary.png" alt-text="Iletişim Hizmetleri kaynağının ana sayfasını gösteren ekran görüntüsü.":::

## <a name="find-your-phone-numbers-on-the-azure-portal"></a>Telefon numaralarınızı Azure portal bulun

[Azure Portal](https://portal.azure.com)Azure iletişim kaynağına gidin:

:::image type="content" source="../media/manage-phone-azure-portal-start.png" alt-text="Iletişim Hizmetleri kaynağının ana sayfasını gösteren ekran görüntüsü.":::

Telefon numaralarınızı yönetmek için menüdeki telefon numaraları dikey penceresini seçin.

:::image type="content" source="../media/manage-phone-azure-portal-phones.png" alt-text="Iletişim Hizmetleri kaynağının ana sayfasını gösteren ekran görüntüsü.":::

> [!NOTE]
> Sağlanan sayıların bu sayfada gösterilmesi birkaç dakika sürebilir.

### <a name="customizing-phone-number-plans"></a>Telefon numarası planlarını özelleştirme
`Numbers`Sayfada, planı özelleştirmek istediğiniz numaraya tıklayarak telefon numarasını seçebilirsiniz...

:::image type="content" source="../media/manage-phone-azure-portal-capability-update.png" alt-text="Iletişim Hizmetleri kaynağının ana sayfasını gösteren ekran görüntüsü.":::

Kullanılabilir arama ve SMS özellikleri listesinden özellikleri seçin ve ardından `Confirm` seçimi uygulamak için tıklayın.

## <a name="troubleshooting"></a>Sorun giderme

Yaygın sorular ve sorunlar:

- Şu anda telefon numaralarının satın alınması için yalnızca US desteklenir. Bu, kaynağın ilişkilendirildiği aboneliğin fatura adresini temel alır. Şu anda bir kaynağı başka bir aboneliğe taşıyamazsınız.

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
