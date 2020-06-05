---
title: Oto ml denemeleri 'de korturlama
titleSuffix: Azure Machine Learning
description: Hangi özelliklerin Azure Machine Learning tekliflerini ve özellik Mühendisliği 'nın otomatikleştirilmiş ml denemeleri 'de nasıl desteklendiğini öğrenin.
author: nibaccam
ms.author: nibaccam
ms.reviewer: nibaccam
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.date: 05/28/2020
ms.custom: seodec18
ms.openlocfilehash: c81e0a71b23e865ca2938f5fbf3c73cdb5c3aeb1
ms.sourcegitcommit: b55d1d1e336c1bcd1c1a71695b2fd0ca62f9d625
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84434298"
---
# <a name="featurization-with-automated-machine-learning"></a>Otomatik makine öğrenimi ile korturlama

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu kılavuzda, ne kadar uygun ayarların sunulmakta olduğunu ve [otomatik makine öğrenimi denemeleri](concept-automated-ml.md)için nasıl özelleştireceğinizi öğrenin.

Özellik Mühendisliği, ML algoritmalarının daha iyi öğrenilmesine yardımcı olan özellikler oluşturmak için verilerin etki alanı bilgisini kullanma işlemidir. Azure Machine Learning, özellik Mühendisliği kolaylaştırmak için veri ölçekleme ve normalleştirme teknikleri uygulanır. Toplu olarak, bu teknikler ve özellik Mühendisliği otomatik makine öğrenimi, otomatik ml, denemeleri 'de korleştirme olarak adlandırılır.

Bu makalede, bir oto ml denemesinin nasıl yapılandırılacağı hakkında zaten bilgi sahibi olduğunuz varsayılır. Ayrıntılar için aşağıdaki makalelere bakın:

* Kod ilk deneyimi için: [Python SDK ile OTOMATIK ml denemeleri yapılandırma](how-to-configure-auto-train.md).
* Düşük/olmayan bir kod deneyimi için: [Azure Machine Learning Studio ile otomatik makine öğrenimi modelleri oluşturun, gözden geçirin ve dağıtın](how-to-use-automated-ml-for-ml-models.md)

## <a name="configure-featurization"></a>Korleştirme yapılandırma

