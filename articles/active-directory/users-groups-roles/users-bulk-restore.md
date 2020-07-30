---
title: Azure Active Directory portalında silinen kullanıcıları toplu geri yükleme | Microsoft Docs
description: Azure Active Directory 'de Azure AD Yönetim Merkezi 'nde toplu olarak silinen kullanıcıları geri yükleme
services: active-directory
author: curtand
ms.author: curtand
manager: mtillman
ms.date: 04/27/2020
ms.topic: how-to
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4fe5cacfec6ee85a5d61204ffce38c856b0d1baf
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87423346"
---
# <a name="bulk-restore-deleted-users-in-azure-active-directory"></a>Azure Active Directory silinen kullanıcıları toplu geri yükleme

Azure Active Directory (Azure AD) toplu Kullanıcı geri yükleme işlemlerini destekler ve Kullanıcı, Grup ve grup üyelerinin listesini indirmeyi destekler.

## <a name="understand-the-csv-template"></a>CSV şablonunu anlama

Azure AD kullanıcılarını toplu olarak başarıyla geri yüklemenize yardımcı olması için CSV şablonunu indirin ve girin. İndirmediğiniz CSV şablonu şu örnekteki gibi görünebilir:

![Her satır ve sütunun amacını ve değerlerini açıklayan karşıya yükleme ve çağrı aşımları için elektronik tablo](./media/users-bulk-restore/understand-template.png)

### <a name="csv-template-structure"></a>CSV şablonu yapısı

İndirilen bir CSV şablonundaki satırlar aşağıdaki gibidir:

- **Sürüm numarası**: sürüm numarasını içeren ilk satır, KARŞıYA yükleme CSV 'ye eklenmelidir.
- **Sütun başlıkları**: sütun başlıklarının biçimi &lt; *öğe adı* &gt; [PropertyName] &lt; *gerekli veya boş* &gt; . Örneğin, `Object ID [objectId] Required`. Şablonun bazı eski sürümlerinde hafif Çeşitlemeler bulunabilir.
- **Örnekler satırı**: şablona her sütun için kabul edilebilir değer örneklerinin bir satırını ekledik. Örnekler satırını kaldırmalı ve kendi girişlerinizin yerine değiştirmelisiniz.

### <a name="additional-guidance"></a>Ek yönergeler

- Karşıya yükleme şablonunun ilk iki satırı kaldırılmamalıdır veya değiştirilmemelidir veya karşıya yükleme işlenemiyor.
- Önce gerekli sütunlar listelenir.
- Şablona yeni sütun eklenmesini önermiyoruz. Eklediğiniz tüm ek sütunlar yoksayılır ve işlenmez.
- CSV şablonunun en son sürümünü mümkün olduğunca sık indirmeniz önerilir.

## <a name="to-bulk-restore-users"></a>Kullanıcıları toplu geri yüklemek için

1. Azure AD kuruluşunda Kullanıcı Yöneticisi olan bir hesapla [Azure AD kuruluşunuzda oturum açın](https://aad.portal.azure.com) .
1. Azure AD 'de, silinen **Kullanıcılar**' ı seçin  >  **Deleted**.
1. **Silinen kullanıcılar** sayfasında, kullanıcıların geri yüklenecek GEÇERLI bir CSV dosyasını karşıya yüklemek için **toplu geri yükleme** ' yi seçin.

    ![Silinen kullanıcılar sayfasında toplu geri yükleme komutunu seçin](./media/users-bulk-restore/bulk-restore.png)

1. CSV şablonunu açın ve geri yüklemek istediğiniz her kullanıcı için bir satır ekleyin. Yalnızca **ObjectID**değeri gereklidir. Ardından dosyayı kaydedin.

    :::image type="content" source="./media/users-bulk-restore/upload-button.png" alt-text="İçinde eklemek istediğiniz kullanıcıları listeettiğiniz yerel bir CSV dosyası seçin":::

1. **Toplu geri yükleme** sayfasında, **CSV dosyanızı karşıya yükleyin**bölümünde dosyaya gidin. Dosyayı seçip **Gönder**' e TıKLADıĞıNıZDA, CSV dosyasının doğrulanması başlar.
1. Dosya içeriği doğrulandığında, **dosyanın başarıyla karşıya yüklendiğini**görürsünüz. Hatalar varsa, işi gönderebilmeniz için önce bunları çözmeniz gerekir.
1. Dosyanız doğrulamayı geçtiğinde, kullanıcıları geri yükleyen Azure toplu işlemini başlatmak için **Gönder** ' i seçin.
1. Geri yükleme işlemi tamamlandığında toplu işlemin başarılı olduğunu belirten bir bildirim görürsünüz.

Hatalar varsa, sonuçlar dosyasını **toplu işlem sonuçları** sayfasında indirebilir ve görüntüleyebilirsiniz. Dosya her hatanın nedenini içerir.

## <a name="check-status"></a>Durumu kontrol etme

Tüm bekleyen toplu isteklerinizin durumunu **toplu işlem sonuçları** sayfasında görebilirsiniz.

[![Toplu Işlemler sonuçları sayfasında durumu kontrol edin.](media/users-bulk-restore/bulk-center.png)](media/users-bulk-restore/bulk-center.png#lightbox)

Daha sonra, geri yüklediğiniz kullanıcıların Azure portal veya PowerShell kullanarak Azure AD kuruluşunda mevcut olup olmadığını kontrol edebilirsiniz.

## <a name="view-restored-users-in-the-azure-portal"></a>Azure portal geri yüklenen kullanıcıları görüntüleme

1. Kuruluşunuzda Kullanıcı Yöneticisi olan bir hesapla [Azure AD Yönetim merkezinde oturum açın](https://aad.portal.azure.com) .
1. Gezinti bölmesinde **Azure Active Directory**' yi seçin.
1. **Yönet** bölümünde **Kullanıcılar**'ı seçin.
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
