---
title: Azure Event Hubs'a erişimi yetkilendirme
description: Bu makalede, Azure Olay Hub'ları kaynaklarına erişim yetkisi vermek için farklı seçenekler hakkında bilgi verilmektedir.
services: event-hubs
ms.service: event-hubs
documentationcenter: ''
author: spelluru
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: spelluru
ms.openlocfilehash: f44be4e1d3d1186f0122bd4669ae800ab42e31d6
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80521304"
---
# <a name="authorize-access-to-azure-event-hubs"></a>Azure Event Hubs'a erişimi yetkilendirme
Bir olay merkezinden olayları/verileri her yayımladığınızda veya tükettiğinizde, istemciniz Olay Hub'ları kaynaklarına erişmeye çalışır. Güvenli bir kaynağa yapılan her istek, hizmetin istemcinin verileri yayımlamak/tüketmek için gerekli izinlere sahip olmasını sağlayabilmesi için yetkilendirilmelidir. 

Azure Etkinlik Hub'ları, güvenli kaynaklara erişim yetkisi vermek için aşağıdaki seçenekleri sunar:

- Azure Active Directory
- Paylaşılan erişim imzası

> [!NOTE]
> Bu makale, hem Olay Hub'ları hem de [Apache Kafka](event-hubs-for-kafka-ecosystem-overview.md) senaryoları için geçerlidir. 

## <a name="azure-active-directory"></a>Azure Active Directory
Etkinlik Hub'ları kaynakları için Azure Etkin Dizin (Azure AD) tümleştirmesi, istemcinin kaynaklara erişimi üzerinde ince ayarlı denetim için rol tabanlı erişim denetimi (RBAC) sağlar. Kullanıcı, grup veya uygulama hizmeti ilkesi olabilecek güvenlik ilkesine izin vermek için rol tabanlı erişim denetimini (RBAC) kullanabilirsiniz. Güvenlik ilkesi, OAuth 2.0 belirteci döndürmek için Azure AD tarafından doğrulanır. Belirteç, olay hub'ları kaynağına erişmek için bir isteği yetkilendirmek için kullanılabilir.

Azure AD ile kimlik doğrulama hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure Etkin Dizinini kullanarak Azure Etkinlik Hub'larına istekleri nidoğrulatın](authenticate-application.md)
- [Azure Etkin Dizin'i kullanarak Etkinlik Hub kaynaklarına erişimi yetkilendirmeyin.](authorize-access-azure-active-directory.md)

## <a name="shared-access-signatures"></a>Paylaşılan erişim imzaları 
Olay Hub'ları kaynakları için paylaşılan erişim imzaları (SAS), Olay Hub'ları kaynaklarına sınırlı temsilci erişimi sağlar. İmzanın geçerli olduğu zaman aralığına veya verdiği izinlere kısıtlamalar eklemek kaynakları yönetmede esneklik sağlar. Daha fazla bilgi için [paylaşılan erişim imzalarını (SAS) kullanarak Kimlik Doğrulaması'ya](authenticate-shared-access-signature.md)bakın. 

Azure AD tarafından döndürülen Bir OAuth 2.0 belirteci kullanarak kullanıcıları veya uygulamaları yetkilendirmek, paylaşılan erişim imzaları (SAS) üzerinden üstün güvenlik ve kullanım kolaylığı sağlar. Azure AD ile, erişim belirteçlerini kodunuzla depolamanıza ve olası güvenlik açıklarını riske atmaya gerek yoktur. Azure AD, Etkinlik Hub'ları kaynaklarına ayrıntılı erişim sağlamak için paylaşılan erişim imzalarını (SAS) kullanmaya devam edebilirken, SAS belirteçlerini yönetmeye veya gizliliği ihlal edilmiş bir SAS'ı iptal etme endişesi duymadan benzer özellikler sunar. 

Varsayılan olarak, tüm Olay Hub'ları kaynakları güvenlidir ve yalnızca hesap sahibi tarafından kullanılabilir. İstemcilere Olay Hub kaynaklarına erişim izni vermek için yukarıda özetlenen yetkilendirme stratejilerinden herhangi birini kullanabilirsiniz. Microsoft, maksimum güvenlik ve kullanım kolaylığı için mümkün olduğunda Azure AD'yi kullanmanızı önerir.

SAS kullanarak yetkilendirme hakkında daha fazla bilgi için, [Paylaşılan Erişim İmzalarını kullanarak Olay Hub'larına erişim yetkisi](authorize-access-shared-access-signature.md)konusuna bakın.

## <a name="next-steps"></a>Sonraki adımlar
- GitHub depomuzda yayınlanan [RBAC örneklerini](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/Rbac) inceleyin. 
- Aşağıdaki makalelere bakın:
    - [Azure Etkin Dizin'i kullanarak bir uygulamadan Azure Etkinlik Hub'larına yönelik istekleri kimlik doğrulaması](authenticate-application.md)
    - [Etkinlik Hub'ları Kaynaklarına erişmek için Azure Etkin Dizini ile yönetilen bir kimliğin kimliğini doğrulatın](authenticate-managed-identity.md)
    - [Paylaşılan Erişim İmzalarını kullanarak Azure Etkinlik Hub'larına istekleri nidoğrulatın](authenticate-shared-access-signature.md)
    - [Azure Active Directory'yi kullanarak Etkinlik Hub kaynaklarına erişimi yetkilendirme](authorize-access-azure-active-directory.md)
    - [Paylaşılan Erişim İmzalarını kullanarak Etkinlik Hubkaynaklarına erişimi yetkilendirme](authorize-access-shared-access-signature.md)

