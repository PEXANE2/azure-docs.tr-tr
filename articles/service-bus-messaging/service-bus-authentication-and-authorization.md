---
title: Azure Hizmet Veri Servisi kimlik doğrulaması ve yetkilendirmesi | Microsoft Dokümanlar
description: Paylaşılan Erişim İmzası (SAS) kimlik doğrulaması yla Hizmet Veri Servisi'ne uygulamaların kimliğini doğrulayın.
services: service-bus-messaging
documentationcenter: na
author: axisc
editor: spelluru
ms.assetid: 18bad0ed-1cee-4a5c-a377-facc4785c8c9
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2019
ms.author: aschhab
ms.openlocfilehash: 7234e33c04e742c77630f8d87481c7831fb00bf2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70013237"
---
# <a name="service-bus-authentication-and-authorization"></a>Service Bus kimlik doğrulaması ve yetkilendirme

Uygulamalar, Paylaşılan Erişim İmzası (SAS) belirteç kimlik doğrulamasını kullanarak Azure Hizmet Veri Servisi kaynaklarına erişim sağlar. SAS ile, başvurular, hem belirteç veren kuruluş hem de Servis Veri Servisi (dolayısıyla "paylaşılan") bilinen simetrik bir anahtarla imzalanmış ve bu anahtar, ileti alma/dinleme veya gönderme izni gibi belirli erişim hakları veren bir kuralla doğrudan ilişkili olan Servis Veri Tos'una bir belirteç sunar. SAS kuralları ad alanında veya doğrudan sıra veya konu gibi varlıklarda yapılandırılır ve ince taneli erişim denetimine olanak sağlar.

SAS belirteçleri doğrudan bir Servis Veri Aracı istemcisi tarafından oluşturulabilir veya istemcinin etkileşimde bulunduğu bazı ara belirteç veren bitiş noktası tarafından oluşturulabilir. Örneğin, bir sistem, istemcinin kimliğini ve sistem erişim haklarını kanıtlamak için Active Directory authorization protected web hizmeti bitiş noktasını aramasını gerektirebilir ve web hizmeti daha sonra uygun Hizmet Veri Yolu belirteci döndürür. Bu SAS belirteci, Azure SDK'da yer alan Servis Veri Mesuli sağlayıcısı kullanılarak kolayca oluşturulabilir. 

> [!IMPORTANT]
> Hizmet Veri Servisi ile Azure Active Directory Access Control (Access Control Service Service veya ACS olarak da bilinir) kullanıyorsanız, bu yönteme yönelik desteğin artık sınırlı olduğunu ve Uygulamanızı SAS'ı kullanmak üzere geçirmeniz gerektiğini unutmayın. Daha fazla bilgi için, [bu blog yazısı](https://blogs.msdn.microsoft.com/servicebus/2017/06/01/upcoming-changes-to-acs-enabled-namespaces/) ve bu [makaleye](service-bus-migrate-acs-sas.md)bakın.

## <a name="azure-active-directory"></a>Azure Active Directory
Hizmet Veri Mes'i kaynakları için Azure Etkin Dizin (Azure AD) tümleştirmesi, istemcinin kaynaklara erişimi üzerinde ince gelişmiş denetim için rol tabanlı erişim denetimi (RBAC) sağlar. Kullanıcı, grup veya uygulama hizmeti ilkesi olabilecek güvenlik ilkesine izin vermek için rol tabanlı erişim denetimini (RBAC) kullanabilirsiniz. Güvenlik ilkesi, OAuth 2.0 belirteci döndürmek için Azure AD tarafından doğrulanır. Belirteç, bir Hizmet Veri Servisi kaynağına (sıra, konu vb.) erişmek için bir isteği yetkilendirmek için kullanılabilir.

Azure AD ile kimlik doğrulama hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Yönetilen kimliklerle kimlik doğrulaması](service-bus-managed-service-identity.md)
- [Uygulamadan kimlik doğrulaması yapma](authenticate-application.md)

> [!IMPORTANT]
> Azure AD tarafından döndürülen OAuth 2.0 belirteci kullanarak kullanıcıları veya uygulamaları yetkilendirmek, paylaşılan erişim imzaları (SAS) üzerinden üstün güvenlik ve kullanım kolaylığı sağlar. Azure AD ile belirteçleri kodunuzda depolamaya ve olası güvenlik açıklarını riske atmaya gerek yoktur. Azure Hizmet Veri Servisi uygulamalarınızla mümkün olduğunda Azure AD'yi kullanmanızı öneririz. 