Her otomatik makine öğrenimi denemesinde, [Otomatik ölçeklendirme ve normalleştirme teknikleri](#featurization) verilerinize varsayılan olarak uygulanır. Bu ölçeklendirme ve normalleştirme teknikleri, farklı ölçeklardaki özelliklerle hassas olan *belirli* algoritmalara yardımcı olan, uygun olmayan türlerdir. Ancak, **eksik değerler imputation, kodlama** ve **dönüşümler**gibi ek özellikler de sağlayabilirsiniz.

> [!NOTE]
> Otomatik makine öğrenimi adımları (özellik normalleştirme, eksik verileri işleme, metni sayısal olarak dönüştürme, vb.) temel modelin bir parçası haline gelir. Tahmin için model kullanılırken, eğitim sırasında uygulanan aynı özellik adımları, giriş verilerinize otomatik olarak uygulanır.

SDK ile yapılandırılan denemeleri için ayarı etkinleştirebilir/devre dışı bırakabilir `featurization` ve denemeniz için kullanılması gereken özellik adımlarını daha fazla belirtebilirsiniz. Azure Machine Learning Studio kullanıyorsanız, bkz. [Bu adımlarla](how-to-use-automated-ml-for-ml-models.md#customize-featurization)korleştirme özelliğini etkinleştirme.

Aşağıdaki tabloda, `featurization` [oto mlconfig sınıfında](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig)için kabul edilen ayarlar gösterilmektedir. 

|Korleştirme yapılandırması | Description|
------------- | ------------- |
|**`"featurization": 'auto'`**| Ön işleme 'nin bir parçası olarak, [veri guardı ve korleştirme adımlarının](#featurization) otomatik olarak gerçekleştirileceğini belirtir. **Varsayılan ayar**.|
|**`"featurization": 'off'`**| Korleştirme adımlarının otomatik olarak yapılmaması gerektiğini gösterir.|
|**`"featurization":`&nbsp;`'FeaturizationConfig'`**| Özelleştirilmiş basamak kullanılması gerektiğini gösterir. [Korleştirme özelleştirmeyi öğrenin](#customize-featurization).|

<a name="featurization"></a>

## <a name="automatic-featurization"></a>Otomatik olarak korleştirme

Aşağıdaki tablo, verilerinize otomatik olarak uygulanan teknikleri özetler. Bu, SDK veya Studio aracılığıyla yapılandırılan denemeleri için gerçekleşir. Bu davranışı devre dışı bırakmak için, `"featurization": 'off'` nesneniz içinde ayarlayın `AutoMLConfig` .

> [!NOTE]
> Auto ML tarafından oluşturulan modellerinizi bir [onnx modeline](concept-onnx.md)dışarı aktarmayı planlıyorsanız, onnx biçiminde yalnızca bir * ile gösterilen featurleştirme seçenekleri desteklenir. [Modelleri ONNX 'e dönüştürme](concept-automated-ml.md#use-with-onnx)hakkında daha fazla bilgi edinin. 

|Korturlama &nbsp; adımları| Description |
| ------------- | ------------- |
|**Yüksek önem düzeyi bırakma veya fark özelliği yok*** |Tüm değerleri eksik olan özellikler de dahil olmak üzere eğitim ve doğrulama kümelerinden bunları bırakın, tüm satırlarda veya yüksek kardinalite (örneğin, karma değerler, kimlikler veya GUID 'Ler) ile aynı değeri kullanın.|
|**Impute eksik değerler*** |Sayısal özellikler için sütunundaki değerleri ortalama olan ımpute.<br/><br/>Kategorik özellikler için en sık değer içeren ımpute.|
|**Ek özellikler oluştur*** |Tarih saat özellikleri için: yıl, ay, gün, haftanın günü, yılın günü, üç aylık dönem, yılın haftası, saat, dakika, saniye.<br/><br/>Metin özellikleri için: tekli gram, Çift gram ve üçlü karakter-gram temelinde Dönem sıklığı.|
|**Dönüştür ve kodla***|Birkaç benzersiz değer içeren sayısal özellikler kategorik özelliklere dönüştürülür.<br/><br/>Düşük kardinalite kategorik için tek bir etkin kodlama gerçekleştirilir; yüksek kardinalite, tek bir sıcak karma kodlama için.|
|**Sözcük katıştırlamaları**|Metin belirteçlerinin vektörlerini, önceden eğitilen bir model kullanarak tümce vektörlerine dönüştüren metin korleştirici. Belge özellik vektörü oluşturmak için bir belgedeki her bir sözcüğün katıştırma vektörü birlikte toplanır.|
|**Hedef kodlamalar**|Kategorik özellikler için, her bir kategoriyi gerileme sorunları için Ortalama hedef değeriyle ve sınıflandırma sorunları için her bir sınıfın sınıf olasılığa eşler. Sıklık tabanlı ağırlığa ve k katlamalı çapraz doğrulama, seyrek veri kategorilerinin neden olduğu eşlemenin ve gürültü üzerine gitmek için geçerlidir.|
|**Metin hedefi kodlaması**|Metin girişi için, her bir sınıfın olasılığını oluşturmak için kelimeleri olan bir yığılmış Doğrusal model kullanılır.|
|**Kanıt ağırlığı (WoE)**|, Kategorik sütunların bağıntısı olarak hedef sütuna bir ölçü olarak, WoE hesaplar. Sınıf içi ve sınıf dışı olasılıkların oranının günlüğü olarak hesaplanır. Bu adım, her sınıf için bir sayısal özellik sütunu verir ve eksik değerler ve aykırı değer işleme gereksinimini ortadan kaldırır.|
|**Küme uzaklığı**|K. a, tüm sayısal sütunlarda kümeleme modeli anlamına gelir.  Her bir örneğin her bir kümenin centroıd değerine her bir örnek arasındaki mesafeyi içeren, her küme için yeni bir sayısal özellik olan yeni özellik çıkışları.|

## <a name="data-guardrails"></a>Veri, guardrayları

Data guardrayları, verileriniz ile ilgili olası sorunları belirlemenize yardımcı olur (örneğin, eksik değerler, [sınıf dengesizliği](concept-manage-ml-pitfalls.md#identify-models-with-imbalanced-data)) ve geliştirilmiş sonuçlar için düzeltici eylemler elde etmenize yardımcı olur. 

Data guardrayları uygulandı 

* **SDK denemeleri için**, ya da `"featurization": 'auto'` `validation=auto` nesneniz içinde belirtildiğinde `AutoMLConfig` .
* **Studio denemeleri Için** *Otomatik* özellik etkinleştirildiğinde.  

Denemenizin ilgili olduğu verileri gözden geçirebilirsiniz

* `show_output=True`Python SDK ile deneme gönderme sırasında ayar yaparak.

* Studio 'da otomatik ML çalıştırın **veri guardları** sekmesinde.

### <a name="data-guardrail-states"></a>Veri guardlığı durumları

Data guardrayları şu üç durumdan birini görüntüler: **başarılı**, **bitti**veya **Uyarı**.

|Eyalet| Açıklama |
|----|---- |
|**Geçiril**| Hiçbir veri sorunu algılanmadı ve Kullanıcı eylemi gerekli değildir. |
|**Bitti**| Verilerinize değişiklikler uygulandı. Kullanıcıların, değişikliklerin beklenen sonuçlarla hizalandığını garantilemek için otomatik ML 'nin düzeltici eylemleri gözden geçirmesini öneririz. |
|**Uyarı**| Düzeltmelere neden olan bir veri sorunu algılandı. Kullanıcıları sorunu gözden geçirmenizi ve düzeltmesini öneririz.| 

Aşağıdaki tabloda, şu anda desteklenmekte olan veriler ve kullanıcıların denedikleri zaman içinde karşılaşabileceğiniz ilgili durumlar açıklanmaktadır.

Guarddemiryolu|Durum|&nbsp;Tetikleyici için &nbsp; koşul
---|---|---
**Eksik özellik değerleri imputation** |*Geçiril* <br><br><br> *Bitti*| Eğitim verilerinizde eksik özellik değeri algılanmadı. [Eksik imputation değeri](https://docs.microsoft.com/azure/machine-learning/how-to-use-automated-ml-for-ml-models#advanced-featurization-options) hakkında daha fazla bilgi edinin. <br><br> Eğitim verilerinizde eksik özellik değerleri algılandı ve genişletilmiş.
**Yüksek kardinalite özelliği işleme** |*Geçiril* <br><br><br> *Bitti*| Girişlerinizin çözümlenmesi ve yüksek kardinalite özelliklerinin saptanmamış olması. [Yüksek kardinalite Özellik algılaması](#automatic-featurization) hakkında daha fazla bilgi edinin. <br><br> Girdilerde yüksek kardinalite özellikleri algılandı ve işlendi.
**Doğrulama bölünmüş işleme** |*Bitti*| Doğrulama yapılandırması ' Auto ' olarak ayarlandı ve eğitim verileri **20.000 satırdan daha az satır**içeriyordu. <br> Eğitilen modelin her yinelemesi çapraz doğrulama aracılığıyla doğrulanmıştı. [Doğrulama verileri](https://docs.microsoft.com/azure/machine-learning/how-to-configure-auto-train#train-and-validation-data) hakkında daha fazla bilgi edinin. <br><br> Doğrulama yapılandırması ' Auto ' olarak ayarlandı ve eğitim verileri **20.000 satırdan fazla satır**içeriyordu. <br> Giriş verileri, modelin doğrulanması için bir eğitim veri kümesine ve bir doğrulama veri kümesine bölündü.
**Sınıf Dengeleme algılaması** |*Geçiril* <br><br><br><br><br> *Uyarı* | Girişlerinizin çözümlenmesi ve tüm sınıfların eğitim verilerinizde dengelenmesi. Her sınıfın veri kümesinde iyi bir temsili varsa, örneklerin sayısı ve oranı ile ölçüldüğü bir veri kümesi dengeli olarak değerlendirilir. <br><br><br> Girdilerde imdengelenmiş sınıflar algılandı. Model sapmalarının giderilmesi için, Dengeleme sorununu düzeltir. [İmdengelenmiş veriler](https://docs.microsoft.com/azure/machine-learning/concept-manage-ml-pitfalls#identify-models-with-imbalanced-data) hakkında daha fazla bilgi edinin.
**Bellek sorunları algılama** |*Geçiril* <br><br><br><br> *Bitti* |<br> Seçilen {ufuk, öteleme, hareketli pencere} değerleri çözümlendi ve olası bellek dışı sorunlar algılandı. Zaman serisi [tahmin yapılandırması](https://docs.microsoft.com/azure/machine-learning/how-to-auto-train-forecast#configure-and-run-experiment) hakkında daha fazla bilgi edinin. <br><br><br>Seçilen {ufuk, öteleme, hareketli pencere} değerleri çözümlendi ve bu, denemenizin belleği tükenmesine neden olacak. Öteleme veya sıralı pencere yapılandırması kapatılmış.
**Sıklık algılama** |*Geçiril* <br><br><br><br> *Bitti* |<br> Zaman serisi çözümlendi ve tüm veri noktaları algılanan sıklığa göre hizalanır. <br> <br> Zaman serisi çözümlendi ve algılanan sıklığa göre hizalanmayan veri noktaları algılandı. Bu veri noktaları, veri kümesinden kaldırılmıştır. [Zaman serisi tahminiyle ilgili veri hazırlığı](https://docs.microsoft.com/azure/machine-learning/how-to-auto-train-forecast#preparing-data) hakkında daha fazla bilgi edinin.

## <a name="customize-featurization"></a>Özelleştirmeleri özelleştirme

Uygun tahminlerde ML modelinizi eğitmek için kullanılan veri ve özelliklerin de yer aldığından emin olmak için, korleştirme ayarlarınızı özelleştirebilirsiniz. 

Korturleri özelleştirmek için,  `"featurization": FeaturizationConfig` nesnenizin içinde öğesini belirtin `AutoMLConfig` . Denemenizin Azure Machine Learning Studio kullanıyorsanız bkz. [nasıl yapılır](how-to-use-automated-ml-for-ml-models.md#customize-featurization).

Desteklenen özelleştirme şunları içerir:

|Özelleştirme|Tanım|
|--|--|
|**Sütun amacı güncelleştirmesi**|Belirtilen sütun için özellik türünü geçersiz kıl.|
|**Transformatör parametresi güncelleştirmesi** |Belirtilen transformatör için parametreleri güncelleştirin. Şu anda ımputer (ortalama, en sık & ortancası) ve HashOneHotEncoder 'ı desteklemektedir.|
|**Bırakma sütunları** |Korturdan bırakılacak sütunlar.|
|**Blok dönüştürücüler**| Korleştirme işleminde kullanılacak dönüştürücüler bloğunu engelleyin.|

API çağrılarını kullanarak FeaturizationConfig nesnesini oluşturun:
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
## <a name="next-steps"></a>Sonraki adımlar

* Otomatikleştirilmiş ML denemeleri ayarlamayı öğrenin,

    * Kod deneyimi müşterileri için: [Azure MACHINE LEARNING SDK ile OTOMATIK ml denemeleri yapılandırın](how-to-configure-auto-train.md).
    * Düşük/kod deneyimi için müşteriler: [Azure Machine Learning Studio 'da otomatik makine öğrenimi denemeleri oluşturun](how-to-use-automated-ml-for-ml-models.md).

* [Bir modelin nasıl ve nereye dağıtılacağı](how-to-deploy-and-where.md)hakkında daha fazla bilgi edinin.

* [Otomatik makine öğrenimi ile regresyon modelini eğitme](tutorial-auto-train-models.md) veya [uzak bir kaynakta otomatik makine öğrenimi kullanarak nasıl eğitme](how-to-auto-train-remote.md)yapılacağı hakkında daha fazla bilgi edinin.
