---
title: Azure Etkin Dizin portalında kullanıcıları toplu silme (önizleme) | Microsoft Dokümanlar
description: Azure Etkin Dizini'ndeki Azure yönetici merkezinde kullanıcıları toplu olarak silme
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
ms.openlocfilehash: d7c47887c12c8bf9be7a0c5b11dfb3f099965cb7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72174366"
---
# <a name="bulk-delete-users-preview-in-azure-active-directory"></a>Azure Etkin Dizini'nde kullanıcıları toplu silme (önizleme)

Azure Etkin Dizin (Azure AD) portalını kullanarak, kullanıcıları toplu olarak silmek için virgülle ayrılmış değerler (CSV) dosyasını kullanarak çok sayıda üyeyi bir gruba kaldırabilirsiniz.

## <a name="to-bulk-delete-users"></a>Kullanıcıları toplu olarak silmek için

1. [Azure REKLAM kuruluşunuzda,](https://aad.portal.azure.com) kuruluşta Kullanıcı yöneticisi olan bir hesapla oturum açın.
1. Azure AD'de, **Kullanıcılar** > **Toplu Silme'yi**seçin.
1. Toplu **silme kullanıcı** sayfasında, kullanıcı özelliklerinden oluşan geçerli bir CSV dosyasını almak için **İndir'i** seçin.

   ![Silmek istediğiniz kullanıcıları listelediğiniz yerel bir CSV dosyasını seçin](./media/users-bulk-delete/bulk-delete.png)

1. CSV dosyasını açın ve silmek istediğiniz her kullanıcı için bir satır ekleyin. Gerekli tek değer **Kullanıcı ana adıdır.** Ardından dosyayı kaydedin.

   ![CSV dosyası, silmek için kullanıcıların adlarını ve adlarını içerir](./media/users-bulk-delete/delete-csv-file.png)

1. Toplu **silme kullanıcı (Önizleme)** sayfasında, **csv dosyanızı yükleyin**altında, dosyaya göz atın. Dosyayı seçip gönder'i tıklattığınızda, CSV dosyasının doğrulaması başlar.
1. Dosya içeriği doğrulandığında, **Dosyanın başarıyla yüklendiğini**görürsünüz. Hatalar varsa, işi göndermeden önce bunları düzeltmeniz gerekir.
1. Dosyanız doğrulamadan geçtiğinde, kullanıcıları silen Azure toplu işlemini başlatmak için **Gönder'i** seçin.
1. Silme işlemi tamamlandığında, toplu işlemin başarılı olduğuna dair bir bildirim görürsünüz.

Hatalar varsa, **Toplu işlem sonuçları** sayfasındaki sonuç dosyasını indirebilir ve görüntüleyebilirsiniz. Dosya, her hatanın nedenini içerir.

## <a name="check-status"></a>Durumu kontrol etme

**Toplu işlem sonuçları (önizleme)** sayfasında bekleyen toplu isteklerinizin durumunu görebilirsiniz.

   ![Toplu İşlemsonuçları sayfasında yükleme durumunu kontrol edin](./media/users-bulk-delete/bulk-center.png)

Ardından, sildiğiniz kullanıcıların Azure portalında veya PowerShell'i kullanarak Azure REKLAM kuruluşunda bulununp var olmadığını denetleyebilirsiniz.

## <a name="verify-deleted-users-in-the-azure-portal"></a>Azure portalında silinen kullanıcıları doğrulama

1. Kuruluşta Kullanıcı yöneticisi olan bir hesapla Azure portalında oturum açın.
1. Gezinti bölmesinde Azure **Etkin Dizin'i**seçin.
1. **Yönet** bölümünde **Kullanıcılar**’ı seçin.
1. **Göster'in**altında, **yalnızca Tüm kullanıcıları** seçin ve sildiğiniz kullanıcıların artık listede olmadığını doğrulayın.

### <a name="verify-deleted-users-with-powershell"></a>PowerShell ile silinen kullanıcıları doğrulama

Şu komutu çalıştırın:

``` PowerShell
Get-AzureADUser -Filter "UserType eq 'Member'"
```

Sildiğiniz kullanıcıların artık listede olmadığını doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Toplu ekleme kullanıcıları](users-bulk-add.md)
- [Kullanıcı listesini indirin](users-bulk-download.md)
- [Toplu geri yükleme kullanıcıları](users-bulk-restore.md)
