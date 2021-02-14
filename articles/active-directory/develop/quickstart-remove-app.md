---
title: 'Nasıl yapılır: Microsoft Identity platformundan kayıtlı bir uygulamayı kaldırma | Mavisi'
titleSuffix: Microsoft identity platform
description: Bu nasıl yapılır, Microsoft Identity platformu ile kaydedilen bir uygulamayı nasıl kaldıracağınızı öğrenirsiniz.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 11/15/2020
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: marsma, aragra, lenalepa, sureshja
ms.openlocfilehash: 4afffb558b9cbf53a762b1b2bb1ce544e554feaf
ms.sourcegitcommit: 126ee1e8e8f2cb5dc35465b23d23a4e3f747949c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100103898"
---
# <a name="how-to-remove-an-application-registered-with-the-microsoft-identity-platform"></a>Microsoft Identity platformu ile kaydedilen bir uygulamayı kaldırma

Microsoft Identity platformu ile kayıtlı uygulamaları olan kurumsal geliştiriciler ve hizmet olarak yazılım (SaaS) sağlayıcılarının bir uygulamanın kaydını kaldırması gerekebilir.

Aşağıdaki bölümlerde, aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

* Kendiniz veya kuruluşunuz tarafından yazılmış bir uygulamayı kaldırma
* Başka bir kuruluş tarafından yazılmış bir uygulamayı kaldırma

## <a name="prerequisites"></a>Önkoşullar

* [Azure AD kiracınızda kayıtlı bir uygulama](quickstart-register-app.md)

## <a name="remove-an-application-authored-by-you-or-your-organization"></a>Kendiniz veya kuruluşunuz tarafından yazılmış bir uygulamayı kaldırma

Kendiniz veya kuruluşunuz tarafından kaydedilmiş uygulamalar gerek bir uygulama nesnesi, gerekse kiracınızdaki bir hizmet sorumlusu nesnesi tarafından temsil edilir. Daha fazla bilgi için bkz. [Uygulama Nesneleri ve Hizmet Sorumlusu Nesneleri](./app-objects-and-service-principals.md).

Bir uygulamayı silmek için, uygulamanın sahibi olarak listelenmiş veya yönetici ayrıcalıklarına sahip olmanız gerekir.

1. <a href="https://portal.azure.com/" target="_blank">Azure portalında</a> oturum açın.
1. Birden fazla kiracıya erişiminiz varsa,  :::image type="icon" source="./media/common/portal-directory-subscription-filter.png" border="false"::: uygulamanın kaydedildiği kiracıyı seçmek Için üst menüdeki Dizin + abonelik filtresini kullanın.
1. **Azure Active Directory** arayın ve seçin. 
1. **Yönet**' in altında **uygulama kayıtları** ' yi seçin ve yapılandırmak istediğiniz uygulamayı seçin. Uygulamayı seçtikten sonra uygulamanın **Genel Bakış** sayfasını görebilirsiniz.
1. **Genel Bakış** sayfasından **Sil**'i seçin.
1. Uygulamayı silmek istediğinizi onaylamak için **Evet**'i seçin.

## <a name="remove-an-application-authored-by-another-organization"></a>Başka bir kuruluş tarafından yazılmış bir uygulamayı kaldırma

Bir kiracı bağlamında **Uygulama kayıtlarını** görüntülüyorsanız, **Tüm uygulamalar** sekmesinin altında görünen uygulamaların bir alt kümesi başka bir kiracıdandır ve kiracınıza onay işlemi sırasında kaydedilmiştir. Daha açık söylersek, bunlar kiracınızda yalnızca bir hizmet sorumlusu nesnesi tarafından ve karşılık gelen herhangi bir uygulama nesnesi olmadan temsil edilir. Uygulama ve hizmet sorumlusu nesneleri arasındaki farklar hakkında daha fazla bilgi için bkz. [Azure AD'de uygulama ve hizmet sorumlusu nesneleri](./app-objects-and-service-principals.md).

Bir uygulamanın dizininize olan daha önce onay verdiğiniz erişimini kaldırmak için şirket yöneticisinin uygulamanın hizmet sorumlusunu kaldırması gerekir. Yöneticinin, genel yönetici erişimi olmalıdır ve uygulamayı Azure portal aracılığıyla kaldırabilir ya da erişimi kaldırmak için [Azure AD PowerShell cmdlet 'lerini](/previous-versions/azure/jj151815(v=azure.100)) kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Microsoft Identity platformunda [uygulama ve hizmet sorumlusu nesneleri](app-objects-and-service-principals.md) hakkında daha fazla bilgi edinin.
