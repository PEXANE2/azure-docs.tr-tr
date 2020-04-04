---
title: Sürekli tümleştirme ve dağıtım
description: Azure Pipelines'ı kullanarak sürekli tümleştirme ve dağıtım için yerleşik destekle veri depolama için kurumsal sınıf Veritabanı DevOps deneyimi.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 02/04/2020
ms.author: kevin
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: 20afa3f37bb85fd268962aea03107f0eaeb9bea2
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80633592"
---
# <a name="continuous-integration-and-deployment-for-data-warehousing"></a>Veri depolama için sürekli entegrasyon ve dağıtım

Bu basit öğretici, SQL Server Veri araçları (SSDT) veritabanı projenizi Azure DevOps ile nasıl entegre edebilirsiniz ve sürekli tümleştirme ve dağıtım ayarlamak için Azure Pipelines'dan nasıl yararlanacağınızı özetlemektedir. Bu öğretici, veri depolama için sürekli tümleştirme ve dağıtım ardışık alan oluşturmanın ikinci adımıdır.

## <a name="before-you-begin"></a>Başlamadan önce

- [Kaynak denetimi tümleştirme öğreticisini](sql-data-warehouse-source-control-integration.md) gözden geçirin

- Azure DevOps'leri ayarlama ve bu tür lere bağlanma

## <a name="continuous-integration-with-visual-studio-build"></a>Visual Studio build ile sürekli entegrasyon

1. Azure Ardışık Hatları'na gidin ve yeni bir yapı ardışık hattı oluşturun.

      ![Yeni Boru Hattı](./media/sql-data-warehouse-continuous-integration-and-deployment/1-new-build-pipeline.png "Yeni İşlem Hattı")

2. Kaynak kod deponuzu (Azure Repos Git) ve .NET Desktop uygulama şablonunu seçin.

      ![Boru Hattı Kurulumu](./media/sql-data-warehouse-continuous-integration-and-deployment/2-pipeline-setup.png "Boru Hattı Kurulumu")

3. Aracınızın uygun havuzunu kullanmak için YAML dosyanızı edin. YAML dosyanız şuna benzer olmalıdır:

      ![YAML](./media/sql-data-warehouse-continuous-integration-and-deployment/3-yaml-file.png "YAML")

Bu noktada, kaynak denetim deposu ana dalınızda yapılan herhangi bir iadenin veritabanı projenizin başarılı bir Visual Studio oluşturmasını otomatik olarak tetiklemesi gereken basit bir ortamınız vardır. Yerel veritabanı projenizde bir değişiklik yaparak ve ana dalınızda bu değişikliği denetleyerek otomasyonun uçuca çalıştığını doğrulayın.

## <a name="continuous-deployment-with-the-azure-sql-data-warehouse-or-database-deployment-task"></a>Azure SQL Veri Ambarı (veya Veritabanı) dağıtım göreviyle sürekli dağıtım

1. Azure SQL Veritabanı [dağıtım görevini](/devops/pipelines/tasks/deploy/sql-azure-dacpac-deployment?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) kullanarak yeni bir görev ekleyin ve hedef veri ambarınıza bağlanmak için gerekli alanları doldurun. Bu görev çalıştığında, önceki yapı işleminden oluşturulan DACPAC hedef veri ambarına dağıtılır. [Azure SQL Veri Ambarı dağıtım görevini](https://marketplace.visualstudio.com/items?itemName=ms-sql-dw.SQLDWDeployment)de kullanabilirsiniz.

      ![Dağıtım Görevi](./media/sql-data-warehouse-continuous-integration-and-deployment/4-deployment-task.png "Dağıtım Görevi")

2. Kendi kendine barındırılan bir aracı kullanıyorsanız, SQL Veri Ambarı için doğru SqlPackage.exe'yi kullanacak şekilde ortam değişkeninizi ayarladığınızdan emin olun. Yol şuna benzer:

      ![Çevre Değişkeni](./media/sql-data-warehouse-continuous-integration-and-deployment/5-environment-variable-preview.png "Çevre Değişkeni")

   C:\Program Dosyaları (x86)\Microsoft Visual Studio\2019\Preview\Common7\IDE\Extensions\Microsoft\SQLDB\DAC\150  

   Boru hattınızı çalıştırın ve doğrulayın. Yerel olarak değişiklik yapabilir ve kaynak denetiminizde otomatik bir yapı ve dağıtım oluşturması gereken değişiklikleri iade edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Synapse SQL havuzu MPP mimarisini](massively-parallel-processing-mpp-architecture.md) keşfedin
- Hızlı [bir](create-data-warehouse-portal.md) şekilde bir SQL havuzu oluşturun
- [Örnek verileri yükleme](load-data-from-azure-blob-storage-using-polybase.md)
- [Videoları](/azure/sql-data-warehouse/sql-data-warehouse-videos) Keşfedin
