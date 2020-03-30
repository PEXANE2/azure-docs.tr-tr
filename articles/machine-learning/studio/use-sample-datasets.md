---
title: Örnek veri kümelerini kullanma
titleSuffix: ML Studio (classic) - Azure
description: Machine Learning Studio (klasik) dahil örnek modellerde kullanılan veri kümelerinin açıklamaları. Bu örnek veri kümelerini denemeleriniz için kullanabilirsiniz.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 01/19/2018
ms.openlocfilehash: b7b8606f7f15f8d6fdd66681a1c7ade60ff506f3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79217775"
---
# <a name="use-the-sample-datasets-in-azure-machine-learning-studio-classic"></a>Azure Machine Learning Studio'daki örnek veri kümelerini kullanma (klasik)

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

[top]: #machine-learning-sample-datasets

Azure Machine Learning Studio'da (klasik) yeni bir çalışma alanı oluşturduğunuzda, varsayılan olarak bir dizi örnek veri kümesi ve deneme dahildir. Bu örnek veri kümelerinin çoğu [Azure AI Galerisi'ndeki](https://gallery.azure.ai/)örnek modeller tarafından kullanılır. Diğerleri genellikle makine öğreniminde kullanılan çeşitli veri türlerine örnek olarak dahildir.

Bu veri kümelerinden bazıları Azure Blob depolama alanında kullanılabilir. Bu veri kümeleri için aşağıdaki tablo doğrudan bir bağlantı sağlar. Bu veri kümelerini denemelerinizde [Veri Alma][import-data] modüllerini kullanarak kullanabilirsiniz.

Bu örnek veri kümelerinin geri **kalanı, Kayıtlı Veri Kümeleri**altında çalışma alanınızda kullanılabilir. Bunu Machine Learning Studio'da (klasik) deney tuvalinin solundaki modül paletinde bulabilirsiniz.
Bu veri kümelerinden herhangi birini kendi denemenizde, deneme tuvalinize sürükleyerek kullanabilirsiniz.

## <a name="datasets"></a>Veri kümeleri

<table>

<tr>
  <th>Veri kümesi adı</th>
  <th>Dataset açıklaması</th>
</tr>

<tr>
  <td>Yetişkin Nüfus Sayımı Gelir İkili Sınıflandırması veri seti</td>
  <td>
1994 Nüfus Sayımı veritabanının bir alt kümesi, 16 yaşın üzerindeki çalışan yetişkinler kullanılarak 100 > düzeltilmiş gelir indeksi ile.
<p></p>
<b>Kullanımı:</b> Bir kişinin yılda 50.000'den fazla kazanıp kazanmadığını tahmin etmek için demografik bilgileri kullanarak insanları sınıflandırın.
<p></p>
<b>İlgili Araştırmalar:</b> Kohavi, R., Becker, B., (1996). UCI Makine Öğrenme Deposu <a href="https://archive.ics.uci.edu/ml">https://archive.ics.uci.edu/ml</a>. Irvine, CA: Kaliforniya Üniversitesi, Bilgi ve Bilgisayar Bilimleri Fakültesi </td>
</tr>

<tr>
  <td>Havaalanı Kodları Dataset</td>
  <td>
Abd havaalanı kodları.
<p></p>
Bu veri kümesi, her ABD havaalanı için havaalanı kimlik numarası ve adını niçin konum şehri ve eyaletle birlikte sağlayan bir satır içerir.
  </td>
</tr>

<tr>
  <td>Otomobil fiyat verileri (Raw)</td>
  <td>
Otomobiller hakkında fiyat da dahil olmak üzere, silindir ve MPG sayısı gibi özellikler, yanı sıra bir sigorta risk puanı ile ilgili bilgiler.
<p></p>
Risk puanı başlangıçta otomatik fiyat ile ilişkilidir. Daha sonra aktüerler için sembol olarak bilinen bir süreçte gerçek risk için ayarlanır. +3 değeri, otomatikin riskli olduğunu ve -3 değerinin büyük olasılıkla güvenli olduğunu gösterir.
<p></p>
<b>Kullanımı:</b> Regresyon veya çok değişkenli sınıflandırma kullanarak risk puanını özelliklere göre tahmin edin. 
<p></p>
<b>İlgili Araştırmalar:</b> Schlimmer, J.C. (1987). UCI Makine Öğrenme Deposu <a href="https://archive.ics.uci.edu/ml">https://archive.ics.uci.edu/ml</a>. Irvine, CA: Kaliforniya Üniversitesi, Bilgi ve Bilgisayar Bilimleri Fakültesi </td>
</tr>

<tr>
  <td>Bisiklet Kiralama UCI veri seti</td>
  <td>
Washington DC'de bisiklet kiralama ağı tutan Capital Bikeshare şirketinin gerçek verilerine dayanan UCI Bike Rental veri seti.
<p></p>
Veri kümesi, 2011 ve 2012'de her günün her saati için toplam 17.379 satır için bir satıra sahiptir. Saatlik bisiklet kiralama çeşitleri 1 ile 977 arasındadır.

  </td>
</tr>

<tr>
  <td>Bill Gates RGB Görüntü</td>
  <td>
CSV verilerine dönüştürülen genel kullanıma açık resim dosyası.
<p></p>
Görüntüyü dönüştürme <strong>kodu, K-Means kümeleme</strong> modeli ayrıntı sayfası kullanılarak Renk nicelleştirmesinde sağlanır.
  </td>
</tr>

<tr>
  <td>Kan bağışı verileri</td>
  <td>
Hsin-Chu City, Tayvan Kan Transfüzyon Hizmet Merkezi kan bağışı veritabanından veri bir alt kümesi.
<p></p>
Bağış verileri, son bağıştan bu yana geçen ayları) ve sıklığı veya bağışların toplam sayısını, son bağıştan bu yana geçen süreyi ve bağışlanan kan miktarını içerir.
<p></p>
<b>Kullanımı:</b> Amaç, donörün Mart 2007'de kan bağışı yapıp etmediğini sınıflandırma yoluyla tahmin etmektir, burada 1 hedef dönemde bir donör gösterir, ve 0 olmayan bir donör. 
<p></p>
<b>İlgili Araştırmalar:</b> Yeh, I.C., (2008). UCI Makine Öğrenme Deposu <a href="https://archive.ics.uci.edu/ml">https://archive.ics.uci.edu/ml</a>. Irvine, CA: Kaliforniya Üniversitesi, Bilgi ve Bilgisayar Bilimleri Fakültesi
<p></p>
Yeh, I-Cheng, Yang, King-Jang ve Ting, Tao-Ming, "Bernoulli sırasını kullanarak RFM modeli nde bilgi keşfi, "Uygulamalarile Uzman Sistemler, 2008,<a href="https://dx.doi.org/10.1016/j.eswa.2008.07.018">https://dx.doi.org/10.1016/j.eswa.2008.07.018</a>
  </td>
