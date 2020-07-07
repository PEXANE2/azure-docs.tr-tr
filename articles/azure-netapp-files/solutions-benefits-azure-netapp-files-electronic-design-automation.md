---
title: Elektronik tasarım otomasyonu için Azure NetApp Files kullanmanın avantajları | Microsoft Docs
description: Azure NetApp Files, yarı iletken ve yonga tasarımı sektörünün ihtiyaçlarını karşılamak için sağladığı çözümü açıklar. Azure NetApp Files kullanarak elektronik tasarım otomasyonu (EDA) için standart bir sektör kıyaslaması çalıştıran test senaryolarını gösterir.
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
ms.date: 04/24/2020
ms.author: b-juche
ms.openlocfilehash: fcede16619e8488796adc6f4c60af30643c1aadf
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82160162"
---
# <a name="benefits-of-using-azure-netapp-files-for-electronic-design-automation"></a>Elektronik tasarım otomasyonu için Azure NetApp Files kullanmanın avantajları

Pazar süresi (TTD), yarı iletken ve yonga tasarımı sektöründe önemli bir konudur. Sektörün depolama için yüksek bant genişliğine ve düşük gecikme süresine sahip olması gerekir. Bu makalede sektörün ihtiyaçlarını karşılamak için Azure NetApp Files sağladığı çözüm açıklanmaktadır. Azure NetApp Files kullanarak elektronik tasarım otomasyonu (EDA) için standart bir sektör kıyaslaması çalıştıran test senaryoları sunar. 

## <a name="test-scenario-configurations"></a>Test senaryosu yapılandırması

Testler aşağıdaki yapılandırmalara sahip üç senaryo içerir. 

|    Senaryo    |    Birimler    |    İstemciler<br> SLES15 D16s_v3  |
|----------------|---------------|--------------------------------|
|    Bir         |    1          |    1                           |
|    İki         |    6          |    24                          |
|    Üç       |    12         |    24                          |

İlk senaryo, tek bir birimin ne kadar bir kez yönlendirilbileceğini ele alınmaktadır.  

İkinci ve üçüncü senaryolar tek bir Azure NetApp Files uç noktasının sınırlarını değerlendirir. G/ç üst limitlerinin ve gecikmesinin olası avantajlarını araştırırlar.

## <a name="test-scenario-results"></a>Test senaryosu sonuçları

Aşağıdaki tabloda test senaryoları sonuçları özetlenmektedir.

|    Senaryo       |    G/ç oranı<br>  2 ms 'de     |    G/ç oranı<br>  kenarda     |    Aktarım hızı<br>  2 ms 'de     |    Aktarım hızı<br>  kenarda     |
|-------------------|---------------------------|--------------------------------|-----------------------------|----------------------------------|
|    1 birim       |    39.601                 |    49.502                      |    692 MIB/sn                 |    866 MIB/sn                      |
|    6 birim      |    255.613                |    317.000                     |    4.577 MIB/sn               |    5.568 MIB/sn                    |
|    12 birim     |    256.612                |    319.196                     |    4.577 MIB/sn               |    5.709 MIB/sn                    |

Tek hacimli senaryo, temel uygulama yapılandırmasını temsil eder. Bu, izleme testi senaryolarında temel senaryosudur.  

Altı hacimli senaryo, doğrusal artışı gösterir (%600) tek hacimli iş yüküne göre.  Tek bir sanal ağ içindeki tüm birimlere tek bir IP adresi üzerinden erişilir.  

12-Volume senaryosunda, altı hacimli senaryoya göre gecikme süresini gösteren genel bir azalma gösterilmektedir. Ancak ulaşılabilir verimlilik içinde karşılık gelen bir artış yoktur.   

Aşağıdaki grafikte Azure NetApp Files için EDA iş yükünün gecikme süresi ve işlem hızı gösterilmektedir.  

![Azure NetApp Files EDA iş yükünün gecikme süresi ve işlem oranı](../media/azure-netapp-files/solutions-electronic-design-automation-workload-latency-operation-rate.png)   

Aşağıdaki grafikte Azure NetApp Files için EDA iş yükünün gecikme süresi ve aktarım hızı gösterilmektedir.  

![Azure NetApp Files için EDA iş yükünün gecikme süresi ve aktarım hızı](../media/azure-netapp-files/solutions-electronic-design-automation-workload-latency-throughput.png) 

## <a name="layout-of-test-scenarios"></a>Test senaryolarının düzeni 

Aşağıdaki tablo, test senaryolarının yerleşimini özetler.

|    Test senaryosu     |    Toplam dizin sayısı     |    Toplam dosya sayısı     |
|----------------------|------------------------------------|------------------------------|
|    1 birim          |    88.000                          |    880.000                   |
|    6 birim         |    568.000                         |    5.680.000                 |
|    12 birim        |    568.000                         |    5.680.000                 |

Tüm iş yükü, eşzamanlı çalışan işlevsel ve fiziksel aşamaların bir karışımından oluşur. Bu, bir EDA araçları kümesinden diğerine tipik bir akışı temsil eder.   

İşlevsel aşama, ilk belirtimlerden ve mantıksal tasarımdan oluşur. Fiziksel aşama, mantıksal bir yonaya dönüştürüldüğünde gerçekleşir. Oturum açma ve bant dışı aşamalar sırasında son denetimler tamamlanır ve tasarım, üretim için bir döküme dağıtılır.  

İşlevsel aşama sıralı ve rastgele okuma ve yazma g/ç 'nin bir karışımını içerir. İşlevsel aşama, dosya stat ve erişim çağrıları gibi meta veri açısından yoğun bir işlemdir. Meta veri işlemleri boyut olmadan etkili olsa da, okuma ve yazma işlemleri 1 K ve 16 K arasında değişir. Çoğu okuma 4 K ile 16 K arasındadır.  Çoğu yazma 4 K veya daha az. Fiziksel aşama, 32 K ve 64 K OP boyutları karışımıyla tamamen sıralı okuma ve yazma işlemlerinden oluşur.  

Yukarıdaki grafiklerde, üretilen işin çoğu iş yükünün sıralı fiziksel aşamasından gelir. G/ç, küçük rastgele ve meta veri yoğun işlevsel aşamasından geliyor. Her iki aşama de paralel olarak gerçekleşir. 

Sonuç olarak, ölçeklenebilir bant genişliğine sahip olmak üzere EDA tasarımı için Azure NetApp Files Azure işlem ile eşleştirirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure NetApp Files kullanan çözüm mimarileri](azure-netapp-files-solution-architectures.md)