## <a name="shared-access-signature"></a>Paylaşılan erişim imzası
[SAS kimlik doğrulaması,](service-bus-sas.md) belirli haklara sahip bir kullanıcıya Hizmet Veri Yardımı kaynaklarına erişim izni vermenizi sağlar. Hizmet Veri Yolunda'ndaki SAS kimlik doğrulaması, Hizmet Veri Servisi kaynağındaki ilişkili haklara sahip bir şifreleme anahtarıyapılandırmasını içerir. İstemciler daha sonra, URI'ye erişilen kaynaktan ve yapılandırılan anahtarla imzalanan bir son kullanma süresinden oluşan bir SAS belirteci sunarak bu kaynağa erişebilirler.

SAS anahtarlarını Hizmet Veri Yolunda ad alanında yapılandırabilirsiniz. Anahtar, bu ad alanındaki tüm ileti varlıkları için geçerlidir. Ayrıca, Servis Veri Servisi kuyruklarında ve konularıüzerinde anahtarları yapılandırabilirsiniz. SAS, [Azure Röle'de](../service-bus-relay/relay-authentication-and-authorization.md)de desteklenir.

SAS'ı kullanmak için, bir Ad alanı, sıra veya konu üzerinde bir [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) nesnesini yapılandırabilirsiniz. Bu kural aşağıdaki öğelerden oluşur:

* *Anahtar Adı*: kuralı tanımlar.
* *PrimaryKey*: SAS belirteçlerini imzalamak/doğrulamak için kullanılan şifreleme anahtarı.
* *SecondaryKey*: SAS belirteçlerini imzalamak/doğrulamak için kullanılan şifreleme anahtarı.
* *Haklar*: Verilen **Dinleme,** **Gönder**veya **Yönet** haklarının toplanmasını temsil eder.

Ad alanı düzeyinde yapılandırılan yetkilendirme kuralları, ilgili anahtar kullanılarak imzalanmış belirteçleri olan istemciler için ad alanındaki tüm varlıklara erişim sağlayabilir. Bir Hizmet Veri Aracı ad alanında, sırada veya konu üzerinde en fazla 12 bu tür yetkilendirme kuralları yapılandırabilirsiniz. Varsayılan olarak, tüm hakları olan bir [SharedAccessAuthorizationRule,](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) ilk olarak sağlandığında her ad alanı için yapılandırılır.

Bir varlığa erişmek için istemci, belirli bir [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule)kullanılarak oluşturulan bir SAS belirteci gerektirir. SAS belirteci, erişimin talep edildiği kaynak URI'den oluşan bir kaynak dizesinin HMAC-SHA256'sı ve yetkilendirme kuralıyla ilişkili bir şifreleme anahtarıyla son kullanma tarihi kullanılarak oluşturulur.

Hizmet Veri Mes'i için SAS kimlik doğrulama desteği Azure .NET SDK sürümleri 2.0 ve sonraki sürümlerde yer almaktadır. SAS, [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule)desteği içerir. Bir bağlantı dizesi parametre olarak kabul eden tüm API'ler SAS bağlantı dizeleri için destek içerir.

## <a name="next-steps"></a>Sonraki adımlar

- SAS hakkında daha fazla bilgi için [Paylaşılan Erişim İmzaları ile Hizmet Veri Aracı kimlik doğrulamasını](service-bus-sas.md) okumaya devam edin.
- Azure [Active Directory Access Control'den (ACS) Paylaşılan Erişim İmzasıfatı yetkilendirmesine](service-bus-migrate-acs-sas.md)nasıl geçirilir?
- [ACS Etkin ad alanlarında yapılan değişiklikler.](https://blogs.msdn.microsoft.com/servicebus/2017/06/01/upcoming-changes-to-acs-enabled-namespaces/)
- Azure Röle kimlik doğrulaması ve yetkilendirmesi hakkında karşılık gelen bilgiler için Azure [Röle kimlik doğrulaması ve yetkilendirmesi](../service-bus-relay/relay-authentication-and-authorization.md)için bkz. 

