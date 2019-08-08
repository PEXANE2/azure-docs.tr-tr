---
title: Verileri keşfet ve dönüştürme (veri kümesi sınıfı)
titleSuffix: Azure Machine Learning service
description: Özet istatistikleri kullanarak verileri araştırma ve veri temizleme, dönüştürme ve özellik mühendisliği aracılığıyla veri hazırlama
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 05/23/2019
ms.openlocfilehash: dbdd088e5a78a4f78eec27b5ee74856c6aecc209
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68847915"
---
# <a name="explore-and-prepare-data-with-the-dataset-class-preview"></a>Veri kümesi sınıfıyla verileri keşfet ve hazırlama (Önizleme)

[Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)'da azureml-veri kümesi paketiyle verileri nasıl araştırıp hazırlayacağınızı öğrenin. Veri [kümesi](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py) sınıfı (Önizleme): örnekleme, Özet istatistikleri ve akıllı dönüşümler gibi işlevleri sağlayarak verilerinizi araştırmanızı ve hazırlamanızı sağlar. Dönüştürme adımları, farklı şemalardan oluşan birden fazla büyük dosyayı yüksek oranda ölçeklenebilir bir şekilde işleme özelliğiyle [veri kümesi tanımlarına](how-to-manage-dataset-definitions.md) kaydedilir.

> [!Important]
> Bazı veri kümesi sınıflarının (Önizleme), [azureml-dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py) PAKETI (GA) üzerinde bağımlılıkları vardır. Dönüştürme işlevleri doğrudan, gaed [veri hazırlama işlevleriyle](how-to-transform-data.md)yapılaken, yeni bir çözüm oluşturuyorsanız bu makalede açıklanan veri kümesi paketi sarmalayıcılarını öneririz. Azure Machine Learning veri kümeleri (Önizleme) yalnızca verilerinizi dönüştürmeye, ayrıca [veri anlık görüntü](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_snapshot.datasetsnapshot?view=azure-ml-py) ve [sürümlü veri kümesi tanımlarını](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset?view=azure-ml-py)depolamanıza olanak tanır. Veri kümeleri, veri hazırlama SDK 'sının bir sonraki sürümüdür ve AI çözümlerinde veri kümelerini yönetmek için genişletilmiş işlevler sunar.

## <a name="prerequisites"></a>Önkoşullar

Verilerinizi incelemek ve hazırlamak için şunları yapmanız gerekir:

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning Service 'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

* Bir Azure Machine Learning hizmeti çalışma alanı. Bkz. [Azure Machine Learning hizmet çalışma alanı oluşturma](how-to-manage-workspace.md).

