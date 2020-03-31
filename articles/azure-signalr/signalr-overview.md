---
title: Azure SinyalHizmeti Nedir?
description: Azure SignalR'ı kullanmak için hangi tipik kullanım durum senaryolarını daha iyi anlayın ve Azure SignalR'ın temel avantajlarını öğrenin.
author: sffamily
ms.service: signalr
ms.topic: overview
ms.date: 11/13/2019
ms.author: zhshang
ms.openlocfilehash: 31cb4570dd0952f60fbff2fa1131b3af36c278cb
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "74157584"
---
# <a name="what-is-azure-signalr-service"></a>Azure SinyalHizmeti Nedir?

Azure SignalR Hizmeti, HTTP üzerinden uygulamalara gerçek zamanlı web işlevselliği ekleme işlemini basitleştirir. Bu gerçek zamanlı işlevsellik, hizmetin bağlı istemcilere tek sayfalık bir web veya mobil uygulaması gibi içerik güncelleştirmeleri göndermesine olanak tanır. Sonuç olarak, istemciler sunucuyu yoklama veya yeni HTTP isteklerini güncelleştirmek üzere gönderme gereksinimi olmadan güncelleştirilir.


Bu makalede Azure SignalR Hizmetine genel bir bakış sunulmaktadır.

## <a name="what-is-azure-signalr-service-used-for"></a>Azure SignalR Hizmeti ne için kullanılır?

Verileri sunucudan istemciye gerçek zamanlı olarak itmeyi gerektiren herhangi bir senaryo, Azure SignalR Hizmetini kullanabilir.

Genellikle sunucudan yoklama gerektiren geleneksel gerçek zamanlı özellikler, Azure SinyalR Hizmetini de kullanabilir.

Azure SignalR Hizmeti, gerçek zamanlı içerik güncelleştirmeleri gerektiren tüm uygulama türü için çok çeşitli sektörlerde kullanılmıştır. Azure SinyalR Hizmetini kullanmak için iyi olan bazı örnekleri listeliyoruz:

* **Yüksek frekanslı veri güncellemeleri:** oyun, oylama, yoklama, açık artırma.
* **Panolar ve izleme:** şirket panosu, finansal piyasa verileri, anında satış güncellemesi, çok oyunculu oyun lider panosu ve IoT izleme.
* **Sohbet:** canlı sohbet odası, sohbet botu, on-line müşteri desteği, gerçek zamanlı alışveriş asistanı, messenger, oyun içi sohbet, ve benzeri.
* **Haritada gerçek zamanlı konum:** lojistik izleme, teslimat durumu izleme, ulaşım durumu güncellemeleri, GPS uygulamaları.
* **Gerçek zamanlı hedefli reklamlar:** kişiselleştirilmiş gerçek zamanlı anında kiremit reklamları ve teklifleri, etkileşimli reklamlar.
* **Ortak uygulamalar:** ortak yazma, beyaz tahta uygulamaları ve ekip toplantı yazılımı.
* **Push bildirimleri:** sosyal ağ, e-posta, oyun, seyahat uyarısı.
* **Gerçek zamanlı yayın:** canlı ses/video yayını, canlı altyazı, çeviri, etkinlikler/haber yayını.
* **IoT ve bağlı aygıtlar:** gerçek zamanlı IoT ölçümleri, uzaktan kumanda, gerçek zamanlı durum ve konum izleme.
* **Otomasyon:** akış yukarı olaylardan gerçek zamanlı tetikleme.

## <a name="what-are-the-benefits-using-azure-signalr-service"></a>Azure SignalR Hizmetini kullanmanın avantajları nelerdir?

**Standart tabanlı:**

