---
title: Grubun üyelerini eklemek veya oluşturmak için toplu karşıya yükleme-Azure Active Directory | Microsoft Docs
description: Grup üyelerini Azure Active Directory Yönetim merkezinde toplu olarak ekleyin.
services: active-directory
author: curtand
ms.author: curtand
manager: mtillman
ms.date: 04/27/2020
ms.topic: conceptual
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8902c3147bbe142fc58d4e2c3fa83601c8ccbba3
ms.sourcegitcommit: 67bddb15f90fb7e845ca739d16ad568cbc368c06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82203547"
---
# <a name="bulk-import-group-members-in-azure-active-directory"></a>Azure Active Directory Grup üyelerini Toplu içe aktarma

Azure Active Directory (Azure AD) portalını kullanarak Grup üyelerini toplu olarak içeri aktarmak için bir virgülle ayrılmış değerler (CSV) dosyası kullanarak gruba çok sayıda üye ekleyebilirsiniz.

## <a name="understand-the-csv-template"></a>CSV şablonunu anlama

Azure AD grup üyelerini toplu olarak başarıyla eklemek için toplu karşıya yükleme CSV şablonunu indirin ve girin. CSV şablonunuz şu örnekteki gibi görünebilir:

![Her satır ve sütunun amacını ve değerlerini açıklayan karşıya yükleme ve çağrı aşımları için elektronik tablo](./media/groups-bulk-import-members/template-with-callouts.png)

### <a name="csv-template-structure"></a>CSV şablonu yapısı

İndirilen bir CSV şablonundaki satırlar aşağıdaki gibidir:

- **Sürüm numarası**: sürüm numarasını içeren ilk satır, KARŞıYA yükleme CSV 'ye eklenmelidir.
- **Sütun başlıkları**: sütun &lt;başlıklarının biçimi *öğe adı* &gt; [PropertyName] &lt; *gerekli veya boş*&gt;. Örneğin, `Member object ID or user principal name [memberObjectIdOrUpn] Required`. Şablonun bazı eski sürümlerinde hafif Çeşitlemeler bulunabilir. Grup üyeliği değişiklikleri için hangi tanımlayıcıyı kullanacağınızı seçebilirsiniz: üye nesne KIMLIĞI veya Kullanıcı asıl adı.
- **Örnekler satırı**: şablona her sütun için kabul edilebilir değer örneklerinin bir satırını ekledik. Örnekler satırını kaldırmalı ve kendi girişlerinizin yerine değiştirmelisiniz.

### <a name="additional-guidance"></a>Ek yönergeler

- Karşıya yükleme şablonunun ilk iki satırı kaldırılmamalıdır veya değiştirilmemelidir veya karşıya yükleme işlenemiyor.
- Önce gerekli sütunlar listelenir.
- Şablona yeni sütun eklenmesini önermiyoruz. Eklediğiniz tüm ek sütunlar yoksayılır ve işlenmez.
- CSV şablonunun en son sürümünü mümkün olduğunca sık indirmeniz önerilir.

## <a name="to-bulk-import-group-members"></a>Grup üyelerini toplu olarak içeri aktarmak için

1. [Azure Portal,](https://portal.azure.com) kuruluştaki bir kullanıcı yönetici hesabıyla oturum açın. Grup sahipleri ayrıca sahip oldukları grupların üyelerini toplu olarak içeri aktarabilir.
1. Azure AD 'de **gruplar** > **tüm gruplar**' ı seçin.
1. Üyeler eklemekte olduğunuz grubu açın ve ardından **Üyeler**' i seçin.
1. **Üyeler** sayfasında, **üyeleri içeri aktar**' ı seçin.
1. **Grup üyelerini toplu içeri aktar** sayfasında, gerekli Grup ÜYESI özellikleriyle CSV dosya şablonunu almak için **İndir** ' i seçin.

    ![Üyeleri Içeri aktar komutu, grubun profil sayfasında bulunur](./media/groups-bulk-import-members/import-panel.png)

1. CSV dosyasını açın ve gruba aktarmak istediğiniz her grup üyesi için bir satır ekleyin (gerekli değerler **üye nesne kimliği** veya **Kullanıcı asıl adı**). Ardından dosyayı kaydedin.

   ![CSV dosyası, içeri aktarılacak üyelerin adlarını ve kimliklerini içerir](./media/groups-bulk-import-members/csv-file.png)

1. **Toplu içeri aktarma grubu üyeleri** sayfasında, **CSV dosyanızı karşıya yükleyin**bölümünde dosyaya gidin. Dosyayı seçtiğinizde, CSV dosyasının doğrulanması başlar.
1. Dosya içeriği doğrulandığında, toplu içeri aktarma sayfası **dosyayı başarıyla karşıya yüklendi**olarak görüntüler. Hatalar varsa, işi gönderebilmeniz için önce bunları çözmeniz gerekir.
1. Dosyanız doğrulamayı geçtiğinde, grup üyelerini gruba aktaran Azure toplu işlemini başlatmak için **Gönder** ' i seçin.
1. İçeri aktarma işlemi tamamlandığında toplu işlemin başarılı olduğunu belirten bir bildirim görürsünüz.

## <a name="check-import-status"></a>İçeri aktarma durumunu denetle

Tüm bekleyen toplu isteklerinizin durumunu **toplu işlem sonuçları** sayfasında görebilirsiniz.

[![](media/groups-bulk-import-members/bulk-center.png "Check status in the Bulk Operations Results page")](media/groups-bulk-import-members/bulk-center.png#lightbox)

Toplu işlemdeki her bir satır öğesi hakkında ayrıntılar için **# Success**, **# Failure**veya **Total Requests** sütunlarının altındaki değerleri seçin. Hatalar oluştuysa, başarısızlık nedenleri listelenecektir.

## <a name="bulk-import-service-limits"></a>Toplu içeri aktarma hizmeti sınırları

Grup üyelerinin bir listesini içeri aktarmaya yönelik her toplu etkinlik, bir saate kadar çalıştırılabilir. Bu, en az 40.000 üye listesinin içe aktarılması işlemini izin vermez.

## <a name="next-steps"></a>Sonraki adımlar

- [Grup üyelerini toplu kaldırma](groups-bulk-remove-members.md)
- [Bir grubun üyelerini indir](groups-bulk-download-members.md)
- [Tüm grupların bir listesini indir](groups-bulk-download.md)
