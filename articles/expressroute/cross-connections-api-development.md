---
title: Azure ExpressRoute CrossConnnections API geliştirme ve tümleştirme
description: Bu makalede, ExpressRoute ortakları için expressRouteCrossConnections kaynak türü hakkında ayrıntılı bir genel bakış sağlar.
services: expressroute
author: mialdrid
ms.service: expressroute
ms.topic: conceptual
ms.date: 02/06/2020
ms.author: mialdrid
ms.openlocfilehash: b4a83013d2cbeb2871a3963ec0c95144c02f4d66
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77187021"
---
# <a name="expressroute-crossconnnections-api-development-and-integration"></a>ExpressRoute CrossConnnections API geliştirme ve entegrasyon

ExpressRoute İş Ortağı Kaynak Yöneticisi API, ExpressRoute iş ortaklarının müşteri ExpressRoute devrelerinin katman-2 ve katman-3 yapılandırmasını yönetmesine olanak tanır. ExpressRoute İş Ortağı Kaynak Yöneticisi API yeni bir kaynak türü tanıttı, **expressRouteCrossConnections.** İş ortakları bu kaynağı müşteri ExpressRoute devrelerini yönetmek için kullanır.

## <a name="workflow"></a>İş akışı

ExpressRouteCrossConnections kaynağı, ExpressRoute devresinin gölge kaynağıdır. Bir Azure müşterisi bir ExpressRoute devresi oluşturduğunda ve belirli bir ExpressRoute iş ortağı seçtiğinde, Microsoft iş ortağının Azure ExpressRoute yönetim aboneliğinde bir ExpressRouteCrossConnections kaynağı oluşturur. Bunu yaparken, Microsoft expressRouteCrossConnections kaynağını oluşturmak için bir kaynak grubu tanımlar. Kaynak grubu için adlandırma standardı **CrossConnection-* PeeringLocation***; nerede PeeringLocation = ExpressRoute Konumu. Örneğin, bir müşteri Denver'da bir ExpressRoute devresi oluşturursa, CrossConnection iş ortağının Azure aboneliğinde aşağıdaki kaynak grubunda oluşturulur: **CrossConnnection-Denver.**

ExpressRoute iş ortakları, expressRouteCrossConnections kaynağına karşı REST işlemleri düzenleyerek katman-2 ve katman-3 yapılandırmasını yönetir.

## <a name="benefits"></a>Avantajlar

ExpressRouteCrossConnections kaynağına geçmenin faydaları:

* ExpressRoute iş ortakları için gelecekteki tüm geliştirmeler ExpressRouteCrossConnection kaynağında kullanıma sunulacaktır.

