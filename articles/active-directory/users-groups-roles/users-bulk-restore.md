---
title: Azure Active Directory portalında silinen kullanıcıları (Önizleme) toplu geri yükleme | Microsoft Docs
description: Azure Active Directory 'de Azure AD Yönetim Merkezi 'nde toplu olarak silinen kullanıcıları geri yükleme
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
ms.openlocfilehash: f53ade09c5e2e7db0499122526a1de482af9378f
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70901626"
---
# <a name="bulk-restore-deleted-users-preview-in-azure-active-directory"></a>Azure Active Directory silinen kullanıcıları toplu geri yükleme (Önizleme)

Azure Active Directory (Azure AD) toplu Kullanıcı oluşturma ve silme işlemlerini, konuklara toplu davet etmeyi ve kullanıcıların, grupların ve grup üyelerinin listesini indirmeyi destekler.

## <a name="to-bulk-restore-users"></a>Kullanıcıları toplu geri yüklemek için

1. Kuruluşunuzda Kullanıcı Yöneticisi olan bir hesapla [Azure AD kuruluşunuzda oturum açın](https://aad.portal.azure.com) .
1. Azure AD 'de,**silinen** **Kullanıcılar** > ' ı seçin.
1. **Silinen kullanıcılar** sayfasında, kullanıcıların geri yüklenecek GEÇERLI bir CSV dosyasını karşıya yüklemek için **toplu geri yükleme** ' yi seçin.

   ![Silinen kullanıcılar sayfasında toplu geri yükleme komutunu seçin](./media/users-bulk-restore/bulk-restore.png)

1. CSV dosyasını düzenlemesini bitirdiğinizde veya kendi karşıya yüklemeye hazırsanız, **CSV dosyanızı karşıya** yükleme ' nin altındaki dosyayı seçin.

   ![İçinde eklemek istediğiniz kullanıcıları listeettiğiniz yerel bir CSV dosyası seçin](./media/users-bulk-restore/upload-button.png)

1. Dosya içeriği doğrulandığında, dosya bilgilerini düzeltemedi ve hatalar varsa dosyayı yeniden gönderebilirsiniz. Geçerli bir dosyanın gönderilmesi, veri yükleme işini otomatik olarak başlatır.
1. CSV dosyası doğrulamayı geçtikten sonra, kullanıcıları geri yükleyen Azure Batch işini başlatmak için **Gönder** ' i seçin. Hatalar varsa, sonuçlar dosyasını toplu işlem sonuçları sayfasında indirebilir ve görüntüleyebilirsiniz. Dosya her hatanın nedenini içerir.

## <a name="check-status"></a>Durumu kontrol etme

Tüm bekleyen toplu isteklerinizin durumunu **toplu işlem sonuçları (Önizleme)** sayfasında görebilirsiniz.

   ![Toplu Işlemler sonuçları sayfasında karşıya yükleme durumunu kontrol edin](./media/users-bulk-restore/bulk-center.png)

Daha sonra, geri yüklediğiniz kullanıcıların Azure portal veya PowerShell kullanarak Azure AD kuruluşunda mevcut olup olmadığını kontrol edebilirsiniz.

## <a name="view-restored-users-in-the-azure-portal"></a>Azure portal geri yüklenen kullanıcıları görüntüleme

1. Kuruluşunuzda Kullanıcı Yöneticisi olan bir hesapla [Azure AD Yönetim merkezinde oturum açın](https://aad.portal.azure.com) .
1. Gezinti bölmesinde **Azure Active Directory**' yi seçin.
1. **Yönet** bölümünde **Kullanıcılar**’ı seçin.
1. **Göster**altında, **tüm kullanıcılar** ' ı seçin ve geri yüklediğiniz kullanıcıların listelendiğini doğrulayın.

### <a name="view-users-with-powershell"></a>PowerShell ile kullanıcıları görüntüleme

Şu komutu çalıştırın:

``` PowerShell
Get-AzureADUser -Filter "UserType eq 'Member'"
```

Geri yüklediğiniz kullanıcıların listelendiğini görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Kullanıcıları toplu içe aktarma](users-bulk-add.md)
- [Kullanıcıları toplu silme](users-bulk-delete.md)
- [Kullanıcı listesini indir](users-bulk-download.md)
