---
title: "Hızlı başlangıç: SYNAPSE Studio 'Yu kullanma"
description: Bu hızlı başlangıçta, SYNAPSE Studio kullanarak çeşitli dosya türlerini sorgulamak için ne kadar kolay olduğunu göreceksiniz ve öğrenirsiniz.
services: synapse-analytics
author: jovanpop-msft
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/15/2020
ms.author: jovanpop
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 271fd77e519ce9d78e2fa4e4837004a77cf73522
ms.sourcegitcommit: 0b2367b4a9171cac4a706ae9f516e108e25db30c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86276065"
---
# <a name="quickstart-use-synapse-studio-preview"></a>Hızlı başlangıç: SYNAPSE Studio 'Yu kullanma (Önizleme)

Bu hızlı başlangıçta, SYNAPSE Studio kullanarak dosyaları sorgulamayı öğreneceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/) oturum açın.

## <a name="prerequisites"></a>Önkoşullar

[Azure SYNAPSE çalışma alanı ve ilişkili depolama hesabı oluşturun](quickstart-create-workspace.md).

## <a name="launch-synapse-studio"></a>Synapse Studio'yu başlatma

Azure portal Azure SYNAPSE çalışma alanınızda, **SYNAPSE Studio 'Yu Başlat**' a tıklayın.

![Synapse Studio'yu başlatma](./media/quickstart-synapse-studio/launch-synapse-workspace.png)

Alternatif olarak, [Azure SYNAPSE Analytics](https://web.azuresynapse.net) ' i tıklatarak ve uygun kiracı, abonelik ve çalışma alanı değerlerini sağlayarak SYNAPSE Studio 'yu başlatabilirsiniz.

## <a name="browse-storage-accounts"></a>Depolama hesaplarına gözatamıyorum

SYNAPSE Studio 'Yu açtıktan sonra **verilere** gidin ve **depolama** hesabı ' nı genişleterek çalışma alanındaki depolama hesabını görüntüleyin.

![Depolamadaki dosyalara gözatamıyorum](./media/quickstart-synapse-studio/browse-files-on-storage.png)

Dosyalarınızı düzenlemek için araç çubuğundaki bağlantıları kullanarak yeni klasörler oluşturabilir ve dosyaları karşıya yükleyebilirsiniz.

## <a name="query-files-on-storage-account"></a>Depolama hesabındaki dosyaları sorgula

> [!IMPORTANT]
> `Storage Blob Reader`Dosyaları sorgulayabilmeniz için temeldeki depolamada rolün bir üyesi olmanız gerekir. [Azure depolama 'Da **Depolama Blobu veri okuyucusu** veya **Depolama Blobu veri katılımcısı** RBAC izinleri atama](../storage/common/storage-auth-aad-rbac-portal.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#assign-a-built-in-rbac-role)hakkında bilgi edinin.

1. Bazı dosyaları karşıya yükleyin `PARQUET` .
2. Bir veya daha fazla dosya seçin ve ardından dosyaların içeriğini görmek için yeni bir SQL betiği veya Spark Not defteri oluşturun. Bir not defteri oluşturmak istiyorsanız [SYNAPSE çalışma alanlarında bir Apache Spark havuzu](quickstart-create-apache-spark-pool-studio.md)oluşturmanız gerekir.

   ![Depolamadaki dosyaları sorgula](./media/quickstart-synapse-studio/query-files-on-storage.png)

3. Dosya içeriğini görmek için oluşturulan sorguyu veya Not defterini çalıştırın.

   ![Dosya içeriğini gör](./media/quickstart-synapse-studio/query-files-on-storage-result.png)

4. Sonuçları filtrelemek ve sıralamak için sorguyu değiştirebilirsiniz. SQL [özelliklerine genel bakış](sql/overview-features.md)için Isteğe bağlı SQL 'de bulunan dil özelliklerini bulun.

## <a name="next-steps"></a>Sonraki adımlar

- Azure [depolama 'Da **Depolama Blobu veri okuyucusu** veya **Depolama Blobu veri katılımcısı** RBAC Izinleri ATAYARAK](../storage/common/storage-auth-aad-rbac-portal.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#assign-a-built-in-rbac-role) dosyaları sorgulamak için Azure AD kullanıcıları 'nı etkinleştirin
- [Azure depolama 'da Isteğe bağlı SQL kullanarak dosyaları sorgulama](sql/on-demand-workspace-overview.md)
- [Azure portalını kullanarak Apache Spark havuzu oluşturma](quickstart-create-apache-spark-pool-portal.md)
- [Azure depolama 'da depolanan dosyalar üzerinde Power BI raporu oluşturma](sql/tutorial-connect-power-bi-desktop.md)
