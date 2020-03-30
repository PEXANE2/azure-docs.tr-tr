---
title: Değerleri Kırpma
titleSuffix: Azure Machine Learning
description: Azure Machine Learning'de, aykırı ları tespit etmek ve değerlerini kesmek veya değiştirmek için Küçük Değerler modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 09/09/2019
ms.openlocfilehash: 6466cea9fe04bb308a670cb03fd3de5314758142
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79456616"
---
# <a name="clip-values"></a>Değerleri Kırpma

Bu makalede, Azure Machine Learning tasarımcısının (önizleme) bir modülü açıklanmaktadır.

Belirli bir eşiğin üstünde veya altında olan veri değerlerini ortalama, sabit veya başka bir yedek değerle tanımlamak ve isteğe bağlı olarak değiştirmek için Küçük Değerler modülünü kullanın.  

Modülü, klibin üzerinde olmak istediğiniz sayılara sahip bir veri kümesine bağlar, çalışacak sütunları seçer ve ardından bir eşik veya değer aralığı ve değiştirme yöntemi belirlersiniz. Modül, yalnızca sonuçları veya orijinal veri kümesine eklenen değiştirilen değerleri çıktı alabilir.

## <a name="how-to-configure-clip-values"></a>Klip değerleri nasıl yapılandırılır?

Başlamadan önce, klibin kırpısıyabilmesini istediğiniz sütunları ve kullanılacak yöntemi tanımlayın. Önce küçük bir veri alt kümesi üzerinde herhangi bir kırpma yöntemini test etmenizi öneririz.

Modül, seçime eklediğiniz **tüm** sütunlara aynı ölçütleri ve değiştirme yöntemini uygular. Bu nedenle, değiştirmek istemediğiniz sütunları hariç tutabildiğinizden emin olun.

Bazı sütunlara kırpma yöntemleri veya farklı ölçütler uygulamanız gerekiyorsa, benzer sütunkümeleri için yeni bir **Küçük Değer** örneği kullanmanız gerekir.

1.  Kesme **Değerleri** modülünü ardışık noktanıza ekleyin ve değiştirmek istediğiniz veri kümesine bağlayın. Bu modülü Veri **Dönüşümü**altında Ölçek **ve Azalt** kategorisinde bulabilirsiniz. 
  
1.  **Sütun lar listesinde,** **Küçük Değerlerin** uygulanacağı sütunları seçmek için Sütun Seçici'yi kullanın.  
  
1.  **Eşikler Kümesi**için açılır listeden aşağıdaki seçeneklerden birini seçin. Bu seçenekler, kırpılması gereken değerler ve kabul edilebilir değerler için üst ve alt sınırları nasıl ayarladığınızı belirler.  
  
    - **ClipPeaks**: Değerleri tepenoktalarına göre kırptığınızda yalnızca bir üst sınır belirtirsiniz. Bu sınır değerinden büyük değerler değiştirilir.
  
    -  **ClipSubpeaks**: Alt tepenoktalarına göre değerleri klibinde yalnızca daha düşük bir sınır belirtirsiniz. Bu sınır değerinden daha az olan değerler değiştirilir.  
  
    - **ClipPeaksAndSubpeaks**: Değerleri tepe noktalarına ve alt tepe noktalarına göre kırptığınızda, hem üst hem de alt sınırları belirtebilirsiniz. Bu aralığın dışındaki değerler değiştirilir. Sınır değerleriyle eşleşen değerler değiştirilmez.
  
1.  Önceki adımdaki seçiminize bağlı olarak, aşağıdaki eşik değerlerini ayarlayabilirsiniz: 

    + **Alt eşik**: Yalnızca **ClipSubPeaks'i** seçerseniz görüntülenir
    + **Üst eşik**: Yalnızca **ClipPeaks'i** seçerseniz görüntülenir
    + **Eşik**: Yalnızca **ClipPeaksAndSubPeaks'i** seçerseniz görüntülenir

    Her eşik türü için **Sabit** veya **Yüzdek'yi**seçin.

1. **Sabit'i**seçerseniz, metin kutusuna en büyük veya minimum değeri yazın. Örneğin, 999 değerinin yer tutucu değeri olarak kullanıldığını bildiğinizi varsayalım. Üst eşik için **Sabit'i** ve **üst eşik için Sabit değeri**olarak 999 yazın'ı seçebilirsiniz.
  