SignalR, gerçek zamanlı web uygulamaları derlemek için kullanılan birkaç tekniğin özetini sunar. [WebSockets](https://wikipedia.org/wiki/WebSocket) ideal aktarım yöntemidir ancak başka seçenek olmadığında [Sunucu ile Gönderilen Olaylar (SSE)](https://wikipedia.org/wiki/Server-sent_events) ve Uzun Yoklama gibi diğer teknikler de kullanılır. SignalR, sunucu ve istemci üzerinde desteklenen özelliklere göre uygun taşıma tekniğini otomatik olarak algılar ve başlatır.

**Yerli ASP.NET Core desteği:**

SignalR Service, ASP.NET Core ve ASP.NET ile yerel programlama deneyimi sağlar. SignalR Hizmeti ile yeni SignalR uygulaması geliştirmek veya mevcut SignalR tabanlı uygulamadan SignalR Hizmetine geçiş yapmak en az çabayı gerektirir.
SignalR Service ayrıca ASP.NET Core'un yeni özelliği olan Sunucu tarafı Blazor'u da destekler.

**Geniş istemci desteği:**

SignalR Hizmeti, web ve mobil tarayıcılar, masaüstü uygulamaları, mobil uygulamalar, sunucu süreci, IoT aygıtları ve oyun konsolları gibi çok çeşitli istemcilerle çalışır. SignalR Service farklı dillerde SDK'lar sunar. SignalR Service, yerel ASP.NET Core veya ASP.NET C# SDK'larına ek olarak, web istemcilerini ve birçok JavaScript çerçevesini etkinleştirmek için JavaScript istemcisi SDK'yı da sağlar. Java istemciSi SDK, Android yerel uygulamaları da dahil olmak üzere Java uygulamaları için de desteklenir. SignalR Hizmeti, Azure Fonksiyonları ve Olay Ağıtı ile tümleştirmeler yoluyla REST API'yi ve sunucusuz ları destekler.

**Büyük ölçekli istemci bağlantılarını işleme:**

SignalR Service büyük ölçekli gerçek zamanlı uygulamalar için tasarlanmıştır. SignalR Hizmeti, milyonlarca istemci bağlantısına ölçeklendirmek için birden çok örneğin birlikte çalışmasına olanak tanır. Hizmet ayrıca, parçalama, yüksek kullanılabilirlik veya olağanüstü durum kurtarma amaçları için birden çok küresel bölgeyi destekler.

**Kendi kendine barındıran SignalR'ın yükünü kaldırın:**

Kendi kendine barındırılan SignalR uygulamalarıyla karşılaştırıldığında, SignalR Hizmeti'ne geçmek, ölçekleri ve istemci bağlantılarını işleyen arka düzlemleri yönetme gereksinimini ortadan kaldırır. Tam olarak yönetilen hizmet aynı zamanda web uygulamalarını basitleştirir ve barındırma maliyetini kaydeder. SignalR Service, küresel erişim ve birinci sınıf veri merkezi ve ağı sunar, milyonlarca bağlantıya ölçekler, SLA'yı garanti eder ve Azure standartlarında tüm uyumluluk ve güvenliği sağlar.

![Yönetilen Sinyalci Hizmeti](./media/signalr-overview/managed-signalr-service.png)

**Farklı mesajlaşma desenleri için zengin API'ler sunun:**

SignalR Hizmeti, sunucunun belirli bir bağlantıya, tüm bağlantılara veya belirli bir kullanıcıya ait veya rasgele bir gruba yerleştirilmiş bir bağlantı alt kümesine ileti göndermesine olanak tanır.

## <a name="how-to-use-azure-signalr-service"></a>Azure SignalR Service Nasıl Kullanılır

Azure SinyalR Hizmeti ile programlamanın birçok farklı yolu vardır, çünkü burada listelenen örneklerden bazıları:

- **[Bir ASP.NET Core SignalR Uygulamasını Ölçeklendirme](signalr-concept-scale-aspnet-core.md)** - Yüzbinlerce bağlantıya ölçeklendirmek için Azure SignalR Hizmetini bir ASP.NET Core SignalR uygulaması ile tümleştirin.
- **[Sunucusuz, gerçek zamanlı uygulamalar oluşturma](signalr-concept-azure-functions.md)** JavaScript, C# ve Java gibi dillerde sunucusuz, gerçek zamanlı uygulamalar oluşturmak için Azure İşlevleri tümleştirmesini Azure SignalR Hizmeti ile kullanın.
- **[REST API aracılığıyla istemcilere sunucudan iletileri gönderme](https://github.com/Azure/azure-signalr/blob/dev/docs/rest-api.md)** - Azure SignalR Hizmeti, REST kullanılabilen tüm dillerle uygulamaların SignalR Hizmeti ile bağlı istemcilere iletiler gönderebilmesi için REST API’sini sunar.
