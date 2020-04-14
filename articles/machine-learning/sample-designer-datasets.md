---
title: Tasarımcı için örnek veriler
titleSuffix: Azure Machine Learning
description: Azure Machine Learning tasarımcısı ile kullanılabilen örnek veri kümeleri hakkında daha fazla bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: likebupt
ms.author: keli19
ms.date: 03/03/2020
ms.openlocfilehash: b19c1ed5aa556a2cfb633ac6fbc0cf9f95d0337f
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81256986"
---
# <a name="sample-datasets-in-azure-machine-learning-designer-preview"></a>Azure Machine Learning tasarımcısında örnek veri kümeleri (önizleme)

Azure Machine Learning tasarımcısında (önizleme) yeni bir ardışık iş parçası oluşturduğunuzda, varsayılan olarak bir dizi örnek veri kümesi dahil edilir. Bu örnek veri kümeleri, tasarımcı ana sayfasındaki örnek ardışık hatlar tarafından kullanılır. 

Örnek veri kümeleri **Datasets**-**Örnekleri** kategorisi altında kullanılabilir. Bunu tasarımcıda tuvalin solundaki modül paletinde bulabilirsiniz. Bu veri kümelerinden herhangi birini kendi ardınızdaki tuvale sürükleyerek kullanabilirsiniz.

## <a name="datasets"></a>Veri kümeleri


