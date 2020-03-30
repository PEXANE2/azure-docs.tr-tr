---
title: Azure İçe Alma/Dışa Aktarma iş durumunu gözden geçirme - v1 | Microsoft Dokümanlar
description: İçe Aktarma/Dışa Aktarma işinin durumunu görmek için içe aktarma veya dışa aktarma işi çalıştırıldığında oluşturulan günlük dosyalarını nasıl kullanacağınızı öğrenin.
author: twooley
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/26/2017
ms.author: twooley
ms.subservice: common
ms.openlocfilehash: 33e6ecca0eb4ca05b1f6bf53cdb3bd4ac8c8d8a9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75978447"
---
# <a name="reviewing-azure-importexport-job-status-with-copy-log-files"></a>Kopyalama günlüğü dosyalarıyla Azure İçe Alma/Dışa Aktarma iş durumunu gözden geçirme
Microsoft Azure İçe Alma/Dışa Aktarma hizmeti, bir içe aktarma veya dışa aktarma işiyle ilişkili sürücüleri işlediğinde, kopya günlüğü dosyalarını içe veya dışa aktardığınız depolama hesabına yazar. Günlük dosyası, dışa aktarılan veya dışa aktarılan her dosya hakkında ayrıntılı durum içerir. Tamamlanan bir işin durumunu sorguladığınız her kopya günlüğü dosyasının URL'si döndürülür; daha fazla bilgi için [İş bul'](https://docs.microsoft.com/rest/api/storageimportexport/Jobs/Get) a bakın.  

## <a name="example-urls"></a>Örnek URL'ler

Aşağıda, iki sürücülü bir alma işi için kopyalama günlüğü dosyaları için örnek URL'ler verilmiştir:  

 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM35C2V_20130921-034307-902_error.xml`  

 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM45A6Q_20130921-042122-021_error.xml`  

 Kopya günlüklerinin biçimi ve durum kodlarının tam listesi için [İçe/Dışa](../storage-import-export-file-format-log.md) Aktar hizmeti Günlük Dosya Biçimi'ne bakın.  

## <a name="next-steps"></a>Sonraki adımlar

 * [Azure İçe Alma/Dışa Aktarma Aracını Ayarlama](storage-import-export-tool-setup-v1.md)   
 * [Sabit sürücüleri içeri aktarma işine hazırlama](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
 * [Bir içeri aktarma işini onarma](../storage-import-export-tool-repairing-an-import-job-v1.md)   
 * [Bir dışarı aktarma işini onarma](../storage-import-export-tool-repairing-an-export-job-v1.md)   
 * [Azure İçeri/Dışarı Aktarma Aracı ile ilgili sorunları giderme](storage-import-export-tool-troubleshooting-v1.md)