* İş ortakları, expressRouteCrossConnection kaynağına [Rol Tabanlı Erişim Denetimi](https://docs.microsoft.com/azure/role-based-access-control/overview) uygulayabilir. Bu denetimler, kullanıcı hesaplarının expressRouteCrossConnection kaynağını değiştirebileceği ve eşleme yapılandırmaları ekleme/güncelleştirme/silme izinlerini tanımlayabilir.

* ExpressRouteCrossConnection kaynağı, ExpressRoute bağlantılarını sorun gidermede yardımcı olabilecek API'leri ortaya çıkarır. Buna ARP tablosu, BGP Rota Tablosu Özeti ve BGP Rota Tablosu ayrıntıları dahildir. Bu özellik klasik dağıtım API'leri tarafından desteklenmez.

* İş ortakları, *RouteFilter* kaynağını kullanarak Microsoft'a bakan reklam topluluklarına da bakabilir.

## <a name="api-development-and-integration-steps"></a>API geliştirme ve entegrasyon adımları

İş Ortağı API'sine karşı gelişmek için ExpressRoute iş ortakları bir test müşterisi ve test ortağı kurulumundan yararlanır. Test müşteri kurulumu, sahte aygıtlar ve bağlantı noktalarıyla eşleyen test eşleme konumlarında ExpressRoute devreleri oluşturmak için kullanılır. Test ortağı kurulumu, test eşleme konumunda oluşturulan ExpressRoute devrelerini yönetmek için kullanılır.

### <a name="1-enlist-subscriptions"></a>1. Kayıt abonelikleri

Test iş ortağını istemek ve müşteri kurulumunu test etmek için ExpressRoute mühendislik iletişim kişinize iki Ödemeyle Ödeme Azure aboneliği kaydedin:
* **ExpressRoute_API_Dev_Provider_Sub:** Bu abonelik, sahte aygıtlarda ve bağlantı noktalarında test eşleme konumlarında oluşturulan ExpressRoute devrelerini yönetmek için kullanılır.

* **ExpressRoute_API_Dev_Customer_Sub:** Bu abonelik, sahte aygıtlara ve bağlantı noktalarına eşleyen test eşleme konumlarında ExpressRoute devreleri oluşturmak için kullanılır.

Test eşleme konumları: sahte aygıtlar ve bağlantı noktaları varsayılan olarak üretim müşterilerine açık değildir. Test kurulumuna eş gösteren ExpressRoute devreleri oluşturmak için bir abonelik özelliği bayrağının etkinleştirilmesi gerekir.

### <a name="2-register-the-dev_provider-subscription-to-access-the-expressroutecrossconnections-api"></a>2. ExpressRouteCrossConnections API'sine erişmek için Dev_Provider aboneliğini kaydedin

ExpressRouteCrossConnections API'sine erişmek için iş ortağı aboneliğinin **Microsoft.Network Kaynak Sağlayıcısı'na**kaydolması gerekir. Kayıt işlemini tamamlamak için [Azure kaynak sağlayıcıları ve türleri](/azure/azure-resource-manager/management/resource-providers-and-types#azure-portal) makalesindeki adımları izleyin.

### <a name="3-set-up-authentication-for-azure-resource-manager-rest-api-calls"></a>3. Azure Kaynak Yöneticisi REST API aramaları için kimlik doğrulaması ayarlama

Azure hizmetlerinin çoğu, hizmet API'lerini çağırmadan önce geçerli kimlik bilgilerini kullanarak müşteri kodunun Kaynak Yöneticisi ile kimliğinin doğrulanmasını gerektirir. Kimlik doğrulama, Azure AD tarafından çeşitli aktörler arasında koordine edilir ve istemciye kimlik doğrulama kanıtı olarak bir erişim belirteci sağlar.

Kimlik doğrulama işlemi iki ana adım içerir:

1. [İstemciyi kaydedin.](https://docs.microsoft.com/rest/api/azure/#register-your-client-application-with-azure-ad)
2. [Erişim isteğini oluşturun.](https://docs.microsoft.com/rest/api/azure/#create-the-request)

### <a name="4-provide-network-contributor-permission-to-the-client-application"></a>4. İstemci uygulamasına Ağ Katılımcısı izni sağlayın

Kimlik doğrulaması başarıyla yapılandırıldıktan sonra, Dev_Provider_Sub altında, Ağ Katılımcısı'na istemci uygulamanıza erişim izni vermeniz gerekir. İzin vermek için Azure [portalında](https://ms.portal.azure.com/#home) oturum açın ve aşağıdaki adımları tamamlayın:

1. Aboneliklere gidin ve Dev_Provider_Sub
2. Erişim Denetimine (IAM) gidin
3. Rol Atama ekle
4. Ağ Katılımcısı Rolünü Seçin
5. Azure AD Kullanıcısına, Grubuna veya Hizmet Sorumlusuna Erişim Atama
6. İstemci uygulamanızı seçin
7. Değişiklikleri kaydetme

### <a name="5-develop"></a>5. Geliştirin

[ExpressRouteCrossConnections API](https://docs.microsoft.com/rest/api/expressroute/expressroutecrossconnections)karşı geliştirin.

## <a name="rest-api"></a>REST API

REST API belgeleri için [ExpressRoute CrossConnections REST API'ye](https://docs.microsoft.com/rest/api/expressroute/expressroutecrossconnections) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Tüm ExpressRoute REST API'leri hakkında daha fazla bilgi için [ExpressRoute REST API'leri'ne](https://docs.microsoft.com/rest/api/expressroute/)bakın.