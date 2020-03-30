---
title: Regresyon modelleri ile yanıtları tahmin etme
titleSuffix: ML Studio (classic) - Azure
description: Nasıl Yeni Başlayanlar video 4 için Veri Bilimi bir fiyat tahmin etmek için basit bir regresyon modeli oluşturmak için. Hedef verilerle doğrusal bir regresyon içerir.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: sdgilley
ms.author: sgilley
ms.custom: seodec18
ms.date: 03/22/2019
ms.openlocfilehash: 6ad9c7912eee6c3f5ec55b9cd7ab340bc79c9db7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73837767"
---
# <a name="predict-an-answer-with-a-simple-model"></a>Basit model ile yanıtı tahmin etme
## <a name="video-4-data-science-for-beginners-series"></a>Video 4: Yeni Başlayanlar serisi için Veri Bilimi
Yeni Başlayanlar için Veri Bilimi video 4 bir elmas fiyatını tahmin etmek için basit bir regresyon modeli oluşturmak için nasıl öğrenin. Hedef verilerle bir regresyon modeli çizeceğiz.

Serinin en iyi şekilde elde etmek için, hepsini izleyin. [Videolar listesine gidin](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/data-science-for-beginners-series-predict-an-answer-with-a-simple-model/player]
>
>

## <a name="other-videos-in-this-series"></a>Bu serinin diğer videoları
*Yeni Başlayanlar için Veri Bilimi* beş kısa video veri bilimine hızlı bir giriştir.

* Video 1: [Veri bilimi cevapları 5 soru](data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 dk 14 sn)*
* Video 2: [Verileriniz veri bilimine hazır mı?](data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 dk 56 sn)*
* Video 3: [Verilerle cevaplanabileceğiniz bir soru sorun](data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 dk 17 sn)*
* Video 4: Basit bir modelle bir yanıtı tahmin etme
* Video 5: [Veri bilimi yapmak için diğer kişilerin çalışmalarını kopyalayın](data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 dk 18 sn)*

## <a name="transcript-predict-an-answer-with-a-simple-model"></a>Transkript: Basit bir modelle bir yanıtı tahmin etme
"Yeni Başlayanlar İçin Veri Bilimi" serisinin dördüncü klibine hoş geldiniz. Bu nda, basit bir model oluşturup bir tahminde bulunacağız.

*Bir model,* verilerimiz hakkında basitleştirilmiş bir hikayedir. Sana ne demek istediğimi göstereceğim.

## <a name="collect-relevant-accurate-connected-enough-data"></a>İlgili, doğru, bağlı, yeterli veri toplama
Bir elmas almak istediğimi söyle. Ben 1.35 karat elmas için bir ayar ile büyükannema ait bir yüzük var, ve ben ne kadara mal olacak bir fikir almak istiyorum. Kuyumcuya bir not defteri ve kalem alıyorum ve çantadaki tüm elmasların fiyatını ve karatağırlığındane ne kadar ağırlık verdiklerini yazıyorum. İlk elmas ile başlayan - 1.01 karat ve 7.366 $ 's.

Şimdi bunu dükkandaki diğer elmaslar için yapıyorum.

![Elmas veri sütunları](./media/data-science-for-beginners-predict-an-answer-with-a-simple-model/diamond-data.png)

Listemizde iki sütun olduğuna dikkat edin. Her sütun farklı bir özniteliğe sahiptir - karat ve fiyat ağırlığı - ve her satır tek bir elmas temsil eden tek bir veri noktasıdır.

Aslında burada küçük bir veri seti oluşturduk- bir tablo. Kalite kriterlerimizi karşıladığına dikkat edin:

* Veriler **ilgili** - ağırlık kesinlikle fiyat ile ilgilidir
* Doğru **-** yazdığımız fiyatları iki kez kontrol ettik.
* **Bağlı** - bu sütunların hiçbirinde boş boşluk yok
* Ve göreceğimiz gibi, sorumuza cevap vermek için **yeterli** veri var.

## <a name="ask-a-sharp-question"></a>Keskin bir soru sorun
Şimdi sorumuzu keskin bir şekilde salacağız: "1.35 karatlık bir elmas satın almak ne kadara mal olacak?"

Listemizde 1.35 karat elmas yok, bu yüzden soruya cevap bulmak için diğer verilerimizi kullanmamız gerekecek.

## <a name="plot-the-existing-data"></a>Varolan verileri çizim
Yapacağımız ilk şey, ağırlıkları grafiklemek için eksen adı verilen yatay bir sayı çizgisi çizmek olacak. Ağırlıkaralığı 0'dan 2'ye kadar, bu yüzden bu aralığı kapsayan bir çizgi çizeceğiz ve her yarım karat için keneler koyacağız.

Daha sonra fiyatı kaydetmek ve yatay ağırlık eksenine bağlamak için dikey bir eksen çizeceğiz. Bu dolar birimleri olacak. Şimdi bir dizi koordinat eksenimiz var.

![Ağırlık ve fiyat eksenleri](./media/data-science-for-beginners-predict-an-answer-with-a-simple-model/weight-and-price-axes.png)

Şimdi bu verileri alıp *bir dağılım konusuna*dönüştüreceğiz. Bu, sayısal veri kümelerini görselleştirmek için harika bir yoldur.

İlk veri noktası için, 1.01 karat dikey bir çizgi göz. Sonra, 7.366 $ yatay bir çizgi göz. Buluştukları yerde bir nokta çizeriz. Bu ilk elmasımızı temsil eder.

