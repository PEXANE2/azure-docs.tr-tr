---
title: Azure Active Directory portalına Kullanıcı eklemek için toplu alma (Önizleme) | Microsoft Docs
description: Azure Active Directory 'de Azure AD Yönetim Merkezi 'nde toplu olarak Kullanıcı ekleme
services: active-directory
author: curtand
ms.author: curtand
manager: mtillman
ms.date: 08/30/2019
ms.topic: article
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: a22052117448cdb71bbc16e1df7899e6d3eb764e
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70901496"
---
# <a name="bulk-import-users-preview-in-azure-active-directory"></a>Azure Active Directory 'da Toplu içe Kullanıcı (Önizleme)

Azure Active Directory (Azure AD) toplu Kullanıcı oluşturma ve silme işlemlerini, konuklara toplu davet etmeyi ve kullanıcıların, grupların ve grup üyelerinin listesini indirmeyi destekler.

## <a name="bulk-import-service-limits"></a>Toplu içeri aktarma hizmeti sınırları

Kullanıcıları oluşturmak için her toplu etkinlik, bir saate kadar çalıştırılabilir. Bu, en az 50.000 kullanıcının toplu oluşturulmasına izin verebilir.

## <a name="required-permissions"></a>Gerekli izinler

Yönetim Portalı 'nda kullanıcıları toplu olarak oluşturmak için, genel yönetici veya Kullanıcı Yöneticisi olarak oturum açmış olmanız gerekir.

## <a name="to-bulk-import-users"></a>Kullanıcıları toplu olarak içeri aktarmak için

1. Kuruluşunuzda Kullanıcı Yöneticisi olan bir hesapla [Azure AD kuruluşunuzda oturum açın](https://aad.portal.azure.com) .
1. Azure AD 'de **Kullanıcılar** > **toplu oluştur**' u seçin.
1. **Toplu kullanıcı oluştur** sayfasında, kullanıcı özelliklerinin geçerli bir virgülle ayrılmış değerler (CSV) dosyasını almak için **İndir** ' i seçin ve ardından yeni kullanıcılarınızı ekleyin.
1. CSV dosyasını düzenlemesini bitirdiğinizde veya kendi karşıya yüklemeye hazırsanız, **CSV dosyanızı karşıya** yükleme ' nin altındaki dosyayı seçin.

   ![İçinde eklemek istediğiniz kullanıcıları listeettiğiniz yerel bir CSV dosyası seçin](./media/users-bulk-add/upload-button.png)

1. Dosya içeriği doğrulandığında, karşıya yükleme işini başlatabilmeniz için önce tüm hataları çözmeniz gerekir.
1. Dosyanız doğrulamayı geçtiğinde, Yeni Kullanıcı bilgilerini ekleyen Azure Batch işini başlatmak için **Gönder** ' i seçin. Hatalar varsa, sonuçlar dosyasını toplu işlem sonuçları sayfasında indirebilir ve görüntüleyebilirsiniz. Dosya her hatanın nedenini içerir.

## <a name="check-status"></a>Durumu kontrol etme

Tüm bekleyen toplu isteklerinizin durumunu **toplu işlem sonuçları (Önizleme)** sayfasında görebilirsiniz.

   ![Toplu Işlemler sonuçları sayfasında karşıya yükleme durumunu kontrol edin](./media/users-bulk-add/bulk-center.png)

Daha sonra, oluşturduğunuz kullanıcıların Azure portal veya PowerShell kullanarak Azure AD kuruluşunda mevcut olup olmadığını kontrol edebilirsiniz.

## <a name="verify-users-in-the-azure-portal"></a>Azure portal kullanıcıları doğrulama

1. Kuruluşunuzda Kullanıcı Yöneticisi olan bir hesapla [Azure AD Yönetim merkezinde oturum açın](https://aad.portal.azure.com) .
1. Gezinti bölmesinde **Azure Active Directory**' yi seçin.
1. **Yönet** bölümünde **Kullanıcılar**’ı seçin.
1. **Göster**altında, **tüm kullanıcılar** ' ı seçin ve oluşturduğunuz kullanıcıların listelendiğini doğrulayın.

### <a name="verify-users-with-powershell"></a>Kullanıcıları PowerShell ile doğrulama

Şu komutu çalıştırın:

``` PowerShell
Get-AzureADUser -Filter "UserType eq 'Member'"
```

Oluşturduğunuz kullanıcıların listelendiğini görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Kullanıcıları toplu silme](users-bulk-delete.md)
- [Kullanıcı listesini indir](users-bulk-download.md)
- [Kullanıcıları toplu geri yükleme](users-bulk-restore.md)
