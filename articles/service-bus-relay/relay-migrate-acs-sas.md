---
title: Azure Röle - Paylaşılan Erişim İmza sıfatına geçir
description: Azure Röle uygulamalarının Azure Active Directory Access Control Service'i kullanımından Paylaşılan Erişim İmzasıfatı yetkilendirmesine nasıl geçirilmeye başlanması gerektiğini açıklar.
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
ms.date: 01/21/2020
ms.author: spelluru
ms.openlocfilehash: 59b9e734526c56016e2ddf59c2afb5b8f7b4ad09
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76514910"
---
# <a name="azure-relay---migrate-from-azure-active-directory-access-control-service-to-shared-access-signature-authorization"></a>Azure Röle - Azure Active Directory Access Control Service'den Paylaşılan Erişim İmza sıfatına geçiş

Azure Röle uygulamaları tarihsel olarak iki farklı yetkilendirme modeli kullanma seçeneğine sahipti: Doğrudan Röle hizmeti tarafından sağlanan [Paylaşılan Erişim İmzası (SAS)](../service-bus-messaging/service-bus-sas.md) belirteç modeli ve yetkilendirme kurallarının yönetiminin [Azure Active Directory](/azure/active-directory/) Access Control Service (ACS) tarafından içeride yönetildiği federe bir model ve ACS'den elde edilen jetonlar istenen özelliklere erişim yetkisi vermek için Relay'e geçirilir.

ACS yetkilendirme modeli uzun tercih edilen model olarak [SAS yetkilendirme](../service-bus-messaging/service-bus-authentication-and-authorization.md) tarafından yerini almıştır ve tüm belgeler, rehberlik ve örnekler sadece bugün SAS kullanın. Ayrıca, artık ACS ile eşleştirilmiş yeni Röle ad alanları oluşturmak mümkün değildir.

SAS, hemen başka bir hizmete bağlı olmaması, ancak istemciye SAS kural adı ve kural anahtarına erişim sağlayarak herhangi bir aracı olmadan doğrudan istemciden kullanılabilmesi açısından avantaja sahiptir. SAS, istemcinin önce başka bir hizmetle yetkilendirme denetimi yapmak zorunda olduğu ve daha sonra bir belirteç verildiği bir yaklaşımla da kolayca entegre edilebilir. İkinci yaklaşım ACS kullanım desenine benzer, ancak ACS'de ifade etmesi zor olan uygulamaya özgü koşullara dayalı erişim belirteçleri verilmesine olanak tanır.

ACS'ye bağımlı olan tüm mevcut uygulamalar için, müşterilerin uygulamalarını yerine SAS'a güvenmeleri için geçirmelerini öneririz.

## <a name="migration-scenarios"></a> Geçiş senaryoları

ACS ve Röle, imza *anahtarının*ortak bilgisi ile entegre edilmiştir. İmza anahtarı, yetkilendirme belirteçlerini imzalamak için bir ACS ad alanı tarafından kullanılır ve belirteçlerin eşleştirilmiş ACS ad alanı tarafından verildiğini doğrulamak için Azure Relay tarafından kullanılır. ACS ad alanı hizmet kimlikleri ve yetkilendirme kuralları tutar. Yetkilendirme kuralları, hangi hizmet kimliğinin veya harici bir kimlik sağlayıcısı tarafından verilen belirteci, röle ad alanı grafiğinin bir bölümüne en uzun önek eşleşmesi biçiminde hangi tür erişim elde ediliş olduğunu tanımlar.

Örneğin, bir ACS kuralı, **Send** bir hizmet kimliğine `/` yol önekiyle ilgili Gönder talebini verebilir, bu da ACS tarafından bu kurala dayalı olarak verilen bir belirteç istemciye ad alanındaki tüm varlıklara gönderme hakkı verdiği anlamına gelir. Yol öneki ise, `/abc`kimlik bu önek altında `abc` adlı veya düzenlenen varlıklara gönderme ile sınırlıdır. Bu geçiş kılavuzunun okuyucularının bu kavramlara zaten aşina oldukları varsayılmaktadır.

Geçiş senaryoları üç geniş kategoriye ayrılır:

