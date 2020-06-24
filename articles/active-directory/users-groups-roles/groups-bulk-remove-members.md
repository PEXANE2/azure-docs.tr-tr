---
title: CSV dosyası karşıya yükleyerek grup üyelerini toplu kaldırma-Azure Active Directory | Microsoft Docs
description: Azure Yönetim merkezinde toplu işlemlerde grup üyelerini kaldırın.
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
ms.openlocfilehash: e9084b486681ded0c194c93f07a404f5f5e88fa6
ms.sourcegitcommit: c4ad4ba9c9aaed81dfab9ca2cc744930abd91298
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84728478"
---
# <a name="bulk-remove-group-members-in-azure-active-directory"></a>Azure Active Directory Grup üyelerini toplu kaldırma

Azure Active Directory (Azure AD) portalını kullanarak, grup üyelerini toplu olarak kaldırmak için bir virgülle ayrılmış değerler (CSV) dosyası kullanarak bir gruptan çok sayıda üyeyi kaldırabilirsiniz.

## <a name="understand-the-csv-template"></a>CSV şablonunu anlama

Azure AD grup üyelerini toplu olarak başarıyla eklemek için toplu karşıya yükleme CSV şablonunu indirin ve girin. CSV şablonunuz şu örnekteki gibi görünebilir:

![Her satır ve sütunun amacını ve değerlerini açıklayan karşıya yükleme ve çağrı aşımları için elektronik tablo](./media/groups-bulk-remove-members/template-example.png)

### <a name="csv-template-structure"></a>CSV şablonu yapısı

İndirilen bir CSV şablonundaki satırlar aşağıdaki gibidir:

- **Sürüm numarası**: sürüm numarasını içeren ilk satır, KARŞıYA yükleme CSV 'ye eklenmelidir.
- **Sütun başlıkları**: sütun başlıklarının biçimi &lt; *öğe adı* &gt; [PropertyName] &lt; *gerekli veya boş* &gt; . Örneğin, `Member object ID or user principal name [memberObjectIdOrUpn] Required`. Şablonun bazı eski sürümlerinde hafif Çeşitlemeler bulunabilir. Grup üyeliği değişiklikleri için hangi tanımlayıcıyı kullanacağınızı seçebilirsiniz: üye nesne KIMLIĞI veya Kullanıcı asıl adı.
- **Örnekler satırı**: şablona her sütun için kabul edilebilir değer örneklerinin bir satırını ekledik. Örnekler satırını kaldırmalı ve kendi girişlerinizin yerine değiştirmelisiniz.

### <a name="additional-guidance"></a>Ek yönergeler

- Karşıya yükleme şablonunun ilk iki satırı kaldırılmamalıdır veya değiştirilmemelidir veya karşıya yükleme işlenemiyor.
- Önce gerekli sütunlar listelenir.
- Şablona yeni sütun eklenmesini önermiyoruz. Eklediğiniz tüm ek sütunlar yoksayılır ve işlenmez.
- CSV şablonunun en son sürümünü mümkün olduğunca sık indirmeniz önerilir.

## <a name="to-bulk-remove-group-members"></a>Grup üyelerini toplu olarak kaldırmak için

1. [Azure Portal,](https://portal.azure.com) kuruluştaki bir kullanıcı yönetici hesabıyla oturum açın. Grup sahipleri, sahip oldukları grupların üyelerini de toplu olarak kaldırabilir.
1. Azure AD 'de **gruplar**  >  **tüm gruplar**' ı seçin.
1. Üyelerini kaldırdığınız grubu açın ve ardından **Üyeler**' i seçin.
1. **Üyeler** sayfasında, **üyeleri kaldır**' ı seçin.
1. **Grup üyelerini toplu kaldırma** sayfasında, gerekli Grup ÜYESI özellikleriyle CSV dosya şablonunu almak için **İndir** ' i seçin.

   ![Üyeleri Kaldır komutu, grubun profil sayfasında bulunur](./media/groups-bulk-remove-members/remove-panel.png)

1. CSV dosyasını açın ve gruptan kaldırmak istediğiniz her grup üyesi için bir satır ekleyin (gerekli değerler üye nesne KIMLIĞI veya Kullanıcı asıl adı). Ardından dosyayı kaydedin.

    :::image type="content" source="./media/groups-bulk-import-members/csv-file.png" alt-text="CSV dosyası, kaldırılacak Grup üyelerinin adlarını ve kimliklerini içerir":::

1. **Grup üyelerini toplu kaldırma** sayfasında, **CSV dosyanızı karşıya yükleyin**bölümünde dosyaya gidin. Dosyayı seçtiğinizde, CSV dosyasının doğrulanması başlar.
1. Dosya içeriği doğrulandığında, toplu içeri aktarma sayfası **dosyayı başarıyla karşıya yüklendi**olarak görüntüler. Hatalar varsa, işi gönderebilmeniz için önce bunları çözmeniz gerekir.
1. Dosyanız doğrulamayı geçtiğinde, grup üyelerini gruptan kaldıran Azure toplu işlemini başlatmak için **Gönder** ' i seçin.
1. Kaldırma işlemi tamamlandığında toplu işlemin başarılı olduğunu belirten bir bildirim görürsünüz.

## <a name="check-removal-status"></a>Kaldırma durumunu denetle

Tüm bekleyen toplu isteklerinizin durumunu **toplu işlem sonuçları** sayfasında görebilirsiniz.

[![](media/groups-bulk-remove-members/bulk-center.png "Check status in the Bulk Operations Results page")](media/groups-bulk-remove-members/bulk-center.png#lightbox)

Toplu işlemdeki her bir satır öğesi hakkında ayrıntılar için **# Success**, **# Failure**veya **Total Requests** sütunlarının altındaki değerleri seçin. Hatalar oluştuysa, başarısızlık nedenleri listelenecektir.

## <a name="bulk-removal-service-limits"></a>Toplu kaldırma hizmeti sınırları

Grup üyelerinin bir listesini kaldırmak için her toplu etkinlik, bir saate kadar çalıştırılabilir. Bu, en az 40.000 üyenin listesini kaldırmaya izin vermez.

## <a name="next-steps"></a>Sonraki adımlar

- [Grup üyelerini Toplu içe aktarma](groups-bulk-import-members.md)
- [Bir grubun üyelerini indir](groups-bulk-download-members.md)
- [Tüm grupların bir listesini indir](groups-bulk-download.md)