Şimdi bu listedeki her elması gözden geçiriyoruz ve aynı şeyi yapacağız. İşimiz bittiğinde, elde ettiğimiz şey şu: bir demet nokta, her elmas için bir tane.

![Dağılım çizimi](./media/data-science-for-beginners-predict-an-answer-with-a-simple-model/scatter-plot.png)

## <a name="draw-the-model-through-the-data-points"></a>Modeli veri noktaları üzerinden çizin
Şimdi eğer nokta ve şaşı bakarsanız, koleksiyon şişman, bulanık bir çizgi gibi görünüyor. İşaretimizi alıp içinden düz bir çizgi çekebiliriz.

Bir çizgi çizerek, bir *model*oluşturduk. Bunu gerçek dünyayı ele almak ve basit bir çizgi film versiyonu yapmak olarak düşünün. Şimdi karikatür yanlış - çizgi tüm veri noktaları üzerinden gitmez. Ama, bu yararlı bir basitleştirme.

![Lineer regresyon hattı](./media/data-science-for-beginners-predict-an-answer-with-a-simple-model/linear-regression-line.png)

Aslında tüm nokta çizgiden tam olarak geçmiyor. Veri bilimciler bunu modelin olduğunu söyleyerek açıklıyorlar - bu çizgi - ve sonra her noktanın onunla ilişkili *bir* gürültü ya da *varyans* ları var. Altta yatan mükemmel bir ilişki var, ve bir de gürültü ve belirsizlik katan cesur, gerçek dünya var.

Çünkü ne kadar olduğu sorusuna cevap vermeye *çalışıyoruz.* *regression* Ve düz bir çizgi kullandığımız için, bu doğrusal bir *gerileme.*

## <a name="use-the-model-to-find-the-answer"></a>Yanıtı bulmak için modeli kullanın
Şimdi bir model var ve biz bizim soru sormak: Ne kadar bir 1.35 karat elmas mal olacak?

Sorumuza cevap vermek için, biz eyeball 1.35 karat ve dikey bir çizgi çizin. Model çizgisini geçtiği yerde dolar eksenine yatay bir çizgi çizeriz. 10,000'e uy. Boom! Bu cevap: 1.35 karat elmas maliyeti yaklaşık 10.000 $ .

![Modelde yanıtı bulun](./media/data-science-for-beginners-predict-an-answer-with-a-simple-model/find-the-answer.png)

## <a name="create-a-confidence-interval"></a>Güven aralığı oluşturma
Bu öngörüyü ne kadar kesin merak etmek doğal. Bu 1.35 karat elmas çok yakın 10.000 $ ya da çok daha yüksek veya daha düşük olacağını bilmek yararlıdır. Bunu anlamak için, noktaların çoğunu içeren regresyon çizgisinin etrafına bir zarf çizelim. Bu zarfa *güven aralığımız*denir: Fiyatların bu zarfın içine düştüğünden oldukça eminiz, çünkü geçmişte çoğu. 1.35 karat lık çizginin zarfın üst ve alt kısmını geçtiği yerden iki yatay çizgi daha çizebiliriz.

![Olasılık aralığı](./media/data-science-for-beginners-predict-an-answer-with-a-simple-model/confidence-interval.png)

Şimdi bizim güven aralığı hakkında bir şeyler söyleyebiliriz: Biz güvenle bir 1,35 karat elmas fiyatı yaklaşık 10.000 $ olduğunu söyleyebiliriz - ama 8.000 $ gibi düşük olabilir ve 12.000 $ gibi yüksek olabilir.

## <a name="were-done-with-no-math-or-computers"></a>Matematik ve bilgisayar olmadan işimiz bitti.
Bilim adamlarına para ödenen verileri yaptık ve bunu sadece çizerek yaptık:

* Verilerle cevaplayabileceğimiz bir soru sorduk.
* *Doğrusal regresyon* kullanarak bir *model* yaptık.
* Biz bir *tahmin*yaptı, bir *güven aralığı* ile tamamlandı

Ve bunu yapmak için matematik ya da bilgisayar kullanmadık.

Şimdi daha fazla bilgi olsaydı, gibi ...

* elmas kesim
* renk varyasyonları (elmas beyaz olmaya ne kadar yakın)
* elmas eklemelerin sayısı

... o zaman daha fazla sütun olurdu. Bu durumda, matematik yararlı olur. İkiden fazla sütununuzun varsa, kağıt üzerinde nokta çizmek zordur. Matematik, o satırı veya düzlemi verilerinize çok güzel bir şekilde sığdırmamanızı sağlar.

Ayrıca, eğer bir avuç elmas yerine, iki bin ya da iki milyon elmasımız olsaydı, o zaman bunu bir bilgisayarla çok daha hızlı bir şekilde yapabilirsiniz.

Bugün, doğrusal regresyon nasıl yapılır hakkında konuştuk, ve verileri kullanarak bir tahminde bulunduk.

Microsoft Azure Machine Learning Studio'nun (klasik) "Yeni Başlayanlar Için Veri Bilimi"ndeki diğer videolara göz atmayı unutmayın.

## <a name="next-steps"></a>Sonraki adımlar
* [Machine Learning Studio (klasik) ile ilk veri bilimi deneyin](create-experiment.md)
* [Microsoft Azure'da Machine Learning'e giriş alın](/azure/machine-learning/preview/overview-what-is-azure-ml)
