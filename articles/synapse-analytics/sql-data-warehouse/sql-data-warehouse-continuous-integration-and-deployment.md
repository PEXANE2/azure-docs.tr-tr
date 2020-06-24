---
title: Sürekli tümleştirme ve dağıtım
description: Azure Pipelines kullanarak sürekli tümleştirme ve dağıtım için yerleşik desteğe sahip veri depolamaya yönelik kurumsal sınıf veritabanı DevOps deneyimi.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: sql-dw
ms.date: 02/04/2020
ms.author: kevin
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: 725e8165f8a7bdb654f61d7257867a2d0bf17110
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85213576"
---
# <a name="continuous-integration-and-deployment-for-data-warehousing"></a>Veri depolamaya yönelik sürekli tümleştirme ve dağıtım

Bu basit öğreticide, SQL Server veri araçları (SSDT) veritabanı projenizi Azure DevOps ile tümleştirme ve sürekli tümleştirme ve dağıtım ayarlamak için Azure Pipelines yararlanma özetlenmektedir. Bu öğretici, veri depolamaya yönelik sürekli tümleştirme ve dağıtım işlem hattınızı oluşturmanın ikinci adımıdır.

## <a name="before-you-begin"></a>Başlamadan önce

- [Kaynak denetimi tümleştirme öğreticisine](sql-data-warehouse-source-control-integration.md) gidin

- Azure DevOps 'ı ayarlama ve bu sunucuya bağlanma

## <a name="continuous-integration-with-visual-studio-build"></a>Visual Studio derlemesi ile sürekli tümleştirme

1. Azure Pipelines gidin ve yeni bir derleme işlem hattı oluşturun.

      ![Yeni işlem hattı](./media/sql-data-warehouse-continuous-integration-and-deployment/1-new-build-pipeline.png "Yeni İşlem Hattı")

2. Kaynak kodu deponuzu (Azure Repos git) seçin ve .NET Masaüstü uygulama şablonunu seçin.

      ![İşlem hattı kurulumu](./media/sql-data-warehouse-continuous-integration-and-deployment/2-pipeline-setup.png "İşlem hattı kurulumu")

3. YAML Dosyanızı, aracınızın uygun havuzunu kullanacak şekilde düzenleyin. YAML dosyanız şuna benzer şekilde görünmelidir:

      ![YAML](./media/sql-data-warehouse-continuous-integration-and-deployment/3-yaml-file.png "YAML")

Bu noktada, kaynak denetimi depo ana dalınızdaki tüm iadelerinizin, veritabanı projenizin başarılı bir Visual Studio derlemesini otomatik olarak tetiklemesi gereken basit bir ortamınız vardır. Yerel veritabanı projenizde bir değişiklik yaparak ve bu değişikliği ana dalınıza iade ederek Otomasyon 'un uçtan uca çalıştığını doğrulayın.

## <a name="continuous-deployment-with-the-azure-sql-data-warehouse-or-database-deployment-task"></a>Azure SQL veri ambarı (veya veritabanı) dağıtım göreviyle sürekli dağıtım

1. [Azure SQL veritabanı dağıtım görevini](/azure/devops/pipelines/targets/azure-sqldb) kullanarak yeni bir görev ekleyin ve hedef veri ambarınıza bağlanmak için gerekli alanları girin. Bu görev çalıştırıldığında, önceki derleme işleminden oluşturulan DACPAC, hedef veri ambarına dağıtılır. [Azure SQL veri ambarı dağıtım görevini](https://marketplace.visualstudio.com/items?itemName=ms-sql-dw.SQLDWDeployment)de kullanabilirsiniz.

      ![Dağıtım görevi](./media/sql-data-warehouse-continuous-integration-and-deployment/4-deployment-task.png "Dağıtım görevi")

2. Kendi kendine barındırılan bir aracı kullanıyorsanız, ortam değişkeninizi SQL veri ambarı için doğru SqlPackage.exe kullanacak şekilde ayarladığınızdan emin olun. Yol şuna benzemelidir:

      ![Ortam değişkeni](./media/sql-data-warehouse-continuous-integration-and-deployment/5-environment-variable-preview.png "Ortam değişkeni")

   C:\Program Files (x86) \Microsoft Visual Studio\2019\Preview\Common7\IDE\Extensions\Microsoft\SQLDB\DAC\150  

   İşlem hattınızı çalıştırın ve doğrulayın. Yerel olarak değişiklik yapabilir ve bir otomatik derleme ve dağıtım oluşturması gereken kaynak denetimi değişiklikler iade edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [SYNAPSE SQL havuzu MPP mimarisini](massively-parallel-processing-mpp-architecture.md) keşfet
- Hızlıca [BIR SQL havuzu oluşturun](create-data-warehouse-portal.md)
- [Örnek verileri yükleme](load-data-from-azure-blob-storage-using-polybase.md)
- [Videoları](sql-data-warehouse-videos.md) keşfet
