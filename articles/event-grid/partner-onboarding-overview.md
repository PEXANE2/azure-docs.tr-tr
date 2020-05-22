---
title: Azure Event Grid iş ortağı olarak ekleme
description: Azure Event Grid Iş ortağı konu türü olarak ekleme. Iş ortağı konuları için kaynak modeli ve yayımlama akışını anlayın.
services: event-grid
author: banisadr
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/18/2020
ms.author: babanisa
ms.openlocfilehash: 2a1f35b86e21099c9fdd0397ae8a3b20aed3cd5d
ms.sourcegitcommit: 0690ef3bee0b97d4e2d6f237833e6373127707a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83758835"
---
# <a name="onboard-as-an-azure-event-grid-partner"></a>Azure Event Grid Iş ortağı olarak ekleme

Bu makalede, Event Grid iş ortağı kaynaklarını özel olarak kullanma ve genel kullanıma açık Iş ortağı konu türü olma konuları açıklanır.

Event Grid iş ortağı olarak yayımlama olayları ile ilişkili Event Grid kaynak türlerini kullanmaya başlamak için özel izninizin olması gerekmez. Aslında, her ikisi de kendi Azure aboneliklerinize özel olarak olay yayımlamak ve bir iş ortağı haline getiriyorsanız kaynak modelini test etmek için kullanabilirsiniz.

## <a name="becoming-an-event-grid-partner"></a>Event Grid iş ortağı olma

