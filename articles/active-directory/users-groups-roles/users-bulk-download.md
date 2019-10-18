---
title: Azure Active Directory portalında Kullanıcı (Önizleme) listesini indirme | Microsoft Docs
description: Kullanıcı kayıtlarını Azure Yönetim Merkezi 'nde Azure Active Directory ' de toplu olarak indirin.
services: active-directory
author: curtand
ms.author: curtand
manager: mtillman
ms.date: 07/15/2019
ms.topic: conceptual
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: fd0829afca05058892d3a0ceeb50c9955d792dc3
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517068"
---
# <a name="download-a-list-of-users-preview-in-azure-active-directory-portal"></a>Azure Active Directory portalında kullanıcıların listesini indirme (Önizleme)

Azure Active Directory (Azure AD) toplu Kullanıcı içeri aktarma (oluşturma) işlemlerini destekler.

## <a name="required-permissions"></a>Gerekli izinler

Azure AD Yönetim merkezinden Kullanıcı listesini indirmek için Azure AD 'de kuruluş düzeyindeki bir veya daha fazla yönetici rolüne atanmış bir kullanıcı ile oturum açmanız gerekir. Konuk davetci ve uygulama geliştiricisi yönetici rolleri olarak değerlendirilmez.

## <a name="to-download-a-list-of-users"></a>Kullanıcı listesini indirmek için

1. Kuruluştaki bir Kullanıcı Yöneticisi hesabıyla [Azure AD kuruluşunuzda oturum açın](https://aad.portal.azure.com) .
1. Azure AD 'de **kullanıcılar** > **kullanıcıları indir**' i seçin.
1. **Kullanıcıları indir** sayfasında, Kullanıcı profili özelliklerini listelemek üzere bir CSV dosyası almak için **Başlat** ' ı seçin. Hatalar varsa, sonuçlar dosyasını toplu işlem sonuçları sayfasında indirebilir ve görüntüleyebilirsiniz. Dosya her hatanın nedenini içerir.

   ![Listeden indirmek istediğiniz kullanıcıları seçin](./media/users-bulk-download/bulk-download.png)

   İndirme dosyası, filtrelenmiş Kullanıcı listesini içerecektir.

   Aşağıdaki Kullanıcı öznitelikleri dahil edilecek: 

   - userPrincipalName
   - DisplayName
   - Soyadı
   - -
   - GivenName
   - Uzantının
   - userType
   - JobTitle
   - Bölüme
   - Manager
   - AccountEnabled
   - usageLocation
   - streetAddress
   - durum
   - Ülke
   - physicalDeliveryOfficeName
   - city
   - PostalCode
   - TelephoneNumber 'dır
   - Mo
   - authenticationPhoneNumber
   - authenticationAlternativePhoneNumber
   - authenticationEmail
   - Alternateemaadresi
   - Yaş
   - consentProvidedForMinor
   - Ligalagegroupclassification

## <a name="check-status"></a>Durumu kontrol etme

Bekleyen toplu isteklerinizin durumunu **toplu işlem sonuçları (Önizleme)** sayfasında görebilirsiniz.

   ![Toplu Işlemler sonuçları sayfasında karşıya yükleme durumunu kontrol edin](./media/users-bulk-download/bulk-center.png)

## <a name="bulk-download-service-limits"></a>Hizmet sınırlarını toplu indirme

Kullanıcıların bir listesini oluşturmak için her toplu etkinlik, bir saate kadar çalışabilir. Bu, en az 500.000 kullanıcı listesinin oluşturulmasını ve indirilmesini mümkün bir şekilde sunar.

## <a name="next-steps"></a>Sonraki adımlar

- [Toplu Kullanıcı ekleme](users-bulk-add.md)
- [Kullanıcıları toplu silme](users-bulk-delete.md)
- [Kullanıcıları toplu geri yükleme](users-bulk-restore.md)
