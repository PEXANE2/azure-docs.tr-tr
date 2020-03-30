---
title: Azure AD'de kurumsal bir uygulamanın adını veya logosunu değiştirme
description: Azure Active Directory'deki özel bir kurumsal uygulamanın adını veya logosunu değiştirme
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/05/2019
ms.author: mimart
ms.reviewer: asteen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: d23849df75d1ab239eb269b462abb21df196e7e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79138510"
---
# <a name="change-the-name-or-logo-of-an-enterprise-application-in-azure-active-directory"></a>Azure Active Directory'de kurumsal bir uygulamanın adını veya logosunu değiştirme

Azure Etkin Dizini'nde (Azure AD) özel bir kurumsal uygulamanın adını veya logosunu değiştirmek kolaydır. Bu değişiklikleri yapmak için uygun izinlere sahip olmalısınız ve özel uygulamanın yaratıcısı siz olmalısınız.

## <a name="how-do-i-change-an-enterprise-applications-name-or-logo"></a>Kurumsal bir uygulamanın adını veya logosunu nasıl değiştirebilirim?

1. Dizin için genel bir yönetici olan bir hesapla [Azure Active Directory portalında](https://aad.portal.azure.com/) oturum açın. **Azure Etkin Dizin yöneticisi merkezi** sayfası görüntülenir.
2. Sol bölmede **Kurumsal uygulamalar**’ı seçin. Kurumsal uygulamalarınızın listesi görüntülenir.
3. Bir uygulama seçin. Uygulamaya genel bakış sayfası görüntülenir.
4. Uygulamaya genel bakış bölmesinde, **Yönet** başlığı altında **Özellikler'i**seçin. **Özellikler** sayfası görüntülenir.
5. Adı değiştirmek istiyorsanız, **Ad** kutusunu seçin, yeni adı yazın ve **Enter**tuşuna basın.
6. Logoyu değiştirmek istiyorsanız, **Logo** alanını bulun ve uygulamanın geçerli logo görüntüsünün altında yer alan dosya kutusunu **seçin'in** yanındaki klasör simgesini seçin.

   ![Özellikler komutunu seçme](./media/change-name-or-logo-portal/change-logo.png)

   Aksi takdirde, logoyu değiştirmiyorsanız, adım 8'e gidin.
7. Dosya seçicide, yeni logo olarak istediğiniz dosyayı seçin. Dosyanın adı geçerli logo görüntüsünün altındaki kutuda görünür.

   > [!NOTE]
   > Azure, logo görüntüsünün bir PNG dosyası olmasını gerektirir ve genişlik, yükseklik ve dosya boyutuna sınırlamalar uygular. Özel logolar tam olarak &times; 215 215 piksel boyutunda olmalı ve PNG formatında olmalıdır. Kullanıcılara en iyi şekilde görünmesi için uygulama logonuzda saydamlık olmayan düz renkli bir arka plan kullanmanızı öneririz.
8. **Kaydet'i**seçin. Yeni bir logo seçtiyseniz, **Logo** alanının görüntüsü yeni logo dosyasını yansıtacak şekilde değişir.

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı Başlangıç: Azure Active Directory’de kuruluşunuzun grupları ve üyelerini görüntüleme](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Bir kurumsal uygulamaya kullanıcı veya grup atama](assign-user-or-group-access-portal.md)
* [Bir kurumsal uygulamadan kullanıcı veya grup ataması kaldırma](remove-user-or-group-access-portal.md)
* [Kurumsal bir uygulama için kullanıcı oturum açma larını devre dışı](disable-user-sign-in-portal.md)
