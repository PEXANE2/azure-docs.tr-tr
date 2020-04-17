---
title: Gruba üye eklemek için toplu içe aktarma yüklemesi - Azure Etkin Dizini | Microsoft Dokümanlar
description: Azure Etkin Dizin yönetici merkezinde grup üyelerini toplu olarak ekleyin.
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
ms.openlocfilehash: 15960caa55274f06159263c1af4a6c8280e83f4e
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81533503"
---
# <a name="bulk-import-group-members-in-azure-active-directory"></a>Azure Etkin Dizini'nde toplu alma grubu üyeleri

Azure Etkin Dizin (Azure AD) portalını kullanarak, toplu alma grubu üyeleri için virgülle ayrılmış değerler (CSV) dosyasını kullanarak gruba çok sayıda üye ekleyebilirsiniz.

## <a name="to-bulk-import-group-members"></a>Toplu alma grubu üyeleri için

1. Kuruluşta bir Kullanıcı yöneticisi hesabıyla [Azure portalında](https://portal.azure.com) oturum açın. Grup sahipleri, sahip oldukları grupların toplu alma üyelerini de toplu olarak alabilir.
1. Azure AD'de**Tüm grupları** **gruplar** > seçin.
1. Üye eklediğiniz grubu açın ve ardından **Üyeler'i**seçin.
1. **Üyeler** sayfasında, **Üye Alma'yı**seçin.
1. Toplu **alma grubu üyeleri** sayfasında, gerekli grup üyesi özelliklerine sahip CSV dosya şablonu almak için **İndir'i** seçin.

    ![Alma Üyeleri komutu grubun profil sayfasında](./media/groups-bulk-import-members/import-panel.png)

1. CSV dosyasını açın ve gruba almak istediğiniz her grup üyesi için bir satır ekleyin (gerekli değerler **Üye nesne kimliği** veya Kullanıcı ana **adıdır).** Ardından dosyayı kaydedin.

   ![CSV dosyası, üyelerin içe aktarması için adlar ve kisimler içerir](./media/groups-bulk-import-members/csv-file.png)

1. Toplu **alma grubu üyeleri** sayfasında, **csv dosyanızı yükleyin**altında, dosyaya göz atın. Dosyayı seçtiğinizde, CSV dosyasının doğrulanması başlar.
1. Dosya içeriği doğrulandığında, toplu içe aktarma sayfası **başarıyla yüklenen Dosyayı**görüntüler. Hatalar varsa, işi göndermeden önce bunları düzeltmeniz gerekir.
1. Dosyanız doğrulamadan geçtiğinde, grup üyelerini gruba aktaran Azure toplu işlemini başlatmak için **Gönder'i** seçin.
1. Alma işlemi tamamlandığında, toplu işlemin başarılı olduğuna dair bir bildirim görürsünüz.

## <a name="check-import-status"></a>Alma durumunu denetleme

Bekleyen toplu isteklerinizin durumunu Toplu işlem **sonuçları** sayfasında görebilirsiniz.

[![](media/groups-bulk-import-members/bulk-center.png "Check status in the Bulk Operations Results page")](media/groups-bulk-import-members/bulk-center.png#lightbox)

Toplu işlemdeki her satır öğesi yle ilgili ayrıntılar için **# Success**, **# Failure**veya Toplam **İstek** sütunları altındaki değerleri seçin. Hatalar oluştuysa, hata nedenleri listelenir.

## <a name="bulk-import-service-limits"></a>Toplu alma hizmet limitleri

Grup üyelerinin listesini almak için her toplu etkinlik bir saate kadar çalıştırılabilir. Bu, en az 40.000 üyeden oluşan bir listenin içe aktarılmasını sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- [Toplu kaldırma grup üyeleri](groups-bulk-remove-members.md)
- [Bir grubun üyelerini indirin](groups-bulk-download-members.md)
- [Tüm grupların listesini indirin](groups-bulk-download.md)
