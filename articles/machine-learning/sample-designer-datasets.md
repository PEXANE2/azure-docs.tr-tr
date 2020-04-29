---
title: Tasarımcı için örnek veriler
titleSuffix: Azure Machine Learning
description: Azure Machine Learning tasarımcısında bulunan örnek veri kümeleri hakkında daha fazla bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: likebupt
ms.author: keli19
ms.date: 03/03/2020
ms.openlocfilehash: b19c1ed5aa556a2cfb633ac6fbc0cf9f95d0337f
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81256986"
---
# <a name="sample-datasets-in-azure-machine-learning-designer-preview"></a>Azure Machine Learning tasarımcısında örnek veri kümeleri (Önizleme)

Azure Machine Learning tasarımcısında yeni bir işlem hattı oluşturduğunuzda (Önizleme), varsayılan olarak bir dizi örnek veri kümesi bulunur. Bu örnek veri kümeleri, tasarımcı giriş hatlarında örnek işlem hatları tarafından kullanılır. 

Örnek veri kümeleri, **veri**-kümeleri**örnekleri** kategorisi altında bulunabilir. Bunu, tasarımcıda tuvalin solundaki modül paletinde bulabilirsiniz. Bu veri kümelerinin herhangi birini tuvalde sürükleyerek kendi işlem hattınızda kullanabilirsiniz.

## <a name="datasets"></a>Veri kümeleri


