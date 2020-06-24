---
title: Azure AD 'de kurumsal uygulamanın adını veya logosunu değiştirme
description: Azure Active Directory içindeki özel bir kurumsal uygulamanın adını veya logosunu değiştirme
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 04/05/2019
ms.author: kenwith
ms.reviewer: asteen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8f8902f306f50659725bc4b12fbbb6fee7563c4b
ms.sourcegitcommit: bc943dc048d9ab98caf4706b022eb5c6421ec459
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2020
ms.locfileid: "84763678"
---
# <a name="change-the-name-or-logo-of-an-enterprise-application-in-azure-active-directory"></a>Azure Active Directory bir kurumsal uygulamanın adını veya logosunu değiştirme

Azure Active Directory (Azure AD) içinde özel bir kurumsal uygulama için adı veya logoyu değiştirmek kolaydır. Bu değişiklikleri yapmak için uygun izinlere sahip olmanız gerekir ve özel uygulamanın oluşturucusu olmanız gerekir.

## <a name="how-do-i-change-an-enterprise-applications-name-or-logo"></a>Nasıl yaparım? bir kurumsal uygulamanın adı veya logosu değiştirilsin mi?

1. [Azure Active Directory portalında](https://aad.portal.azure.com/) , dizin için genel yönetici olan bir hesapla oturum açın. **Azure Active Directory Yönetim Merkezi** sayfası görüntülenir.
2. Sol bölmede **Kurumsal uygulamalar**’ı seçin. Kurumsal uygulamalarınızın listesi görüntülenir.
3. Bir uygulama seçin. Uygulamaya genel bakış sayfası görüntülenir.
4. Uygulamaya Genel Bakış bölmesinde, **Yönet** başlığı altında **Özellikler**' i seçin. **Özellikler** sayfası görüntülenir.
5. Adı değiştirmek istiyorsanız, **ad** kutusunu seçin, yeni adı yazın ve **ENTER**tuşuna basın.
6. Logoyu değiştirmek istiyorsanız, **logo** alanını bulun ve uygulamanın geçerli logosu görüntüsünün altında olan **Dosya Seç** kutusunun yanındaki klasör simgesini seçin.

   ![Özellikler komutunu seçme](./media/change-name-or-logo-portal/change-logo.png)

   Aksi takdirde, logoyu değiştirmiyorsanız, 8. adıma gidin.
7. Dosya seçicide yeni logo olarak istediğiniz dosyayı seçin. Dosyanın adı geçerli logo resminin altındaki kutuda görüntülenir.

   > [!NOTE]
   > Azure, logo resminin bir PNG dosyası olmasını gerektirir ve genişlik, yükseklik ve dosya boyutu sınırlarını uygular. Özel logolar tam olarak 215 &times; 215 piksel boyutunda olmalıdır ve PNG biçiminde olmalıdır. Kullanıcılara en iyi şekilde görünebilmeniz için uygulama logonuzda saydamlık olmadan bir düz renk arka planı kullanmanızı öneririz.
8. **Kaydet**’i seçin. Yeni bir logo seçerseniz, **logo** alanının görüntüsü yeni logo dosyasını yansıtacak şekilde değişir.

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı Başlangıç: Azure Active Directory’de kuruluşunuzun grupları ve üyelerini görüntüleme](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Kurumsal uygulamaya Kullanıcı veya Grup atama](assign-user-or-group-access-portal.md)
* [Bir kurumsal uygulamadan Kullanıcı veya grup atamasını kaldırma](remove-user-or-group-access-portal.md)
* [Kurumsal uygulama için Kullanıcı oturum açma işlemlerini devre dışı bırakma](disable-user-sign-in-portal.md)
