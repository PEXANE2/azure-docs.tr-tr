---
title: Azure Relay-paylaşılan erişim Imzası yetkilendirmesi 'ne geçiş
description: Access Control Service uygulamaları SAS 'ye geçirme
services: service-bus-relay
documentationcenter: ''
author: clemensv
manager: timlt
editor: ''
ms.service: service-bus-relay
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2019
ms.author: spelluru
ms.openlocfilehash: 8aec2483f39f698a62be60f6da6018f8981df423
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75355042"
---
# <a name="azure-relay---migrate-from-azure-active-directory-access-control-service-to-shared-access-signature-authorization"></a>Azure Relay-Azure Active Directory Access Control Service, paylaşılan erişim Imzası yetkilendirmesi 'ne geçirme

Azure Relay uygulamalarda iki farklı yetkilendirme modeli kullanma seçeneği vardı: doğrudan geçiş hizmeti tarafından sunulan [paylaşılan erişim imzası (SAS)](../service-bus-messaging/service-bus-sas.md) belirteç modeli ve yetkilendirme kurallarının yönetiminin [Azure ACTIVE DIRECTORY](/azure/active-directory/) Access Control Service (ACS) tarafından yönetildiği BIR Federasyon modeli ve ACS 'den alınan belirteçler, istenen özelliklere erişim yetkisi veren geçişe geçirilir.

ACS yetkilendirme modelinin süresi, tercih edilen model olarak [SAS yetkilendirmesi](../service-bus-messaging/service-bus-authentication-and-authorization.md) ile değiştirilmiştir ve tüm belgeler, kılavuzluk ve örnekler günümüzde SAS kullanır. Üstelik, artık ACS ile eşleştirilmiş yeni geçiş ad alanları oluşturmak mümkün değildir.

SAS kural adı ve kural anahtarına istemci erişimi vererek, SAS, başka bir hizmete hemen bağımlı olmasa da, hiçbir aracı olmadan doğrudan bir istemciden doğrudan kullanılabilir. SAS ayrıca, bir istemcinin ilk olarak başka bir hizmetle bir yetkilendirme denetimi geçirmesi gereken bir yaklaşım ile kolayca tümleştirilebilir ve ardından bir belirteç verilir. İkinci yaklaşım ACS kullanım düzenine benzerdir, ancak ACS 'de hızlı bir şekilde ifade edilen uygulamaya özgü koşullara göre erişim belirteçleri verilmesine olanak sağlar.

ACS 'ye bağlı olan tüm mevcut uygulamalar için, müşterileri uygulamalarını SAS 'ye göre geçirmeye geçiririz.

## <a name="migration-scenarios"></a>Geçiş senaryoları

ACS ve geçiş, bir *İmzalama anahtarının*paylaşılan bilgisi aracılığıyla tümleşiktir. İmzalama anahtarı, bir ACS ad alanı tarafından, yetkilendirme belirteçlerini imzalamak için kullanılır ve belirtecin eşleştirilmiş ACS ad alanı tarafından verildiğini doğrulamak için Azure Relay tarafından kullanılır. ACS ad alanı hizmet kimliklerini ve yetkilendirme kurallarını barındırır. Yetkilendirme kuralları, hangi hizmet kimliğinin veya bir dış kimlik sağlayıcısı tarafından verilen belirtecin, en uzun ön ek eşleşme biçiminde geçiş ad alanı grafiğinin bir bölümüne hangi tür erişimi olduğunu tanımlar.

Örneğin, bir ACS kuralı, bir hizmet kimliğine `/` yol ön ekine **gönderilen** isteği verebilir, bu da bu KURALA göre ACS tarafından verilen belirtecin, istemci haklarının ad alanındaki tüm varlıklara gönderilmesini sağlar. Yol ön eki `/abc`ise, kimlik, bu ön ek altında `abc` olarak adlandırılan varlıklara gönderilmeye kısıtlıdır. Bu geçiş kılavuzunun okuyucularının bu kavramlarla zaten tanıdık olduğu varsayılır.

Geçiş senaryoları üç geniş kategoriye ayrılır:

