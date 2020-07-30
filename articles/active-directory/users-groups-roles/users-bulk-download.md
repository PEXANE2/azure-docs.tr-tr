---
title: Azure Active Directory portalındaki kullanıcıların bir listesini indirin | Microsoft Docs
description: Kullanıcı kayıtlarını Azure Yönetim Merkezi 'nde Azure Active Directory ' de toplu olarak indirin.
services: active-directory
author: curtand
ms.author: curtand
manager: daveba
ms.date: 04/16/2020
ms.topic: how-to
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: krbain
ms.collection: M365-identity-device-management
ms.openlocfilehash: cb119ef0ffd4da4dc524c9d3c0a88b94e2251142
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87423567"
---
# <a name="download-a-list-of-users-in-azure-active-directory-portal"></a>Azure Active Directory portalındaki kullanıcıların listesini indirme

Azure Active Directory (Azure AD) toplu Kullanıcı içeri aktarma (oluşturma) işlemlerini destekler.

## <a name="required-permissions"></a>Gerekli izinler

Azure AD Yönetim merkezinden Kullanıcı listesini indirmek için Azure AD 'de kuruluş düzeyindeki bir veya daha fazla yönetici rolüne (Kullanıcı Yöneticisi gereken en düşük rol) atanmış bir kullanıcı ile oturum açmanız gerekir. Konuk davetci ve uygulama geliştiricisi yönetici rolleri olarak değerlendirilmez.

## <a name="to-download-a-list-of-users"></a>Kullanıcı listesini indirmek için

1. Kuruluştaki bir Kullanıcı Yöneticisi hesabıyla [Azure AD kuruluşunuzda oturum açın](https://aad.portal.azure.com) .
2. Azure Active Directory > kullanıcılara gidin. Ardından, her kullanıcının yanındaki sol sütundaki kutuyu seçerek karşıdan yüklemeye dahil etmek istediğiniz kullanıcıları seçin. Note: Şu anda, dışarı aktarma için tüm kullanıcıları seçmenin bir yolu yoktur. Her birinin tek tek seçilmiş olması gerekir.
3. Azure AD **'de kullanıcılar**  >  **kullanıcıları indirir**' ı seçin.
4. **Kullanıcıları indir** sayfasında, Kullanıcı profili özelliklerini listelemek üzere bir CSV dosyası almak için **Başlat** ' ı seçin. Hatalar varsa, sonuçlar dosyasını toplu işlem sonuçları sayfasında indirebilir ve görüntüleyebilirsiniz. Dosya her hatanın nedenini içerir.

   ![Listeden indirmek istediğiniz kullanıcıları seçin](./media/users-bulk-download/bulk-download.png)

   İndirme dosyası, filtrelenmiş Kullanıcı listesini içerecektir.

   Aşağıdaki Kullanıcı öznitelikleri dahil edilmiştir:

   - userPrincipalName
   - displayName
   - surname
   - posta
   - givenName
   - Uzantının
   - userType
   - jobTitle
   - bölüm
   - accountEnabled
   - usageLocation
   - streetAddress
   - durum
   - ülke
   - physicalDeliveryOfficeName
   - city
   - postalCode
   - telephoneNumber 'dır
   - mobil
   - authenticationPhoneNumber
   - authenticationAlternativePhoneNumber
   - authenticationEmail
   - Alternateemaadresi
   - ageGroup
   - consentProvidedForMinor
   - Ligalagegroupclassification

## <a name="check-status"></a>Durumu kontrol etme

Bekleyen toplu isteklerinizin durumunu **toplu işlem sonuçları** sayfasında görebilirsiniz.

[![Toplu Işlemler sonuçları sayfasında durumu kontrol edin.](media/users-bulk-download/bulk-center.png)](media/users-bulk-download/bulk-center.png#lightbox)

## <a name="bulk-download-service-limits"></a>Hizmet sınırlarını toplu indirme

Kullanıcıların bir listesini oluşturmak için her toplu etkinlik, bir saate kadar çalışabilir. Bu, en az 500.000 kullanıcı listesinin oluşturulmasını ve indirilmesini mümkün bir şekilde sunar.

## <a name="next-steps"></a>Sonraki adımlar

- [Toplu Kullanıcı ekleme](users-bulk-add.md)
- [Kullanıcıları toplu silme](users-bulk-delete.md)
- [Kullanıcıları toplu geri yükleme](users-bulk-restore.md)
