---
title: Azure AD Connect bulut sağlama destekli topolojiler ve senaryolar
description: Bu konu, ön koşulları ve donanım gereksinimlerini bulut sağlama açıklar.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/26/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 386af46bbee623d37bc914d2ee9130c914c6c885
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77620864"
---
# <a name="azure-ad-connect-cloud-provisioning-supported-topologies-and-scenarios"></a>Azure AD Connect bulut sağlama destekli topolojiler ve senaryolar
Bu makalede, Azure AD Connect bulut sağlama yı kullanan çeşitli şirket içi ve Azure Etkin Dizin (Azure AD) topolojileri açıklanmaktadır. Bu makalede yalnızca desteklenen yapılandırmalar ve senaryolar içerir.

> [!IMPORTANT]
> Microsoft, Azure AD Connect bulut sağlamanın resmi olarak belgelenmiş yapılandırmalar veya eylemler dışında değiştirilmesini veya çalıştırını desteklemez. Bu yapılandırmalardan veya eylemlerden herhangi biri, tutarsız veya desteklenmeyen bir Azure AD Connect bulut sağlama durumuna neden olabilir. Sonuç olarak Microsoft, bu tür dağıtımlar için teknik destek sağlayamaz.

## <a name="things-to-remember-about-all-scenarios-and-topologies"></a>Tüm senaryolar ve topolojiler hakkında hatırlanması gerekenler
Aşağıda, bir çözüm seçerken göz önünde bulundurulması gereken bir bilgi listesi veremeyiz.

- Kullanıcılar ve gruplar tüm ormanlarda benzersiz olarak tanımlanmalıdır
- Ormanlar arasında eşleştirme bulut sağlama ile oluşmaz
- Bir kullanıcı veya grup tüm ormanlarda yalnızca bir kez temsil edilmelidir
- Nesneler için kaynak çapa otomatik olarak seçilir.  Varsa ms-DS-ConsistencyGuid kullanır, aksi takdirde ObjectGUID kullanılır.
- Kaynak çapa için kullanılan özniteliği değiştiremezsiniz.

## <a name="single-forest-single-azure-ad-tenant"></a>Tek orman, tek Azure AD kiracı
![Tek bir orman ve tek bir kiracı için topoloji](media/plan-cloud-provisioning-topologies/single-forest.png)

En basit topoloji, bir veya birden çok etki alanı olan tek bir şirket içi orman ve tek bir Azure AD kiracısıdır.  Bu senaryonun bir örneği için [Bkz. Öğretici: Tek bir Azure AD kiracısı olan tek bir orman](tutorial-single-forest.md)


## <a name="multi-forest-single-azure-ad-tenant"></a>Çok ormanlı, tek Azure AD kiracı
![Çok ormanlı ve tek bir kiracı için topoloji](media/plan-cloud-provisioning-topologies/multi-forest.png)

Yaygın bir topoloji, bir veya birden çok etki alanı olan birden çok AD ormanı ve tek bir Azure AD kiracısıdır.  

## <a name="existing-forest-with-azure-ad-connect-new-forest-with-cloud-provisioning"></a>Azure AD Connect ile mevcut orman, bulut Sağlama ile yeni orman
![Tek bir orman ve tek bir kiracı için topoloji](media/plan-cloud-provisioning-topologies/existing-forest-new-forest.png)

Bu senaryo topoloji çoklu orman senaryosuna benzer, ancak bu durum varolan bir Azure AD Connect ortamını içerir ve ardından Azure AD Connect bulut sağlama yı kullanarak yeni bir orman getirir.  Bu senaryonun bir örneği için [bkz.](tutorial-existing-forest.md)

## <a name="piloting-azure-ad-connect-cloud-provisioning-in-an-existing-hybrid-ad-forest"></a>Azure AD'yi pilot uygulama Varolan karma bir AD ormanında bulut sağlama
![Tek bir orman ve tek](media/plan-cloud-provisioning-topologies/migrate.png) bir kiracı için topoloji Pilot senaryo, aynı ormanda hem Azure AD Connect hem de Azure AD Connect bulut sağlamanın varlığını ve kullanıcıları ve grupları buna göre kapsama sunmasını içerir. NOT: Bir nesne yalnızca araçlardan birinde kapsam içinde olmalıdır. 

Bu senaryonun bir örneği için [bkz.](tutorial-pilot-aadc-aadccp.md)



## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)