</tr>

<tr>
  <td>Meme kanseri verileri</td>
  <td>
Onkoloji Enstitüsü tarafından sağlanan ve makine öğrenimi literatüründe sıkça görülen kanserle ilgili üç veri kümesinden biri. Tanısal bilgileri yaklaşık 300 doku örneğinin laboratuvar analizinden elde edilen özelliklerle birleştirir.
<p></p>
<b>Kullanımı:</b> Bazıları doğrusal, bazıları kategorik olan 9 özelliğine göre kanser türünü sınıflandırın. 
<p></p>
<b>İlgili Araştırmalar:</b> Wohlberg, W.H., Street, W.N., & Mangasarian, O.L. (1995). UCI Makine Öğrenme Deposu <a href="https://archive.ics.uci.edu/ml">https://archive.ics.uci.edu/ml</a>. Irvine, CA: Kaliforniya Üniversitesi, Bilgi ve Bilgisayar Bilimleri Fakültesi </td>
</tr>

<tr>
  <td>Meme Kanseri Özellikleri <td>
Veri seti, her biri 117 özellik tarafından açıklanan 102K şüpheli bölgelere (adaylara) ait X-Ray görüntülerinin bilgilerini içerir. Özellikleri tescilli ve anlamları dataset yaratıcıları (Siemens Healthcare) tarafından ortaya değildir. 
  </td>
</tr>

<tr>
  <td>Meme Kanseri Bilgi</td>
  <td>
