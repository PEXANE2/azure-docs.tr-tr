---
title: Azure Active Directory portalında silinen kullanıcıları toplu olarak geri yükleme (önizleme) | Microsoft Dokümanlar
description: Azure Etkin Dizini'ndeki Azure AD yönetici merkezinde silinen kullanıcıları toplu olarak geri yükleme
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
ms.openlocfilehash: d392ae97a8325dd4a56acd807ebfb2b951216eae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72174235"
---
# <a name="bulk-restore-deleted-users-preview-in-azure-active-directory"></a>Azure Etkin Dizini'nde silinen kullanıcıları toplu olarak geri yükleme (önizleme)

Azure Etkin Dizin (Azure AD), toplu kullanıcı oluşturma ve silme işlemlerini, misafirler için toplu daveti ve kullanıcı, grup ve grup üyelerinin indirme listelerini destekler.

## <a name="to-bulk-restore-users"></a>Kullanıcıları toplu olarak geri yüklemek için

1. Azure AD kuruluşunuzdaki Kullanıcı yöneticisi bir hesapla [Azure REKLAM kuruluşunuzda oturum açın.](https://aad.portal.azure.com)
1. Azure AD'de,**Silinen** **Kullanıcılar'ı** > seçin.
1. **Silinen kullanıcılar** sayfasında, kullanıcıların özelliklerinin yüklenmesi için geçerli bir CSV dosyasını yüklemek için **Toplu geri yükleme'yi** seçin.

   ![Silinen kullanıcılar sayfasındaki toplu geri yükleme komutunu seçin](./media/users-bulk-restore/bulk-restore.png)

1. CSV dosyasını açın ve geri yüklemek istediğiniz her kullanıcı için bir satır ekleyin. Gerekli tek değer **ObjectID'dir.** Ardından dosyayı kaydedin.

   ![Eklemek istediğiniz kullanıcıları listelediğiniz yerel bir CSV dosyasını seçin](./media/users-bulk-restore/upload-button.png)

1. Toplu **geri yükleme (Önizleme)** sayfasında, **csv dosyanızı yükleyin**altında, dosyaya göz atın. Dosyayı seçip **Gönder'i**tıklattığınızda, CSV dosyasının doğrulaması başlar.
1. Dosya içeriği doğrulandığında, **Dosyanın başarıyla yüklendiğini**görürsünüz. Hatalar varsa, işi göndermeden önce bunları düzeltmeniz gerekir.
1. Dosyanız doğrulamadan geçtiğinde, kullanıcıları geri yükleyen Azure toplu işlemini başlatmak için **Gönder'i** seçin.
1. Geri yükleme işlemi tamamlandığında, toplu işlemin başarılı olduğuna dair bir bildirim görürsünüz.

Hatalar varsa, **Toplu işlem sonuçları** sayfasındaki sonuç dosyasını indirebilir ve görüntüleyebilirsiniz. Dosya, her hatanın nedenini içerir.

## <a name="check-status"></a>Durumu kontrol etme

**Toplu işlem sonuçları (önizleme)** sayfasında bekleyen toplu isteklerinizin durumunu görebilirsiniz.

   ![Toplu İşlemsonuçları sayfasında yükleme durumunu kontrol edin](./media/users-bulk-restore/bulk-center.png)

Ardından, geri yüklediğiniz kullanıcıların Azure portalında veya PowerShell'i kullanarak Azure REKLAM kuruluşunda bulununp var olmadığını denetleyebilirsiniz.

## <a name="view-restored-users-in-the-azure-portal"></a>Azure portalında geri yüklenen kullanıcıları görüntüleme

1. Kuruluşta Kullanıcı yöneticisi olan bir hesapla [Azure AD yönetici merkezinde oturum açın.](https://aad.portal.azure.com)
1. Gezinti bölmesinde Azure **Etkin Dizin'i**seçin.
1. **Yönet** bölümünde **Kullanıcılar**’ı seçin.
1. **Göster'in**altında, **Tüm kullanıcıları** seçin ve geri yüklediğiniz kullanıcıların listelenmiş olduğunu doğrulayın.

### <a name="view-users-with-powershell"></a>PowerShell ile kullanıcıları görüntüleyin

Şu komutu çalıştırın:

``` PowerShell
Get-AzureADUser -Filter "UserType eq 'Member'"
```

Geri yüklediğiniz kullanıcıların listelenmiş olduğunu görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Toplu içe aktarma kullanıcıları](users-bulk-add.md)
- [Toplu silme kullanıcıları](users-bulk-delete.md)
- [Kullanıcı listesini indirin](users-bulk-download.md)
