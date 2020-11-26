---
title: 'Hızlı başlangıç: kayıtlı bir uygulamayı Microsoft Identity platformundan kaldırma | Mavisi'
titleSuffix: Microsoft identity platform
description: Bu hızlı başlangıçta, Microsoft Identity platformu ile kaydedilen bir uygulamayı nasıl kaldıracağınızı öğreneceksiniz.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 05/08/2019
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: aragra, lenalepa, sureshja
ms.openlocfilehash: aa0662ae0b1caa63938f5e07aea10b9af60661d3
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2020
ms.locfileid: "96173269"
---
# <a name="quickstart-remove-an-application-registered-with-the-microsoft-identity-platform"></a>Hızlı başlangıç: Microsoft Identity platformu ile kaydedilmiş bir uygulamayı kaldırma

Microsoft kimlik platformuna kayıtlı uygulamaları olan kurumsal geliştiricilerin ve hizmet olarak yazılım (SaaS) sağlayıcılarının bir uygulamanın kaydını kaldırması gerekebilir.

Bu hızlı başlangıçta şunları yapmayı öğreneceksiniz:

* Kendiniz veya kuruluşunuz tarafından yazılmış bir uygulamayı kaldırma
* Başka bir kuruluş tarafından yazılmış bir uygulamayı kaldırma

## <a name="prerequisites"></a>Ön koşullar

* [Hızlı başlangıç: Microsoft Identity platformu ile bir uygulamayı kaydetme](quickstart-register-app.md)

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

Microsoft Identity platformunda uygulama ve hizmet sorumlusu nesneleri hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Azure Active Directory'deki uygulama ve hizmet sorumlusu nesneleri](app-objects-and-service-principals.md)