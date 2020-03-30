---
title: 'Kümeye Veri Atama: Modül Başvurusu'
titleSuffix: Azure Machine Learning
description: Kümeleme modeli puanlamak için Azure Machine Learning'de Kümemodülüne Veri Atama'yı nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 11/19/2019
ms.openlocfilehash: 207172f10277589af2b22ae2f41b07234a0925b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477723"
---
# <a name="module-assign-data-to-clusters"></a>Modül: Kümelere Veri Atama

Bu makalede, Azure Machine Learning tasarımcısında (önizleme) *Kümelere Veri Atama* modülünasıl kullanılacağı açıklanmaktadır. Modül, *K-araçları kümeleme* algoritması ile eğitilmiş bir kümeleme modeli aracılığıyla öngörüler oluşturur.

Kümelere Veri Atama modülü, her yeni veri noktası için olası atamaları içeren bir veri kümesi döndürür. 

## <a name="how-to-use-assign-data-to-clusters"></a>Kümelere Veri Atama nasıl kullanılır?
  
1. Azure Machine Learning tasarımcısında, önceden eğitilmiş bir kümeleme modeli bulun. Aşağıdaki yöntemlerden birini kullanarak bir kümeleme modeli oluşturabilir ve eğitebilirsiniz:  
  
    - [K-Araçları Kümeleme](k-means-clustering.md) modüllerini kullanarak K-araçları kümeleme algoritmasını yapılandırın ve bir veri kümesi ve Tren Kümeleme Modeli modüllerini (bu makalede) kullanarak modeli eğitin.  
  
    - Ayrıca, çalışma alanınızda **Kayıtlı Modeller** grubundan varolan bir eğitilmiş kümeleme modeli de ekleyebilirsiniz.

2. Bilgili modeli **Kümelere Veri Atama'nın**sol giriş noktasına takın.  

3. Giriş olarak yeni bir veri kümesi takın. 

   Bu veri kümesinde etiketler isteğe bağlıdır. Genellikle kümeleme denetimsiz bir öğrenme yöntemidir. Kategorileri önceden bilmeniz beklenmez. Ancak, giriş sütunları kümeleme modelini eğitiminde kullanılan sütunlarla aynı olmalıdır veya bir hata oluşur.

    > [!TIP]
    > Küme tahminlerinden tasarımcıya yazılan sütun sayısını azaltmak için [veri kümesindeki sütunları seç](select-columns-in-dataset.md)ve sütunların bir alt kümesini seçin. 
    
4. Sonuç **için Onay'ı bırakın veya sonuç için onay al, yalnızca** sonuçların sonuçları görüntüleyen bir sütun (küme atamaları) dahil olmak üzere tam giriş veri kümesini içermesini istiyorsanız seçili onay kutusunu bırakın.
  
    Bu onay kutusunu temizlerseniz, yalnızca sonuçlar döndürülür. Bu seçenek, bir web hizmetinin parçası olarak öngörüler oluşturduğunuzda yararlı olabilir.
  
5.  Boru hattını gönderin.  
  
### <a name="results"></a>Sonuçlar

+  Veri kümesindeki değerleri görüntülemek için modülü sağ tıklatın ve ardından **Visualize'ı**seçin. Veya modülü seçin ve sağ paneldeki **Çıktılar** sekmesine geçin, sonucu görselleştirmek için **Bağlantı Noktası çıkışlarında** histogram simgesine tıklayın.

