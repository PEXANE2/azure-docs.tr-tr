---
title: Görüntü Dizinini Bölme
titleSuffix: Azure Machine Learning
description: Bir görüntü dizininin görüntülerini iki ayrı küme içine bölmek için Azure Machine Learning tasarımcısında bölünmüş görüntü dizini modülünü nasıl kullanacağınızı öğrenin (Önizleme).
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 09/26/2020
ms.openlocfilehash: fe5ba25904298fe1a394a4b01d6bdacc72d599c9
ms.sourcegitcommit: 3792cf7efc12e357f0e3b65638ea7673651db6e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91448682"
---
# <a name="split-image-directory"></a>Görüntü Dizinini Bölme

Bu konuda, bir görüntü dizininin görüntülerini iki ayrı kümesine bölmek için Azure Machine Learning tasarımcısında bölünmüş görüntü dizini modülünün nasıl kullanılacağı açıklanmaktadır.

Bu modül, görüntü verilerini eğitim ve test kümelerine ayırmanız gerektiğinde özellikle yararlıdır. 

## <a name="how-to-configure-split-image-directory"></a>Bölünmüş görüntü dizinini yapılandırma

1. Ardışık Düzen **Dizin** modülünü işlem hattınızı ekleyin. Bu modülü ' Görüntü İşleme/Image Data Transformation ' kategorisinin altında bulabilirsiniz.

2. Çıktıyı çıktının görüntü dizini olduğu modüle bağlayın.

3. Sol bölünmeye yerleştirilecek verilerin yüzdesini belirtmek için, varsayılan 0,9 olarak, **ilk çıktıda görüntülerin giriş bölümü** . Kesir sonucu tamsayı değilse, modül yakın küçük tamsayı kullanır.


## <a name="technical-notes"></a>Teknik notlar

### <a name="expected-inputs"></a>Beklenen girişler

| Ad                  | Tür           | Açıklama              |
| --------------------- | -------------- | ------------------------ |
| Giriş resmi dizini | ImageDirectory | Bölünecek görüntü dizini |

### <a name="module-parameters"></a>Modül parametreleri

| Ad                                   | Tür  | Aralık | İsteğe Bağlı | Açıklama                            | Varsayılan |
| -------------------------------------- | ----- | ----- | -------- | -------------------------------------- | ------- |
| İlk çıktıda görüntülerin kesri | Float | 0-1   | Gerekli | İlk çıktıda görüntülerin kesri | 0,9     |

### <a name="outputs"></a>Çıkışlar

| Ad                    | Tür           | Açıklama                              |
| ----------------------- | -------------- | ---------------------------------------- |
| Çıkış resmi Directory1 | ImageDirectory | Seçili görüntüleri içeren görüntü dizini |
| Çıkış resmi directory2 | ImageDirectory | Diğer tüm görüntüleri içeren görüntü dizini |

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 

