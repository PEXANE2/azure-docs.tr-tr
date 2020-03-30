---
title: Azure Veri Fabrikası'nda desteklenen dosya biçimleri
description: Bu konu, Azure Veri Fabrikası'ndaki dosya tabanlı bağlayıcılar tarafından desteklenen dosya biçimlerini ve sıkıştırma kodlarını açıklar.
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 12/10/2019
ms.author: jingwang
ms.openlocfilehash: 6855eea5939419c9a0a867de4e0621b4d4ae02b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75439564"
---
# <a name="supported-file-formats-and-compression-codecs-in-azure-data-factory"></a>Azure Veri Fabrikası'nda desteklenen dosya biçimleri ve sıkıştırma kodlayıcıları

*Bu makale aşağıdaki bağlayıcılar için geçerlidir: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Veri Gölü Depolama Gen1](connector-azure-data-lake-store.md), [Azure Veri Gölü Depolama Gen2](connector-azure-data-lake-storage.md), [Azure Dosya Depolama](connector-azure-file-storage.md), Dosya [Sistemi](connector-file-system.md), [FTP](connector-ftp.md), [Google Bulut Depolama](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [HTTP](connector-http.md), ve [SFTP](connector-sftp.md).*

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Dosyaları iki dosya tabanlı veri deposu arasında olduğu gibi kopyalamak için [Kopyala etkinliğini](copy-activity-overview.md) kullanabilirsiniz ve bu durumda veriler herhangi bir serileştirme veya deserialization olmadan verimli bir şekilde kopyalanır. 

Buna ek olarak, belirli bir biçimdeki dosyaları ayrıştabilir veya oluşturabilirsiniz. Örneğin, aşağıdakileri gerçekleştirebilirsiniz:

* Şirket içi BIR SQL Server veritabanındaki verileri kopyalayın ve Parke formatında Azure Veri Gölü Depolama Gen2'ye yazın.
* Dosyaları şirket içi bir dosya sisteminden metin (CSV) biçiminde kopyalayın ve Avro biçiminde Azure Blob depolamasına yazın.
* Sıkıştırılmış dosyaları şirket içi bir dosya sisteminden kopyalayın, anında sıkıştırmayı debzorlayın ve çıkarılan dosyaları Azure Veri Gölü Depolama Gen2'ye yazın.
* Verileri Azure Blob depolamaalanından Gzip sıkıştırılmış metin (CSV) biçiminde kopyalayın ve Azure SQL Veritabanı'na yazın.
* Serileştirme/deserialization veya sıkıştırma/dekompresyon gerektiren daha birçok etkinlik.

## <a name="next-steps"></a>Sonraki adımlar

Diğer Kopyalama Etkinliği makalelerini görün:

- [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)
- [Etkinlik performansını kopyalama](copy-activity-performance.md)