Veri kümesi, X-Ray görüntüsünün her şüpheli bölgesi için ek bilgiler içerir. Her örnek, Meme Kanseri Özellikleri veri kümesinde karşılık gelen satır numarası hakkında bilgi (örneğin, etiket, hasta kimliği, tüm resme göre yama koordinatları) sağlar. Her hastanın bir dizi örneği vardır. Kanser olan hastalar için, bazı örnekler pozitif ve bazı negatif. Kanser olmayan hastalar için, tüm örnekler negatiftir. Veri kümesinde 102K örnekleri vardır. Veri kümesi önyargılı, puanların %0.6'sı pozitif, geri kalanı negatif. Veri seti Siemens Healthcare tarafından kullanıma sunulmuştur.
  </td>
</tr>

<tr>
  <td>CRM Appetency Etiketleri Paylaşıldı</td>
  <td>
Etiketler KDD Kupası 2009 müşteri ilişkileri tahmin sorun<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_appetency.labels">(orange_small_train_appetency.labels).</a>
  </td>
</tr>

<tr>
  <td>CRM Churn Etiketleri Paylaşıldı</td>
  <td>
Etiketler KDD Kupası 2009 müşteri ilişkileri tahmin sorun<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_churn.labels">(orange_small_train_churn.labels).</a>
  </td>
</tr>

<tr>
  <td>CRM Dataset Paylaşıldı</td>
  <td>
Bu veriler KDD Cup 2009 müşteri ilişkileri tahmini meydan<a href="http://www.sigkdd.org/site/2009/files/orange_small_train.data.zip">(orange_small_train.data.zip)</a>geliyor.
<p></p>
Veri seti, Fransız Telekom şirketi Orange'dan 50K müşteri içerir. Her müşterinin 190'ı sayısal, 40'ı kategorik olmak üzere 230 anonim leştirilmiş özelliği vardır. Özellikleri çok seyrek.
  </td>
</tr>

<tr>
  <td>CRM Upselling Etiketleri Paylaşıldı</td>
  <td>
Etiketler KDD Kupası 2009 müşteri ilişkileri tahmin sorun<a href="http://www.sigkdd.org/site/2009/files/orange_large_train_upselling.labels">(orange_large_train_upselling.labels).</a>
  </td>
</tr>

<tr>
  <td>Enerji Verimliliği Regresyon verileri</td>
  <td>
12 farklı bina şekline dayanan simüle edilmiş enerji profilleri koleksiyonu. Binalar sekiz özelliğe göre ayırt edilmiştir. Buna cam alanı, cam alanı dağılımı ve oryantasyon dahildir.
<p></p>
<b>Kullanımı:</b> İki gerçek değerli yanıttan biri olarak temel alan enerji verimliliği derecelendirmesini tahmin etmek için regresyon veya sınıflandırma kullanın. Çok sınıflı sınıflandırma için, yanıt değişkenini en yakın sayacına yuvarlar. 
<p></p>
<b>İlgili Araştırmalar:</b> Xifara, A. & Tsanas, A. (2012). UCI Makine Öğrenme Deposu <a href="https://archive.ics.uci.edu/ml">https://archive.ics.uci.edu/ml</a>. Irvine, CA: Kaliforniya Üniversitesi, Bilgi ve Bilgisayar Bilimleri Fakültesi </td>
</tr>

<tr>
  <td>Uçuş Gecikmeleri Verileri</td>
  <td>
Abd Ulaştırma Bakanlığı'nın TranStats veri koleksiyonundan alınan yolcu uçuşu zamanında performans verileri<a href="https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">(Zamanında).</a>
<p></p>
Veri kümesi Nisan-Ekim 2013 dönemini kapsar. Azure Machine Learning Studio'ya (klasik) yüklemeden önce veri kümesi aşağıdaki gibi işlenmiştir:
<ul>
  <li>Veri seti kıta ABD'de sadece 70 en işlek havaalanları kapsayacak şekilde filtrelendi</li>
  <li>İptal edilen uçuşlar 15 dakikadan fazla gecikmeli olarak etiketlendi</li>
  <li>Yönlendirilen uçuşlar filtrelendi</li>
  <li>Aşağıdaki sütunlar seçildi: Yıl, Ay, DayofMonth, DayOfWeek, Carrier, OriginAirportID, DestAirportID, CRSDepTime, DepDelay, DepDel15, CRSArrTime, ArrDelay, ArrDel15, İptal edildi</li>
