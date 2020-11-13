---
title: Azure Active Directory ile SCıM eşitlemesi
description: Azure Active Directory ile SCıM eşitlemesini elde etmeye yönelik mimari rehberlik.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 10/10/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: f957070ec94fc4c61089f31fe91261a2f52c4ee4
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2020
ms.locfileid: "94578867"
---
# <a name="scim-synchronization-with-azure-active-directory"></a>Azure Active Directory ile SCıM eşitlemesi

Etki alanları arası kimlik yönetimi (SCıM) sistemi, kimlik alanları ve BT sistemleri arasında Kullanıcı kimlik bilgilerinin değişimini otomatik hale getirmeye yönelik açık bir standart protokoldür. SCıM, Insan büyük yönetim (HCM) sistemine eklenen çalışanların Azure Active Directory (Azure AD) veya Windows Server Active Directory oluşturulan hesaplara otomatik olarak sahip olmasını sağlar. Kullanıcı öznitelikleri ve profilleri, iki sistem arasında eşitlenir ve Kullanıcı durumu veya rol değişikliğine bağlı olarak kullanıcıları kaldırma güncellenecektir.

SCıM iki uç noktanın standartlaştırılmış bir tanımıdır:/Users ' uç noktası ve/groups uç noktası. Nesneleri oluşturmak, güncelleştirmek ve silmek için ortak REST fiillerini kullanır. Ayrıca grup adı, Kullanıcı adı, ad, soyadı ve e-posta gibi ortak öznitelikler için önceden tanımlanmış bir şema kullanır. Bir SCıM 2,0 REST API sunan uygulamalar, Özel Kullanıcı yönetimi API 'Leri veya ürünleriyle çalışma durumunu azaltabilir veya ortadan kaldırabilir. Örneğin, herhangi bir SCıM uyumlu istemci, yeni bir kullanıcı girişi oluşturmak için/Users uç noktasına bir JSON nesnesinin HTTP GÖNDERISINI yapabilir. Aynı temel eylemler için biraz farklı bir API sağlamak yerine, SCıM standardına uyan uygulamalar, önceden var olan istemcilerden, araçlardan ve koddan hemen faydalanabilir. 

## <a name="use-when"></a>Şu durumlarda kullanın: 

Kullanıcı bilgilerini bir HCM sisteminden Azure AD 'ye ve Windows Server Active Directory ' e otomatik olarak sağlamak ve gerekirse hedef sistemleri hedeflemek istiyorsunuz. 

![mimari diyagram](./media/authentication-patterns/scim-auth.png)


## <a name="components-of-system"></a>Sistem bileşenleri 

* **HCM sistemi** : çalışan yaşam döngüsü boyunca HR işlemlerini destekleyen ve otomatikleştiren ınsan sermaye yönetimi sürecini ve uygulamalarını etkinleştiren uygulamalar ve teknolojiler. 

* **Azure AD sağlama hizmeti** : otomatik sağlama için scım 2,0 protokolünü kullanır. Hizmet, uygulama için SCıM uç noktasına bağlanır ve kullanıcıların ve grupların sağlanması ve sağlanması GERI almak için SCıM Kullanıcı nesne şemasını ve REST API 'Lerini kullanır.  

* **Azure AD** : kimlik yaşam döngüsünü ve bunların yetkilendirmelerini yönetmek için kullanılan kullanıcı deposu. 

* **Hedef sistem** : Kullanıcı ve grupların otomatik olarak sağlanmasını sağlamak için SCIM uç noktası olan ve Azure AD sağlama ile birlikte çalışarak uygulama veya sistem.  

## <a name="implement-scim-with-azure-ad"></a>Azure AD ile SCıM 'i uygulama 

* [Azure AD 'de sağlama nasıl yapılır? ](https://docs.microsoft.com/azure/active-directory/app-provisioning/how-provisioning-works)

* [Azure portal kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme ](https://docs.microsoft.com/azure/active-directory/app-provisioning/configure-automatic-user-provisioning-portal)

* [Azure AD ile bir SCıM uç noktası oluşturun ve Kullanıcı sağlamasını yapılandırın  ](https://docs.microsoft.com/azure/active-directory/app-provisioning/use-scim-to-provision-users-and-groups)

* [SCıM 2,0 Azure AD sağlama hizmeti 'nin protokol uyumluluğu](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-config-problem-scim-compatibility)

