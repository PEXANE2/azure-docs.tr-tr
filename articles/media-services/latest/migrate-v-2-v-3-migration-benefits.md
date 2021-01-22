---
title: Adım 1-Media Services API v3 'e geçiş avantajlarından yararlanın | Microsoft Docs
description: Bu makalede Media Services V2 'den v3 'e geçiş avantajları listelenmektedir.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: conceptual
ms.workload: media
ms.date: 1/14/2020
ms.author: inhenkel
ms.openlocfilehash: c1e3fae35ff249b4435cf2fdcd2bf691bc393d56
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98690721"
---
# <a name="step-1---understand-the-benefits-of-migrating-to-media-services-api-v3"></a>Adım 1-Media Services API v3 'e geçiş avantajlarından yararlanın

![Geçiş Kılavuzu logosu](./media/migration-guide/azure-media-services-logo-migration-guide.svg)

<hr color="#5ea0ef" size="10">

![geçiş adımları 2](./media/migration-guide/steps-1.svg)

Yeni özellikler, işlevler ve performans iyileştirmeleri yalnızca geçerli v3 API 'sinde bulunduğundan avantajlara ulaşmak için Azure Media Services v3 API 'sinin 2020-05-01 sürümünü kullanmaya başlamanız önerilir.

Portalda API sürümünü, en son SDK 'Ları, en son CLı ve REST API doğru sürüm dizesiyle değiştirebilirsiniz.

V3 ile Media Services yönelik önemli iyileştirmeler yapılmıştır.  

## <a name="benefits-of-media-services-v3"></a>Media Services v3 avantajları

