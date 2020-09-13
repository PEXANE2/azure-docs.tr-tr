---
title: Azure SignalR hizmetinde hizmet modu
description: Azure SignalR hizmetindeki farklı hizmet modlarına genel bakış, aralarındaki farkları ve geçerli kullanıcı senaryolarını açıklayın
author: chenkennt
ms.service: signalr
ms.topic: conceptual
ms.date: 08/19/2020
ms.author: kenchen
ms.openlocfilehash: 2cdce64b0ff03521a5848e2b4fd6e01431b5cc16
ms.sourcegitcommit: 7f62a228b1eeab399d5a300ddb5305f09b80ee14
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89514908"
---
# <a name="service-mode-in-azure-signalr-service"></a>Azure SignalR hizmetinde hizmet modu

Hizmet modu, Azure SignalR hizmeti 'nde önemli bir kavramdır. Yeni bir SignalR kaynağı oluşturduğunuzda, bir hizmet modu belirtmeniz istenir:

:::image type="content" source="media/concept-service-mode/create.png" alt-text="Oluşturma sırasında hizmet modunu seç":::

Bunu daha sonra Ayarlar menüsünde de değiştirebilirsiniz:

:::image type="content" source="media/concept-service-mode/update.png" alt-text="Hizmet modunu Güncelleştir":::

Azure SignalR hizmeti şu anda üç hizmet modunu desteklemektedir: **varsayılan**, **sunucusuz** ve **Klasik**. SignalR kaynağınız farklı modlarda farklı davranacaktır. Bu makalede, kendi farklılıklarını ve senaryonuza bağlı olarak doğru hizmet modunu nasıl seçebileceğinizi öğreneceksiniz.

## <a name="default-mode"></a>Varsayılan mod

Yeni bir SignalR kaynağı oluşturduğunuzda varsayılan mod, hizmet modu için varsayılan değerdir. Bu modda, uygulamanız tipik bir ASP.NET Core (veya ASP.NET) SignalR uygulaması olarak çalışarak hub 'ı barındıran bir Web sunucunuz (burada hub sunucusu adı verilir) ve istemciler hub sunucusu ile çift yönlü gerçek zamanlı iletişime sahip olabilir. Tek fark, istemci ve sunucuyu doğrudan bağlamak yerine, istemci ve sunucu her ikisi de SignalR hizmetine bağlanır ve hizmeti bir proxy olarak kullanır. Varsayılan modda tipik uygulama yapısını gösteren bir diyagram aşağıda verilmiştir:

:::image type="content" source="media/concept-service-mode/default.png" alt-text="Varsayılan modda uygulama yapısı":::

Bu nedenle, bir SignalR uygulamanız varsa ve SignalR hizmeti ile tümleştirme istiyorsanız, çoğu durumda varsayılan mod doğru seçim olmalıdır.

### <a name="connection-routing-in-default-mode"></a>Varsayılan modda bağlantı yönlendirmesi

Varsayılan modda, hub sunucusu ile SignalR hizmeti (sunucu bağlantıları denir) arasında WebSocket bağlantısı olacaktır. Bu bağlantılar, sunucu ve istemci arasında ileti aktarmak için kullanılır. Yeni bir istemci bağlandığında, SignalR hizmeti, var olan sunucu bağlantıları aracılığıyla istemciyi tek bir hub sunucusuna (birden fazla sunucunuz olduğunu varsayar) yönlendirir. Daha sonra istemci bağlantısı, yaşam süresi boyunca aynı hub sunucusunu kontrol edecektir. İstemci iletileri gönderdiğinde, her zaman aynı hub sunucusuna gider. Bu davranışla birlikte, hub sunucunuzdaki bireysel bağlantılar için bazı durumları güvenli bir şekilde koruyabilirsiniz. Örneğin, sunucu ve istemci arasında bir şeyi akışa almak istiyorsanız, veri paketlerinin farklı sunuculara gitmesi durumunda dikkat etmeniz gerekmez.

