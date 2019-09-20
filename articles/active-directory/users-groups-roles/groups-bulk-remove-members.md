---
title: CSV dosyası karşıya yükleyerek grup üyelerini toplu kaldırma-Azure Active Directory | Microsoft Docs
description: Azure Yönetim Merkezi 'nde toplu olarak Kullanıcı ekleyin.
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
ms.openlocfilehash: 08708c23f9c8f4c4a8fc9f2f0aa5cd20d8333a42
ms.sourcegitcommit: b03516d245c90bca8ffac59eb1db522a098fb5e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71146362"
---
# <a name="bulk-remove-group-members-preview-in-azure-active-directory"></a>Azure Active Directory Grup üyelerini toplu kaldırma (Önizleme)

Azure Active Directory (Azure AD) portalını kullanarak, grup üyelerini toplu olarak kaldırmak için bir virgülle ayrılmış değerler (CSV) dosyası kullanarak bir gruptan çok sayıda üyeyi kaldırabilirsiniz.

> [!NOTE]
> Azure AD toplu işlemleri, Azure AD 'nin genel önizleme özelliğidir ve ücretli Azure AD lisans planıyla birlikte kullanılabilir. Önizleme kullanım koşulları hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="to-bulk-remove-group-members"></a>Grup üyelerini toplu olarak kaldırmak için

1. [Azure Portal,](https://portal.azure.com) kuruluştaki bir kullanıcı yönetici hesabıyla oturum açın. Grup sahipleri, sahip oldukları grupların üyelerini de toplu olarak kaldırabilir.
1. Azure AD 'de **gruplar** > **tüm gruplar**' ı seçin.
1. Üyelerini kaldırdığınız grubu açın ve ardından **Üyeler**' i seçin.
1. **Üyeler** sayfasında, **üyeleri kaldır**' ı seçin.
1. **Grup üyelerini toplu Kaldır (Önizleme)** sayfasında, gerekli Grup ÜYESI özellikleriyle CSV dosya şablonunu almak için **İndir** ' i seçin.

   ![Üyeleri Kaldır komutu, grubun profil sayfasında bulunur](./media/groups-bulk-remove-members/remove-panel.png)

1. CSV dosyasını açın ve gruptan kaldırmak istediğiniz her grup üyesi için bir satır ekleyin (gerekli değerler üye nesne KIMLIĞI veya Kullanıcı asıl adı). Ardından dosyayı kaydedin.

   ![CSV dosyası, kaldırılacak üyelerin adlarını ve kimliklerini içerir](./media/groups-bulk-remove-members/csv-file.png)

1. **Grup üyelerini toplu Kaldır (Önizleme)** sayfasında, **CSV dosyanızı karşıya yükleyin**bölümünde dosyasına gidin. Dosyayı seçtiğinizde,. csv dosyasının doğrulanması başlar.
1. Dosya içeriği doğrulandığında, toplu içeri aktarma sayfası **dosyayı başarıyla karşıya yüklendi**olarak görüntüler. Hatalar varsa, işi gönderebilmeniz için önce bunları çözmeniz gerekir.
1. Dosyanız doğrulamayı geçtiğinde, grup üyelerini gruptan kaldıran Azure toplu işlemini başlatmak için **Gönder** ' i seçin.
1. Kaldırma işlemi tamamlandığında toplu işlemin başarılı olduğunu belirten bir bildirim görürsünüz.

## <a name="check-removal-status"></a>Kaldırma durumunu denetle

Tüm bekleyen toplu isteklerinizin durumunu **toplu işlem sonuçları (Önizleme)** sayfasında görebilirsiniz.

   ![Toplu işlem sonuçları sayfasında, toplu istek durumu gösterilir](./media/groups-bulk-remove-members/bulk-center.png)

Toplu işlemdeki her bir satır öğesi hakkında ayrıntılar için **# Success**, **# Failure**veya **Total Requests** sütunlarının altındaki değerleri seçin. Hatalar oluştuysa, başarısızlık nedenleri listelenecektir.

## <a name="bulk-removal-service-limits"></a>Toplu kaldırma hizmeti sınırları

Grup üyelerinin bir listesini kaldırmak için her toplu etkinlik, bir saate kadar çalıştırılabilir. Bu, en az 40.000 üyenin listesini kaldırmaya izin vermez.

## <a name="next-steps"></a>Sonraki adımlar

- [Grup üyelerini Toplu içe aktarma](groups-bulk-import-members.md)
- [Bir grubun üyelerini indir](groups-bulk-download-members.md)
- [Tüm grupların bir listesini indir](groups-bulk-download.md)