| **V3 özelliği** | **Avantaj** |
| --- | --- |
| **Azure portalı** | |
| Azure portal güncelleştirmeleri | Azure portal, v3 API varlıklarının yönetimini içerecek şekilde güncelleştirilmiştir. Müşterilerin, canlı akış başlatmak, v3 dönüşüm işleri göndermesi, içerik koruma ilkelerini yönetmesi, akış uç noktaları, API erişimi alması, bağlantılı depolama hesaplarını yönetmesi ve izleme görevlerini gerçekleştirmek için portalı kullanmasına izin verir. |
| **Hesaplar ve depolama** | |
| Azure rol tabanlı erişim denetimi (RBAC) | Müşteriler artık kendi rollerini tanımlayabilir ve Media Services ARM API 'sindeki her bir varlığa erişimi denetleyebilir. Bu, AAD hesaplarına göre kaynaklara erişimi denetlemeye yardımcı olur. |
| Yönetilen kimlikler | Yönetilen kimlikler, Azure AD 'de Azure kaynağı için bir kimlik sağlayarak geliştiricilerin kimlik bilgilerini yönetme ihtiyacını ortadan kaldırır. [Burada](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)Yönetilen kimlikler hakkındaki ayrıntılara bakın. |
| Özel bağlantı desteği | Müşteriler, anahtar teslimi, LiveEvents ve Streammingendpoints için Media Services uç noktalarına, VNet 'teki bir PrivateEndpoint aracılığıyla erişir. |
| [Müşteri tarafından yönetilen anahtarlar](concept-use-customer-managed-keys-byok.md) veya kendi anahtarınızı getır (bYok) desteği | Müşteriler, Media Services hesabındaki verileri Azure Key Vault bir anahtar kullanarak şifreleyebilir. |
| **Varlıklar** | |
| Bir varlık, her biri farklı [dinamik paketleme](dynamic-packaging-overview.md) ve dinamik şifreleme ayarlarına sahip birden fazla [akış Konumlandırıcı](streaming-locators-concept.md) olabilir. | Her varlık için izin verilen 100 akış belirleyicilerinin sınırı vardır. Müşteriler, varlık içinde medya içeriğinin tek bir kopyasını saklayabilir, ancak hedeflenen bir izleyiciyi temel alan farklı akış ilkelerine veya içerik koruma ilkelerine sahip farklı akış URL 'Lerini paylaşabilir.
| **İş işleme** ||
| V3 [](transforms-jobs-concept.md)   dosya tabanlı Iş işleme için dönüşüm kavramını tanıtır. | Bir dönüşüm yeniden kullanılabilir yapılandırma oluşturmak, Azure Resource Manager şablonları oluşturmak ve birden çok müşteri veya kiracı arasında işlem ayarlarını yalıtmak için kullanılabilir. |
| Dosya tabanlı iş işleme için, giriş olarak bir HTTP (S) URL 'SI kullanabilirsiniz. | Azure 'da zaten depolanmış içeriklere sahip olmanız veya giriş varlıkları oluşturmanız gerekmez. |
| **Canlı olaylar** ||
| Premium 1080p canlı etkinlikleri | Yeni canlı etkinlik SKU 'SU, müşterilerin en fazla 1080p 'e kadar bir çıkış ile bulut kodlaması almasına olanak tanır. |
| Canlı olaylarda yeni [düşük gecikmeli](live-event-latency.md) canlı akış desteği. | Bu, kullanıcıların canlı etkinlikleri gerçek zamanlı olarak daha yakından izlemesini sağlar ve bu ayar etkin değildir. |
| Canlı etkinlik önizlemesi, [dinamik paketleme](dynamic-packaging-overview.md)   ve dinamik şifrelemeyi destekler. | Bu, önizleme ve DASH ve HLS paketlenmesi için içerik korumasına izin verebilir. |
| Canlı çıkışlar Programları Değiştir | Canlı çıktı, v2 API 'Lerinde program varlığından daha basittir. |
| Canlı olaylar için RTMP alma, daha fazla kodlayıcılara yönelik destekle geliştirilmiştir | Kararlılığı artırır ve kaynak kodlayıcı esnekliği sağlar. |
| Canlı olaylar 7/24 akışa alabilir | Canlı bir olay barındırabilir ve hedef kitlenizi daha uzun dönemler için kullanılabilir tutabilirsiniz. |
| Canlı etkinlikler üzerinde canlı döküm | Canlı döküm, müşterilerin canlı bir olay yayını sırasında dili gerçek zamanlı olarak otomatik olarak metne dönüştürme olanağı sağlar. Bu, canlı olayların erişilebilirliğini önemli ölçüde geliştirir. |
| Canlı olaylarda [tek tek modu](live-events-outputs-concept.md#standby-mode) | Bekleme durumundaki canlı olaylar, canlı olayları çalıştırmanın daha maliyetlidir. Bu, müşterilerin, bir dizi çalışan canlı olayı korumadan daha düşük bir maliyetle çalışmaya hazırlanma bir dizi canlı olayı korumasına olanak tanır. Bekleyen canlı etkinlikler için azaltılmış fiyatlandırma, çoğu bölge için Şubat 2021 ' de, geri kalan 2021 Nisan 'da geçerli olacak.
|**İçerik koruma** ||
| [İçerik koruma](content-key-policy-concept.md)   Çoklu anahtar özelliklerini destekler. | Müşteriler artık akış bulleyicileri üzerinde birden fazla içerik şifreleme anahtarı kullanabilir. |
| **İzleme** | |
| [Azure EventGrid](reacting-to-media-services-events.md) bildirim desteği | EventGrid bildirimleri daha zengin özelliktir. Daha fazla türde bildirim, kendi uygulamanızda bildirimleri almak için daha geniş SDK desteği ve olay işleyicileri gibi davranan mevcut Azure hizmetleri. |
| [Azure portal Azure Izleyici desteği ve tümleştirmesi](monitor-events-portal-how-to.md) | Bu, müşterilerin Media Services hesap kota kullanımını, akış uç noktalarının gerçek zamanlı istatistiklerini ve canlı olaylar için alma ve arşivleme istatistiklerini görselleştirmesini sağlar. Müşteriler artık uyarıları ayarlayabiliyor ve gerçek zamanlı ölçüm verilerine göre gerekli eylemleri gerçekleştirebilir. |

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [migration guide next steps](./includes/migration-guide-next-steps.md)]
