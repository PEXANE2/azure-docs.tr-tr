---
title: Azure SQL DB 'yi bir güvenlik duvarının arkasında Tara
description: Azure purview portalını kullanarak bir güvenlik duvarının arkasındaki kaynakları taramayı öğrenin.
author: sudhandkumar
ms.author: kumarsud
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 10/16/2020
ms.openlocfilehash: 4e1e76aaae2951e6d65fd1ebaaa5798ff417daf9
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96553814"
---
# <a name="scan-azure-sql-db-behind-a-firewall-in-azure-purview"></a>Azure purview 'da güvenlik duvarının arkasındaki Azure SQL DB 'yi tarayın

Bu makalede, bir güvenlik duvarının arkasındaki bir kaynağı taramak için Azure purview 'ın nasıl kullanılacağını öğrenirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

* Kendi [Azure Active Directory kiracınız](../active-directory/fundamentals/active-directory-access-create-new-tenant.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure hesabınızla [Azure portalında](https://portal.azure.com) oturum açın.

## <a name="set-up-sql-storage-behind-a-firewall"></a>Bir güvenlik duvarının arkasında SQL depolaması ayarlama

İlk adım, [bir Azure SQL veritabanı 'Na kayıt ve tarama](register-scan-azure-sql-database.md)aracılığıyla kataloğun MSI 'sini BIR Azure SQL DB 'ye eklemektir.

## <a name="scan-sql-db-from-purview"></a>SQL DB 'yi purview 'dan Tara

1. Takip görünümü giriş sayfasına gidin.

1. Sol gezinti bölmesinde **Yönetim Merkezi** ' ni seçin.

1. **Kaynaklar ve tarama** altında **veri kaynakları** ' nı seçin.

1. Veri kaynağını eklemek için **+ Yeni** seçeneğini belirleyin.

1. **Azure SQL veritabanı**' nı seçin.

   :::image type="content" source="./media/scan-resource-firewall/scan-sql-source.png" alt-text="SQL Server 'ı seçmek için ekran görüntüsü.":::

1. Yeni veri kaynağı için bir ad girin, **Azure aboneliği ' nden** seçim yapın ve açılan listeden aboneliğinizi ve sunucu adınızı seçin.

   Kaydı tamamlamaya **son** ' u seçin. 

   :::image type="content" source="./media/scan-resource-firewall/scan-sql-register.png" alt-text="Seçimi tamamlayacak ekran görüntüsü":::

1. Güvenlik Duvarı ve test bağlantısı arkasındaki depolama için **taramayı ayarla** ' yı seçin. Bağlantı, başarılı olduğunu gösterir. 

   :::image type="content" source="./media/scan-resource-firewall/scan-sql-setscan.png" alt-text="T0 'ın taramasını ayarla seçiminin ekran görüntüsü.":::

1. Tarama sıklığını ayarlayın ve **devam**' ı seçin.

   :::image type="content" source="./media/scan-resource-firewall/scan-sql-once.png" alt-text="SQL taramasını başlatmak için seçimin ekran görüntüsü.":::

1.  Tarama tamamlanır ve durum veri kaynakları listesinde güncelleştirilir.

   :::image type="content" source="./media/scan-resource-firewall/scan-sql-success.png" alt-text="Tamamlanan ileti taramasının ekran görüntüsü":::
   
## <a name="next-steps"></a>Sonraki adımlar

Daha sonra, bir tarama kuralı kümesi [oluşturabilirsiniz](create-a-scan-rule-set.md) .
