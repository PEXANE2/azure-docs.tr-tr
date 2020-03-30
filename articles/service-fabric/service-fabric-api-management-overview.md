---
title: API Yönetimi'ne genel bakışa sahip Azure Hizmet Kumaşı
description: Bu makale, Hizmet Kumaşı uygulamalarınız için bir ağ geçidi olarak Azure API Yönetimi'ni kullanmaya giriştir.
author: vturecek
ms.topic: conceptual
ms.date: 06/22/2017
ms.author: vturecek
ms.openlocfilehash: 2a331715d4e4538cfdda8d958ff549a81b627b79
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76028542"
---
# <a name="service-fabric-with-azure-api-management-overview"></a>Service Fabric ve Azure API Management'a genel bakış

Bulut uygulamalarının normalde kullanıcılar, cihazlar ve diğer uygulamalara tek giriş noktası sağlamak için bir ön uç ağ geçidine ihtiyacı vardır. Hizmet Kumaşı'nda ağ geçidi, [ASP.NET Core uygulaması](service-fabric-reliable-services-communication-aspnetcore.md)veya Olay [Hub'ları,](https://docs.microsoft.com/azure/event-hubs/) [IoT Hub'ı](https://docs.microsoft.com/azure/iot-hub/)veya Azure [API Yönetimi](https://docs.microsoft.com/azure/api-management/)gibi trafik girişi için tasarlanmış başka bir hizmet gibi herhangi bir devletsiz hizmet olabilir.

Bu makale, Hizmet Kumaşı uygulamalarınız için bir ağ geçidi olarak Azure API Yönetimi'ni kullanmaya giriştir. API Yönetimi, arka uç Hizmet Kumaşı hizmetlerinize zengin bir yönlendirme kuralları kümesiyle API'leri yayımlamanızı sağlayan Doğrudan Service Fabric ile tümleşir.

## <a name="availability"></a>Kullanılabilirlik

> [!IMPORTANT]
> Bu özellik, gerekli sanal ağ desteği sayesinde API Management'ın **Premium** ve **Geliştirici** katmanlarında kullanılabilir.

## <a name="architecture"></a>Mimari

Ortak bir Service Fabric mimarisi, HTTP API'lerini ortaya çıkaran arka uç hizmetlerine HTTP çağrıları yapan tek sayfalık bir web uygulaması kullanır. [Hizmet Kumaşı başlangıç örneği uygulaması](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started) bu mimarinin bir örneğini gösterir.

Bu senaryoda, devletsiz bir web hizmeti Hizmet Kumaşuygulamasına ağ geçidi görevi görehizmet eder. Bu yaklaşım, aşağıdaki diyagramda gösterildiği gibi, ARKA UÇ hizmetlerine HTTP isteklerini ekleyebilir bir web hizmeti yazmanızı gerektirir:

![Azure API Yönetimi topolojisi ile Hizmet Kumaşı][sf-web-app-stateless-gateway]

Uygulamalar karmaşıklık ta arttıkça, sayısız arka uç hizmetlerinin önünde bir API sunması gereken ağ geçitleri de büyür. Azure API Yönetimi, karmaşık API'leri yönlendirme kuralları, erişim denetimi, hız sınırlaması, izleme, olay günlüğe kaydetme ve yanıt önbelleğe alma işlemlerini en az çalışmayla işlemek üzere tasarlanmıştır. Azure API Yönetimi, kendi bağımsız API ağ geçidinizi yazmak zorunda kalmamak için istekleri Doğrudan Hizmet Dokusu'ndaki arka uç hizmetlerine yönlendirmek için Service Fabric hizmet bulma, bölüm çözme ve yineleme seçimini destekler. 

Bu senaryoda, WEB Web Arama Birimi hala bir web hizmeti aracılığıyla sunulurken, HTTP API çağrıları aşağıdaki diyagramda gösterildiği gibi Azure API Yönetimi aracılığıyla yönetilir ve yönlendirilir:

![Azure API Yönetimi topolojisi ile Hizmet Kumaşı][sf-apim-web-app]

## <a name="application-scenarios"></a>Uygulama senaryoları

Hizmet Kumaşı'ndaki hizmetler durumsuz veya durum lu olabilir ve üç şemandan biri kullanılarak bölümlere bölünebilir: singleton, int-64 aralığı ve adlandırılmış. Hizmet bitiş noktası çözümlemesi, belirli bir hizmet örneğinin belirli bir bölümü nlemist olarak tanımlanmasını gerektirir. Bir hizmetin bitiş noktasını çözerken, tek ton bölümü `fabric:/myapp/myservice`dışında hem hizmet örneği adı (örneğin,) hem de hizmetin belirli bölümü belirtilmelidir.

Azure API Yönetimi, durum durumu olmayan hizmetlerin, durum durumu hizmetinin ve herhangi bir bölümleme düzeninin herhangi bir birleşimi ile kullanılabilir.

## <a name="send-traffic-to-a-stateless-service"></a>Trafiği vatansız bir hizmete gönderme

En basit durumda, trafik devletsiz bir hizmet örneğine iletilir. Bunu başarmak için, bir API Yönetimi işlemi, Hizmet Kumaşı arka uçtaki belirli bir durum dışı hizmet örneğine eş leyen Bir Hizmet Kumaşı arka ucunu içeren gelen bir işlem ilkesi içerir. Bu hizmete gönderilen istekler, hizmetin rasgele bir örneğine gönderilir.

**Örnek**

Aşağıdaki senaryoda, Hizmet Dokusu uygulaması, `fabric:/app/fooservice`dahili bir HTTP API'yi ortaya çıkaran, "devletsiz" bir hizmet içerir. Hizmet örneği adı iyi bilinir ve doğrudan API Yönetimi gelen işleme ilkesinde sabit kodlanabilir. 

![Azure API Yönetimi topolojisi ile Hizmet Kumaşı][sf-apim-static-stateless]

## <a name="send-traffic-to-a-stateful-service"></a>Trafiği devlet hizmetine gönderme

Durumsuz hizmet senaryosuna benzer şekilde, trafik durumlu bir hizmet örneğine iletilebilir. Bu durumda, bir API Yönetimi işlemi, isteği belirli bir *durum lu* hizmet örneğinin belirli bir bölümüyle eşleyen Hizmet Dokusu arka ucuna sahip gelen bir işleme ilkesi içerir. Her isteği eşlemek için yapılan bölüm, URL yolundaki bir değer gibi gelen HTTP isteğinden bazı girişler kullanılarak bir lambda yöntemi yle hesaplanır. İlke, istekleri yalnızca birincil yinelemeye veya okuma işlemleri için rasgele bir yinelemeye göndermek üzere yapılandırılabilir.

**Örnek**

Aşağıdaki senaryoda, Bir Hizmet Kumaş uygulaması dahili `fabric:/app/userservice` bir HTTP API ortaya çıkaran bir bölümlü durum hizmeti içerir. Hizmet örneği adı iyi bilinir ve doğrudan API Yönetimi gelen işleme ilkesinde sabit kodlanabilir.  

Hizmet, int64 bölüm şemasını iki bölüm ve `Int64.MinValue` `Int64.MaxValue`'ye yayılan bir anahtar aralığı yla birlikte bölümlendirilir. Arka uç ilkesi, URL istek yolunda sağlanan `id` değeri 64 bit tamsayıya dönüştürerek bu aralıktaki bir bölüm anahtarını hesaplar, ancak burada bölüm anahtarını hesaplamak için herhangi bir algoritma kullanılabilir. 

![Azure API Yönetimi topolojisi ile Hizmet Kumaşı][sf-apim-static-stateful]

## <a name="send-traffic-to-multiple-stateless-services"></a>Birden çok sürücüsüz hizmete trafik gönderme

Daha gelişmiş senaryolarda, istekleri birden fazla hizmet örneğine eşleyen bir API Yönetimi çalışması tanımlayabilirsiniz. Bu durumda, her işlem, url yolu veya sorgu dizesi gibi gelen HTTP isteğindeki değerleri ve hizmet örneği içinde bir bölüm olarak durum lu hizmetler söz konusu olduğunda, istekleri belirli bir hizmet örneğine eşleyen bir ilke içerir.

Bunu başarmak için, bir API Yönetimi işlemi, gelen HTTP isteğinden alınan değerlere dayalı olarak Hizmet Kumaşı arka uçtaki durum dışı bir hizmet örneğine eş leyen Bir Hizmet Kumaşı arka ucunu içeren gelen bir işlem ilkesi içerir. Bir hizmete gelen istekler, hizmetin rasgele bir örneğine gönderilir.

**Örnek**

Bu örnekte, aşağıdaki formülü kullanarak dinamik olarak oluşturulan bir ada sahip bir uygulamanın her kullanıcısı için yeni bir durum ifadesüz hizmet örneği oluşturulur:

- `fabric:/app/users/<username>`

  Her hizmetin benzersiz bir adı vardır, ancak hizmetler kullanıcı veya yönetici girişine yanıt olarak oluşturulduğundan ve bu nedenle APIM ilkelerine veya yönlendirme kurallarına sabit kodlanamadığından adlar önceden bilinmemektedir. Bunun yerine, istek gönderilen hizmetin adı, URL istek yolunda sağlanan `name` değerden arka uç ilke tanımında oluşturulur. Örnek:

  - `/api/users/foo` İstek, hizmet örneğine yönlendirilir`fabric:/app/users/foo`
  - `/api/users/bar` İstek, hizmet örneğine yönlendirilir`fabric:/app/users/bar`

![Azure API Yönetimi topolojisi ile Hizmet Kumaşı][sf-apim-dynamic-stateless]

## <a name="send-traffic-to-multiple-stateful-services"></a>Trafiği birden çok devlet hizmetine gönderme

Durum suz hizmet örneğine benzer şekilde, bir API Yönetimi işlemi istekleri birden fazla **durum lu** hizmet örneğine eşleyebilir ve bu durumda her durum lu hizmet örneği için bölüm çözümlemesi gerçekleştirmeniz de gerekebilir.

Bunu başarmak için, bir API Yönetimi işlemi, gelen HTTP isteğinden alınan değerlere dayalı olarak Hizmet Kumaşı arka uçlarında durumlu bir hizmet örneğine eş veren Bir Hizmet Kumaşı arka ucunu içeren gelen bir işlem ilkesi içerir. Bir isteği belirli hizmet örneğine eşlemenin yanı sıra, istek hizmet örneği içinde belirli bir bölüme ve isteğe bağlı olarak birincil yinelemeye veya bölüm içindeki rasgele ikincil yinelemeye eşlenebilir.

**Örnek**

Bu örnekte, aşağıdaki formülü kullanarak dinamik olarak oluşturulan bir adla uygulamanın her kullanıcısı için yeni bir durum lu hizmet örneği oluşturulur:

- `fabric:/app/users/<username>`

  Her hizmetin benzersiz bir adı vardır, ancak hizmetler kullanıcı veya yönetici girişine yanıt olarak oluşturulduğundan ve bu nedenle APIM ilkelerine veya yönlendirme kurallarına sabit kodlanamadığından adlar önceden bilinmemektedir. Bunun yerine, istek gönderilen hizmetin adı, URL istek yolunu sağlanan `name` değerden arka uç ilke tanımında oluşturulur. Örnek:

  - `/api/users/foo` İstek, hizmet örneğine yönlendirilir`fabric:/app/users/foo`
  - `/api/users/bar` İstek, hizmet örneğine yönlendirilir`fabric:/app/users/bar`

Her hizmet örneği de iki bölüm ve `Int64.MinValue` `Int64.MaxValue`'ye yayılan bir anahtar aralığı ile Int64 bölüm düzeni kullanılarak bölümlenir. Arka uç ilkesi, URL istek yolunda sağlanan `id` değeri 64 bit tamsayıya dönüştürerek bu aralıktaki bir bölüm anahtarını hesaplar, ancak burada bölüm anahtarını hesaplamak için herhangi bir algoritma kullanılabilir. 

![Azure API Yönetimi topolojisi ile Hizmet Kumaşı][sf-apim-dynamic-stateful]

## <a name="next-steps"></a>Sonraki adımlar

API Yönetimi ve API Yönetimi aracılığıyla hizmetlerinize akış istekleriile ilk Service Fabric kümenizi ayarlamak için [öğreticiyi](service-fabric-tutorial-deploy-api-management.md) izleyin.

<!-- links -->

<!-- pics -->
[sf-apim-web-app]: ./media/service-fabric-api-management-overview/sf-apim-web-app.png
[sf-web-app-stateless-gateway]: ./media/service-fabric-api-management-overview/sf-web-app-stateless-gateway.png
[sf-apim-static-stateless]: ./media/service-fabric-api-management-overview/sf-apim-static-stateless.png
[sf-apim-static-stateful]: ./media/service-fabric-api-management-overview/sf-apim-static-stateful.png
[sf-apim-dynamic-stateless]: ./media/service-fabric-api-management-overview/sf-apim-dynamic-stateless.png
[sf-apim-dynamic-stateful]: ./media/service-fabric-api-management-overview/sf-apim-dynamic-stateful.png
