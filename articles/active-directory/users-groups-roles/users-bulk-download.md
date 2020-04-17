---
title: Azure Active Directory portalındaki kullanıcıların listesini indirin | Microsoft Dokümanlar
description: Azure Active Directory'deki Azure yönetici merkezinde kullanıcı kayıtlarını toplu olarak indirin.
services: active-directory
author: curtand
ms.author: curtand
manager: daveba
ms.date: 04/16/2020
ms.topic: conceptual
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: krbain
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4b35163387ed4ce71f7a2019835a1d9fdbff3051
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81532670"
---
# <a name="download-a-list-of-users-in-azure-active-directory-portal"></a>Azure Active Directory portalındaki kullanıcıların listesini indirin

Azure Etkin Dizin (Azure AD), toplu kullanıcı alma (oluşturma) işlemlerini destekler.

## <a name="required-permissions"></a>Gerekli izinler

Kullanıcıların listesini Azure AD yönetici merkezinden indirmek için, Azure AD'de bir veya daha fazla kuruluş düzeyinde yönetici rolüne atanmış bir kullanıcıyla oturum açmış olmanız gerekir (Kullanıcı Yöneticisi gereken minimum roldür). Konuk davetçi ve uygulama geliştiricisi yönetici rolleri olarak kabul edilmez.

## <a name="to-download-a-list-of-users"></a>Kullanıcı listesini indirmek için

1. Kuruluştaki bir Kullanıcı yöneticisi hesabıyla [Azure REKLAM kuruluşunuzda oturum açın.](https://aad.portal.azure.com)
2. Azure Etkin Dizini > Kullanıcılar'a gidin. Ardından, her kullanıcının yanındaki sol sütundaki kutuyu işaretleyerek indirmeye eklemek istediğiniz kullanıcıları seçin. Not: Şu anda, dışa aktarma için tüm kullanıcıları seçmenin bir yolu yoktur. Her biri ayrı ayrı seçilmelidir.
3. Azure AD'de, **Kullanıcıları** > **İndir kullanıcılarını**seçin.
4. Kullanıcıları **İndir** sayfasında, kullanıcı profili özelliklerini listeleyen bir CSV dosyaalmak için **Başlat'ı** seçin. Hatalar varsa, Toplu işlem sonuçları sayfasındaki sonuç dosyasını indirebilir ve görüntüleyebilirsiniz. Dosya, her hatanın nedenini içerir.

   ![İndirmek istediğiniz kullanıcıların listesini istediğiniz yeri seçin](./media/users-bulk-download/bulk-download.png)

   İndirme dosyası, filtre uygulanmış kullanıcı listesini içerir.

   Aşağıdaki kullanıcı öznitelikleri dahildir:

   - userPrincipalName
   - displayName
   - surname
   - posta
   - givenName
   - Objectıd
   - Usertype
   - jobTitle
   - bölüm
   - hesapEtkin
   - kullanımKonum
   - Streetaddress
   - durum
   - ülke
   - physicalDeliveryOfficeName
   - city
   - Postakodu
   - Telephonenumber
   - mobil
   - kimlik doğrulamaTelefon Numarası
   - kimlik doğrulamaAlternativePhoneNumber
   - kimlik doğrulamaEmail
   - alternatifEmailAddress
   - ageGroup
   - rızaProvidedForMinor
   - yasalAgeGroupClassification

## <a name="check-status"></a>Durumu kontrol etme

Bekleyen toplu isteklerinizin durumunu Toplu **işlem sonuçları** sayfasında görebilirsiniz.

[![](media/users-bulk-download/bulk-center.png "Check status in the Bulk Operations Results page")](media/users-bulk-download/bulk-center.png#lightbox)

## <a name="bulk-download-service-limits"></a>Toplu indirme hizmeti sınırları

Kullanıcıların listesini oluşturmak için her toplu etkinlik bir saate kadar çalıştırılabilir. Bu, en az 500.000 kullanıcıdan oluşan bir listenin oluşturulmasını ve indirilmesini sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- [Toplu ekleme kullanıcıları](users-bulk-add.md)
- [Toplu silme kullanıcıları](users-bulk-delete.md)
- [Toplu geri yükleme kullanıcıları](users-bulk-restore.md)