| Dataset&nbsp;adı&nbsp;&nbsp;&nbsp;&nbsp;| Dataset açıklaması |
|-------------|:--------------------|
| Yetişkin Nüfus Sayımı Gelir İkili Sınıflandırması veri seti | 1994 Nüfus Sayımı veritabanının bir alt kümesi, 16 yaşın üzerindeki çalışan yetişkinler kullanılarak 100 > düzeltilmiş gelir indeksi ile.<br/>**Kullanımı**: Bir kişinin yılda 50.000'den fazla kazanıp kazanmadığını tahmin etmek için demografik bilgileri kullanarak insanları sınıflandırın.<br/> **İlgili Araştırma**: Kohavi, R., Becker, B., (1996). [UCI Makine Öğrenme Deposu](https://archive.ics.uci.edu/ml). Irvine, CA: Kaliforniya Üniversitesi, Bilgi ve Bilgisayar Bilimleri Fakültesi|
|Otomobil fiyat verileri (Raw)|Otomobiller hakkında fiyat da dahil olmak üzere, silindir ve MPG sayısı gibi özellikler, yanı sıra bir sigorta risk puanı ile ilgili bilgiler.<br/> Risk puanı başlangıçta otomatik fiyat ile ilişkilidir. Daha sonra aktüerler için sembol olarak bilinen bir süreçte gerçek risk için ayarlanır. +3 değeri, otomatikin riskli olduğunu ve -3 değerinin büyük olasılıkla güvenli olduğunu gösterir.<br/>**Kullanımı**</b> : Regresyon veya çok değişkenli sınıflandırma kullanarak risk puanını özelliklere göre tahmin edin.<br/>**İlgili**Araştırma</b> : Schlimmer, J.C. (1987). [UCI Makine Öğrenme Deposu](https://archive.ics.uci.edu/ml). Irvine, CA: Kaliforniya Üniversitesi, Bilgi ve Bilgisayar Bilimleri Fakültesi. |
| CRM Appetency Etiketleri Paylaşıldı |Etiketler KDD Kupası 2009 müşteri ilişkileri tahmin sorun[(orange_small_train_appetency.labels).](http://www.sigkdd.org/site/2009/files/orange_small_train_appetency.labels)|
|CRM Churn Etiketleri Paylaşıldı|Etiketler KDD Kupası 2009 müşteri ilişkileri tahmin sorun[(orange_small_train_churn.labels).](http://www.sigkdd.org/site/2009/files/orange_small_train_churn.labels)|
|CRM Dataset Paylaşıldı | Bu veriler KDD Cup 2009 müşteri ilişkileri tahmini meydan[(orange_small_train.data.zip)](http://www.sigkdd.org/site/2009/files/orange_small_train.data.zip)geliyor. <br/>Veri seti, Fransız Telekom şirketi Orange'dan 50K müşteri içerir. Her müşterinin 190'ı sayısal, 40'ı kategorik olmak üzere 230 anonim leştirilmiş özelliği vardır. Özellikleri çok seyrek. |
|CRM Upselling Etiketleri Paylaşıldı|Etiketler KDD Kupası 2009 müşteri ilişkileri tahmin sorun ([orange_large_train_upselling.labels](http://www.sigkdd.org/site/2009/files/orange_large_train_upselling.labels)|
|Uçuş Gecikmeleri Verileri|Abd Ulaştırma Bakanlığı'nın TranStats veri koleksiyonundan alınan yolcu uçuşu zamanında performans verileri[(Zamanında).](https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time)<br/>Veri kümesi Nisan-Ekim 2013 dönemini kapsar. Tasarımcıya yüklemeden önce, veri kümesi aşağıdaki gibi işlenmiştir: <br/>- Veri seti kıta ABD'de sadece 70 en işlek havaalanları kapsayacak şekilde filtreedildi <br/>- İptal edilen uçuşlar 15 dakikadan fazla gecikmeli olarak etiketlendi <br/>- Yönlendirilen uçuşlar filtrelendi <br/>- Aşağıdaki sütunlar seçildi: Yıl, Ay, DayofMonth, DayOfWeek, Carrier, OriginAirportID, DestAirportID, CRSDepTime, DepDelay, DepDel15, CRSArrTime, ArrDelay, ArrDel15, İptal|
|Alman Kredi Kartı UCI veri seti|UCI Statlog (Alman Kredi Kartı) veri seti ([Statlog+German+Credit+Data](https://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data))), german.data dosyasını kullanarak.<br/>Veri kümesi, bir dizi öznitelik tarafından açıklanan kişileri düşük veya yüksek kredi riskleri olarak sınıflandırılır. Her örnek bir kişiyi temsil eder. Sayısal ve kategorik 20 özellik ve ikili etiket (kredi risk değeri) vardır. Yüksek kredi riski girişleri etiketi = 2, düşük kredi riski girişleri etiketi = 1 var. Düşük risk örneğini yüksek olarak sınıflandırmanın maliyeti 1'dir, yüksek risk örneğini düşük olarak sınıflandırmanın maliyeti 5'tir.|
|IMDB Film Başlıkları|Dataset, Twitter tweetlerinde derecelendirilen filmler hakkında bilgi içerir: IMDB film kimliği, film adı, tür ve yapım yılı. Veri setinde 17K film vardır. Veri seti "S. " adlı kağıtta tanıtıldı. Dooms, T. De Pessemier ve L. Martens. MovieTweetings: Twitter'dan Toplanan Bir Film Derecelendirme Dataset. Tavsiye Sistemleri için Crowdsourcing ve İnsan Hesaplama Çalıştayı, RecSys 2013'te CrowdRec."|
|Film Reytingleri|Dataset, Movie Tweetings veri kümesinin genişletilmiş bir sürümüdür. Dataset, Twitter'daki iyi yapılandırılmış tweetlerden çıkarılan filmler için 170K derecelendirmeye sahiptir. Her örnek bir tweet temsil eder ve bir tuple: kullanıcı kimliği, IMDB film kimliği, derecelendirme, zaman damgası, bu tweet için sık sayısı ve bu tweet retweets sayısı. Dataset, Tavsiye Sistemleri Mücadelesi 2014 için A. Said, S. Dooms, B. Loni ve D. Tikk tarafından kullanıma sunulmuştur.|
|Hava Durumu Dataset|NOAA'dan saatlik kara tabanlı hava gözlemleri[(201304 ile 201310 verileri birleştirilmiştir).](https://az754797.vo.msecnd.net/data/WeatherDataset.csv)<br/>Hava durumu verileri, Nisan-Ekim 2013 dönemini kapsayan havaalanı hava istasyonlarından yapılan gözlemleri kapsamaktadır. Tasarımcıya yüklemeden önce, veri kümesi aşağıdaki gibi işlenmiştir:    <br/> - Hava istasyonu işleri ilgili hava alanı işleri işlemişti    <br/> - En işlek 70 havaalanıyla ilişkili olmayan hava istasyonları filtrelendi    <br/> - Tarih sütunu ayrı Yıl, Ay ve Gün sütunlarına bölündü    <br/> - Aşağıdaki sütunlar seçildi: AirportID, Yıl, Ay, Gün, Saat, TimeZone, SkyCondition, Görünürlük, WeatherType, DryBulbFarenheit, DryBulbCelsius, WetBulbFarenheit, WetBulbCelsius, DewPointFarenheit, DewPointFarenheit, RelativeHumidity, WindSpeed, WindDirection, ValueForWindCharacter, StationPressure, PressureTendency, PressureChange, SeaLevelPressure, RecordType, HourlyPrecip, Altimeter|
|Vikipedi SP 500 Veri Seti|Veriler Wikipedia'danhttps://www.wikipedia.org/) türetilmiştir ( her S&P 500 şirketinin makaleleri temel alınca, XML verileri olarak depolanır.    <br/>Tasarımcıya yüklemeden önce, veri kümesi aşağıdaki gibi işlenmiştir:    <br/> - Her şirket için metin içeriğini ayıklama    <br/> - Wiki biçimlendirmeyi kaldırma    <br/> - Alfanümerik olmayan karakterleri kaldırın    <br/> - Tüm metni küçük harfe dönüştürün    <br/> - Bilinen şirket kategorileri eklendi    <br/>Bazı şirketler için bir makalebulunamadı, bu nedenle kayıt sayısının 500'den az olduğunu unutmayın.|

## <a name="next-steps"></a>Sonraki adımlar

* Öğretici ile tahmine dayalı analitik ve makine öğreniminin temellerini [öğrenin: Tasarımcıyla otomobil fiyatını tahmin edin](tutorial-designer-automobile-price-train-score.md)

* Varolan tasarımcı [örneklerini](samples-designer.md) gereksinimlerinize uyarlamak için nasıl değiştirip değiştirebilirsiniz.
