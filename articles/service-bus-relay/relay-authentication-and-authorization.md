---
title: Azure Röle kimlik doğrulama ve yetkilendirme | Microsoft Dokümanlar
description: Bu makalede, Azure Röle hizmetiyle Paylaşılan Erişim İmzası (SAS) kimlik doğrulaması hakkında genel bir bakış sağlanmaktadır.
services: service-bus-relay
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/21/2020
ms.author: spelluru
ms.openlocfilehash: aac5c973a99b13d5918a0162feb7f1ede443463b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76514587"
---
# <a name="azure-relay-authentication-and-authorization"></a>Azure Röle kimlik doğrulaması ve yetkilendirme

Uygulamalar, Paylaşılan Erişim İmzası (SAS) kimlik doğrulamasını kullanarak Azure Relay'e kimlik doğrulaması yapabilir. SAS kimlik doğrulaması, Relay ad alanında yapılandırılan bir erişim anahtarını kullanarak uygulamaların Azure Röle hizmetine kimlik doğrulaması yapılmasını sağlar. Daha sonra bu anahtarı, istemcilerin geçiş hizmetine kimlik doğrulamak için kullanabileceği paylaşılan erişim imzası belirteci oluşturmak için kullanabilirsiniz.

## <a name="shared-access-signature-authentication"></a>Paylaşılan Erişim İmza kimlik doğrulaması

[SAS kimlik doğrulaması,](../service-bus-messaging/service-bus-sas.md) belirli haklara sahip Azure Röle kaynaklarına kullanıcı erişimi sağlamanıza olanak tanır. SAS kimlik doğrulaması, bir kaynak üzerinde ilişkili haklara sahip bir şifreleme anahtarıyapılandırmasını içerir. İstemciler daha sonra, URI'ye erişilen kaynaktan ve yapılandırılan anahtarla imzalanan bir son kullanma süresinden oluşan bir SAS belirteci sunarak bu kaynağa erişebilirler.

SAS için anahtarları Röle ad alanında yapılandırabilirsiniz. Service Bus iletisinin aksine, [Relay Hybrid Connections](relay-hybrid-connections-protocol.md) yetkisiz veya anonim gönderenleri destekler. Portaldan aşağıdaki ekran görüntüsünde gösterildiği gibi, oluşturduğunuzda varlık için anonim erişimi etkinleştirebilirsiniz:

![][0]

SAS'ı kullanmak için, aşağıdakilerden oluşan bir Röle ad alanında bir [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) nesnesini yapılandırabilirsiniz:

* Kuralı tanımlayan *Anahtar Adı.*
* *PrimaryKey,* SAS belirteçlerini imzalamak/doğrulamak için kullanılan bir şifreleme anahtarıdır.
* *SecondaryKey,* SAS belirteçlerini imzalamak/doğrulamak için kullanılan bir şifreleme anahtarıdır.
* Verilen dinleme, gönderme veya yönetme haklarının toplanmasını *temsil* eden haklar.

Ad alanı düzeyinde yapılandırılan yetkilendirme kuralları, ilgili anahtar kullanılarak imzalanan belirteçleri olan istemciler için ad alanındaki tüm röle bağlantılarına erişim sağlayabilir. Bu tür yetkilendirme kuralları 12'ye kadar Relay ad alanında yapılandırılabilir. Varsayılan olarak, tüm hakları olan bir [SharedAccessAuthorizationRule,](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) ilk olarak sağlandığında her ad alanı için yapılandırılır.

Bir varlığa erişmek için istemci, belirli bir [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule)kullanılarak oluşturulan bir SAS belirteci gerektirir. SAS belirteci, erişimin talep edildiği kaynak URI'den oluşan bir kaynak dizesinin HMAC-SHA256'sı ve yetkilendirme kuralıyla ilişkili bir şifreleme anahtarıyla son kullanma tarihi kullanılarak oluşturulur.

Azure Röle için SAS kimlik doğrulama desteği, Azure .NET SDK sürümleri 2.0 ve sonraki sürümlerde yer almaktadır. SAS, [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule)desteği içerir. Bir bağlantı dizesi parametre olarak kabul eden tüm API'ler SAS bağlantı dizeleri için destek içerir.

## <a name="next-steps"></a>Sonraki adımlar

- SAS hakkında daha fazla bilgi için [Paylaşılan Erişim İmzaları ile Hizmet Veri Aracı kimlik doğrulamasını](../service-bus-messaging/service-bus-sas.md) okumaya devam edin.
- Karma Bağlantılar özelliği hakkında ayrıntılı bilgi için [Azure Röle Karma Bağlantılar protokolü kılavuzuna](relay-hybrid-connections-protocol.md) bakın.
- Servis Veri Servisi İleti kimlik doğrulaması ve yetkilendirmesi hakkında ilgili bilgiler için [Servis Veri Servisi kimlik doğrulaması ve yetkilendirmesi'ne](../service-bus-messaging/service-bus-authentication-and-authorization.md)bakın. 

[0]: ./media/relay-authentication-and-authorization/hcanon.png