> [!IMPORTANT]
> Bu, varsayılan mod istemcisinin, önce sunucu bağlantısı olmadan bağlanamadan da anlamına gelir. Ağ kesintisi veya sunucu yeniden başlatması nedeniyle tüm Hub sunucularınızın bağlantısı kesilirse, istemci bağlantınız hiçbir sunucu bağlı olmadığını söyleyen bir hata alır. Bu nedenle, SignalR hizmetine bağlı en az bir hub sunucusu olan herhangi bir zamanda (örneğin, birden fazla hub sunucusu vardır ve bakım gibi şeyler için aynı anda çevrimdışı olarak geçmeyecekler) emin olmak sizin sorumluluğunuzdadır.

Bu yönlendirme modeli aynı zamanda bir hub sunucusu çevrimdışı olduğunda, bu sunucunun yönlendirildiği bağlantıların bırakılmayacağı anlamına gelir. Bu nedenle, hub sunucunuz bakım için çevrimdışıyken bağlantı bırakmaya ve uygulamanıza olumsuz etkileri olmayacak şekilde yeniden bağlanmayacak şekilde beklemeniz gerekir.

## <a name="serverless-mode"></a>Sunucusuz mod

Sunucusuz mod, adından da anlaşılacağı gibi, herhangi bir hub sunucunuz olamaz. Bu modda, varsayılan mod ile karşılaştırma, hub sunucusunun bağlanmasını gerektirmez. Tüm bağlantılar hizmete "sunucusuz" modunda bağlanır ve hizmet istemci ping işlemleri (varsayılan modda bu, hub sunucuları tarafından işlenir) gibi istemci bağlantılarını sürdürmekten sorumludur.