* Azureml için Azure Machine Learning SDK (sürüm 1.0.21 veya üzeri), bu, azureml veri kümesi paketini içerir. SDK 'nın en son sürümünü yüklemek veya güncelleştirmek için bkz. [SDK 'Yı yüklemek veya güncelleştirmek](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

* Azure Machine Learning Data Prep SDK 'Sı. En son sürümü yüklemek veya güncelleştirmek için bkz. [Data Prep SDK 'Sını yüklemek veya güncelleştirmek](https://docs.microsoft.com/python/api/overview/azure/dataprep/intro?view=azure-dataprep-py).

* Örnek dosyaları aşağıdaki örneklerle birlikte indirin: [suc. csv](https://dprepdata.blob.core.windows.net/dataset-sample-files/crime.csv) ve [City. JSON](https://dprepdata.blob.core.windows.net/dataset-sample-files/city.json).

## <a name="sampling"></a>Örnekleme

Veri mimarinizi ve içeriğinizi ilk kez anlamak için verilerinizin bir örneğini alın. Şu anda, [`sample()`](https://docs.microsoft.com//python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py#sample-sample-strategy--arguments-) DataSet sınıfındaki yöntem ilk N, basit rastgele ve düzensiz örnekleme stratejilerini destekler.

```Python
from azureml.core.dataset import Dataset
import random

# create an in-memory Dataset from a local file
dataset = Dataset.auto_read_files('./data/crime.csv')

# create seed for Simple Random and Stratified sampling
seed = random.randint(0, 4294967295)
```

### <a name="top-n-sample"></a>İlk N örnek

İlk N örnekleme için, veri kümenizin ilk n kaydı örneğdir. Bu, veri kayıtlarınızın ne gibi göründüğünü veya hangi alanların verilerinizde olduğunu görmek için yararlıdır.

```Python
top_n_sample_dataset = dataset.sample('top_n', {'n': 5})
top_n_sample_dataset.to_pandas_dataframe()
```

||id|Büyük/küçük harf numarası|Tarih|Engelle|IUCR|Birincil tür|...|
-|--|-----------|----|-----|----|------------|---
0|10498554|HZ239907|4/4/2016 23:56|007XX E 111TH ST|1153|ALDATICI UYGULAMA|...
1\.|10516598|HZ258664|4/15/2016 17:00|082XX S ŞEKLIK ALANI AVE|890|HIRSIZLIĞI|...
2|10519196|HZ261252|4/15/2016 10:00|104XX S SACRAMENTO AVE|1154|ALDATICI UYGULAMA|...
3|10519591|HZ261534|4/15/2016 9:00|113XX S PRAIRIE AVE|1120|ALDATICI UYGULAMA|...
4|10534446|HZ277630|4/15/2016 10:00|055XX N KEDZIE AVE|890|HIRSIZLIĞI|...

### <a name="simple-random-sample"></a>Basit rastgele örnek

Basit rastgele örnekleme içinde, veri popülasyonunun her üyesinin, örneğin bir parçası olarak bir eşit seçilebilme olasılığı vardır. `simple_random` Örnek stratejide, veri kümenizdeki kayıtlar belirtilen olasılığa göre seçilir ve değiştirilmiş bir veri kümesini döndürür. Çekirdek parametresi isteğe bağlıdır.

```Python
simple_random_sample_dataset = dataset.sample('simple_random', {'probability':0.3, 'seed': seed})
simple_random_sample_dataset.to_pandas_dataframe()
```

||id|Büyük/küçük harf numarası|Tarih|Engelle|IUCR|Birincil tür|...|
-|--|-----------|----|-----|----|------------|---
0|10516598|HZ258664|4/15/2016 17:00|082XX S ŞEKLIK ALANI AVE|890|HIRSIZLIĞI|...
1\.|10519196|HZ261252|4/15/2016 10:00|104XX S SACRAMENTO AVE|1154|ALDATICI UYGULAMA|...
2|10534446|HZ277630|4/15/2016 10:00|055XX N KEDZIE AVE|890|HIRSIZLIĞI|...

### <a name="stratified-sample"></a>Bağlı örnek

Oluşturulan örnekler, bir popülasyon belirli gruplarının örnekte temsil edildiğinden emin olmanızı sağlamaktır. Örnek stratejide, popülasyon, benzerlere göre konuşmasıyla Strata veya alt gruplar halinde bölünür ve kayıtlar, `fractions` parametre tarafından belirtilen konuşmasıyla Strata ağırlıklarını göre her bir konuşmasıyla Strata 'dan rastgele seçilir. `stratified`

Aşağıdaki örnekte, her bir kaydı belirtilen sütunlara göre gruplandırıyoruz ve ' deki konuşmasıyla Strata X ağırlığı bilgilerine göre diyor olan `fractions`kaydı dahil ettik. Bir konuşmasıyla Strata belirtilmemişse veya kayıt gruplandırılamamışsa, varsayılan olarak örnek ağırlığı 0 ' dır.

```Python
# take 50% of records with `Primary Type` as `THEFT` and 20% of records with `Primary Type` as `DECEPTIVE PRACTICE` into sample Dataset
fractions = {}
fractions[('THEFT',)] = 0.5
fractions[('DECEPTIVE PRACTICE',)] = 0.2

sample_dataset = dataset.sample('stratified', {'columns': ['Primary Type'], 'fractions': fractions, 'seed': seed})

sample_dataset.to_pandas_dataframe()
```

||id|Büyük/küçük harf numarası|Tarih|Engelle|IUCR|Birincil tür|...|
-|--|-----------|----|-----|----|------------|---
0|10516598|HZ258664|4/15/2016 17:00|082XX S ŞEKLIK ALANI AVE|890|HIRSIZLIĞI|...
1\.|10534446|HZ277630|4/15/2016 10:00|055XX N KEDZIE AVE|890|HIRSIZLIĞI|...
2|10535059|HZ278872|4/15/2016 4:30|004XX SN KILBOURN AVE|810|HIRSIZLIĞI|...

## <a name="explore-with-summary-statistics"></a>Özet istatistikleriyle keşfet

 [`get_profile()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#get-profile-arguments-none--generate-if-not-exist-true--workspace-none--compute-target-none-) Yöntemi ile anomali, eksik değer veya hata sayısını tespit edin. Bu işlev, verilerinizin profilini ve Özet istatistiklerini alır. Bu, sırasıyla uygulanacak gerekli veri hazırlama işlemlerini belirlemesine yardımcı olur.

```Python
dataset.get_profile()
```

||Type|Min|Maks|Sayı|Eksik sayısı|Sayısı eksik|Eksik yüzde|Hata sayısı|Boş sayısı|% 0,1 Quantile|%1 Quantile|%5 Quantile|% 25 Quantile|% 50 Quantile|% 75 Quantile|% 95 Quantile|% 99 Quantile|% 99,9 Quantile|Ortalama|Standart Sapma|Varyans|Komutunu|Basıklık
-|----|---|---|-----|-------------|-----------------|---------------|-----------|-----------|-------------|-----------|-----------|------------|------------|------------|------------|------------|--------------|----|------------------|--------|--------|--------
id|,. INTEGER|1.04986 e + 07|1.05351 e + 07|10.0|0.0|10.0|0.0|0.0|0.0|1.04986 e + 07|1.04992 e + 07|1.04986 e + 07|1.05166 e + 07|1.05209 e + 07|1.05259 e + 07|1.05351 e + 07|1.05351 e + 07|1.05351 e + 07|1.05195 e + 07|12302,7|1.51358 e + 08|-0,495701|-1,02814
Büyük/küçük harf numarası|FieldType.STRING|HZ239907|HZ278872|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
Date|Güncellik TARIHI|2016-04-04 23:56:00 + 00:00|2016-04-15 17:00:00 + 00:00|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
Engelle|FieldType.STRING|004XX SN KILBOURN AVE|113XX S PRAIRIE AVE|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
IUCR|,. INTEGER|810|1154|10.0|0.0|10.0|0.0|0.0|0.0|810|850|810|890|1136|1153|1154|1154|1154|1058,5|137,285|18847,2|-0,785501|-1,3543
Birincil tür|FieldType.STRING|ALDATICI UYGULAMA|HIRSIZLIĞI|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
Açıklama|FieldType.STRING|SAHTE DENETIM|$500 ÜZERINDEN|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
Konum açıklaması|FieldType.STRING||OKUL, GENEL, OLUŞTURMA|10.0|0.0|10.0|0.0|0.0|1.0||||||||||||||
Arrest|,. BOOLEAN|False|False|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
Yurt içi|,. BOOLEAN|False|False|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
Sinyal|,. INTEGER|531|2433|10.0|0.0|10.0|0.0|0.0|0.0|531|531|531|614|1318,5|1911|2433|2433|2433|1371,1|692,094|478994|0,105418|-1,60684
Bölge|,. INTEGER|5|24|10.0|0.0|10.0|0.0|0.0|0.0|5|5|5|6|13|19|24|24|24|13.5|6,94822|48,2778|0,0930109|-1,62325
İleri Git|,. INTEGER|1\.|48|10.0|0.0|10.0|0.0|0.0|0.0|1\.|5|1\.|9|22.5|40|48|48|48|24.5|16,2635|264,5|0,173723|-1,51271
Topluluk alan|,. INTEGER|4|77|10.0|0.0|10.0|0.0|0.0|0.0|4|8.5|4|24|37,5|71|77|77|77|41,2|26,6366|709,511|0,112157|-1,73379
FBI kod|,. INTEGER|6|11|10.0|0.0|10.0|0.0|0.0|0.0|6|6|6|6|11|11|11|11|11|9.4|2,36643|5.6|-0,702685|-1,59582
X koordinatı|,. INTEGER|1.16309 e + 06|1.18336 e + 06|10.0|7.0|3,0|0,7|0.0|0.0|1.16309 e + 06|1.16309 e + 06|1.16309 e + 06|1.16401 e + 06|1.16678 e + 06|1.17921 e + 06|1.18336 e + 06|1.18336 e + 06|1.18336 e + 06|1.17108 e + 06|10793,5|1.165 e + 08|0,335126|-2,33333
Y koordinatı|,. INTEGER|1.8315 e + 06|1.908 e + 06|10.0|7.0|3,0|0,7|0.0|0.0|1.8315 e + 06|1.8315 e + 06|1.8315 e + 06|1.83614 e + 06|1.85005 e + 06|1.89352 e + 06|1.908 e + 06|1.908 e + 06|1.908 e + 06|1.86319 e + 06|39905,2|1.59243 e + 09|0,293465|-2,33333
Yıl|,. INTEGER|2016|2016|10.0|0.0|10.0|0.0|0.0|0.0|2016|2016|2016|2016|2016|2016|2016|2016|2016|2016|0|0|NaN|NaN
Güncelleştirme tarihi|Güncellik TARIHI|2016-05-11 15:48:00 + 00:00|2016-05-27 15:45:00 + 00:00|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
Enlem|FieldType.DECIMAL|41,6928|41,9032|10.0|7.0|3,0|0,7|0.0|0.0|41,6928|41,6928|41,6928|41,7057|41,7441|41,8634|41,9032|41,9032|41,9032|41,78|0,109695|0,012033|0,292478|-2,33333
Boylam|FieldType.DECIMAL|-87,6764|-87,6043|10.0|7.0|3,0|0,7|0.0|0.0|-87,6764|-87,6764|-87,6764|-87,6734|-87,6645|-87,6194|-87,6043|-87,6043|-87,6043|-87,6484|0,0386264|0,001492|0,344429|-2,33333
Location|FieldType.STRING||(41,903206037,-87,676361925)|10.0|0.0|10.0|0.0|0.0|7.0||||||||||||||

## <a name="impute-missing-values"></a>Eksik değerleri impute

Veri kümelerinde, null değerler, NaN ve hiçbir içerik içermeyen değerler eksik değerler olarak kabul edilir. Bunlar, Machine Learning modellerinizin performansını etkileyebilir veya geçersiz ekibinizle 'e yol açabilir. Imputation, eksik değerleri ele almak için sık karşılaşılan bir yaklaşımdır ve yalnızca silmeniz gereken eksik değer kayıtlarının yüksek bir yüzdesine sahip olduğunuzda yararlıdır.

Önceki bölümde oluşturulan veri kümesi profilinden, ve `Latitude` `Longitude` sütunlarının yüksek oranda eksik değerleri olduğunu görüyoruz. Bu örnekte, bu iki sütun için Ortalama ve ımpute eksik değerleri hesapladık.

İlk olarak, veri kümesinin en son tanımına sahip [`get_definition()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#get-definition-version-id-none-) ve verileri halden [`keep_columns()`](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow#keep-columns-columns--multicolumnselection--validate-column-exists--bool---false-----azureml-dataprep-api-dataflow-dataflow)alın, bu nedenle yalnızca adreslemenizi istediğimiz sütunları görüntüleyebiliriz.

```Python
from azureml.core.dataset import Dataset
import azureml.dataprep as dprep

# get the latest definition of Dataset
ds_def = dataset.get_definition()

# keep useful columns for this example
ds_def = ds_def.keep_columns(['ID', 'Arrest', 'Latitude', 'Longitude'])
ds_def.head(3)
```

||id|Arrest| Enlem|Boylam|
-|---------|-----|---------|----------|
|0|10498554|False|41,692834|-87,604319|
|1\.|10516598|False| 41,744107 |-87,664494|
|2|10519196|False| NaN|NaN|

Sonra, [`summarize()`](/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-ml-py#summarize-summary-columns--typing-union-typing-list-azureml-dataprep-api-dataflow--summarycolumnsvalue---nonetype----none--group-by-columns--typing-union-typing-list-str---nonetype----none--join-back--bool---false--join-back-columns-prefix--typing-union-str--nonetype----none-----azureml-dataprep-api-dataflow-dataflow) işlevini kullanarak `MEAN` Enlem sütununun değerini kontrol edin. Bu işlev dizi sütunların kabul `group_by_columns` toplama düzeyini belirtmek için parametre. Parametresi, geçerli sütun `SummaryColumnsValue` adı, yeni hesaplanan `SummaryFunction` alan adı ve gerçekleştirilecek öğesini belirten işlevini kabul eder. `summary_columns`

```Python
lat_mean = ds_def.summarize(group_by_columns = ['Arrest'],
                            summary_columns = [dprep.SummaryColumnsValue(column_id = 'Latitude',
                                                                         summary_column_name = 'Latitude_MEAN',
                                                                         summary_function = dprep.SummaryFunction.MEAN)])
lat_mean = lat_mean.filter(lat_mean['Arrest'] == False)
lat_mean.head(1)
```

||Arrest|Latitude_MEAN|
--|-----|--------|
|0|False|41,780049|

Değerleri bir kez daha doğruladıktan sonra [`ImputeMissingValuesBuilder`](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.api.builders.imputemissingvaluesbuilder?view=azure-dataprep-py) , hesaplanmış `MIN`, `MAX`, `MEAN` değer veya bir `CUSTOM` değere sahip sütunları izleyen sabit bir ifade öğrenmek için kullanın. Zaman `group_by_columns` belirtilirse, eksik değerler imputed grubuyla tarafından `MIN`, `MAX`, ve `MEAN` grubuna göre hesaplanır.

, [`ImputeColumnArguments`](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.api.builders.imputecolumnarguments?view=azure-dataprep-pyfunction) Bir column_id dizesini kabul eder ve ımpute türünü belirtmek için bir `ReplaceValueFunction` . Eksik boylam değeri için, dış bilgiye göre-87 ile uyumlu hale gelir.

```Python
# impute with MEAN
impute_mean = dprep.ImputeColumnArguments(column_id = 'Latitude',
                                          impute_function = dprep.ReplaceValueFunction.MEAN)

# impute with custom value -87
impute_custom = dprep.ImputeColumnArguments(column_id='Longitude',
                                            custom_impute_value=-87)
```

Impute adımları, `ImputeMissingValuesBuilder` [`Builders`](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.api.builders.builders?view=azure-dataprep-py) sınıf işlevi [`impute_missing_values()`](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.api.builders.builders?view=azure-dataprep-py#impute-missing-values-impute-columns--typing-list-azureml-dataprep-api-builders-imputecolumnarguments----none--group-by-columns--typing-union-typing-list-str---nonetype----none-----azureml-dataprep-api-builders-imputemissingvaluesbuilder)kullanılarak bir nesne içinde birlikte zincirleme yapılabilir. `impute_columns` Parametreyi kabul eden bir dizi `ImputeColumnArguments` nesneleri.

```Python
# get instance of ImputeMissingValuesBuilder
impute_builder = ds_def.builders.impute_missing_values(impute_columns=[impute_mean, impute_custom],
                                                   group_by_columns=['Arrest'])
```

Impute adımlarını depolamak için [`to_dataflow()`](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.api.builders.imputemissingvaluesbuilder?view=azure-dataprep-py#to-dataflow------azureml-dataprep-api-dataflow-dataflow) [işleviçağırınvekullanarakbunlarıbirveriakışınesnesineuygulayın.`learn()`](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.api.builders.imputemissingvaluesbuilder?view=azure-dataprep-py#learn------none)

```Python
impute_builder.learn()
ds_def = impute_builder.to_dataflow()
ds_def.head(3)
```

Aşağıdaki çıktı tablosunda gösterildiği gibi, eksik Enlem `MEAN` `Arrest==False` grup değeriyle birlikte verildi ve eksik Boylam-87 ile güncelleştirildi.

||id|Arrest|Enlem|Boylam
-|---------|-----|---------|----------
0|10498554|False|41,692834|-87,604319
1\.|10516598|False|41,744107|-87,664494
2|10519196|False|41,780049|-87,000000

Gerçekleştirilen dönüştürme adımlarını tutmak [`update_definition()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py#update-definition-definition--definition-update-message-) için veri kümesi tanımını ile güncelleştirin.

```Python
dataset = dataset.update_definition(ds_def, 'Impute Missing')
dataset.head(3)
```

||id|Arrest|Enlem|Boylam
-|---------|-----|---------|----------
0|10498554|False|41,692834|-87,604319
1\.|10516598|False|41,744107|-87,664494
2|10519196|False|41,780049|-87,000000

## <a name="create-assertion-rules"></a>Onaylama kuralları oluşturma

Verileri temizleme ve hazırlama sırasında birlikte çalışmadığımız veriler, üretim için gereken toplam veri kümesinin yalnızca bir alt kümesidir. Sonuç olarak, temizimizin bir parçası olarak yaptığımız bazı varsayımlar false olarak değişebilir. Örneğin, sürekli olarak güncelleştiren bir veri kümesinde, özgün olarak yalnızca belirli bir aralıktaki sayıları içeren bir sütun, sonraki yürütmeler için daha geniş bir değer aralığı içerebilir. Bu hatalar genellikle bozuk işlem hatları veya hatalı verilere neden olur.

Veri kümeleri, ardışık düzen yürütülene değerlendirilen veriler üzerinde onaylama oluşturmayı destekler. Bu onay onayları, verilerdeki varsayımlarımızın doğru olmaya devam ettiğini ve ne zaman, ne zaman uygun şekilde işleneceğini doğrulamamızı sağlar.

Örneğin, veri kümenizdeki `Latitude` `Longitude` verileri [`assert_value()`](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py#assert-value-columns--multicolumnselection--expression--azureml-dataprep-api-expressions-expression--policy--azureml-dataprep-api-engineapi-typedefinitions-assertpolicy----assertpolicy-errorvalue--1---error-code--str----assertionfailed------azureml-dataprep-api-dataflow-dataflow) belirli sayısal aralıklara kısıtlamak istiyorsanız, yöntemi her zaman büyük/küçük harf olmasını sağlar.

```Python
from azureml.dataprep import value
from azureml.core.dataset import Dataset

# get the latest definition of the Dataset
ds_def = dataset.get_definition()

# set assertion rules for `Latitude` and `Longitude` columns
ds_def = ds_def.assert_value('Latitude', (value <= 90) & (value >= -90), error_code='InvalidLatitude')
ds_def = ds_def.assert_value('Longitude', (value <= 180) & (value >= -87), error_code='InvalidLongitude')

ds_def.get_profile()
```

||Type|Min|Maks|Sayı|Eksik sayısı|Sayısı eksik|Eksik yüzde|Hata sayısı|Boş sayısı|% 0,1 Quantile|%1 Quantile|%5 Quantile|% 25 Quantile|% 50 Quantile|% 75 Quantile|% 95 Quantile|% 99 Quantile|% 99,9 Quantile|Ortalama|Standart Sapma|Varyans|Komutunu|Basıklık
-|----|---|---|-----|-------------|-----------------|---------------|-----------|-----------|-------------|-----------|-----------|------------|------------|------------|------------|------------|--------------|----|------------------|--------|--------|--------
id|,. INTEGER|1.04986 e + 07|1.05351 e + 07|10.0|0.0|10.0|0.0|0.0|0.0|1.04986 e + 07|1.04992 e + 07|1.04986 e + 07|1.05166 e + 07|1.05209 e + 07|1.05259 e + 07|1.05351 e + 07|1.05351 e + 07|1.05351 e + 07|1.05195 e + 07|12302,7|1.51358 e + 08|-0,495701|-1,02814
Arrest|,. BOOLEAN|False|False|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
Enlem|FieldType.DECIMAL|41,6928|41,9032|10.0|0.0|10.0|0.0|0.0|0.0|41,6928|41,7185|41,6928|41,78|41,78|41,78|41,9032|41,9032|41,9032|41,78|0,0517107|0,002674|0,837593|1,05
Boylam|,. INTEGER|-87|-87|10.0|0.0|10.0|0.0|3,0|0.0|-87|-87|-87|-87|-87|-87|-87|-87|-87|-87|0|0|NaN|NaN

Profil ' de, `Error Count` `Longitude` sütununun 3 olduğunu görürsünüz. Aşağıdaki kod, veri kümesini filtreler, hatayı alır ve hangi değerin onaylama başarısız olmasına neden olduğunu görür. Buradan kodunuzu ayarlayın ve verilerinizi uygun şekilde yeniden yapın.

```Python
from azureml.dataprep import col

ds_error = ds_def.filter(col('Longitude').is_error())
error = ds_error.head(10)['Longitude'][0]

print(error.originalValue)
```

    -87.60431945

## <a name="derive-columns-by-example"></a>Sütunları örneğe göre türet

Veri kümeleri için daha gelişmiş araçlardan biri, istenen sonuçların örneklerini kullanarak sütunları türetebilme yeteneğidir. Bu, SDK 'ya bir örnek vermenizi sağlar, böylece amaçlanan dönüştürmeleri elde etmek için kod üretebilir.

```Python
from azureml.core.dataset import Dataset

# create an in-memory Dataset from a local file
dataset = Dataset.auto_read_files('./data/crime.csv')
dataset.head(3)
```

||id|Büyük/küçük harf numarası|Tarih|Engelle|...|
-|---------|-----|---------|----|---
0|10498554|HZ239907|2016-04-04 23:56:00|007XX E 111TH ST|...
1\.|10516598|HZ258664|2016-04-15 17:00:00|082XX S ŞEKLIK ALANI AVE|...
2|10519196|HZ261252|2016-04-15 10:00:00|104XX S SACRAMENTO AVE|...

Tarih ve saat biçimini ' 2016-04-04 10PM-12:00:00 ' olarak dönüştürmeniz gerektiğini varsayalım. Bağımsız değişkeninde, Şu biçimdeki `example_data` parametreye istediğiniz çıkışın örneklerini sağlayın: *(özgün çıkış, istenen çıkış).* [`derive_column_by_example()`](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py#derive-column-by-example-source-columns--sourcecolumns--new-column-name--str--example-data--exampledata-----azureml-dataprep-api-dataflow-dataflow)

Aşağıdaki kod, istenen çıkışın iki örneğini sağlar, ("2016-04-04 23:56:00", "2016-04-04 10PM-12HAR") ve ("2016-04-15 17:00:00", "2016-04-15 4PM-6PM")

```Python
ds_def = dataset.get_definition()
ds_def = ds_def.derive_column_by_example(
        source_columns = "Date",
        new_column_name = "Date_Time_Range",
        example_data = [("2016-04-04 23:56:00", "2016-04-04 10PM-12AM"), ("2016-04-15 17:00:00", "2016-04-15 4PM-6PM")]
    )
ds_def.keep_columns(['ID','Date','Date_Time_Range']).head(3)
```

Aşağıdaki tabloda, Date_Time_Range yeni bir sütunun belirtilen biçimdeki kayıtları içerdiğini fark edersiniz.

||id|Date|Date_Time_Range
-|--------|-----|----
0|10498554|2016-04-04 23:56:00|2016-04-04 10PM-12:00
1|10516598|2016-04-15 17:00:00|2016-04-15 4PM-6PM
2|10519196|2016-04-15 10:00:00|2016-04-15 10:00-12PM

```Python
# update Dataset definition to keep the transformation steps performed.
dataset = dataset.update_definition(ds_def, 'Derive Date_Time_Range')
```

## <a name="fuzzy-groupings"></a>Benzer gruplandırmalar

Farklı kaynaklardan veri topladığınızda, yazım, büyük harf veya aynı varlıkların kısaltmalarının çeşitlemelerine karşılaşabilirsiniz. Bu türevlerini SDK 'nın benzer gruplandırmasına veya metin kümelemesine, işlevlerle otomatik olarak standartlaştırın ve mutabık kılma.

Örneğin, sütun `inspections.business.city` "San Francisco" Şehir adının birkaç formunu içerir.

```Python
from azureml.core.dataset import Dataset

# create an in-memory Dataset from a local json file
dataset = Dataset.auto_read_files('./data/city.json')
dataset.head(5)
```

||ınspections. Business. business_id|ınspections. business_name|ınspections. Business. Address|ınspections. Business. City|...|
-|-----|-------------------------|------------|--|---
0|16162|Hızlı-N-Ezee Hint bilgisayarları|3861 24|VARIŞ|...
1\.|67565|Tay dili Noodles Cafe King|1541 TARAFıMVAL St|SAN FRANCISCO|...
2|67565|Tay dili Noodles Cafe King|1541 TARAFıMVAL St|SAN FRANCISCO|...
3|68701|Grindz|832 Clement St|VARIŞ|...
4|69186|Premier & Olaylar, Inc.|1255 22 St|S.F.|...

Otomatik olarak "San [`fuzzy_group_column()`](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.dataflow?view=azure-dataprep-py#fuzzy-group-column-source-column--str--new-column-name--str--similarity-threshold--float---0-8--similarity-score-column-name--str---none-----azureml-dataprep-api-dataflow-dataflow) Francisco" adlı kurallı biçimde algılanan bir sütun eklemek için yöntemini kullanalım. Bağımsız değişkenler `source_column` ve `new_column_name` gereklidir. Ayrıca şunları yapabilirsiniz:

* Her orijinal ve kurallı değer `similarity_score_column_name`çifti arasındaki benzerlik Puanını gösteren yeni bir sütun oluşturun.

* Birlikte gruplandırılacak değerler için en küçük benzerlik puanı olan bir `similarity_threshold`belirtin.

```Python
# get the latest Dataset definition
ds_def = dataset.get_definition()
ds_def = ds_def.fuzzy_group_column(source_column='inspections.business.city',
                                       new_column_name='city_grouped',
                                       similarity_threshold=0.8,
                                       similarity_score_column_name='similarity_score')
ds_def.head(5)
```

||ınspections. Business. business_id|ınspections. business_name|ınspections. Business. Address|ınspections. Business. City|city_grouped|similarity_score|...|
-|-----|-------------------------|------------|--|---|---|---
0|16162|Hızlı-N-Ezee Hint bilgisayarları|3861 24|VARIŞ|San Francisco|0,814806|...
1\.|67565|Tay dili Noodles Cafe King|1541 TARAFıMVAL St|SAN FRANCISCO|San Francisco|1,000000|...
2|67565|Tay dili Noodles Cafe King|1541 TARAFıMVAL St|SAN FRANCISCO|San Francisco|1,000000|...
3|68701|Grindz|832 Clement St|VARIŞ|San Francisco|0,814806|...
4|69186|Premier & Olaylar, Inc.|1255 22 St|S.F.|San Francisco|0,814806|...

Elde edilen veri kümesi tanımında, veride "San Francisco" öğesini temsil eden tüm farklı Çeşitlemeler, "San Francisco" olarak aynı dizeye normalleştirilir.

Bu benzer gruplandırma adımını ile `update_definition()`en son veri kümesi tanımına kaydedin.

```Python
dataset = dataset.update_definition(ds_def, 'fuzzy grouping')
```

## <a name="next-steps"></a>Sonraki adımlar

* Regresyon modeli örneği için bkz. otomatik makine öğrenimi [öğreticisi](tutorial-auto-train-models.md) .

* Tasarım desenleri ve kullanım örnekleri için SDK ['ya genel bakış](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) bölümüne bakın.

* Veri kümelerini kullanmayla ilgili bir örnek için bkz. [örnek Not defterleri](https://aka.ms/dataset-tutorial).
