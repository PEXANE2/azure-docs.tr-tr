---
title: Azure rezervasyon önerileriyle ilgili sorunları giderme
description: Bu makale, Azure portalda gösterilen Azure rezervasyon önerilerini anlamanıza ve bunlarla ilgili sorunları gidermenize yardımcı olur.
author: bandersmsft
ms.service: cost-management-billing
ms.subservice: reservations
ms.author: banders
ms.reviewer: yashar
ms.topic: troubleshooting
ms.date: 10/19/2020
ms.openlocfilehash: a3137d779908bf2791ca396068a8c9edf5d56739
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92492257"
---
# <a name="troubleshoot-azure-reservation-recommendations"></a>Azure rezervasyon önerileriyle ilgili sorunları giderme

Bu makale, Azure portalda gösterilen Azure rezervasyon önerilerini anlamanıza ve bunlarla ilgili sorunları gidermenize yardımcı olur. Hiç öneri görmeyebilir veya beklentilerinizi karşılamayan öneriler görebilirsiniz. Örneğin, belirli bir VM yapılandırması için zaten ayrılmış bir örneğiniz olabilir. Belirli bir VM yapılandırmasıyla ilgili öneri görmeyi bekliyor olabilirsiniz.

## <a name="symptoms"></a>Belirtiler

1. [Azure portalda](https://portal.azure.com/) oturum açın ve **Rezervasyonlar** ’a gidin.
2. **+ Ekle** seçeneğini belirleyin ve sonra bir ürün seçin.
3. **Önerilen** sekmesinde hiç öneri görmeyebilir veya beklentilerinizi karşılamayan öneriler görebilirsiniz.

## <a name="cause"></a>Nedeni

Önerilen ürünler listesi, ürünün rezervasyon maliyetiyle kapsamınızdaki (paylaşılan veya tek) kullanım miktarınız karşılaştırılarak oluşturulur. Tasarruf algılaması durumunda öneri altyapısı, satın alabileceğiniz bir ürün önerir. Ancak altyapı bir tasarruf olduğunu belirlemezse ürün önermez.

Belirli bir yapılandırmaya sahip bir kaynağı çalıştırırken, bu yapılandırma için rezervasyon satın aldığınızda tasarruf edeceğinizin garantisi yoktur. Örneğin, aralıklı bir kullanımınız olabilir. Bu durumda rezervasyonun toplam maliyeti, rezervasyon süresinin ömrü boyunca tasarruf etmenizi sağlamayabilir.

Kapsam seçiminin önerileri ne şekilde etkilediğini anlamak da önemlidir. Kapsam **Paylaşılan** olarak ayarlandığında listedeki öneriler, faturalama aboneliğiyle ilişkili kaydın tamamı için tasarruf etmenin mümkün olduğunun Azure tarafından belirlendiği ayrılmış örnekleri gösterir. Kapsam **Tek** olarak ayarlandığında, listedeki öneriler yalnızca abonelikte çalıştırılan kaynaklar için geçerli olur. Bazı VM boyutları bir kapsam için önerilip başka bir kapsam için önerilmeyebilir. Örneğin kaydınızda, kayıt kapsamında rezervasyon satın alma maliyetini gerekçelendirebilecek yükseklikte **Standard_B1ls** toplu kullanımınız olabilir. Ancak kayıttaki tek bir abonelikte, kapsam içi rezervasyon satın alma maliyeti için bir gerekçe oluşturacak kadar kullanım olmayabilir. **Paylaşılan** olan kapsamı **Tek** olarak değiştirdiğinizde farklı öneriler alabilirsiniz.

Azure, belirlenen maliyet tasarruflarını dikkate alarak bazı dönemler için rezervasyon satın almayı önerirken bazıları için önermeyebilir. Özellikle, üç yıllık dönemlerde bir yıllık dönemlere kıyasla daha yüksek miktarda indirim sunulur. Azure üç yıllık dönem için bir yıllık döneme kıyasla muhtemelen daha fazla tasarruf tespit edecektir.

Azure’ın neden belirli bir kaynak boyutu ve miktarı önerdiğini anlamak istiyorsanız zaman içindeki olası tasarrufları gösteren ayrıntılı görselleştirmeyi görmek üzere **&lt;Miktar&gt; Ayrıntıları göster** seçeneğini belirleyin.

:::image type="content" source="./media/troubleshoot-reservation-recommendation/see-details-link.png" alt-text="Rezervasyon önerisi Ayrıntıları göster bağlantısının gösterildiği örnek" lightbox="./media/troubleshoot-reservation-recommendation/see-details-link.png" :::

## <a name="solution"></a>Çözüm

Dilediğiniz dönem için istediğiniz kadar rezervasyon satın alabilirsiniz. Öneriden farklı miktarda ve dönemde rezervasyon satın almak büyük ihtimalle idealin altında tasarruf ve kullanım elde etmenize neden olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

- Rezervasyon önerileri hakkında daha fazla bilgi için bkz. [Rezervasyon satın alma önerileri](determine-reservation-purchase.md).
