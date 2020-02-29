---
title: 'Hızlı başlangıç: ölçek işlem-Azure portal '
description: Azure portal SQL havuzunda işlem ölçeğini ölçeklendirin. Daha iyi performans için işlem ölçeğini genişletin veya maliyet tasarrufu için işlem ölçeğini daraltın.
services: sql-data-warehouse
author: Antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.subservice: implement
ms.date: 04/17/2018
ms.author: anvang
ms.reviewer: jrasnick
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 7463849223fdf81466237c7d0c912763988e80e6
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78200370"
---
# <a name="quickstart-scale-compute-in-azure-synapse-analytics-sql-pool-in-the-azure-portal"></a>Hızlı başlangıç: Azure portal Azure SYNAPSE Analytics SQL havuzunda işlem ölçeğini işleme

Azure portal SQL havuzunda işlem ölçeğini ölçeklendirin. Daha iyi performans için [işlem ölçeğini genişletin](sql-data-warehouse-manage-compute-overview.md) veya maliyet tasarrufu için işlem ölçeğini daraltın. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="before-you-begin"></a>Başlamadan önce

Zaten sahip olduğunuz bir SQL havuzunu ölçeklendirebilir veya [hızlı başlangıç: oluşturma ve bağlanma-Portal](create-data-warehouse-portal.md) ' ı kullanarak **mysampledatawarehouse**adlı bir SQL havuzu oluşturabilirsiniz.  Bu hızlı başlangıç, **mySampleDataWarehouse** öğesini ölçeklendirir.

>[!Note]
>Ölçeklendirilmesi için SQL havuzunuzun çevrimiçi olması gerekir. 

## <a name="scale-compute"></a>Hesaplamayı ölçeklendirme

SQL havuzu işlem kaynakları, veri ambarı birimleri arttırılarak veya azaltılarak ölçeklendirilebilir. [Oluşturma ve bağlanma-Portal] hızlı başlangıç (Create-Data-Warehouse-portal.md), **Mysampledatawarehouse** oluşturdu ve 400 dwus ile başlatıldı. Aşağıdaki adımlar, **mySampleDataWarehouse** için DWU’ları ayarlar.

Veri ambarı birimlerini değiştirmek için:

1. Azure portal sol sayfasında **Azure SYNAPSE Analytics (eski ADıYLA SQL DW)** seçeneğine tıklayın.
2. **Azure SYNAPSE Analytics (eski ADıYLA SQL DW)** sayfasından **mysampledatawarehouse** öğesini seçin. SQL Havuzu açılır.
3. **Ölçek** seçeneğine tıklayın.

    ![Ölçek seçeneğine tıklayın](media/quickstart-scale-compute-portal/click-scale.png)

2. Ölçek panelinde kaydırıcıyı sola veya sağa hareket ettirerek DWU ayarını değiştirin. Ardından ölçek ' i seçin.

    ![Kaydırıcıyı Taşıyın](media/quickstart-scale-compute-portal/scale-dwu.png)

## <a name="next-steps"></a>Sonraki adımlar
Artık SQL havuzunun işlem ölçeğini ölçeklendirdiniz. SQL havuzu hakkında daha fazla bilgi edinmek için veri yükleme öğreticisine geçin.

> [!div class="nextstepaction"]
>[Verileri SQL havuzuna yükleme](load-data-from-azure-blob-storage-using-polybase.md)