</ul>
</td>
</tr>

<tr>
  <td>Uçuş zamanında performans (Raw)</td>
  <td>
Ekim 2011'den itibaren Amerika Birleşik Devletleri içinde uçak uçuşlarının geliş ve gidişlerinin kayıtları.
<p></p>
<b>Kullanımı:</b> Uçuş gecikmelerini tahmin edin. 
<p></p>
<b>İlgili Araştırmalar:</b> ABD Ulaştırma <a href="https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time</a>Bakanlığı'ndan.
  </td>
</tr>

<tr>
  <td>Orman yangınları veri</td>
  <td>
Sıcaklık ve nem endeksleri ve rüzgar hızı gibi hava durumu verilerini içerir. Veriler, Portekiz'in kuzeydoğusundaki bir bölgeden alınarak orman yangınlarının kayıtlarıyla birleştirildi.
<p></p>
<b>Kullanımı:</b> Bu zor bir regresyon görevi, amacı orman yangınları yanmış alanı tahmin etmektir. 
<p></p>
<b>İlgili Araştırmalar:</b> Cortez, P., & Morais, A. (2008). UCI Makine Öğrenme Deposu <a href="https://archive.ics.uci.edu/ml">https://archive.ics.uci.edu/ml</a>. Irvine, CA: Kaliforniya Üniversitesi, Bilgi ve Bilgisayar Bilimleri Fakültesi
<p></p>
[Cortez ve Morais, 2007] P. Cortez ve A. Morais. Meteorolojik Verileri Kullanarak Orman Yangınlarını Tahmin Etmek Için Veri Madenciliği Yaklaşımı. J. Neves,M. F. Santos ve J. Machado Eds., Yapay Zekada Yeni Trendler, 13. APPIA, ISBN-13 978-989-95618-0-9. Kullanılabilir: <a href="http://www.dsi.uminho.pt/~pcortez/fires.pdf">http://www.dsi.uminho.pt/~pcortez/fires.pdf</a>.
  </td>
</tr>

<tr>
  <td>Alman Kredi Kartı UCI veri seti</td>
  <td>
UCI Statlog (Alman Kredi Kartı) veri seti (<a href="https://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">Statlog+German+Credit+Data</a>), german.data dosyasını kullanarak.
<p></p>
Veri kümesi, bir dizi öznitelik tarafından açıklanan kişileri düşük veya yüksek kredi riskleri olarak sınıflandırılır. Her örnek bir kişiyi temsil eder. Sayısal ve kategorik 20 özellik ve ikili etiket (kredi risk değeri) vardır. Yüksek kredi riski girişleri etiketi = 2, düşük kredi riski girişleri etiketi = 1 var. Düşük risk örneğini yüksek olarak sınıflandırmanın maliyeti 1'dir, yüksek risk örneğini düşük olarak sınıflandırmanın maliyeti 5'tir.
  </td>
</tr>

<tr>
  <td>IMDB Film Başlıkları</td>
  <td>
Dataset, Twitter tweetlerinde derecelendirilen filmler hakkında bilgi içerir: IMDB film kimliği, film adı, tür ve yapım yılı. Veri setinde 17K film vardır. Veri seti "S. " adlı kağıtta tanıtıldı. Dooms, T. De Pessemier ve L. Martens. MovieTweetings: Twitter'dan Toplanan Bir Film Derecelendirme Dataset. Tavsiye Sistemleri için Crowdsourcing ve İnsan Hesaplama Çalıştayı, RecSys 2013'te CrowdRec."
  </td>
</tr>

<tr>
  <td>Iris iki sınıf veri</td>
  <td>
Bu belki de desen tanıma literatüründe bulunan en iyi bilinen veritabanıdır. Veri kümesi nispeten küçüktür ve her biri üç iris çeşidine ait yaprak ölçümlerinden 50'şer örnek içerir.
<p></p>
<b>Kullanımı:</b> Ölçümlerden iris türünü tahmin edin.  
<p></p>
<b>İlgili Araştırmalar:</b> Fisher, R.A. (1988). UCI Makine Öğrenme Deposu <a href="https://archive.ics.uci.edu/ml">https://archive.ics.uci.edu/ml</a>. Irvine, CA: Kaliforniya Üniversitesi, Bilgi ve Bilgisayar Bilimleri Fakültesi </td>
</tr>