Ayrıca, bu modda sunucu bağlantısı yoktur (sunucu bağlantısı kurmak için Service SDK 'yı kullanmayı denerseniz, bir hata alırsınız). Bu nedenle, bağlantı yönlendirme ve sunucu-istemci sürekliliği de yoktur (varsayılan mod bölümünde açıklandığı gibi). Ancak, istemcilere iletileri göndermek için sunucu tarafı uygulamanız olabilir. Bu iki şekilde yapılabilir, birden fazla iletiyi daha verimli bir şekilde gönderebilmek için [REST API 'leri](https://github.com/Azure/azure-signalr/blob/dev/docs/rest-api.md) bir kerelik gönderimi veya bir WebSocket bağlantısı aracılığıyla gerçekleştirebilirsiniz (Bu WebSocket bağlantısının sunucu bağlantısından farklı olduğunu unutmayın).

> [!NOTE]
> SignalR hizmeti [Yönetim SDK 'sında](https://github.com/Azure/azure-signalr/blob/dev/docs/management-sdk-guide.md)hem REST API hem de WebSocket yöntemi desteklenir. .NET dışında bir dil kullanıyorsanız, bu [belirtimi](https://github.com/Azure/azure-signalr/blob/dev/docs/rest-api.md)takıp eden REST API 'leri el ile de çağırabilirsiniz.
>
> Azure Işlevleri 'ni kullanıyorsanız, iletileri çıkış bağlaması olarak göndermek için [Azure işlevleri Için SignalR hizmeti bağlamalarını](https://docs.microsoft.com/azure/azure-functions/functions-bindings-signalr-service) (Bu işlev bağlamayı çağırdı) kullanabilirsiniz.

Sunucu uygulamanızın istemcilerden ileti ve bağlantı olayları alması de mümkündür. Hizmet, Web kancaları kullanılarak önceden yapılandırılmış uç noktalara iletiler ve bağlantı olayları (yukarı akış olarak adlandırılır) sağlar. Varsayılan moda kıyasla, düşme garantisi yoktur ve HTTP istekleri WebSocket bağlantılarından daha az verimli olabilir.

Yukarı akışı yapılandırma hakkında daha fazla bilgi için bu [belgeye](https://docs.microsoft.com/azure/azure-signalr/concept-upstream)bakın.

Sunucusuz modunun nasıl çalıştığını gösteren bir diyagram aşağıda verilmiştir:

:::image type="content" source="media/concept-service-mode/serverless.png" alt-text="Sunucusuz modda uygulama yapısı":::

> [!NOTE]
> Lütfen varsayılan modda, hub sunucusu üzerinden gitmek istemiyorsanız istemciye doğrudan ileti göndermek için REST API/Yönetim SDK/işlev bağlamayı da kullanabilirsiniz. Ancak varsayılan modda istemci bağlantıları, hub sunucuları tarafından hala işlenir ve yukarı akış bu modda çalışmaz.

## <a name="classic-mode"></a>Klasik mod

Klasik, varsayılan ve sunucusuz moddan oluşan karma bir moddur. Bu modda, bağlantı moduna, istemci bağlantısı kurulduktan sonra hub sunucusu bağlı olup olmadığına karar verilir. Hub sunucusu varsa, istemci bağlantısı bir hub sunucusuna yönlendirilir. Aksi halde, istemci-sunucu iletisinin hub sunucusuna teslim edilebileceği sunucusuz bir mod girer. Bu, bazı tutarsızlıklara neden olur, örneğin, tüm Hub sunucuları kısa bir süre için kullanılamaz durumdaysa, bu süre içinde oluşturulan tüm istemci bağlantıları sunucusuz modda olur ve hub sunucusuna ileti gönderemezler.

> [!NOTE]
> Klasik mod temel olarak, varsayılan ve sunucusuz mod kullanılmadan önce oluşturulan uygulamalar için geriye dönük uyumluluk içindir. Bu modun artık kullanılması önemle önerilir. Yeni uygulamalar için lütfen senaryonuz temelinde varsayılan veya sunucusuz ' ı seçin. Mevcut uygulamalar için, kullanım örneklerini gözden geçirmeniz ve uygun bir hizmet modu seçmeniz da önerilir.

Klasik mod, sunucusuz modda yukarı akış gibi bazı yeni özellikleri de desteklemez.

## <a name="choose-the-right-service-mode"></a>Doğru hizmet modunu seçin

Artık hizmet modları arasındaki farkları anlamanız ve bunların arasından nasıl seçim yapılacağını bilmeniz gerekir. Önceki bölümde zaten öğrendiğiniz gibi klasik mod önerilir ve yalnızca varsayılan ve sunucusuz arasında seçim yapmanız gerekir. Yeni uygulamalar için doğru seçim yapmanıza ve mevcut uygulamalar için Klasik modu devre dışı bırakmaya yardımcı olabilecek bazı ipuçları aşağıda verilmiştir.

* SignalR kitaplığının nasıl çalıştığını ve şirket içinde barındırılan bir SignalR 'den Azure SignalR hizmetini kullanmak üzere geçiş yapmak istiyorsanız varsayılan mod ' u seçin. Varsayılan mod, otomatik olarak barındırılan SignalR ile tamamen aynı şekilde çalışır (ve SignalR kitaplığı 'nda aynı programlama modelini kullanabilirsiniz), SignalR hizmeti yalnızca istemciler ve hub sunucuları arasında bir proxy görevi görür.

* Yeni bir uygulama oluşturuyorsanız ve hub sunucusu ile sunucu bağlantılarını sürdürmek istemiyorsanız sunucusuz mod ' u seçin. Bu mod genellikle Azure Işlevleri ile birlikte çalışarak her türlü sunucuyu korumanıza gerek kalmaz. Çift yönlü iletişimlere sahip olabilirsiniz (REST API/Yönetim SDK 'Sı/işlev bağlama + yukarı akış), ancak programlama modeli SignalR kitaplığından farklı olacaktır.

* İstemci bağlantılarına ve arka uç uygulamasına iletileri doğrudan istemcilere göndermek için (örneğin, REST API) her iki hub sunucunuz varsa, yine de varsayılan mod ' u seçmeniz gerekir. Varsayılan ve sunucusuz mod arasındaki önemli farkın, hub sunucularınız ve istemci bağlantılarının nasıl yönlendirildiğini aklınızda bulundurun. REST API/Yönetim SDK/işlev bağlama her iki modda da kullanılabilir.

* Örneğin, tek bir karma senaryonuz varsa, biri geleneksel bir SignalR hub 'ı ve diğeri de Azure Işlevleri ile kullanılan ve hub sunucusu olmayan iki farklı hub 'ınız varsa, bunlardan biri varsayılan modda diğeri sunucusuz modda olmak üzere iki SignalR kaynağına ayırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Varsayılan ve sunucusuz modu kullanma hakkında daha fazla bilgi edinmek için aşağıdaki makaleleri okuyun:

* [Azure SignalR Service iç işlevleri](signalr-concept-internals.md)

* [Azure SignalR Hizmeti ile Azure İşlevleri geliştirme ve yapılandırma](signalr-concept-serverless-development-config.md)
