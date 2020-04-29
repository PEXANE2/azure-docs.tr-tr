---
title: Azure Data Factory 'de desteklenen dosya biçimleri
description: Bu konu, Azure Data Factory dosya tabanlı bağlayıcılar tarafından desteklenen dosya biçimlerini ve sıkıştırma kodlarını açıklamaktadır.
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 12/10/2019
ms.author: jingwang
ms.openlocfilehash: dbcfad3dd3db9f5c9431e07d85d77a77a10283c4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81419043"
---
# <a name="supported-file-formats-and-compression-codecs-in-azure-data-factory"></a>Azure Data Factory 'de desteklenen dosya biçimleri ve sıkıştırma codec bileşenleri
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

*Bu makale aşağıdaki bağlayıcılar için geçerlidir: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Data Lake Storage 1.](connector-azure-data-lake-store.md), [Azure Data Lake Storage 2.](connector-azure-data-lake-storage.md), [Azure dosya depolama](connector-azure-file-storage.md), [dosya sistemi](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md),, [HDFS](connector-hdfs.md), [http](connector-http.md)ve [SFTP](connector-sftp.md).*

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

[Kopyalama etkinliğini](copy-activity-overview.md) , dosyaları iki dosya tabanlı veri deposu arasında olduğu gibi kopyalamak için kullanabilirsiniz. Bu durumda, veriler serileştirme veya seri durumundan çıkarma yapılmadan verilerin verimli bir şekilde kopyalanabilmesi. 

Ayrıca, belirli bir biçimin dosyalarını ayrıştırır veya oluşturabilirsiniz. Örneğin, şunları yapabilirsiniz:

* Şirket içi SQL Server veritabanından veri kopyalayın ve Parquet biçiminde Azure Data Lake Storage 2. yazın.
* Metin (CSV) biçimindeki dosyaları şirket içi bir dosya sisteminden kopyalayın ve avro biçiminde Azure Blob depolama alanına yazın.
* ZIP dosyalarını şirket içi bir dosya sisteminden kopyalayın, açık olarak açıp Azure Data Lake Storage 2. ve ayıklanan dosyaları yazın.
* Verileri Azure Blob depolama alanından gzip sıkıştırılmış metin (CSV) biçiminde kopyalayın ve Azure SQL veritabanı 'na yazın.
* Serileştirme/seri durumdan çıkarma veya sıkıştırma/sıkıştırmayı gerektiren çok sayıda etkinlik.

## <a name="next-steps"></a>Sonraki adımlar

Diğer kopyalama etkinliği makalelerine bakın:

- [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)
- [Kopyalama etkinliği performansı](copy-activity-performance.md)
