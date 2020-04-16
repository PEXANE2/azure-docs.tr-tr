---
title: Synapse Studio'u kullanma (önizleme)
description: Bu hızlı başlangıçta, Synapse Studio'u kullanarak çeşitli dosya türlerini sorgulamanın ne kadar kolay olduğunu görecek ve öğreneceksiniz.
services: synapse-analytics
author: jovanpop-msft
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/15/2020
ms.author: jovanpop
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 294f53fe929343708bdbb9564b23c46865cf02ac
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81423869"
---
# <a name="quickstart-using-synapse-studio-preview"></a>Quickstart: Synapse Studio kullanma (önizleme)

Bu hızlı başlangıçta, Synapse Studio'u kullanarak dosyaları nasıl sorgulayabileceğinizi öğreneceksiniz.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="prerequisites"></a>Ön koşullar

[Azure Synapse çalışma alanı ve ilişkili depolama hesabı oluşturun.](quickstart-create-workspace.md)

## <a name="launch-synapse-studio"></a>Synapse Studio'u başlatın

Azure portalındaki Azure Synapse çalışma alanınızda **Synapse Studio'yu Başlat'ı**tıklatın.

![Synapse Studio'u başlatın](./media/quickstart-synapse-studio/launch-synapse-workspace.png)

Alternatif olarak, [Azure Synapse Analytics'i](https://web.azuresynapse.net) tıklatarak ve uygun kiracı, abonelik ve çalışma alanı değerlerini sağlayarak Synapse Studio'yu başlatabilirsiniz.

## <a name="browse-storage-accounts"></a>Depolama hesaplarına göz atın

Synapse Studio'yu açtıktan **sonra, Çalışma** alanındaki depolama hesabını görüntülemek için Data'ya göz atın ve ardından **Depolama hesaplarını** genişletin.

![Depolama yla ilgili dosyalara göz atın](./media/quickstart-synapse-studio/browse-files-on-storage.png)

Dosyalarınızı düzenlemek için araç çubuğundaki bağlantıları kullanarak yeni klasörler oluşturabilir ve dosya yükleyebilirsiniz.

## <a name="query-files-on-storage-account"></a>Depolama hesabındaki dosyaları sorgula

> [!IMPORTANT]
> Dosyaları sorgulayabilmek için `Storage Blob Reader` temel depolama alanında rolün bir üyesi olmanız gerekir. [Azure **Depolama'da Depolama Blob Veri Okuyucu** veya Depolama **Blob Veri Katılımcısı** RBAC izinlerini](../storage/common/storage-auth-aad-rbac-portal.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#assign-a-built-in-rbac-role)nasıl atayacağımöğrenin.

1. Bazı `PARQUET` dosyaları yükleyin.
2. Bir veya daha fazla dosya seçin ve dosyaların içeriğini görmek için yeni bir SQL komut dosyası veya Spark not defteri oluşturun. Bir not defteri oluşturmak istiyorsanız, çalışma [alanında Apache Spark havuzu](spark/apache-spark-notebook-create-spark-use-sql.md)oluşturmanız gerekir.

   ![Depolama daki dosyaları sorgula](./media/quickstart-synapse-studio/query-files-on-storage.png)

3. Dosyanın içeriğini görmek için oluşturulan sorguyu veya not defterini çalıştırın:

   ![Dosyanın içeriğini görme](./media/quickstart-synapse-studio/query-files-on-storage-result.png)

4. Sorguyu filtrelemek ve sonuçları sıralamak için değiştirebilirsiniz. [SQL özelliklerine genel bakışta](sql/overview-features.md)isteğe bağlı olarak kullanılabilir dil özelliklerini bulun.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Depolama'da Depolama **Blob Veri Okuyucusu** veya **Depolama Blob Veri Katılımcısı** RBAC izinleri atayarak](../storage/common/storage-auth-aad-rbac-portal.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#assign-a-built-in-rbac-role) Azure AD kullanıcılarının dosyaları sorgulamasını sağlama
- [SQL İsteğe Bağlı kullanarak Azure Depolama'daki dosyaları sorgula](sql/on-demand-workspace-overview.md)
- [Apache Spark havuzu oluşturma](spark/apache-spark-notebook-create-spark-use-sql.md)
- [Azure Depolama'da depolanan dosyalar hakkında Power BI raporu oluşturma](sql/tutorial-connect-power-bi-desktop.md)
