---
title: Örnek veri kümelerini kullanma
titleSuffix: Azure Machine Learning
description: Machine Learning tasarımcısında bulunan örnek modellerde kullanılan veri kümelerinin açıklamaları. İşlem hatlarınız için bu örnek veri kümelerini kullanabilirsiniz.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.date: 02/19/2020
ms.openlocfilehash: 14e514f0025d474b3afb45524753583b7c2e8a7d
ms.sourcegitcommit: 1f738a94b16f61e5dad0b29c98a6d355f724a2c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "78165068"
---
# <a name="use-the-sample-datasets-in-azure-machine-learning-designer-preview"></a>Azure Machine Learning tasarımcısında örnek veri kümelerini kullanma (Önizleme)

Azure Machine Learning tasarımcısında yeni bir işlem hattı oluşturduğunuzda (Önizleme), varsayılan olarak bir dizi örnek veri kümesi bulunur. Bu örnek veri kümelerinin çoğu, tasarımcı giriş sayfasında örnek modeller tarafından kullanılır. Diğer bir deyişle, makine öğreniminde genellikle kullanılan çeşitli veri türlerine örnek olarak dahildir.

Bu veri kümelerinden bazıları Azure Blob depolama alanında kullanılabilir. Bu veri kümeleri için, aşağıdaki tablo doğrudan bir bağlantı sağlar. [Veri alma](./algorithm-module-reference/import-data.md) modülünü kullanarak bu veri kümelerini ardışık düzenlerinizde kullanabilirsiniz.

Bu örnek veri kümelerinin geri kalanı, **veri kümeleri**-**örnekleri** kategorisi altında bulunabilir. Bunu, tasarımcıda tuvalin solundaki modül paletinde bulabilirsiniz. Bu veri kümelerinin herhangi birini tuvalde sürükleyerek kendi işlem hattınızda kullanabilirsiniz.

## <a name="datasets"></a>Veri kümeleri

<table>

<tr>
  <th>Veri kümesi adı</th>
  <th>Veri kümesi açıklaması</th>
</tr>

<tr>
  <td>Yetişkin Census geliri Ikili sınıflandırma veri kümesi</td>
  <td>
> 100 ' nin ayarlanmış bir gelir dizini ile 16 yaşın üzerinde çalışan yetişkinler kullanılarak 1994 Census veritabanının bir alt kümesi.
<p></p>
<b>Kullanım:</b> Bir kişinin 50 bin üzerinde bir yıl boyunca mi olduğunu tahmin etmek için demografik kullanan kişileri sınıflandırın.
<p></p>
<b>Ilgili araştırma:</b> Kohavi, R., Becker, B., (1996). UCı Machine Learning depo <a href="https://archive.ics.uci.edu/ml">https://archive.ics.uci.edu/ml</a>. Irvine, CA: California Üniversitesi, bilgi Okulu ve bilgisayar bilimi </td>
</tr>

<tr>
  <td>Otomobil fiyat verileri (ham)</td>
  <td>
, Bir sigorta riski puanı ve bu fiyat dahil olmak üzere, marka ve model bazında otomobil hakkında bilgiler.
<p></p>
Risk puanı başlangıçta otomatik fiyatla ilişkilendirilir. Daha sonra, söz konusu bir işlemdeki bir işlem için, symboling olarak bilinen bir işlemde gerçek risk için ayarlanır. \+ 3 değeri, otomatik olarak riskli olduğunu ve büyük olasılıkla güvenli olduğunu belirten-3 değerini gösterir.
<p></p>
<b>Kullanım:</b> Gerileme veya çok sayıda sınıflandırma kullanarak, özellik ile risk Puanını tahmin edin. 
<p></p>
<b>Ilgili araştırma:</b> Schliyoya, J.C. (1987). UCı Machine Learning depo <a href="https://archive.ics.uci.edu/ml">https://archive.ics.uci.edu/ml</a>. Irvine, CA: California Üniversitesi, bilgi Okulu ve bilgisayar bilimi </td>
</tr>


