---
title: Init Image dönüştürme Tionply Image dönüşümü
titleSuffix: Azure Machine Learning
description: Görüntü dönüştürmeyi başlatmak için Init Image dönüştürme modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 05/26/2020
ms.openlocfilehash: f54dd09ac82ddefeb2528462c7131aa921592d2a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84487959"
---
# <a name="init-image-transformation"></a>Init Görüntü Dönüşümü

Bu makalede, görüntünün nasıl dönüştürülmesini istediğinizi belirtmek üzere görüntü dönüşümünü başlatmak için Azure Machine Learning tasarımcısında (Önizleme) **Init görüntü dönüştürme** modülünün nasıl kullanılacağı açıklanır.

## <a name="how-to-configure-init-image-transformation"></a>Init Image dönüşümünü yapılandırma

1.  Tasarımcı 'daki işlem hattınızla **Init Image dönüştürme** modülünü ekleyin. 

2.  **Yeniden boyutlandırma**için, giriş PIL görüntüsünün verilen boyuta göre yeniden boyutlandırılıp boyutlandırılmayacağını belirtin. ' True ' seçeneğini belirlerseniz, istenen çıkış görüntüsü boyutunu varsayılan olarak **256 olarak belirtebilirsiniz**. 

3.  **Merkez kırpma**için, verilen PIL görüntüsünün merkezinde kırpılacağını belirtin. ' True ' seçeneğini belirlerseniz, kırpma boyutu ' nu varsayılan olarak 224 ' de kırpma **boyutunda**istenen çıktı görüntüsü boyutunu belirtebilirsiniz.  

4.  **Pad**için, belirtilen PIL resminin tüm kenarlarda 0 doldurma değeri ile gösterilip gösterilmeyeceğini belirtin. ' True ' seçeneğini belirlerseniz, **doldurma**içindeki her bir kenarlığa doldurma (kaç piksel eklenecek) belirtebilirsiniz.

5.  **Renk değişimi**için bir görüntünün parlaklığını, kontrastını ve doygunluğu rastgele olarak değiştirip değiştirmeyeceğinizi belirtin.

6.  **Gri tonlamalı**için görüntünün gri tonlamaya dönüştürüp dönüştürülmeyeceğini belirtin.

