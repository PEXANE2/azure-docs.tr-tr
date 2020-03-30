---
title: Gösterge Değerlerine dönüştürme
titleSuffix: Azure Machine Learning
description: Kategorik değerler içeren sütunları bir dizi ikili gösterge sütununa dönüştürmek için Azure Machine Learning'de Gösterge Değerlerine Dönüştür modülünün nasıl kullanılacağını öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/11/2020
ms.openlocfilehash: f1b194f2c65f95ad4daff0353d05ca589db9ce51
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477672"
---
# <a name="convert-to-indicator-values"></a>Gösterge Değerlerine dönüştürme
Bu makalede, Azure Machine Learning tasarımcısının bir modülü açıklanmaktadır.

Kategorik değerler içeren sütunları bir dizi ikili gösterge sütununa dönüştürmek için Azure Machine Learning tasarımcısında **Gösterge Değerlerine Dönüştür** modüllerini kullanın.  

Bu modül aynı zamanda gösterge değerlerine dönüştürmek için kullanılan dönüşümün bir tanımını da ortaya çıkar. Bu dönüşümü, [Dönüşüm Uygula](apply-transformation.md) modüllerini kullanarak aynı şema olan diğer veri kümelerinde yeniden kullanabilirsiniz.

## <a name="how-to-configure-convert-to-indicator-values"></a>Convert to Indicator Değerlerine Yapılandırma nasıl yapılandırılır?

1.  Dönüşüm **Gösterge Değerlerini** bulun ve boru hattı taslağınıza sürükleyin. Bu modülü Veri **Dönüşümü** kategorisinde bulabilirsiniz.
    > [!NOTE]
    > Hedef sütun(lar)ı kategorik olarak işaretlemek için **İndiciator Değerlerine Dönüştür** modülünden önce [Meta Verileri Edit](edit-metadata.md) modüllerini kullanabilirsiniz.

1. Dönüştürme yi **Gösterge Değerlerine dönüştür** modülünü dönüştürmek istediğiniz sütunları içeren veri kümesine bağlayın. 

1. Bir veya daha fazla kategorik sütun seçmek için **Sütunu Edit'i** seçin.

1. **Yalnızca** yeni Boolean sütunlarını çıktırmak **istiyorsanız, Üzerine Yaz kategorik sütunlar** seçeneğini belirleyin. Varsayılan olarak, bu değer kapalıdır.
    

    > [!TIP]
    >  Üzerine yazma seçeneğini seçerseniz, kaynak sütun gerçekte silinmez veya değiştirilmez. Bunun yerine, yeni sütunlar oluşturulur ve çıktı veri kümesinde sunulur ve kaynak sütun çalışma alanında kullanılabilir kalır. Özgün verileri görmeniz gerekiyorsa, kaynak sütunu geri eklemek için istediğiniz zaman [Sütun Ekle](add-columns.md) modüllerini kullanabilirsiniz.

1. Boru hattını gönderin.

## <a name="results"></a>Sonuçlar

Bir sunucunun yüksek, orta veya düşük hata olasılığına sahip olup olmadığını gösteren puanları olan bir sütununuz olduğunu varsayalım.  

| Sunucu Kimliği | Hata puanı |
| --------- | ------------- |
| 10301     | Düşük           |
| 10302     | Orta        |
| 10303     | Yüksek          |

**Gösterge Değerlerine Dönüştür'ü**uyguladığınız zaman, tasarımcı tek bir etiket sütununu Boolean değerlerini içeren birden çok sütuna dönüştürür:  

| Sunucu Kimliği | Hata puanı - Düşük | Başarısızlık puanı - Orta | Başarısızlık puanı - Yüksek |
| --------- | ------------------- | ---------------------- | -------------------- |
| 10301     | 1                   | 0                      | 0                    |
| 10302     | 0                   | 1                      | 0                    |
| 10303     | 0                   | 0                      | 1                    |

Dönüşüm şu şekilde çalışır:  

-   Riski açıklayan **Hata puanı** sütununda yalnızca üç olası değer (Yüksek, Orta ve Düşük) vardır ve eksik değer yoktur. Yani, tam olarak üç yeni sütun oluşturulur.  

-   Yeni gösterge sütunları, kaynak sütunun sütun başlıkları ve değerleri temel alınarak bu deseni kullanır: * \<kaynak sütun>- \<veri değeri>. *  

-   Her sunucunun yalnızca bir risk derecelendirmesi olabileceğinden, tam olarak bir gösterge sütununda 1 ve diğer tüm gösterge sütunlarında 0 olmalıdır.  

