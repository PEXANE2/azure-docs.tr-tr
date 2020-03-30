---
title: Synapse SQL havuzu (Azure portalı) için ölçek hesaplama
description: Azure portalını kullanarak Synapse SQL havuzu (veri ambarı) için bilgi işlem ölçeklendirebilirsiniz.
services: synapse-analytics
author: Antvgski
manager: craigg
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/17/2018
ms.author: anvang
ms.reviewer: jrasnick
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: f92152658b9db83740ffc2de2dc6956003849e06
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350828"
---
# <a name="quickstart-scale-compute-for-synapse-sql-pool-with-the-azure-portal"></a>Quickstart: Azure portalı ile Synapse SQL havuzu için hesaplamayı ölçeklendirin

Azure portalını kullanarak Synapse SQL havuzu (veri ambarı) için bilgi işlem ölçeklendirebilirsiniz. Daha iyi performans için [işlemin ölçeğini genişletin](sql-data-warehouse-manage-compute-overview.md) veya maliyet tasarrufu sağlamak için işlemin ölçeğini geri daraltın. 

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/)oturum açın.

## <a name="before-you-begin"></a>Başlamadan önce

Zaten sahip olduğunuz bir SQL havuzuna ölçeklendirebilirsiniz veya [Quickstart'ı kullanabilirsiniz:](create-data-warehouse-portal.md) **mySampleDataWarehouse**adında bir SQL havuzu oluşturmak ve bağlanmak - portalı. Bu hızlı başlangıç, **mySampleDataWarehouse** öğesini ölçeklendirir.

>[!IMPORTANT] 
>SQL havuzunuz ölçeklendirmek için çevrimiçi olmalıdır. 

## <a name="scale-compute"></a>Hesaplamayı ölçeklendirme

SQL havuzu bilgi işlem kaynakları, veri ambarı birimlerini artırarak veya azaltarak ölçeklendirilebilir. [Create and connect - portal] quickstart (create-data-warehouse-portal.md) **mySampleDataWarehouse'u** oluşturdu ve 400 DWUs ile başlattı. Aşağıdaki adımlar, **mySampleDataWarehouse** için DWU’ları ayarlar.

Veri ambarı birimlerini değiştirmek için:

1. Azure portalının sol sayfasında **Azure Synapse Analytics'i (eski adıyla SQL DW)** tıklatın.
2. **Azure Synapse Analytics (eski adıyla SQL DW)** sayfasından **mySampleDataWarehouse'u** seçin. SQL havuzu açılır.
3. **Ölçek** seçeneğine tıklayın.

    ![Ölçek seçeneğine tıklayın](./media/quickstart-scale-compute-portal/click-scale.png)

2. Ölçek panelinde kaydırıcıyı sola veya sağa hareket ettirerek DWU ayarını değiştirin. Sonra ölçek seçin.

    ![Kaydırıcıyı Taşıyın](./media/quickstart-scale-compute-portal/scale-dwu.png)

## <a name="next-steps"></a>Sonraki adımlar
SQL havuzu hakkında daha fazla bilgi edinmek [için, verileri SQL havuzu öğreticisine yüklemeye](load-data-from-azure-blob-storage-using-polybase.md) devam edin. 