7.  **Rastgele yeniden boyutlandırılmış kırpma**için, verilen PIL görüntüsünün rastgele boyut ve en boy oranına kırpılıp kırpılmayacağını belirtin. Özgün boyutun (0,08 ile 1,0 arasında) bir kırpılacağını ve orijinal boyut oranının rastgele bir en boy oranını (3/4 Aralık 'tan 4/3) Bu kırpma, son olarak verilen boyut olarak yeniden boyutlandırılır.
    Bu, yaygın olarak kullanılan ağları eğitmek için kullanılır. ' True ' seçeneğini belirlerseniz, varsayılan 256 olarak her bir kenarın beklenen çıkış boyutunu **rastgele boyut**olarak belirtebilirsiniz.

8.  **Rastgele kırpma**için, verilen PIL görüntüsünün rastgele bir konumda kırpılacağını belirtin. ' True ' seçeneğini belirlerseniz, kırpın istenen çıktı boyutunu **rastgele kırpma boyutunda**(varsayılan olarak 224) belirtebilirsiniz.

9.  **Rastgele yatay çevir**için, verilen PIL görüntüsünü, olasılık 0,5 ile rastgele bir şekilde ters çevirmek isteyip istemediğinizi belirtin.

10.  **Rastgele Dikey Çevir**için, verilen PIL görüntüsünü, olasılık 0,5 ile rastgele olarak ters çevirmeyeceğinizi belirtin.

11.  **Rastgele döndürme**için, resmin açılı olarak döndürülüp çevrilmeyeceğini belirtin. ' True ' seçeneğini belirlerseniz, **rastgele döndürme dereceleri**(yani, derece, + derece), varsayılan 0 olarak ayarlayarak derece aralığı içinde belirtebilirsiniz.

12.  **Rastgele**bir ölçü için, görüntünün rastgele bir şekilde dönüştürülmesini istediğiniz görüntüyü belirtin. ' True ' seçeneğini belirlerseniz, varsayılan 0 olarak, (-derece, + derece) anlamına gelen **rastgele bir derece**cinsinden seçilecek derece aralığını belirtebilirsiniz.

13.  **Rastgele gri tonlamalı**için, olasılık 0,1 ile görüntüye rastgele gri tonlamalı olarak dönüştürme yapılıp yapılmayacağını belirtin.

14.  **Rastgele perspektif**için, olasılık 0,5 Ile verilen PIL görüntüsünün perspektif dönüştürmesinin yapılıp yapılmayacağını belirtin.


16.  Yukarıda belirtilen dönüştürmeyi giriş resmi veri kümesine uygulamak için [görüntü dönüştürme modülünü Uygula](apply-image-transformation.md) ' ya bağlanın.

17. İşlem hattını gönderme.

## <a name="results"></a>Sonuçlar

Dönüştürme tamamlandıktan sonra, [görüntü dönüştürme](apply-image-transformation.md) modülünün çıkışında dönüştürülmüş görüntüleri bulabilirsiniz.


## <a name="technical-notes"></a>Teknik notlar  

[https://pytorch.org/docs/stable/torchvision/transforms.html](https://pytorch.org/docs/stable/torchvision/transforms.html)Görüntü dönüştürmesi hakkında daha fazla bilgi için bölümüne bakın.

###  <a name="module-parameters"></a>Modül parametreleri  

| Name                    | Aralık   | Tür    | Varsayılan | Açıklama                              |
| ----------------------- | ------- | ------- | ------- | ---------------------------------------- |
| Yeniden boyutlandırma                  | Herhangi biri     | Boole | True    | Giriş PIL görüntüsünü verilen boyuta göre yeniden boyutlandır |
| Boyut                    | >= 1     | Tamsayı | 256     | İstenen çıkış boyutunu belirtin          |
| Ortadan Kırp             | Herhangi biri     | Boole | True    | Verilen PIL görüntüsünü merkezden kırpar  |
| Kırpma boyutu               | >= 1     | Tamsayı | 224     | Kırpma için istenen çıkış boyutunu belirtin |
| Pad                     | Herhangi biri     | Boole | False   | Verilen "Pad" değeri ile tüm kenarlarda verilen PIL görüntüsünü doldurur |
| İç Boşluk                 | >= 0     | Tamsayı | 0       | Her kenarlıkta doldurma                   |
| Renk değişimi            | Herhangi biri     | Boole | False   | Görüntünün parlaklığını, kontrastını ve doygunluğunu rastgele değiştirme |
| Ölçeğe               | Herhangi biri     | Boole | False   | Görüntüyü gri tonlamaya Dönüştür               |
| Rastgele yeniden boyutlandırılmış kırpma     | Herhangi biri     | Boole | False   | Verilen PIL görüntüsünü rastgele boyut ve en boy oranına göre kırpın |
| Rastgele boyut             | >= 1     | Tamsayı | 256     | Her kenarın beklenen çıkış boyutu        |
| Rastgele kırpma             | Herhangi biri     | Boole | False   | Verilen PIL görüntüsünü rastgele bir konumda kırpın |
| Rastgele kırpma boyutu        | >= 1     | Tamsayı | 224     | Kırpma için istenen çıktı boyutu          |
| Rastgele yatay çevir  | Herhangi biri     | Boole | True    | Belirli bir olasılığa sahip verilen PIL görüntüsünü yatay olarak rastgele çevirin |
| Rastgele Dikey Çevir    | Herhangi biri     | Boole | False   | Verilen PIL görüntüsünü belirli bir olasılığa göre dikey olarak ters çevirin |
| Rastgele döndürme         | Herhangi biri     | Boole | False   | Görüntüyü açıda döndürün                |
| Rastgele döndürme dereceleri | [0180] | Tamsayı | 0       | Seçilecek derece aralığı          |
| Rastgele afin           | Herhangi biri     | Boole | False   | Görüntünün rastgele Afine dönüştürmesi, merkezi sabit tutma |
| Rastgele afin derecesi   | [0180] | Tamsayı | 0       | Seçilecek derece aralığı          |
| Rastgele gri tonlamalı        | Herhangi biri     | Boole | False   | Olasılık 0,1 ile görüntüyü gri tonlamaya rastgele dönüştürün |
| Rastgele perspektif      | Herhangi biri     | Boole | False   | Verilen PIL görüntüsünün perspektif dönüşümünü 0,5 olasılık ile rastgele gerçekleştirir |
| Rastgele silme          | Herhangi biri     | Boole | False   | Görüntüde rastgele bir dikdörtgen bölgesi seçer ve 0,5 olasılığa sahip piksellerini siler |

###  <a name="output"></a>Çıktı  

| Name                        | Tür                    | Açıklama                              |
| --------------------------- | ----------------------- | ---------------------------------------- |
| Çıkış görüntüsü dönüştürme | Dönüştürme Tiondirectory | **Görüntü dönüştürme modülünü uygulamak** için bağlanýlabilen çıkış resmi dönüştürmesi. |

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 