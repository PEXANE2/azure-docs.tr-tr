---
title: Olay işleyicilerine olay teslimini doğrulama (Azure Event Grid)
description: Bu makalede, Azure Event Grid ' de olay işleyicilerine teslimin doğrulanması için farklı yollar açıklanmaktadır.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: abe16c9598c8c10caa832150aafac997dd7f1624
ms.sourcegitcommit: f988fc0f13266cea6e86ce618f2b511ce69bbb96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87460652"
---
# <a name="authenticate-event-delivery-to-event-handlers-azure-event-grid"></a>Olay işleyicilerine olay teslimini doğrulama (Azure Event Grid)
Bu makalede olay işleyicilerine olay teslimini doğrulama hakkında bilgi sağlanır. Ayrıca, Azure Active Directory (Azure AD) veya paylaşılan bir gizli dizi kullanarak Event Grid olayları almak için kullanılan Web kancası uç noktalarının güvenliğini nasıl güvence altına alacağını gösterir.

## <a name="use-system-assigned-identities-for-event-delivery"></a>Olay teslimi için sistem tarafından atanan kimlikleri kullanma
Bir konu veya etki alanı için sistem tarafından atanan yönetilen kimliği etkinleştirebilir ve olayları Service Bus kuyrukları ve konuları, Olay Hub 'ları ve depolama hesapları gibi desteklenen hedeflere iletmek için kimliğini kullanabilirsiniz.

Adımlar şunlardır: 

1. Bir sistem tarafından atanan kimliğe sahip bir konu veya etki alanı oluşturun ya da kimliği etkinleştirmek için mevcut bir konuyu veya etki alanını güncelleştirin. 
1. Kimliği, hedef üzerinde (örneğin, bir Service Bus kuyruğu) uygun bir role (örneğin, Service Bus veri gönderici) ekleyin.
1. Olay abonelikleri oluşturduğunuzda, hedefe olayları iletmek için kimliğin kullanımını etkinleştirin. 

Ayrıntılı adım adım yönergeler için bkz. [yönetilen kimlik Ile olay teslimi](managed-service-identity.md).


## <a name="authenticate-event-delivery-to-webhook-endpoints"></a>Web kancası uç noktalarına olay teslimi kimlik doğrulaması
Aşağıdaki bölümlerde, Web kancası uç noktalarına olay tesliminin nasıl doğrulanabilmesi anlatılmaktadır. Kullandığınız yöntemden bağımsız olarak bir doğrulama el sıkışma mekanizması kullanmanız gerekir. Ayrıntılar için bkz. [Web kancası olay teslimi](webhook-event-delivery.md) . 


### <a name="using-azure-active-directory-azure-ad"></a>Azure Active Directory kullanma (Azure AD)
Azure AD 'yi kullanarak Event Grid olayları almak için kullanılan Web kancası uç noktasının güvenliğini sağlayabilirsiniz. Bir Azure AD uygulaması oluşturmanız, uygulamanızda yetkilendirme Event Grid bir rol ve hizmet sorumlusu oluşturmanız ve olay aboneliğini Azure AD uygulamasını kullanacak şekilde yapılandırmanız gerekir. [Event Grid Azure Active Directory yapılandırmayı](secure-webhook-delivery.md)öğrenin.

### <a name="using-client-secret-as-a-query-parameter"></a>Sorgu parametresi olarak istemci gizli anahtarını kullanma
Ayrıca, olay aboneliği oluşturmanın bir parçası olarak belirtilen Web kancası hedef URL 'sine sorgu parametreleri ekleyerek Web kancası uç noktanızı da güvenli hale getirebilirsiniz. Sorgu parametrelerinden birini, [erişim belirteci](https://en.wikipedia.org/wiki/Access_token) veya paylaşılan gizlilik gibi bir istemci gizli anahtarı olacak şekilde ayarlayın. Event Grid hizmet, Web kancasına her olay teslim isteğindeki tüm sorgu parametrelerini içerir. Web kancası hizmeti gizli dizi alabilir ve doğrulayabilir. İstemci parolası güncelleştirilirse olay aboneliğinin da güncelleştirilmesi gerekir. Bu gizli anahtar sırasında teslimat hatalarından kaçınmak için, Web kancasının, olay aboneliğini yeni gizli anahtar ile güncelleştirmeden önce sınırlı bir süre için hem eski hem de yeni gizli dizileri kabul etmesini sağlayın. 

Sorgu parametreleri, istemci gizli dizileri içerebildiği için, bunlar daha fazla dikkatli işlenir. Bunlar şifreli olarak depolanır ve hizmet işleçlerine erişemez. Hizmet günlüklerinin/izlemelerinin bir parçası olarak günlüğe kaydedilmez. Olay aboneliği özellikleri alınırken, hedef sorgu parametreleri varsayılan olarak döndürülmez. Örneğin: [--Include-Full-Endpoint-URL](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-show) parametresi Azure [CLI](/cli/azure?view=azure-cli-latest)'da kullanılır.

Web kancalarına olay sunma hakkında daha fazla bilgi için bkz. [Web kancası olay teslimi](webhook-event-delivery.md)

> [!IMPORTANT]
Azure Event Grid yalnızca **https** Web kancası uç noktalarını destekler. 


## <a name="next-steps"></a>Sonraki adımlar
Konular veya etki alanlarına istemci yayımlama olaylarını doğrulamak hakkında bilgi edinmek için bkz. [Yayımlama Istemcilerinin kimlik doğrulaması](security-authenticate-publishing-clients.md) . 
