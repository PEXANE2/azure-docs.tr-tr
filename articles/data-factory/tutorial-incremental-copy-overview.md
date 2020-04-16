---
title: Verileri artımlı olarak kopyalayarak
description: Bu öğreticiler verilerin, kaynak veri deposundan hedef veri deposuna nasıl artımlı olarak kopyalanacağını gösterir. İlki bir tablodan veri kopyalar.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
manager: anandsub
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: tutorial
ms.custom: seo-lt-2019
ms.date: 01/22/2018
ms.openlocfilehash: 00d54f9adcab93152881852b6fcac417e9d7c7d9
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81409913"
---
# <a name="incrementally-load-data-from-a-source-data-store-to-a-destination-data-store"></a>Bir kaynak veri deposundan hedef veri deposuna artımlı olarak veri yükleme

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

İlk veriler yüklendikten sonra verileri artımlı olarak (veya delta) yükleme senaryosu, veri tümleştirme çözümlerinde sıkça kullanılır. Bu bölümdeki öğreticiler, Azure Data Factory kullanarak artımlı veri yüklemenin farklı yollarını gösterir.

## <a name="delta-data-loading-from-database-by-using-a-watermark"></a>Filigran kullanarak veritabanından delta veri yükleme
Bu durumda, kaynak veritabanında bir filigran tanımlayın. Filigran, en son güncelleştirilen zaman damgası veya artan bir anahtarı olan bir sütundur. Delta yükleme çözümü, eski bir filigran ile yeni bir filigran arasında değiştirilen verileri yükler. Bu yaklaşıma yönelik iş akışı şu diyagramda gösterilmiştir: 

![Filigran kullanmaya yönelik iş akışı](media/tutorial-incremental-copy-overview/workflow-using-watermark.png)

Adım adım yönergeler için şu öğreticilere bakın: 
- [Azure SQL Veritabanı’ndaki bir tablodan Azure Blob depolama Alanı’na artımlı olarak veri kopyalama](tutorial-incremental-copy-powershell.md)
- [Şirket içi SQL Server’daki birden fazla tablodan Azure SQL Veritabanı’na artımlı olarak veri kopyalama](tutorial-incremental-copy-multiple-tables-powershell.md)

Şablonlar için aşağıdakilere bakın:
- [Denetim tablosuyla delta kopyalama](solution-template-delta-copy-with-control-table.md)

## <a name="delta-data-loading-from-sql-db-by-using-the-change-tracking-technology"></a>Değişim İzleme teknolojisini kullanarak SQL DB'den Delta veri yüklemesi
Değişiklik İzleme teknolojisi, SQL Server ve Azure SQL Veritabanı’nda bulunan, uygulamalar için verimli bir değişiklik izleme mekanizması sağlayan basit bir çözümdür. Bir uygulamanın eklenen, güncelleştirilen veya silinen verileri kolayca tanımlamasına olanak sağlar. 

Bu yaklaşıma yönelik iş akışı şu diyagramda gösterilmiştir:

![Değişiklik İzleme kullanmaya yönelik iş akışı](media/tutorial-incremental-copy-overview/workflow-using-change-tracking.png)

Adım adım yönergeler için şu öğreticilere bakın: <br/>
- [Değişiklik İzleme teknolojisini kullanarak Azure SQL Veritabanı’ndan Azure Blob depolama alanına artımlı olarak veri kopyalama](tutorial-incremental-copy-change-tracking-feature-powershell.md)

## <a name="loading-new-and-changed-files-only-by-using-lastmodifieddate"></a>Yalnızca LastModifiedDate kullanarak yeni ve değiştirilmiş dosyaları yükleme
Yeni ve değiştirilen dosyaları yalnızca LastModifiedDate'i kullanarak hedef mağazaya kopyalayabilirsiniz. ADF kaynak mağazadaki tüm dosyaları tarayıp, dosya filtresini LastModifiedDate'lerine göre uygular ve yalnızca son kez yeni ve güncelleştirilmiş dosyayı hedef mağazaya kopyalar.  ADF'nin büyük miktarda dosyayı taramasına izin verirseniz, ancak yalnızca birkaç dosyayı hedefe kopyalamanıza izin verirseniz, dosya taraması nedeniyle uzun sürenin de zaman alıcı olmasını beklersiniz.   

Adım adım yönergeler için şu öğreticilere bakın: <br/>
- [Azure Blob depolama alanından Azure Blob depolama alanına LastModifiedDate'i temel alan yeni ve değiştirilmiş dosyaları aşamalı olarak kopyalayın](tutorial-incremental-copy-lastmodified-copy-data-tool.md)

Şablonlar için aşağıdakilere bakın:
- [LastModifiedDate parametresine göre yeni dosyaları kopyalama](solution-template-copy-new-files-lastmodifieddate.md)

## <a name="loading-new-files-only-by-using-time-partitioned-folder-or-file-name"></a>Yeni dosyaları yalnızca zaman bölümlenmiş klasör veya dosya adı kullanarak yükleme.
Yalnızca, dosya veya klasörlerin dosya veya klasör adının bir parçası olarak zaman dilimi bilgileriyle zaman dilimi bölümlerine eklendiği (örneğin, /yyyy/mm/dd/file.csv) yeni dosyaları kopyalayabilirsiniz. Yeni dosyaları aşamalı olarak yüklemek için en önemli yaklaşımdır. 

Adım adım yönergeler için şu öğreticilere bakın: <br/>
- [Azure Blob depolamadan Azure Blob depolamasına zaman bölümlenmiş klasörveya dosya adına dayalı olarak yeni dosyaları aşamalı olarak kopyalayın](tutorial-incremental-copy-partitioned-file-name-copy-data-tool.md)

## <a name="next-steps"></a>Sonraki adımlar
Şu öğreticiye ilerleyin: 

> [!div class="nextstepaction"]
>[Azure SQL Veritabanı’ndaki bir tablodan Azure Blob depolama Alanı’na artımlı olarak veri kopyalama](tutorial-incremental-copy-powershell.md)
