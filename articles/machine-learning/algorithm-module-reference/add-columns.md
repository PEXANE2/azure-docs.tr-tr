---
title: 'Sütun Ekle: Modül Başvurusu'
titleSuffix: Azure Machine Learning
description: İki veri kümesini biraraya getirmek için Azure Machine Learning'de Sütun Ekle modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/22/2019
ms.openlocfilehash: f2e067f76d6ed7d89a38e9b8920c407f161969a8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79456787"
---
# <a name="add-columns-module"></a>Sütun Ekle modülü

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

İki veri kümesini birleştirmek için bu modülü kullanın. Tek bir veri kümesi oluşturmak için giriş olarak belirttiğiniz iki veri kümesindeki tüm sütunları birleştirirsiniz. İkiden fazla veri kümesini birleştirmeniz gerekiyorsa, Sütun **Ekle'nin**birkaç örneğini kullanın.



## <a name="how-to-configure-add-columns"></a>Sütun Ekle nasıl yapılandırılır?
1. **Ardışık** hattınıza Sütun Ekle modüllerini ekleyin.

2. Birleştirmek istediğiniz iki veri kümesini bağlayın. İkiden fazla veri kümesini birleştirmek istiyorsanız, **Sütun Ekle'nin**birkaç birleşimini birbirine zincirleyebilirsiniz.

    - Farklı sayıda satıra sahip iki sütunu birleştirmek mümkündür. Çıktı veri kümesi, daha küçük kaynak sütundaki her satır için eksik değerlerle tamamlanır.

    - Eklenecek tek tek sütunları seçemezsiniz. **Sütun Ekle'yi**kullandığınızda her veri kümesindeki tüm sütunlar birleştirilmiştir. Bu nedenle, sütunların yalnızca bir alt kümesini eklemek istiyorsanız, istediğiniz sütunlarla bir veri kümesi oluşturmak için Veri Kümesi'ndeki Sütunları Seç'i kullanın.

3. Boru hattını gönderin.

### <a name="results"></a>Sonuçlar
Boru hattı çalıştırıldıktan sonra:

- Yeni veri kümesinin ilk satırlarını görmek için **Sütun Ekle** modülüne sağ tıklayın ve Visualize'ı seçin. Veya modülü seçin ve sağ paneldeki **Çıktılar** sekmesine geçin, sonucu görselleştirmek için **Bağlantı Noktası çıkışlarında** histogram simgesine tıklayın.

Yeni veri kümesindeki sütun sayısı, her iki giriş veri kümesinin sütunlarının toplamına eşittir.

Giriş veri kümelerinde aynı ada sahip iki sütun varsa, sütunun adına sayısal bir sonek eklenir. Örneğin, TargetOutcome adlı bir sütunun iki örneği varsa, sol sütun TargetOutcome_1 olarak yeniden adlandırılır ve sağ sütun TargetOutcome_2 olarak yeniden adlandırılır.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 