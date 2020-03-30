---
title: CSV dosyası yükleyerek grup üyelerini toplu olarak kaldırma - Azure Active Directory | Microsoft Dokümanlar
description: Azure yönetici merkezinde kullanıcıları toplu olarak ekleyin.
services: active-directory
author: curtand
ms.author: curtand
manager: mtillman
ms.date: 09/11/2019
ms.topic: conceptual
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9d384ea4749e2d0bc7edf8df7ac0508566f2f76b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72517098"
---
# <a name="bulk-remove-group-members-preview-in-azure-active-directory"></a>Azure Etkin Dizini'nde grup üyelerini toplu olarak kaldırma (önizleme)

Azure Etkin Dizin (Azure AD) portalını kullanarak, grup üyelerini toplu olarak kaldırmak için virgülle ayrılmış değerler (CSV) dosyasını kullanarak çok sayıda üyeyi gruptan kaldırabilirsiniz.

## <a name="to-bulk-remove-group-members"></a>Grup üyelerini toplu olarak kaldırmak için

1. Kuruluşta bir Kullanıcı yöneticisi hesabıyla [Azure portalında](https://portal.azure.com) oturum açın. Grup sahipleri, sahip oldukları grupların üyelerini toplu olarak kaldırabilir.
1. Azure AD'de**Tüm grupları** **gruplar** > seçin.
1. Üye kaldırdığınız grubu açın ve ardından **Üyeler'i**seçin.
1. **Üyeler** **sayfasında, Üye Kaldır'ı**seçin.
1. Toplu **kaldırma grubu üyeleri (Önizleme)** sayfasında, gerekli grup üye özelliklerine sahip CSV dosya şablonu almak için **İndir'i** seçin.

   ![Üyeleri Kaldır komutu grubun profil sayfasında](./media/groups-bulk-remove-members/remove-panel.png)

1. CSV dosyasını açın ve gruptan kaldırmak istediğiniz her grup üyesi için bir satır ekleyin (gerekli değerler Üye nesne kimliği veya Kullanıcı ana adıdır). Ardından dosyayı kaydedin.

   ![CSV dosyası, üyelerin kaldırabilmek için adlar ve kisimler içerir](./media/groups-bulk-remove-members/csv-file.png)

1. Toplu **kaldırmak grup üyeleri (Önizleme)** sayfasında, **csv dosyanızı yükleyin**altında, dosyaya göz atın. Dosyayı seçtiğinizde,.csv dosyasının doğrulanması başlar.
1. Dosya içeriği doğrulandığında, toplu içe aktarma sayfası **başarıyla yüklenen Dosyayı**görüntüler. Hatalar varsa, işi göndermeden önce bunları düzeltmeniz gerekir.
1. Dosyanız doğrulamadan geçtiğinde, grup üyelerini gruptan kaldıran Azure toplu işlemini başlatmak için **Gönder'i** seçin.
1. Kaldırma işlemi tamamlandığında, toplu işlemin başarılı olduğuna dair bir bildirim görürsünüz.

## <a name="check-removal-status"></a>Kaldırma durumunu denetleme

**Toplu işlem sonuçları (önizleme)** sayfasında bekleyen toplu isteklerinizin durumunu görebilirsiniz.

   ![Toplu işlem sonuçları sayfası toplu istek durumunu gösterir](./media/groups-bulk-remove-members/bulk-center.png)

Toplu işlemdeki her satır öğesi yle ilgili ayrıntılar için **# Success**, **# Failure**veya Toplam **İstek** sütunları altındaki değerleri seçin. Hatalar oluştuysa, hata nedenleri listelenir.

## <a name="bulk-removal-service-limits"></a>Toplu kaldırma hizmeti sınırları

Grup üyelerinin listesini kaldırmak için yapılan her toplu etkinlik bir saate kadar çalıştırılabilir. Bu, en az 40.000 üyeden oluşan bir listenin kaldırılmasını sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- [Toplu alma grubu üyeleri](groups-bulk-import-members.md)
- [Bir grubun üyelerini indirin](groups-bulk-download-members.md)
- [Tüm grupların listesini indirin](groups-bulk-download.md)
