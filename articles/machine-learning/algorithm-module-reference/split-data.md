---
title: 'Verileri Böl: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Bir veri kümesini iki ayrı küme içine bölmek için Azure Machine Learning hizmetinde bölünmüş veri modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: 31612e10e7978e94f1ed467b5ffbecde40910ef9
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70128458"
---
# <a name="split-data-module"></a>Veri modülünü Böl

Bu makalede Azure Machine Learning hizmeti için görsel arabirim (Önizleme) modülü açıklanır.

Bir veri kümesini iki ayrı küme içine bölmek için bu modülü kullanın.

Bu modül özellikle, verileri eğitim ve test kümelerine ayırmanız gerektiğinde faydalıdır. Verilerin ne şekilde bölüneceğini de özelleştirebilirsiniz. Bazı seçenekler verilerin rastgele olarak kullanımını destekler; bazıları belirli bir veri türü veya model türü için uyarlanmıştır.

## <a name="how-to-configure"></a>Yapılandırma

> [!TIP]
> Bölme modunu seçmeden önce, ihtiyacınız olan bölme türünü belirlemek için tüm seçenekleri okuyun.
> Bölme modunu değiştirirseniz diğer tüm seçenekler sıfırlanabilir.

1. Arabirim içindeki denemenize **veri bölme** modülünü ekleyin. Bu modülü, **veri dönüştürme**altında, **örnek ve bölünmüş** kategoride bulabilirsiniz.

2. **Bölme modu**: Sahip olduğunuz veri türüne ve onu bölmek istediğiniz bir değere bağlı olarak aşağıdaki modlardan birini seçin. Her bölme modunun farklı seçenekleri vardır. Ayrıntılı yönergeler ve örnekler için aşağıdaki konulara tıklayın. 

    - **Satırları Böl**: Yalnızca verileri iki parçaya bölmek istiyorsanız bu seçeneği kullanın. Her bir bölünmeye yerleştirilecek verilerin yüzdesini belirtebilirsiniz, ancak varsayılan olarak, veriler bölünmüş 50-50 ' dir.

        Ayrıca, her grupta satır seçimini rastgele rasgele hale getirebilirsiniz ve bunları Strata örnekleme kullanabilirsiniz. Bağlı örnekleme sırasında, değerlerinin iki sonuç veri kümesi arasında eşit olarak olmasını istediğiniz tek bir veri sütunu seçmelisiniz.  

    - **Normal Ifade bölme**  Bir değer için tek bir sütunu test ederek veri kümenizi bölmek istediğinizde bu seçeneği belirleyin.

        Örneğin, yaklaşımı analiz ediyorsanız, bir metin alanında belirli bir ürün adının varlığını denetleyebilir ve sonra veri kümesini hedef ürün adı ve olmadan satırlara bölebilirsiniz.

    - **Göreli Ifade bölme**:  Bir sayı sütununa koşul uygulamak istediğinizde bu seçeneği kullanın. Sayı bir tarih/saat alanı, Yaş veya dolar miktarları içeren bir sütun veya hatta bir yüzde olabilir. Örneğin, öğelerin maliyetine göre veri kümesini bölmek, kişileri yaş aralıklarına göre gruplamak veya bir Takvim tarihine göre verileri ayırmak isteyebilirsiniz.

### <a name="split-rows"></a>Satırları Böl
1.  Arabirim içindeki denemenize [veri ayırma](./split-data.md) modülünü ekleyin ve ayırmak istediğiniz veri kümesini bağlayın.
  
2.  **Ayırma modu**Için **satırları Böl**' ü seçin. 

3.  **İlk çıkış veri kümesindeki satırların kesri**. İlk (sol taraftaki) çıktıya kaç satır gideceğini öğrenmek için bu seçeneği kullanın. Diğer tüm satırlar ikinci (sağ) çıktıya gidecektir.

    Oran, ilk çıkış veri kümesine gönderilen satırların yüzdesini temsil eder, bu nedenle 0 ile 1 arasında bir ondalık sayı yazmanız gerekir.
     
     Örneğin, değer olarak 0,75 yazarsanız, veri kümesi, ilk çıkış veri kümesine gönderilen satırların% 75 ' i ve ikinci çıkış veri kümesine gönderilen% 25 ' lik bir 75:25 oranı kullanılarak bölünür.
  