| Veri&nbsp;kümesi adı&nbsp;&nbsp;&nbsp;&nbsp;| Veri kümesi açıklaması |
|-------------|:--------------------|
| Yetişkin Census geliri Ikili sınıflandırma veri kümesi | > 100 ' nin ayarlanmış bir gelir dizini ile 16 yaşın üzerinde çalışan yetişkinler kullanılarak 1994 Census veritabanının bir alt kümesi.<br/>**Kullanım**: bir kişinin 50 bin üzerinde bir yıl boyunca mi olduğunu tahmin etmek için demografik kullanan kişileri sınıflandırın.<br/> **Ilgili araştırma**: Kohavi, R., Becker, B., (1996). [UCI Machine Learning deposu](https://archive.ics.uci.edu/ml). Irvine, CA: California Üniversitesi, bilgi Okulu ve bilgisayar bilimi|
|Otomobil fiyat verileri (ham)|, Bir sigorta riski puanı ve bu fiyat dahil olmak üzere, marka ve model bazında otomobil hakkında bilgiler.<br/> Risk puanı başlangıçta otomatik fiyatla ilişkilendirilir. Daha sonra, söz konusu bir işlemdeki bir işlem için, symboling olarak bilinen bir işlemde gerçek risk için ayarlanır. + 3 değeri, otomatik olarak riskli olduğunu ve büyük olasılıkla güvenli olduğunu belirten-3 değerini gösterir.<br/>**Kullanım**:</b> gerileme veya çok sayıda sınıflandırma kullanarak, özelliklerle risk Puanını tahmin edin.<br/>**İlgili araştırma**:</b> schlikökü, J.C. (1987). [UCI Machine Learning deposu](https://archive.ics.uci.edu/ml). Irvine, CA: California Üniversitesi, bilgi Okulu ve bilgisayar bilimi. |
| CRM uygulama paylaşılan etiketleri |KDD Fin2009 müşteri ilişkisi tahmin sınamasına ait Etiketler ([orange_small_train_appetency. Etiketler](http://www.sigkdd.org/site/2009/files/orange_small_train_appetency.labels)).|
|Paylaşılan CRM dalgalanması etiketleri|KDD Fin2009 müşteri ilişkisi tahmin sınamasına ait Etiketler ([orange_small_train_churn. Etiketler](http://www.sigkdd.org/site/2009/files/orange_small_train_churn.labels)).|
|CRM veri kümesi paylaşıldı | Bu veriler, KDD Kupa 2009 müşteri ilişkisi tahmin Challenge ([orange_small_train. Data. zip](http://www.sigkdd.org/site/2009/files/orange_small_train.data.zip)) ' den gelir. <br/>Veri kümesi, Fransız telekomünikasyon şirketi turuncu 'dan 50K müşterileri içerir. Her müşterinin 230 anonim özelliği vardır, bunlar sayısal ve 40 kategorik olan 190 ' dir. Özellikler çok seyrek. |
|Paylaşılan CRM satış etiketleri|KDD Fin2009 müşteri ilişkisi tahmin sınamasına ait Etiketler ([orange_large_train_upselling. Etiketler](http://www.sigkdd.org/site/2009/files/orange_large_train_upselling.labels)|
|Uçuş gecikmeleri verileri|Passenger, ABD 'nin ulaşım bölümünün ([zamanında](https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time)) geçiş Bakanlığı veri koleksiyonundan alınan zaman içindeki performans verilerini alır.<br/>Veri kümesi Nisan-Ekim 2013 ' de geçen süreyi içerir. Tasarımcıya yüklemeden önce veri kümesi şu şekilde işlenir: <br/>-Veri kümesi, kıst ABD 'de yalnızca 70 ortalamanızı havaalanları kapsayacak şekilde filtrelenmiştir <br/>-İptal edilen fışıkları 15 dakikadan uzun bir süre gecikti olarak etiketlendi <br/>-Ayrılan fışıklar filtrelendi <br/>-Şu sütunlar seçildi: Year, month, DayofMonth, DayOfWeek, taşıyıcı, Originairportıd, Destairportıd, CRSDepTime, DepDelay, DepDel15, CRSArrTime, ArrDelay, ArrDel15, Iptal edildi|
|Alman kredi kartı UCı veri kümesi|Alman. Data dosyasını kullanarak UCı Statgünlüğü (Almanya kredi kartı) veri kümesi ([Statlog + Almanya + kredi + veri](https://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data))).<br/>Veri kümesi, bir öznitelik kümesiyle tanımlanan kişileri, düşük veya yüksek kredi riskleri olarak sınıflandırır. Her örnek bir kişiyi temsil eder. Hem sayısal hem de kategorik ve bir ikili etiketi (kredi risk değeri) 20 özellik vardır. Yüksek kredi risk girişlerinde etiket = 2, düşük kredi risk girişlerinde etiket = 1 vardır. Düşük riskli bir örneği yüksek olarak sınıflandırın maliyeti 1 ' dir, ancak yüksek riskli örnek 5 ' i yanlış sınıflandırma maliyeti 5 ' tir.|
|IMDB film başlıkları|Veri kümesi Twitter 'da derecelendirilen filmler hakkında bilgiler içerir: ıMDB film KIMLIĞI, film adı, tarz ve üretim yılı. Veri kümesinde 17K filmler vardır. Veri kümesi "S" kağıdına tanıtılmıştı. Çizeneler, T. de Pessemier ve L. Maronlar. MovieTweetings: Twitter 'Dan toplanan bir film derecelendirmesi veri kümesi. Öneren sistemleri için Crowdsour, ve ınsan hesaplamasında Workshop, RecSys 2013. "|
|Film derecelendirmeleri|Veri kümesi, film arası doldurulabilir veri kümesinin genişletilmiş bir sürümüdür. Veri kümesi, filmler için 170K derecelendirmesine sahiptir ve Twitter 'daki iyi yapılandırılmış ara kaynaklardan ayıklanır. Her örnek bir tweet temsil eder ve bir tanımlama grubu: Kullanıcı KIMLIĞI, ıMDB film KIMLIĞI, derecelendirme, zaman damgası, bu tweet için sık kullanılanlar sayısı ve bu tweet yeniden dengelendirme sayısı. DataSet, öneren Systems Challenge 2014 için bir. diyor, S. Çizms, B. Loni ve D. Tikk tarafından kullanılabilir hale getirilir.|
|Hava durumu veri kümesi|NOAA 'den saatlik Land tabanlı hava durumu gözlemlerini ([201304 ile 201310 arasında Birleşik veriler](https://az754797.vo.msecnd.net/data/WeatherDataset.csv)).<br/>Hava durumu verileri, Havaalanı hava durumu istasyonlarından yapılan ve Nisan-Ekim 2013 zaman dilimini kapsayan gözlemlerini kapsamaktadır. Tasarımcıya yüklemeden önce veri kümesi şu şekilde işlenir:    <br/> -Hava durumu istasyonu kimlikleri ilgili Havaalanı kimliklerine eşlendi    <br/> -70 ortalamanızı ile ilişkilendirilmemiş Hava durumu istasyonları filtrelendi    <br/> -Tarih sütunu ayrı yıl, ay ve gün sütunlarına bölündü    <br/> -Şu sütunlar seçildi: Airportıd, Year, ay, gün, saat, saat dilimi, ufuk koşulu, görünürlük, dalgalı Thertype, DryBulbFarenheit, Dronblik, WetBulbFarenheit, Wetbulbgrat, DewPointFarenheit, Dewpointsanti, Relativenem, WIN,,,.|
|Vikipedi SP 500 veri kümesi|Veriler,, XML verileri olarakhttps://www.wikipedia.org/) depolanan her bir S&P 500 şirketinin makalelerine bağlı olarak, vikipden türetilir.    <br/>Tasarımcıya yüklemeden önce veri kümesi şu şekilde işlenir:    <br/> -Belirli bir şirkete ait metin içeriğini Ayıkla    <br/> -Wiki biçimlendirmesini kaldır    <br/> -Alfasayısal olmayan karakterleri kaldır    <br/> -Tüm metni küçük harfe Dönüştür    <br/> -Bilinen şirket kategorileri eklendi    <br/>Bazı şirketlerde bir makalenin bulunamadığını, bu nedenle kayıt sayısının 500 ' den küçük olduğunu unutmayın.|

## <a name="next-steps"></a>Sonraki adımlar

* Öğretici ile tahmine dayalı analiz ve makine öğrenimi hakkında temel bilgileri öğrenin [: tasarımcı ile otomobil fiyatlarını tahmin](tutorial-designer-automobile-price-train-score.md) etme

* Mevcut [Tasarımcı örneklerini](samples-designer.md) gereksinimlerinize uyarlayacak şekilde nasıl değiştireceğinizi öğrenin.
