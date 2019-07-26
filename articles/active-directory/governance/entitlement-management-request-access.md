---
title: Azure AD Yetkilendirme Yönetimi 'nde (Önizleme) bir erişim paketine erişim isteme-Azure Active Directory
description: Azure Active Directory yetkilendirme yönetimi 'nde (Önizleme) bir erişim paketine erişim istemek için erişim portalını nasıl kullanacağınızı öğrenin.
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
ms.date: 04/19/2019
ms.author: ajburnle
ms.reviewer: mamkumar
ms.collection: M365-identity-device-management
ms.openlocfilehash: 578f78d1853194cad000aa2e54b40d1ac5003544
ms.sourcegitcommit: bafb70af41ad1326adf3b7f8db50493e20a64926
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68489040"
---
# <a name="request-access-to-an-access-package-in-azure-ad-entitlement-management-preview"></a>Azure AD Yetkilendirme Yönetimi 'nde (Önizleme) bir erişim paketine erişim isteme

> [!IMPORTANT]
> Azure Active Directory (Azure AD) yetkilendirme yönetimi şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="sign-in-to-the-my-access-portal"></a>Erişim Portalı 'nda oturum açın

İlk adım, erişim paketine erişim isteyebileceğiniz erişim portalı 'nda oturum açmak için kullanılır.

**Önkoşul rolü:** İstek sahibi

1. Çalıştığınız proje veya iş Yöneticisi için bir e-posta veya bir ileti arayın. E-posta, erişmeniz gereken erişim paketine bir bağlantı içermelidir. Bağlantı şu şekilde başlayacaktır:

    `https://myaccess.microsoft.com`

1. Bağlantıyı açın.

1. Erişim Portalı ' nda oturum açın.

    Kuruluş hesabınızı kullandığınızdan emin olun. Emin değilseniz, projeniz veya Business Manager ile görüşün.

## <a name="request-an-access-package"></a>Erişim paketi isteme

Access Portal 'da erişim paketini bulduktan sonra bir istek gönderebilirsiniz.

**Önkoşul rolü:** İstek sahibi

1. Erişim paketini seçmek için onay işaretine tıklayın.

    ![Erişim Portalı-erişim paketlerim](./media/entitlement-management-shared/my-access-access-packages.png)

1. Istek erişimi bölmesini açmak için **erişim iste** ' ye tıklayın.

1. **İş gerekçe** kutusu görüntüleniyorsa, erişim gereksinimi için bir gerekçe yazın.

1. **Belirli bir döneme yönelik istek** etkinleştirilirse, **Evet** veya **Hayır**' ı seçin.

1. Gerekirse, başlangıç tarihini ve bitiş tarihini belirtin.

    ![Erişim Portalı-erişim ISTEME](./media/entitlement-management-shared/my-access-request-access.png)

1. İşiniz bittiğinde, isteğinizi göndermek için **Gönder** ' e tıklayın.

1. İsteklerinizin ve durumun listesini görmek için **istek geçmişi** ' ne tıklayın.

    Erişim paketi onay gerektiriyorsa, istek artık bekleyen bir onay durumundadır.

## <a name="cancel-a-request"></a>İsteği iptal et

Bir erişim isteği gönderirseniz ve istek hala **Bekleyen onay** durumundaysa, isteği iptal edebilirsiniz.

**Önkoşul rolü:** İstek sahibi

1. İsteklerinizin ve durumun listesini görmek için, sol taraftaki erişim Portalı ' nda, **istek geçmişi** ' ne tıklayın.

1. İptal etmek istediğiniz isteğin **Görünüm** bağlantısına tıklayın.

1. İstek hala **Bekleyen onay** durumundaysa, isteği iptal etmek Için Isteği **iptal et** ' e tıklayabilirsiniz.

    ![Erişim Portalı-isteği Iptal et](./media/entitlement-management-request-access/my-access-cancel-request.png)

1. İsteğin iptal edildiğini onaylamak için **istek geçmişi** ' ne tıklayın.

## <a name="select-a-policy"></a>İlke seçme

Uygulanan birden çok ilkeye sahip bir erişim paketine erişim isteğinde bulunduğunuzu seçerseniz, bir ilke seçmeniz istenebilir. Örneğin, bir erişim paketi Yöneticisi iki dahili çalışan grubu için iki ilkeye sahip bir erişim paketi yapılandırabilir. İlk ilke 60 gün boyunca erişime izin verebilir ve onay gerektirebilir. İkinci ilke 2 gün boyunca erişime izin verebilir ve onay gerektirmez. Bu senaryoyla karşılaşırsanız, kullanmak istediğiniz ilkeyi seçmeniz gerekir.

**Önkoşul rolü:** İstek sahibi

## <a name="next-steps"></a>Sonraki adımlar

- [Erişim isteklerini onaylama veya reddetme](entitlement-management-request-approve.md)
- [İşlem ve e-posta bildirimleri iste](entitlement-management-process.md)
