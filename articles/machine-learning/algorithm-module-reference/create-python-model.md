---
title: 'Python Model in oluşturma: Modül başvurusu'
titleSuffix: Azure Machine Learning
description: Azure Machine Learning'de Özel bir modelleme veya veri işleme modülü oluşturmak için Python Modeli Oluştur modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 11/19/2019
ms.openlocfilehash: 929938bba9c9512ecfd663a540cf4a7ebbf68e2b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79371826"
---
# <a name="create-python-model-module"></a>Python Model modül oluşturma

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

Python komut dosyasından eğitimsiz bir model oluşturmak için Python Model Oluştur modüllerini nasıl kullanacağınızı öğrenin. Modeli, Azure Machine Learning tasarımcı ortamında python paketinde bulunan tüm öğrencilere dayandırabilirsiniz. 

Modeli oluşturduktan sonra, Azure Machine Learning'deki diğer öğrenciler gibi modeli bir veri kümesinde eğitmek için [Train Model'i](train-model.md) kullanabilirsiniz. Eğitimli model tahmin yapmak için [Puan Modeli](score-model.md) geçirilebilir. Daha sonra eğitimli modeli kaydedebilir ve puanlama iş akışını bir web hizmeti olarak yayımlayabilirsiniz.

> [!WARNING]
> Şu anda, Bir Python modelinin puanlı sonuçlarını [Model'i değerlendirmek](evaluate-model.md)için geçmek mümkün değildir. Bir modeli değerlendirmeniz gerekiyorsa, özel bir Python komut dosyası yazabilir ve Python Script modüllerini [çalıştırarak](execute-python-script.md) çalıştırabilirsiniz.  


## <a name="configure-the-module"></a>Modülü yapılandırın

Bu modülün kullanımı Python'un ara veya uzman bilgisi gerektirir. Modül, Azure Machine Learning'de zaten yüklü olan Python paketlerine dahil olan tüm öğrencilerin kullanımını destekler. [Python Komut Dosyası Yürüt'te](execute-python-script.md)önceden yüklenmiş Python paket listesine bakın.
  

Bu makalede, basit bir ardışık işlem ile **Python Model oluştur** nasıl kullanılacağını gösterir. Burada boru hattı nın bir diyagramı:

![Python Modeli Oluşturma Diyagramı](./media/module/create-python-model.png)

1. **Python Modeli Oluştur'u**seçin ve modelleme veya veri yönetimi işleminizi uygulamak için komut dosyasını düzenlediniz. Modeli, Azure Machine Learning ortamında python paketinde bulunan tüm öğrencilere dayandırabilirsiniz.

   İki sınıfna ait Naive Bayes sınıflandırıcısının aşağıdaki örnek kodu popüler *sklearn* paketini kullanır:

   ```Python

   # The script MUST define a class named AzureMLModel.
   # This class MUST at least define the following three methods:
       # __init__: in which self.model must be assigned,
       # train: which trains self.model, the two input arguments must be pandas DataFrame,
       # predict: which generates prediction result, the input argument and the prediction result MUST be pandas DataFrame.
   # The signatures (method names and argument names) of all these methods MUST be exactly the same as the following example.


   import pandas as pd
   from sklearn.naive_bayes import GaussianNB


   class AzureMLModel:
       def __init__(self):
           self.model = GaussianNB()
           self.feature_column_names = list()

       def train(self, df_train, df_label):
           self.feature_column_names = df_train.columns.tolist()
           self.model.fit(df_train, df_label)

       def predict(self, df):
           return pd.DataFrame(
               {'Scored Labels': self.model.predict(df[self.feature_column_names]), 
                'probabilities': self.model.predict_proba(df[self.feature_column_names])[:, 1]}
           )


   ```

1. Model ve **Puan Modelini** **Eğitmek** için oluşturduğunuz **Python Model** Oluştur modülünü bağlayın.

1. Modeli değerlendirmeniz gerekiyorsa, Bir [Execute Python Script](execute-python-script.md) modülü ekleyin ve Python komut dosyasını düzenlemeyi.

   Aşağıdaki komut dosyası örnek değerlendirme kodudur:

   ```Python


   # The script MUST contain a function named azureml_main
   # which is the entry point for this module.

   # imports up here can be used to 
   import pandas as pd

   # The entry point function can contain up to two input arguments:
   #   Param<dataframe1>: a pandas.DataFrame
   #   Param<dataframe2>: a pandas.DataFrame
   def azureml_main(dataframe1 = None, dataframe2 = None):
    
       from sklearn.metrics import accuracy_score, precision_score, recall_score, roc_auc_score, roc_curve
       import pandas as pd
       import numpy as np
    
       scores = dataframe1.ix[:, ("income", "Scored Labels", "probabilities")]
       ytrue = np.array([0 if val == '<=50K' else 1 for val in scores["income"]])
       ypred = np.array([0 if val == '<=50K' else 1 for val in scores["Scored Labels"]])    
       probabilities = scores["probabilities"]
    
       accuracy, precision, recall, auc = \
       accuracy_score(ytrue, ypred),\
       precision_score(ytrue, ypred),\
       recall_score(ytrue, ypred),\
       roc_auc_score(ytrue, probabilities)
    
       metrics = pd.DataFrame();
       metrics["Metric"] = ["Accuracy", "Precision", "Recall", "AUC"];
       metrics["Value"] = [accuracy, precision, recall, auc]
    
       return metrics,

   ```

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 