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
ms.openlocfilehash: eb01b46d61b6ba99c3ec9c537dccc350074f5e05
ms.sourcegitcommit: b03516d245c90bca8ffac59eb1db522a098fb5e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71146419"
---
# <a name="bulk-delete-users-preview-in-azure-active-directory"></a>Azure Active Directory 'da kullanıcıları toplu silme (Önizleme)

Azure Active Directory (Azure AD) portalını kullanarak, kullanıcıları toplu olarak silmek için bir virgülle ayrılmış değerler (CSV) dosyası kullanarak çok sayıda üyeyi bir gruba kaldırabilirsiniz.

## <a name="to-bulk-delete-users"></a>Kullanıcıları toplu olarak silmek için

1. Kuruluşunuzda Kullanıcı Yöneticisi olan bir hesapla Azure AD kuruluşunuzda oturum açın.
1. Azure AD 'de **Kullanıcılar** > **toplu silme**' yi seçin.
1. **Toplu Kullanıcı silme** sayfasında, kullanıcı özelliklerinin GEÇERLI bir CSV dosyasını almak için **İndir** ' i seçin ve ardından silmek istediğiniz kullanıcıları ekleyin.

   ![CSV dosyası, silinecek kullanıcıların adlarını ve kimliklerini içerir](./media/users-bulk-delete/delete-csv-file.png)

1. CSV dosyasını düzenlemesini bitirdiğinizde, doğrulanacak **CSV dosyanızı karşıya yükleyin** altında dosyayı seçin.

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
