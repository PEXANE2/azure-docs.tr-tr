---
title: 'Incrementally copy data by using Azure Data Factory '
description: Bu öğreticiler verilerin, kaynak veri deposundan hedef veri deposuna nasıl artımlı olarak kopyalanacağını gösterir. İlki bir tablodan veri kopyalar.
services: data-factory
documentationcenter: ''
author: dearandyxu
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: tutorial
ms.date: 01/22/2018
ms.author: yexu
ms.openlocfilehash: 42dcd6ecc6df1c9877581d89bf22724054e305d0
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74217280"
---
# <a name="incrementally-load-data-from-a-source-data-store-to-a-destination-data-store"></a>Bir kaynak veri deposundan hedef veri deposuna artımlı olarak veri yükleme

İlk veriler yüklendikten sonra verileri artımlı olarak (veya delta) yükleme senaryosu, veri tümleştirme çözümlerinde sıkça kullanılır. Bu bölümdeki öğreticiler, Azure Data Factory kullanarak artımlı veri yüklemenin farklı yollarını gösterir.

## <a name="delta-data-loading-from-database-by-using-a-watermark"></a>Delta data loading from database by using a watermark
Bu durumda, kaynak veritabanında bir filigran tanımlayın. Filigran, en son güncelleştirilen zaman damgası veya artan bir anahtarı olan bir sütundur. Delta yükleme çözümü, eski bir filigran ile yeni bir filigran arasında değiştirilen verileri yükler. Bu yaklaşıma yönelik iş akışı şu diyagramda gösterilmiştir: 

![Filigran kullanmaya yönelik iş akışı](media/tutorial-incremental-copy-overview/workflow-using-watermark.png)

Adım adım yönergeler için şu öğreticilere bakın: 
- [Azure SQL Veritabanı’ndaki bir tablodan Azure Blob depolama Alanı’na artımlı olarak veri kopyalama](tutorial-incremental-copy-powershell.md)
- [Şirket içi SQL Server’daki birden fazla tablodan Azure SQL Veritabanı’na artımlı olarak veri kopyalama](tutorial-incremental-copy-multiple-tables-powershell.md)

For templates, see the following:
- [Delta copy with control table](solution-template-delta-copy-with-control-table.md)

## <a name="delta-data-loading-from-sql-db-by-using-the-change-tracking-technology"></a>Delta data loading from SQL DB by using the Change Tracking technology
Değişiklik İzleme teknolojisi, SQL Server ve Azure SQL Veritabanı’nda bulunan, uygulamalar için verimli bir değişiklik izleme mekanizması sağlayan basit bir çözümdür. Bir uygulamanın eklenen, güncelleştirilen veya silinen verileri kolayca tanımlamasına olanak sağlar. 

Bu yaklaşıma yönelik iş akışı şu diyagramda gösterilmiştir:

![Değişiklik İzleme kullanmaya yönelik iş akışı](media/tutorial-incremental-copy-overview/workflow-using-change-tracking.png)

Adım adım yönergeler için şu öğreticilere bakın: <br/>
- [Değişiklik İzleme teknolojisini kullanarak Azure SQL Veritabanı’ndan Azure Blob depolama alanına artımlı olarak veri kopyalama](tutorial-incremental-copy-change-tracking-feature-powershell.md)

## <a name="loading-new-and-changed-files-only-by-using-lastmodifieddate"></a>Loading new and changed files only by using LastModifiedDate
You can copy the new and changed files only by using LastModifiedDate to the destination store. ADF will scan all the files from the source store, apply the file filter by their LastModifiedDate, and only copy the new and updated file since last time to the destination store.  Please be aware if you let ADF scan huge amounts of files but only copy a few files to destination, you would still expect the long duration due to file scanning is time consuming as well.   

Adım adım yönergeler için şu öğreticilere bakın: <br/>
- [Incrementally copy new and changed files based on LastModifiedDate from Azure Blob storage to Azure Blob storage](tutorial-incremental-copy-lastmodified-copy-data-tool.md)

For templates, see the following:
- [Copy new files by LastModifiedDate](solution-template-copy-new-files-lastmodifieddate.md)

## <a name="loading-new-files-only-by-using-time-partitioned-folder-or-file-name"></a>Loading new files only by using time partitioned folder or file name.
You can copy new files only, where files or folders has already been time partitioned with timeslice information as part of the file or folder name (for example, /yyyy/mm/dd/file.csv). It is the most performant approach for incrementally loading new files. 

Adım adım yönergeler için şu öğreticilere bakın: <br/>
- [Incrementally copy new files based on time partitioned folder or file name from Azure Blob storage to Azure Blob storage](tutorial-incremental-copy-partitioned-file-name-copy-data-tool.md)

## <a name="next-steps"></a>Sonraki adımlar
Şu öğreticiye ilerleyin: 

> [!div class="nextstepaction"]
>[Azure SQL Veritabanı’ndaki bir tablodan Azure Blob depolama Alanı’na artımlı olarak veri kopyalama](tutorial-incremental-copy-powershell.md)