<tr>
  <td>Film Tweets</td>
  <td>
Dataset, Movie Tweetings veri kümesinin genişletilmiş bir sürümüdür. Dataset, Twitter'daki iyi yapılandırılmış tweetlerden çıkarılan filmler için 170K derecelendirmeye sahiptir. Her örnek bir tweet temsil eder ve bir tuple: kullanıcı kimliği, IMDB film kimliği, derecelendirme, zaman damgası, bu tweet için sık sayısı ve bu tweet retweets sayısı. Dataset, Tavsiye Sistemleri Mücadelesi 2014 için A. Said, S. Dooms, B. Loni ve D. Tikk tarafından kullanıma sunulmuştur.
  </td>
</tr>

<tr>
  <td>Çeşitli otomobiller için MPG verileri</td>
  <td>
Bu veri kümesi, Carnegie Mellon Üniversitesi StatLib kütüphanesi tarafından sağlanan veri kümesinin biraz değiştirilmiş bir sürümüdür. Dataset 1983 Amerikan İstatistik Birliği Fuarı'nda kullanılmıştır.
<p></p>
Veriler galon başına mil çeşitli otomobiller için yakıt tüketimi listeler. Ayrıca silindir sayısı, motor deplasmanı, beygir gücü, toplam ağırlık ve ivme gibi bilgileri içerir.
<p></p>
<b>Kullanımı:</b> Üç çok değerli ayrıöz özniteliklere ve beş sürekli özdeme dayalı yakıt ekonomisini tahmin edin. 
<p></p>
<b>İlgili Araştırmalar:</b> StatLib, Carnegie Mellon Üniversitesi, (1993). UCI Makine Öğrenme Deposu <a href="https://archive.ics.uci.edu/ml">https://archive.ics.uci.edu/ml</a>. Irvine, CA: Kaliforniya Üniversitesi, Bilgi ve Bilgisayar Bilimleri Fakültesi </td>
</tr>

<tr>
  <td>Pima Indians Diyabet İkili Sınıflandırma dataset</td>
  <td>
Ulusal Diyabet Enstitüsü ve Sindirim ve Böbrek Hastalıkları veritabanından bir veri alt kümesi. Veri seti Pima Hint kökenli kadın hastalara odaklanmak için filtrelendi. Veriler, glikoz ve insülin düzeyleri gibi tıbbi verilerin yanı sıra yaşam tarzı faktörlerini de içermektedir.
<p></p>
<b>Kullanımı:</b> Deneğin diyabeti olup olmadığını tahmin edin (ikili sınıflandırma). 
<p></p>
<b>İlgili Araştırmalar:</b> Sigillito, V. (1990). UCI Makine Öğrenme Deposu <a href="https://archive.ics.uci.edu/ml"> https://archive.ics.uci.edu/ml"</a>. Irvine, CA: Kaliforniya Üniversitesi, Bilgi ve Bilgisayar Bilimleri Fakültesi </td>
</tr>

<tr>
  <td>Restoran müşteri verileri</td>
  <td>
Demografi ve tercihler de dahil olmak üzere müşterilerle ilgili bir dizi meta veri.
<p></p>
<b>Kullanımı:</b> Bir tavsiye sistemini eğitmek ve test etmek için diğer iki restoran veri kümesiyle birlikte bu veri kümesini kullanın. 
<p></p>
<b>İlgili Araştırmalar:</b> Bache, K. ve Lichman, M. (2013). UCI Makine Öğrenme Deposu <a href="https://archive.ics.uci.edu/ml">https://archive.ics.uci.edu/ml</a>. Irvine, CA: Kaliforniya Üniversitesi, Bilgi ve Bilgisayar Bilimleri Fakültesi.
  </td>
</tr>

<tr>
  <td>Restoran özelliği verileri</td>
  <td>
