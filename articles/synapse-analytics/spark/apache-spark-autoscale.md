---
title: Azure SYNAPSE Apache Spark örneklerini otomatik olarak ölçeklendirme
description: Apache Spark örneklerini otomatik olarak ölçeklendirmek için Azure SYNAPSE otomatik ölçeklendirme özelliğini kullanın
author: euangMS
ms.author: euang
ms.reviewer: euang
services: synapse-analytics
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: spark
ms.date: 03/31/2020
ms.openlocfilehash: 86e9b7425c8c75495a17b66af710d2c55e6e59ca
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85194289"
---
# <a name="automatically-scale-azure-synapse-analytics-apache-spark-pools"></a>Azure SYNAPSE Analytics Apache Spark havuzlarını otomatik olarak ölçeklendirme

Azure SYNAPSE Analytics havuzunun otomatik ölçeklendirme özelliği için Apache Spark, bir küme örneğindeki düğümlerin sayısını yukarı ve aşağı otomatik olarak ölçeklendirir. Azure SYNAPSE Analytics havuzu için yeni bir Apache Spark oluşturulması sırasında, otomatik ölçeklendirme seçildiğinde en düşük ve en fazla düğüm sayısı ayarlanabilir. Sonra otomatik ölçeklendirme, yükün kaynak gereksinimlerini izler ve düğüm sayısını yukarı veya aşağı ölçeklendirir. Bu özellik için ek ücret alınmaz.

## <a name="metrics-monitoring"></a>Ölçüm izleme

Otomatik ölçeklendirme Spark örneğini sürekli izler ve aşağıdaki ölçümleri toplar:

|Metric|Açıklama|
|---|---|
|Toplam bekleyen CPU|Tüm bekleyen düğümlerin yürütülmesini başlatmak için gereken toplam çekirdek sayısı.|
|Toplam bekleyen bellek|Tüm bekleyen düğümlerin yürütülmesini başlatmak için gereken toplam bellek (MB cinsinden).|
|Toplam boş CPU|Etkin düğümlerdeki tüm kullanılmayan çekirdekler toplamı.|
|Toplam boş bellek|Etkin düğümlerde kullanılmayan belleğin (MB cinsinden) toplamı.|
|Düğüm başına kullanılan bellek|Bir düğümdeki yük. 10 GB belleğin kullanıldığı bir düğüm, 2 GB kullanılan bellekle çalışan bir çalışandan daha fazla yük altında değerlendirilir.|

Yukarıdaki ölçümler her 30 saniyede bir denetlenir. Otomatik ölçeklendirme, ölçek artırma ve ölçeği azaltma kararlarını bu ölçümlere göre yapar.

## <a name="load-based-scale-conditions"></a>Yük tabanlı ölçek koşulları

Aşağıdaki koşullar algılandığında otomatik ölçeklendirme bir ölçek isteği ister:

|Ölçeği büyütme|Ölçeği azalt|
|---|---|
|Toplam bekleyen CPU, 1 dakikadan uzun bir toplam boş CPU 'dan daha fazla.|Toplam bekleyen CPU, 2 dakikadan uzun bir süre boyunca toplam boş CPU 'dan daha az.|
|Toplam bekleyen bellek, 1 dakikadan uzun bir toplam boş bellekten fazla.|Toplam bekleyen bellek 2 dakikadan uzun bir süre boyunca toplam boş bellekten daha az.|

Ölçek artırma için, Azure SYNAPSE otomatik ölçeklendirme hizmeti, geçerli CPU ve bellek gereksinimlerini karşılamak için kaç yeni düğüm gerektiğini hesaplar ve ardından gerekli sayıda düğüm eklemek için bir ölçek isteği yayınlar.

Ölçek azaltma için, yürütme sayısına, düğüm başına uygulama ana bilgisayarlara ve geçerli CPU ve bellek gereksinimlerine göre otomatik ölçeklendirme, belirli sayıda düğümü kaldırma isteği verir. Hizmet ayrıca hangi düğümlerin geçerli iş yürütmeye göre kaldırılması gerektiğini de algılar. Ölçeği azaltma işlemi, önce düğümleri yeniden komisyonlar ve sonra kümeden kaldırır.

## <a name="get-started"></a>başlarken

### <a name="create-a-spark-pool-with-autoscaling"></a>Otomatik ölçeklendirme ile Spark havuzu oluşturma

Otomatik ölçeklendirme özelliğini etkinleştirmek için, normal havuz oluşturma sürecinin bir parçası olarak aşağıdaki adımları izleyin:

1. **Temel bilgiler** sekmesinde **Otomatik ölçeklendirmeyi etkinleştir** onay kutusunu seçin.
1. Aşağıdaki özellikler için istenen değerleri girin:  

    * **En az** düğüm sayısı.
    * **En fazla** düğüm sayısı.

İlk düğüm sayısı en düşük değer olacaktır. Bu değer, oluşturulduğu sırada Örneğin ilk boyutunu tanımlar. Düğüm sayısı alt sınırı üçten az olamaz.

## <a name="best-practices"></a>En iyi uygulamalar

### <a name="consider-the-latency-of-scale-up-or-scale-down-operations"></a>Ölçek artırma veya azaltma işlemlerinin gecikme süresini göz önünde bulundurun

Ölçeklendirme işleminin tamamlanması 1 ila 5 dakika sürebilir.

### <a name="preparation-for-scaling-down"></a>Ölçeği azaltma hazırlığı

Örnek ölçeklendirme işlemi sırasında, otomatik ölçeklendirme, düğüm üzerinde yeni bir yürütme işlemi yapabilmesi için düğümleri kullanımdan kaldırma durumuna koyar.

Çalışan işler çalışmaya ve sonuna kadar devam edecektir. Bekleyen işler, daha az kullanılabilir düğüm ile normal şekilde zamanlanmayı bekler.

## <a name="next-steps"></a>Sonraki adımlar

Yeni Spark havuzu ayarlamak için hızlı başlangıç [Spark havuzu oluşturma](../quickstart-create-apache-spark-pool-portal.md)
