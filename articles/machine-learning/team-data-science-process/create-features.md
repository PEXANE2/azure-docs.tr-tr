---
title: Veri biliminde özellik mühendisliği - Takım Veri Bilimi Süreci
description: Özellik mühendisliğinin amaçlarını açıklar ve makine öğreniminin veri geliştirme sürecindeki rolüne örnekler sağlar.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 02f109f250fa9bcd4c77cecd0b1b3e4514ecd8bc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76721141"
---
# <a name="feature-engineering-in-data-science"></a>Veri biliminde özellik mühendisliği
Bu makalede, özellik mühendisliğinin amaçları açıklanmaktadır ve makine öğreniminin veri geliştirme sürecindeki rolüne örnekler verilmiştir. Bu işlemi göstermek için kullanılan örnekler Azure Machine Learning Studio'dan alınmıştır. 

Bu [görev, Ekip Veri Bilimi Süreci'nin (TDSP)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/)bir adımıdır.

Özellik mühendisliği, öğrenme sürecini kolaylaştıran ham verilerden özellikler oluşturarak öğrenme algoritmalarının tahmin gücünü artırmaya çalışır. Özelliklerin mühendislik ve seçimi, Team Data Science Process [yaşam döngüsü nedir?](overview.md) Özellik mühendisliği ve seçimi, TDSP'nin **Özellikler Geliştirme** adımının bir parçasıdır. 

* **özellik mühendisliği**: Bu işlem, verilerdeki mevcut ham özelliklerden ek ilgili özellikler oluşturmaya ve öğrenme algoritmasının tahmin gücünü artırmaya çalışır.
* **özellik seçimi**: Bu işlem, eğitim sorununun boyutlarını azaltmak amacıyla özgün veri özelliklerinin temel alt kümesini seçer.

Normalde **özellik mühendisliği** ek özellikler oluşturmak için ilk uygulanır ve daha sonra **özellik seçimi** adımı alakasız, gereksiz veya yüksek korelasyon özellikleri ortadan kaldırmak için gerçekleştirilir.

Makine öğreniminde kullanılan eğitim verileri genellikle toplanan ham verilerden alınan özelliklerin çıkarılmasıyla geliştirilebilir. El yazısı karakterlerin görüntülerini sınıflandırmayı öğrenme bağlamında tasarlanmış bir özelliğin bir örneği, ham bit dağıtım verilerinden oluşturulmuş bit yoğunluklu bir haritanın oluşturulmasıdır. Bu harita, doğrudan ham dağıtımı kullanmaktan daha verimli karakterlerin kenarlarını bulmada yardımcı olabilir.

Belirli ortamlardaki veriler için özellikler oluşturmak için aşağıdaki makalelere bakın:

* [SQL Server'da veri için özellikler oluşturma](create-features-sql-server.md)
* [Hive sorgularını kullanarak Hadoop kümesindeki veriler için özellikler oluşturma](create-features-hive.md)

## <a name="create-features-from-your-data---feature-engineering"></a>Verilerinizden özellikler oluşturun - özellik mühendisliği
Eğitim verileri, her biri bir dizi özelliğe (sütunlarda depolanan değişkenler veya alanlar) sahip olan örneklerden (satırlar halinde depolanan kayıtlar veya gözlemler) oluşan bir matristen oluşur. Deneysel tasarımda belirtilen özelliklerin verilerdeki desenleri karakterize etmesi beklenmektedir. Ham veri alanlarının çoğu bir modeli eğitmek için kullanılan seçili özellik kümesine doğrudan dahil edilebilse de, gelişmiş bir eğitim veri kümesi oluşturmak için ham verilerdeki özelliklerden ek (mühendislik) özelliklerin oluşturulması gerekir.

Bir modeli eğitirken veri kümesini geliştirmek için ne tür özellikler oluşturulmalıdır? Eğitimi geliştiren mühendislik özellikleri, verilerdeki desenleri daha iyi ayırt eden bilgiler sağlar. Yeni özelliklerin, özgün veya varolan özellik kümesinde açıkça yakalanmayan veya kolayca belirgin olmayan ek bilgiler sağlaması beklenir. Ama bu süreç bir sanat eseridir. Sağlam ve üretken kararlar genellikle bazı etki alanı uzmanlığı gerektirir.

Azure Machine Learning ile başlarken, Stüdyo'da sağlanan örnekleri kullanarak bu işlemi somut olarak kavramak en kolayıdır. Burada iki örnek sunulmuştur:

* Bir regresyon örneği Hedef değerlerin bilindiği denetlenen bir deneydeki [bisiklet kiralama sayısının](https://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4) tahmini
* [Özellik Karma'yı](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) kullanarak metin madenciliği sınıflandırma örneği

## <a name="example-1-add-temporal-features-for-a-regression-model"></a>Örnek 1: Bir regresyon modeli için zamansal özellikler ekleme
Bir regresyon görevi için özelliklerin nasıl tasarlandığını göstermek için Azure Machine Learning Studio'daki (klasik) "Bisikletlerin talep tahmini" denemesini kullanalım. Bu deneyin amacı, bisikletlere olan talebi, yani belirli bir ay/gün/saat içinde bisiklet kiralama sayısını tahmin etmektir. "Bike Rental UCI dataset" veri kümesi ham giriş verileri olarak kullanılır. Bu veri kümesi, Abd'de Washington DC'de bisiklet kiralama ağı tutan Capital Bikeshare şirketinin gerçek verilerine dayanmaktadır. Veri kümesi, 2011 ve 2012 yılında günün belirli bir saatinde bisiklet kiralama sayısını temsil eder ve 17379 satır ve 17 sütun içerir. Ham özellik seti hava koşulları (sıcaklık/nem/rüzgar hızı) ve günün türünü (tatil/hafta içi) içerir. Tahmin etmek için alan belirli bir saat içinde bisiklet kiralama temsil eden ve 1 ile 977 arasında değişen 'cnt' sayısıdır.

Eğitim verilerinde etkili özellikler oluşturmak amacıyla, dört regresyon modeli aynı algoritma kullanılarak ancak dört farklı eğitim veri kümesi yle oluşturulur. Dört veri kümesi aynı ham giriş verilerini temsil, ancak artan sayıda özellik kümesi ile. Bu özellikler dört kategoriye ayrılır:

1. A = hava + tatil + hafta içi + öngörülen gün için hafta sonu özellikleri
2. B = önceki 12 saatin her birinde kiralanan bisiklet sayısı
3. C = önceki 12 günün her birinde aynı saatte kiralanan bisiklet sayısı
4. D = önceki 12 haftanın her birinde aynı saat ve aynı gün kiralanan bisiklet sayısı

Orijinal ham verilerde zaten var olan özellik kümesi A'nın yanı sıra, diğer üç özellik kümesi özellik mühendisliği işlemi yle oluşturulur. Özellik kümesi B, bisikletlere yönelik son talebi yakalar. Özellik kümesi C, belirli bir saatte bisiklete olan talebi yakalar. Özellik seti D, belirli bir saatte ve haftanın belirli bir gününde bisikletlere olan talebi yakalar. Dört eğitim veri kümesinin her biri sırasıyla A, A+B, A+B+C ve A+B+C+D özellik kümesini içerir.

Azure Machine Learning deneyinde, bu dört eğitim veri kümesi önceden işlenmiş giriş veri kümesinden dört dal aracılığıyla oluşturulur. En sol dal dışında, bu dalların her biri, türemiş özelliklerin (B, C ve D özellik kümesi) sırasıyla oluşturuldurıldığı ve içe aktarılan veri kümesine eklendiği bir [Execute R Script](https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/) modülü içerir. Aşağıdaki şekilde, ikinci sol dalda B özelliği kümesi oluşturmak için kullanılan R komut dosyası gösterin.

![özellikler oluşturma](./media/create-features/addFeature-Rscripts.png)

Dört modelin performans sonuçlarının karşılaştırılması aşağıdaki tabloda özetlenmiştir: 

![sonuç karşılaştırması](./media/create-features/result1.png)

En iyi sonuçlar A+B+C özellikleriyle gösterilir. Ek özellik kümesi eğitim verilerine dahil edildiğinde hata oranı azalır. B, C özellik kümesinin regresyon görevi için ek ilgili bilgiler sağladığı varsayımını doğrular. Ancak D özelliğinin eklenmesi hata oranında ek bir azalma sağlamaz.

## <a name="example-2-creating-features-in-text-mining"></a><a name="example2"></a>Örnek 2: Metin madenciliğinde özellikler oluşturma
Özellik mühendisliği, belge sınıflandırması ve duyarlılık analizi gibi metin madenciliği ile ilgili görevlerde yaygın olarak uygulanır. Örneğin, belgeleri çeşitli kategorilere sınıflandırmak istediğinizde, tipik bir varsayım, bir doküman kategorisinde yer alan sözcük/tümceciklerin başka bir doküman kategorisinde oluşma olasılığının daha düşük olduğudur. Başka bir deyişle, sözcük/tümcecik dağılımının sıklığı farklı belge kategorilerini karakterize eder. Metin madenciliği uygulamalarında, metin içeriğinin tek tek parçaları genellikle giriş verisi olarak hizmet verdiğinden, sözcük/tümcecik sıklıklarını içeren özellikleri oluşturmak için özellik mühendisliği işlemi gereklidir.

Bu görevi başarmak için, rasgele metin özelliklerini indekslere verimli bir şekilde dönüştürmek için **özellik karma** adlı bir teknik uygulanır. Bu yöntem, her metin özelliğini (sözcük/tümcecikler) belirli bir dizinle ilişkilendirmek yerine, özelliklere karma işlev uygulayarak ve karma değerlerini doğrudan endeks olarak kullanarak işlev görür.

Azure Machine Learning'de, sözcük/tümcecik özelliklerini rahatça oluşturan bir [Özellik Karma](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) modülü vardır. Aşağıdaki şekil bu modülü kullanarak bir örnek gösterir. Giriş veri kümesi iki sütun içerir: 1 ile 5 arasında değişen kitap derecelendirmesi ve gerçek inceleme içeriği. Bu Özellik [Karma](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) modülünün amacı, ilgili kitap incelemesinde karşılık gelen sözcüğün(ler)/tümcecik(ler) oluşum sıklığını gösteren bir grup yeni özellik almaktır. Bu modülü kullanmak için aşağıdaki adımları tamamlayın:

* İlk olarak, giriş metnini içeren sütunu seçin ("Bu örnekte"Col2").
* İkinci olarak, "Hashing bitsize"ı 8 olarak ayarlayın, bu da 2^8=256 özelliklerin oluşturulacağı anlamına gelir. Tüm metindeki sözcük/aşama 256 endekse işlenecektir. "Karma bit boyutu" parametresi 1 ile 31 arasında değişir. Sözcük(ler)/tümcecik(ler) daha büyük bir sayı olarak ayarlanırsa aynı dizin içinde haşlanması daha düşüktür.
* Üçüncü olarak, "N-gram" parametresini 2 olarak ayarlayın. Bu değer, giriş metninden tekagramların (her bir sözcük için bir özellik) ve bigramların (her bitişik sözcük çifti için bir özellik) oluşum sıklığını alır. "N-gram" parametresi 0 ile 10 arasında değişir ve bu da bir özelliğe eklenecek en fazla sıralı sözcük sayısını gösterir.  

!["Özellik Karma" modülü](./media/create-features/feature-Hashing1.png)

Aşağıdaki şekil, bu yeni özelliğin nasıl göründüğünü gösterir.

!["Özellik Karma" örneği](./media/create-features/feature-Hashing2.png)

## <a name="conclusion"></a>Sonuç
Tasarlanmış ve seçilmiş özellikler, verilerde yer alan temel bilgileri ayıklamayı amaçlayan eğitim sürecinin verimliliğini artırır. Ayrıca, giriş verilerini doğru bir şekilde sınıflandırmak ve ilginin sonuçlarını daha sağlam bir şekilde tahmin etmek için bu modellerin gücünü artırırlar. Özellik mühendisliği ve seçimi de öğrenme daha hesaplamalı olarak traktör yapmak için birleştirebilir. Bunu, bir modeli kalibre etmek veya eğitmek için gereken özelliklerin sayısını artırarak ve azaltarak yapar. Matematiksel olarak konuşursak, modeli eğitmek için seçilen özellikler, verilerdeki desenleri açıklayan ve sonuçları başarılı bir şekilde tahmin eden en az bağımsız değişken kümesidir.

Her zaman mutlaka özellik mühendisliği veya özellik seçimi gerçekleştirmek değildir. Gerekli olup olmadığı el veya toplanan verilere, seçilen algoritmaya ve denemenin amacına bağlıdır.

