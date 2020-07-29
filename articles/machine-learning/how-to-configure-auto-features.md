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
ms.openlocfilehash: b01d6c36b31ef4f03522d03ca327439cfa31be8d
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87373751"
---
# <a name="featurization-in-automated-machine-learning"></a>Otomatik makine öğreniminde korleştirme

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu kılavuzda şunları öğreneceksiniz:

- Ne kadar uygun ayar Azure Machine Learning sunar.
- [Otomatik makine öğrenimi denemeleri](concept-automated-ml.md)için bu özellikleri özelleştirme.

*Özellik Mühendisliği* , makine öğrenimi (ml) algoritmalarının daha iyi öğrenilmesine yardımcı olan özellikler oluşturmak için verilerin etki alanı bilgisini kullanma işlemidir. Azure Machine Learning, özellik Mühendisliği kolaylaştırmak için veri ölçekleme ve normalleştirme teknikleri uygulanır. Toplu olarak, bu teknikler ve bu özellik Mühendisliği otomatik makine öğrenimi veya *Otomatik ml*, denemeleri içinde *korleştirme* olarak adlandırılır.

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
|**Ek özellikler oluştur*** |Tarih saat özellikleri için: yıl, ay, gün, haftanın günü, yılın günü, üç aylık dönem, yılın haftası, saat, dakika, saniye.<br/><br/>Metin özellikleri için: tek tek gram, bigram ve trigram temelinde Dönem sıklığı. [BERT ile bunun nasıl yapılacağı](#bert-integration) hakkında daha fazla bilgi edinin.|
|**Dönüştür ve kodla***|Çok sayıda benzersiz değere sahip sayısal özellikleri kategorik Özellikler halinde dönüştürün.<br/><br/>Tek yönlü kodlama, düşük önemlilik kategorik özellikleri için kullanılır. Yüksek kardinalite kategorik özellikler için tek bir Hot-Hash kodlaması kullanılır.|
|**Sözcük katıştırlamaları**|Bir metin özelliği, önceden eğitilen bir model kullanarak metin belirteçlerinin vektörlerini tümce vektörlerine dönüştürür. Belgedeki her bir sözcüğün katıştırma vektörü, bir belge özelliği vektörü oluşturmak için geri kalan ile toplanır.|
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
**Sınıf Dengeleme algılaması** |Geçiril <br><br><br><br><br> Uyarı | Girişlerinizin çözümlenmesi ve tüm sınıfların eğitim verilerinizde dengelenmesi. Her sınıfın veri kümesinde iyi bir temsili varsa, örneklerin sayısı ve oranı ile ölçüldüğü bir veri kümesi dengeli olarak değerlendirilir. <br><br><br> Girdilerde imdengelenmiş sınıflar algılandı. Model sapmalarının giderilmesi için, Dengeleme sorununu düzeltir. [İmdengelenmiş veriler](https://docs.microsoft.com/azure/machine-learning/concept-manage-ml-pitfalls#identify-models-with-imbalanced-data)hakkında daha fazla bilgi edinin.
**Bellek sorunları algılama** |Geçiril <br><br><br><br> Bitti |<br> Seçilen değerler (ufuk, öteleme, sıralı pencere) çözümlendi ve olası bellek dışı sorunlar algılandı. Zaman serisi [tahmin yapılandırması](https://docs.microsoft.com/azure/machine-learning/how-to-auto-train-forecast#configure-and-run-experiment)hakkında daha fazla bilgi edinin. <br><br><br>Seçilen değerler (ufuk, öteleme, sıralı pencere) çözümlendi ve bu, denemenizin belleği tükenmesine neden olacak. Gecikme veya sıralı pencere yapılandırması kapatılmış.
**Sıklık algılama** |Geçiril <br><br><br><br> Bitti |<br> Zaman serisi çözümlenmekte ve tüm veri noktaları algılanan sıklığa göre hizalanır. <br> <br> Zaman serisi çözümlendi ve algılanan sıklığa göre hizalanamayan veri noktaları algılandı. Bu veri noktaları, veri kümesinden kaldırılmıştır. [Zaman serisi tahminiyle ilgili veri hazırlığı](https://docs.microsoft.com/azure/machine-learning/how-to-auto-train-forecast#preparing-data)hakkında daha fazla bilgi edinin.

## <a name="customize-featurization"></a>Özelleştirmeleri özelleştirme

Uygun tahminlerde ML modelinizi eğitmek için kullanılan veri ve özelliklerin bir sonucu elde etmek için, korleştirme ayarlarınızı özelleştirebilirsiniz.

Korturleri özelleştirmek için,  `"featurization": FeaturizationConfig` nesnenizin içinde öğesini belirtin `AutoMLConfig` . Denemeniz için Azure Machine Learning Studio kullanıyorsanız [nasıl yapılır makalesine](how-to-use-automated-ml-for-ml-models.md#customize-featurization)bakın. Öngörülebilir görev türleri için korleştirme özelleştirmek üzere, [tahmini nasıl yapılır?](how-to-auto-train-forecast.md#customize-featurization)bölümüne bakın.

Desteklenen özelleştirmeler şunlardır:

|Özelleştirme|Tanım|
|--|--|
|**Sütun amacı güncelleştirmesi**|Belirtilen sütun için otomatik algılanan Özellik türünü geçersiz kılın.|
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

## <a name="bert-integration"></a>BERT tümleştirmesi 
[Bert](https://techcommunity.microsoft.com/t5/azure-ai/how-bert-is-integrated-into-azure-automated-machine-learning/ba-p/1194657) , otomatik ml 'nin korleştirme katmanında kullanılır. Bu katmanda, bir sütunun zaman damgaları veya basit sayılar gibi boş metin veya diğer tür verileri içerip içermediğinizi tespit ettik ve buna uygun şekilde koruyoruz. BERT için Kullanıcı tarafından sağlanmış etiketlerin kullanılmasıyla model üzerinde ince ayar yapın/eğeceğiz, sonra zaman damgası tabanlı Özellikler (örneğin, haftanın günü) veya birçok tipik veri kümesinin bulunduğu sayılar gibi diğer özelliklerle ilgili özellikler olarak, belge katıştırılamaları (yani, özel [CLS] belirteciyle ilişkili son gizli durum). 

BERT 'yi etkinleştirmek için, eğitim için GPU işlem kullanmanız gerekir. Bir CPU işlemi kullanılıyorsa, BERT 'in yerine, tabtdnn featurizer 'ı etkinleştirir. BERT 'yi çağırmak için automl_settings içinde "enable_dnn: true" ayarlamanız ve GPU işlem (örn. vm_size = "STANDARD_NC6" veya daha yüksek bir GPU) kullanmanız gerekir. Lütfen [bir örnek için bu not defterine](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-text-dnn/auto-ml-classification-text-dnn.ipynb)bakın.

Aşağıdaki adımlar, BERT için aşağıdaki adımları alır (lütfen bu öğelerin gerçekleşmesi için automl_settings "enable_dnn: true" ayarlamanız gerektiğini unutmayın):

1. Tüm metin sütunlarının simgeleştirme özelliği de dahil olmak üzere ön işleme (son modelin korleştirme özetinde "StringCast" transformatörü görürsünüz. Bu şekilde, model oluşturma özetinin yöntemi kullanılarak nasıl oluşturulacağı hakkında bir örnek görmek için lütfen [Bu Not defterini](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-text-dnn/auto-ml-classification-text-dnn.ipynb) ziyaret edin `get_featurization_summary()` .

```python
text_transformations_used = []
for column_group in fitted_model.named_steps['datatransformer'].get_featurization_summary():
    text_transformations_used.extend(column_group['Transformations'])
text_transformations_used
```

2. Tüm metin sütunlarını tek bir metin sütununda birleştirir, bu nedenle son modelde "StringConcatTransformer" görüntülenir. 

> [!NOTE]
> BERT uygulamamız, bir eğitim örneğinin toplam metin uzunluğunu 128 belirteç olarak sınırlandırır. Yani, art arda tüm metin sütunlarının en çok 128 belirteç uzunluğunda olması gerekir. İdeal olarak, birden çok sütun varsa, bu koşulun karşılanması için her sütun ayıklanmalıdır. Örneğin, verilerde iki metin sütunu varsa, verileri oto ml 'ye yazdırmadan önce her iki metin sütununun de her biri 64 belirtece (her iki sütunun de en son birleştirilmiş metin sütununda aynı şekilde temsil olmasını istediğinizi varsayarak) ayıklanmalıdır. >128 belirteçlerinin bir birleştirilmiş sütunları için, Bert 'in belirteç ayırıcı katmanı bu girişi 128 belirteçlere keser.

3. Özellik üst kısmında, oto ml, verilerin bir örneği üzerinde BERT 'yi (kelimeler özelliği ve önceden eğitilen kelime eklenebilir) ile karşılaştırır ve BERT 'in doğruluk iyileştirmeleri verip veremeyeceğini belirler. BERT 'in temelden daha iyi çalıştığını belirlerse, oto ve en iyi performans stratejisi olarak metin kullanımı için BERT 'yi kullanır ve verilerin tamamını kordoğru bir şekilde devam eder. Bu durumda, son modelde "Pretraınedtextdnntransformer" metnini görürsünüz.

JML Şu anda 100 dil etrafında destek ve veri kümesinin diline bağlı olarak, oto ml uygun BERT modelini seçer. Almanya verileri için Almanca BERT modelini kullanıyoruz. Ingilizce için, Ingilizce BERT modelini kullanıyoruz. Diğer tüm diller için, çok dilli BERT modelini kullanırız.

Aşağıdaki kodda, veri kümesi dili ' DEU ' olarak belirtildiğinden ve [ISO sınıflandırmasına](https://iso639-3.sil.org/code/hbs)göre Almanca için 3 harfli dil kodu olan, Almanya Bert modeli tetiklenir:

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
