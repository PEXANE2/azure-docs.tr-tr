---
title: 'Verileri El Ile Girin: Modül başvurusu'
titleSuffix: Azure Machine Learning
description: Değerleri yazarak küçük bir veri kümesi oluşturmak için Azure Machine Learning'de Verileri El Ile Gir modüllerini nasıl kullanacağınızı öğrenin. Veri kümesinde birden çok sütun olabilir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 35e31e5ace53654e8aad794dd3e25fc04bd9a088
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79367525"
---
# <a name="enter-data-manually-module"></a>Veri Modüllerini El Ile Girin

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

Değerleri yazarak küçük bir veri kümesi oluşturmak için **Verileri El Ile Gir** modüllerini kullanın. Veri kümesinde birden çok sütun olabilir.
  
Bu modül gibi senaryolarda yararlı olabilir:  
  
- Sınama için küçük bir değer kümesi oluşturma.  
- Etiketlerin kısa bir listesini oluşturma.  
- Veri kümesine eklemek için sütun adlarının listesini yazma.

## <a name="create-a-dataset"></a>Veri kümesi oluşturma 
  
1. Veri [Girin modüllerini](./enter-data-manually.md) el ile ardınıza ekleyin. Bu modülü Azure Machine Learning'de **Veri Girişi ve Çıktısı** kategorisinde bulabilirsiniz. 
  
1. **DataFormat**için aşağıdaki seçeneklerden birini seçin. Bu seçenekler, sağladığınız verilerin nasıl ayrışdırılması gerektiğini belirler. Her biçim için gereksinimler büyük ölçüde farklıdır, bu nedenle ilgili konuları okuduğunuzdan emin olun.  
  
   - **ARFF**: Weka tarafından kullanılan öznitelik ilişkisi dosya biçimi.   
   - **CSV**: Virgülden ayrılmış değerler biçimidir. Daha fazla bilgi için [CSV'ye dönüştür'e](./convert-to-csv.md)bakın.    
   - **SVMLight**: Vowpal Wabbit ve diğer makine öğrenimi çerçeveleri tarafından kullanılan biçimdir.    
   - **TSV**: Sekme ayrılmış değerler biçimi.

   Bir biçim seçerseniz ve biçim belirtimlerini karşılayan veriler sağlamazsanız, çalışma zamanı hatası oluşur.
  
1. Veri girmeye başlamak için **Veri** metin kutusunun içini tıklatın. Aşağıdaki biçimler özel dikkat gerektirir:  
  
   - **CSV**: Birden çok sütun oluşturmak, virgülle ayrılmış metne yapıştırmak veya alanlar arasında virgül kullanarak birden çok sütun yazın.
  
     **HasHeader** seçeneğini seçerseniz, sütun başlığı olarak ilk değer satırını kullanabilirsiniz.  
  
     Bu seçeneği seçerseniz, sütun adları (Col1, Col2 ve benzeri) kullanılır. Daha sonra [Meta Verilerini Edit'i](./edit-metadata.md)kullanarak sütun adları ekleyebilir veya değiştirebilirsiniz.  
  
   - **TSV**: Birden çok sütun oluşturmak için, sekme ayrılmış metne yapıştırın veya alanlar arasında sekmeler kullanarak birden çok sütun yazın.  
  
     **HasHeader** seçeneğini seçerseniz, sütun başlığı olarak ilk değer satırını kullanabilirsiniz.  
  
     Bu seçeneği seçerseniz, sütun adları (Col1, Col2 ve benzeri) kullanılır. Daha sonra [Meta Verilerini Edit'i](./edit-metadata.md)kullanarak sütun adları ekleyebilir veya değiştirebilirsiniz.  
  
   - **ARFF**: Varolan bir ARFF biçim dosyasına yapıştırın. Değerleri doğrudan yazıyorsanız, verilerin başında isteğe bağlı üstbilgi ve gerekli öznitelik alanlarını eklediğinizden emin olun. 

     Örneğin, aşağıdaki üstbilgi ve öznitelik satırları basit bir listeye eklenebilir. Sütun başlığı `SampleText`. String türünün desteklenmediğini unutmayın.
    
     ```text
     % Title: SampleText.ARFF  
     % Source: Enter Data module  
     @ATTRIBUTE SampleText NUMERIC  
     @DATA  
     \<type first data row here>  
     ```

   - **SVMLight : SVMLight**biçimini kullanarak değerlere yazın veya yapıştırın.  
  
     Örneğin, aşağıdaki örnek SVMLight biçiminde Kan Bağışı veri kümesinin ilk birkaç satırını temsil eder:  
  
     ```text  
     # features are [Recency], [Frequency], [Monetary], [Time]  
     1 1:2 2:50 3:12500 4:98   
     1 1:0 2:13 3:3250 4:28   
     ```  
  
     [Veri Girin](./enter-data-manually.md) modüllerini çalıştırdığınızda, bu satırlar aşağıdaki gibi sütunlar ve dizin değerleri veri kümesine dönüştürülür:  
  
     |Süt1|Süt2|Col3|Col4|Etiketler|  
     |-|-|-|-|-|  
     |0.00016|0.004|0.999961|0.00784|1|  
     |0|0.004|0.999955|0.008615|1|  
  
1. Yeni bir satır başlatmak için her satırdan sonra Enter tuşunu seçin.      
     
   Birden çok boş satır eklemek için birden çok kez Enter'u seçerseniz, boş satırlar kaldırılır veya kırpılır.  
  
   Eksik değerlere sahip satırlar oluşturursanız, bunları her zaman daha sonra filtreleyebilirsiniz.  
  
1. Çıkış bağlantı noktasını diğer modüllere bağlayın ve ardışık hattı çalıştırın.  
  
   Veri kümesini görüntülemek için modülü sağ tıklatın ve **Visualize'ı**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 