Restoranlar ve özellikleri hakkında gıda türü, yemek stili ve konum gibi bir dizi meta veri.
<p></p>
<b>Kullanımı:</b> Bir tavsiye sistemini eğitmek ve test etmek için diğer iki restoran veri kümesiyle birlikte bu veri kümesini kullanın. 
<p></p>
<b>İlgili Araştırmalar:</b> Bache, K. ve Lichman, M. (2013). UCI Makine Öğrenme Deposu <a href="https://archive.ics.uci.edu/ml">https://archive.ics.uci.edu/ml</a>. Irvine, CA: Kaliforniya Üniversitesi, Bilgi ve Bilgisayar Bilimleri Fakültesi.
  </td>
</tr>

<tr>
  <td>Restoran derecelendirmeleri</td>
  <td>
Kullanıcılar tarafından restoranlara 0 ile 2 arasında ölçekte verilen derecelendirmeleri içerir.
<p></p>
<b>Kullanımı:</b> Bir tavsiye sistemini eğitmek ve test etmek için diğer iki restoran veri kümesiyle birlikte bu veri kümesini kullanın. 
<p></p>
<b>İlgili Araştırmalar:</b> Bache, K. ve Lichman, M. (2013). UCI Makine Öğrenme Deposu <a href="https://archive.ics.uci.edu/ml">https://archive.ics.uci.edu/ml</a>. Irvine, CA: Kaliforniya Üniversitesi, Bilgi ve Bilgisayar Bilimleri Fakültesi.
  </td>
</tr>

<tr>
  <td>Çelik Tavlama çok sınıflı veri seti</td>
  <td>