Ortak bir Event Grid iş ortağı olmak istiyorsanız, [Bu formu](https://aka.ms/gridpartnerform)doldurarak ve sonra Event Grid ekibine başvurarak ' ı başlatın [GridPartner@microsoft.com](mailto:gridpartner@microsoft.com) .

## <a name="how-partner-topics-work"></a>İş ortağı konuları nasıl çalışır?
İş ortağı konuları depolama ve IoT Hub gibi Azure kaynaklarından olayları yayımlamak için Event Grid zaten kullandığı mevcut mimariyi alır ve bu araçların herkesin kullanmasına açık bir şekilde kullanılabilmesini sağlar. Bu araçların kullanılması, varsayılan olarak yalnızca Azure aboneliğiniz için özeldir. Olaylarınızın genel kullanıma açık olmasını sağlamak için yukarıdaki formu doldurun ve [Event Grid ekibine başvurun](mailto:gridpartner@microsoft.com).

İş ortağı konuları çok kiracılı tüketim için Azure Event Grid olayları yayımlamanıza olanak sağlar.

### <a name="onboarding-and-event-publishing-overview"></a>Ekleme ve olay yayımlamaya genel bakış

#### <a name="partner-flow"></a>İş ortağı akışı

1. Henüz yoksa bir Azure kiracısı oluşturun.
1. CLı kullanarak yeni bir Event Grid oluşturun `partnerRegistration` . Bu kaynak, görünen ad, açıklama, kurulum URI 'SI gibi bilgileri içerir.

    ![Iş ortağı oluşturma konusu](./media/partner-onboarding-how-to/create-partner-registration.png)

1. Olayları yayımlamak istediğiniz her bölgede bir veya daha fazla oluşturun `partnerNamespaces` . Bunun bir parçası olarak, Event Grid hizmet bir yayımlama uç noktası (örneğin, `https://contoso.westus-1.eventgrid.azure.net/api/events` ) ve erişim anahtarları sağlayacak.

    ![Iş ortağı ad alanı oluştur](./media/partner-onboarding-how-to/create-partner-namespace.png)

1. Müşterilerin sisteminize bir iş ortağı konusu gibi kaydolmaları için bir yol sağlayın.
1. Iş ortağı konu tipinin Genel hale gelmesini istediğinizi bize bildirmek için Event Grid ekibine başvurun.

#### <a name="customer-flow"></a>Müşteri akışı

1. Müşteriniz, ' de Iş ortağı konusunun oluşturulmasını istediğiniz Azure abonelik KIMLIĞI ve kaynak grubu ' nu ziyaret edecek Azure portal.
1. Müşteri, sisteminiz aracılığıyla bir Iş ortağı konusu talep eder. Yanıt olarak, Iş ortağı ad alanınızı bir olay tüneli olarak oluşturacaksınız.
1. Event Grid, müşterinin Azure aboneliğinde ve kaynak grubunda **bekleyen** bir Iş ortağı konusu oluşturur.

    ![Olay kanalı oluştur](./media/partner-onboarding-how-to/create-event-tunnel-partner-topic.png)

1. Müşteri, Azure portal aracılığıyla Iş ortağı konusunu etkinleştirir. Olaylar artık hizmetinize müşterinin Azure aboneliğine akabilir.

    ![Iş ortağı konusunu etkinleştirin](./media/partner-onboarding-how-to/activate-partner-topic.png)

## <a name="resource-model"></a>Kaynak modeli

Iş ortağı konuları için kaynak modeli aşağıda verilmiştir.

### <a name="partner-registrations"></a>İş ortağı kayıtları
* Kaynak`partnerRegistrations`
* Kullanan: Iş ortakları
* Açıklama: SaaS ortağının genel meta verilerini yakalar (örneğin, ad, görünen ad, açıklama, kurulum URI 'SI).
    
    İş ortağı kaydı oluşturma/güncelleştirme, iş ortakları için self servis bir işlemdir. Bu self servis özelliği, iş ortaklarının tam uçtan uca akışı oluşturmasını ve test etmesini sağlar.
    
    Yalnızca Microsoft tarafından onaylanan iş ortağı kayıtları müşteriler tarafından bulunabilir.
* Kapsam: iş ortağının Azure aboneliğinde oluşturuldu. Ortak olarak müşterilere görünür metaveri.

### <a name="partner-namespaces"></a>İş ortağı ad alanları
* Kaynak: partnerNamespaces
* Kullanan: Iş ortakları
* Açıklama: müşteri olaylarını yayımlamak için bir bölgesel kaynak sağlar. Her Iş ortağı ad alanı bir yayımlama uç noktası ve kimlik doğrulama anahtarlarına sahiptir. Ad alanı Ayrıca ortağın belirli bir müşteri için bir Iş ortağı konusu istemesi ve etkin müşterileri listelemesidir.
* Kapsam: iş ortağının aboneliğinde bulunur.

### <a name="event-channel"></a>Olay kanalı
* Kaynak`partnerNamespaces/eventChannels`
* Kullanan: Iş ortakları
* Açıklama: olay tünelleri, müşterinin Iş ortağı konusunun bir yansımasıdır. Bir olay tüneli oluşturarak ve müşterinin Azure aboneliğini ve kaynak grubunu meta verilerde belirterek, müşteri için bir Iş ortağı konusu oluşturmak üzere Event Grid sinyalle karşılaşrsınız. Event Grid, müşterinin aboneliğine karşılık gelen bir partneri konusu oluşturmak için ARM çağrısı yayımlayacak. İş ortağı konusu "beklemede" durumunda oluşturulur. Her bir eventTunnel ve partnerTopic arasında bir 1-1 bağlantısı vardır.
* Kapsam: iş ortağının aboneliğinde bulunur.

### <a name="partner-topics"></a>İş ortağı konuları
* Kaynak`partnerTopics`
* Kullanan: müşteriler
* Açıklama: Iş ortağı konuları, Event Grid özel konu ve sistem konusuna benzerdir. Her Iş ortağı konusu belirli bir "kaynak" (örneğin, `Contoso:myaccount` ) ve belirli bir iş ortağı konu türü (örneğin, "contoso") ile ilişkilendirilir. Müşteriler, olayları çeşitli olay işleyicilerine yönlendirmek için Iş ortağı konusunda olay abonelikleri oluşturur.

    Müşteriler bu kaynağı doğrudan oluşturamaz. Iş ortağı konusu oluşturmanın tek yolu, bir olay tüneli oluşturan iş ortağı işlemidir.
* Kapsam: müşterinin aboneliğindeki yaşamın.

### <a name="partner-topic-types"></a>İş ortağı konu türleri
* Kaynak`partnerTopicTypes`
* Kullanan: müşteriler
* Açıklama: Iş ortağı konu türleri, müşterilerin onaylanan iş ortağı konu türleri listesini bulmasını sağlayan kiracı genelinde kaynak türleridir. URL şöyle görünür:https://management.azure.com/providers/Microsoft.EventGrid/partnerTopicTypes)
* Kapsam: genel

## <a name="publishing-events-to-event-grid"></a>Olayları Event Grid yayımlama
Bir Azure bölgesinde bir partnerNamespace oluşturduğunuzda, bölgesel bir uç nokta ve buna karşılık gelen kimlik doğrulama anahtarlarını alırsınız. Bu ad alanındaki tüm müşteri olay tünelleri için olay toplu işlerini bu uç noktaya yayımlayın. Olaydaki "kaynak" alanı temelinde, Azure Event Grid her bir olayı ilgili iş ortağı konusuyla eşler.

### <a name="event-schema-cloudevents-v10"></a>Olay şeması: CloudEvents v 1.0
CloudEvents 1,0 şemasını kullanarak Azure Event Grid olayları yayımlayın. Event Grid hem yapılandırılmış modu hem de toplu modunu destekler. CloudEvents 1,0, Iş ortağı ad alanları için desteklenen tek olay şemadır.

### <a name="example-flow"></a>Örnek akış

1.  Yayımlama hizmeti bir HTTP GÖNDERISINI `https://contoso.westus2-1.eventgrid.azure.net/api/events?api-version=2018-01-01` .
2.  İstekte, kimlik doğrulaması için bir anahtar içeren AEG-SAS-Key adlı bir başlık değeri ekleyin. Bu anahtar, partnerNamespace oluşturma sırasında sağlanır. Örneğin, geçerli bir üst bilgi değeri AEG-SAS-Key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg = = olur.
3.  Content-Type üstbilgisini "Application/cloudevents-Batch + JSON" olarak ayarlayın. charset = UTF-8 ".
4.  Yukarıdaki yayımlama URL 'sinde, bu bölgeye karşılık gelen bir olay toplu iş içeren bir HTTP POST işlemi gerçekleştirin. Örneğin:

``` json
[
{
    "specversion" : "1.0-rc1",
    "type" : "com.contoso.ticketcreated",
    "source" : " com.contoso.account1",
    "subject" : "tickets/123",
    "id" : "A234-1234-1234",
    "time" : "2019-04-05T17:31:00Z",
    "comexampleextension1" : "value",
    "comexampleothervalue" : 5,
    "datacontenttype" : "application/json",
    "data" : {
          object-unique-to-each-publisher
    }
},
{
    "specversion" : "1.0-rc1",
    "type" : "com.contoso.ticketclosed",
    "source" : "https://contoso.com/account2",
    "subject" : "tickets/456",
    "id" : "A234-1234-1234",
    "time" : "2019-04-05T17:31:00Z",
    "comexampleextension1" : "value",
    "comexampleothervalue" : 5,
    "datacontenttype" : "application/json",
    "data" : {
          object-unique-to-each-publisher
    }
}
]
```

PartnerNamespace uç noktasına gönderdikten sonra bir yanıt alırsınız. Yanıt standart bir HTTP yanıt kodudur. Bazı genel yanıtlar şunlardır:

| Sonuç                             | Yanıt              |
|------------------------------------|-----------------------|
| Başarılı                            | 200 TAMAM                |
| Olay verileri hatalı biçime sahip    | 400 Hatalı Istek       |
| Geçersiz erişim anahtarı                 | 401 Yetkisiz      |
| Geçersiz uç nokta                 | 404 Bulunamadı         |
| Dizi veya olay boyut sınırlarını aşıyor | 413 yükü çok büyük |

## <a name="reference"></a>Başvuru

  * [Swagger](https://github.com/ahamad-MS/azure-rest-api-specs/blob/master/specification/eventgrid/resource-manager/Microsoft.EventGrid/preview/2020-04-01-preview/EventGrid.json)
  * [ARM şablonu](https://docs.microsoft.com/azure/templates/microsoft.eventgrid/allversions)
  * [ARM şablon şeması](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2020-04-01-preview/Microsoft.EventGrid.json)
  * [REST API'leri](https://docs.microsoft.com/rest/api/eventgrid/version2020-04-01-preview/partnernamespaces)
  * [CLı uzantısı](https://docs.microsoft.com/cli/azure/ext/eventgrid/?view=azure-cli-latest)

### <a name="sdks"></a>SDK
  * [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.EventGrid/5.3.1-preview)
  * [Python](https://pypi.org/project/azure-mgmt-eventgrid/3.0.0rc6/)
  * [Java](https://search.maven.org/artifact/com.microsoft.azure.eventgrid.v2020_04_01_preview/azure-mgmt-eventgrid/1.0.0-beta-3/jar)
  * [Ruby](https://rubygems.org/gems/azure_mgmt_event_grid/versions/0.19.0)
  * [JS](https://www.npmjs.com/package/@azure/arm-eventgrid/v/7.0.0)
  * [Git](https://github.com/Azure/azure-sdk-for-go)


## <a name="next-steps"></a>Sonraki Adımlar
- [İş ortağı konularına genel bakış](partner-topics-overview.md)
- [İş ortağı konuları ekleme formu](https://aka.ms/gridpartnerform)
- [Auth0 Iş ortağı konusu](auth0-overview.md)
- [Auth0 Iş ortağı konusunu kullanma](auth0-how-to.md)
