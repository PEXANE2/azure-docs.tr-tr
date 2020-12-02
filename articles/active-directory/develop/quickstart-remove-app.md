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
ms.openlocfilehash: edfed8ca361d456e622096e23079d5887f96ab30
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96452988"
---
# <a name="how-to-remove-an-application-registered-with-the-microsoft-identity-platform"></a>Microsoft Identity platformu ile kaydedilen bir uygulamayı kaldırma

Microsoft kimlik platformuna kayıtlı uygulamaları olan kurumsal geliştiricilerin ve hizmet olarak yazılım (SaaS) sağlayıcılarının bir uygulamanın kaydını kaldırması gerekebilir.

Aşağıdaki bölümlerde, aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

* Kendiniz veya kuruluşunuz tarafından yazılmış bir uygulamayı kaldırma
* Başka bir kuruluş tarafından yazılmış bir uygulamayı kaldırma

## <a name="prerequisites"></a>Önkoşullar

* [Azure AD kiracınızda kayıtlı bir uygulama](quickstart-register-app.md)

## <a name="remove-an-application-authored-by-you-or-your-organization"></a>Kendiniz veya kuruluşunuz tarafından yazılmış bir uygulamayı kaldırma

Kendiniz veya kuruluşunuz tarafından kaydedilmiş uygulamalar gerek bir uygulama nesnesi, gerekse kiracınızdaki bir hizmet sorumlusu nesnesi tarafından temsil edilir. Daha fazla bilgi için bkz. [Uygulama Nesneleri ve Hizmet Sorumlusu Nesneleri](./app-objects-and-service-principals.md).

Uygulama silmek için uygulamanın sahiplerinden biri olarak listelenmeniz veya yönetici ayrıcalıklarına sahip olmanız gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
1. Sol taraftaki Gezinti bölmesinde **Azure Active Directory** hizmetini seçin ve **uygulama kayıtları**' ı seçin. Yapılandırmak istediğiniz uygulamayı bulun ve seçin. Uygulamayı seçtikten sonra uygulamanın **Genel Bakış** sayfasını görebilirsiniz.
1. **Genel Bakış** sayfasından **Sil**'i seçin.
1. Uygulamayı silmek istediğinizi onaylamak için **Evet**'i seçin.

## <a name="remove-an-application-authored-by-another-organization"></a>Başka bir kuruluş tarafından yazılmış bir uygulamayı kaldırma

Bir kiracı bağlamında **Uygulama kayıtlarını** görüntülüyorsanız, **Tüm uygulamalar** sekmesinin altında görünen uygulamaların bir alt kümesi başka bir kiracıdandır ve kiracınıza onay işlemi sırasında kaydedilmiştir. Daha açık söylersek, bunlar kiracınızda yalnızca bir hizmet sorumlusu nesnesi tarafından ve karşılık gelen herhangi bir uygulama nesnesi olmadan temsil edilir. Uygulama ve hizmet sorumlusu nesneleri arasındaki farklar hakkında daha fazla bilgi için bkz. [Azure AD'de uygulama ve hizmet sorumlusu nesneleri](./app-objects-and-service-principals.md).

Bir uygulamanın dizininize olan daha önce onay verdiğiniz erişimini kaldırmak için şirket yöneticisinin uygulamanın hizmet sorumlusunu kaldırması gerekir. Yöneticinin genel yönetim erişimi olması gerekir ve uygulamayı Azure portalı aracılığıyla kaldırabilir veya erişimi kaldırmak için [Azure AD PowerShell Cmdlet'lerini](/previous-versions/azure/jj151815(v=azure.100)) kullanabilir.

## <a name="next-steps"></a>Sonraki adımlar

Microsoft Identity platformunda [uygulama ve hizmet sorumlusu nesneleri](app-objects-and-service-principals.md) hakkında daha fazla bilgi edinin.
