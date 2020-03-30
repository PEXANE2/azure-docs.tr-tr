---
title: Azure AD uygulamaları için bağlantılı oturum açma - Microsoft kimlik platformu
description: Microsoft kimlik platformundaki (Azure AD) uygulamalarınızla bağlantılı tek oturum açma (SSO) yapılandırma
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: article
ms.workload: identity
ms.date: 05/08/2019
ms.author: mimart
ms.reviewer: arvinh,luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: dfe4aeb17f482cc9d4126efc6d65d3f7d173536b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063552"
---
# <a name="configure-linked-sign-on"></a>Bağlantılı oturum açmayı yapılandırma

Bir galeri veya galeri olmayan web uygulaması eklediğinizde, kullanabileceğiniz tek oturum açma seçeneklerinden biri [oturum açma bağlantılıdır.](what-is-single-sign-on.md) Kuruluşunuzun Azure AD Erişim Paneli veya Office 365 portalındaki uygulamaya bağlantı eklemek için bu seçeneği belirleyin. Bu yöntemi, kimlik doğrulaması için Azure AD yerine şu anda Active Directory Federation Services (veya başka federasyon hizmeti) kullanan özel web uygulamalarına bağlantılar eklemek için kullanabilirsiniz. Veya, kullanıcınızın Erişim Panellerinde görünmesini istediğiniz belirli SharePoint sayfalarına veya diğer web sayfalarına derin bağlantılar ekleyebilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Uygulama Azure AD kiracınıza eklenmediyse, [bkz.](add-gallery-app.md) [Add a non-gallery app](add-non-gallery-app.md)

### <a name="open-the-app-and-select-linked-sign-on"></a>Uygulamayı açın ve bağlantılı oturum açma'yı seçin

1. [Azure portalında](https://portal.azure.com) bulut uygulaması yöneticisi veya Azure AD kiracınız için bir uygulama yöneticisi olarak oturum açın.

1. Azure **Active Directory** > **Enterprise uygulamalarına**gidin. Azure AD kiracınızdaki uygulamaların rasgele bir örneği görüntülenir. 

1. Uygulama **Türü** menüsünde **Tüm uygulamaları**seçin ve sonra **Uygula'yı**seçin.

1. Arama kutusuna uygulamanın adını girin ve ardından sonuçlardan uygulamayı seçin.

1. **Yönet** bölümünde, **Tek oturum açma'yı**seçin. 

1. **Bağlantılı'yı**seçin.

1. Bağlantı vermek için uygulamanın URL'sini girin. URL yazın ve **Kaydet'i**seçin. 
 
1. Uygulamanın [Office 365 uygulama başlatıcısında](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) veya bu kullanıcılar için [Azure AD erişim panelinde](end-user-experiences.md) görünmesine neden olan uygulamaya kullanıcıları ve grupları atayabilirsiniz.

1. **Kaydet'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Kullanıcı veya grupları uygulamaya atama](methods-for-assigning-users-and-groups.md)
- [Otomatik kullanıcı hesabı sağlama yapılandırma](../app-provisioning/configure-automatic-user-provisioning-portal.md)