Bu veri kümesi, çelik tavlama denemelerinden bir dizi kayıt içerir. Ortaya çıkan çelik türlerinin fiziksel özelliklerini (genişlik, kalınlık, tip (bobin, levha, vb.) içerir.
<p></p>
<b>Kullanımı:</b> İki sayısal sınıf özniteliklerinden herhangi birini tahmin edin; sertlik veya güç. Öznitelikler arasındaki bağıntıları da çözümleyebilir.
<p></p>
Çelik kaliteleri, SAE ve diğer kuruluşlar tarafından tanımlanan bir standart izler. Belirli bir 'sınıf' (sınıf değişkeni) arıyorsunuz ve gereken değerleri anlamak istiyorsunuz. 
<p></p>
<b>İlgili Araştırmalar:</b> Sterling, D. & Buntine, W. (NA). UCI Makine Öğrenme Deposu <a href="https://archive.ics.uci.edu/ml">https://archive.ics.uci.edu/ml</a>. Irvine, CA: Kaliforniya Üniversitesi, Bilgi ve Bilgisayar Bilimleri Fakültesi
<p></p>
Çelik kaliteleri için yararlı bir kılavuz burada bulunabilir:<a href="https://otk-sitecore-prod-v2-cdn.azureedge.net/-/media/from-sharepoint/documents/product/outokumpu-steel-grades-properties-global-standards.pdf">https://otk-sitecore-prod-v2-cdn.azureedge.net/-/media/from-sharepoint/documents/product/outokumpu-steel-grades-properties-global-standards.pdf</a>
  </td>
</tr>

<tr>
  <td>Teleskop verileri</td>
  <td>
Her ikisi de Monte Carlo işlemi kullanılarak simüle edilen arka plan gürültüsüyle birlikte yüksek enerjili gama parçacık patlamalarının kaydı.
<p></p>
Simülasyonun amacı yer tabanlı atmosferik Cherenkov gama teleskoplarının doğruluğunu artırmaktı. Bu istenilen sinyal (Cherenkov radyasyon duş) ve arka plan gürültüsü (üst atmosferde kozmik ışınlar tarafından başlatılan hadronik duş) arasında ayrım yapmak için istatistiksel yöntemler kullanılarak yapılır.
<p></p>
Uzun ekseni kamera merkezine doğru yönlendirilmiş uzun bir küme oluşturmak için veriler önceden işlenmiş. Bu elipsin özellikleri (genellikle Hillas parametreleri olarak adlandırılır) ayrımcılık için kullanılabilecek görüntü parametreleri arasındadır.
<p></p>
<b>Kullanımı:</b> Duş görüntüsünün sinyal mi yoksa arka plan gürültüsümü temsil edip etmediğini tahmin edin.
<p></p>
<b>Notlar:</b> Bir arka plan olayını sinyal olarak sınıflandırmak, bir sinyal olayını arka plan olarak sınıflandırmaktan daha kötü olduğundan, basit sınıflandırma doğruluğu bu veriler için anlamlı değildir. Farklı sınıflandırıcıların karşılaştırılması için ROC grafiği kullanılmalıdır. Bir arka plan olayını sinyal olarak kabul etme olasılığı aşağıdaki eşiklerden birinin altında olmalıdır: 0,01, 0,02, 0,05, 0,1 veya 0,2.
<p></p>
Ayrıca, arka plan olaylarının sayısı (h, hadronik duşlar için) hafife olduğunu unutmayın. Gerçek ölçümlerde, h veya gürültü sınıfı olayların çoğunluğunu temsil eder. 
<p></p>
<b>İlgili Araştırmalar:</b> Bock, R.K. (1995). UCI Makine Öğrenme Deposu <a href="https://archive.ics.uci.edu/ml">https://archive.ics.uci.edu/ml</a>. Irvine, CA: Kaliforniya Üniversitesi, Bilgi Okulu </td>
</tr>

<tr>
  <td>Hava Durumu Dataset</td>
  <td>
NOAA'dan saatlik kara tabanlı hava gözlemleri<a href="https://az754797.vo.msecnd.net/data/WeatherDataset.csv">(201304 ile 201310 verileri birleştirilmiştir).</a>
<p></p>
Hava durumu verileri, Nisan-Ekim 2013 dönemini kapsayan havaalanı hava istasyonlarından yapılan gözlemleri kapsamaktadır. Azure Machine Learning Studio'ya (klasik) yüklemeden önce veri kümesi aşağıdaki gibi işlenmiştir:
<ul>
  <li>Meteoroloji istasyonu işleri ilgili hava alanı işleri işlemişti</li>
  <li>En işlek 70 havaalanıyla ilişkili olmayan hava istasyonları filtrelendi</li>
  <li>Tarih sütunu ayrı Yıl, Ay ve Gün sütunlarına bölündü</li>
  <li>Aşağıdaki sütunlar seçildi: AirportID, Yıl, Ay, Gün, Saat Zone, SkyCondition, Görünürlük, WeatherType, DryBulbFarenheit, DryBulbCelsius, WetBulbFarenheit, WetBulbCelsius, DewPointFarenheit, DewPointFarenheit, DewPointCelsius, RelativeHumidity, WindSpeed, WindDirection, ValueForWindCharacter, StationPressure, PressureTendency, PressureChange, SeaLevelPressure, RecordType, HourlyPrecip, Altimeter</li>
</ul>
  </td>
</tr>

<tr>
  <td>Vikipedi SP 500 Veri Seti</td>
  <td>
Veriler, XML<a href="https://www.wikipedia.org/">https://www.wikipedia.org/</a>verisi olarak depolanan her S&P 500 şirketinin makalelerine dayanan Wikipedia 'dan türetilmiştir.
<p></p>
Azure Machine Learning Studio'ya (klasik) yüklemeden önce veri kümesi aşağıdaki gibi işlenmiştir:
<ul>
  <li>Her şirket için metin içeriğini ayıklama</li>
  <li>Wiki biçimlendirmeyi kaldırma</li>
  <li>Alfanümerik olmayan karakterleri kaldırma</li>
  <li>Tüm metni küçük harfe dönüştürme</li>
  <li>Bilinen şirket kategorileri eklendi</li>
</ul>
<p></p>
Bazı şirketler için bir makalebulunamadı, bu nedenle kayıt sayısının 500'den az olduğunu unutmayın.
  </td>
</tr>

<tr>
  <td><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/direct_marketing.csv">direct_marketing.csv</a></td>
  <td>
Veri kümesi, müşteri verilerini ve doğrudan posta kampanyasına verdikleri yanıtla ilgili göstergeler içerir. Her satır bir müşteriyi temsil eder. Veri kümesi, kullanıcı demografisi ve geçmiş davranışla ilgili dokuz özellik ve üç etiket sütunu (ziyaret, dönüştürme ve harcama) içerir.  Ziyaret, bir müşterinin pazarlama kampanyasından sonra ziyaret ettiğini gösteren ikili bir sütundur. Dönüşüm, müşterinin bir şey satın aldığını gösterir. Harcama, harcanan miktardır.  Veri seti Kevin Hillstrom tarafından MineThatData E-Posta Analizi ve Veri Madenciliği Mücadelesi için kullanıma sunulmuştur.
  </td>
</tr>

<tr>
  <td><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_test.csv">lyrl2004_tokens_test.csv</a></td>
  <td>
RCV1-V2 Reuters haber veri setindetest örneklerinin özellikleri. Dataset'te 781K haber makaleleri ve kimliklerinin (veri kümesinin ilk sütunu) bulunur. Her makale belirtilmiş, durdurulan ve köklenmiş. Veri kümesi David tarafından kullanıma sunuldu. D. Lewis.
  </td>
</tr>

<tr>
  <td><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_train.csv">lyrl2004_tokens_train.csv</a></td>
  <td>
RCV1-V2 Reuters haber veri setinde eğitim örneklerinin özellikleri. Dataset'te 23K haber makaleleri ve kimliklerinin (veri kümesinin ilk sütunu) bulunur. Her makale belirtilmiş, durdurulan ve köklenmiş. Veri kümesi David tarafından kullanıma sunuldu. D. Lewis.
  </td>
</tr>

<tr>
  <td><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a><br></td>
  <td>
KDD Kupası 1999 Bilgi Bulma ve Veri Madenciliği Araçları Yarışması<a href="https://kdd.ics.uci.edu/databases/kddcup99/kddcup99.html">(kddcup99.html)</a>gelen Dataset .
<p></p>
Veri kümesi azure blob depolama<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">(network_intrusion_detection.csv)</a>tarafından indirilmiş ve depolanmış ve hem eğitim hem de test veri kümelerini içerir. Eğitim veri kümesi, etiketler de dahil olmak üzere yaklaşık 126K satırve 43 sütuna sahiptir. Üç sütun etiket bilgilerinin bir parçasıdır ve modeli eğitmek için sayısal ve dize/kategorik özelliklerden oluşan 40 sütun kullanılabilir. Test verileri, eğitim verilerinde olduğu gibi 43 sütuna sahip yaklaşık 22,5K test örneklerine sahiptir.
  </td>
</tr>

<tr>
  <td><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/rcv1-v2.topics.qrels.csv">rcv1-v2.topics.qrels.csv</a></td>
  <td>
RCV1-V2 Reuters haber veri setinde haber makaleleri için konu atamaları. Bir haber makalesi çeşitli konulara atanabilir. Her satırın biçimi&lt;"&gt; &lt;konu&gt; adı belge id 1"dir. Veri kümesi 2,6M konu atamaları içerir. Veri kümesi David tarafından kullanıma sunuldu. D. Lewis.
  </td>
</tr>

<tr>
  <td><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a></td>
  <td>
Bu veriler KDD Cup 2010 Öğrenci performans değerlendirme mücadelesinden<a href="https://www.kdd.org/kdd-cup/view/kdd-cup-2010-student-performance-evaluation">(öğrenci performans değerlendirmesi)</a>elde edilmektedir. Kullanılan veriler Algebra_2008_2009 eğitim setidir (Stamper, J., Niculescu-Mizil, A., Ritter, S., Gordon, G.J., & Koedinger, K.R. (2010). Cebir I 2008-2009. KDD Cup 2010 Eğitim veri madenciliği mücadelesi nden gelen veri setine meydan okuyun. <a href="https://pslcdatashop.web.cmu.edu/KDDCup/downloads.jsp">downloads.jsp</a>de bulun.
<p></p>
Veri kümesi indirildi ve Azure Blob depolama<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">(student_performance.txt)</a>depolandı ve bir öğrenci özel ders sisteminden günlük dosyaları içerir. Sağlanan özellikler, sorunlu kimliği ve kısa açıklamasını, öğrenci kimliğini, zaman damgasını ve öğrencinin sorunu doğru şekilde çözmeden önce kaç deneme de yaptığını içerir. Orijinal veri kümesi 8.9M kayıtlarına sahiptir; bu veri kümesi ilk 100K satırlarına indirilmiştir. Veri kümesinde çeşitli türlerde 23 sekme ayrılmış sütun vardır: sayısal, kategorik ve zaman damgası.
  </td>
</tr>

</table>

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Denemelerinizi örneklerle başlatın](sample-experiments.md)

<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
