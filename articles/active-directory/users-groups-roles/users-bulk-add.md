---
title: Azure Active Directory portalında toplu Kullanıcı oluşturma (Önizleme) | Microsoft Docs
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
ms.openlocfilehash: 87c62cbe71f2e02c6f2c09620a8470a97ae57392
ms.sourcegitcommit: b03516d245c90bca8ffac59eb1db522a098fb5e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71146303"
---
# <a name="bulk-create-users-preview-in-azure-active-directory"></a>Azure Active Directory 'da toplu Kullanıcı oluşturma (Önizleme)

Azure Active Directory (Azure AD) toplu Kullanıcı oluşturma ve silme işlemlerini, konuklara toplu davet etmeyi ve kullanıcıların, grupların ve grup üyelerinin listesini indirmeyi destekler.

## <a name="bulk-import-service-limits"></a>Toplu içeri aktarma hizmeti sınırları

Kullanıcıları oluşturmak için her toplu etkinlik, bir saate kadar çalıştırılabilir. Bu, en az 50.000 kullanıcının toplu oluşturulmasına izin verebilir.

## <a name="required-permissions"></a>Gerekli izinler

Yönetim Portalı 'nda kullanıcıları toplu olarak oluşturmak için, genel yönetici veya Kullanıcı Yöneticisi olarak oturum açmış olmanız gerekir.

## <a name="to-bulk-import-users"></a>Kullanıcıları toplu olarak içeri aktarmak için

1. Kuruluşunuzda Kullanıcı Yöneticisi olan bir hesapla [Azure AD kuruluşunuzda oturum açın](https://aad.portal.azure.com) .
1. Azure AD 'de **Kullanıcılar** > **toplu oluştur**' u seçin.
1. **Toplu kullanıcı oluştur** sayfasında, kullanıcı özelliklerinin geçerli bir virgülle ayrılmış değerler (CSV) dosyasını almak için **İndir** ' i seçin ve ardından yeni kullanıcılarınızı ekleyin.

   ![CSV dosyası oluşturulacak kullanıcıların adlarını ve kimliklerini içerir](./media/users-bulk-add/add-csv-file.png)

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
