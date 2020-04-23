---
title: Azure Active Directory'ye erişimi yetkilendirme
description: Bu makalede, Azure Etkin Dizini'ni kullanarak Olay Hub kaynaklarına erişim yetkisi hakkında bilgi verilmektedir.
services: event-hubs
ms.service: event-hubs
documentationcenter: ''
author: spelluru
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: spelluru
ms.openlocfilehash: 6216b56c8e8f0de4f9cd60306bbf9c5ed49a11ad
ms.sourcegitcommit: 75089113827229663afed75b8364ab5212d67323
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "82025212"
---
# <a name="authorize-access-to-event-hubs-resources-using-azure-active-directory"></a>Azure Active Directory'yi kullanarak Etkinlik Hub kaynaklarına erişimi yetkilendirme
Azure Etkinlik Hub'ları, İstekleri Etkinlik Hub'ları kaynaklarına yetkilendirmek için Azure Active Directory (Azure AD) kullanmayı destekler. Azure AD ile, bir kullanıcı veya uygulama hizmeti ilkesi olabilecek bir güvenlik ilkesine izin vermek için rol tabanlı erişim denetimini (RBAC) kullanabilirsiniz. Roller ve rol atamaları hakkında daha fazla bilgi edinmek için farklı [rolleri anlama 'ya](../role-based-access-control/overview.md)bakın.

## <a name="overview"></a>Genel bakış
Bir güvenlik sorumlusu (kullanıcı veya uygulama) olay hub'ları kaynağına erişmeye çalıştığında, isteğin yetkilendirilmesi gerekir. Azure AD ile kaynağa erişim iki adımlı bir işlemdir. 

 1. İlk olarak, güvenlik sorumlusunun kimliği doğrulanır ve bir OAuth 2.0 belirteci döndürülür. Belirteç istemek için kaynak `https://eventhubs.azure.net/`adı. Kafka istemcileri için, bir belirteç istemek için kaynak . `https://<namespace>.servicebus.windows.net`
 1. Ardından, belirteç, belirtilen kaynağa erişim yetkisi vermek üzere Olay Hub'ları hizmetine yapılan isteğin bir parçası olarak geçirilir.

Kimlik doğrulama adımı, bir uygulama isteğinin çalışma zamanında bir OAuth 2.0 erişim belirteci içermesini gerektirir. Bir uygulama Azure VM, sanal makine ölçeği kümesi veya Azure İşlevi uygulaması gibi bir Azure varlığı içinde çalışıyorsa, kaynaklara erişmek için yönetilen bir kimliği kullanabilir. Etkinlik Hub'ları hizmetinde yönetilen bir kimlik tarafından yapılan isteklerin nasıl doğrudoğruyapılacağını öğrenmek için Azure [Etkin Dizini ve Azure Kaynakları için yönetilen kimliklerle Azure Etkinlik Hub kaynaklarına kimlik doğrulama](authenticate-managed-identity.md)erişimi konusuna bakın. 

Yetkilendirme adımı, bir veya daha fazla RBAC rolünün güvenlik ilkesine atanmasını gerektirir. Azure Olay Hub'ları, Olay Hub'ları kaynakları için izin kümelerini kapsayan RBAC rolleri sağlar. Bir güvenlik ilkesine atanan roller, asıl ilkelerin sahip olacağı izinleri belirler. RBAC rolleri hakkında daha fazla bilgi için [Azure Etkinlik Hub'ları için Yerleşik RBAC rolleri'ne](#built-in-rbac-roles-for-azure-event-hubs)bakın. 

Etkinlik Hub'larına istekte bulunan yerel uygulamalar ve web uygulamaları da Azure AD ile yetki lendirme yapabilir. Bir erişim jetonunu nasıl isteyeceğinizi ve Olay Hub'ları kaynaklarına yönelik istekleri yetkilendirmek için nasıl kullanacağınızı öğrenmek için, bir uygulamadan Azure AD ile [Azure Etkinlik Hub'larına Kimlik Doğrulama erişimi](authenticate-application.md)ne bakın. 

## <a name="assign-rbac-roles-for-access-rights"></a>Erişim hakları için RBAC rollerini atama
Azure Etkin Dizin (Azure AD), rol tabanlı [erişim denetimi (RBAC)](../role-based-access-control/overview.md)aracılığıyla güvenli kaynaklara erişim haklarına izin vermektedir. Azure Olay Hub'ları, olay hub verilerine erişmek için kullanılan ortak izin kümelerini kapsayan yerleşik RBAC rolleri kümesini tanımlar ve verilere erişmek için özel roller de tanımlayabilirsiniz.

Bir Azure REKLAM güvenlik ilkesine bir RBAC rolü atandığında, Azure bu güvenlik ilkesi için bu kaynaklara erişim sağlar. Erişim, abonelik düzeyine, kaynak grubuna, Olay Hub'larına veya altındaki herhangi bir kaynağa kadar kapsama edilebilir. Azure AD güvenlik ilkesi bir kullanıcı veya uygulama hizmeti ilkesi veya [Azure kaynakları için yönetilen](../active-directory/managed-identities-azure-resources/overview.md)bir kimlik olabilir.

## <a name="built-in-rbac-roles-for-azure-event-hubs"></a>Azure Etkinlik Hub'ları için yerleşik RBAC rolleri
Azure, Azure AD ve OAuth kullanarak Etkinlik Hub'ları verilerine erişim yetkisi vermek için aşağıdaki yerleşik RBAC rollerini sağlar:

- [Azure Olay Hub'ları Veri sahibi](../role-based-access-control/built-in-roles.md#azure-event-hubs-data-owner): Olay Hub'ları kaynaklarına tam erişim sağlamak için bu rolü kullanın.
- [Azure Olay Hub'ları Veri gönderen :](../role-based-access-control/built-in-roles.md#azure-event-hubs-data-receiver)Olay Hub'ları kaynaklarına göndere erişim sağlamak için bu rolü kullanın.
- [Azure Etkinlik Hub'ları Veri alıcısı](../role-based-access-control/built-in-roles.md#azure-event-hubs-data-sender): Olay Hub'ları kaynaklarını tüketen/alan kaynaklara erişim sağlamak için bu rolü kullanın.

## <a name="resource-scope"></a>Kaynak kapsamı 
Bir güvenlik ilkesine Bir RBAC rolü atamadan önce, güvenlik ilkesinin sahip olması gereken erişim kapsamını belirleyin. En iyi uygulamalar, yalnızca mümkün olan en dar kapsamı vermenin her zaman en iyisi olduğunu belirtir.

Aşağıdaki liste, en dar kapsamdan başlayarak Olay Hub'ları kaynaklarına erişimi kapsama alabileceğiniz düzeyleri açıklar:

- **Tüketici grubu**: Bu kapsamda, rol ataması yalnızca bu varlık için geçerlidir. Şu anda Azure portalı, bu düzeydeki bir güvenlik ilkesine RBAC rolü atamasını desteklemez. 
- **Olay merkezi**: Rol ataması Olay Hub'ı varlığı ve altındaki tüketici grubu için geçerlidir.
- **Namespace**: Rol ataması, Olay Hub'larının tüm topolojisini ad alanı altında ve onunla ilişkili tüketici grubuna kapsar.
- **Kaynak grubu**: Rol ataması kaynak grubu altındaki tüm Olay Hub'ları kaynakları için geçerlidir.
- **Abonelik**: Rol ataması, abonelikteki tüm kaynak gruplarındaki tüm Olay Hub'ları kaynakları için geçerlidir.

> [!NOTE]
> - RBAC rol atamalarının yayılması beş dakika kadar sürebilir. 
> - Bu içerik, Apache Kafka için hem Olay Hub'ları hem de Etkinlik Hub'ları için geçerlidir. Kafka desteği için Etkinlik Hub'ları hakkında daha fazla bilgi için [Kafka için Etkinlik Hub'larına](event-hubs-for-kafka-ecosystem-overview.md#security-and-authentication)bakın - güvenlik ve kimlik doğrulama.


Yerleşik rollerin nasıl tanımlandığı hakkında daha fazla bilgi için [bkz.](../role-based-access-control/role-definitions.md#management-and-data-operations) Özel RBAC rolleri oluşturma hakkında bilgi [için](../role-based-access-control/custom-roles.md)bkz.



## <a name="samples"></a>Örnekler
- [Microsoft.Azure.EventHubs örnekleri](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/Rbac). 
    
    Bu örnekler eski **Microsoft.Azure.EventHubs** kitaplığını kullanır, ancak en son **Azure.Messaging.EventHubs** kitaplığını kullanarak kolayca güncelleştirebilirsiniz. Örneği eski kitaplığı kullanmaktan yenisine taşımak [için Microsoft.Azure.EventHub'larından Azure.Messaging.EventHubs'a geçiş kılavuzuna](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/eventhub/Azure.Messaging.EventHubs/MigrationGuide.md)bakın.
- [Azure.Messaging.EventHubs örnekleri](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Azure.Messaging.EventHubs/ManagedIdentityWebApp)

    Bu örnek en son **Azure.Messaging.EventHubs** kitaplığını kullanacak şekilde güncelleştirildi.
- [Kafka için Olay Hubs - OAuth örnekleri](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/oauth). 


## <a name="next-steps"></a>Sonraki adımlar
- Bir güvenlik ilkesine yerleşik RBAC rolü nasıl atayabilirsiniz öğrenin, [Azure Etkin Dizini kullanarak Olay Hub kaynaklarına kimlik doğrulama erişimi](authenticate-application.md)hakkında bilgi edinin.
- [RBAC ile özel roller oluşturmayı](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/Rbac/CustomRole)öğrenin.
- [EH ile Azure Etkin Dizini nasıl kullanacağınızı](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/Rbac/AzureEventHubsSDK) öğrenin

Aşağıdaki ilgili makalelere bakın:

- [Azure Etkin Dizin'i kullanarak bir uygulamadan Azure Etkinlik Hub'larına yönelik istekleri kimlik doğrulaması](authenticate-application.md)
- [Etkinlik Hub'ları Kaynaklarına erişmek için Azure Etkin Dizini ile yönetilen bir kimliğin kimliğini doğrulatın](authenticate-managed-identity.md)
- [Paylaşılan Erişim İmzalarını kullanarak Azure Etkinlik Hub'larına istekleri nidoğrulatın](authenticate-shared-access-signature.md)
- [Paylaşılan Erişim İmzalarını kullanarak Etkinlik Hubkaynaklarına erişimi yetkilendirme](authorize-access-shared-access-signature.md)
