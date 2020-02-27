---
title: Azure AD Connect bulut sağlama desteklenen topolojiler ve senaryolar
description: Bu konuda, önkoşulların ve donanım gereksinimlerinin bulut sağlaması açıklanmaktadır.
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
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77620864"
---
# <a name="azure-ad-connect-cloud-provisioning-supported-topologies-and-scenarios"></a>Azure AD Connect bulut sağlama desteklenen topolojiler ve senaryolar
Bu makalede Azure AD Connect bulut sağlamasını kullanan çeşitli şirket içi ve Azure Active Directory (Azure AD) topolojileri açıklanmaktadır. Bu makalede yalnızca desteklenen yapılandırma ve senaryolar bulunur.

> [!IMPORTANT]
> Microsoft, resmi olarak belgelenen yapılandırmaların veya eylemlerin dışında bulut Sağlama Azure AD Connect değiştirmeyi veya işletim sistemlerini desteklemez. Bu yapılandırmaların veya eylemlerin herhangi biri, Azure AD Connect bulut sağlama tutarsız veya desteklenmeyen bir duruma neden olabilirler. Sonuç olarak Microsoft, bu tür dağıtımlar için teknik destek sağlayamaz.

## <a name="things-to-remember-about-all-scenarios-and-topologies"></a>Tüm senaryolar ve topolojiler hakkında hatırlamaları gerekenler
Aşağıda bir çözüm seçerken aklınızda bulundurmanız gereken bilgilerin bir listesi verilmiştir.

- Kullanıcılar ve gruplar tüm ormanlarda benzersiz şekilde tanımlanmalıdır
- Orman genelinde eşleştirme, bulut sağlama ile gerçekleşmez
- Bir kullanıcı veya grup tüm ormanlarda yalnızca bir kez temsil etmelidir
- Nesneler için kaynak bağlantısı otomatik olarak seçilir.  Varsa, ms-DS-ımbu GUID kullanır, aksi halde Objectguıd kullanılır.
- Kaynak Bağlayıcısı için kullanılan özniteliği değiştiremezsiniz.

## <a name="single-forest-single-azure-ad-tenant"></a>Tek orman, tek Azure AD kiracısı
![Tek bir orman ve tek bir kiracı için topoloji](media/plan-cloud-provisioning-topologies/single-forest.png)

En basit topoloji, bir veya birden çok etki alanı ve tek bir Azure AD kiracısı içeren tek bir şirket içi ormandır.  Bu senaryonun bir örneği için bkz [. Öğretici: tek bir Azure AD kiracısıyla tek bir orman](tutorial-single-forest.md)


## <a name="multi-forest-single-azure-ad-tenant"></a>Çoklu orman, tek Azure AD kiracısı
![Çok ormanlı ve tek bir kiracı için topoloji](media/plan-cloud-provisioning-topologies/multi-forest.png)

Ortak topoloji, bir veya birden çok etki alanı ve tek bir Azure AD kiracısı olan birden çok AD ormandır.  

## <a name="existing-forest-with-azure-ad-connect-new-forest-with-cloud-provisioning"></a>Azure AD Connect, bulut sağlaması ile yeni bir ormana sahip olan orman
![Tek bir orman ve tek bir kiracı için topoloji](media/plan-cloud-provisioning-topologies/existing-forest-new-forest.png)

Bu senaryo, çok ormanlı senaryoya benzerdir, ancak bu, mevcut bir Azure AD Connect ortamını içerir ve sonra Azure AD Connect bulut sağlaması kullanılarak yeni bir ormana gönderilir.  Bu senaryonun bir örneği için bkz [. Öğretici: tek bir Azure AD kiracısına sahip mevcut bir orman](tutorial-existing-forest.md)

## <a name="piloting-azure-ad-connect-cloud-provisioning-in-an-existing-hybrid-ad-forest"></a>Mevcut bir karma AD ormanında bulut Sağlama Azure AD Connect
tek bir orman ve tek bir kira](media/plan-cloud-provisioning-topologies/migrate.png) cıya yönelik ![topoloji, aynı ormanda hem Azure AD Connect hem de Azure AD Connect bulut sağlamasının varlığını ve Kullanıcı ve grupları uygun şekilde kapsamını içerir. NOTE: bir nesnenin yalnızca bir araç kapsamında olması gerekir. 

Bu senaryonun bir örneği için bkz [. Öğretici: Pilot Azure AD Connect var olan eşitlenmiş BIR ad ormanında bulut sağlama](tutorial-pilot-aadc-aadccp.md)



## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)

