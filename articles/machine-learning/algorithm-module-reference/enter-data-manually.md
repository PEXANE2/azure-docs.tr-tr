---
title: 'Verileri el Ile girin: modül başvurusu'
titleSuffix: Azure Machine Learning
description: Değer yazarak küçük bir veri kümesi oluşturmak için Azure Machine Learning verileri el Ile gir modülünü nasıl kullanacağınızı öğrenin. Veri kümesinde birden çok sütun olabilir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 35e31e5ace53654e8aad794dd3e25fc04bd9a088
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79367525"
---
# <a name="enter-data-manually-module"></a>Verileri el Ile gir modülü

Bu makalede Azure Machine Learning tasarımcısında modül (Önizleme) açıklanmaktadır.

Değer yazarak küçük bir veri kümesi oluşturmak için **verileri el Ile gir** modülünü kullanın. Veri kümesinde birden çok sütun olabilir.
  
Bu modül, aşağıdaki senaryolarda yararlı olabilir:  
  
- Test için küçük bir değer kümesi oluşturuluyor.  
- Etiketlerin kısa bir listesini oluşturma.  
- Bir veri kümesine eklenecek sütun adlarının listesini yazma.

## <a name="create-a-dataset"></a>Veri kümesi oluşturma 
  
1. [Verileri el Ile gir](./enter-data-manually.md) modülünü ardışık düzene ekleyin. Bu modülü, Azure Machine Learning **veri girişi ve çıkış** kategorisinde bulabilirsiniz. 
  
1. **DataFormat**için aşağıdaki seçeneklerden birini seçin. Bu seçenekler, sağladığınız verilerin nasıl ayrıştırılaceğini belirlemektir. Her biçimin gereksinimleri önemli ölçüde farklılık gösterir, bu nedenle ilgili konuları okuduğunuzdan emin olun.  
  
   - **Arff**: WEKA tarafından kullanılan öznitelik ilişkisi dosya biçimi.   
   - **CSV**: virgülle ayrılmış değerler biçimi. Daha fazla bilgi için bkz. [CSV 'ye dönüştürme](./convert-to-csv.md).    
   - **Svmlight**: Vowpal Wabbit ve diğer makine öğrenimi çerçeveleri tarafından kullanılan biçim.    
   - **TSV**: sekmeyle ayrılmış değerler biçimi.

   Bir biçim seçer ve biçim belirtimlerini karşılayan veriler sağlamazsanız, bir çalışma zamanı hatası oluşur.
  
1. Veri girmeye başlamak için **veri** metin kutusunun içine tıklayın. Aşağıdaki biçimler özel dikkat gerektirir:  
  
   - **CSV**: birden çok sütun oluşturmak için, virgülle ayrılmış metinde yapıştırın veya alanlar arasında virgül kullanarak birden çok sütun yazın.
  
     **HasHeader** seçeneğini belirlerseniz, ilk değer satırını sütun başlığı olarak kullanabilirsiniz.  
  
     Bu seçeneğin işaretini kaldırırsanız, sütun adları (Sütun1, Col2, vb.) kullanılır. Daha sonra, [meta verileri Düzenle](./edit-metadata.md)kullanarak sütun adlarını ekleyebilir veya değiştirebilirsiniz.  
  
   - **TSV**: birden çok sütun oluşturmak için, sekmeyle ayrılmış metin içine yapıştırın veya alanlar arasında sekmeleri kullanarak birden çok sütun yazın.  
  
     **HasHeader** seçeneğini belirlerseniz, ilk değer satırını sütun başlığı olarak kullanabilirsiniz.  
  
     Bu seçeneğin işaretini kaldırırsanız, sütun adları (Sütun1, Col2, vb.) kullanılır. Daha sonra, [meta verileri Düzenle](./edit-metadata.md)kullanarak sütun adlarını ekleyebilir veya değiştirebilirsiniz.  
  
   - **Arff**: varolan bir arff biçim dosyasını yapıştırın. Değerleri doğrudan yazıyorsanız, verilerin başına isteğe bağlı üstbilgiyi ve gerekli öznitelik alanlarını eklediğinizden emin olun. 

     Örneğin, aşağıdaki üst bilgi ve öznitelik satırları basit bir listeye eklenebilir. Sütun başlığı şöyle olacaktır `SampleText`. Dize türünün desteklenmediğini unutmayın.
    
     ```text
     % Title: SampleText.ARFF  
     % Source: Enter Data module  
     @ATTRIBUTE SampleText NUMERIC  
     @DATA  
     \<type first data row here>  
     ```

   - **Svmlight**: svmlight biçimini kullanarak değerlere yazın veya yapıştırın.  
  
     Örneğin, aşağıdaki örnek, SVMLight biçiminde kan bağış veri kümesinin ilk birkaç satırını temsil eder:  
  
     ```text  
     # features are [Recency], [Frequency], [Monetary], [Time]  
     1 1:2 2:50 3:12500 4:98   
     1 1:0 2:13 3:3250 4:28   
     ```  
  
     [Verileri el Ile gir](./enter-data-manually.md) modülünü çalıştırdığınızda, bu satırlar sütun ve dizin değerlerinin veri kümesine aşağıdaki şekilde dönüştürülür:  
  
     |Süt1|Süt2|Col3|Col4|Etiketler|  
     |-|-|-|-|-|  
     |0,00016|0,004|0,999961|0,00784|1|  
     |0|0,004|0,999955|0,008615|1|  
  
1. Yeni bir satır başlatmak için her satırdan sonra Enter tuşunu seçin.      
     
   Birden fazla boş son satır eklemek için birden çok kez gir ' i seçerseniz boş satırlar kaldırılır veya kırpılır.  
  
   Eksik değerleri olan satırlar oluşturursanız, bunları her zaman daha sonra filtreleyebilirsiniz.  
  
1. Çıkış bağlantı noktasını diğer modüllere bağlayın ve işlem hattını çalıştırın.  
  
   Veri kümesini görüntülemek için modüle sağ tıklayın ve **Görselleştir**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 