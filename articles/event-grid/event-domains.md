---
title: Azure Olay Tablosunda Etkinlik Etki Alanları
description: Bu makalede, çeşitli iş kuruluşları, müşteriler veya uygulamalar için özel olayların akışını yönetmek için olay etki alanları nasıl kullanılacağı açıklanmaktadır.
services: event-grid
author: banisadr
ms.service: event-grid
ms.author: babanisa
ms.topic: conceptual
ms.date: 01/21/2020
ms.openlocfilehash: f6698f91d7659f9fc2c314a9291380301146f8ed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78898859"
---
# <a name="understand-event-domains-for-managing-event-grid-topics"></a>Olay Izgara konularını yönetmek için olay etki alanlarını anlama

Bu makalede, çeşitli iş kuruluşları, müşteriler veya uygulamalar için özel olayların akışını yönetmek için olay etki alanları nasıl kullanılacağı açıklanmaktadır. Etkinlik etki alanlarını şu şekilde kullanın:

* Çok kiracılı olay mimarilerini ölçekte yönetin.
* Yetkilendirmenizi ve kimlik doğrulamanızı yönetin.
* Her biri ayrı ayrı yönetmeden konularınızı bölümlere ayırır.
* Konu bitiş noktalarınızın her birine ayrı ayrı yayımlamaktan kaçının.

## <a name="event-domain-overview"></a>Olay etki alanına genel bakış

Olay etki alanı, aynı uygulamayla ilgili çok sayıda Olay Izgara konusu için bir yönetim aracıdır. Bunu binlerce ayrı konuya sahip olabilecek bir meta-konu olarak düşünebilirsiniz.

Olay etki alanları, etkinliklerini yayımlamak için Azure hizmetleri (Depolama ve IoT Hub gibi) tarafından kullanılan aynı mimariyi kullanıma sunar. Binlerce konuya etkinlik yayınlamanızı sağlarlar. Etki alanları, kiracılarınızı bölümlere alabilmeniz için her konu üzerinde yetkilendirme ve kimlik doğrulama denetimi de sağlar.

### <a name="example-use-case"></a>Örnek kullanım örneği

Olay etki alanları en kolay bir örnek kullanılarak açıklanabilir. Diyelim ki, traktör, kazma ekipmanı ve diğer ağır makineleri ürettiğiniz Contoso Construction Machinery'i işletiyorsunuz. İşletmenin bir parçası olarak, müşterilere ekipman bakımı, sistem sağlığı ve sözleşme güncelleştirmeleri hakkında gerçek zamanlı bilgi aktarAbilirsiniz. Tüm bu bilgiler uygulamanız, müşteri uç noktalarınız ve müşterilerin ayarlamış olduğu diğer altyapı dahil olmak üzere çeşitli uç noktalara gider.

Olay etki alanları, Contoso Construction Machinery'i tek bir olaylı varlık olarak modellemenize olanak sağlar. Müşterilerinizin her biri etki alanı içinde bir konu olarak temsil edilir. Kimlik doğrulama ve yetkilendirme Azure Etkin Dizin kullanılarak işlenir. Müşterilerinizin her biri kendi konusuna abone olabilir ve etkinliklerini kendilerine teslim edebilirsiniz. Olay etki alanı üzerinden yönetilen erişim, yalnızca kendi konusuna erişebilmesini sağlar.

Ayrıca, tüm müşteri etkinliklerinizi yayınlayabileceğiniz tek bir bitiş noktası da sağlar. Olay Grid, her konunun yalnızca kiracısına yönelik olaylardan haberdar olduğundan emin olmaya özen decektir.

![Contoso İnşaat Örneği](./media/event-domains/contoso-construction-example.png)

## <a name="access-management"></a>Erişim yönetimi

Bir etki alanıyla, Azure'un rol tabanlı erişim denetimi (RBAC) aracılığıyla her konu üzerinde ince tane yetkilendirme ve kimlik doğrulama denetimi elde elabilirsiniz. Bu rolleri, uygulamanızdaki her kiracıyı yalnızca onlara erişim izni vermek istediğiniz konularla sınırlamak için kullanabilirsiniz.

RBAC, etki alanlarının Olay Ağı ve Azure'un geri kalanında [yönetilen erişim denetiminin](security-authorization.md) çalıştığı şekilde çalışması durumunda çalışır. Olay etki alanlarında özel rol tanımları oluşturmak ve uygulamak için RBAC'ı kullanın.

### <a name="built-in-roles"></a>Rollerde yerleşik

