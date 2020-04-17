---
title: Azure Etkin Dizin portalında silinen kullanıcıları toplu olarak geri yükleme | Microsoft Dokümanlar
description: Azure Etkin Dizini'ndeki Azure AD yönetici merkezinde silinen kullanıcıları toplu olarak geri yükleme
services: active-directory
author: curtand
ms.author: curtand
manager: mtillman
ms.date: 04/16/2020
ms.topic: conceptual
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: f75fe224491c2853f819a45db678e87849dc72d1
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81532738"
---
# <a name="bulk-restore-deleted-users-in-azure-active-directory"></a>Azure Etkin Dizini'nde silinen kullanıcıları toplu olarak geri yükleme

Azure Etkin Dizin (Azure AD), toplu kullanıcı oluşturma ve silme işlemlerini, misafirler için toplu daveti ve kullanıcı, grup ve grup üyelerinin indirme listelerini destekler.

## <a name="to-bulk-restore-users"></a>Kullanıcıları toplu olarak geri yüklemek için

1. Azure AD kuruluşunuzdaki Kullanıcı yöneticisi bir hesapla [Azure REKLAM kuruluşunuzda oturum açın.](https://aad.portal.azure.com)
1. Azure AD'de,**Silinen** **Kullanıcılar'ı** > seçin.
1. **Silinen kullanıcılar** sayfasında, kullanıcıların özelliklerinin yüklenmesi için geçerli bir CSV dosyasını yüklemek için **Toplu geri yükleme'yi** seçin.

   ![Silinen kullanıcılar sayfasındaki toplu geri yükleme komutunu seçin](./media/users-bulk-restore/bulk-restore.png)

1. CSV dosyasını açın ve geri yüklemek istediğiniz her kullanıcı için bir satır ekleyin. Gerekli tek değer **ObjectID'dir.** Ardından dosyayı kaydedin.

   ![Eklemek istediğiniz kullanıcıları listelediğiniz yerel bir CSV dosyasını seçin](./media/users-bulk-restore/upload-button.png)

1. Toplu **geri yükleme** sayfasında, **csv dosyanızı yükleyin**altında, dosyaya göz atın. Dosyayı seçip **Gönder'i**tıklattığınızda, CSV dosyasının doğrulaması başlar.
1. Dosya içeriği doğrulandığında, **Dosyanın başarıyla yüklendiğini**görürsünüz. Hatalar varsa, işi göndermeden önce bunları düzeltmeniz gerekir.
1. Dosyanız doğrulamadan geçtiğinde, kullanıcıları geri yükleyen Azure toplu işlemini başlatmak için **Gönder'i** seçin.
1. Geri yükleme işlemi tamamlandığında, toplu işlemin başarılı olduğuna dair bir bildirim görürsünüz.

Hatalar varsa, **Toplu işlem sonuçları** sayfasındaki sonuç dosyasını indirebilir ve görüntüleyebilirsiniz. Dosya, her hatanın nedenini içerir.

## <a name="check-status"></a>Durumu kontrol etme

Bekleyen toplu isteklerinizin durumunu Toplu işlem **sonuçları** sayfasında görebilirsiniz.

[![](media/users-bulk-restore/bulk-center.png "Check status in the Bulk Operations Results page")](media/users-bulk-restore/bulk-center.png#lightbox)

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
