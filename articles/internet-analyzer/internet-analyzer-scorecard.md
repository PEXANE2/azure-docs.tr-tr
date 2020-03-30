---
title: Karnenizi Yorumlama | Microsoft Dokümanlar
description: Azure Internet Çözümleyicisi için SSS.
services: internet-analyzer
author: mattcalder
ms.service: internet-analyzer
ms.topic: guide
ms.date: 10/16/2019
ms.author: mebeatty
ms.openlocfilehash: a05c704f62d93bdc110d8a5943dd266ca22eb5f4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73512892"
---
# <a name="interpreting-your-scorecard"></a>Karnenizi yorumlama

Karne sekmesi, testlerinizin toplu ve çözümlenmiş sonuçlarını içerir. Her testin kendi karneleri vardır. Karneler, ağ gereksinimleriniz için veri odaklı sonuçlar sağlamak için ölçüm sonuçlarının hızlı ve anlamlı özetlerini sağlar. Internet Analyzer, karara odaklanmanızı sağlayarak analizle ilgilenir.

Karne sekmesi Internet Analyzer kaynak menüsünde bulunabilir. 


## <a name="filters"></a>Filtreler

* ***Test:*** Sonuçları görüntülemek istediğiniz testi seçin - her testin kendi karnesi vardır. Test verileri, analizi tamamlamak için yeterli veri olduğunda görünür – çoğu durumda, bu 24 saat içinde olmalıdır. 
* ***Bitiş tarihinden & süre:*** Günde üç karne oluşturulur – her karne farklı bir toplama dönemini yansıtır – 24 saat önce (gün), yedi gün önce (hafta) ve 30 gün önceki (ay). Görmek istediğiniz zaman döneminin son gününü seçmek için "Bitiş Tarihi" filtresini kullanın. 
* ***Ülke:*** Son kullanıcınız olan her ülke için bir karne oluşturulur. Genel filtre tüm son kullanıcıları içerir.

## <a name="measurement-count"></a>Ölçüm sayısı

Ölçüm sayısı analizin güvenini etkiler. Sayım ne kadar yüksekse, sonuç o kadar doğru olur. Testler en azından günde uç nokta başına en az 100 ölçüm hedeflemelidir. Ölçüm sayıları çok düşükse, lütfen JavaScript istemcisini uygulamanızda daha sık yürütecek şekilde yapılandırın. Küçük farklar beklenebilir ve tamam olsa da, A ve B uç noktaları için ölçüm sayıları çok benzer olmalıdır. Büyük farklılıklar söz konusu olduğunda, sonuçlara güvenilmemelidir.

## <a name="percentiles"></a>Yüzdelik Dilimleri

Milisaniye cinsinden ölçülen gecikme, Internet'teki bir kaynak ve hedef arasındaki hızı ölçmek için kullanılan popüler bir ölçütdür. Gecikme sonu verileri normalde dağıtılmaz (yani bir "Çan Eğrisi" takip etmez) çünkü aritmetik ortalama gibi istatistikleri kullanırken sonuçları çarpıtan büyük gecikme değerlerinin bir "uzun kuyruk" vardır. Alternatif olarak, yüzdelik veriler analiz etmek için "dağıtımgerektirmez" bir yol sağlar. Örnek olarak, ortanca veya yüzde 50, dağılımın ortasını özetler - değerlerin yarısı bunun üzerinde, yarısı ise bunun altındadır. 75. yüzdelik değer, dağıtımdaki tüm değerlerin %75'inden daha büyük olduğu anlamına gelir. Internet Analyzer, stenodaki yüzdelik dilimleri P50, P75 ve P95 olarak ifade eder.

Internet Analyzer yüzdelik _örneklerdir._ Bu, gerçek popülasyon _ölçüsünün_aksinedir. Örneğin, Güney Kaliforniya Üniversitesi ve Microsoft öğrencileri arasındaki günlük gerçek nüfus ortanca gecikmesi, o gün deki tüm isteklerin ortanca gecikme değeridir. Uygulamada, tüm isteklerin değerini ölçmek pratik değildir, bu nedenle makul derecede büyük bir örneğin gerçek popülasyonu temsil ettiğini varsayıyoruz.

Analiz amacıyla, P50 (ortanca), gecikme dağılımı için beklenen bir değer olarak yararlıdır. P95 gibi daha yüksek yüzdelik artışlar, en kötü durumlarda gecikme gecikmesinin ne kadar yüksek olduğunu belirlemek için yararlıdır. Genel olarak müşteri gecikmesini anlamak istiyorsanız, P50 odaklanmak için doğru metriktir. Eğer en kötü performans gösteren müşteriler için performans anlamak ile ilgili iseniz, o zaman P95 odak noktası olmalıdır. P75 bu ikisi arasında bir dengedir.


## <a name="deltas"></a>Deltalar

Delta, A ve B uç noktaları için metrik değerler arasındaki farktır, B'nin A'ya göre yararını göstermek için hesaplanır. Deltalar mutlak (örn. 10 milisaniye) veya göreceli (%5) olabilir.

## <a name="confidence-interval"></a>Olasılık aralığı 

Güven aralıkları (CI), ortanca, P75 veya ortalama gibi popülasyon metriklerini içerme olasılığı olan bir değer aralığıdır. Biz% 95 CI kullanarak ortak istatistiksel kuralı izleyin.

Internet Analyzer için, örnek ölçümün büyük olasılıkla gerçek popülasyon ölçümüne çok yakın olduğunu gösterdiğinden, dar bir güven aralığı iyidir. Geniş bir güven aralığı, örnek ölçümümüzün gerçek popülasyon ölçümümüzü yansıttığından daha az kesinlik anlamına gelir. CI'yi geliştirmenin en iyi yolu ölçüm sayılarını artırmaktır.

## <a name="time-series"></a>Zaman serisi 

Bir zaman serisi, bir ölçümün zaman içinde nasıl değiştiğini gösterir. Internet'te, yoğun trafik dönemleri, hafta içi-hafta sonu nüfus farklılıkları ve tatiller gibi performansı etkileyen birçok zamansal faktör vardır.


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek için [Internet Analyzer Genel](internet-analyzer-overview.md)Bakış'ımıza bakın.