4. Veri seçimini iki gruba rastgele atamak istiyorsanız **rastgele bölme** seçeneğini belirleyin. Bu, eğitim ve test veri kümeleri oluştururken tercih edilen seçenektir.

5.  **Rastgele çekirdek**: Kullanılacak örnek dizisini başlatmak için negatif olmayan bir tamsayı değeri yazın. Bu varsayılan çekirdek, rastgele sayılar üreten tüm modüllerde kullanılır. 

     Bir çekirdek belirtmek sonuçları genellikle tekrarlanabilir hale getirir. Bir bölme işleminin sonuçlarını tekrarlamanız gerekiyorsa, rastgele numara Oluşturucu için bir çekirdek belirtmeniz gerekir. Aksi takdirde, rastgele çekirdek varsayılan olarak 0 olarak ayarlanır; Bu, ilk çekirdek değer sistem saatinden elde edilen anlamına gelir. Sonuç olarak, her bölme yaptığınızda verilerin dağıtılması biraz farklı olabilir. 

6. **Bağlı bölme**: İki çıkış veri kümesinin *konuşmasıyla Strata sütunu* veya *Stratification Key sütunundaki*değerlerin temsili bir örneğini içermesini sağlamak için bu seçeneği **true** olarak ayarlayın. 

    Birbirine bağlı örnekleme ile, veriler her bir çıktı veri kümesinin her bir hedef değerin kabaca aynı yüzdesini almasını sağlayan şekilde bölünür. Örneğin, eğitim ve test kümelerinizin, sonuca göre veya cinsiyet gibi başka bir sütuna göre kabaca dengeleneceği şekilde emin olmak isteyebilirsiniz.

7. Denemeyi çalıştırın.


## <a name="regular-expression-split"></a>Normal ifade bölme

1.  [Veri ayırma](./split-data.md) modülünü denemenize ekleyin ve ayırmak istediğiniz veri kümesine giriş olarak bağlayın.  
  
2.  **Ayırma modu**için **normal ifade Böl**' ü seçin.

3. **Normal ifade** kutusuna geçerli bir normal ifade yazın. 
  
   Normal ifade, Python normal ifade söz dizimini izlemelidir.


4. Denemeyi çalıştırın.

    Sağladığınız normal ifadeye bağlı olarak, veri kümesi iki satır kümesine ayrılır: ifadeyle eşleşen değerleri olan satırlar ve kalan tüm satırlar. 

## <a name="relative-expression-split"></a>Göreli ifade bölme.

1. [Veri ayırma](./split-data.md) modülünü denemenize ekleyin ve ayırmak istediğiniz veri kümesine giriş olarak bağlayın.
  
2. **Ayırma modu**için **göreli ifade Böl**' ü seçin.
  
3. **İlişkisel ifade** metin kutusuna, tek bir sütunda karşılaştırma işlemi gerçekleştiren bir ifade yazın:


 - Sayısal sütun:
    - Sütun, tarih/saat veri türleri dahil olmak üzere herhangi bir sayısal veri türü sayı içerir.

    - İfade, en fazla bir sütun adına başvurabilir.

    - VE işlemi için ve karakteri (&) kullanın ve ya da işlem için kanal karakterini (|) kullanın.

    - Aşağıdaki işleçler desteklenir: `<`, `>`, `<=` `>=`, ,`==`,`!=`

    - `(` Ve`)`kullanarak işlemleri gruplayabilirsiniz.

 - Dize sütunu: 
    - Aşağıdaki işleçler desteklenir: `==`,`!=`



4. Denemeyi çalıştırın.

    İfade, veri kümesini iki satır kümesine böler: koşula uyan değerler içeren satırlar ve kalan tüm satırlar.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti için [kullanılabilen modül kümesine](module-reference.md) bakın. 