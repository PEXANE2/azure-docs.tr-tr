---
title: Azure AD Yetkilendirme Yönetimi 'nde (Önizleme) erişim isteklerini onaylama veya reddetme-Azure Active Directory
description: Azure Active Directory yetkilendirme yönetimi 'nde (Önizleme) bir erişim paketine yönelik istekleri onaylamak veya reddetmek için erişim portalını nasıl kullanacağınızı öğrenin.
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
ms.date: 04/18/2019
ms.author: ajburnle
ms.reviewer: mamkumar
ms.collection: M365-identity-device-management
ms.openlocfilehash: 78243f02cbbe3d06b94ee52d6193865dbfa27121
ms.sourcegitcommit: bafb70af41ad1326adf3b7f8db50493e20a64926
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68488993"
---
# <a name="approve-or-deny-access-requests-in-azure-ad-entitlement-management-preview"></a>Azure AD Yetkilendirme Yönetimi 'nde erişim isteklerini onaylama veya reddetme (Önizleme)

> [!IMPORTANT]
> Azure Active Directory (Azure AD) yetkilendirme yönetimi şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure AD yetkilendirme yönetimi sayesinde, ilkeleri erişim paketleri için onay gerektirecek şekilde yapılandırabilir ve bir veya daha fazla onaylayan seçebilirsiniz. Bu makalede, belirlenen onaylayanlar erişim paketleri için istekleri onaylama veya reddetme işlemlerinin nasıl yapılacağı açıklanır.

## <a name="open-request"></a>Açık istek

Erişim isteklerini onaylamaya veya reddetmeye yönelik ilk adım, onay bekleyen erişim isteğini bulup açar. Erişim isteğini açmak için iki yol vardır.

**Önkoşul rolü:** Onaylayan

1. Bir isteği onaylamayı veya reddetmenizi isteyen Microsoft Azure bir e-posta bulun. Örnek bir e-posta aşağıda verilmiştir:

    ![Paket e-postasına erişim isteğini onayla](./media/entitlement-management-shared/email-approve-request.png)

1. Erişim isteğini açmak için **Onayla veya reddetme isteği** bağlantısına tıklayın.

1. Erişim Portalı ' nda oturum açın.

E-postanız yoksa, bu adımları izleyerek Onayınızı bekleyen erişim isteklerini bulabilirsiniz.

1. Konumundaki [https://myaccess.microsoft.com](https://myaccess.microsoft.com)erişim portalında oturum açın.

1. Onay bekleyen erişim isteklerinin bir listesini görmek için sol taraftaki menüden **onaylar** ' a tıklayın.

1. **Bekleyen** sekmesinde, isteği bulun.

## <a name="approve-or-deny-request"></a>İsteği onayla veya Reddet

Onay bekleyen bir erişim isteği açtıktan sonra, bir onaylama veya reddetme kararı vermenize yardımcı olacak ayrıntıları görebilirsiniz.

**Önkoşul rolü:** Onaylayan

1. Erişim isteği bölmesini açmak için **Görünüm** bağlantısına tıklayın.

1. Erişim isteğiyle ilgili ayrıntıları görmek için **Ayrıntılar** ' a tıklayın.

    Ayrıntılar arasında kullanıcının adı, kuruluş, erişim başlangıç ve bitiş tarihi, iş gerekçe, isteğin gönderildiği zaman ve isteğin ne zaman dolacağı bulunur.

1. **Onayla** veya **Reddet**' e tıklayın.

1. Gerekirse bir neden girin.

    ![Erişim Portalı-erişim isteği](./media/entitlement-management-shared/my-access-approve-request.png)

1. Kararınızı göndermek için **Gönder** ' e tıklayın.

    Bir ilke birden çok onaylayıcı ile yapılandırıldıysa, yalnızca bir onaylayanın bekleyen onay ile ilgili bir karar vermesini sağlaması gerekir. Bir onaylayan, erişim isteğine kararını gönderdikten sonra, istek tamamlanır ve diğer onaylayanların isteği onaylaması veya reddetmesi için artık kullanılamaz. Diğer onaylayanlar, erişim Portalı ' nda istek kararlarını ve karar Oluşturucu 'yu görebilir. Şu anda yalnızca tek aşamalı onay desteklenir.

    Yapılandırılmış onaylayanlardan hiçbiri erişim isteğini onaylayabilir veya reddedeiyorsa, istek süresi yapılandırılan istek süresinden sonra dolar. Kullanıcı, erişim isteğinin bittiğini ve erişim isteğini yeniden gönderebilmeleri gerektiğini bildirdi.

## <a name="next-steps"></a>Sonraki adımlar

- [Erişim paketine erişim isteme](entitlement-management-request-access.md)
- [İşlem ve e-posta bildirimleri iste](entitlement-management-process.md)