<tr>
  <td>CRM uygulama paylaşılan etiketleri</td>
  <td>
KDD Fin2009 müşteri ilişkisi tahmin sınamasına ait Etiketler (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_appetency.labels">orange_small_train_appetency. Etiketler</a>).
  </td>
</tr>

<tr>
  <td>Paylaşılan CRM dalgalanması etiketleri</td>
  <td>
KDD Fin2009 müşteri ilişkisi tahmin sınamasına ait Etiketler (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_churn.labels">orange_small_train_churn. Etiketler</a>).
  </td>
</tr>

<tr>
  <td>CRM veri kümesi paylaşıldı</td>
  <td>
Bu veriler, KDD Kupa 2009 müşteri ilişkisi tahmin Challenge (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train.data.zip">orange_small_train. Data. zip</a>) ' den gelir.
<p></p>
Veri kümesi, Fransız telekomünikasyon şirketi turuncu 'dan 50K müşterileri içerir. Her müşterinin 230 anonim özelliği vardır, bunlar sayısal ve 40 kategorik olan 190 ' dir. Özellikler çok seyrek.
  </td>
</tr>

<tr>
  <td>Paylaşılan CRM satış etiketleri</td>
  <td>
KDD Fin2009 müşteri ilişkisi tahmin sınamasına ait Etiketler (<a href="http://www.sigkdd.org/site/2009/files/orange_large_train_upselling.labels">orange_large_train_upselling. Etiketler</a>).
  </td>
</tr>

<tr>
  <td>Uçuş gecikmeleri verileri</td>
  <td>
Passenger, ABD 'nin ulaşım bölümünün (<a href="https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">zamanında</a>) geçiş Bakanlığı veri koleksiyonundan alınan zaman içindeki performans verilerini alır.
<p></p>
Veri kümesi Nisan-Ekim 2013 ' de geçen süreyi içerir. Tasarımcıya yüklemeden önce veri kümesi şu şekilde işlenir:
<ul>
  <li>Veri kümesi, kıst ABD 'de yalnızca 70 ortalamanızı havaalanları kapsayacak şekilde filtrelenmiştir</li>
  <li>İptal edilen fışıkları 15 dakikadan uzun bir süre gecikti olarak etiketlendi</li>
  <li>Ayrılan fışıkların ölçeği filtrelendi</li>
  <li>Şu sütunlar seçildi: Year, month, DayofMonth, DayOfWeek, taşıyıcı, Originairportıd, Destairportıd, CRSDepTime, DepDelay, DepDel15, CRSArrTime, ArrDelay, ArrDel15, Iptal edildi</li>
</ul>
</td>
</tr>

<tr>
  <td>Alman kredi kartı UCı veri kümesi</td>
  <td>
Alman. Data dosyasını kullanarak UCı Statgünlüğü (Almanya kredi kartı) veri kümesi (<a href="https://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">Statlog + Almanya + kredi + veri</a>).
<p></p>
Veri kümesi, bir öznitelik kümesiyle tanımlanan kişileri, düşük veya yüksek kredi riskleri olarak sınıflandırır. Her örnek bir kişiyi temsil eder. Hem sayısal hem de kategorik ve bir ikili etiketi (kredi risk değeri) 20 özellik vardır. Yüksek kredi risk girişlerinde etiket = 2, düşük kredi risk girişlerinde etiket = 1 vardır. Düşük riskli bir örneği yüksek olarak sınıflandırın maliyeti 1 ' dir, ancak yüksek riskli örnek 5 ' i yanlış sınıflandırma maliyeti 5 ' tir.
  </td>
</tr>

<tr>
  <td>IMDB film başlıkları</td>
  <td>
Veri kümesi Twitter 'da derecelendirilen filmler hakkında bilgiler içerir: ıMDB film KIMLIĞI, film adı, tarz ve üretim yılı. Veri kümesinde 17K filmler vardır. Veri kümesi "S" kağıdına tanıtılmıştı. Çizeneler, T. de Pessemier ve L. Maronlar. MovieTweetings: Twitter 'Dan toplanan bir film derecelendirmesi veri kümesi. Öneren sistemleri için Crowdsour, ve ınsan hesaplamasında Workshop, RecSys 2013. "
  </td>
</tr>

<tr>
  <td>Film derecelendirmeleri</td>
  <td>
Veri kümesi, film arası doldurulabilir veri kümesinin genişletilmiş bir sürümüdür. Veri kümesi, filmler için 170K derecelendirmesine sahiptir ve Twitter 'daki iyi yapılandırılmış ara kaynaklardan ayıklanır. Her örnek bir tweet temsil eder ve bir tanımlama grubu: Kullanıcı KIMLIĞI, ıMDB film KIMLIĞI, derecelendirme, zaman damgası, bu tweet için sık kullanılanlar sayısı ve bu tweet yeniden dengelendirme sayısı. DataSet, öneren Systems Challenge 2014 için bir. diyor, S. Çizms, B. Loni ve D. Tikk tarafından kullanılabilir hale getirilir.
  </td>
</tr>


<tr>
  <td>Hava durumu veri kümesi</td>
  <td>
NOAA 'den saatlik Land tabanlı hava durumu gözlemlerini (<a href="https://az754797.vo.msecnd.net/data/WeatherDataset.csv">201304 ile 201310 arasında Birleşik veriler</a>).
<p></p>
Hava durumu verileri, Havaalanı hava durumu istasyonlarından yapılan ve Nisan-Ekim 2013 zaman dilimini kapsayan gözlemlerini kapsamaktadır. Tasarımcıya yüklemeden önce veri kümesi şu şekilde işlenir:
<ul>
  <li>Hava durumu istasyonu kimlikleri ilgili Havaalanı kimliklerine eşlendi</li>
  <li>70 ortalamanızı Havaalanı ile ilişkilendirilmemiş Hava durumu istasyonları filtrelendi</li>
  <li>Tarih sütunu ayrı yıl, ay ve gün sütunlarına bölündü</li>
  <li>Şu sütunlar seçildi: Airportıd, Year, month, Day, Time, TimeZone, ufuk koşulunun, görünürlük, dalgalı bir tür, DryBulbFarenheit, Drybulbgrat, WetBulbFarenheit, Wetbulbgrat, DewPointFarenheit, Dewpointsanti, Relativenem, PressureTendency Speed, WınPressureChange Direction, Valueforwınpr karakteri, Stationbasınç,,, deniz Levelbasınç, RecordType, Hourlyprecıp, Altimeter</li>
</ul>
  </td>
</tr>

<tr>
  <td>Vikipedi SP 500 veri kümesi</td>
  <td>
Veriler, XML verileri olarak depolanan her bir S & P 500 şirketinin makalelerini temel alan Vikipden (<a href="https://www.wikipedia.org/">https://www.wikipedia.org/</a>) türetilir.
<p></p>
Tasarımcıya yüklemeden önce veri kümesi şu şekilde işlenir:
<ul>
  <li>Belirli bir şirkete ait metin içeriğini Ayıkla</li>
  <li>Wiki biçimlendirmesini kaldır</li>
  <li>Alfasayısal olmayan karakterleri kaldır</li>
  <li>Tüm metni küçük harfe Dönüştür</li>
  <li>Bilinen şirket kategorileri eklendi</li>
</ul>
<p></p>
Bazı şirketlerde bir makalenin bulunamadığını, bu nedenle kayıt sayısının 500 ' den küçük olduğunu unutmayın.
  </td>
</tr>

</table>

## <a name="next-steps"></a>Sonraki adımlar

* Öğretici ile tahmine dayalı analiz ve makine öğrenimi hakkında temel bilgileri öğrenin [: tasarımcı ile otomobil fiyatlarını tahmin](tutorial-designer-automobile-price-train-score.md) etme

* Örnek veri kümelerini içeri aktarmak için [veri alma](./algorithm-module-reference/import-data.md) modülünü kullanın
