---
title: Azure Active Directory portalında toplu oluşturma kullanıcıları (önizleme) | Microsoft Dokümanlar
description: Azure Etkin Dizin'deki Azure AD yönetici merkezinde kullanıcıları toplu olarak ekleme
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
ms.openlocfilehash: a10dfffa69652ee2b75053c04b97f6492c46811e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72174339"
---
# <a name="bulk-create-users-preview-in-azure-active-directory"></a>Azure Etkin Dizini'nde toplu oluşturma kullanıcıları (önizleme)

Azure Etkin Dizin (Azure AD), toplu kullanıcı oluşturma ve silme işlemlerini, misafirler için toplu daveti ve kullanıcı, grup ve grup üyelerinin indirme listelerini destekler.

## <a name="required-permissions"></a>Gerekli izinler

Yönetim portalında toplu olarak kullanıcı oluşturmak için, Global yönetici veya Kullanıcı yöneticisi olarak oturum açmış olmanız gerekir.

## <a name="to-create-users-in-bulk"></a>Toplu olarak kullanıcı oluşturmak için

1. [Azure REKLAM kuruluşunuzda,](https://aad.portal.azure.com) kuruluşta Kullanıcı yöneticisi olan bir hesapla oturum açın.
1. Azure AD'de, **Kullanıcılar** > **Toplu oluşturma'yı**seçin.
1. Toplu **oluşturma kullanıcı** sayfasında, kullanıcı özelliklerinden oluşan geçerli bir virgülle ayrılmış değerler (CSV) dosyasını almak için **İndir'i** seçin ve ardından oluşturmak istediğiniz kullanıcıları ekle ekleyin.

   ![Eklemek istediğiniz kullanıcıları listelediğiniz yerel bir CSV dosyasını seçin](./media/users-bulk-add/upload-button.png)

1. CSV dosyasını açın ve oluşturmak istediğiniz her kullanıcı için bir satır ekleyin. Gerekli olan tek değerler **Ad,** **Kullanıcı ana adı,** **İlk parola** ve Blok oturum **açma (Evet/Hayır)**'dir. Ardından dosyayı kaydedin.

   ![CSV dosyası oluşturmak için kullanıcıların adlarını ve adlarını içerir](./media/users-bulk-add/add-csv-file.png)

1. Toplu **oluşturma kullanıcı (Önizleme)** sayfasında, CSV dosyanızı yükleyin altında, dosyaya göz atın. Dosyayı seçip **Gönder'i**tıklattığınızda, CSV dosyasının doğrulaması başlar.
1. Dosya içeriği doğrulandıktan sonra **Dosyanın başarıyla yüklendiğini**görürsünüz. Hatalar varsa, işi göndermeden önce bunları düzeltmeniz gerekir.
1. Dosyanız doğrulamadan geçtiğinde, yeni kullanıcıları içeri alan Azure toplu işlemini başlatmak için **Gönder'i** seçin.
1. Alma işlemi tamamlandığında, toplu işlem iş durumu bildirimi ni görürsünüz.

Hatalar varsa, **Toplu işlem sonuçları** sayfasındaki sonuç dosyasını indirebilir ve görüntüleyebilirsiniz. Dosya, her hatanın nedenini içerir.

## <a name="check-status"></a>Durumu kontrol etme

**Toplu işlem sonuçları (önizleme)** sayfasında bekleyen toplu isteklerinizin durumunu görebilirsiniz.

   ![Toplu İşlemsonuçları sayfasında yükleme durumunu kontrol edin](./media/users-bulk-add/bulk-center.png)

Ardından, oluşturduğunuz kullanıcıların Azure portalında veya PowerShell'i kullanarak Azure REKLAM kuruluşunda bulununp var olmadığını denetleyebilirsiniz.

## <a name="verify-users-in-the-azure-portal"></a>Azure portalındaki kullanıcıları doğrulama

1. Kuruluşta Kullanıcı yöneticisi olan bir hesapla [Azure AD yönetici merkezinde oturum açın.](https://aad.portal.azure.com)
1. Gezinti bölmesinde Azure **Etkin Dizin'i**seçin.
1. **Yönet** bölümünde **Kullanıcılar**’ı seçin.
1. **Göster'in**altında, **Tüm kullanıcıları** seçin ve oluşturduğunuz kullanıcıların listelenmiş olduğunu doğrulayın.

### <a name="verify-users-with-powershell"></a>PowerShell ile kullanıcıları doğrulayın

Şu komutu çalıştırın:

``` PowerShell
Get-AzureADUser -Filter "UserType eq 'Member'"
```

Oluşturduğunuz kullanıcıların listelenmiş olduğunu görmeniz gerekir.

## <a name="bulk-import-service-limits"></a>Toplu alma hizmet limitleri

Kullanıcıları oluşturmak için her toplu etkinlik bir saate kadar çalıştırılabilir. Bu, en az 50.000 kullanıcının toplu oluşturulmasını sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- [Toplu silme kullanıcıları](users-bulk-delete.md)
- [Kullanıcı listesini indirin](users-bulk-download.md)
- [Toplu geri yükleme kullanıcıları](users-bulk-restore.md)