Olay Izgarası, RBAC'ı olay etki alanlarıyla çalışmayı kolaylaştırmak için iki yerleşik rol tanımına sahiptir. Bu roller **EventGrid EventSubscription Katılımcı (Önizleme)** ve **EventGrid EventSubscription Reader (Önizleme)** vardır. Bu rolleri etkinlik etki alanınızdaki konulara abone olması gereken kullanıcılara atarsınız. Rol atamasını yalnızca kullanıcıların abone olması gereken konuya göre kapsamda

Bu roller hakkında bilgi için [Olay Izgarası için Yerleşik rollere](security-authorization.md#built-in-roles)bakın.

## <a name="subscribing-to-topics"></a>Konulara abone olma

Olay etki alanındaki bir konuyla ilgili olaylara abone olmak, [özel bir konuda Etkinlik Aboneliği oluşturmak](./custom-event-quickstart.md) veya bir Azure hizmetinden bir etkinliğe abone olmakla aynıdır.

### <a name="domain-scope-subscriptions"></a>Etki alanı kapsamı abonelikleri

Olay etki alanları da etki alanı kapsamı abonelikleri için izin verir. Bir etkinlik etki alanında bir olay aboneliği, olayların gönderildiği konu ne olursa olsun etki alanına gönderilen tüm etkinlikleri alır. Etki alanı kapsamı abonelikleri yönetim ve denetim amaçları için yararlı olabilir.

## <a name="publishing-to-an-event-domain"></a>Etkinlik etki alanına yayımlama

Bir olay etki alanı oluşturduğunuzda, Olay Izgara'sında bir konu oluşturmuş sanız, benzer bir yayımlama bitiş noktası verilir. 

Olay Etki Alanı'ndaki herhangi bir konuya olayları yayımlamak [için,](./post-to-custom-topic.md)olayları etki alanının bitiş noktasına, özel bir konu için yaptığınız gibi itin. Tek fark, etkinliğin teslim edilmesini istediğiniz konuyu belirtmeniz gerektiğidir.

Örneğin, aşağıdaki olaylar dizisini yayımlamak `"id": "1111"` olay `foo` konusunu topa `"id": "2222"` gönderirken, `bar`olay ile ilgili konuya gönderilir:

```json
[{
  "topic": "foo",
  "id": "1111",
  "eventType": "maintenanceRequested",
  "subject": "myapp/vehicles/diggers",
  "eventTime": "2018-10-30T21:03:07+00:00",
  "data": {
    "make": "Contoso",
    "model": "Small Digger"
  },
  "dataVersion": "1.0"
},
{
  "topic": "bar",
  "id": "2222",
  "eventType": "maintenanceCompleted",
  "subject": "myapp/vehicles/tractors",
  "eventTime": "2018-10-30T21:04:12+00:00",
  "data": {
    "make": "Contoso",
    "model": "Big Tractor"
  },
  "dataVersion": "1.0"
}]
```

Olay etki alanları sizin için konulara yayımlama işleyebilir. Tek tek yönettiğiniz her konuya etkinlik yayımlamak yerine, tüm etkinliklerinizi etki alanının bitiş noktasına yayımlayabilirsiniz. Olay Izgaraher olay doğru konuya gönderilir emin olun.

## <a name="limits-and-quotas"></a>Limitler ve kotalar
Olay etki alanlarıyla ilgili sınırlar ve kotalar şunlardır:

- Etkinlik etki alanı başına 100.000 konu 
- Azure aboneliği başına 100 etkinlik alanı 
- Bir olay etki alanında konu başına 500 etkinlik aboneliği
- 50 etki alanı kapsamı aboneliği 
- Saniyede 5.000 olay (bir etki alanına)

Bu sınırlar size uymuyorsa, bir destek bileti açarak veya e-posta [askgrid@microsoft.com](mailto:askgrid@microsoft.com)göndererek ürün ekibine ulaşın. 

## <a name="pricing"></a>Fiyatlandırma
Olay etki alanları, Olay Izgara'daki diğer tüm özelliklerin kullandığı aynı [işlem fiyatlandırmasını](https://azure.microsoft.com/pricing/details/event-grid/) kullanır.

İşlemler, olay etki alanlarında özel konularda olduğu gibi aynı şekilde çalışır. Bir olayın bir olay etki alanına girişi bir işlemdir ve bir olay için her teslim girişimi bir işlemdir.

## <a name="next-steps"></a>Sonraki adımlar

* Etkinlik etki alanlarını ayarlama, konu oluşturma, etkinlik abonelikleri oluşturma ve etkinlik yayımlama hakkında bilgi edinmek için [bkz.](./how-to-event-domains.md)
