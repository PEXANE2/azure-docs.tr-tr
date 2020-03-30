---
title: Erişim paketi isteme - Azure AD yetkilendirme yönetimi
description: Azure Active Directory yetkilendirme yönetiminde bir erişim paketine erişim isteğinde bulunmak için Erişimim portalını nasıl kullanacağınızı öğrenin.
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
ms.date: 10/26/2019
ms.author: ajburnle
ms.reviewer: mamkumar
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4b1ccde2f1f92237978ab4e68acaa26393bbb9d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79261755"
---
# <a name="request-access-to-an-access-package-in-azure-ad-entitlement-management"></a>Azure AD yetkilendirme yönetiminde bir erişim paketine erişim isteğinde bulunmak

Azure AD yetkilendirme yönetimi yle, erişim paketi, erişim paketinin ömrü boyunca erişimi otomatik olarak yöneten kaynakların ve ilkelerin tek seferlik bir şekilde ayarlanmasını sağlar. 

Bir erişim paketi yöneticisi, kullanıcıların paketlere erişmesi için onay gerektirecek şekilde ilkeleri yapılandırabilir. Bir erişim paketine erişmesi gereken bir kullanıcı erişim için istek gönderebilir. Bu makalede, bir erişim isteği nasıl gönderilen açıklanmaktadır.

## <a name="sign-in-to-the-my-access-portal"></a>Erişimim portalında oturum açın

İlk adım, bir erişim paketine erişim isteğinde bulunabileceğiniz Erişimim portalında oturum açmanızdır.

**Önkoşul rolü:** Isteyici

1. Birlikte çalıştığınız projeden veya işletme yöneticisinden bir e-posta veya ileti arayın. E-posta, erişmesi gereken erişim paketine bir bağlantı içermelidir. Bağlantı, `myaccess`bir dizin ipucu içerir ve bir erişim paketi kimliği ile sona erer.  (ABD Hükümeti için, etki `https://myaccess.microsoft.us` alanı bunun yerine olabilir.)
 
    `https://myaccess.microsoft.com/@<directory_hint>#/access-packages/<access_package_id>`

1. Bağlantıyı açın.

1. Erişimim portalında oturum açın.

    Kuruluş (iş veya okul) hesabınızı kullandığınızdan emin olun. Emin değilseniz, projenizi veya işletme yöneticinizi kontrol edin.

## <a name="request-an-access-package"></a>Erişim paketi isteme

Erişim paketini Erişimim portalında bulduktan sonra bir istek gönderebilirsiniz.

**Önkoşul rolü:** Isteyici

1. Listedeki erişim paketini bulun.  Gerekirse, bir arama dizesi yazıp **Ad,** **Katalog**veya **Kaynaklar** filtresini seçerek arama yapabilirsiniz.

    ![Erişim portalım - Kaynak arama](./media/entitlement-management-request-access/my-access-resource-search.png)

1. Erişim paketini seçmek için onay işaretini tıklatın.

1. İstek erişim bölmesini açmak için **Erişim İsteği'ni** tıklatın.

    ![Erişim portalım - Erişim paketleri](./media/entitlement-management-request-access/my-access-request-access-button.png)

1. İş **yaslama** kutusu görüntülenirse, erişim eihtiyacınız için bir gerekçe yazın.

1. Belirli bir dönem için İstek etkinleştirilirse, **Evet** veya **Hayır'ı**seçin. **Request for specific period?**

1. Gerekirse, başlangıç tarihini ve bitiş tarihini belirtin.

    ![Erişim portalım - Erişim isteği](./media/entitlement-management-shared/my-access-request-access.png)

1. Tamamlandığında, isteğinizi göndermek için **Gönder'i** tıklatın.

1. İsteklerinizin ve durumun listesini görmek için **Geçmiş İstek'i** tıklatın.

    Erişim paketi onay gerektiriyorsa, istek artık bekleyen bir onay durumundadır.

### <a name="select-a-policy"></a>İlke seçme

Birden çok ilke içeren bir erişim paketine erişim isteğinde bulunursanız, bir ilke seçmeniz istenebilir. Örneğin, bir erişim paketi yöneticisi, iki dahili çalışan grubu için iki ilke içeren bir erişim paketini yapılandırabilir. İlk ilke 60 gün boyunca erişime izin verebilir ve onay gerektirebilir. İkinci ilke 2 gün boyunca erişime izin verebilir ve onay gerektirmeyebilir. Bu senaryoyla karşılaşırsanız, kullanmak istediğiniz ilkeyi seçmeniz gerekir.

![Erişim portalım - Erişim isteği - birden çok ilke](./media/entitlement-management-request-access/my-access-multiple-policies.png)

## <a name="resubmit-a-request"></a>İsteği yeniden gönderme

Bir erişim paketine erişim isteğinde bulunduğunuzda, isteğiniz reddedilebilir veya onaylayanların zamanında yanıt vermezse isteğinizin süresi dolabilir. Erişime ihtiyacınız varsa, yeniden deneyebilir ve isteğinizi yeniden gönderebilirsiniz. Aşağıdaki yordam, bir erişim isteğinin nasıl yeniden gönderilen nasıl açıklanmaktadır:

**Önkoşul rolü:** Isteyici

1. **Erişimim** portalında oturum açın.

1. Soldaki gezinti menüsünden **Geçmiş İste'yi** tıklatın.

1. İsteğe yeniden gönderdiğiniz erişim paketini bulun.

1. Erişim paketini seçmek için onay işaretini tıklatın.

1. Seçili erişim paketinin sağındaki mavi **Görünüm** bağlantısını tıklatın.
    
    ![Erişim paketini seçin ve bağlantıyı görüntüle](./media/entitlement-management-request-access/resubmit-request-select-request-and-view.png)

    Erişim paketi için istek geçmişiyle birlikte sağa bir bölme açılır.
    
    ![Yeniden Gönder düğmesini seçin](./media/entitlement-management-request-access/resubmit-request-select-resubmit.png)

1. Bölmenin altındaki **Yeniden Gönder** düğmesini tıklatın.

## <a name="cancel-a-request"></a>İsteği iptal etme

Bir erişim isteği gönderirseniz ve istek hala **bekleyen onay** durumundaysa, isteği iptal edebilirsiniz.

**Önkoşul rolü:** Isteyici

1. Soldaki Erişimim portalında, isteklerinizin ve durumun listesini görmek için **Geçmiş İstek'i** tıklatın.

1. İptal etmek istediğiniz istek için **Görünüm** bağlantısını tıklatın.

1. İstek **bekleyen onay** durumundaysa, isteği iptal etmek için **İstek'i İptal** et isteğini tıklatabilirsiniz.

    ![Erişim portalım - İstek iptal et](./media/entitlement-management-request-access/my-access-cancel-request.png)

1. **İsteğin** iptal edildiğini doğrulamak için İstek geçmişini tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

- [Erişim isteklerini onaylama veya reddetme](entitlement-management-request-approve.md)
- [İstek süreci ve e-posta bildirimleri](entitlement-management-process.md)
