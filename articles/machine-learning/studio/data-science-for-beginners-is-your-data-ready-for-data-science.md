---
title: Veri değerlendirmesi
titleSuffix: ML Studio (classic) - Azure
description: Veri bilimine hazır olmak için verilerinizin karşılaması gereken dört kriter. Bu videoda temel veri değerlendirmesine yardımcı olacak somut örnekler vardır.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: sdgilley
ms.author: sgilley
ms.custom: seodec18
ms.date: 03/22/2019
ms.openlocfilehash: ccc422dfb3105fd1e12569a84a4ebfd22182b225
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73837810"
---
# <a name="is-your-data-ready-for-data-science"></a>Verileriniz veri bilimi için hazır mı?
## <a name="video-2-data-science-for-beginners-series"></a>Video 2: Yeni Başlayanlar serisi için Veri Bilimi
Veri bilimine hazır olmak için temel ölçütleri karşıladığından emin olmak için verilerinizi nasıl değerlendireceğinizi öğrenin.

Serinin en iyi şekilde elde etmek için, hepsini izleyin. [Videolar listesine gidin](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Shows/SupervisionNotRequired/9/player]
>
>

## <a name="other-videos-in-this-series"></a>Bu serinin diğer videoları
*Yeni Başlayanlar için Veri Bilimi* beş kısa video veri bilimine hızlı bir giriştir.

* Video 1: [Veri bilimi cevapları 5 soru](data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 dk 14 sn)*
* Video 2: Verileriniz veri bilimine hazır mı?
* Video 3: [Verilerle cevaplanabileceğiniz bir soru sorun](data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 dk 17 sn)*
* Video 4: [Basit bir modelle bir yanıtı tahmin edin](data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 dk 42 sn)*
* Video 5: [Veri bilimi yapmak için diğer kişilerin çalışmalarını kopyalayın](data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 dk 18 sn)*

## <a name="transcript-is-your-data-ready-for-data-science"></a>Transkript: Verileriniz veri bilimine hazır mı?
"Verileriniz veri bilimine hazır mı?" *Yeni başlayanlar için*serisi Veri Bilimi ikinci video .  

Veri bilimi size istediğiniz cevapları vermeden önce, çalışmak için bazı yüksek kaliteli hammadde vermek zorunda. Sadece bir pizza yapmak gibi, daha iyi malzemelerle başlamak, daha iyi nihai ürün. 

## <a name="criteria-for-data"></a>Veri ölçütleri
Veri biliminde, şunlardır:

* Ilgili
* Bağlı
* Doğru
* Çalışmak için yeterli

## <a name="is-your-data-relevant"></a>Verileriniz alakalı mı?
Yani ilk madde - ilgili verilere ihtiyacınız var.

![İlgili veriler vs alakasız veriler - verileri değerlendirmek](./media/data-science-for-beginners-is-your-data-ready-for-data-science/relevant-and-irrelevant-data.png)

Solda, tablo boston bar dışında test yedi kişinin kan alkol seviyesi sunar, Red Sox son oyunda ortalama vuruş, ve en yakın bakkal süt fiyatı.

Bunların hepsi tamamen yasal veriler. Tek hata, konuyla ilgisi olmaması. Bu sayılar arasında bariz bir ilişki yok. Eğer birisi sana şu anki süt fiyatını ve Red Sox vuruş ortalamasını verdiyse kandaki alkol içeriğini tahmin etmenin imkanı yok.

Şimdi sağdaki masaya bak. Bu sefer her insanın vücut kütlesi ve içtiği içki sayısı ölçüldü.  Her satırdaki sayılar artık birbiriyle alakalıdır. Eğer sana vücut kütlemi ve sahip olduğum margarita sayısını verirsem kanımdaki alkol oranımı tahmin edebilirdin.

## <a name="do-you-have-connected-data"></a>Bağlı verileriniz var mı?
Bir sonraki bileşen bağlı verilerdir.

