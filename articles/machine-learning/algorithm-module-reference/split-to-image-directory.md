---
title: Görüntü dizinini Böl
titleSuffix: Azure Machine Learning
description: Eğitilen bir görüntü modeli kullanarak tahmin oluşturmak için Azure Machine Learning görüntü modeli modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 05/26/2020
ms.openlocfilehash: 9673b3260425fd9244c635beaf77d367a14cac54
ms.sourcegitcommit: 0a5bb9622ee6a20d96db07cc6dd45d8e23d5554a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84450839"
---
# <a name="split-image-directory"></a>Görüntü dizinini Böl

Bu konu başlığı altında, bir görüntü dizininin görüntülerini iki ayrı küme içine bölmek için Azure Machine Learning Designer 'da (Önizleme) bölünmüş görüntü dizini modülünün nasıl kullanılacağı açıklanmaktadır.

Bu modül, görüntü verilerini eğitim ve test kümelerine ayırmanız gerektiğinde özellikle yararlıdır. 

## <a name="how-to-configure-split-image-directory"></a>Bölünmüş görüntü dizinini yapılandırma

1. Ardışık Düzen **Dizin** modülünü işlem hattınızı ekleyin. Bu modülü ' Görüntü İşleme/Image Data Transformation ' kategorisinin altında bulabilirsiniz.

2. Çıktıyı çıktının görüntü dizini olduğu modüle bağlayın.

3. Sol bölünmeye yerleştirilecek verilerin yüzdesini belirtmek için, varsayılan 0,9 olarak, **ilk çıktıda görüntülerin giriş bölümü** .


## <a name="technical-notes"></a>Teknik notlar

### <a name="expected-inputs"></a>Beklenen girişler

| Adı                  | Tür           | Açıklama              |
| --------------------- | -------------- | ------------------------ |
| Giriş resmi dizini | ImageDirectory | Bölünecek görüntü dizini |

### <a name="module-parameters"></a>Modül parametreleri

| Adı                                   | Tür  | Aralık | İsteğe Bağlı | Açıklama                            | Varsayılan |
| -------------------------------------- | ----- | ----- | -------- | -------------------------------------- | ------- |
| İlk çıktıda görüntülerin kesri | Float | 0-1   | Gerekli | İlk çıktıda görüntülerin kesri | 0.9     |

### <a name="outputs"></a>Çıkışlar

| Adı                    | Tür           | Açıklama                              |
| ----------------------- | -------------- | ---------------------------------------- |
| Çıkış resmi Directory1 | ImageDirectory | Seçili görüntüleri içeren görüntü dizini |
| Çıkış resmi directory2 | ImageDirectory | Diğer tüm görüntüleri içeren görüntü dizini |

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 

