---
title: Oto ml denemeleri 'de korturlama
titleSuffix: Azure Machine Learning
description: Ne kadar uygun Azure Machine Learning teklifleri ve özellik Mühendisliği 'nın otomatikleştirilmiş ML denemeleri 'de nasıl desteklendiğini öğrenin.
author: nibaccam
ms.author: nibaccam
ms.reviewer: nibaccam
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to
ms.date: 05/28/2020
ms.openlocfilehash: a5eb24b5420431a43afa2ffd006ac821f0e907c9
ms.sourcegitcommit: faeabfc2fffc33be7de6e1e93271ae214099517f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88185766"
---
# <a name="featurization-in-automated-machine-learning"></a>Otomatik makine öğreniminde korleştirme

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu kılavuzda şunları öğrenirsiniz:

- Ne kadar uygun ayar Azure Machine Learning sunar.
- [Otomatik makine öğrenimi denemeleri](concept-automated-ml.md)için bu özellikleri özelleştirme.

*Özellik Mühendisliği* , makine öğrenimi (ml) algoritmalarının daha iyi öğrenilmesine yardımcı olan özellikler oluşturmak için verilerin etki alanı bilgisini kullanma işlemidir. Azure Machine Learning, özellik Mühendisliği kolaylaştırmak için veri ölçekleme ve normalleştirme teknikleri uygulanır. Toplu olarak, bu teknikler ve bu özellik Mühendisliği otomatik makine öğrenimi veya *Otomatik ml*, denemeleri içinde *korleştirme* olarak adlandırılır.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, bir oto ml denemesinin nasıl yapılandırılacağı zaten bildiğiniz varsayılmaktadır. Yapılandırma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- İlk olarak bir kod deneyimi için: [Python için Azure Machine Learning SDK 'yı kullanarak OTOMATIK ml denemeleri yapılandırın](how-to-configure-auto-train.md).
- Düşük kod veya kod içermeyen bir deneyim için: [Azure Machine Learning Studio 'yu kullanarak otomatik makine öğrenimi modelleri oluşturun, gözden geçirin ve dağıtın](how-to-use-automated-ml-for-ml-models.md).

## <a name="configure-featurization"></a>Korleştirme yapılandırma

Her otomatik makine öğrenimi denemesinde, [Otomatik ölçeklendirme ve normalleştirme teknikleri](#featurization) verilerinize varsayılan olarak uygulanır. Bu teknikler, farklı ölçeklerde özelliklerle hassas olan *belirli* algoritmalara yardımcı olan, uygun olmayan türlerdir. Ancak, *eksik değerler imputation*, *kodlama*ve *dönüşümler*gibi ek özellikler de etkinleştirebilirsiniz.

> [!NOTE]
> Otomatik makine öğrenimi için adımlar (özellik normalleştirme, eksik verileri işleme veya metni sayısal olarak dönüştürme gibi), temel alınan modelin bir parçası haline gelir. Tahmin için model kullandığınızda, eğitim sırasında uygulanan aynı özellik adımları, giriş verilerinize otomatik olarak uygulanır.

Python SDK ile yapılandırdığınız denemeleri için, korleştirme ayarını etkinleştirebilir veya devre dışı bırakabilir ve denemeniz için kullanılacak özellik adımlarını daha fazla belirtebilirsiniz. Azure Machine Learning Studio kullanıyorsanız, özelliği [etkinleştirme adımlarına](how-to-use-automated-ml-for-ml-models.md#customize-featurization)bakın.

Aşağıdaki tabloda, `featurization` [oto mlconfig sınıfında](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig)için kabul edilen ayarlar gösterilmektedir:

|Korleştirme yapılandırması | Açıklama|
------------- | ------------- |
|`"featurization": 'auto'`| Ön işleme kapsamında, [veri guardı ve korleştirme adımlarının](#featurization) otomatik olarak yapılacağını belirtir. Bu varsayılan ayardır.|
|`"featurization": 'off'`| Korturlama adımlarının otomatik olarak yapılmadığından emin olun.|
|`"featurization":`&nbsp;`'FeaturizationConfig'`| Özelleştirilmiş özellik adımlarının kullanılacağını belirtir. [Korleştirme özelleştirmeyi öğrenin](#customize-featurization).|

<a name="featurization"></a>

## <a name="automatic-featurization"></a>Otomatik olarak korleştirme

Aşağıdaki tabloda verilerinize otomatik olarak uygulanan teknikler özetlenmektedir. Bu teknikler SDK veya Studio kullanılarak yapılandırılan denemeleri için geçerlidir. Bu davranışı devre dışı bırakmak için, `"featurization": 'off'` nesneniz içinde ayarlayın `AutoMLConfig` .

> [!NOTE]
> Bir [onnx modeline](concept-onnx.md), oto tarafından oluşturulan bir modellerinizi dışarı aktarmayı planlıyorsanız, onnx biçiminde yalnızca bir yıldız işareti ("*") ile gösterilen fealeştirme seçenekleri desteklenir. [Modelleri ONNX 'e dönüştürme](concept-automated-ml.md#use-with-onnx)hakkında daha fazla bilgi edinin.

|Korturlama &nbsp; adımları| Açıklama |
| ------------- | ------------- |
|**Yüksek önem düzeyi bırakma veya fark özelliği yok*** |Bu özellikleri eğitim ve doğrulama kümelerinden bırakın. Tüm satırlarda veya yüksek kardinalite (örneğin, karmaları, kimlikler veya GUID 'Ler) ile aynı değere sahip tüm değerleri eksik olan özellikler için geçerlidir.|
|**Impute eksik değerler*** |Sayısal özellikler için, sütundaki değerlerin ortalaması ile ımpute.<br/><br/>Kategorik özellikler için en sık kullanılan değer ile ımpute.|
|**Ek özellikler oluştur*** |Tarih saat özellikleri için: yıl, ay, gün, haftanın günü, yılın günü, üç aylık dönem, yılın haftası, saat, dakika, saniye.<br><br> *Tahmin görevleri için,* bu ek tarih saat özellikleri oluşturulur: ISO yılı, yarı yarı yıl, takvim ayı, dize, hafta günü, haftanın günü, ay günü, yıl günü, yıl/saat (0 ' dan önce ise, 1), saat, günün saati (12 saat)<br/><br/>Metin özellikleri için: tek tek gram, bigram ve trigram temelinde Dönem sıklığı. [BERT ile bunun nasıl yapılacağı](#bert-integration) hakkında daha fazla bilgi edinin.|
|**Dönüştür ve kodla***|Çok sayıda benzersiz değere sahip sayısal özellikleri kategorik Özellikler halinde dönüştürün.<br/><br/>Tek yönlü kodlama, düşük önemlilik kategorik özellikleri için kullanılır. Yüksek kardinalite kategorik özellikler için tek bir Hot-Hash kodlaması kullanılır.|
|**Sözcük katıştırlamaları**|Bir metin kullanımı, metin belirteçlerinin vektörlerini, önceden eğitilen bir model kullanarak tümce vektörlerine dönüştürür. Belgedeki her bir sözcüğün katıştırma vektörü, bir belge özelliği vektörü oluşturmak için geri kalan ile toplanır.|
|**Hedef kodlamalar**|Kategorik özellikler için, bu adım her bir kategoriyi gerileme sorunları için Ortalama bir hedef değerle ve sınıflandırma sorunları için her sınıf için sınıf olasılığa eşler. Sıklık tabanlı ağırlığa ve k katlamalı çapraz doğrulama, seyrek veri kategorilerinin neden olduğu eşlemenin ve gürültü üzerine fazla sığdırmayı azaltmak için uygulanır.|
|**Metin hedefi kodlaması**|Metin girişi için, her bir sınıfın olasılığını oluşturmak için kelimeleri olan bir yığılmış Doğrusal model kullanılır.|
|**Kanıt ağırlığı (WoE)**|, Kategorik sütunların bağıntısı olarak hedef sütuna bir ölçü olarak, WoE hesaplar. WoE, sınıf içi ve sınıf dışı olasılıkların oranının günlüğü olarak hesaplanır. Bu adım, her sınıf için bir sayısal özellik sütunu üretir ve eksik değerler ve aykırı değer işleme gereksinimini ortadan kaldırır.|
|**Küme uzaklığı**|K. a, tüm sayısal sütunlarda kümeleme modeli anlamına gelir. Her bir örneğin her bir kümenin centroıd değerine her birinin uzaklığını içeren *k* yeni özellikler (küme başına yeni bir sayısal özellik) üretir.|

## <a name="data-guardrails"></a>Veri, guardrayları

*Data guardrayları* , verileriniz ile ilgili olası sorunları belirlemenize yardımcı olur (örneğin, eksik değerler veya [sınıf dengesizliği](concept-manage-ml-pitfalls.md#identify-models-with-imbalanced-data)). Ayrıca, geliştirilmiş sonuçlar için düzeltici eylemler almanıza yardımcı olur.

Data guardrayları uygulandı:

- **SDK denemeleri için**: parametreler `"featurization": 'auto'` veya `validation=auto` `AutoMLConfig` nesneniz belirtildiğinde.
- **Studio denemeleri için**: otomatik hale getirme etkinleştirildiğinde.

Denemeniz için veri guardları 'nı gözden geçirebilirsiniz:

- `show_output=True`SDK kullanarak bir deneme gönderdiğinizde ayar yaparak.

- Studio 'da, otomatik ML çalıştırlarınızın **veri guardları** sekmesinde.

### <a name="data-guardrail-states"></a>Veri guardlığı durumları

Data guardrayları üç durumdan birini görüntüler:

|Durum| Açıklama |
|----|---- |
|**Geçiril**| Hiçbir veri sorunu algılanmadı ve sizin için herhangi bir eylem gerekmiyor. |
|**Bitti**| Verilerinize değişiklikler uygulandı. Değişikliklerin beklenen sonuçlarla hizalandığını sağlamak için, oto ml 'nin aldığı düzeltici eylemleri incelemenizi öneririz. |
|**Uyarı**| Bir veri sorunu algılandı, ancak çözümlenemedi. Sorunu düzeltip düzeltmeniz önerilir.|

### <a name="supported-data-guardrails"></a>Desteklenen veri guardrayları

Aşağıdaki tabloda, şu anda desteklenen veri guardları ve deneme hesabınızı gönderdiğinizde görebileceğiniz ilgili durumlar açıklanmaktadır:

Guarddemiryolu|Durum|&nbsp;Tetikleyici için &nbsp; koşul
---|---|---
**Eksik özellik değerleri imputation** |Geçiril <br><br><br> Bitti| Eğitim verilerinizde eksik özellik değeri algılanmadı. [Eksik değerli imputation](https://docs.microsoft.com/azure/machine-learning/how-to-use-automated-ml-for-ml-models#advanced-featurization-options) hakkında daha fazla bilgi edinin. <br><br> Eğitim verilerinizde eksik özellik değerleri algılandı ve bu şekilde karşılaşıldı.
**Yüksek kardinalite özelliği işleme** |Geçiril <br><br><br> Bitti| Girişlerinizin çözümlenmesi ve yüksek kardinalite özelliklerinin algılanması. <br><br> Girdilerde yüksek kardinalite özellikleri algılandı ve işlendi.
**Doğrulama bölünmüş işleme** |Bitti| Doğrulama yapılandırması olarak ayarlandı `'auto'` ve eğitim verileri *20.000 satırdan daha az satır*içeriyordu. <br> Eğitilen modelin her yinelemesi, çapraz doğrulama kullanılarak doğrulanmıştı. [Doğrulama verileri](https://docs.microsoft.com/azure/machine-learning/how-to-configure-auto-train#train-and-validation-data)hakkında daha fazla bilgi edinin. <br><br> Doğrulama yapılandırması olarak ayarlandı `'auto'` ve eğitim verileri *20.000 'den fazla satır*içeriyordu. <br> Giriş verileri, modelin doğrulanması için bir eğitim veri kümesine ve bir doğrulama veri kümesine bölündü.
**Sınıf Dengeleme algılaması** |Geçiril <br><br>Uyarı <br><br>Bitti | Girişlerinizin çözümlenmesi ve tüm sınıfların eğitim verilerinizde dengelenmesi. Her sınıfın veri kümesinde iyi bir temsili varsa, örneklerin sayısı ve oranı ile ölçüldüğü bir veri kümesi dengeli olarak değerlendirilir. <br><br><br> Girdilerde imdengelenmiş sınıflar algılandı. Model sapmalarının giderilmesi için, Dengeleme sorununu düzeltir. [İmdengelenmiş veriler](https://docs.microsoft.com/azure/machine-learning/concept-manage-ml-pitfalls#identify-models-with-imbalanced-data)hakkında daha fazla bilgi edinin.<br><br><br> Girdilerde imledengelenmiş sınıflar algılandı ve bu mantık, dengelemenin uygulanmasını belirledi.
**Bellek sorunları algılama** |Geçiril <br><br><br><br> Bitti |<br> Seçilen değerler (ufuk, öteleme, sıralı pencere) çözümlendi ve olası bellek dışı sorunlar algılandı. Zaman serisi [tahmin yapılandırması](https://docs.microsoft.com/azure/machine-learning/how-to-auto-train-forecast#configure-and-run-experiment)hakkında daha fazla bilgi edinin. <br><br><br>Seçilen değerler (ufuk, öteleme, sıralı pencere) çözümlendi ve bu, denemenizin belleği tükenmesine neden olacak. Gecikme veya sıralı pencere yapılandırması kapatılmış.
**Sıklık algılama** |Geçiril <br><br><br><br> Bitti |<br> Zaman serisi çözümlenmekte ve tüm veri noktaları algılanan sıklığa göre hizalanır. <br> <br> Zaman serisi çözümlendi ve algılanan sıklığa göre hizalanamayan veri noktaları algılandı. Bu veri noktaları, veri kümesinden kaldırılmıştır. [Zaman serisi tahminiyle ilgili veri hazırlığı](https://docs.microsoft.com/azure/machine-learning/how-to-auto-train-forecast#preparing-data)hakkında daha fazla bilgi edinin.

## <a name="customize-featurization"></a>Özelleştirmeleri özelleştirme

Uygun tahminlerde ML modelinizi eğitmek için kullanılan veri ve özelliklerin bir sonucu elde etmek için, korleştirme ayarlarınızı özelleştirebilirsiniz.

Korturleri özelleştirmek için,  `"featurization": FeaturizationConfig` nesnenizin içinde öğesini belirtin `AutoMLConfig` . Denemeniz için Azure Machine Learning Studio kullanıyorsanız [nasıl yapılır makalesine](how-to-use-automated-ml-for-ml-models.md#customize-featurization)bakın. Öngörülebilir görev türleri için korleştirme özelleştirmek üzere, [tahmini nasıl yapılır?](how-to-auto-train-forecast.md#customize-featurization)bölümüne bakın.

Desteklenen özelleştirmeler şunlardır:

|Özelleştirme|Tanım|
|--|--|
|**Sütun amacı güncelleştirmesi**|Belirtilen sütun için, oto algılanan Özellik türünü geçersiz kılın.|
|**Transformatör parametresi güncelleştirmesi** |Belirtilen transformatör için parametreleri güncelleştirin. Şu anda *ımputer* (ortalama, en sık ve ortanca) ve *Hashonehotencoder*'ı desteklemektedir.|
|**Bırakma sütunları** |Bir şekilde bırakılacak sütunları belirler.|
|**Blok dönüştürücüler**| Korleştirme işleminde kullanılacak blok dönüştürücüler belirtir.|

`FeaturizationConfig`API çağrılarını kullanarak nesneyi oluşturun:

```python
featurization_config = FeaturizationConfig()
featurization_config.blocked_transformers = ['LabelEncoder']
featurization_config.drop_columns = ['aspiration', 'stroke']
featurization_config.add_column_purpose('engine-size', 'Numeric')
featurization_config.add_column_purpose('body-style', 'CategoricalHash')
#default strategy mean, add transformer param for for 3 columns
featurization_config.add_transformer_params('Imputer', ['engine-size'], {"strategy": "median"})
featurization_config.add_transformer_params('Imputer', ['city-mpg'], {"strategy": "median"})
featurization_config.add_transformer_params('Imputer', ['bore'], {"strategy": "most_frequent"})
featurization_config.add_transformer_params('HashOneHotEncoder', [], {"number_of_bits": 3})
```

## <a name="featurization-transparency"></a>Korleştirme saydamlığı

Her otomatik ml modeli, otomatik olarak uygulanmış şekilde uygulanır.  Korleştirme otomatik özellik Mühendisliği (ne zaman `"featurization": 'auto'` ) ve ölçekleme ve normalleştirme içerir, bu da seçili algoritmayı ve hiper parametre değerlerini etkiler. , Modelinize uygulanmış olan görünürlüğe sahip olduğunuzdan emin olmak için, oto, farklı yöntemleri destekler.

Bu tahmin örneğini göz önünde bulundurun:

+ Dört giriş özelliği vardır: A (sayısal), B (sayısal), C (sayısal), D (Tarih saat).
+ Sayısal Özellik C, tüm benzersiz değerler içeren bir ID sütunu olduğundan bırakıldı.
+ A ve B sayısal özelliklerinde eksik değerler var ve bu nedenle bu, ortalama olarak düzenlenmiş.
+ Tarih saat özelliği D, 11 farklı mühendislik uygulanmış özelliklere sahiptir.

Bu bilgileri almak için `fitted_model` OTOMATIK ml denemenizin çalıştırıldığı çıktıyı kullanın.

```python
automl_config = AutoMLConfig(…)
automl_run = experiment.submit(automl_config …)
best_run, fitted_model = automl_run.get_output()
```
### <a name="automated-feature-engineering"></a>Otomatik Özellik Mühendisliği 
, `get_engineered_feature_names()` Uygulanan özellik adlarının bir listesini döndürür.

  >[!Note]
  >Görev = ' tahmin ' için ' timeseriestransformer ' kullanın, aksi takdirde ' gerileme ' veya ' sınıflandırma ' görevi için ' datatransformer ' kullanın.

  ```python
  fitted_model.named_steps['timeseriestransformer']. get_engineered_feature_names ()
  ```

Bu liste, uygulanan tüm özellik adlarını içerir. 

  ```
  ['A', 'B', 'A_WASNULL', 'B_WASNULL', 'year', 'half', 'quarter', 'month', 'day', 'hour', 'am_pm', 'hour12', 'wday', 'qday', 'week']
  ```

`get_featurization_summary()`Tüm giriş özelliklerinin korleştirme özetini alır.

  ```python
  fitted_model.named_steps['timeseriestransformer'].get_featurization_summary()
  ```

Çıktı

  ```
  [{'RawFeatureName': 'A',
    'TypeDetected': 'Numeric',
    'Dropped': 'No',
    'EngineeredFeatureCount': 2,
    'Tranformations': ['MeanImputer', 'ImputationMarker']},
   {'RawFeatureName': 'B',
    'TypeDetected': 'Numeric',
    'Dropped': 'No',
    'EngineeredFeatureCount': 2,
    'Tranformations': ['MeanImputer', 'ImputationMarker']},
   {'RawFeatureName': 'C',
    'TypeDetected': 'Numeric',
    'Dropped': 'Yes',
    'EngineeredFeatureCount': 0,
    'Tranformations': []},
   {'RawFeatureName': 'D',
    'TypeDetected': 'DateTime',
    'Dropped': 'No',
    'EngineeredFeatureCount': 11,
    'Tranformations': ['DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime']}]
  ```

   |Çıktı|Tanım|
   |----|--------|
   |RawFeatureName|Girilen veri kümesindeki giriş özelliği/sütun adı.|
   |Typedetesiyonu|Giriş özelliğinin veri türü algılandı.|
   |Bırakılmış|Giriş özelliğinin bırakılıp bırakılmadığını veya kullanıldığını gösterir.|
   |EngineeringFeatureCount|Otomatikleştirilmiş Özellik Mühendisliği dönüştürmeleri aracılığıyla oluşturulan özellik sayısı.|
   |Dönüşümler|Uygulanan özellikler oluşturmak için giriş özelliklerine uygulanan dönüşümlerin listesi.|

### <a name="scaling-and-normalization"></a>Ölçeklendirme ve normalleştirme

Ölçeklendirmeyi/normalleştirmeyi ve seçili algoritmayı hiper parametre değerleriyle anlamak için kullanın `fitted_model.steps` . 

Aşağıdaki örnek çıktı, `fitted_model.steps` Seçilen bir çalıştırma için çalışır:

```
[('RobustScaler', 
  RobustScaler(copy=True, 
  quantile_range=[10, 90], 
  with_centering=True, 
  with_scaling=True)), 

  ('LogisticRegression', 
  LogisticRegression(C=0.18420699693267145, class_weight='balanced', 
  dual=False, 
  fit_intercept=True, 
  intercept_scaling=1, 
  max_iter=100, 
  multi_class='multinomial', 
  n_jobs=1, penalty='l2', 
  random_state=None, 
  solver='newton-cg', 
  tol=0.0001, 
  verbose=0, 
  warm_start=False))
```

Daha fazla bilgi edinmek için bu yardımcı işlevi kullanın: 

```python
from pprint import pprint

def print_model(model, prefix=""):
    for step in model.steps:
        print(prefix + step[0])
        if hasattr(step[1], 'estimators') and hasattr(step[1], 'weights'):
            pprint({'estimators': list(
                e[0] for e in step[1].estimators), 'weights': step[1].weights})
            print()
            for estimator in step[1].estimators:
                print_model(estimator[1], estimator[0] + ' - ')
        else:
            pprint(step[1].get_params())
            print()

print_model(model)
```

Bu yardımcı işlev belirli bir çalıştırma için özel algoritma olarak kullanılan aşağıdaki çıktıyı döndürür `LogisticRegression with RobustScalar` .

```
RobustScaler
{'copy': True,
'quantile_range': [10, 90],
'with_centering': True,
'with_scaling': True}

LogisticRegression
{'C': 0.18420699693267145,
'class_weight': 'balanced',
'dual': False,
'fit_intercept': True,
'intercept_scaling': 1,
'max_iter': 100,
'multi_class': 'multinomial',
'n_jobs': 1,
'penalty': 'l2',
'random_state': None,
'solver': 'newton-cg',
'tol': 0.0001,
'verbose': 0,
'warm_start': False}
```

### <a name="predict-class-probability"></a>Sınıf olasılığını tahmin etme

Otomatikleştirilmiş ML kullanılarak oluşturulan modellerin, açık kaynaklı kaynak sınıfından işlevselliği yansıtan sarmalayıcı nesneleri vardır. Otomatik ML tarafından döndürülen çoğu sınıflandırma modeli sarmalayıcı nesneleri, `predict_proba()` özelliklerinizin (X Values) dizi benzeri veya seyrek matris veri örneğini kabul eden işlevini uygular ve her bir örnekteki n boyutlu bir dizi ve ilgili sınıf olasılığını döndürür.

Yukarıdaki aynı çağrıları kullanarak en iyi çalıştırma ve bağlı modeli elde ettiğiniz varsayılarak, `predict_proba()` `X_test` model türüne bağlı olarak uygun biçimde bir örnek sağlayarak doğrudan monte edilen modelden çağrı yapabilirsiniz.

```python
best_run, fitted_model = automl_run.get_output()
class_prob = fitted_model.predict_proba(X_test)
```

Temeldeki model `predict_proba()` işlevi desteklemiyorsa veya biçim yanlış ise, model sınıfına özgü özel durum oluşturulur. Bu işlevin farklı model türleri için nasıl uygulandığı hakkında örnekler için bkz. [Randomforestsınıflandırıcı](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html#sklearn.ensemble.RandomForestClassifier.predict_proba) ve [xgboost](https://xgboost.readthedocs.io/en/latest/python/python_api.html) başvuru belgeleri.

## <a name="bert-integration"></a>BERT tümleştirmesi

[Bert](https://techcommunity.microsoft.com/t5/azure-ai/how-bert-is-integrated-into-azure-automated-machine-learning/ba-p/1194657) , oto ml 'nin korleştirme katmanında kullanılır. Bu katmanda, bir sütun, zaman damgaları veya basit sayılar gibi boş metin veya diğer veri türleri içeriyorsa, uygun şekilde uygulanır.

BERT için model ince ayarlanır ve Kullanıcı tarafından sağlanmış Etiketler kullanılarak eğitilmiş olur. Buradan, belge katıştırılır, zaman damgası tabanlı özellikler, haftanın günü gibi diğer özelliklerle birlikte çıktı olarak oluşur. 


### <a name="bert-steps"></a>BERT adımları

BERT 'yi çağırmak için `enable_dnn: True` automl_settings ayarlamanız ve bır GPU işlem (örn. `vm_size = "STANDARD_NC6"` veya daha yüksek bir GPU) kullanmanız gerekir. Bir CPU işlemi kullanılıyorsa, BERT 'in yerine, JML 'nin, bilmi DNN featurizer 'ı kullanmasına izin verilir.

Oto ml, BERT için aşağıdaki adımları alır. 

1. **Tüm metin sütunlarının ön işleme ve simgeleştirme**. Örneğin, "StringCast" transformatörü, son modelin korturleştirme özetinde bulunabilir. [Bu not defterinde](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-text-dnn/auto-ml-classification-text-dnn.ipynb)modelin korleştirme özetinin nasıl üretileceğini gösteren bir örnek bulabilirsiniz.

2. Tüm metin sütunlarını, bu nedenle son modelde **tek bir metin sütununda birleştirin** `StringConcatTransformer` . 

    BERT uygulamamız, bir eğitim örneğinin toplam metin uzunluğunu 128 belirteç olarak sınırlandırır. Yani, art arda tüm metin sütunlarının en çok 128 belirteç uzunluğunda olması gerekir. Birden çok sütun varsa, bu koşulun karşılanması için her sütun ayıklanmalıdır. Aksi takdirde, >128 belirteçleri BERT 'nin Simgeleştirici katmanının, bu girişi 128 belirteçlere kesen birleştirilmiş sütunlar için.

3. **Özellik tatbası kapsamında, oto ml, verilerin bir örneği üzerinde BERT 'yi taban çizgisine (sözcük çantası özellikleri) karşı karşılaştırır.** Bu karşılaştırma, BERT 'in doğruluk iyileştirmeleri verip veremeyeceğini belirler. BERT, temelden daha iyi sonuç taşıyorsa, oto ve tüm veriler için metin kullanımı için BERT 'i kullanır. Bu durumda, ' yi `PretrainedTextDNNTransformer` son modelde görürsünüz.

BERT genellikle diğer korbinlardan daha uzun çalışır. Daha iyi performans için, RDMA özellikleri için "STANDARD_NC24r" veya "STANDARD_NC24rs_V3" kullanmanızı öneririz. 

Oto, varsa, BERT eğitimini birden çok düğüm arasında dağıtır (en fazla sekiz düğüm). Bu `AutoMLConfig` , `max_concurrent_iterations` parametreyi 1 ' den yüksek olacak şekilde ayarlayarak nesneniz üzerinde yapılabilir. 
### <a name="supported-languages"></a>Desteklenen diller

JML Şu anda 100 dil etrafında destek ve veri kümesinin diline bağlı olarak, oto ml uygun BERT modelini seçer. Almanya verileri için Almanca BERT modelini kullanıyoruz. Ingilizce için, Ingilizce BERT modelini kullanıyoruz. Diğer tüm diller için, çok dilli BERT modelini kullanırız.

Aşağıdaki kodda, veri kümesi dili olarak belirtildiği için Alman BERT modeli tetiklenir. Bu, `deu` [ISO sınıflandırmasına](https://iso639-3.sil.org/code/deu)göre Alman için üç harfli dil kodudur:

```python
from azureml.automl.core.featurization import FeaturizationConfig

featurization_config = FeaturizationConfig(dataset_language='deu')

automl_settings = {
    "experiment_timeout_minutes": 120,
    "primary_metric": 'accuracy', 
# All other settings you want to use 
    "featurization": featurization_config,
    
  "enable_dnn": True, # This enables BERT DNN featurizer
    "enable_voting_ensemble": False,
    "enable_stack_ensemble": False
}
```

## <a name="next-steps"></a>Sonraki adımlar

* Otomatik ML denemeleri 'nizi ayarlamayı öğrenin:

    * İlk olarak bir kod deneyimi için: [Azure MACHINE LEARNING SDK kullanarak OTOMATIK ml denemeleri yapılandırın](how-to-configure-auto-train.md).
    * Düşük kod veya kod içermeyen bir deneyim için: [Azure Machine Learning Studio 'da OTOMATIK ml denemeleri oluşturun](how-to-use-automated-ml-for-ml-models.md).

* [Bir modelin nasıl ve nereye dağıtılacağı](how-to-deploy-and-where.md)hakkında daha fazla bilgi edinin.

* [Otomatik makine öğrenimi kullanarak bir gerileme modelini eğitme](tutorial-auto-train-models.md) veya [uzak bir kaynakta otomatik makine öğrenimi kullanarak eğitme](how-to-auto-train-remote.md)hakkında daha fazla bilgi edinin.