1.  **Değiştirilmemiş varsayılanlar**. Bazı müşteriler bir [Sharedsecrettokenprovider](/dotnet/api/microsoft.servicebus.sharedsecrettokenprovider) nesnesi kullanarak, otomatik olarak oluşturulan **sahip** hizmet kimliğini ve ACS ad alanı için gizli anahtarını geçirerek, geçiş ad alanıyla eşleştirilmiş ve yeni kurallar eklemez.

2.  **Basit kurallara sahip özel hizmet kimlikleri**. Bazı müşteriler yeni hizmet kimlikleri ekler ve her yeni hizmet kimliğini belirli bir varlık için **gönderme**, **dinleme**ve **yönetme** izinleri verir.

3.  **Karmaşık kurallara sahip özel hizmet kimlikleri**. Çok az sayıda müşteri, dışarıdan verilen belirteçlerin geçiş üzerindeki haklara eşlendiği veya tek bir hizmet kimliğinin birden çok kural aracılığıyla çeşitli ad alanı yollarında farklılaştırılan haklar atandığı karmaşık kural kümelerine sahiptir.

Karmaşık kural kümelerinin geçişine ilişkin yardım için [Azure desteğine](https://azure.microsoft.com/support/options/)başvurabilirsiniz. Diğer iki senaryo doğrudan geçişe olanak tanır.

### <a name="unchanged-defaults"></a>Değiştirilmemiş varsayılanlar

Uygulamanız ACS varsayılanlarını değiştirmediyse, tüm [Sharedsecrettokenprovider](/dotnet/api/microsoft.servicebus.sharedsecrettokenprovider) kullanımını bir [Sharedaccesssignaturetokenprovider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) nesnesiyle değiştirebilir ve ACS **Owner** hesabı yerine önceden yapılandırılmış **RootManageSharedAccessKey** ad alanını kullanabilirsiniz. Bu hesap/kural ad alanı üzerinde tüm varlıkları silme izni de dahil olmak üzere tam yönetim yetkilisi sağladığından, ACS **sahip** hesabıyla bile bu yapılandırmanın (ve yine de) önerildiğine unutmayın.

### <a name="simple-rules"></a>Basit kurallar

Uygulama basit kurallarla özel hizmet kimlikleri kullanıyorsa, geçiş işlemi, belirli bir geçişte erişim denetimi sağlamak üzere bir ACS hizmet kimliğinin oluşturulduğu durumda basittir. Bu senaryo genellikle her bir geçişinin kiracı sitesi veya şube ofisi için bir köprü olarak kullanıldığı ve bu belirli site için hizmet kimliğinin oluşturulduğu SaaS stili çözümlerde bir durumdur. Bu durumda, ilgili hizmet kimliği, doğrudan geçiş üzerinde bir paylaşılan erişim Imza kuralına geçirilebilir. Hizmet kimliği adı SAS kuralı adı olabilir ve hizmet kimliği anahtarı SAS kural anahtarı olabilir. SAS kuralının hakları daha sonra varlık için sırasıyla geçerli ACS kuralına eşit olarak yapılandırılır.

ACS ile federe olan mevcut bir ad alanı üzerinde bu yeni ve ek SAS yapılandırmalarını yerinde yapabilir ve ACS 'den uzağa geçiş daha sonra [Sharedsecrettokenprovider](/dotnet/api/microsoft.servicebus.sharedsecrettokenprovider)yerine [Sharedaccesssignaturetokenprovider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) kullanılarak gerçekleştirilir. Ad alanının ACS 'den bağlantısının kesilme gereksinimi yoktur.

### <a name="complex-rules"></a>Karmaşık kurallar

SAS kurallarının hesap olmaması gerekmez, ancak haklarıyla ilişkili İmzalama anahtarları adlandırılmaktadır. Bu nedenle, uygulamanın birçok hizmet kimliği oluşturduğu ve bunlara çeşitli varlıklar için erişim hakları veren senaryolar veya tüm ad alanı hala belirteç veren bir aracı gerektirir. [Destek ile iletişime geçerek](https://azure.microsoft.com/support/options/)böyle bir aracı için rehberlik elde edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Relay kimlik doğrulaması hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Azure Relay kimlik doğrulaması ve yetkilendirme](relay-authentication-and-authorization.md)
* [Paylaşılan erişim Imzaları ile kimlik doğrulamasını Service Bus](../service-bus-messaging/service-bus-sas.md)


