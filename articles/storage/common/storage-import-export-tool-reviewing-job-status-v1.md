---
title: Azure Içeri/dışarı aktarma iş durumunu gözden geçirme-v1 | Microsoft Docs
description: İçeri/dışarı aktarma işinin durumunu görmek için içeri veya dışarı aktarma işi çalıştırıldığında oluşturulan günlük dosyalarını nasıl kullanacağınızı öğrenin.
author: twooley
services: storage
ms.service: storage
ms.topic: how-to
ms.date: 01/26/2017
ms.author: twooley
ms.subservice: common
ms.openlocfilehash: 5ab0dd7f0e0ed90c205c37ddbb7ea3a9fca74ae9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85512263"
---
# <a name="reviewing-azure-importexport-job-status-with-copy-log-files"></a>Azure Içeri/dışarı aktarma iş durumunu kopyalama günlük dosyalarıyla gözden geçirme
Microsoft Azure İçeri/Dışarı Aktarma hizmeti bir içeri veya dışarı aktarma işiyle ilişkili sürücüleri işlediğinde, kopyalama günlük dosyalarını depolama hesabına, blob 'ları içeri veya dışarı aktarma yaptığınız veya dışarı aktardığınız depolama hesabına yazar. Günlük dosyası, içeri aktarılan veya aktarılan her dosya hakkında ayrıntılı durum içerir. Tamamlanan bir işin durumunu sorguladığınızda her bir kopya günlük dosyasının URL 'SI döndürülür; daha fazla bilgi için bkz. [Iş al](https://docs.microsoft.com/rest/api/storageimportexport/Jobs/Get) .  

## <a name="example-urls"></a>Örnek URL 'Ler

Aşağıda, iki sürücü içeren bir içeri aktarma işine ait günlük dosyalarının kopyalanması için örnek URL 'Ler verilmiştir:  

 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM35C2V_20130921-034307-902_error.xml`  

 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM45A6Q_20130921-042122-021_error.xml`  

 Kopya günlüklerinin biçimi ve durum kodlarının tam listesi için bkz. [içeri/dışarı aktarma hizmeti günlük dosyası biçimi](../storage-import-export-file-format-log.md) .  

## <a name="next-steps"></a>Sonraki adımlar

 * [Azure Içeri/dışarı aktarma aracı 'nı ayarlama](storage-import-export-tool-setup-v1.md)   
 * [Sabit sürücüleri içeri aktarma işine hazırlama](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
 * [Bir içeri aktarma işini onarma](../storage-import-export-tool-repairing-an-import-job-v1.md)   
 * [Bir dışarı aktarma işini onarma](../storage-import-export-tool-repairing-an-export-job-v1.md)   
 * [Azure İçeri/Dışarı Aktarma Aracı ile ilgili sorunları giderme](storage-import-export-tool-troubleshooting-v1.md)
