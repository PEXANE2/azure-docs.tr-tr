---
title: Azure Active Directory portalında kullanıcıları toplu silme (Önizleme) | Microsoft Docs
description: Azure Active Directory Azure Yönetim merkezinde toplu olarak kullanıcıları silme
services: active-directory
author: curtand
ms.author: curtand
manager: mtillman
ms.date: 08/15/2019
ms.topic: conceptual
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6c2204c572ca1f74f8060d0b6176df69359fe69a
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70901717"
---
# <a name="bulk-delete-users-preview-in-azure-active-directory"></a>Azure Active Directory 'da kullanıcıları toplu silme (Önizleme)

Azure Active Directory (Azure AD) toplu Kullanıcı oluşturma ve silme işlemlerini, konuklara toplu davet etmeyi ve kullanıcıların, grupların ve grup üyelerinin listesini indirmeyi destekler.

## <a name="to-bulk-delete-users"></a>Kullanıcıları toplu olarak silmek için

1. Kuruluşunuzda Kullanıcı Yöneticisi olan bir hesapla Azure AD kuruluşunuzda oturum açın.
1. Azure AD 'de **Kullanıcılar** > **toplu silme**' yi seçin.
1. **Toplu Kullanıcı silme** sayfasında, kullanıcı özelliklerinin GEÇERLI bir CSV dosyasını almak için **İndir** ' i seçin ve ardından silmek istediğiniz kullanıcıları ekleyin.
1. CSV dosyasını düzenlemesini bitirdiğinizde veya kendi karşıya yüklemeye hazırsanız, **CSV dosyanızı karşıya** yükleme ' nin altındaki dosyayı seçin.

   ![Silmek istediğiniz kullanıcıları listeettiğiniz yerel bir CSV dosyası seçin](./media/users-bulk-delete/bulk-delete.png)

1. Dosya içeriği doğrulandığında, iş gönderilmeden önce herhangi bir hata düzeltilmelidir.
1. Dosyanız doğrulamayı geçtiğinde, kullanıcıları silen Azure Batch işini başlatmak için **Gönder** ' i seçin. Hatalar varsa, sonuçlar dosyasını toplu işlem sonuçları sayfasında indirebilir ve görüntüleyebilirsiniz. Dosya her hatanın nedenini içerir.

## <a name="check-status"></a>Durumu kontrol etme

Tüm bekleyen toplu isteklerinizin durumunu **toplu işlem sonuçları (Önizleme)** sayfasında görebilirsiniz.

   ![Toplu Işlemler sonuçları sayfasında karşıya yükleme durumunu kontrol edin](./media/users-bulk-delete/bulk-center.png)

Bundan sonra, sildiğiniz kullanıcıların Azure AD kuruluşunda Azure portal veya PowerShell kullanarak var olup olmadığını kontrol edebilirsiniz.

## <a name="verify-deleted-users-in-the-azure-portal"></a>Azure portal silinen kullanıcıları doğrulama

1. Kuruluşta Kullanıcı Yöneticisi olan bir hesapla Azure portal oturum açın.
1. Gezinti bölmesinde **Azure Active Directory**' yi seçin.
1. **Yönet** bölümünde **Kullanıcılar**’ı seçin.
1. **Göster**altında yalnızca **tüm kullanıcılar** ' ı seçin ve sildiğiniz kullanıcıların artık listelenmediğini doğrulayın.

### <a name="verify-deleted-users-with-powershell"></a>Silinen kullanıcıları PowerShell ile doğrulama

Şu komutu çalıştırın:

``` PowerShell
Get-AzureADUser -Filter "UserType eq 'Member'"
```

Sildiğiniz kullanıcıların artık listelenmediğini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Toplu Kullanıcı ekleme](users-bulk-add.md)
- [Kullanıcı listesini indir](users-bulk-download.md)
- [Kullanıcıları toplu geri yükleme](users-bulk-restore.md)
