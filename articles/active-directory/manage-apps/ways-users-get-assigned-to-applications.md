---
title: Kullanıcılar uygulamalara nasıl atanırken | Microsoft Dokümanlar
description: Kullanıcıların kiracınızdaki bir uygulamaya nasıl atandığını anlama
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: b818fe1d8b6bbc9d2d8c5b460b4d71dccdd39366
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65825983"
---
# <a name="how-to-assign-users-to-applications"></a>Kullanıcılar uygulamalara nasıl atandırılabilen

Bu makale, kullanıcıların kiracınızdaki bir uygulamaya nasıl atandığını anlamanıza yardımcı olur.

## <a name="how-do-users-get-assigned-to-an-application-in-azure-ad"></a>Kullanıcılar Azure AD'deki bir uygulamaya nasıl atanabilir?

Bir kullanıcının bir uygulamaya erişmesi için önce bir şekilde bu uygulamaya atanması gerekir. Atama bir yönetici, bir iş temsilcisi veya bazen kullanıcının kendileri tarafından gerçekleştirilebilir. Aşağıda, kullanıcıların uygulamalara nasıl atanabilecekleri açıklanmaktadır:

1.  Yönetici, [bir kullanıcıyı](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) doğrudan uygulamaya atar

2.  Yönetici, kullanıcının uygulamaya üyesi olduğu [bir grup atar:](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

    * Şirket içinde senkronize edilmiş bir grup

    * Bulutta oluşturulan statik bir güvenlik grubu

    * Bulutta oluşturulan [dinamik bir güvenlik grubu](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal)

    * Bulutta oluşturulan bir Office 365 grubu

    * [Tüm Kullanıcılar](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-dedicated-groups) grubu

3.  Yönetici, kullanıcının [Uygulama Erişim Paneli](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) Ekle **Uygulaması** özelliğini kullanarak iş onayı **olmadan** uygulama eklemesine izin vermek için Self Servis [Uygulama Erişimi'ni](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) etkinleştirir

4.  Yönetici, kullanıcının [Uygulama Erişim Paneli](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) Ekle **Uygulaması** özelliğini kullanarak bir uygulama eklemesine izin vermek için Self Servis [Uygulama Erişimi'ni](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) etkinleştirir, ancak yalnızca seçilen iş**onaylayanlar kümesinden önceden onay**

5.  Yönetici, [Self Servis Grup Yönetimi'nin](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) bir kullanıcının iş onayı **olmadan** bir uygulamanın atandığı bir gruba katılmasına izin vermesini sağlar

6.  Yönetici, [Self Servis Grup Yönetimi'nin](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) bir kullanıcının bir uygulamanın atandığı bir gruba katılmasına izin verdiğini, ancak yalnızca **seçili iş onaylayıcıları kümesinin önceden onayı ile**

7.  Yönetici, [Microsoft Office 365](https://products.office.com/) gibi doğrudan birinci taraf bir uygulama için kullanıcıya lisans atar

8.  Yönetici, kullanıcının üyesi olduğu bir gruba [Microsoft Office 365](https://products.office.com/) gibi birinci taraf uygulamasına lisans atar

9.  Yönetici, tüm kullanıcılar tarafından kullanılacak [bir uygulamayı kabul eder](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview) ve ardından kullanıcı uygulamaya kabul eder

10. Kullanıcı, uygulamada oturum imzalayarak [bir uygulamayı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview) kabul eder

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamaları Azure Active Directory ile yönetme](what-is-application-management.md)