1.  **Değişmeyen varsayılanlar.** Bazı müşteriler, otomatik olarak oluşturulan **sahibi** hizmet kimliğini ve ACS ad alanı için gizli anahtarını geçirerek, Relay ad alanıyla eşleştirilmiş bir [SharedSecretTokenProvider](/dotnet/api/microsoft.servicebus.sharedsecrettokenprovider) nesnesi kullanır ve yeni kurallar eklemez.

2.  **Basit kurallara sahip özel hizmet kimlikleri.** Bazı müşteriler yeni hizmet kimlikleri ekler ve her yeni hizmet **kimliği**ne zaman gönder, **dinle**ve belirli bir tüzel kişi için izinleri **yönet** verir.

3.  **Karmaşık kuralları ile özel hizmet kimlikleri.** Çok az müşteri, dışarıdan verilen belirteçlerin Relay'deki haklara eşlendiği veya birden çok kural aracılığıyla birden çok ad alanı yolunda farklılaştırılmış haklar atandığı karmaşık kural kümelerine sahiptir.

Karmaşık kural kümelerinin geçişiyle ilgili yardım için [Azure desteğine](https://azure.microsoft.com/support/options/)başvurabilirsiniz. Diğer iki senaryo basit geçişi etkinleştirin.

### <a name="unchanged-defaults"></a>Değişmemiş varsayılanlar

Uygulamanız ACS varsayılanlarını değiştirmediyse, tüm [SharedSecretTokenProvider](/dotnet/api/microsoft.servicebus.sharedsecrettokenprovider) kullanımını bir [SharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) nesnesi ile değiştirebilir ve ACS **sahibi** hesabı yerine ad alanı önceden yapılandırılmış **RootManageSharedAccessKey'i** kullanabilirsiniz. BU hesap/kural, herhangi bir varlığı silme izni de dahil olmak üzere ad alanı üzerinde tam yönetim yetkisi sağladığından, ACS **sahibi** hesabında bile bu yapılandırmanın genellikle önerilmediğini (ve hala da önerilmediğini) unutmayın.

### <a name="simple-rules"></a>Basit kurallar

Uygulama basit kurallarla özel hizmet kimlikleri kullanıyorsa, belirli bir rölede erişim denetimi sağlamak için acs hizmet kimliğinin oluşturulduğu durumlarda geçiş basittir. Bu senaryo genellikle, her röle bir kiracı siteye veya şubeye köprü olarak kullanıldığı ve hizmet kimliğinin söz konusu site için oluşturulduğu SaaS tarzı çözümlerde sıklıkla durumdur. Bu durumda, ilgili hizmet kimliği doğrudan röle üzerinde Paylaşılan Erişim İmza kuralına geçirilebilir. Hizmet kimlik adı SAS kural adı olabilir ve hizmet kimlik anahtarı SAS kural anahtarı olabilir. SAS kuralının hakları daha sonra varlık için sırasıyla geçerli ACS kuralına eşdeğer olarak yapılandırılır.

Bu yeni ve ek SAS yapılandırmasını ACS ile beslenen varolan herhangi bir ad alanında yerinde yapabilirsiniz ve ACS'den uzak geçiş daha sonra [SharedSecretTokenProvider](/dotnet/api/microsoft.servicebus.sharedsecrettokenprovider)yerine [SharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) kullanılarak gerçekleştirilir. Ad alanının ACS'den bağlantısının kalınması gerekmez.

### <a name="complex-rules"></a>Karmaşık kurallar

SAS kuralları nın hesap olması amaçlanmaz, ancak haklarla ilişkili imzalama anahtarları olarak adlandırılır. Bu nedenle, uygulamanın birçok hizmet kimliği oluşturduğu ve onlara birden fazla varlık veya tüm ad alanı için erişim hakları verdiği senaryolar yine de belirteç veren bir aracı gerektirir. Böyle bir aracı için destek le [iletişimkurarak](https://azure.microsoft.com/support/options/)rehberlik alabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Röle kimlik doğrulaması hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Azure Röle kimlik doğrulaması ve yetkilendirme](relay-authentication-and-authorization.md)
* [Paylaşılan Erişim İmzaları ile Servis Veri Aracı kimlik doğrulaması](../service-bus-messaging/service-bus-sas.md)


