---
title: 'Python modeli oluşturma: modül başvurusu'
titleSuffix: Azure Machine Learning
description: Özel Modelleme veya veri işleme modülü oluşturmak için Azure Machine Learning Python modeli oluşturma modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 11/19/2019
ms.openlocfilehash: 0285520c2733cd6e190f9055824cdfed0ce4b842
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82189863"
---
# <a name="create-python-model-module"></a>Python model modülü oluşturma

Bu makalede Azure Machine Learning tasarımcısında modül (Önizleme) açıklanmaktadır.

Python betiğiyle eğitilen bir model oluşturmak için Python modeli oluşturma modülünü nasıl kullanacağınızı öğrenin. Modeli, Azure Machine Learning tasarımcı ortamındaki bir Python paketine dahil olan herhangi bir öğrenici üzerine temelleyebilirsiniz. 

Modeli oluşturduktan sonra, Azure Machine Learning diğer öğrenme gibi bir veri kümesi üzerinde modeli eğmek için [eğitme modeli](train-model.md) ' ni kullanabilirsiniz. Eğitilen model, tahmine dayalı hale getirmek için [puan modeline](score-model.md) geçirilebilir. Daha sonra eğitilen modeli kaydedebilir ve Puanlama iş akışını bir Web hizmeti olarak yayımlayabilirsiniz.

> [!WARNING]
> Şu anda [modeli değerlendirmek](evaluate-model.md)Için bir Python modelinin puanlanmış sonuçlarının iletilmesi mümkün değildir. Bir modeli değerlendirmeniz gerekiyorsa, özel bir Python betiği yazabilir ve [Python betiği Yürüt](execute-python-script.md) modülünü kullanarak çalıştırabilirsiniz.  


## <a name="configure-the-module"></a>Modülü yapılandırma

Bu modülün kullanımı, Python 'un ara veya uzman bilgisini gerektirir. Modül, Azure Machine Learning ' de zaten yüklü olan Python paketlerine dahil olan herhangi bir Learner 'ın kullanımını destekler. [Python betiğini yürütme](execute-python-script.md)içindeki önceden yüklenmiş Python paketi listesine bakın.

> [!NOTE]
> Lütfen komut dosyanızı yazarken dikkatli olun ve bildirilmeyen bir nesne ya da içeri aktarılmamış bir modül kullanma gibi bir sözdizimi hatası olmadığından emin olun.

> [!NOTE]
> Ayrıca, [Python betiğini yürütme](execute-python-script.md)bölümünde önceden yüklenmiş modüller listesine ek zayıflalar da ödeyin. Yalnızca önceden yüklenmiş modülleri içeri aktar. Lütfen bu betikte "PIP install xgboost" gibi ek paketleri yüklemeyin, aksi takdirde, alt akış modüllerinde modeller okunurken hatalar oluşur.
  
Bu makalede, basit bir işlem hattı ile **Python modeli oluşturma** 'nın nasıl kullanılacağı gösterilmektedir. Ardışık düzenin diyagramı aşağıda verilmiştir:

![Python modeli oluşturma diyagramı](./media/module/create-python-model.png)

1. **Python modeli oluştur**' u seçin ve komut dosyasını düzenleyerek modelleme veya veri yönetimi işleminizi uygulayın. Modeli, Azure Machine Learning ortamındaki bir Python paketine dahil olan herhangi bir öğrenici üzerine temelleyebilirsiniz.

> [!NOTE]
> Lütfen betiğin örnek kodundaki açıklamalara daha fazla dikkat ödeyin ve sınıf adı, Yöntemler ve Yöntem imzası dahil olmak üzere betiğinizin gereksinime kesinlikle uyduğundan emin olun. İhlalin özel durumlara yol açacaktır. 

   İki sınıftaki Naive Bayes sınıflandırıcısıdır aşağıdaki örnek kod, popüler *sköğren* paketini kullanır:

   ```Python

   # The script MUST define a class named AzureMLModel.
   # This class MUST at least define the following three methods:
       # __init__: in which self.model must be assigned,
       # train: which trains self.model, the two input arguments must be pandas DataFrame,
       # predict: which generates prediction result, the input argument and the prediction result MUST be pandas DataFrame.
   # The signatures (method names and argument names) of all these methods MUST be exactly the same as the following example.

   # Please do not install extra packages such as "pip install xgboost" in this script,
   # otherwise errors will be raised when reading models in down-stream modules.
   
   import pandas as pd
   from sklearn.naive_bayes import GaussianNB


   class AzureMLModel:
       def __init__(self):
           self.model = GaussianNB()
           self.feature_column_names = list()

       def train(self, df_train, df_label):
           # self.feature_column_names records the column names used for training.
           # It is recommended to set this attribute before training so that the
           # feature columns used in predict and train methods have the same names.
           self.feature_column_names = df_train.columns.tolist()
           self.model.fit(df_train, df_label)

       def predict(self, df):
           # The feature columns used for prediction MUST have the same names as the ones for training.
           # The name of score column ("Scored Labels" in this case) MUST be different from any other columns in input data.
           return pd.DataFrame(
               {'Scored Labels': self.model.predict(df[self.feature_column_names]), 
                'probabilities': self.model.predict_proba(df[self.feature_column_names])[:, 1]}
           )


   ```

2. Model ve **puan modeli** **eğitimi** Için yeni oluşturduğunuz **Python modeli oluşturma** modülünü bağlayın.

3. Modeli değerlendirmeniz gerekiyorsa, bir [Python betik modülünü yürütün](execute-python-script.md) ve Python betiğini düzenleyin.

   Aşağıdaki betik örnek değerlendirme kodudur:

   ```Python


   # The script MUST contain a function named azureml_main
   # which is the entry point for this module.

   # imports up here can be used to 
   import pandas as pd

   # The entry point function MUST have two input arguments:
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

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 