---
title: SYNAPSE SQL havuzu için ölçek işlem (Azure portal)
description: Azure portal kullanarak SYNAPSE SQL havuzunun (veri ambarı) işlem ölçeğini ölçeklendirebilirsiniz.
services: synapse-analytics
author: Antvgski
manager: craigg
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: sql-dw
ms.date: 04/28/2020
ms.author: anvang
ms.reviewer: jrasnick
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: d944ff86cdc11c30142792c6fd15d03e3cfd4fa9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85212947"
---
# <a name="quickstart-scale-compute-for-synapse-sql-pool-with-the-azure-portal"></a>Hızlı başlangıç: Azure portal ile SYNAPSE SQL havuzu için işlem ölçekleme

Azure portal kullanarak SYNAPSE SQL havuzunun (veri ambarı) işlem ölçeğini ölçeklendirebilirsiniz. Daha iyi performans için [işlemin ölçeğini genişletin](sql-data-warehouse-manage-compute-overview.md) veya maliyet tasarrufu sağlamak için işlemin ölçeğini geri daraltın. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="before-you-begin"></a>Başlamadan önce

Zaten sahip olduğunuz bir SQL havuzunu ölçeklendirebilir veya [hızlı başlangıç: oluşturma ve bağlanma-Portal](create-data-warehouse-portal.md) ' a, **mysampledatawarehouse**adlı bir SQL havuzu oluşturabilirsiniz. Bu hızlı başlangıç, **mySampleDataWarehouse** öğesini ölçeklendirir.

>[!IMPORTANT] 
>Ölçeklendirilmesi için SQL havuzunuzun çevrimiçi olması gerekir. 

## <a name="scale-compute"></a>Hesaplamayı ölçeklendirme

SQL havuzu işlem kaynakları, veri ambarı birimleri arttırılarak veya azaltılarak ölçeklendirilebilir. [Hızlı başlangıç: oluşturma ve bağlanma-Portal](create-data-warehouse-portal.md) **mysampledatawarehouse** oluşturdu ve 400 dwus ile başlatıldı. Aşağıdaki adımlar, **mySampleDataWarehouse** için DWU’ları ayarlar.

Veri ambarı birimlerini değiştirmek için:

1. Azure portal sol sayfasında **Azure SYNAPSE Analytics (eski ADıYLA SQL DW)** seçeneğine tıklayın.
2. **Azure SYNAPSE Analytics (eski ADıYLA SQL DW)** sayfasından **mysampledatawarehouse** öğesini seçin. SQL Havuzu açılır.
3. **Ölçek** seçeneğine tıklayın.

    ![Ölçek seçeneğine tıklayın](./media/quickstart-scale-compute-portal/click-scale.png)

2. Ölçek panelinde kaydırıcıyı sola veya sağa hareket ettirerek DWU ayarını değiştirin. Ardından ölçek ' i seçin.

    ![Kaydırıcıyı Taşıyın](./media/quickstart-scale-compute-portal/scale-dwu.png)

## <a name="next-steps"></a>Sonraki adımlar
SQL havuzu hakkında daha fazla bilgi edinmek için [VERILERI SQL havuzu](load-data-from-azure-blob-storage-using-polybase.md) öğreticisine yüklemeye devam edin. 
