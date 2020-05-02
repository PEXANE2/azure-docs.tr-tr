---
title: SYNAPSE Studio 'Yu kullanma (Önizleme)
description: Bu hızlı başlangıçta, SYNAPSE Studio kullanarak çeşitli dosya türlerini sorgulamak için ne kadar kolay olduğunu göreceksiniz ve öğrenirsiniz.
services: synapse-analytics
author: jovanpop-msft
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/15/2020
ms.author: jovanpop
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 64895e5f48168305aea3e83596cdbcae982a6cb9
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82692417"
---
# <a name="quickstart-using-synapse-studio-preview"></a>Hızlı başlangıç: SYNAPSE Studio 'Yu kullanma (Önizleme)

Bu hızlı başlangıçta, SYNAPSE Studio kullanarak dosyaları sorgulamayı öğreneceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="prerequisites"></a>Ön koşullar

[Azure SYNAPSE çalışma alanı ve ilişkili depolama hesabı oluşturun](quickstart-create-workspace.md).

## <a name="launch-synapse-studio"></a>SYNAPSE Studio 'Yu Başlat

Azure portal Azure SYNAPSE çalışma alanınızda, **SYNAPSE Studio 'Yu Başlat**' a tıklayın.

![SYNAPSE Studio 'Yu Başlat](./media/quickstart-synapse-studio/launch-synapse-workspace.png)

Alternatif olarak, [Azure SYNAPSE Analytics](https://web.azuresynapse.net) ' i tıklatarak ve uygun kiracı, abonelik ve çalışma alanı değerlerini sağlayarak SYNAPSE Studio 'yu başlatabilirsiniz.

## <a name="browse-storage-accounts"></a>Depolama hesaplarına gözatamıyorum

SYNAPSE Studio 'Yu açtıktan sonra **verilere** gidin ve **depolama** hesabı ' nı genişleterek çalışma alanındaki depolama hesabını görüntüleyin.

![Depolamadaki dosyalara gözatamıyorum](./media/quickstart-synapse-studio/browse-files-on-storage.png)

Dosyalarınızı düzenlemek için araç çubuğundaki bağlantıları kullanarak yeni klasörler oluşturabilir ve dosyaları karşıya yükleyebilirsiniz.

## <a name="query-files-on-storage-account"></a>Depolama hesabındaki dosyaları sorgula

> [!IMPORTANT]
> Dosyaları sorgulayabilmeniz için temeldeki depolamada `Storage Blob Reader` rolün bir üyesi olmanız gerekir. [Azure depolama 'Da **Depolama Blobu veri okuyucusu** veya **Depolama Blobu veri katılımcısı** RBAC izinleri atama](../storage/common/storage-auth-aad-rbac-portal.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#assign-a-built-in-rbac-role)hakkında bilgi edinin.

1. Bazı `PARQUET` dosyaları karşıya yükleyin.
2. Bir veya daha fazla dosya seçin ve ardından dosyaların içeriğini görmek için yeni bir SQL betiği veya Spark Not defteri oluşturun. Bir not defteri oluşturmak isterseniz, [çalışma alanında Apache Spark havuzu](quickstart-create-apache-spark-pool.md)oluşturmanız gerekir.

   ![Depolamadaki dosyaları sorgula](./media/quickstart-synapse-studio/query-files-on-storage.png)

3. Dosya içeriğini görmek için oluşturulan sorguyu veya Not defterini çalıştırın.

   ![Dosya içeriğini gör](./media/quickstart-synapse-studio/query-files-on-storage-result.png)

4. Sonuçları filtrelemek ve sıralamak için sorguyu değiştirebilirsiniz. SQL [özelliklerine genel bakış](sql/overview-features.md)için Isteğe bağlı SQL 'de bulunan dil özelliklerini bulun.

## <a name="next-steps"></a>Sonraki adımlar

- Azure [depolama 'Da **Depolama Blobu veri okuyucusu** veya **Depolama Blobu veri katılımcısı** RBAC Izinleri ATAYARAK dosyaları sorgulamak için Azure AD kullanıcıları 'nı etkinleştirin](../storage/common/storage-auth-aad-rbac-portal.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#assign-a-built-in-rbac-role)
- [Azure depolama 'da Isteğe bağlı SQL kullanarak dosyaları sorgulama](sql/on-demand-workspace-overview.md)
- [Azure portal kullanarak Apache Spark havuzu oluşturma](quickstart-create-apache-spark-pool.md)
- [Azure depolama 'da depolanan dosyalar üzerinde Power BI raporu oluşturma](sql/tutorial-connect-power-bi-desktop.md)
