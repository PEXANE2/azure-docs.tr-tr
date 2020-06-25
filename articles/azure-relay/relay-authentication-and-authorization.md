---
title: Kimlik doğrulaması ve yetkilendirme Azure Relay | Microsoft Docs
description: Bu makalede, Azure Relay hizmeti ile paylaşılan erişim Imzası (SAS) kimlik doğrulamasına ilişkin bir genel bakış sunulmaktadır.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 63e075bc9bf75005a92866f9fa0f90ddaba2f016
ms.sourcegitcommit: 01cd19edb099d654198a6930cebd61cae9cb685b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85316938"
---
# <a name="azure-relay-authentication-and-authorization"></a>Azure Relay kimlik doğrulaması ve yetkilendirme

Uygulamalar, paylaşılan erişim Imzası (SAS) kimlik doğrulaması kullanarak Azure Relay kimlik doğrulaması yapabilir. SAS kimlik doğrulaması, uygulamaların geçiş ad alanında yapılandırılmış bir erişim anahtarı kullanarak Azure Relay hizmetinde kimlik doğrulaması yapmasına olanak sağlar. Daha sonra bu anahtarı, istemcilerin geçiş hizmetinde kimlik doğrulamak için kullanabileceği bir paylaşılan erişim Imza belirteci oluşturmak için kullanabilirsiniz.

## <a name="shared-access-signature-authentication"></a>Paylaşılan erişim Imzası kimlik doğrulaması

[SAS kimlik doğrulaması](../service-bus-messaging/service-bus-sas.md) , kullanıcıya belirli haklara sahip Azure Relay kaynaklarına erişim izni vermenizi sağlar. SAS kimlik doğrulaması, bir kaynakta ilişkili haklara sahip bir şifreleme anahtarı yapılandırmasını içerir. İstemciler, erişilmekte olan kaynak URI 'sinden ve yapılandırılan anahtarla imzalanmış bir süre sonu olan bir SAS belirteci sunarak bu kaynağa erişim elde edebilir.

Bir geçiş ad alanında SAS için anahtarlar yapılandırabilirsiniz. Service Bus mesajından farklı olarak, [geçiş karma bağlantılar](relay-hybrid-connections-protocol.md) yetkisiz veya anonim Gönderenler destekler. Portaldan aşağıdaki ekran görüntüsünde gösterildiği gibi, oluşturduğunuzda varlık için anonim erişimi etkinleştirebilirsiniz:

![][0]

SAS kullanmak için, aşağıdakilerden oluşan bir geçiş ad alanı üzerinde bir [Sharedaccessauthorizationrule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) nesnesi yapılandırabilirsiniz:

* Kuralı tanımlayan *KeyName* .
* *PrimaryKey* , SAS belirteçlerini imzalamak/doğrulamak için kullanılan bir şifreleme anahtarıdır.
* *Secondarykey* , SAS belirteçlerini imzalamak/doğrulamak için kullanılan bir şifreleme anahtarıdır.
* Dinleme, gönderme veya yönetme haklarının toplanmasını temsil eden *haklar* .

Ad alanı düzeyinde yapılandırılan yetkilendirme kuralları, ilgili anahtar kullanılarak imzalanmış belirteçleri olan istemciler için bir ad alanındaki tüm geçiş bağlantılarına erişim izni verebilir. Bir geçiş ad alanı üzerinde en fazla 12 tür yetkilendirme kuralı yapılandırılabilir. Varsayılan olarak, tüm haklara sahip bir [Sharedaccessauthorizationrule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) , ilk sağlandığı sırada her ad alanı için yapılandırılır.

Bir varlığa erişmek için, istemci belirli bir [Sharedaccessauthorizationrule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule)kullanılarak oluşturulan bir SAS belirteci gerektirir. SAS belirteci, erişimi talep edilen kaynak URI 'sinden ve yetkilendirme kuralıyla ilişkili bir şifreleme anahtarıyla bir süre sonu olan bir kaynak dizesinin HMAC-SHA256 kullanılarak oluşturulur.

Azure Relay için SAS kimlik doğrulama desteği, Azure .NET SDK 2,0 ve üzeri sürümlerine dahildir. SAS, [Sharedaccessauthorizationrule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule)için destek içerir. Parametre olarak bir bağlantı dizesini kabul eden tüm API 'Ler SAS bağlantı dizeleri için destek içerir.

## <a name="next-steps"></a>Sonraki adımlar

- SAS hakkında daha fazla bilgi için [paylaşılan erişim imzalarıyla Service Bus kimlik doğrulaması](../service-bus-messaging/service-bus-sas.md) okumaya devam edin.
- Karma Bağlantılar özelliği hakkında ayrıntılı bilgi için [Azure Relay karma bağlantılar protokol kılavuzuna](relay-hybrid-connections-protocol.md) bakın.
- Service Bus mesajlaşma kimlik doğrulaması ve yetkilendirmesi hakkında ilgili bilgiler için bkz. [Service Bus kimlik doğrulaması ve yetkilendirme](../service-bus-messaging/service-bus-authentication-and-authorization.md). 

[0]: ./media/relay-authentication-and-authorization/hcanon.png