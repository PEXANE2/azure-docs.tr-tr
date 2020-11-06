---
title: PyTorch Modelini eğitme
titleSuffix: Azure Machine Learning
description: Modelleri sıfırdan eğitme veya mevcut modelleri ince ayar yapmak için Azure Machine Learning Designer 'daki Pytorch modellerini eğitme modülünü kullanın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 09/26/2020
ms.openlocfilehash: 2d88069f33995bdbe9dd479afe9a4e72ab9939b6
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2020
ms.locfileid: "93420674"
---
# <a name="train-pytorch-model"></a>PyTorch Modelini eğitme

Bu makalede, DenseNet gibi pytorch modellerini eğmek için Azure Machine Learning Designer 'da **Pytorch model** modülünün nasıl kullanılacağı açıklanır. Bir modeli tanımladıktan ve parametrelerini ayarladıktan sonra ve etiketli veriler gerektirdiğinde eğitim gerçekleşir. 

## <a name="how-to-use-train-pytorch-model"></a>Pytorch modeliyle eğitme kullanma 

1. Tasarımcı 'daki işlem hattı taslağınızı [Densenet](densenet.md) modülü veya [ResNet](resnet.md) olarak ekleyin.

2. İşlem hattına **Pytorch model** modülünü ekleyin. Bu modülü **model eğitimi** kategorisi altında bulabilirsiniz. **Eğit** ' i genişletin ve ardından **Train Pytorch Model**

   > [!NOTE]
   > **Pytorch model modülünü eğitme** , büyük veri kümesi için **GPU** türü hesaplama üzerinde daha iyi çalışır, aksi takdirde ardışık düzen başarısız olur. Modülün sağ bölmesinde, **diğer işlem hedefini kullan** ' ı seçerek belirli bir modül için işlem seçebilirsiniz.

3.  Sol girişte, eğitimli olmayan bir model ekleyin. Eğitim veri kümesini ve doğrulama veri kümesini, **Pytorch modelini eğitme** ve sağ taraftaki girişe iliştirin.

    Eğitilmiş olmayan model için, DenseNet gibi bir pytorch modeli olmalıdır. Aksi takdirde, ' ınvalidmodeldirectoryerror ' atılır.

    Veri kümesi için eğitim veri kümesi etiketli bir görüntü dizini olmalıdır. Etiketli görüntü dizini alma için **görüntü dizinine dönüştürme** bölümüne bakın. Etiketlenmişse, ' NotLabeledDatasetError ' atılır.

    Eğitim veri kümesi ve doğrulama veri kümesi aynı etiket kategorilerine sahiptir, aksi takdirde bir InvalidDatasetError oluşturulur.

4.  **Dönemler** için, kaç dönemler arasında eğmek istediğinizi belirtin. Tüm veri kümesi, varsayılan olarak her dönem içinde tekrarlandırılır.

5.  **Toplu iş boyutu** için, bir toplu işte kaç örnek eğitmek istediğinizi belirtin, varsayılan olarak 16.

6.  **Öğrenme oranı** için *öğrenme oranı* için bir değer belirtin. Öğrenme oranı değerleri, model her test edildiğinde ve düzeltildiğinde her seferinde SGD gibi iyileştiricinin kullandığı adımın boyutunu denetler.

    Hızı daha az yaparak, yerel bir Play 'de kalmış olabileceğiniz riskle modeli daha sık test edersiniz. Adımı daha büyük yaparak, doğru Minima 'yı aşırı atma riskine göre daha hızlı bir şekilde yakınsama yapabilirsiniz. Varsayılan olarak 0,001.

7.  **Rastgele çekirdek** için, isteğe bağlı olarak, çekirdek olarak kullanılacak bir tamsayı değeri yazın. Çalıştırmanın çalışmalarından reproducibility emin olmak istiyorsanız çekirdek kullanılması önerilir.

8.  **Haence** için, doğrulama kaybı arka arkaya azalmadığında eğitimin erken olarak nasıl durdurulacağını belirten bir dönemler belirtin. Varsayılan olarak 3.

9.  İşlem hattını gönderme. Veri kümenizin boyutu daha büyükse, bu işlem biraz zaman alabilir ve GPU işlem yapmanız önerilir.

## <a name="results"></a>Sonuçlar

İşlem hattı çalıştırması tamamlandıktan sonra, puanlama modelini kullanmak için, yeni giriş örneklerine ilişkin değerleri tahmin etmek üzere, bir [görüntü modeline](score-image-model.md)yönelik [eğitim](train-pytorch-model.md)

## <a name="technical-notes"></a>Teknik notlar
###  <a name="expected-inputs"></a>Beklenen girişler  

| Ad               | Tür                    | Açıklama                              |
| ------------------ | ----------------------- | ---------------------------------------- |
| Eğitilen model    | Untraınedmodeldirectory | Eğitilen model, pytorch gerektir         |
| Eğitim veri kümesi   | ImageDirectory          | Eğitim veri kümesi                         |
| Doğrulama veri kümesi | ImageDirectory          | Her dönem değerlendirmesi için doğrulama veri kümesi |

###  <a name="module-parameters"></a>Modül parametreleri  

| Ad          | Aralık            | Tür    | Varsayılan | Açıklama                              |
| ------------- | ---------------- | ------- | ------- | ---------------------------------------- |
| Dönemler        | >0               | Tamsayı | 5       | Etiket veya sonuç sütununu içeren sütunu seçin |
| Toplu iş boyutu    | >0               | Tamsayı | 16      | Bir toplu işte eğiten örnek sayısı   |
| Öğrenme oranı | >= Double. Upsilon | Float   | 0.001   | Stochastic gradyanı ilgili iyileştiricinin ilk öğrenme oranı. |
| Rastgele çekirdek   | Herhangi bir              | Tamsayı | 1       | Model tarafından kullanılan rastgele sayı oluşturucusunun çekirdek. |
| Beklediğiniz      | >0               | Tamsayı | 3       | Eğitimin erken durdurulması için kaç dönemler   |

###  <a name="outputs"></a>Çıkışlar  

| Ad          | Tür           | Açıklama   |
| ------------- | -------------- | ------------- |
| Eğitilen model | ModelDirectory | Eğitilen model |

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 



