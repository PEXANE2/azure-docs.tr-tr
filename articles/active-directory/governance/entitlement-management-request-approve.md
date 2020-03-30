---
title: Erişim isteklerini onaylama veya reddetme - Azure AD yetkilendirme yönetimi
description: Azure Active Directory yetkilendirme yönetiminde bir erişim paketine yapılan istekleri onaylamak veya reddetmek için Erişimim portalını nasıl kullanacağınızı öğrenin.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: mamtakumar
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 10/27/2019
ms.author: ajburnle
ms.reviewer: mamkumar
ms.collection: M365-identity-device-management
ms.openlocfilehash: 20ec63efe16f1120ca6e7d07c8917d8ad2b3a0e0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79261729"
---
# <a name="approve-or-deny-access-requests-in-azure-ad-entitlement-management"></a>Azure AD yetkilendirme yönetiminde erişim isteklerini onaylama veya reddetme

Azure AD yetkilendirme yönetimi yle, ilkeleri erişim paketleri için onay gerektirecek şekilde yapılandırabilir ve bir veya daha fazla onaylayıcı seçebilirsiniz. Bu makalede, atanan onaylayıcıların erişim paketleri isteklerini nasıl onaylayabilir veya reddedebilir.

## <a name="open-request"></a>Açık istek

Erişim isteklerini onaylamak veya reddetmek için ilk adım, onay bekleyen erişim isteğini bulmak ve açmaktır. Erişim isteğini açmanın iki yolu vardır.

**Önkoşul rolü:** Onaylayan

1. Microsoft Azure'dan isteği onaylamanızı veya reddetmenizi isteyen bir e-postayı arayın. İşte örnek bir e-posta:

    ![Paket e-postasına erişmek için isteği onaylama](./media/entitlement-management-shared/approver-request-email.png)

1. Erişim isteğini açmak için **İstek bağlantısını onayla veya reddet** bağlantısını tıklatın.

1. Erişimim portalında oturum açın.

E-postanız yoksa, aşağıdaki adımları izleyerek onayınızı bekleyen erişim isteklerini bulabilirsiniz.

1. adresindeki Erişim portalım'da [https://myaccess.microsoft.com](https://myaccess.microsoft.com)oturum açın.  (ABD Hükümeti için, Erişim portalıbağlantımdaki `myaccess.microsoft.us`etki alanı olacaktır.)

1. Sol menüde, onay bekleyen erişim isteklerilistesini görmek için **Onaylar'ı** tıklatın.

1. Bekleme **sekmesinde** isteği bulun.

## <a name="approve-or-deny-request"></a>İsteği onaylama veya reddetme

Onay bekleyen bir erişim isteğini açtıktan sonra, onay veya reddetme kararı vermenize yardımcı olacak ayrıntıları görebilirsiniz.

**Önkoşul rolü:** Onaylayan

1. Access istek bölmesini açmak için **Görünüm** bağlantısını tıklatın.

1. Erişim isteğiyle ilgili ayrıntıları görmek için **Ayrıntılar'ı** tıklatın.

    Ayrıntılar, kullanıcının adını, kuruluşunu, sağlandığı takdirde erişim başlangıç ve bitiş tarihini, iş gerekçesini, isteğin ne zaman gönderildiğini ve isteğin ne zaman sona ereceğini içerir.

1. Onayla veya **Reddet'i** tıklatın. **Deny**

1. Gerekirse, bir neden girin.

    ![Erişim portalım - Erişim isteği](./media/entitlement-management-request-approve/my-access-approve-request.png)

1. Kararınızı göndermek için **Gönder'i** tıklatın.

    Bir ilke birden çok onaylayıcıyla yapılandırılırsa, bekleyen onay hakkında yalnızca bir onaylayıcının karar alması gerekir. Bir onaylayıcı kararını erişim isteğine sunduktan sonra, istek tamamlanır ve diğer onaylayanların isteği onaylaması veya reddetmesi artık kullanılamaz. Diğer onaylayanlar istek kararını ve karar vericiyi Erişim portalında görebilirler. Şu anda yalnızca tek aşamalı onay desteklenir.

    Yapılandırılan onaylayanlardan hiçbiri erişim isteğini onaylayamaz veya reddedemiyorsa, istek yapılandırılan istek süresinden sonra sona erer. Kullanıcı, erişim isteğinin süresinin dolduğunu ve erişim isteğini yeniden göndermesi gerektiğini bildirdi.

## <a name="next-steps"></a>Sonraki adımlar

- [Erişim paketine erişim isteği](entitlement-management-request-access.md)
- [İstek süreci ve e-posta bildirimleri](entitlement-management-process.md)