Artık üç gösterge sütunlarını bir makine öğrenme modelinde özellik olarak kullanabilirsiniz.

Modül iki çıktı döndürür:

- **Sonuç veri kümesi**: Dönüştürülmüş gösterge değerleri sütunları olan bir veri kümesi. Temizleme için seçilmemiş sütunlar da "geçirilir".
- **Gösterge değerleri dönüştürme**: Çalışma alanınızda kaydedilebilen ve daha sonra yeni verilere uygulanabilen gösterge değerlerine dönüştürmek için kullanılan veri dönüşümü.

## <a name="apply-a-saved-indicator-values-operation-to-new-data"></a>Kaydedilen gösterge değerleri işlemini yeni verilere uygulama

Gösterge değerleri işlemlerini sık sık yinelemeniz gerekiyorsa, veri işleme adımlarınızı aynı veri kümesiyle yeniden kullanmak için *dönüştürme* olarak kaydedebilirsiniz. Bu, sık sık yeniden içe aktarmanız ve ardından aynı şema olan verileri temizlemeniz gerekiyorsa yararlıdır.

1. Dönüşüm [Uygula](apply-transformation.md) modüllerini ardınıza ekleyin.

1. Temizlemek istediğiniz veri kümesini ekleyin ve veri kümesini sağ giriş bağlantı noktasına bağlayın.

1. Tasarımcının sol bölmesinde **Veri Dönüşümü** grubunu genişletin. Kaydedilen dönüşümü bulun ve boru hattına sürükleyin.

1. Kaydedilen dönüşümü [Dönüşüm Uygula'nın](apply-transformation.md)sol giriş bağlantı noktasına bağlayın.

   Kaydedilmiş bir dönüşüm uyguladığınız zaman, hangi sütunların dönüştürüleceklerini seçemezsiniz. Bunun nedeni, dönüşümün tanımlanmış olması ve özgün işlemde belirtilen veri türlerine otomatik olarak uygulanmış olmasıdır.

1. Boru hattını gönderin.
 
## <a name="technical-notes"></a>Teknik notlar  

Bu bölümde uygulama ayrıntıları, ipuçları ve sık sorulan soruların yanıtları içerir.

### <a name="usage-tips"></a>Kullanım ipuçları

-   Yalnızca kategorik olarak işaretlenmiş sütunlar gösterge sütunlarına dönüştürülebilir. Aşağıdaki hatayı görürseniz, seçtiğiniz sütunlardan birinin kategorik olmaması olasıdır:  

     Hata 0056: Ad \<sütun adı> sütun una sahip sütun izin verilen bir kategoride değildir.  

     Varsayılan olarak, dize sütunlarının çoğu dize özellikleri olarak işlenir, bu nedenle Meta [Verileri Edit'i](edit-metadata.md)kullanarak bunları açıkça kategorik olarak işaretlemeniz gerekir.  

-   Gösterge sütunlarına dönüştürebileceğiniz sütun sayısında sınır yoktur. Ancak, her değer sütunu birden çok gösterge sütunu verim alabildiği için, aynı anda sadece birkaç sütunu dönüştürmek ve gözden geçirmek isteyebilirsiniz.  

-   Sütun eksik değerler içeriyorsa, eksik kategori için bu adla birlikte ayrı bir gösterge sütunu oluşturulur: * \<kaynak sütun>- Eksik*  

-   Gösterge değerlerine dönüştürdüğünüz sütun sayılar içeriyorsa, bunlar da diğer özellik sütunları gibi kategorik olarak işaretlenmelidir. Bunu yaptıktan sonra, sayılar ayrık değerler olarak kabul edilir. Örneğin, MPG değerlerine sahip sayısal bir sütununuz varsa, her ayrı kıvrık değer için yeni bir gösterge sütunu oluşturulur:  

    | Marka       | Karayolu mpg -25 | Karayolu mpg -26 | Karayolu mpg -27 | Karayolu mpg -28 | Karayolu mpg -29 | Otoyol mpg -30 |
    | ---------- | --------------- | --------------- | --------------- | --------------- | --------------- | --------------- |
    | Contoso Arabalar | 0               | 0               | 0               | 0               | 0               | 1               |

- Veri setinize çok fazla boyut eklememek için. Önce sütundaki değer sayısını kontrol etmenizi ve verileri uygun şekilde ölçmenizi veya ölçmenizi öneririz.  


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 
