---
title: Linux için Azure NetApp Files performans değerlendirmeleri | Microsoft Docs
description: Linux için Azure NetApp Files performans kıyaslamaları açıklanmaktadır.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/29/2020
ms.author: b-juche
ms.openlocfilehash: b763a734866dd5fed5bf0500d4d52b9324c92a79
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82614596"
---
# <a name="azure-netapp-files-performance-benchmarks-for-linux"></a>Linux için Azure NetApp Files performans değerlendirmeleri

Bu makalede, Linux için Azure NetApp Files performans kıyaslamaları açıklanmaktadır.

## <a name="linux-scale-out"></a>Linux genişleme

Bu bölümde, Linux iş yükü işleme ve iş yükü ıOPS 'nin performans kıyaslamaları açıklanmaktadır.

### <a name="linux-workload-throughput"></a>Linux iş yükü işleme  

Aşağıdaki grafik, 64-kibibyte (KiB) sıralı iş yükünü ve 1-TiB çalışma kümesini temsil eder. Tek bir Azure NetApp Files biriminin ~ 1.600 MiB/s saf sıralı yazma işlemleri ile ~ 4.500 MiB/s saf sıralı okumaları arasında işleyebileceğini gösterir.  

Grafik, saf okumayı saf yazma 'dan bir kerede %10 oranında azaltır. Değişen okuma/yazma oranları (%100: %0, 90%: %10, %80: %20, vb.) kullanılırken ne beklendiğini gösterir.

![Linux iş yükü işleme](../media/azure-netapp-files/performance-benchmarks-linux-workload-throughput.png)  

### <a name="linux-workload-iops"></a>Linux iş yükü ıOPS  

Aşağıdaki grafik 4-kibibyte (KiB) rastgele iş yükünü ve 1-TiB çalışma kümesini temsil eder. Grafik, Azure NetApp Files bir birimin ~ 130.000 saf rastgele yazma ve ~ 460.000 saf rastgele okuma arasında işleyebileceğini gösterir.  

Bu grafik, saf okumayı saf yazma 'dan bir kerede %10 oranında azaltır. Değişen okuma/yazma oranları (%100: %0, 90%: %10, %80: %20, vb.) kullanılırken ne beklendiğini gösterir.

![Linux iş yükü ıOPS](../media/azure-netapp-files/performance-benchmarks-linux-workload-iops.png)  

## <a name="linux-scale-up"></a>Linux ölçeği artırma  

Linux 5,3 çekirdeği, NFS için tek istemci genişleme ağını mümkün bir şekilde sunar `nconnect` . Bu bölümdeki grafikler, NFSv3 ile istemci tarafı bağlama seçeneğinin doğrulama testi sonuçlarını gösterir. Bu özellik SUSE (SLES12SP4 ile başlayan) ve Ubuntu (19,10 sürümü ile başlayarak) için kullanılabilir. Hem çok kanallı SMB hem de Oracle doğrudan NFS kavramıyla benzerdir.

Grafikler, ' nin avantajlarını `nconnect` bağlı olmayan bağlanmış bir birime karşılaştırır. Grafiklerde, FIO, US-west2 Azure bölgesindeki tek bir D32s_v3 örneğinden iş yükünü oluşturdu.

### <a name="linux-read-throughput"></a>Linux okuma performansı  

Aşağıdaki grafiklerde, yaklaşık 3.500 MIB/s ile okunan `nconnect` , kabaca 2.3 x olan sıralı okumaları gösterilmektedir `nconnect` .

![Linux okuma performansı](../media/azure-netapp-files/performance-benchmarks-linux-read-throughput.png)  

### <a name="linux-write-throughput"></a>Linux yazma performansı  

Aşağıdaki grafiklerde sıralı yazma işlemleri gösterilmektedir. Bunlar `nconnect` , sıralı yazma işlemleri için dikkat çekici bir avantajın olmadığını gösterir. 1.500 MIB/s kabaca hem sıralı yazma birimi üst sınırı hem de D32s_v3 örneği çıkış limiti.

![Linux yazma performansı](../media/azure-netapp-files/performance-benchmarks-linux-write-throughput.png)  

### <a name="linux-read-iops"></a>Linux okuma ıOPS 'si  

Aşağıdaki grafiklerde `nconnect` , kabaca 3x olmayan, yaklaşık olarak yaklaşık 200.000 Okuma IOPS 'nin rastgele okumaları gösterilmektedir `nconnect` .

![Linux okuma ıOPS 'si](../media/azure-netapp-files/performance-benchmarks-linux-read-iops.png)  

### <a name="linux-write-iops"></a>Linux yazma ıOPS  

Aşağıdaki grafiklerde `nconnect` , kabaca 3x olmayan, yaklaşık 135.000 Yazma IOPS 'nin rastgele yazmaları gösterilmektedir `nconnect` .

![Linux yazma ıOPS](../media/azure-netapp-files/performance-benchmarks-linux-write-iops.png)  

## <a name="next-steps"></a>Sonraki adımlar

- [Azure NetApp Files: bulut Depolamalarınızdan En Iyi şekilde yararlanmak](https://cloud.netapp.com/hubfs/Resources/ANF%20PERFORMANCE%20TESTING%20IN%20TEMPLATE.pdf?hsCtaTracking=f2f560e9-9d13-4814-852d-cfc9bf736c6a%7C764e9d9c-9e6b-4549-97ec-af930247f22f)
