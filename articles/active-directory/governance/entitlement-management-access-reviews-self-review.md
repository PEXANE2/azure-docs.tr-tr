---
title: Azure AD yetkilendirme yönetiminde bir erişim paketinin kendi kendine gözden geçirilmesi
description: Azure Active Directory erişim incelemelerinde yetkilendirme yönetimi erişim paketlerinin kullanıcı erişimini nasıl inceleyiş olarak inceleyiş gösterebilirsiniz (Önizleme).
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 11/01/2019
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 45da1170705bab1206a98c59e02c7616c25ce502
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78967765"
---
# <a name="self-review-of-an-access-package-in-azure-ad-entitlement-management"></a>Azure AD yetkilendirme yönetiminde bir erişim paketinin kendi kendine gözden geçirilmesi

Azure AD yetkilendirme yönetimi, işletmelerin gruplara, uygulamalara ve SharePoint sitelerine erişimi nasıl yönettiğini basitleştirir. Bu makalede, bir kullanıcının atanan erişim paketini (ler) nasıl kendi kendine gözden geçirdiğini açıklamaktadır.

## <a name="open-the-access-review"></a>Erişim incelemesini açma

Erişim incelemesi yapmak için önce erişim incelemesini açmanız gerekir. Erişim incelemesini bulmak ve açmak için aşağıdaki yordamı kullanın:

1. Microsoft'tan erişimi gözden geçirmenizi isteyen bir e-posta alabilirsiniz. Erişim incelemesini açmak için e-postayı bulun. Erişimin gözden geçirilmesini isteyen bir e-posta örneği aşağıda verilmiştir: 
    
    ![Kendi kendini gözden geçiren e-postaya erişin](./media/entitlement-management-access-reviews-review-access/self-review-reviewer-email.png)

1. Erişim bağlantısını **gözden geçir'i** tıklatın.

1. Ayrıca, e-posta https://myaccess.microsoft.com almazsanız bekleyen erişim yorumlarınızı bulmak için doğrudan gidebilirsiniz.  (ABD Hükümeti için, bunun yerine kullanın.) `https://myaccess.microsoft.us`

1. Size atanan bekleyen erişim yorumlarının listesini görmek için sol daki gezinti çubuğundaki **Erişim yorumlarını** tıklatın.


1.  Başlamak istediğiniz incelemeyi tıklatın.

## <a name="perform-the-access-review"></a>Erişim incelemesini gerçekleştirme

Erişim incelemesini açtıktan sonra erişiminizi görebilirsiniz. Erişim incelemesini yapmak için aşağıdaki yordamı kullanın:

1.  Erişim paketine hala erişmeye ihtiyacınız olup olmadığına karar verin. Örneğin, üzerinde çalıştığınız proje tamamlanmadığından, proje üzerinde çalışmaya devam etmek için yine de erişime ihtiyacınız var.

1.  Erişiminizi korumak için **Evet'i** tıklatın veya erişiminizi kaldırmak için **Hayır'ı** tıklatın.
    >[!NOTE]
    >Artık erişime ihtiyacınız olmadığını belirttiyseniz, erişim paketinden hemen kaldırılmaz. İnceleme sona erdiğinde veya yönetici incelemeyi durdurursa erişim paketinden kaldırılırsınız.

1.  **Evet'i**tıklattıysanız, **Neden** kutusuna bir yama bildirimi eklemeniz gerekebilir.

1.  **Gönder'i**tıklatın.

Fikrinizi değiştirir ve inceleme nin bitiminden önce yanıtınızı değiştirmeye karar verirseniz incelemeye geri dönebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Erişim paketlerine erişimi gözden geçirme](entitlement-management-access-reviews-review-access.md) 
