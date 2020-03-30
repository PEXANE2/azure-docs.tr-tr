---
title: Gerçek zamanlı uygulama oluşturun - Azure Fonksiyonları & Azure SignalR Hizmeti
description: Aşağıdaki örneği vererek Azure SignalR Hizmeti ile gerçek zamanlı sunucusuz web uygulamasını nasıl geliştireceklerini öğrenin.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 11/13/2019
ms.author: zhshang
ms.openlocfilehash: cbb1fcf320a78f11045bf9627ffcc438af3e388a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74157612"
---
# <a name="build-real-time-apps-with-azure-functions-and-azure-signalr-service"></a>Azure İşlevleri ve Azure SinyalR Hizmeti ile gerçek zamanlı Uygulamalar oluşturun

Azure Sinyal RHizmeti ve Azure Fonksiyonları, altyapıyı yönetmek yerine uygulama oluşturmaya odaklanmanızı sağlayan tam olarak yönetilen, yüksek ölçeklenebilir hizmetler olduğundan, [sunucusuz](https://azure.microsoft.com/solutions/serverless/) bir ortamda gerçek zamanlı iletişim sağlamak için iki hizmeti birlikte kullanmak yaygındır.

> [!NOTE]
> Etkileşimli öğreticide SignalR ve Azure Fonksiyonlarını birlikte kullanmayı öğrenin [Azure İşlevlerini ve SignalR Hizmetini kullanarak bir web uygulamasında otomatik güncelleştirmeleri etkinleştirin.](https://docs.microsoft.com/learn/modules/automatic-update-of-a-webapp-using-azure-functions-and-signalr)

## <a name="integrate-real-time-communications-with-azure-services"></a>Gerçek zamanlı iletişimleri Azure hizmetleriyle tümleştirme

Azure İşlevleri, bulutta olaylar oluştuğunda tetikleyen JavaScript, Python, C#ve Java gibi [çeşitli dillerde](../azure-functions/supported-languages.md)kod yazmanıza olanak tanır. Bu olayların örnekleri arasında şunlar yer alır:

* HTTP ve web kancası istekleri
* Periyodik zamanlayıcılar
* Azure hizmetlerindeki olaylar, örneğin:
    - Event Grid
    - Event Hubs
    - Service Bus
    - Cosmos DB değişiklik akışı
    - Depolama - bloblar ve kuyruklar
    - Salesforce ve SQL Server gibi Logic Apps bağlayıcıları

BU olayları Azure SignalR Service ile tümleştirmek üzere Azure İşlevlerini kullanarak olaylar gerçekleştiğinde binlerce istemciye bildirim gönderme özelliğine sahip olursunuz.

Azure Functions ve SignalR Service ile uygulayabileceğiniz gerçek zamanlı sunucusuz mesajlaşmaya ilişkin bazı yaygın senaryolar arasında şunlar yer alır:

* IoT cihaz telemetrisini gerçek zamanlı bir pano veya haritada görselleştirme
* Belgeler Cosmos DB’de güncelleştirildiğinde uygulamadaki verileri güncelleştirme
* Salesforce’da yeni siparişler oluşturulduğunda uygulama içi bildirimler gönderme

## <a name="signalr-service-bindings-for-azure-functions"></a>Azure İşlevleri için SignalR Service bağlamaları

Azure İşlevleri için SignalR Service bağlamaları Azure İşlevi uygulamasının SignalR Service’e bağlı istemcilerde iletiler yayımlamasına olanak tanır. İstemciler .NET, JavaScript ve Java’da kullanıma sunulan bir SignalR istemci SDK’sını kullanarak hizmete bağlanabilir, daha fazla dil desteği yakında gelecektir.

### <a name="an-example-scenario"></a>Örnek Senaryo

SignalR Service bağlamalarının kullanma örneklerinden birisi Cosmos DB değişiklik akışında yeni olaylar belirdiğinde gerçek zamanlı iletiler göndermek üzere Azure Cosmos DB ve SignalR Service ile tümleştirmek için Azure İşlevlerini kullanmaktır.

![Cosmos DB, Azure İşlevleri, SignalR Hizmeti](media/signalr-concept-azure-functions/signalr-cosmosdb-functions.png)

1. Cosmos DB koleksiyonunda bir değişiklik yapıldı
2. Değişiklik olayı Cosmos DB değişiklik akışına yayıldı
3. Azure İşlevleri, Cosmos DB tetikleyicisini kullanan değişiklik olayı tarafından tetiklendi
4. SignalR Service çıktı bağlaması SignalR Service’te bir ileti yayımlar
5. SignalR Service iletiyi tüm bağlı istemcilerde yayımlar

### <a name="authentication-and-users"></a>Kimlik doğrulama ve kullanıcılar

SignalR Service iletilerin tüm istemcilere veya tek bir kullanıcıya ait olanlar gibi belirli bir istemci alt kümesine yayınlanmasına olanak tanır. Azure İşlevleri için SignalR Service bağlamaları; Azure Active Directory, Facebook ve Twitter gibi sağlayıcılarda kullanıcıların kimliğini doğrulamak için App Service Kimlik Doğrulama ile birleştirilebilir. Ardından iletileri doğrudan bu kimliği doğrulanmış kullanıcılara gönderebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, geniş yelpazedeki gerçek zamanlı mesajlaşma senaryolarını etkinleştirmek üzere Azure İşlevlerini SignalR Service ile birlikte kullanmaya yönelik bir genel bakış edinirsiniz.

Azure İşlevlerini ve SignalR Hizmetinin birlikte nasıl kullanılacağı hakkında ayrıntılı bilgi için aşağıdaki kaynakları ziyaret edin:

* [SignalR Hizmeti ile Azure Fonksiyonları geliştirme ve yapılandırma](signalr-concept-serverless-development-config.md)
* [Azure İşlevleri ve SignalR Hizmeti’ni kullanarak bir web uygulamasında otomatik güncelleştirmeleri etkinleştirme](https://docs.microsoft.com/learn/modules/automatic-update-of-a-webapp-using-azure-functions-and-signalr)

Daha fazla bilgi edinmek için bu hızlı başlangıçlardan birini izleyin.

* [Azure SignalR Service Sunucusuz Hızlı Başlangıcı - C#](signalr-quickstart-azure-functions-csharp.md)
* [Azure SignalR Service Sunucusuz Hızlı Başlangıcı - JavaScript](signalr-quickstart-azure-functions-javascript.md)