![Bağlı veri vs bağlantısız veri - veri kriterleri, veri hazır](./media/data-science-for-beginners-is-your-data-ready-for-data-science/connected-vs-disconnected-data.png)

Burada hamburger kalitesi bazı ilgili veriler: ızgara sıcaklığı, börek ağırlığı, ve yerel gıda dergisinde derecelendirme. Ama soldaki tablodaki boşluklara dikkat edin.

Çoğu veri kümesinde bazı değerler eksik. Bu gibi delikler olması yaygındır ve bunları etrafında çalışmak için yollar vardır. Ama çok fazla eksik varsa, verileriniz İsviçre peyniri gibi görünmeye başlar.

Soldaki masaya bakarsanız, o kadar çok eksik veri var ki, ızgara sıcaklığı ile köfte ağırlığı arasında herhangi bir ilişki bulmak zor. Bu örnek, bağlantısı kesilen verileri gösterir.

Sağdaki tablo, ancak, tam ve tam - bağlı veri bir örnektir.

## <a name="is-your-data-accurate"></a>Verileriniz doğru mu?
Bir sonraki bileşen doğruluk. Burada vurulması gereken dört hedef var.

![Doğru veriler ve yanlış veriler - veri ölçütleri](./media/data-science-for-beginners-is-your-data-ready-for-data-science/inaccurate-vs-accurate-data.png)

Sağ üstteki hedefe bak. Boğaların gözünün etrafında sıkı bir gruplaşma var. Bu, tabii ki, doğrudur. Gariptir ki, veri bilimi dilinde, hedef sağ altında performans da doğru olarak kabul edilir.

Eğer bu okların merkezini haritalabilseydin, boğaların gözüne çok yakın olduğunu görürsün. Oklar hedefin her tarafına yayılmış.

Şimdi sol üst hedefe bak. Burada oklar birbirine çok yakın, sıkı bir gruplaşma vurdu. Kesindirler, ama yanlıştırlar çünkü merkez boğaların gözünden çok uzaktır. Sol alt taki oklar hem yanlış hem de kesin değildir. Bu okçunun daha fazla pratiğe ihtiyacı var.

## <a name="do-you-have-enough-data-to-work-with"></a>Çalışmak için yeterli veriniz var mı?
Son olarak, madde #4 yeterli veridir.

![Analiz için yeterli veriniz var mı? Veri değerlendirmesi](./media/data-science-for-beginners-is-your-data-ready-for-data-science/barely-enough-data.png)

Tablonuzdaki her veri noktasını bir resimde fırça darbesi olarak düşünün. Eğer sadece birkaç tane varsa, resim bulanık olabilir - ne olduğunu söylemek zor.

Biraz daha fırça darbesi eklerseniz, resminiz biraz daha keskinolmaya başlar.

Ancak yeterli vuruş varsa, sadece bazı geniş kararlar vermek için yeterli bakın. Ziyaret etmek isteyebileceğim bir yer mi? Bu temiz su gibi görünüyor, parlak görünüyor - evet, ben tatile gidiyorum.

Daha fazla veri ekledikçe, resim daha net hale gelir ve daha ayrıntılı kararlar alabilirsiniz. Şimdi sol daki üç otele bakabilirsiniz. Ön planda olanın mimari özelliklerini fark edebilirsiniz. Hatta görünümü nedeniyle üçüncü katta kalmayı seçebilirsiniz.

İlgili, bağlı, doğru ve yeterli verilerle, yüksek kaliteli veri bilimi yapmak için gereken tüm bileşenlere sahip siniz.

Microsoft Azure Machine Learning Studio'dan (klasik) *Yeni Başlayanlar İçin Veri Bilimi'ndeki* diğer dört videoya göz atmayı unutmayın.

## <a name="next-steps"></a>Sonraki adımlar
* [Machine Learning Studio (klasik) ile ilk veri bilimi deneyin](create-experiment.md)
* [Microsoft Azure'da Machine Learning'e giriş alın](/azure/machine-learning/preview/overview-what-is-azure-ml)
