---
title: Azure Data Factory 'de desteklenen dosya biçimleri
description: Bu konuda dosya biçimlerini ve dosya tabanlı bağlayıcı Azure Data Factory tarafından desteklenen bir sıkıştırma kodları açıklanmaktadır.
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 12/10/2019
ms.author: jingwang
ms.openlocfilehash: 6855eea5939419c9a0a867de4e0621b4d4ae02b9
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75439564"
---
# <a name="supported-file-formats-and-compression-codecs-in-azure-data-factory"></a>Desteklenen dosya biçimleri ve Azure Data factory'de sıkıştırma codec bileşenleri

*Bu makale aşağıdaki bağlayıcılar için geçerlidir: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Data Lake Storage 1.](connector-azure-data-lake-store.md), [Azure Data Lake Storage 2.](connector-azure-data-lake-storage.md), [Azure dosya depolama](connector-azure-file-storage.md), [dosya sistemi](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md),, [](connector-hdfs.md), [http](connector-http.md)ve [SFTP](connector-sftp.md).*

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

[Kopyalama etkinliğini](copy-activity-overview.md) , dosyaları iki dosya tabanlı veri deposu arasında olduğu gibi kopyalamak için kullanabilirsiniz. Bu durumda, veriler serileştirme veya seri durumundan çıkarma yapılmadan verilerin verimli bir şekilde kopyalanabilmesi. 

Ayrıca, belirli bir biçimin dosyalarını ayrıştırır veya oluşturabilirsiniz. Örneğin, şunları yapabilirsiniz:

* Şirket içi SQL Server veritabanından veri kopyalayın ve Parquet biçiminde Azure Data Lake Storage 2. yazın.
* Metin (CSV) biçimindeki dosyaları şirket içi bir dosya sisteminden kopyalayın ve avro biçiminde Azure Blob depolama alanına yazın.
* ZIP dosyalarını şirket içi bir dosya sisteminden kopyalayın, açık olarak açıp Azure Data Lake Storage 2. ve ayıklanan dosyaları yazın.
* Verileri Azure Blob depolama alanından gzip sıkıştırılmış metin (CSV) biçiminde kopyalayın ve Azure SQL veritabanı 'na yazın.
* Serileştirme/seri durumdan çıkarma veya sıkıştırma/sıkıştırmayı gerektiren çok sayıda etkinlik.

## <a name="next-steps"></a>Sonraki adımlar

Bir kopyalama etkinliği makalelere bakın:

- [Kopyalama etkinliği'ne genel bakış](copy-activity-overview.md)
- [Etkinlik performansını Kopyala](copy-activity-performance.md)