1. **Yüzdele'yi**seçerseniz, sütun değerlerini yüzdelik aralıkla sınırlandırMış olursunuz. 

    Örneğin, yalnızca yüzdelik aralığındaki değerleri tutmak istediğinizi varsayalım ve diğer tüm değerleri değiştirin. **Yüzdele'yi**seçersiniz ve daha sonra **daha düşük eşik için Yeşİl değeri için**10 yazar sınız ve üst eşik için **Yüzdelik değeri**için 80 yazın. 

    Yüzdelik aralıklarının nasıl kullanılacağına ilişkin bazı örnekler için [yüzdelik](#examples-for-clipping-using-percentiles) dilimleri bölümüne bakın.  
  
1.  Yedek bir değer tanımlayın.

    Belirttiğiniz sınırlara tam olarak uyan sayılar izin verilen değerler aralığında kabul edilir ve bu nedenle değiştirilmez. Belirtilen aralığın dışına düşen tüm sayılar yedek değerle değiştirilir. 
  
    + **Zirvelerin yerine geçen değer**: Belirtilen eşikten büyük tüm sütun değerlerinin yerine geçecek değeri tanımlar.  
    + **Alt zirvelerin yerine değer**: Belirtilen eşikten daha az olan tüm sütun değerlerinin yerine kullanılacak değeri tanımlar.  
    + **ClipPeaksAndSubpeaks** seçeneğini kullanıyorsanız, kırpılabilen üst ve alt değerler için ayrı değiştirme değerleri belirtebilirsiniz.  

    Aşağıdaki değiştirme değerleri desteklenir:  
  
    -   **Eşik**: Kırpılmış değerleri belirtilen eşik değeriyle değiştirir.  
  
    -   **Ortalama**: Kırpılmış değerleri sütun değerlerinin ortalamasıyla değiştirir. Değerler kırpılmadan önce ortalama hesaplanır.  
  
    -   **Ortanca**: Kırpık değerleri sütun değerlerinin ortancasıyla değiştirir. Değerler kırpılmadan önce ortanca hesaplanır.   
  
    -   **Kayıp**. Kırpılak değerleri eksik (boş) değerle değiştirir.  
  
1.  **Gösterge sütunları ekleme**: Belirtilen kırpma işleminin o satırdaki verilere uygulanıp uygulanmadığını belirten yeni bir sütun oluşturmak istiyorsanız bu seçeneği seçin. Bu seçenek, yeni bir kırpma ve değiştirme değerleri kümesini sınarken yararlıdır.  
  
1. **Üstyazı :** Yeni değerlerin nasıl oluşturulmasını istediğinizi belirtin. Varsayılan olarak, **Küçük Değerler** istenen eşiğe kırpılmış tepe değerleri ile yeni bir sütun inşa eder. Yeni değerler özgün sütunun üzerine yazılır.  
  
    Özgün sütunu tutmak ve kırpılen değerlere sahip yeni bir sütun eklemek için bu seçeneği seçin.  
  
1.  Boru hattını gönderin.  
  
    **Clip Values** modülüne sağ tıklayın ve **Visualize'ı** seçin veya modülü seçin ve sağ paneldeki **Çıktılar** sekmesine geçin, Bağlantı **Noktası çıkışlarında**histogram simgesine tıklayın, değerleri gözden geçirin ve kırpma işleminin beklentilerinizi karşıladığınızdan emin olun.  
 
### <a name="examples-for-clipping-using-percentiles"></a>Yüzdelik dilimleri kullanarak kırpma örnekleri

Yüzdelik kırpma nın nasıl çalıştığını anlamak için, 1-10 değerlerinin her biri bir örneği olan 10 satırlı bir veri kümesi düşünün.  
  
- Yüzdelik olarak yüzdeyi kullanıyorsanız, 90 yüzdelik değerin değerinde, veri kümesindeki tüm değerlerin yüzde 90'ı bu değerden daha az olmalıdır.  
  
- Yüzdelik ile'yi alt eşik olarak kullanıyorsanız, yüzde 10'luk değerin değerinde, veri kümesindeki tüm değerlerin yüzde 10'u bu değerden daha az olmalıdır.  
  
1.  **Eşikler kümesi için** **ClipPeaksAndSubPeaks'i**seçin.  
  
1.  **Üst eşik için,** **Yüzdele'yi**seçin ve **Yüzdele sayısı**için 90 yazın.  
  
1.  **Üst yedek değer**için Eksik **Değer'i**seçin.  
  
1.  **Alt eşik için,** **Yüzdeli'yi**seçin ve **Yüzdele sayısı**için , 10 yazın.  
  
1.  **Daha düşük yedek değer**için Eksik **Değer'i**seçin.  
  
1.  **Overwrite bayrağı**seçeneğini seçin ve gösterge **sütunu ekle**seçeneğini seçin.  
  
Şimdi üst yüzdelik eşik olarak 60 ve alt yüzdelik eşik olarak 30 kullanarak aynı ardışık deneyin ve değiştirme değeri olarak eşik değerini kullanın. Aşağıdaki tablo da bu iki sonucu karşılaştırır:  
  
1.  Eksik ile değiştirin; Üst eşik = 90; Alt eşik = 20  
  
1.  Eşik ile değiştirin; Üst yüzdelik = 60; Daha düşük yüzdelik = 40  
  
|Orijinal veriler|Eksikle değiştir|Eşikle değiştir|  
|-------------------|--------------------------|----------------------------|  
|1<br /><br /> 2<br /><br /> 3<br /><br /> 4<br /><br /> 5<br /><br /> 6<br /><br /> 7<br /><br /> 8<br /><br /> 9<br /><br /> 10|TRUE<br /><br /> TRUE<br /><br /> 3, YANLIŞ<br /><br /> 4, YANLIŞ<br /><br /> 5, YANLIŞ<br /><br /> 6, YANLIŞ<br /><br /> 7, YANLIŞ<br /><br /> 8, YANLIŞ<br /><br /> 9, YANLIŞ<br /><br /> TRUE|4, DOĞRU<br /><br /> 4, DOĞRU<br /><br /> 4, DOĞRU<br /><br /> 4, DOĞRU<br /><br /> 5, YANLIŞ<br /><br /> 6, YANLIŞ<br /><br /> 7, DOĞRU<br /><br /> 7, DOĞRU<br /><br /> 7, DOĞRU<br /><br /> 7, DOĞRU| 
 
## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 
