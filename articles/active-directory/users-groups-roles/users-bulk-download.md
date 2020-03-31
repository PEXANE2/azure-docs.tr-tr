---
title: Azure Active Directory portalında kullanıcıların listesini (önizleme) indirin | Microsoft Dokümanlar
description: Azure Active Directory'deki Azure yönetici merkezinde kullanıcı kayıtlarını toplu olarak indirin.
services: active-directory
author: curtand
ms.author: curtand
manager: daveba
ms.date: 02/06/2020
ms.topic: conceptual
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: krbain
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4716ff9547f64dc6551b4d4adb0a8578da9fa83e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063841"
---
# <a name="download-a-list-of-users-preview-in-azure-active-directory-portal"></a>Azure Active Directory portalında kullanıcıların listesini (önizleme) indirin

Azure Etkin Dizin (Azure AD), toplu kullanıcı alma (oluşturma) işlemlerini destekler.

## <a name="required-permissions"></a>Gerekli izinler

Kullanıcı listesini Azure AD yönetici merkezinden indirmek için, Azure AD'deki bir veya daha fazla kuruluş düzeyinde yönetici rolüne atanmış bir kullanıcıyla oturum açmış olmanız gerekir. Konuk davetçi ve uygulama geliştiricisi yönetici rolleri olarak kabul edilmez.

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

Bekleyen toplu isteklerinizin durumunu Toplu **işlem sonuçları (önizleme)** sayfasında görebilirsiniz.

   ![Toplu İşlemsonuçları sayfasında yükleme durumunu kontrol edin](./media/users-bulk-download/bulk-center.png)

## <a name="bulk-download-service-limits"></a>Toplu indirme hizmeti sınırları

Kullanıcıların listesini oluşturmak için her toplu etkinlik bir saate kadar çalıştırılabilir. Bu, en az 500.000 kullanıcıdan oluşan bir listenin oluşturulmasını ve indirilmesini sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- [Toplu ekleme kullanıcıları](users-bulk-add.md)
- [Toplu silme kullanıcıları](users-bulk-delete.md)
- [Toplu geri yükleme kullanıcıları](users-bulk-restore.md)
