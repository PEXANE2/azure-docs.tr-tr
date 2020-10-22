---
title: Azure Active Directory ile SCıM eşitlemesi
description: Bu eşitleme modelini elde etmek için mimari yönergeler
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
ms.openlocfilehash: d1dda465dd675e0f5f519f86289df2621be0b9bb
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92367865"
---
# <a name="scim-synchronization-with-azure-active-directory"></a>Azure Active Directory ile SCıM eşitlemesi

Etki alanları arası kimlik yönetimi (SCıM) sistemi, kimlik alanları ve BT sistemleri arasında Kullanıcı kimlik bilgilerinin değişimini otomatik hale getirmeye yönelik açık bir standart protokoldür. SCıM, Insan büyük yönetim (HCM) sistemine eklenen çalışanların Azure Active Directory (Azure AD) veya Windows Server Active Directory oluşturulan hesaplara otomatik olarak sahip olmasını sağlar. Kullanıcı öznitelikleri ve profilleri, iki sistem arasında eşitlenir ve Kullanıcı durumu veya rol değişikliğine bağlı olarak kullanıcıları kaldırma güncellenecektir.

SCıM iki uç noktanın standartlaştırılmış bir tanımıdır:/Users ' uç noktası ve/groups uç noktası. Nesneleri oluşturmak, güncelleştirmek ve silmek için ortak REST fiillerini kullanır. Ayrıca grup adı, Kullanıcı adı, ad, soyadı ve e-posta gibi ortak öznitelikler için önceden tanımlanmış bir şema kullanır. Bir SCıM 2,0 REST API sunan uygulamalar, Özel Kullanıcı yönetimi API 'Leri veya ürünleriyle çalışma durumunu azaltabilir veya ortadan kaldırabilir. Örneğin, herhangi bir SCıM uyumlu istemci, yeni bir kullanıcı girişi oluşturmak için/Users uç noktasına bir JSON nesnesinin HTTP GÖNDERISINI yapabilir. Aynı temel eylemler için biraz farklı bir API sağlamak yerine, SCıM standardına uyan uygulamalar, önceden var olan istemcilerden, araçlardan ve koddan hemen faydalanabilir. 

## <a name="use-when"></a>Şu durumlarda kullanın: 

Kullanıcı bilgilerini bir HCM sisteminden Azure AD 'ye ve Windows Server Active Directory ' e otomatik olarak sağlamak ve gerekirse hedef sistemleri hedeflemek istiyorsunuz. 

![mimari diyagram](./media/authentication-patterns/scim-auth.png)


## <a name="components-of-system"></a>Sistem bileşenleri 

* **HCM sistemi**: çalışan yaşam döngüsü boyunca HR işlemlerini destekleyen ve otomatikleştiren ınsan sermaye yönetimi sürecini ve uygulamalarını etkinleştiren uygulamalar ve teknolojiler. 

* **Azure AD sağlama hizmeti**: otomatik sağlama için scım 2,0 protokolünü kullanır. Hizmet, uygulama için SCıM uç noktasına bağlanır ve kullanıcıların ve grupların sağlanması ve sağlanması GERI almak için SCıM Kullanıcı nesne şemasını ve REST API 'Lerini kullanır.  

* **Azure AD**: kimlik yaşam döngüsünü ve bunların yetkilendirmelerini yönetmek için kullanılan kullanıcı deposu. 

* **Hedef sistem**: Kullanıcı ve grupların otomatik olarak sağlanmasını sağlamak için SCIM uç noktası olan ve Azure AD sağlama ile birlikte çalışarak uygulama veya sistem.  

## <a name="implement-scim-with-azure-ad"></a>Azure AD ile SCıM 'i uygulama 

* [Azure AD 'de sağlama nasıl yapılır? ](https://docs.microsoft.com/azure/active-directory/app-provisioning/how-provisioning-works)

* [Azure portal kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme ](https://docs.microsoft.com/azure/active-directory/app-provisioning/configure-automatic-user-provisioning-portal)

* [Azure AD ile bir SCıM uç noktası oluşturun ve Kullanıcı sağlamasını yapılandırın  ](https://docs.microsoft.com/azure/active-directory/app-provisioning/use-scim-to-provision-users-and-groups)

* [SCıM 2,0 Azure AD sağlama hizmeti 'nin protokol uyumluluğu](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-config-problem-scim-compatibility)

