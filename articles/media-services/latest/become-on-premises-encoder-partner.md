---
title: Şirket içi kodlayıcı iş ortağı olun - Azure Medya Hizmetleri
description: Bu makalede, şirket içi canlı akış kodlayıcılarınızı nasıl doğrulayınız tartışılmaktadır.
services: media-services
author: johndeu
manager: johndeu
ms.author: johndeu
ms.date: 03/02/2020
ms.topic: article
ms.service: media-services
ms.openlocfilehash: f98d9942f8c30f06b0144503b056c1e8a393ae52
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79298640"
---
# <a name="how-to-verify-your-on-premises-live-streaming-encoder"></a>Şirket içi canlı akış kodlayıcınızı doğrulama

Azure Medya Hizmetleri şirket içi kodlayıcı iş ortağı olarak Media Services, kodlayıcınızı kurumsal müşterilere önererek ürününüzü tanıtıyor. Şirket içi kodlayıcı ortağı olmak için, şirket içi kodlayıcınızın Medya Hizmetleri ile uyumluluğunu doğrulamanız gerekir. Bunu yapmak için aşağıdaki doğrulamaları tamamlayın.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="pass-through-live-event-verification"></a>Pass-through Canlı Etkinlik doğrulaması

1. Medya Hizmetleri hesabınızda, Akış **Bitiş Noktası'nın** çalıştığını unutmayın. 
2. **Geçiş** live event oluşturun ve başlatın. <br/> Daha fazla bilgi için [Canlı Etkinlik durumları ve faturalandırma konusuna](live-event-states-billing.md)bakın.
3. En yüksek URL'leri alın ve şirket içi kodlayıcınızı, Medya Hizmetleri'ne çok bit hızında canlı akış göndermek için URL'yi kullanacak şekilde yapılandırın.
4. Önizleme URL'sini alın ve kodlayıcıdan gelen girişin gerçekten alındığını doğrulamak için kullanın.
5. Yeni bir **Varlık** nesnesi oluşturun.
6. Canlı **Çıktı** oluşturun ve oluşturduğunuz varlık adını kullanın.
7. Yerleşik **Akış İlkesi** türlerini içeren bir **Akış Bulucu** oluşturun.
8. Kullanılacak URL'leri geri almak için **Akış Bulucu'daki** yolları listele.
9. Akış yapmak istediğiniz **Akış Bitiş Noktası'nın** ana bilgisayar adını alın.
10. Tam URL'yi almak için adım 8'deki URL'yi 9.
11. Canlı kodlayıcınızı yaklaşık 10 dakika çalıştırın.
12. Canlı Etkinliği Durdurun. 
13. Oynatmanın tüm kalite düzeylerinde görünür bir aksaklık olmadığından emin olmak için arşivlenmiş varlığı izlemek için [Azure Media Player](https://aka.ms/azuremediaplayer) gibi bir oynatıcı kullanın. Veya canlı oturum sırasında önizleme URL'si üzerinden izleyin ve doğrulayın.
14. Varlık kimliğini, canlı arşiv için yayınlanan akış URL'sini ve canlı kodlayıcınızdan kullanılan ayarları ve sürümü kaydedin.
15. Her örnek oluşturduktan sonra Canlı Olay durumunu sıfırla.
16. Kodlayıcınız tarafından desteklenen tüm yapılandırmalar için (reklam sinyali, altyazılar veya farklı kodlama hızları olan ve olmayan) 5 ile 15 adımlarını yineleyin.

## <a name="live-encoding-live-event-verification"></a>Canlı kodlama Canlı Olay doğrulama

1. Medya Hizmetleri hesabınızda, Akış **Bitiş Noktası'nın** çalıştığını unutmayın. 
2. **Canlı Kodlama** Live Event'i oluşturun ve başlatın. <br/> Daha fazla bilgi için [Canlı Etkinlik durumları ve faturalandırma konusuna](live-event-states-billing.md)bakın.
3. En yüksek URL'leri alın ve kodlayıcınızı Media Services'e tek bit hızında canlı akışı itecek şekilde yapılandırın.
4. Önizleme URL'sini alın ve kodlayıcıdan gelen girişin gerçekten alındığını doğrulamak için kullanın.
5. Yeni bir **Varlık** nesnesi oluşturun.
6. Canlı **Çıktı** oluşturun ve oluşturduğunuz varlık adını kullanın.
7. Yerleşik **Akış İlkesi** türlerini içeren bir **Akış Bulucu** oluşturun.
8. Kullanılacak URL'leri geri almak için **Akış Bulucu'daki** yolları listele.
9. Akış yapmak istediğiniz **Akış Bitiş Noktası'nın** ana bilgisayar adını alın.
10. Tam URL'yi almak için adım 8'deki URL'yi 9.
11. Canlı kodlayıcınızı yaklaşık 10 dakika çalıştırın.
12. Canlı Etkinliği Durdurun.
13. Oynatmanın tüm kalite düzeyleri için görünür bir aksaklık olmadığından emin olmak için arşivlenmiş varlığı izlemek için [Azure Media Player](https://aka.ms/azuremediaplayer) gibi bir oynatıcı kullanın. Veya canlı oturum sırasında önizleme URL'si üzerinden izleyin ve doğrulayın.
14. Varlık kimliğini, canlı arşiv için yayınlanan akış URL'sini ve canlı kodlayıcınızdan kullanılan ayarları ve sürümü kaydedin.
15. Her örnek oluşturduktan sonra Canlı Olay durumunu sıfırla.
16. Kodlayıcınız tarafından desteklenen tüm yapılandırmalar için (reklam sinyali, altyazılar veya farklı kodlama hızları olan ve olmayan) 5 ile 15 adımlarını yineleyin.

## <a name="longevity-verification"></a>Uzun ömür doğrulaması

Adım 11 dışında [Pass-through Live Event doğrulamasında](#pass-through-live-event-verification) ki adımları izleyin. <br/>10 dakika yerine, canlı kodlayıcınızı bir hafta veya daha uzun süre çalıştırın. Oynatmanın görünür bir hatası olmadığından emin olmak için zaman zaman canlı akışı (veya arşivlenmiş bir varlık) izlemek için [Azure Media Player](https://aka.ms/azuremediaplayer) gibi bir oynatıcı kullanın.

## <a name="email-your-recorded-settings"></a>Kaydettiğiniz ayarları e-postayla gönder

Son olarak, kaydettiğiniz ayarları nızı ve canlı amshelp@microsoft.com arşiv parametrelerinizi azure medya hizmetlerine tüm kendi kendini doğrulama denetimlerinin geçtiğini bildiren bir bildirim olarak e-postayla gönder. Ayrıca, herhangi bir takip için iletişim bilgilerinizi ekleyin. Bu işlemle ilgili sorularınız için Azure Medya Hizmetleri ekibiyle iletişime geçebilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.

[Şirket içinde test edilmiş kodlayıcılar](recommended-on-premises-live-encoders.md)

## <a name="next-steps"></a>Sonraki adımlar

[Medya Hizmetleri v3 ile canlı akış](live-streaming-overview.md)
