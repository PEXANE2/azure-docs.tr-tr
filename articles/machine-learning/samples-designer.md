---
title: Veri kümeleri & örnek tasarımcı işlem hatları
titleSuffix: Azure Machine Learning
description: Makine öğrenimi işlem hatlarınızı atlayaöğrenmek için Azure Machine Learning tasarımcısında örnekleri kullanın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 03/29/2020
ms.custom: designer
ms.openlocfilehash: e40c49cdcaae6fa84a72c645f5fea4bac99ee03f
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83654558"
---
# <a name="example-pipelines--datasets-for-azure-machine-learning-designer"></a>Azure Machine Learning Designer için veri kümeleri & örnek işlem hatları

Kendi makine öğrenimi işlem hatlarınızı oluşturmaya hemen başlamak için Azure Machine Learning tasarımcısında yerleşik örnekleri kullanın. Azure Machine Learning Designer [GitHub deposu](https://github.com/Azure/MachineLearningDesigner) , bazı yaygın makine öğrenimi senaryolarını anlamanıza yardımcı olacak ayrıntılı belgeler içerir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://aka.ms/AMLFree)oluşturun.
* Kurumsal SKU 'SU olan bir Azure Machine Learning çalışma alanı.

## <a name="use-sample-pipelines"></a>Örnek işlem hatlarını kullanma

Tasarımcı, örnek işlem hatlarının bir kopyasını Studio çalışma alanınıza kaydeder. İşlem hattını düzenleyerek gereksinimlerinize uyarlayabilir ve kendi kendinize kaydedebilirsiniz. Projelerinizi başlatmak için bunları bir başlangıç noktası olarak kullanın.

Tasarımcı örneği kullanımı aşağıda verilmiştir:

1. <a href="https://ml.azure.com?tabs=jre" target="_blank">Ml.Azure.com</a>'de oturum açın ve birlikte çalışmak istediğiniz çalışma alanını seçin.

1. **Tasarımcı**' yı seçin.

1. **Yeni işlem hattı** bölümünde örnek bir işlem hattı seçin.

    Örneklerin tamamen listesi için **daha fazla örnek göster** ' i seçin.

1. Bir işlem hattını çalıştırmak için, önce varsayılan işlem hedefini, ardışık düzeni çalıştırmak için ayarlamanız gerekir.

   1. Tuvalin sağındaki **Ayarlar** bölmesinde, **işlem hedefini seç**' i seçin.

   1. Görüntülenen iletişim kutusunda, var olan bir işlem hedefini seçin veya yeni bir tane oluşturun. **Kaydet**’i seçin.

   1. Bir işlem hattı çalıştırması göndermek için tuvalin en üstünde **Gönder** ' i seçin.

   Örnek ardışık düzen ve işlem ayarlarına bağlı olarak, çalıştırmanın tamamlanması biraz zaman alabilir. Varsayılan işlem ayarlarının minimum düğüm boyutu 0 ' dır ve bu, tasarımcının boşta kaldıktan sonra kaynakları ayırması gerektiği anlamına gelir. İşlem kaynakları zaten ayrıldığından tekrarlanan işlem hattı çalıştırmaları daha az zaman alır. Ayrıca tasarımcı, verimliliği artırmak için her modül için önbelleğe alınmış sonuçları kullanır.


1. İşlem hattı çalışmaya başladıktan sonra, daha fazla bilgi edinmek için işlem hattını gözden geçirebilir ve her bir modülün çıktısını görüntüleyebilirsiniz. Modül çıkışlarını görüntülemek için aşağıdaki adımları kullanın:

   1. Tuvalde bir modül seçin.

   1. Tuvalin sağ tarafındaki modül ayrıntıları bölmesinde, **çıktılar + Günlükler**' i seçin. ![ ](./media/tutorial-designer-automobile-price-train-score/visualize-icon.png) Her modülün sonucunu görmek için grafik simgesi Görselleştir simgesini seçin. 

   En yaygın makine öğrenimi senaryolarından bazıları için örnekleri başlangıç noktaları olarak kullanın.

## <a name="regression"></a>Regresyon

Bu yerleşik gerileme örneklerini gezin.

| Örnek başlık | Açıklama | 
| --- | --- |
| [Örnek 1: gerileme-otomobil fiyat tahmini (temel)](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-regression-automobile-price-basic.md) | Doğrusal regresyon kullanarak otomobil fiyatlarını tahmin edin. |
| [Örnek 2: gerileme-otomobil fiyat tahmini (Gelişmiş)](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-regression-automobile-price-compare-algorithms.md) | Karar ormanını kullanarak otomobil fiyatlarını tahmin edin ve karar ağacı gerilediğini kullanın. En iyi algoritmayı bulmak için modelleri karşılaştırın.

## <a name="classification"></a>Sınıflandırma

Bu yerleşik sınıflandırma örneklerini keşfet. Örnekleri açıp modül açıklamalarını görüntüleyerek, belge bağlantıları olmadan örnekler hakkında daha fazla bilgi edinebilirsiniz.

| Örnek başlık | Açıklama | 
| --- | --- |
| [Örnek 3: Özellik seçimi ile Ikili sınıflandırma-gelir tahmini](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-classification-predict-income.md) | İki sınıf artırılmış bir karar ağacı kullanarak geliri yüksek veya düşük olarak tahmin edin. Özellikleri seçmek için Pearson bağıntısını kullanın.
| [Örnek 4: özel Python betiği ile Ikili sınıflandırma-kredi riski tahmini](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-classification-credit-risk-cost-sensitive.md) | Kredi uygulamalarını yüksek veya düşük riskli olarak sınıflandırın. Verilerinizi ağırlığa yönelik Python betiği yürütme modülünü kullanın.
| [Örnek 5: Ikili sınıflandırma-müşteri Ilişkisi tahmini](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-classification-churn.md) | İki sınıf artırılmış bir karar ağacının kullanıldığı müşteri karmaşıklığını tahmin edin. Taraflı verileri örneklemek için IÇTE 'yi kullanın.
| [Örnek 7: metin sınıflandırması-Vikipedi SP 500 veri kümesi](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-text-classification.md) | Birden çok Lass Lojistik gerileme ile Vikipedi makalelerinden şirket türlerini sınıflandırın. |
| Örnek 12: birden çok Lass sınıflandırması-harf tanıma | Yazılan harflerin sınıflandırılmasında ikili Sınıflandırıcıların bir kopyasını oluşturun. |

## <a name="recommender"></a>Öneren

Bu yerleşik öneren örnekleri keşfedebilirsiniz. Örnekleri açıp modül açıklamalarını görüntüleyerek, belge bağlantıları olmadan örnekler hakkında daha fazla bilgi edinebilirsiniz.

| Örnek başlık | Açıklama | 
| --- | --- |
| Örnek 10: öneri-film derecelendirmesi arası | Film başlıklarından ve derecelendirmeden bir film öneren altyapısı oluşturun. |

## <a name="utility"></a>Yardımcı program

Machine Learning yardımcı programlarını ve özelliklerini gösteren örnekler hakkında daha fazla bilgi edinin. Örnekleri açıp modül açıklamalarını görüntüleyerek, belge bağlantıları olmadan örnekler hakkında daha fazla bilgi edinebilirsiniz.

| Örnek başlık | Açıklama | 
| --- | --- |
| [Örnek 6: özel R betiği kullanma-Uçuş gecikmesi tahmini](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/how-to-designer-sample-classification-flight-delay.md) |
| Örnek 8: Ikili sınıflandırma için çapraz doğrulama-yetişkinlere yönelik gelir tahmini | Yetişkinlere yönelik gelir için ikili bir sınıflandırıcı oluşturmak üzere çapraz doğrulama kullanın.
| Örnek 9: permütasyon özelliği önem derecesi | Test veri kümesinin önem puanlarını hesaplamak için permütasyon özelliği önem derecesini kullanın. 
| Örnek 11: Ikili sınıflandırmayla ilgili parametreleri ayarlama-yetişkinlere yönelik gelir tahmini | İkili bir sınıflandırıcı oluşturmak için en uygun hiper parametreleri bulmak için model hiper parametrelerini ayarla ' yı kullanın. |

## <a name="datasets"></a>Veri kümeleri

Azure Machine Learning tasarımcısında yeni bir işlem hattı oluşturduğunuzda, bir dizi örnek veri kümesi varsayılan olarak dahil edilir. Bu örnek veri kümeleri, tasarımcı giriş hatlarında örnek işlem hatları tarafından kullanılır. 

Örnek veri kümeleri, veri kümeleri **Datasets** - **örnekleri** kategorisi altında bulunabilir. Bunu, tasarımcıda tuvalin solundaki modül paletinde bulabilirsiniz. Bu veri kümelerinin herhangi birini tuvalde sürükleyerek kendi işlem hattınızda kullanabilirsiniz.

| Veri kümesi &nbsp; adı&nbsp;&nbsp;&nbsp;&nbsp;| Veri kümesi açıklaması |
|-------------|:--------------------|
| Yetişkin Census geliri Ikili sınıflandırma veri kümesi | > 100 ' nin ayarlanmış bir gelir dizini ile 16 yaşın üzerinde çalışan yetişkinler kullanılarak 1994 Census veritabanının bir alt kümesi.<br/>**Kullanım**: bir kişinin 50 bin üzerinde bir yıl boyunca mi olduğunu tahmin etmek için demografik kullanan kişileri sınıflandırın.<br/> **Ilgili araştırma**: Kohavi, R., Becker, B., (1996). [UCI Machine Learning deposu](https://archive.ics.uci.edu/ml). Irvine, CA: California Üniversitesi, bilgi Okulu ve bilgisayar bilimi|
|Otomobil fiyat verileri (ham)|, Bir sigorta riski puanı ve bu fiyat dahil olmak üzere, marka ve model bazında otomobil hakkında bilgiler.<br/> Risk puanı başlangıçta otomatik fiyatla ilişkilendirilir. Daha sonra, söz konusu bir işlemdeki bir işlem için, symboling olarak bilinen bir işlemde gerçek risk için ayarlanır. + 3 değeri, otomatik olarak riskli olduğunu ve büyük olasılıkla güvenli olduğunu belirten-3 değerini gösterir.<br/>**Kullanım**: </b> gerileme veya çok sayıda sınıflandırma kullanarak, özelliklerle risk Puanını tahmin edin.<br/>**Ilgili araştırma**: </b> schlikökü, J.C. (1987). [UCI Machine Learning deposu](https://archive.ics.uci.edu/ml). Irvine, CA: California Üniversitesi, bilgi Okulu ve bilgisayar bilimi. |
| CRM uygulama paylaşılan etiketleri |KDD Fin2009 müşteri ilişkisi tahmin sınamasına ait Etiketler ([orange_small_train_appetency. Etiketler](http://www.sigkdd.org/site/2009/files/orange_small_train_appetency.labels)).|
|Paylaşılan CRM dalgalanması etiketleri|KDD Fin2009 müşteri ilişkisi tahmin sınamasına ait Etiketler ([orange_small_train_churn. Etiketler](http://www.sigkdd.org/site/2009/files/orange_small_train_churn.labels)).|
|CRM veri kümesi paylaşıldı | Bu veriler, KDD Kupa 2009 müşteri ilişkisi tahmin Challenge ([orange_small_train. Data. zip](http://www.sigkdd.org/site/2009/files/orange_small_train.data.zip)) ' den gelir. <br/>Veri kümesi, Fransız telekomünikasyon şirketi turuncu 'dan 50K müşterileri içerir. Her müşterinin 230 anonim özelliği vardır, bunlar sayısal ve 40 kategorik olan 190 ' dir. Özellikler çok seyrek. |
|Paylaşılan CRM satış etiketleri|KDD Fin2009 müşteri ilişkisi tahmin sınamasına ait Etiketler ([orange_large_train_upselling. Etiketler](http://www.sigkdd.org/site/2009/files/orange_large_train_upselling.labels)|
|Uçuş gecikmeleri verileri|Passenger, ABD 'nin ulaşım bölümünün ([zamanında](https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time)) geçiş Bakanlığı veri koleksiyonundan alınan zaman içindeki performans verilerini alır.<br/>Veri kümesi Nisan-Ekim 2013 ' de geçen süreyi içerir. Tasarımcıya yüklemeden önce veri kümesi şu şekilde işlenir: <br/>-Veri kümesi, kıst ABD 'de yalnızca 70 ortalamanızı havaalanları kapsayacak şekilde filtrelenmiştir <br/>-İptal edilen fışıkları 15 dakikadan uzun bir süre gecikti olarak etiketlendi <br/>-Ayrılan fışıklar filtrelendi <br/>-Şu sütunlar seçildi: Year, month, DayofMonth, DayOfWeek, taşıyıcı, Originairportıd, Destairportıd, CRSDepTime, DepDelay, DepDel15, CRSArrTime, ArrDelay, ArrDel15, Iptal edildi|
|Alman kredi kartı UCı veri kümesi|Alman. Data dosyasını kullanarak UCı Statgünlüğü (Almanya kredi kartı) veri kümesi ([Statlog + Almanya + kredi + veri](https://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data))).<br/>Veri kümesi, bir öznitelik kümesiyle tanımlanan kişileri, düşük veya yüksek kredi riskleri olarak sınıflandırır. Her örnek bir kişiyi temsil eder. Hem sayısal hem de kategorik ve bir ikili etiketi (kredi risk değeri) 20 özellik vardır. Yüksek kredi risk girişlerinde etiket = 2, düşük kredi risk girişlerinde etiket = 1 vardır. Düşük riskli bir örneği yüksek olarak sınıflandırın maliyeti 1 ' dir, ancak yüksek riskli örnek 5 ' i yanlış sınıflandırma maliyeti 5 ' tir.|
|IMDB film başlıkları|Veri kümesi Twitter 'da derecelendirilen filmler hakkında bilgiler içerir: ıMDB film KIMLIĞI, film adı, tarz ve üretim yılı. Veri kümesinde 17K filmler vardır. Veri kümesi "S" kağıdına tanıtılmıştı. Çizeneler, T. de Pessemier ve L. Maronlar. MovieTweetings: Twitter 'Dan toplanan bir film derecelendirmesi veri kümesi. Öneren sistemleri için Crowdsour, ve ınsan hesaplamasında Workshop, RecSys 2013. "|
|Film derecelendirmeleri|Veri kümesi, film arası doldurulabilir veri kümesinin genişletilmiş bir sürümüdür. Veri kümesi, filmler için 170K derecelendirmesine sahiptir ve Twitter 'daki iyi yapılandırılmış ara kaynaklardan ayıklanır. Her örnek bir tweet temsil eder ve bir tanımlama grubu: Kullanıcı KIMLIĞI, ıMDB film KIMLIĞI, derecelendirme, zaman damgası, bu tweet için sık kullanılanlar sayısı ve bu tweet yeniden dengelendirme sayısı. DataSet, öneren Systems Challenge 2014 için bir. diyor, S. Çizms, B. Loni ve D. Tikk tarafından kullanılabilir hale getirilir.|
|Hava durumu veri kümesi|NOAA 'den saatlik Land tabanlı hava durumu gözlemlerini ([201304 ile 201310 arasında Birleşik veriler](https://az754797.vo.msecnd.net/data/WeatherDataset.csv)).<br/>Hava durumu verileri, Havaalanı hava durumu istasyonlarından yapılan ve Nisan-Ekim 2013 zaman dilimini kapsayan gözlemlerini kapsamaktadır. Tasarımcıya yüklemeden önce veri kümesi şu şekilde işlenir:    <br/> -Hava durumu istasyonu kimlikleri ilgili Havaalanı kimliklerine eşlendi    <br/> -70 ortalamanızı ile ilişkilendirilmemiş Hava durumu istasyonları filtrelendi    <br/> -Tarih sütunu ayrı yıl, ay ve gün sütunlarına bölündü    <br/> -Şu sütunlar seçildi: Airportıd, Year, ay, gün, saat, saat dilimi, ufuk koşulu, görünürlük, dalgalı Thertype, DryBulbFarenheit, Dronblik, WetBulbFarenheit, Wetbulbgrat, DewPointFarenheit, Dewpointsanti, Relativenem, WIN,,,.|
|Vikipedi SP 500 veri kümesi|Veriler https://www.wikipedia.org/) ,, XML verileri olarak depolanan her bir S&P 500 şirketinin makalelerine bağlı olarak, Vikipden türetilir.    <br/>Tasarımcıya yüklemeden önce veri kümesi şu şekilde işlenir:    <br/> -Belirli bir şirkete ait metin içeriğini Ayıkla    <br/> -Wiki biçimlendirmesini kaldır    <br/> -Alfasayısal olmayan karakterleri kaldır    <br/> -Tüm metni küçük harfe Dönüştür    <br/> -Bilinen şirket kategorileri eklendi    <br/>Bazı şirketlerde bir makalenin bulunamadığını, bu nedenle kayıt sayısının 500 ' den küçük olduğunu unutmayın.|


## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-ui-cleanup](../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Öğretici ile tahmine dayalı analiz ve makine öğrenimi hakkında temel bilgileri öğrenin [: tasarımcı ile otomobil fiyatlarını tahmin](tutorial-designer-automobile-price-train-score.md) etme

