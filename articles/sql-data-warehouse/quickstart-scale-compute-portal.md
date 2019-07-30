---
title: "Hızlı Başlangıç: Azure SQL veri ambarı 'nda işlem ölçeğini genişletme-Azure portal | Microsoft Docs"
description: Azure portalından Azure SQL Veri Ambarı’nda işlemi ölçeklendirin. Daha iyi performans için işlem ölçeğini genişletin veya maliyet tasarrufu için işlem ölçeğini daraltın.
services: sql-data-warehouse
author: Antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.subservice: implement
ms.date: 04/17/2018
ms.author: anvang
ms.reviewer: jrasnick
ms.openlocfilehash: b392bfe44c61b46009ab1ab7f87fb6ef874a7f88
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68641069"
---
## <a name="quickstart-scale-compute-in-azure-sql-data-warehouse-in-the-azure-portal"></a>Hızlı Başlangıç: Azure portal Azure SQL veri ambarı 'nda işlem ölçeğini ölçeklendirme

Azure portalından Azure SQL Veri Ambarı’nda işlemi ölçeklendirin. Daha iyi performans için [işlem ölçeğini genişletin](sql-data-warehouse-manage-compute-overview.md) veya maliyet tasarrufu için işlem ölçeğini daraltın. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="before-you-begin"></a>Başlamadan önce

Zaten sahip olduğunuz bir veri ambarını ölçeklendirebilir veya [hızlı başlangıç: oluşturma ve bağlanma-Portal](create-data-warehouse-portal.md) ' ı kullanarak **mysampledatawarehouse**adlı bir veri ambarı oluşturabilirsiniz.  Bu hızlı başlangıç, **mySampleDataWarehouse** öğesini ölçeklendirir.

>[!Note]
>Ölçeklendirilmesi için veri ambarınızın çevrimiçi olması gerekir. 

## <a name="scale-compute"></a>Hesaplamayı ölçeklendirme

SQL veri ambarı işlem kaynakları, veri ambarı birimleri arttırılarak veya azaltılarak ölçeklendirilebilir. [Oluşturma ve bağlanma-Portal] hızlı başlangıç (Create-Data-Warehouse-portal.md), **Mysampledatawarehouse** oluşturdu ve 400 dwus ile başlatıldı. Aşağıdaki adımlar, **mySampleDataWarehouse** için DWU’ları ayarlar.

Veri ambarı birimlerini değiştirmek için:

1. Azure portalının sol taraftaki sayfasında **SQL veri ambarları**’na tıklayın.
2. **SQL veri ambarları** sayfasından **mySampleDataWarehouse** seçeneğini belirleyin. Veri ambarı açılır.
3. **Ölçek** seçeneğine tıklayın.

    ![Ölçek seçeneğine tıklayın](media/quickstart-scale-compute-portal/click-scale.png)

2. Ölçek panelinde kaydırıcıyı sola veya sağa hareket ettirerek DWU ayarını değiştirin.

    ![Kaydırıcıyı Taşıyın](media/quickstart-scale-compute-portal/scale-dwu.png)

3. **Kaydet**’e tıklayın. Bir onay iletisi görüntülenir. Onaylamak için **evet**’e, iptal etmek için **hayır**’a tıklayın.

    ![Kaydet’e tıklayın.](media/quickstart-scale-compute-portal/confirm-change.png)



## <a name="next-steps"></a>Sonraki adımlar
Artık veri ambarınızın işlem ölçeğini ölçeklendirdiniz. Azure SQL Veri Ambarı hakkında daha fazla bilgi edinmek için, veri yükleme öğreticisiyle devam edin.

> [!div class="nextstepaction"]
>[SQL veri ambarına veri yükleme](load-data-from-azure-blob-storage-using-polybase.md)
