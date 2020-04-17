---
title: Sürümdeki yenilikler nelerdir?
titleSuffix: Azure Machine Learning
description: Azure Machine Learning ile ilgili en son güncellemeler ve makine öğrenimi ve veri hazırlama Python SDK'ları hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
ms.author: jmartens
author: j-martens
ms.date: 03/10/2020
ms.openlocfilehash: 9ed17ed0825e0011d1402efc45155755a1ecbadf
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81459177"
---
# <a name="azure-machine-learning-release-notes"></a>Azure Machine Learning sürüm notları

Bu makalede, Azure Machine Learning sürümleri hakkında bilgi edinin.  SDK başvuru içeriğinin tamamı için, Python başvuru sayfası için Azure Machine Learning'in [**ana SDK'sını**](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) ziyaret edin.

Bilinen hatalar ve geçici geçici işler hakkında bilgi edinmek için [bilinen sorunların listesine](resource-known-issues.md) bakın.

## <a name="2020-04-13"></a>2020-04-13

### <a name="azure-machine-learning-sdk-for-python-v130"></a>Python v1.3.0 için Azure Machine Learning SDK

+ **Hata düzeltmeleri ve iyileştirmeler**
  + **azureml-automl-core**
    + Eğitim sonrası operasyonlar etrafında ek telemetri eklendi.
    + 100'den uzun bir dizi için koşullu karetoplamı (CSS) eğitimini kullanarak otomatik ARIMA eğitimini hızlandırAr. Kullanılan uzunluğun TimeSeriesInternal sınıfında ki sabit ARIMA_TRIGGER_CSS_TRAINING_LENGTH w/in /src/azureml-automl-core/azureml/automl/core/shared/constants.py adresinde depolanır
    + Tahmin çalıştırmalarının kullanıcı günlüğe kaydetmesi geliştirildi, şimdi şu anda hangi aşamanın çalıştığına ilişkin daha fazla bilgi günlükte gösterilecektir
    + Target_rolling_window_size daha az değerlere ayarlanacak şekilde ayarlanmasına izin verilmeyen 2
  + **azureml-automl-runtime**
    + Yinelenen zaman damgaları bulunduğunda gösterilen hata iletisi geliştirildi.
    + İzin verilmeyen target_rolling_window_size daha az sonra 2 değerleri olarak ayarlanacak.
    + Gecikme imputasyon hatası düzeltildi. Sorun, bir diziyi mevsimsel olarak ayrıştırmak için gereken gözlem sayısının yetersizliğinden kaynaklanıyordu. "Mevsimsizleştirilmiş" veriler, gecikme uzunluğunu belirlemek için kısmi bir otokorelasyon işlevini (PACF) hesaplamak için kullanılır.
    + Featurization config tarafından görevleri tahmin etmek için etkin sütun amacı featurization özelleştirme. Görevleri tahmin etmek için sütun amacı olarak Sayısal ve Kategorik artık desteklenir.
    + Featurization config tarafından görevleri tahmin etmek için etkin damla sütun featurization özelleştirme.
    + Featurization config tarafından görevleri tahmin etmek için etkin imputization özelleştirme. Hedef sütun ve ortalama, ortanca, most_frequent ve eğitim verileri için sabit değer imputasyonu için sabit değer imputasyonu artık desteklenir.
  + **azureml-contrib-pipeline-steps**
    + ParallelRunConfig'e geçirilecek dize işlem adlarını kabul etme
  + **azureml-core**
    +  Çevre nesnesinin bir kopyasını oluşturmak için Environment.clone(new_name) API eklendi
    +  Environment.docker.base_dockerfile dosya yolunu kabul eder. Bir dosyayı çözebilirse, içerik base_dockerfile ortam özelliğine okunur
    + Kullanım alanı Çevre'de el ile bir değer belirlediğinde base_image ve base_dockerfile için birbirini dışlayan değerleri otomatik olarak sıfırlayın.docker
    +  Dataset: unicode karakter içeren veri yolu varsa sabit veri kümesi indirme hatası
    +  Dataset: Azure Machine Learning Compute'daki minimum disk alanı gereksinimine uymak için geliştirilmiş veri kümesi bağlama önbelleğe alma mekanizması, düğümü kullanılamaz hale getirmenizi ve işin iptal edilmesine neden olur
    + Ortamın kullanıcı tarafından mı yoksa AzureML tarafından mı yönetildiğini gösteren RSection user_managed bayrağı eklendi.
    + Dataset: Zaman dizisi tabanlı veri erişimine erişimi hızlandırmak için kullanılan bir zaman dizisi veri kümesine pandalar veri çerçevesi olarak erişdiğinizde zaman aralığı sütununa bir dizin ekleriz.  Daha önce, dizin zaman damgası sütunu ile aynı ad verildi, hangi gerçek zaman damgası sütunu ve dizin hakkında kullanıcıları kafa karıştırıcı. Sütun olarak kullanılmaması gerektiğinden, artık dizine belirli bir ad vermiyoruz. 
  + **azureml-dataprep**
    + Egemen bulutta veri kümesi kimlik doğrulama sorunu düzeltildi
    + Azure `Dataset.to_spark_dataframe` PostgreSQL veri mağazalarından oluşturulan veri kümeleri için hata düzeltildi
  + **azureml-interpret**
    + Yerel önem değerleri seyrekse görselleştirmeye global puanlar eklendi
    + Yorumlama-topluluk 0.9'u kullanmak için azureml-yorumlama ile güncelleştirildi.*
    + Seyrek değerlendirme verilerine sahip açıklama indirme ile ilgili sorun düzeltildi
    + AutoML'de açıklama nesnesinin seyrek biçimieklendi
  + **azureml-boru hattı-çekirdek**
    + Destek ComputeInstance boru hatlarında işlem hedefi olarak
  + **azureml-train-automl-client**
    + Eğitim sonrası operasyonlar etrafında ek telemetri eklendi.
    + Erken durdurmada gerileme düzeltildi
    + Giriş verileri için geçerli bir tür olarak deprecated azureml.dprep.Dataflow.
    +  Varsayılan AutoML deneme zaman dilimini 6 güne değiştirme.
  + **azureml-train-automl-runtime**
    + Eğitim sonrası operasyonlar etrafında ek telemetri eklendi.
    + seyrek automl e2e desteği eklendi
  + **azureml-opendatasets**
    + Servis monitörü için ek telemetri eklendi.
    + Stabiliteyi artırmak için blob için ön kapıyı etkinleştirin 
## <a name="2020-03-23"></a>2020-03-23

### <a name="azure-machine-learning-sdk-for-python-v120"></a>Python v1.2.0 için Azure Machine Learning SDK

+ **Yeni değişiklikler**
  + Python 2.7 için bırakma desteği

+ **Hata düzeltmeleri ve iyileştirmeler**
  + **masmavi-cli-ml**
    + CLI'deki `az ml model/computetarget/service` komutlara "--abonelik kimliği" ekler
    + ACI dağıtımı için müşteri yönetilen anahtar (CMK) vault_url, key_name ve key_version için destek ekleme
  + **azureml-automl-core** 
    + Hem X hem de y veri tahmin görevleri için sabit değere sahip özelleştirilmiş imputasyon etkinleştirildi.
    + Kullanıcıya hata iletileri gösteren sorunu düzeltildi.    
  + **azureml-automl-runtime**
    + Yalnızca bir satıra sahip taneler içeren veri kümelerinde tahmin le ilgili sorunu düzeltti
    + Tahmin görevlerinin gerektirdiği bellek miktarını azalttı.
    + Zaman sütunu yanlış biçime sahipse daha iyi hata iletileri eklendi.
    + Hem X hem de y veri tahmin görevleri için sabit değere sahip özelleştirilmiş imputasyon etkinleştirildi.
  + **azureml-core**
    + AZUREML_SERVICE_PRINCIPAL_ID, AZUREML_SERVICE_PRINCIPAL_TENANT_ID ve AZUREML_SERVICE_PRINCIPAL_PASSWORD: Ortam değişkenlerinden Hizmet Müdürü yükleme desteği ekleyin
    + Yeni bir parametre `support_multi_line` `Dataset.Tabular.from_delimited_files`olarak tanıtılan`support_multi_line=False`: Varsayılan olarak ( ), tüm satır sonları, alıntı alan değerleri de dahil olmak üzere, bir kayıt sonu olarak yorumlanacaktır. Verileri bu şekilde okumak, birden çok CPU çekirdeğinde paralel yürütme için daha hızlı ve daha optimize edilebiyidir. Ancak, sessizce yanlış hizalanmış alan değerleri ile daha fazla kayıt üreten neden olabilir. Bu, sınırlandırılmış `True` dosyaların teklif edilen satır sonlarını içerdiği bilinen zaman ayarlanmalıdır.
    + Azure Machine Learning CLI'ye ADLS Gen2'yi kaydetme özelliği eklendi
    + Parametrelerin kullanımını daha iyi yansıtmak için TabularDataset'teki with_timestamp_columns() yöntemi için fine_grain_timestamp 'zaman damgası' ve parametre 'coarse_grain_timestamp' ile 'partition_timestamp' olarak yeniden adlandırılmıştır.
    + Maksimum deneme adı uzunluğu 255'e yükseltildi.
  + **azureml-interpret**
    + Yorumlama-topluluk 0.7.* için güncelleştirilmiş azureml-yorumlama
  + **azureml-sdk**
    + Ön sürüm ve kararlı sürümlerde yama desteği için uyumlu sürüm Tilde ile bağımlılıkları değiştirme.


## <a name="2020-03-11"></a>2020-03-11

### <a name="azure-machine-learning-sdk-for-python-v115"></a>Python v1.1.5 için Azure Machine Learning SDK

+ **Özellik amortismanı**
  + **Python 2.7**
    + Python 2.7'yi destekleyen son sürüm

+ **Yeni değişiklikler**
  + **Anlamsal Versiyon 2.0.0**
    + Sürüm 1.1 Azure ML Python SDK ile başlayarak Anlamsal Sürüm 2.0.0 benimser. [Burada daha fazla bilgi edinin](https://semver.org/). Sonraki tüm sürümler yeni numaralandırma şeması ve anlamsal sürüm sözleşme takip edecektir. 

+ **Hata düzeltmeleri ve iyileştirmeler**
  + **masmavi-cli-ml**
    + Uç nokta CLI komut adını tutarlılık için 'az ml endpoint aks' ile 'az ml endpoint realtime' olarak değiştirin.
    + kararlı ve deneysel dal CLI için CLI kurulum talimatlarını güncelleştirin
    + Tek örnek profil leme bir öneri üretmek için sabit ve çekirdek sdk kullanılabilir hale getirilmiştir.
  + **azureml-automl-core**
    + Automl ONNX modelleri için Toplu Iş modu çıkarımını (bir kez birden fazla satır alarak) etkinleştirildi
    + Veri kümelerinde frekans tespiti, veri eksikliği veya düzensiz veri noktaları içeren geliştirilmiş
    + Baskın frekansa uymayan veri noktalarını kaldırma özelliği eklendi.
    + İlgili sütunlar için imputation seçeneklerini uygulamak için seçenekler inemasını almak için oluşturucunun girdisini değiştirdi.
    + Hata günlüğe kaydetme geliştirildi.
  + **azureml-automl-runtime**
    + Eğitim setinde bulunmayan tane test setinde belirse atılan hata yla ilgili sorun giderildi
    + Tahmin hizmetinde puanlama sırasında y_query gereksinimi kaldırıldı
    + Veri kümesi uzun zaman boşlukları olan kısa tanecikler içerdiğinde tahmin sorunu giderildi.
    + Otomatik max ufuk açık ken ve tarih sütununda dizeleri şeklinde tarihler içerdiğinde sorun giderildi. Bugüne dönüşüm mümkün olmadığında uygun dönüşüm ve hata iletileri eklendi
    + FileCacheStore için ara verileri serihale getirmek ve deserialize etmek için yerel NumPy ve SciPy'yi kullanma (yerel AutoML çalıştırmaları için kullanılır)
    + Başarısız çocuk çalıştırmaları Çalışan durumda sıkışmış olabilir bir hata düzeltildi.
    + Featurization artan hız.
    + Puanlama sırasında frekans kontrolü düzeltildi, şimdi tahmin görevleri tren ve test kümesi arasında sıkı frekans eşdeğerliği gerektirmez.
    + İlgili sütunlar için imputation seçeneklerini uygulamak için seçenekler inemasını almak için oluşturucunun girdisini değiştirdi.
    + Gecikme türü seçimiyle ilgili düzeltme hataları.
    + Veri kümelerinde yükseltilen sınıflandırılmamış hata düzeltildi, tek satırlı taneler
    + Frekans algılama yavaşlığı ile sorun giderildi.
    + Eğitim başarısızlığının gerçek nedeninin bir AttributeError ile değiştirilmesine neden olan AutoML özel durum işlemedeki bir hatayı düzeltir.
  + **azureml-cli-yaygın**
    + Tek örnek profil leme bir öneri üretmek için sabit ve çekirdek sdk kullanılabilir hale getirilmiştir.
  + **azureml-contrib-mir**
    + Erişim Belirteci'ni almak için MirWebservice sınıfında işlevsellik ekler
    + MirWebservice.run() araması sırasında varsayılan olarak MirWebservice için belirteç auth kullanın - Yalnızca arama başarısız olursa yenileyin
    + Mir webservice dağıtımı artık sırasıyla [Ds2v2, A2v2 ve F16] yerine uygun Skus [Standard_DS2_v2, Standard_F16, Standard_A2_v2] gerektirir.
  + **azureml-contrib-pipeline-steps**
    + ParallelRunStep'e isteğe bağlı parametre side_inputs eklendi. Bu parametre kapsayıcıya klasör monte etmek için kullanılabilir. Şu anda desteklenen türler DataReference ve PipelineData'dır.
    + ParallelRunConfig'de geçirilen parametreler, boru hattı parametrelerini şimdi geçirerek üzerine yazılabilir. yeni boru hattı parametreleri aml_mini_batch_size, aml_error_threshold, aml_logging_level, aml_run_invocation_timeout (aml_node_count ve aml_process_count_per_node zaten önceki sürüm bir parçasıdır) desteklenen.
  + **azureml-core**
    + Dağıtılan AzureML Web hizmetleri artık `INFO` günlüğe kaydetmeye varsayılan olarak sunulur. Bu, dağıtılan hizmette `AZUREML_LOG_LEVEL` ortam değişkeni ayarlayarak denetlenebilir.
    + Python sdk, 'ardışık lıklar' yerine 'api' uç noktasını kullanmak için bulma hizmetini kullanır.
    + Tüm SDK aramalarında yeni rotalara geçiş
    + ModelManagementService'e yapılan çağrıları yeni birleştirilmiş yapıya yönlendirmede değişiklikler
      + Çalışma alanı güncelleştirme yöntemini genel kullanıma sunuldu.
      + Kullanıcının görüntü oluşturma için bilgi işlem güncelleştirmesine izin vermek için çalışma alanı güncelleştirme yöntemine image_build_compute parametre eklendi
    +  Eski profil oluşturma iş akışına amortisman iletileri eklendi. Profil oluşturma işlemcisi ve bellek sınırları düzeltildi
    + R işleri çalıştırmak için Çevre'nin bir parçası olarak RSection eklendi
    +  Veri kümesinin `Dataset.mount` kaynağıerişilebilir olmadığında veya herhangi bir veri içermediğinde hata yükseltmek için doğrulama eklendi.
    + Azure `--grant-workspace-msi-access` Blob Kapsayıcısı'nı kaydetmek için Datastore CLI için ek bir parametre olarak eklendi ve vnet'in arkasındaki Blob Kapsayıcısı'nı kaydettirmenizi sağlayacak
    + Tek örnek profil leme bir öneri üretmek için sabit ve çekirdek sdk kullanılabilir hale getirilmiştir.
    + aks.py _deploy sorunu giderildi
    + Sessiz depolama hatalarını önlemek için yüklenen modellerin bütünlüğünü doğrular.
    + Kullanıcı artık web hizmetleri için anahtarları yenilerken auth tuşu için bir değer belirtebilir.
    + Büyük harflerin veri kümesinin giriş adı olarak kullanılamadığı sabit hata
  + **azureml varsayılanları**
    + `azureml-dataprep`şimdi bir parçası olarak `azureml-defaults`yüklenecektir. Veri kümelerini monte etmek için veri prep[fuse] veri lerini bilgi işlem hedeflerine el ile yüklemeniz artık gerekmez.
  + **azureml-interpret**
    + Yorumlama-topluluk 0.6.* için güncelleştirilmiş azureml-yorumlama
    + Yorumlama topluluğuna bağlı olarak azureml-yorumlama 0.5.0 olarak güncelleştirildi
    + Azureml yorumlamak için azureml stili özel durumlar eklendi
    + Keras modelleri için DeepScoringExplainer serileştirme düzeltildi
  + **azureml-mlflow**
    + azureml.mlflow'a egemen bulutlar için destek ekleyin
  + **azureml-boru hattı-çekirdek**
    + Pipeline toplu puanlama dizüstü bilgisayar şimdi ParallelRunStep kullanır
    + PythonScriptStep sonuçlarının bağımsız değişkenler listesini değiştirmerağmen yanlış yeniden kullanılabildiği bir hata düzeltildi
    + parse_* yöntemlerini ararken sütunların türünü ayarlama özelliği eklendi`PipelineOutputFileDataset`
  + **azureml-boru hattı adımları**
    + `azureml-pipeline-steps` Paketi `AutoMLStep` taşıdım. İçerde `AutoMLStep` `azureml-train-automl-runtime`amortismana uğradım.
    + PythonScriptStep girişi olarak veri kümesi için eklenen belge örneği
  + **azureml-tensorboard**
    + tensorflow 2.0'ı desteklemek için azureml-tensorboard güncellendi
    + İşlem Örneği'nde özel bir Tensorboard bağlantı noktası kullanırken doğru bağlantı noktası numarasını göster
  + **azureml-train-automl-client**
    + Uzaktan çalıştırmalarda belirli paketlerin yanlış sürümlerde yüklenebileceği bir sorun giderildi.
    + özel featurization config filtreleri sorunu geçersiz kılan düzeltildi FeaturizationConfig.
  + **azureml-train-automl-runtime**
    + Uzaktan çalıştırmalarda frekans algılama sorunu düzeltildi
    + `AutoMLStep` Paketi taşıdım. `azureml-pipeline-steps` İçerde `AutoMLStep` `azureml-train-automl-runtime`amortismana uğradım.
  + **azureml-train-core**
    + PyTorch Tahmincisi'nde PyTorch sürüm 1.4'ün desteklenmesi
  
## <a name="2020-03-02"></a>2020-03-02

### <a name="azure-machine-learning-sdk-for-python-v112rc0-pre-release"></a>Python v1.1.2rc0 için Azure Machine Learning SDK (Ön sürüm)

+ **Hata düzeltmeleri ve iyileştirmeler**
  + **azureml-automl-core**
    + Automl ONNX modelleri için Toplu Iş modu çıkarımını (bir kez birden fazla satır alarak) etkinleştirildi
    + Veri kümelerinde frekans tespiti, veri eksikliği veya düzensiz veri noktaları içeren geliştirilmiş
    + Baskın frequrncy uymayan veri noktaları kaldırmak için yeteneği eklendi.
  + **azureml-automl-runtime**
    + Eğitim setinde bulunmayan tane test setinde belirse atılan hata yla ilgili sorun giderildi
    + Tahmin hizmetinde puanlama sırasında y_query gereksinimi kaldırıldı
  + **azureml-contrib-mir**
    + Erişim Belirteci'ni almak için MirWebservice sınıfında işlevsellik ekler
  + **azureml-core**
    + Dağıtılan AzureML Web hizmetleri artık `INFO` günlüğe kaydetmeye varsayılan olarak sunulur. Bu, dağıtılan hizmette `AZUREML_LOG_LEVEL` ortam değişkeni ayarlayarak denetlenebilir.
    + Çalışma alanına kayıtlı `Dataset.get_all` tüm veri kümelerini döndürmek için yinele'yi düzeltin.
    + Geçersiz tür veri kümesi oluşturma `path` API'lerinin bağımsız değişkenine geçirildiğinde hata iletisini geliştirin.
    + Python sdk, 'ardışık lıklar' yerine 'api' uç noktasını kullanmak için bulma hizmetini kullanır.
    + Tüm SDK aramalarında yeni rotalara geçiş
    + ModelManagementService'e yapılan çağrıları yeni birleştirilmiş yapıya yönlendirmede değişiklikler
      + Çalışma alanı güncelleştirme yöntemini genel kullanıma sunuldu.
      + Kullanıcının görüntü oluşturma için bilgi işlem güncelleştirmesine izin vermek için çalışma alanı güncelleştirme yöntemine image_build_compute parametre eklendi
    +  Eski profil oluşturma iş akışına amortisman iletileri eklendi. Profil oluşturma işlemcisi ve bellek sınırları düzeltildi
  + **azureml-interpret**
    + azureml-interpret-interpret-community 0.6.* güncelleme
  + **azureml-mlflow**
    + azureml.mlflow'a egemen bulutlar için destek ekleyin
  + **azureml-boru hattı adımları**
    + Taşınmış `AutoMLStep` `azureml-pipeline-steps package`. İçerde `AutoMLStep` `azureml-train-automl-runtime`amortismana uğradım.
  + **azureml-train-automl-client**
    + Uzaktan çalıştırmalarda belirli paketlerin yanlış sürümlerde yüklenebileceği bir sorun giderildi.
  + **azureml-train-automl-runtime**
    + Uzaktan çalıştırmalarda frekans algılama sorunu düzeltildi
    + Taşınmış `AutoMLStep` `azureml-pipeline-steps package`. İçerde `AutoMLStep` `azureml-train-automl-runtime`amortismana uğradım.
  + **azureml-train-core**
    + Taşınmış `AutoMLStep` `azureml-pipeline-steps package`. İçerde `AutoMLStep` `azureml-train-automl-runtime`amortismana uğradım.

## <a name="2020-02-18"></a>2020-02-18

### <a name="azure-machine-learning-sdk-for-python-v111rc0-pre-release"></a>Python v1.1.1rc0 için Azure Machine Learning SDK (Ön sürüm)

+ **Hata düzeltmeleri ve iyileştirmeler**
  + **masmavi-cli-ml**
    + Tek örnek profil leme bir öneri üretmek için sabit ve çekirdek sdk kullanılabilir hale getirilmiştir.
  + **azureml-automl-core**
    + Hata günlüğe kaydetme geliştirildi.
  + **azureml-automl-runtime**
    + Veri kümesi uzun zaman boşlukları olan kısa tanecikler içerdiğinde tahmin sorunu giderildi.
    + Otomatik max ufuk açık ken ve tarih sütununda dizeleri şeklinde tarihler içerdiğinde sorun giderildi. Bugüne kadar dönüşüm mümkün değilse uygun dönüştürme ve mantıklı hata ekledik
    + FileCacheStore için ara verileri serihale getirmek ve deserialize etmek için yerel NumPy ve SciPy'yi kullanma (yerel AutoML çalıştırmaları için kullanılır)
    + Başarısız çocuk çalıştırmaları Çalışan durumda sıkışmış olabilir bir hata düzeltildi.
  + **azureml-cli-yaygın**
    + Tek örnek profil leme bir öneri üretmek için sabit ve çekirdek sdk kullanılabilir hale getirilmiştir.
  + **azureml-core**
    + Azure `--grant-workspace-msi-access` Blob Kapsayıcısı'nı kaydetmek için Datastore CLI için ek bir parametre olarak eklendi ve vnet'in arkasındaki Blob Kapsayıcısı'nı kaydettirmenizi sağlayacak
    + Tek örnek profil leme bir öneri üretmek için sabit ve çekirdek sdk kullanılabilir hale getirilmiştir.
    + aks.py _deploy sorunu giderildi
    + Sessiz depolama hatalarını önlemek için yüklenen modellerin bütünlüğünü doğrular.
  + **azureml-interpret**
    + azureml yorumlamak için azureml tarzı özel durumlar eklendi
    + keras modelleri için sabit DeepScoringExplainer serileştirme
  + **azureml-boru hattı-çekirdek**
    + Pipeline toplu puanlama dizüstü bilgisayar şimdi ParallelRunStep kullanır
  + **azureml-boru hattı adımları**
    + `AutoMLStep` Paketi taşıdım. `azureml-pipeline-steps` İçerde `AutoMLStep` `azureml-train-automl-runtime`amortismana uğradım.
  + **azureml-contrib-pipeline-steps**
    + ParallelRunStep'e isteğe bağlı parametre side_inputs eklendi. Bu parametre kapsayıcıya klasör monte etmek için kullanılabilir. Şu anda desteklenen türler DataReference ve PipelineData'dır.
  + **azureml-tensorboard**
    + tensorflow 2.0'ı desteklemek için azureml-tensorboard güncellendi
  + **azureml-train-automl-client**
    + özel featurization config filtreleri sorunu geçersiz kılan düzeltildi FeaturizationConfig.
  + **azureml-train-automl-runtime**
    + `AutoMLStep` Paketi taşıdım. `azureml-pipeline-steps` İçerde `AutoMLStep` `azureml-train-automl-runtime`amortismana uğradım.
  + **azureml-train-core**
    + PyTorch Tahmincisi'nde PyTorch sürüm 1.4'ün desteklenmesi
  
## <a name="2020-02-04"></a>2020-02-04

### <a name="azure-machine-learning-sdk-for-python-v110rc0-pre-release"></a>Python v1.1.0rc0 için Azure Machine Learning SDK (Ön sürüm)

+ **Yeni değişiklikler**
  + **Anlamsal Versiyon 2.0.0**
    + Sürüm 1.1 Azure ML Python SDK ile başlayarak Anlamsal Sürüm 2.0.0 benimser. [Burada daha fazla bilgi edinin](https://semver.org/). Sonraki tüm sürümler yeni numaralandırma şeması ve anlamsal sürüm sözleşme takip edecektir. 
  
+ **Hata düzeltmeleri ve iyileştirmeler**
  + **azureml-automl-runtime**
    + Featurization artan hız.
    + Puanlama sırasında frekans kontrolü düzeltildi, şimdi tahmin görevlerinde tren ve test seti arasında sıkı frekans denkliği gerektirmez.
  + **azureml-core**
    + Kullanıcı artık web hizmetleri için anahtarları yenilerken auth tuşu için bir değer belirtebilir.
  + **azureml-interpret**
    + Yorumlama topluluğuna bağlı olarak azureml-yorumlama 0.5.0 olarak güncelleştirildi
  + **azureml-boru hattı-çekirdek**
    + PythonScriptStep sonuçlarının bağımsız değişkenler listesini değiştirmerağmen yanlış yeniden kullanılabildiği bir hata düzeltildi
  + **azureml-boru hattı adımları**
    + PythonScriptStep girişi olarak veri kümesi için eklenen belge örneği
  + **azureml-contrib-pipeline-steps**
    + ParallelRunConfig'de geçirilen parametreler, boru hattı parametrelerini şimdi geçirerek üzerine yazılabilir. yeni boru hattı parametreleri aml_mini_batch_size, aml_error_threshold, aml_logging_level, aml_run_invocation_timeout (aml_node_count ve aml_process_count_per_node zaten önceki sürüm bir parçasıdır) desteklenen.
  
## <a name="2020-01-21"></a>2020-01-21

### <a name="azure-machine-learning-sdk-for-python-v1085"></a>Python v1.0.85 için Azure Machine Learning SDK

+ **Yeni özellikler**
  + **azureml-core**
    + Belirli bir çalışma alanında ve abonelikte AmlCompute kaynakları için geçerli temel kullanım ve kota sınırlamasını alın
  
  + **azureml-contrib-pipeline-steps**
    + Kullanıcının önceki adımdan paralel adıma ara sonuç olarak tabular veri kümesini geçirmesini sağlama

+ **Hata düzeltmeleri ve iyileştirmeler**
  + **azureml-automl-runtime**
    + Dağıtılan tahmin hizmetine istekteki y_query sütun gereksinimi kaldırıldı. 
    + 'y_query' Dominik'in Orange Juice dizüstü bilgisayar hizmeti isteği bölümünden kaldırıldı.
    + Dağıtılan modellerde tahmin oluşturmayı engelleyen hata, tarih saati sütunları ile veri kümeleri üzerinde çalışırken düzeltildi.
    + Hem ikili hem de çok sınıflı sınıflandırma için bir sınıflandırma ölçütü olarak Matthews Korelasyon Katsayısı eklendi.
  + **azureml-contrib-interpret**
    + Metin açıklamalarını azureml-contrib-interpret'tan kaldırılan metin açıklamaları, yakında yayınlanacak olan yorumlama metni repo'ya taşınmıştır.
  + **azureml-core**
    + Dataset: dosya veri kümesi için kullanımlar artık python env'de kurulacak numpy ve pandalara bağlı değildir.
    + Yerel WebService.wait_for_deployment() değiştirilen yerel Docker kapsayıcısının durumunu kontrol etmek için, sistem durumu bitiş noktasını pinglemeye çalışmadan önce, başarısız bir dağıtımı bildirmek için gereken süreyi büyük ölçüde azaltır.
    + Hizmet nesnesi LocalWebservice() oluşturucusu kullanılarak varolan bir dağıtımdan oluşturulduğunda LocalWebservice.reload()'de kullanılan dahili bir özelliğin başlatılması düzeltildi.
    + Açıklama için düzenlenen hata iletisi.
    + AksWebservice'e erişim belirteci, zaman damgası sonra yenileme, zaman damgası ve belirteç türünde son kullanma tarihi içeren AksServiceAccessToken nesnesini döndürecek get_access_token() adlı yeni bir yöntem eklendi. 
    + AksWebservice'te mevcut get_token() yöntemi, yeni yöntem bu yöntemin tüm bilgileri döndürür gibi deprecated.
    + Az ml servis get-access-token komutunun modifiye çıktısı. Token ve refreshBy'ye erişmek için yeniden adlandırılmış belirteçAfter. Son kullanma tarihiOn ve tokenType özellikleri eklendi.
    + Sabit get_active_runs
  + **azureml-explain-model**
    + 0.33.0 ve yorumlama-topluluk 0.4.* için güncelleştirilmiş shap
  + **azureml-interpret**
    + 0.33.0 ve yorumlama-topluluk 0.4.* için güncelleştirilmiş shap
  + **azureml-train-automl-runtime**
    + Hem ikili hem de çok sınıflı sınıflandırma için bir sınıflandırma ölçütü olarak Matthews Korelasyon Katsayısı eklendi.
    + Koddan amortisman öncesi bayrak ve featurization ile değiştirilir -featurization varsayılan olarak

## <a name="2020-01-06"></a>2020-01-06

### <a name="azure-machine-learning-sdk-for-python-v1083"></a>Python v1.0.83 için Azure Machine Learning SDK

+ **Yeni özellikler**
  + Dataset: İki `on_error` seçenek `out_of_range_datetime` `to_pandas_dataframe` ekleyin ve veri ile doldurmak yerine `None`hata değerleri olduğunda başarısız olmak için .
  + Çalışma alanı: `hbi_workspace` Daha fazla şifreleme sağlayan ve çalışma alanlarında gelişmiş tanılamadevre devre dışı sağlayan hassas verilere sahip çalışma alanları için bayrak eklendi. Ayrıca, çalışma alanınızı sağlarken aboneliğinizde bir Cosmos DB `cmk_keyvault` örneği `resource_cmk_uri` oluşturan bir çalışma alanı oluştururken parametreleri ve parametreleri belirterek ilişkili Cosmos DB örneği için kendi anahtarlarınızı getirmek için destek ekledik. [Burada daha fazla bilgi edinin.](https://docs.microsoft.com/azure/machine-learning/concept-enterprise-security#azure-cosmos-db)

+ **Hata düzeltmeleri ve iyileştirmeler**
  + **azureml-automl-runtime**
    + 3.5.4'ün altındaki Python sürümlerinde AutoML çalıştırılırken TypeError'ın yükseltilmesine neden olan bir gerileme düzeltildi.
  + **azureml-core**
    + Göreli `./` `datastore.upload_files` yolun kullanılmadığı hata düzeltildi.
    + Tüm Resim sınıfı kod yolları için amortisman iletileri eklendi
    + Mooncake bölgesi için Sabit Model Yönetimi URL yapısı.
    + source_dir kullanan modellerin Azure İşlevler için paketlemediği sorun düzeltildi.    
    + Görüntüyü AzureML çalışma alanı kapsayıcı kayıt defterine itmek için [Environment.build_local()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) seçeneği eklendi
    + SDK'yı azure sinaps üzerinde yeni belirteç kitaplığını arka uyumlu bir şekilde kullanmak üzere güncelleştirildi.
  + **azureml-interpret**
    + Hiçbir açıklama karşıdan yüklenebilir ken Hiçbiri döndürüldüğü hata düzeltildi. Şimdi başka bir yerde eşleşen davranış, bir istisna yükseltir.
  + **azureml-boru hattı adımları**
    + Bir 's `DatasetConsumptionConfig` `Estimator` `inputs` parametre için izin `Estimator` verilmez geçiş zaman `EstimatorStep`.
  + **azureml-sdk**
    + Azureml-sdk paketine AutoML istemcisi eklendi ve uzaktan AutoML çalıştırmalarının tam AutoML paketini yüklemeden gönderilmesini sağladı.
  + **azureml-train-automl-client**
    + Otomatik ml çalıştırmaları için konsol çıkışında düzeltilmiş hizalama
    + Pandaların yanlış sürümünün uzak amlcompute yüklenebileceği bir hata düzeltildi.

## <a name="2019-12-23"></a>2019-12-23

### <a name="azure-machine-learning-sdk-for-python-v1081"></a>Python v1.0.81 için Azure Machine Learning SDK

+ **Hata düzeltmeleri ve iyileştirmeler**
  + **azureml-contrib-interpret**
    + shap bağımlılığını azureml-yorumlamadan yorumlama-topluma erteleme
  + **azureml-core**
    + İşlem hedefi artık ilgili dağıtım config nesnelerine bir parametre olarak belirtilebilir. Bu, özellikle SDK nesnesine değil, dağıtılabilmek için gereken bilgi işlem hedefinin adıdır.
    + Model ve Hizmet nesnelerine CreatedBy bilgileri eklendi. .created_by üzerinden <var>erişilebilir
    + Docker konteynerinin HTTP bağlantı noktasını doğru şekilde ayarlamayan ContainerImage.run(), düzeltildi.
    + cli `az ml dataset register` komutu için isteğe bağlı olun `azureml-dataprep`
    + Yanlış bir `TabularDataset.to_pandas_dataframe` şekilde alternatif bir okuyucuya geri düşecek ve bir uyarı yazdıracak bir hata düzeltildi.
  + **azureml-explain-model**
    + shap bağımlılığını azureml-yorumlamadan yorumlama-topluma erteleme
  + **azureml-boru hattı-çekirdek**
    + Yerel bir `NotebookRunnerStep`dizüstü bilgisayarı, ardışık hatlar ayında bir adım olarak çalıştırmak için yeni bir boru hattı adımı eklendi.
    + Yayınlanan Pipelines, Schedules ve PipelineEndpoints için amortismana get_all işlevleri kaldırıldı
  + **azureml-train-automl-client**
    + AutoML'e giriş olarak data_script amortismanı başladı.


## <a name="2019-12-09"></a>2019-12-09

### <a name="azure-machine-learning-sdk-for-python-v1079"></a>Python v1.0.79 için Azure Machine Learning SDK

+ **Hata düzeltmeleri ve iyileştirmeler**
  + **azureml-automl-core**
    + Kaldırıldı featurizationConfig günlüğe kaydedilecek
      + Yalnızca "otomatik"/"kapalı"/"özelleştirilmiş" günlüğe kaydetmek için günlük bilgileri güncelleştirildi.
  + **azureml-automl-runtime**
    + Pandalar için destek eklendi. Seriler ve pandalar. Sütun veri türünü algılamak için kategorik. Daha önce sadece desteklenen numpy.ndarray
      + Kategorik dtype'ı doğru işlemek için ilgili kod değişiklikleri eklendi.
    + Tahmin işlevi arabirimi geliştirildi: y_pred parametresi isteğe bağlı yapıldı. -Doktrinler geliştirildi.
  + **azureml-contrib-dataset**
    + Etiketli veri kümelerinin monte edilemediğiniz bir hata düzeltildi.
  + **azureml-core**
    + Hata düzeltme `Environment.from_existing_conda_environment(name, conda_environment_name)`için . Kullanıcı, yerel ortamın tam kopyası olan bir Çevre örneği oluşturabilir
    + Zaman serisiyle ilgili Datasets `include_boundary=True` yöntemlerini varsayılan olarak değiştirildi.
  + **azureml-train-automl-client**
    + Gösteri çıktısı false olarak ayarlandığında doğrulama sonuçlarının yazdırılmadığı düzeltilmiş sorun.


## <a name="2019-11-25"></a>2019-11-25

### <a name="azure-machine-learning-sdk-for-python-v1076"></a>Python v1.0.76 için Azure Machine Learning SDK

+ **Yeni değişiklikler**
  + Azureml-Train-AutoML yükseltme sorunları
    + Azureml-train-automl>=1.0.76'ya yükseltme<1.0.76 kısmi kurulumlara neden olabilir ve bazı otomatik ml içeriak aktarımlarının başarısız lığa neden olabilir. Bunu çözmek için, 'de https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/automl_setup.cmdbulunan kurulum komut dosyasını çalıştırabilirsiniz. Veya doğrudan pip kullanıyorsanız şunları yapabilirsiniz:
      + "pip yüklemek --azureml-train-automl yükseltme"
      + "pip install --yoksayma yüklü azureml-train-automl-client"
    + veya yükseltmeden önce eski sürümü kaldırabilirsiniz
      + "pip azureml-tren-automl kaldırın"
      + "pip yüklemek azureml-tren-automl"

+ **Hata düzeltmeleri ve iyileştirmeler**
  + **azureml-automl-runtime**
    + AutoML artık ikili sınıflandırma görevleri için ortalama skaler ölçümleri hesaplarken hem doğru hem de yanlış sınıfları dikkate alacaktır.
    + AzureML-AutoML-Core'daki Makine öğrenimi ve eğitim kodunu yeni bir pakete taşıdı AzureML-AutoML-Runtime.
  + **azureml-contrib-dataset**
    + İndirme `to_pandas_dataframe` seçeneğiyle etiketlenmiş bir veri kümesini ararken, artık varolan dosyaların üzerine yazıp yazılmayacağınızı belirtebilirsiniz.
    + Arama `keep_columns` yaparken `drop_columns` veya bir zaman dizisi, etiket veya görüntü sütununun düşmesiyle sonuçlanırsa, veri kümesi için de ilgili özellikler bırakılır.
    + Nesne algılama görevi için pytorch yükleyici ile ilgili bir sorun giderildi.
  + **azureml-contrib-interpret**
    + Azureml-contrib-interpret'tan kaldırılan açıklama panosu widget'ı, interpret_community yenisine başvurmak için paket değiştirildi
    + Yorumlama topluluğunun 0.2.0'a güncelleştirilmiş sürümü
  + **azureml-core**
    + Performansını artırın. `workspace.datasets`
    + Kullanıcı adı ve parola kimlik doğrulaması kullanarak Azure SQL Database Datastore'a kaydolma özelliği eklendi
    + RunConfigurations'ı göreli yollardan yüklemek için düzeltin.
    + Arama `keep_columns` yaparken `drop_columns` veya bir zaman dizisi sütununun düşmesine neden olurken, veri kümesi için de ilgili özellikler bırakılır.
  + **azureml-interpret**
    + yorumlama topluluğunun güncelleştirilmiş sürümü 0.2.0
  + **azureml-boru hattı adımları**
    + Azure makine öğrenimi `runconfig_pipeline_params` ardışık iş adımları için belgelenmiş desteklenen değerler.
  + **azureml-boru hattı-çekirdek**
    + Pipeline komutları için json formatında çıktı indirmek için CLI seçeneği eklendi.
  + **azureml-tren-automl**
    + AzureML-Train-AutoML'yi 2 pakete, bir istemci paketi AzureML-Train-AutoML-Client'a ve ML eğitim paketine AzureML-Train-AutoML-Runtime'a bölün
  + **azureml-train-automl-client**
    + Herhangi bir makine öğrenimi bağımlılığını yerel olarak yüklemeye gerek kalmadan AutoML denemeleri göndermek için ince bir istemci eklendi.
    + Uzaktan çalıştırmalarda otomatik olarak algılanan gecikmelerin, yuvarlanan pencere boyutlarının ve maksimum ufukların sabit günlüğe kaydetmesi.
  + **azureml-train-automl-runtime**
    + İstemciden makine öğrenimini ve çalışma zamanı bileşenlerini yalıtmak için yeni bir AutoML paketi eklendi.
  + **azureml-contrib-tren-rl**
    + SDK'ya takviye öğrenme desteği eklendi.
    + RL SDK'da AmlWindowsCompute desteği eklendi.


## <a name="2019-11-11"></a>2019-11-11

### <a name="azure-machine-learning-sdk-for-python-v1074"></a>Python v1.0.74 için Azure Machine Learning SDK

  + **Önizleme özellikleri**
    + **azureml-contrib-dataset**
      + Azureml-contrib-dataset'i aktardıktan sonra, `._Labeled` etiketli bir veri kümesi oluşturmak yerine arayabilirsiniz. `Dataset.Labeled.from_json_lines`
      + İndirme `to_pandas_dataframe` seçeneğiyle etiketlenmiş bir veri kümesini ararken, artık varolan dosyaların üzerine yazıp yazılmayacağınızı belirtebilirsiniz.
      + Arama `keep_columns` yaparken `drop_columns` veya bir zaman serisi, etiket veya görüntü sütununun düşmesiyle sonuçlanırsa, veri kümesi için de ilgili özellikler bırakılır.
      + Ararken `dataset.to_torchvision()`PyTorch yükleyici ile ilgili sorunlar giderildi.

+ **Hata düzeltmeleri ve iyileştirmeler**
  + **masmavi-cli-ml**
    + Önizleme CLI modeline Model Profil oluşturma eklendi.
    + Azure Depolama'daki bozuk düzeltmeleri, AzureML CLI'nin başarısız olmasını sağlar.
    + AKS tipleri için MLC'ye Yük Dengeleyici Türü Eklendi
  + **azureml-automl-core**
    + Eksik değerlere ve birden çok taneye sahip, zaman serisindeki maksimal ufkun algılanması yla ilgili sorun giderildi.
    + Çapraz doğrulama bölmeleri oluşturma sırasında başarısızlıklarla ilgili sorun giderildi.
    + Sürüm notlarında görünmesi için bu bölümü işaretleme biçiminde bir iletiyle değiştirin: -Tahmin veri kümelerinde kısa tanelerin daha iyi işlenmesi.
    + Günlüğe kaydetme sırasında bazı kullanıcı bilgilerinin maskeleme sorunu giderildi. -Tahmin çalıştırmaları sırasında hataların geliştirilmiş günlüğe kaydetme.
    + Otomatik oluşturulan yml dağıtım dosyasına conda bağımlılığı olarak psutil ekleme.
  + **azureml-contrib-mir**
    + Azure Depolama'daki bozuk düzeltmeleri, AzureML CLI'nin başarısız olmasını sağlar.
  + **azureml-core**
    + Azure İşlevler'de dağıtılan modellerin 500'ler oluşturmasına neden olan bir hatayı düzeltir.
    + Anlık görüntülerde amlignore dosyasının uygulanmadığı bir sorun düzeltildi.
    + Belirli bir amlcompute üzerinde çalışan ve sıralı çalışır için bir jeneratör döndürür yeni bir API amlcompute.get_active_runs eklendi.
    + AKS tipleri için MLC'ye Yük Dengeleyici Türü eklendi.
    + run.py ve artifacts_client'de download_artifacts_from_prefix download_files append_prefix bool parametresi eklendi. Bu bayrak, yalnızca dosya veya klasör adının output_directory
    + Dataset kullanımı yla `run_config.yml` ilgili deserialization sorununu düzeltin.
    + Arama `keep_columns` yaparken `drop_columns` veya bir zaman serisi sütununun düşmesine neden olurken, veri kümesi için de ilgili özellikler bırakılır.
  + **azureml-interpret**
    + 0.1.0.3 için güncelleştirilmiş yorumlama topluluk sürümü
  + **azureml-tren-automl**
    + doğrulama sorunlarını automl_step yazdırılmayabileceği bir sorun giderildi.
    + Modelin ortamı yerel bağımlılıklar eksik olsa bile başarılı olmak için register_model düzeltildi.
    + Bazı uzaktan çalıştırmaların docker etkin olmadığı bir sorun giderildi.
    + Yerel bir çalıştırmanın zamanından önce başarısız olması nedeniyle özel durum günlüğe kaydedin.
  + **azureml-train-core**
    + Otomatik hiperparametre atoklama en iyi alt çalışır hesaplamasında resume_from çalışır düşünün.
  + **azureml-boru hattı-çekirdek**
    + Boru hattı bağımsız değişkeni yapımında sabit parametre işleme.
    + Boru hattı açıklaması ve adım türü yaml parametresi eklendi.
    + Pipeline adımı için yeni yaml biçimi ve eski biçim için amortisman uyarısı eklendi.



## <a name="2019-11-04"></a>2019-11-04

### <a name="web-experience"></a>Web deneyimi

İşbirlikçi çalışma alanı [https://ml.azure.com](https://ml.azure.com) açılış sayfası geliştirildi ve Azure Machine Learning stüdyosu (önizleme) olarak yeniden markalandı.

Stüdyodan veri kümeleri, boru hatları, modeller, uç noktalar ve daha fazlası gibi Azure Machine Learning varlıklarını eğitebilir, sınayabilir, dağıtabilir ve yönetebilirsiniz.

Stüdyodan aşağıdaki web tabanlı yazma araçlarına erişin:

| Web tabanlı araç | Açıklama | Sürüm |
|-|-|-|
| Notebook VM(önizleme) | Tam yönetilen bulut tabanlı iş istasyonu | Temel & İşletmesi |
| [Otomatik makine öğrenimi](tutorial-first-experiment-automated-ml.md) (önizleme) | Makine öğrenimi modeli geliştirmeyi otomatikleştirmek için kod deneyimi yok | Enterprise |
| [Tasarımcı](concept-designer.md) (önizleme) | Sürükle ve bırak makine öğrenme modelleme aracı eski tasarımcı olarak bilinen | Enterprise |


### <a name="azure-machine-learning-designer-enhancements"></a>Azure Machine Learning tasarımcı geliştirmeleri

+ Eskiden görsel arabirim olarak bilinen 
+    Özellik mühendisliği, çapraz doğrulama ve veri dönüşümü de dahil olmak üzere tavsiye [ediciler, sınıflandırıcılar](algorithm-module-reference/module-reference.md) ve eğitim yardımcı programları dahil olmak üzere 11 yeni modül.

### <a name="r-sdk"></a>R SDK 
 
Veri bilimciler ve Yapay Yazılım geliştiricileri, Azure Machine Learning ile makine öğrenimi iş akışları oluşturmak ve çalıştırmak [için R için Azure Machine Learning SDK'yı](tutorial-1st-r-experiment.md) kullanır.

R için Azure Machine Learning `reticulate` SDK, Python SDK'ya bağlamak için paketi kullanır. Doğrudan Python'a bağlanarak, R için SDK seçtiğiniz herhangi bir R ortamından Python SDK'da uygulanan temel nesnelere ve yöntemlere erişmenizi sağlar.

SDK'nın ana yetenekleri şunlardır:

+    Makine öğrenmesi denemelerinizi izlemek, günlüğe kaydetmek ve düzenlemek için bulut kaynaklarını yönetme.
+    GPU hızlandırılmış model eğitimi de dahil olmak üzere bulut kaynaklarını kullanan modelleri eğitin.
+    Modellerinizi Azure Kapsayıcı Örnekleri (ACI) ve Azure Kubernetes Hizmeti'nde (AKS) web hizmetleri olarak dağıtın.

Tam belgeler için [paket web sitesine](https://azure.github.io/azureml-sdk-for-r) bakın.

### <a name="azure-machine-learning-integration-with-event-grid"></a>Olay Izgara ile Azure Machine Learning entegrasyonu 

Azure Machine Learning artık Olay Izgara'sı için bir kaynak sağlayıcısıdır, makine öğrenimi etkinliklerini Azure portalı veya Azure CLI aracılığıyla yapılandırabilirsiniz. Kullanıcılar, çalıştırma tamamlama, model kaydı, model dağıtımı ve algılanan veri kayması için olaylar oluşturabilir. Bu olaylar, tüketim için Olay Ağıt tarafından desteklenen olay işleyicilerine yönlendirilebilir. Daha fazla bilgi için makine öğrenimi olay [şeması,](https://docs.microsoft.com/azure/event-grid/event-schema-machine-learning) [kavramlar](https://docs.microsoft.com/azure/machine-learning/concept-event-grid-integration) ve [öğretici](https://docs.microsoft.com/azure/machine-learning/how-to-use-event-grid) makalelere bakın.

## <a name="2019-10-31"></a>2019-10-31

### <a name="azure-machine-learning-sdk-for-python-v1072"></a>Python v1.0.72 için Azure Machine Learning SDK

+ **Yeni özellikler**
  + [**Azureml-datadrift**](https://docs.microsoft.com/python/api/azureml-datadrift) paketi aracılığıyla eklenen veri kümesi monitörleri, zaman içinde veri kayması veya diğer istatistiksel değişiklikler için zaman serisi veri kümelerini izlemeolanağı sağlar. Sürüklenme algılanırsa veya verilerdeki diğer koşullar karşılanırsa uyarılar ve olaylar tetiklenebilir. Ayrıntılar için [belgelerimize](https://aka.ms/datadrift) bakın.
  + Azure Machine Learning'de iki yeni sürümü (birbirinin yerine SKU olarak da adlandırılır) duyurmak. Bu sürümle artık bir Temel veya Kurumsal Azure Makine Öğrenimi çalışma alanı oluşturabilirsiniz. Varolan tüm çalışma alanları Temel sürümüne varsayılan olarak verilir ve çalışma alanını istediğiniz zaman yükseltmek için Azure portalına veya stüdyoya gidebilirsiniz. Azure portalından Temel veya Kurumsal çalışma alanı oluşturabilirsiniz. Daha fazla bilgi edinmek için lütfen [belgelerimizi](https://docs.microsoft.com/azure/machine-learning/how-to-manage-workspace) okuyun. SDK'dan, çalışma alanınızın sürümü çalışma alanı nesnenizin "sku" özelliği kullanılarak belirlenebilir.
  + Azure Machine Learning Compute'da da geliştirmeler yaptık - artık hata ayıklama için Tanılama günlüklerini görüntülemenin yanı sıra Azure Monitor'da kümelerinizin ölçümlerini (toplam düğümler, çalışan düğümler, toplam çekirdek kotası gibi) görüntüleyebilirsiniz. Ayrıca, kümenizde şu anda çalışan veya sıralanmış çalıştırmaları ve kümenizdeki çeşitli düğümlerin IP'leri gibi ayrıntıları da görüntüleyebilirsiniz. Bunları portalda veya SDK veya CLI'deki ilgili işlevleri kullanarak görüntüleyebilirsiniz.

  + **Önizleme özellikleri**
    + Azure Machine Learning Compute'da yerel SSD'nizin disk şifrelemesi için önizleme desteği siliyoruz. Aboneliğinizin bu özelliği kullanmak için beyaz listeye alınması için lütfen bir teknik destek bileti yükseltin.
    + Azure Machine Learning Toplu Çıkarım Genel Önizleme. Azure Machine Learning Toplu Çıkarım, zamana duyarlı olmayan büyük çıkarım işlerini hedefler. Toplu Çıkarım, eşzamanlı uygulamalar için benzersiz bir iş ortası ile uygun maliyetli çıkarım hesaplama ölçekleme sağlar. Büyük veri koleksiyonları üzerinde yüksek iş, yangın ve unut çıkarımı için optimize edinilir.
    + [**azureml-contrib-dataset**](https://docs.microsoft.com/python/api/azureml-contrib-dataset)
        + Etiketli veri kümesi için etkin işlevler
        ```Python
        import azureml.core
        from azureml.core import Workspace, Datastore, Dataset
        import azureml.contrib.dataset
        from azureml.contrib.dataset import FileHandlingOption, LabeledDatasetTask

        # create a labeled dataset by passing in your JSON lines file
        dataset = Dataset._Labeled.from_json_lines(datastore.path('path/to/file.jsonl'), LabeledDatasetTask.IMAGE_CLASSIFICATION)

        # download or mount the files in the `image_url` column
        dataset.download()
        dataset.mount()

        # get a pandas dataframe
        from azureml.data.dataset_type_definitions import FileHandlingOption
        dataset.to_pandas_dataframe(FileHandlingOption.DOWNLOAD)
        dataset.to_pandas_dataframe(FileHandlingOption.MOUNT)

        # get a Torchvision dataset
        dataset.to_torchvision()
        ```

+ **Hata düzeltmeleri ve iyileştirmeler**
  + **masmavi-cli-ml**
    + CLI artık model paketlemeyi destekliyor.
    + CLI veri seti eklendi. Daha fazla bilgi için:`az ml dataset --help`
    + InferenceConfig örneği olmadan dağıtım ve paketleme destekli modeller (ONNX, scikit-learn ve TensorFlow) için ek destek.
    + SDK ve CLI'de hizmet dağıtımı (ACI ve AKS) için üzerine yazı bayrağı eklendi. Sağlandığı takdirde, ad içeren hizmet zaten varsa, varolan hizmetin üzerine yazılır. Hizmet yoksa, yeni hizmet oluşturur.
    + Modeller iki yeni çerçeve, Onnx ve Tensorflow ile kaydedilebilir. - Model kaydı örnek giriş verilerini, örnek çıktı verilerini ve model için kaynak yapılandırmasını kabul eder.
  + **azureml-automl-core**
    + Bir yinelemenin eğitimi, yalnızca çalışma zamanı kısıtlamaları ayarlandığında bir alt işlemde çalışır.
    + Belirli bir max_horizon verilen makinede bellek sorununa neden olup olmayacağını kontrol etmek için, tahmin görevleri için bir korkuluk eklendi. Olacaksa, bir korkuluk iletisi görüntülenir.
    + 2 yıl 1 ay gibi karmaşık frekanslar için destek eklendi. -Frekans belirlenemezse anlaşılır hata iletisi eklendi.
    + Model dağıtım hatasını çözmek için otomatik oluşturulan conda env'ye azureml varsayılanları ekleme
    + Azure Machine Learning Pipeline'daki ara verilerin tabular veri kümesine dönüştürülmesine ve 'de `AutoMLStep`kullanılmasına izin ver.
    + Akış için uygulanan sütun amacı güncelleştirmesi.
    + Akış için Imputer ve HashOneHotEncoder için transformatör parametre güncellemesi uygulanmıştır.
    + Doğrulama hatası iletilerine geçerli veri boyutu ve gerekli minimum veri boyutu eklendi.
    + Her doğrulama kıvrımında en az iki örnek sağlamak için Çapraz doğrulama için gereken minimum veri boyutu güncelleştirildi.
  + **azureml-cli-yaygın**
    + CLI artık model paketlemeyi destekliyor.
    + Modeller iki yeni çerçeve, Onnx ve Tensorflow ile kaydedilebilir.
    + Model kaydı örnek giriş verilerini, örnek çıktı verilerini ve model için kaynak yapılandırmasını kabul eder.
  + **azureml-contrib-gbdt**
    + not defteri için yayın kanalı düzeltildi
    + Desteklemediğimiz AmlCompute olmayan işlem hedefi için bir uyarı eklendi
    + Azureml-contrib-gbdt paketine LightGMB Tahmincisi eklendi
  + [**azureml-core**](https://docs.microsoft.com/python/api/azureml-core)
    + CLI artık model paketlemeyi destekliyor.
    + Amortismana alınan Dataset API'leri için amortisman uyarısı ekleyin. Bkz. Dataset API https://aka.ms/tabular-datasetdeğişiklik bildirimi .
    + Veri [`Dataset.get_by_id`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset%28class%29#get-by-id-workspace--id-) kümesi kayıtlıysa, kayıt adını ve sürümünü döndürmek için değiştirin.
    + Bağımsız değişken olarak veri kümesi ile ScriptRunConfig deneme çalıştırmak göndermek için tekrar tekrar kullanılamaz bir hata düzeltmek.
    + Bir çalıştırma sırasında alınan veri kümeleri izlenir ve çalışma ayrıntıları sayfasında [`run.get_details()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29#get-details--) veya çalıştırma tamamlandıktan sonra arayarak görülebilir.
    + Azure Machine Learning Pipeline'daki ara verilerin tabular veri kümesine dönüştürülmesine ve 'de [`AutoMLStep`](/python/api/azureml-train-automl-runtime/azureml.train.automl.runtime.automlstep)kullanılmasına izin ver.
    + InferenceConfig örneği olmadan dağıtım ve paketleme destekli modeller (ONNX, scikit-learn ve TensorFlow) için ek destek.
    + SDK ve CLI'de hizmet dağıtımı (ACI ve AKS) için üzerine yazı bayrağı eklendi. Sağlandığı takdirde, ad içeren hizmet zaten varsa, varolan hizmetin üzerine yazılır. Hizmet yoksa, yeni hizmet oluşturur.
    +  Modeller iki yeni çerçeve, Onnx ve Tensorflow ile kaydedilebilir. Model kaydı örnek giriş verilerini, örnek çıktı verilerini ve model için kaynak yapılandırmasını kabul eder.
    + MySQL için Azure Veritabanı için yeni veri deposu eklendi. Azure Machine Learning Pipelines'da DataTransferStep'te MySQL için Azure Veritabanı'nı kullanmak için örnek eklendi.
    + Denemelerden etiket eklemek ve kaldırmak için işlevsellik eklendi Etiketleri çalıştırmalardan kaldırmak için işlevsellik eklendi
    + SDK ve CLI'de hizmet dağıtımı (ACI ve AKS) için üzerine yazı bayrağı eklendi. Sağlandığı takdirde, ad içeren hizmet zaten varsa, varolan hizmetin üzerine yazılır. Hizmet yoksa, yeni hizmet oluşturur.
  + [**azureml-datadrift**](https://docs.microsoft.com/python/api/azureml-datadrift)
    + Taşınmış `azureml-contrib-datadrift` içine`azureml-datadrift`
    + Sürüklenme ve diğer istatistiksel önlemler için zaman serisi veri kümelerini izlemek için ek destek
    + Yeni `create_from_model()` yöntemler `create_from_dataset()` ve [`DataDriftDetector`](https://docs.microsoft.com/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector(class)) sınıfa. Yöntem `create()` amortismana alınacaktır.
    + Azure Machine Learning stüdyosunda Python ve UI'deki görselleştirmelerde ayarlamalar.
    + Veri seti monitörleri için günlük verilere ek olarak haftalık ve aylık monitör planlamasını destekleyin.
    + Veri kümesi monitörleri için geçmiş verileri çözümlemek için veri izleme ölçümlerinin geri dolgusu destekleyin.
    + Çeşitli hata düzeltmeleri
  + [**azureml-boru hattı-çekirdek**](https://docs.microsoft.com/python/api/azureml-pipeline-core)
    + azureml-dataprep artık bir Azure Machine Learning Pipeline çalıştırın `yaml` için gerekli değildir.
  + [**azureml-tren-automl**](/python/api/azureml-train-automl-runtime/)
    + Model dağıtım hatasını çözmek için otomatik oluşturulan conda env'ye azureml varsayılanları ekleme
    + AutoML uzaktan eğitim şimdi çıkarım için eğitim env yeniden izin vermek için azureml varsayılaniçerir.
  + **azureml-train-core**
    + TahmincipyTorch [`PyTorch`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch) 1.3 desteği eklendi

## <a name="2019-10-21"></a>2019-10-21

### <a name="visual-interface-preview"></a>Görsel arayüz (önizleme)

+ Azure Machine Learning görsel arabirimi (önizleme) [Azure Machine Learning ardışık hatlarında](concept-ml-pipelines.md)çalışacak şekilde elden geçirildi. Görsel arabirimde yazılmış boru hatları (daha önce deneme olarak bilinir) artık temel Azure Machine Learning deneyimiyle tamamen entegre edilmiştir.
  + SDK varlıklarıyla birleşik yönetim deneyimi
  + Görsel arabirim modelleri, ardışık hatlar ve uç noktalar için sürüm ve izleme
  + Yeniden tasarlanan kullanıcı Arabirimi
  + Toplu çıkarım dağıtımı eklendi
  + Çıkarım bilgi işlem hedefleri için Azure Kubernetes Hizmeti (AKS) desteği eklendi
  + Yeni Python adım lı ardışık hatlar iş akışı yazma
  + Görsel yazma araçları için yeni [açılış sayfası](https://ml.azure.com)

+ **Yeni modüller**
  + Matematik işlemi uygula
  + SQL dönüşümü uygula
  + Klip değerleri
  + Verileri özetle
  + SQL veritabanından alma

## <a name="2019-10-14"></a>2019-10-14

### <a name="azure-machine-learning-sdk-for-python-v1069"></a>Python v1.0.69 için Azure Machine Learning SDK

+ **Hata düzeltmeleri ve iyileştirmeler**
  + **azureml-automl-core**
    + Model açıklamalarını her çalışma için hesaplama açıklamaları yerine en iyi çalışmayla sınırlama. Bu davranışı yerel, uzak ve ADB için değiştirme yapma.
    + UI için isteğe bağlı model açıklamaları için destek eklendi
    + Bir bağımlılık olarak psutil `automl` eklendi ve amlcompute bir conda bağımlılığı olarak psutil dahil.
    + Tahmin verilerindeki sezgisel gecikmeler ve yuvarlanma pencere boyutlarıyla ilgili sorun düzeltildi, bazı seriler lineer cebir hatalarına neden olabilir
      + Tahmin çalıştırmalarında heuristically belirlenen parametreler için çıktı eklendi.
  + **azureml-contrib-datadrift**
    + Veri kümesi düzeyi kayması ilk bölümde değilse çıktı ölçümleri oluştururken koruma eklendi.
  + **azureml-contrib-interpret**
    + azureml-contrib-explain-model paketi azureml-contrib-interpret olarak yeniden adlandırıldı
  + **azureml-core**
    + Veri kümelerini kayıt dışı kullanmak için API eklendi. `dataset.unregister_all_versions()`
    + azureml-contrib-explain-model paketi azureml-contrib-interpret olarak yeniden adlandırılmıştır.
  + **[azureml-core](https://docs.microsoft.com/python/api/azureml-core)**
    + Veri kümelerini kayıt dışı kullanmak için API eklendi. Dataset. [unregister_all_versions()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.abstract_datastore.abstractdatastore#unregister--).
    + Değiştirilen veriyi denetlemek için Dataset API eklendi. `dataset.data_changed_time`.
    + Azure Machine `FileDataset` Learning `TabularDataset` Pipeline'da `PythonScriptStep` `EstimatorStep`ve `HyperDriveStep` "Azure Machine Learning Pipeline"da ve girdiler olarak tüketebilme
    + `FileDataset.mount` Çok sayıda dosyaiçeren klasörler için performansın performansı artırıldı
    + Azure Machine Learning Pipeline'da [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep), [EstimatorStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.estimatorstep)ve [HyperDriveStep'e](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.hyperdrivestep) girdi olarak [FileDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.filedataset) ve [TabularDataset'i](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset) kullanabilmek.
    + FileDataset'in performansı. [mount()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.filedataset#mount-mount-point-none----kwargs-) çok sayıda dosyaiçeren klasörler için geliştirilmiştir
    + Çalışma ayrıntılarında bilinen hata önerilerine URL eklendi.
    + Çalışan bir hata düzeltildi.get_metrics bir çalıştırmada çok fazla çocuk varsa isteklerin başarısız olacağı get_metrics
    + Çalışan bir hata [düzeltildi.get_metrics](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run#get-metrics-name-none--recursive-false--run-type-none--populate-false-) bir çalıştırmada çok fazla çocuk varsa isteklerin başarısız olacağı bir hata düzeltildi
    + Arcadia kümesinde kimlik doğrulama desteği eklendi.
    + Deneme nesnesi oluşturma, çalışma geçmişi izleme için Azure Machine Learning çalışma alanında denemeyi alır veya oluşturur. Deneme kimliği ve arşivlenmiş zaman, oluşturma üzerindeki Deneme nesnesinde doldurulur. Örnek: deneme = Deneme = Deneme (çalışma alanı, "Yeni Deneme") experiment_id = experiment.id arşiv() ve yeniden etkinleştirme() bir denemede, denemenin UX'de gösterilmesini gizlemek ve geri yüklemek için çağrılan işlevlerdir veya denemeleri listeleme çağrısında varsayılan olarak döndürülebilir. Arşivlenmiş bir denemeyle aynı ada sahip yeni bir deneme oluşturulursa, arşivlenmiş denemeyi yeni bir ad geçirerek yeniden adlandırabilirsiniz. Belirli bir adla yalnızca bir etkin deneme olabilir. Örnek: deneme1 = Deneme(çalışma alanı, "Etkin Deneme") experiment1.archive() # Arşivle aynı adla yeni etkin deneme oluşturun. deney2. = Deneme(çalışma alanı, "Etkin Deneme") experiment1.reactivate(new_name="Önceki Etkin Deneme") Deneme'deki statik yöntem listesi() bir ad filtresi ve ViewType filtresi alabilir. ViewType değerleri "ACTIVE_ONLY", "ARCHIVED_ONLY" ve "ALL" Örnek: archived_experiments = Experiment.list(çalışma alanı, view_type="ARCHIVED_ONLY") all_first_experiments = Experiment.list(çalışma alanı, name="İlk Deneme", view_type="ALL")
    + Model dağıtma ve hizmet güncelleştirmesi için ortamı kullanma desteği
  + **azureml-datadrift**
    + DataDriftDector sınıfının gösteri özelliği artık isteğe bağlı bağımsız değişken 'with_details' desteklemez. Gösteri özniteliği yalnızca veri sürüklenme katsayısını ve özellik sütunlarının veri kayması katkısını sunacaktır.
    + DataDriftDetector özniteliği 'get_output' davranış değişiklikleri:
      + Giriş parametresi start_time, end_time zorunlu yerine isteğe bağlıdır;
      + Aynı belirli bir run_id belirli bir start_time ve/veya end_time girişi, karşılıklı olarak münhasır olduğu için değer hatası istisnası ile sonuçlanır
      + Girişe özgü start_time ve/veya end_time, yalnızca zamanlanmış çalıştırmaların sonuçları döndürülür;
      + Parametre 'daily_latest_only' amortismana sokulmadı.
    + Dataset tabanlı Data Drift çıktılarını alma desteği.
  + **azureml-explain-model**
    + AzureML açık-model paketini AzureML yorumlamaya yeniden adlandırır ve eski paketi şimdilik geriye dönük uyumluluk için saklar
    + açıklamaIstemciden karşıdan yüklemede varsayılan olarak regresyon yerine sınıflandırma görevi için ayarlanmış ham açıklamalar ile sabit `automl` hata
    + Doğrudan kullanılarak `ScoringExplainer` oluşturulacak destek ekleme`MimicWrapper`
  + **azureml-boru hattı-çekirdek**
    + Büyük Boru Hattı oluşturma için geliştirilmiş performans
  + **azureml-train-core**
    + TensorFlow Tahmincisi'nde TensorFlow 2.0 desteği eklendi
  + **azureml-tren-automl**
    + [Deneme](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment) nesnesi oluşturma, çalışma geçmişi izleme için Azure Machine Learning çalışma alanında denemeyi alır veya oluşturur. Deneme kimliği ve arşivlenmiş zaman, oluşturma üzerindeki Deneme nesnesinde doldurulur. Örnek:

        ```py
        experiment = Experiment(workspace, "New Experiment")
        experiment_id = experiment.id
        ```
        [arşiv()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment#archive--) ve [yeniden etkinleştirme()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment#reactivate-new-name-none-) bir denemede UX'de gösterilmesini gizlemek ve geri yüklemek için çağrılan veya denemeleri listeleme çağrısında varsayılan olarak döndürülen işlevlerdir. Arşivlenmiş bir denemeyle aynı ada sahip yeni bir deneme oluşturulursa, arşivlenmiş denemeyi yeni bir ad geçirerek yeniden adlandırabilirsiniz. Belirli bir adla yalnızca bir etkin deneme olabilir. Örnek:

        ```py
        experiment1 = Experiment(workspace, "Active Experiment")
        experiment1.archive()
        # Create new active experiment with the same name as the archived.
        experiment2 = Experiment(workspace, "Active Experiment")
        experiment1.reactivate(new_name="Previous Active Experiment")
        ```
        Deneme'deki statik yöntem [listesi()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment#list-workspace--experiment-name-none--view-type--activeonly---tags-none-) bir ad filtresi ve ViewType filtresi alabilir. ViewType değerleri "ACTIVE_ONLY", "ARCHIVED_ONLY" ve "ALL" dır. Örnek:

        ```py
        archived_experiments = Experiment.list(workspace, view_type="ARCHIVED_ONLY")
        all_first_experiments = Experiment.list(workspace, name="First Experiment", view_type="ALL")
        ```
    + Model dağıtma ve hizmet güncelleştirmesi için ortamı kullanma desteği.
  + **[azureml-datadrift](https://docs.microsoft.com/python/api/azureml-datadrift)**
    + [DataDriftDetector](https://docs.microsoft.com/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector.datadriftdetector) sınıfının gösteri özelliği artık isteğe bağlı bağımsız değişken 'with_details' desteklemez. Gösteri özniteliği yalnızca veri sürüklenme katsayısını ve özellik sütunlarının veri kayması katkısını sunacaktır.
    + DataDriftDetector fonksiyonu [get_output]https://docs.microsoft.com/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector.datadriftdetector#get-output-start-time-none--end-time-none--run-id-none-) davranış değişiklikleri:
      + Giriş parametresi start_time, end_time zorunlu yerine isteğe bağlıdır;
      + Aynı run_id belirli bir run_id belirli bir start_time ve/veya end_time girişi, birbirini dışladığı için değer hatası istisnası ile sonuçlanır;
      + Girişe özgü start_time ve/veya end_time, yalnızca zamanlanmış çalıştırmaların sonuçları döndürülür;
      + Parametre 'daily_latest_only' amortismana sokulmadı.
    + Dataset tabanlı Data Drift çıktılarını alma desteği.
  + **azureml-explain-model**
    + AzureML açık-model paketini AzureML yorumlamaya yeniden adlandırır ve eski paketi şimdilik geriye dönük uyumluluk için saklar.
    + AçıklamaClient'dan karşıdan yüklemede varsayılan olarak regresyon yerine sınıflandırma görevine ayarlanmış ham açıklamalarlı AutoML hatasını düzeltti.
    + Doğrudan MimicWrapper kullanılarak oluşturulacak [ScoringExplainer](/python/api/azureml-interpret/azureml.interpret.scoring.scoring_explainer.scoringexplainer?view=azure-ml-py) için destek ekleyin
  + **[azureml-boru hattı-çekirdek](https://docs.microsoft.com/python/api/azureml-pipeline-core)**
    + Büyük Pipeline oluşturma için geliştirilmiş performans.
  + **[azureml-train-core](https://docs.microsoft.com/python/api/azureml-train-core)**
    + TensorFlow Tahmincisi'nde [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow) 2.0 desteği eklendi.
  + **[azureml-tren-automl](/python/api/azureml-train-automl-runtime/)**
    + Düzenleme zaten icabına baktığından, ana çalıştırma artık kurulum yinelemesi başarısız olduğunda başarısız olmaz.
    + AutoML denemeleri için yerel docker ve yerel-conda desteği eklendi
    + AutoML denemeleri için yerel docker ve yerel-conda desteği eklendi.


## <a name="2019-10-08"></a>2019-10-08

### <a name="new-web-experience-preview-for-azure-machine-learning-workspaces"></a>Azure Machine Learning çalışma alanları için yeni web deneyimi (önizleme)

Veri bilimcilerin deneyleri daha performant bir şekilde izleyebileceği şekilde [yeni çalışma alanı portalındaki](https://ml.azure.com) Deney sekmesi güncellendi. Aşağıdaki özellikleri keşfedebilirsiniz:
+ Deneme listenizi kolayca filtrelemek ve sıralamak için meta verileri deneme
+ Koşularınızı görselleştirmenize ve karşılaştırmanıza olanak tanıyan basitleştirilmiş ve performanslı deneme ayrıntıları sayfaları
+ Eğitim çalışmalarınızı anlamak ve izlemek için ayrıntıları sayfaları çalıştırmak için yeni tasarım

## <a name="2019-09-30"></a>2019-09-30

### <a name="azure-machine-learning-sdk-for-python-v1065"></a>Python v1.0.65 için Azure Machine Learning SDK

  + **Yeni özellikler**
    + Küratörlü ortamlar eklendi. Bu ortamlar, yaygın makine öğrenimi görevleri için kitaplıklarla önceden yapılandırılmışve daha hızlı yürütme için Docker görüntüleri olarak önbelleğe alınmıştır. Bunlar varsayılan olarak Çalışma Alanı'nın ortam listesinde "AzureML" önekiyle görünür.
    + Küratörlü ortamlar eklendi. Bu ortamlar, yaygın makine öğrenimi görevleri için kitaplıklarla önceden yapılandırılmışve daha hızlı yürütme için Docker görüntüleri olarak önbelleğe alınmıştır. Varsayılan olarak [Workspace'in](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace%28class%29)ortam listesinde "AzureML" önekiyle görünürler.

  + **azureml-tren-automl**
  + **[azureml-tren-automl](/python/api/azureml-train-automl-runtime/)**
    + ADB ve HDI için ONNX dönüşüm desteği eklendi

+ **Önizleme özellikleri**
  + **azureml-tren-automl**
  + **[azureml-tren-automl](/python/api/azureml-train-automl-runtime/)**
    + Metin featurizer olarak desteklenen BERT ve BiLSTM (önizleme sadece)
    + Sütun amacı ve transformatör parametreleri için desteklenen featurization özelleştirme (yalnızca önizleme)
    + Kullanıcı eğitim sırasında model açıklamasını etkinleştirdiğinde desteklenen ham açıklamalar (yalnızca önizleme)
    + Eğitilebilir `timeseries` bir boru hattı olarak tahmin için Peygamber eklendi (önizleme sadece)

  + **azureml-contrib-datadrift**
    + Azureml-contrib-datadrift'ten azureml-datadrift'e taşınan paketler; `contrib` paket gelecekteki bir sürümde kaldırılacak

+ **Hata düzeltmeleri ve iyileştirmeler**
  + **azureml-automl-core**
    + AutoMLConfig ve AutoMLBaseSettings için FeaturizationConfig tanıtıldı
    + [AutoMLConfig](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig) ve AutoMLBaseSettings için FeaturizationConfig tanıtıldı
      + Verilen sütun ve özellik türü ile Featurization için Sütun Amacı Geçersiz Kılma
      + Geçersiz kılma transformatör parametreleri
    + explain_model() ve retrieve_model_explanations() için amortisman iletisi eklendi
    + Eğitilebilir bir boru hattı olarak Peygamber eklendi (sadece önizleme)
    + explain_model() ve retrieve_model_explanations() için amortisman iletisi eklendi.
    + Eğitilebilir bir boru hattı olarak Peygamber eklendi (sadece önizleme).
    + Hedef gecikmelerin otomatik olarak algılanması, yuvarlanma penceresi boyutu ve maksimum ufuk için destek eklendi. target_lags, target_rolling_window_size veya max_horizon biri 'otomatik' olarak ayarlanmışsa, eğitim verilerine dayalı olarak karşılık gelen parametrenin değerini tahmin etmek için buluşsal uygulama uygulanır.
    + Veri kümesi bir tane sütunu içerdiğinde, bu tane sayısal bir türde olduğunda ve tren ve test kümesi arasında bir boşluk olduğunda sabit tahmin
    + Tahmin görevlerinde uzak çalıştırmada yinelenen dizin le ilgili hata iletisi düzeltildi
    + Veri kümesi bir tane sütunu içerdiğinde, bu tane sayısal bir türde olduğunda ve tren ve test kümesi arasında bir boşluk olduğunda, durum tahmini düzeltilir.
    + Tahmin görevlerinde uzak çalıştırmada yinelenen dizin hakkındaki hata iletisi düzeltildi.
    + Veri kümesinin dengesiz olup olmadığını kontrol etmek için bir korkuluk eklendi. Bu ysa, konsola bir korkuluk iletisi yazılır.
  + **azureml-core**
    + Model nesnesi aracılığıyla depolama alanında modele SAS URL'si alma özelliği eklendi. Örn: model.get_sas_url()
    + Gönderilen `run.get_details()['datasets']` çalıştırmayla ilişkili veri kümelerini almak için tanıtın
    + JSON `Dataset.Tabular.from_json_lines_files` Lines dosyalarından bir TabularDataset oluşturmak için API ekleyin. TabularDataset'teki JSON Lines dosyalarındaki bu tabular https://aka.ms/azureml-data veriler hakkında bilgi edinmek için lütfen belgeler için ziyaret edin.
    + supported_vmsizes () işlevine ek VM boyutu alanları (OS Disk, GPU sayısı) eklendi
    + Çalıştırmayı, özel ve genel IP'yi, bağlantı noktasını vb. göstermek için list_nodes () işlevine ek alanlar eklendi.
    + Küme sağlama sırasında yeni bir alan belirtebilme -remotelogin_port_public_access küme oluşturma sırasında SSH bağlantı noktasını açık veya kapalı bırakmak isteyip istediğinize bağlı olarak etkin veya devre dışı bırakılabilir. Belirtmezseniz, kümeyi bir VNet içinde dağıtıp dağıtmadığınıza bağlı olarak hizmet bağlantı noktasını akıllıca açar veya kapatır.
  + **azureml-explain-model**
  + **[azureml-core](https://docs.microsoft.com/python/api/azureml-core/azureml.core)**
    + Model nesnesi aracılığıyla depolama alanında modele SAS URL'si alma özelliği eklendi. Örn: model. [get_sas_url()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model#get-sas-urls--)
    + Çalıştır'ı tanıtın. [gönderilen](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29#get-details--)çalıştırmayla ilişkili veri kümelerini almak için get_details ['datasets']
    + API `Dataset.Tabular`ekleyin. json lines dosyalarından bir TabularDataset oluşturmak için [from_json_lines_files()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory#from-json-lines-files-path--validate-true--include-path-false--set-column-types-none--partition-format-none-) TabularDataset'teki JSON Lines dosyalarındaki bu tabular https://aka.ms/azureml-data veriler hakkında bilgi edinmek için lütfen belgeler için ziyaret edin.
    + [supported_vmsizes()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#supported-vmsizes-workspace--location-none-) işlevine ek VM boyutu alanları (OS Disk, GPU sayısı) eklendi
    + Çalıştırmayı, özel ve genel IP'yi, bağlantı noktasını vb. göstermek için [list_nodes()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#list-nodes--) işlevine ek alanlar eklendi.
    + Küme oluşturma sırasında SSH bağlantı noktasını açık veya kapalı bırakmak isteyip istediğinize bağlı olarak etkin veya devre dışı bırakılabilir küme [sağlama](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#provisioning-configuration-vm-size-----vm-priority--dedicated---min-nodes-0--max-nodes-none--idle-seconds-before-scaledown-none--admin-username-none--admin-user-password-none--admin-user-ssh-key-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--tags-none--description-none--remote-login-port-public-access--notspecified--) `--remotelogin_port_public_access` sırasında yeni bir alan belirtme yeteneği. Belirtmezseniz, kümeyi bir VNet içinde dağıtıp dağıtmadığınıza bağlı olarak hizmet bağlantı noktasını akıllıca açar veya kapatır.
  + **azureml-explain-model**
    + Sınıflandırma senaryosundaki Açıklama çıktıları için geliştirilmiş belgeler.
    + Değerlendirme örneklerinin açıklamasına öngörülen y değerlerini yükleme özelliği eklendi. Daha kullanışlı görselleştirmelerin kilidini açar.
    + Altta yatan MimicExplainer'ı almak için MimicWrapper'a açıklayıcı özelliği eklendi.
  + **azureml-boru hattı-çekirdek**
    + Modül, ModuleVersion ve ModuleStep'i tanımlamak için not defteri eklendi
  + **azureml-boru hattı adımları**
    + AML ardışık boru hattı üzerinden R komut dosyası çalıştırmak desteklemek için RScriptStep eklendi.
    + AzureBatchStep'te ayrıştırma olan ve "parametre SubscriptionId için atama belirtilmedi" hata iletisine neden olan meta veri parametreleri düzeltildi.
  + **azureml-tren-automl**
    + Desteklenen training_data, validation_data, label_column_name, veri giriş biçimi olarak weight_column_name
    + explain_model() ve retrieve_model_explanations() için amortisman iletisi eklendi
  + **[azureml-boru hattı-çekirdek](https://docs.microsoft.com/python/api/azureml-pipeline-core)**
    + [Modül, ModuleVersion](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.module(class))ve [ModuleVersion](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.moduleversion) [ModuleStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.modulestep)tanımlamak için bir [not defteri](https://aka.ms/pl-modulestep) eklendi.
  + **[azureml-boru hattı adımları](https://docs.microsoft.com/python/api/azureml-pipeline-steps)**
    + AML ardışık boru hattı üzerinden R komut dosyası çalıştırmak desteklemek için [RScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.rscriptstep) eklendi.
    + [AzureBatchStep'te](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.azurebatchstep) ayrıştırma olan ve "parametre SubscriptionId için atama belirtilmedi" hata iletisine neden olan meta veri parametreleri düzeltildi.
  + **[azureml-tren-automl](/python/api/azureml-train-automl-runtime/)**
    + Desteklenen training_data, validation_data, label_column_name, veri giriş biçimi olarak weight_column_name.
    + [explain_model()](/python/api/azureml-train-automl-runtime/azureml.train.automl.runtime.automlexplainer#explain-model-fitted-model--x-train--x-test--best-run-none--features-none--y-train-none----kwargs-) ve [retrieve_model_explanations()](/python/api/azureml-train-automl-runtime/azureml.train.automl.runtime.automlexplainer#retrieve-model-explanation-child-run-)için amortisman iletisi eklendi.


## <a name="2019-09-16"></a>2019-09-16

### <a name="azure-machine-learning-sdk-for-python-v1062"></a>Python v1.0.62 için Azure Machine Learning SDK

+ **Yeni özellikler**
  + TabularDataset'teki `timeseries` özellik tanıtıldı. Bu özellik, tüm verileri belirli bir zaman aralığı veya en son veriler arasında almak gibi bir TabularDataset verisine kolay zaman damgası filtreleme olanağı sağlar. TabularDataset'teki `timeseries` özelliği öğrenmek için lütfen https://aka.ms/azureml-data belgeler veya https://aka.ms/azureml-tsd-notebook örnek bir not defteri için ziyaret edin.
  + TabularDataset ve FileDataset ile etkin leştirilmiş eğitim. Lütfen https://aka.ms/dataset-tutorial örnek bir not defteri için ziyaret edin.

  + **azureml-train-core**
      + PyTorch tahmincisi eklendi `Nccl` ve `Gloo` destek

+ **Hata düzeltmeleri ve iyileştirmeler**
  + **azureml-automl-core**
    + AutoML ayarını 'lag_length' ve LaggingTransformer'ı devre demledi.
    + Veri akışı biçiminde belirtilmişse giriş verilerinin düzeltilmiş doğru doğru geçerliliği
    + Grafik json oluşturmak ve eserleryüklemek için fit_pipeline.py değiştirilmiştir.
    + Grafiği kullanarak `Cytoscape`altında `userrun` işledi.
  + **azureml-core**
    + ADB kodundaki özel durum işlemeyi yeniden gözden geçirin ve yeni hata işlemeye göre değişiklik yapın
    + Notebook VM'leri için otomatik MSI kimlik doğrulaması eklendi.
    + Başarısız yeniden denemeler nedeniyle bozuk veya boş modellerin yüklenebileceği hatayı düzeltir.
    + `DataReference` Mod değiştiğinde `DataReference` adın değiştiği hata düzeltildi (örn. `as_download`, `as_mount` `as_upload`veya )
    + Yapmak `mount_point` `target_path` ve `FileDataset.mount` isteğe bağlı ve `FileDataset.download`.
    + Zaman damgası sütununun bulunamaması, zaman serileri ile ilgili API'nin ince zaman damgası sütunu atanmadan veya atanan zaman damgası sütunları düşürülmeden çağrılması durumunda atılır.
    + Zaman serileri sütunları türü Date olan sütunla atanmalıdır, aksi takdirde özel durum beklenmektedir
    + API 'with_timestamp_columns' atayan zaman serileri sütunları, daha önce atanan zaman damgası sütunlarını temizleyen Hiçbir değer ince/kaba zaman damgası sütun adını alabilir.
    + Kaba tane veya ince taneli zaman damgası sütunu, damlalama listesinde zaman damgası sütunu hariç tutulduktan sonra veya zaman damgası sütunlarını serbest bırakmak için Hiçbiri değeri olan with_time_stamp aradıktan sonra bırakılabileceğini gösteren bir göstergeyle kullanıcıya bırakıldığında özel durum ortadan fırlacaktır.
    + Kaba tane veya ince taneli zaman damgası sütunu, kullanıcı için tutmanın sütun listesinde zaman damgası sütunu veya zaman damgası sütunlarını serbest bırakmak için Hiçbiri değeri olan with_time_stamp araması sonrasında yapılabileceğini gösteren bir göstergeyle sütun tutma listesine dahil edilmediğinde istisna ortadan fırlacaktır.
    + Kayıtlı bir modelin boyutu için günlük eklendi.
  + **azureml-explain-model**
    + "Ambalaj" python paketi yüklenmediği zaman konsola yazdırılan uyarı düzeltildi: "Lightgbm'in desteklenen sürümünden daha eski sürümü kullanarak, lütfen 2.2.1'den büyük sürüme yükseltin"
    + Birçok özelliğe sahip küresel açıklamalar için parçalama ile sabit indirme modeli açıklaması
    + Çıktı açıklamasında eksik başlatma örnekleri düzeltildi mimik açıklayıcısı
    + İki farklı model türü kullanarak açıklama istemcisi ile yükleme yaparken ayarlanan özelliklerde sabit hata düzeltildi
    + Puanlama açıklayıcısına get_raw bir param eklendi .explain() böylece bir puanlama açıklayıcısı hem tasarlanmış hem de ham değerleri döndürebilir.
  + **azureml-tren-automl**
    + SDK açıklamalarını desteklemek için AutoML'den `automl` kamu API'leri tanıtıldı - AutoML featurization'ı ayırarak Ve SDK'yı açıklayarak AutoML açıklamalarını desteklemenin yeni yolu - Azureml'dan entegre ham açıklama desteği, AutoML modelleri için SDK'yı açıklar.
    + Azureml varsayılanlarını uzak eğitim ortamlarından kaldırma.
    + Azure Databricks kod yolunda AutoML için AzureFileCacheStore'a dosya önbelleği deposu konumunu FileCacheStore tabanlı bir konum değiştirildi.
    + Veri akışı biçiminde belirtilmişse giriş verilerinin düzeltilmiş doğru doğru geçerliliği
  + **azureml-train-core**
    + Amortisman source_directory_data_store geri döndürüldü.
    + Azureml yüklü paket sürümlerini geçersiz kılma özelliği eklendi.
    + Tahmincilerde parametreye `environment_definition` dockerfile desteği eklendi.
    + Tahmincilerde basitleştirilmiş dağıtılmış eğitim parametreleri.

         ```py
        from azureml.train.dnn import TensorFlow, Mpi, ParameterServer
        ```

## <a name="2019-09-09"></a>2019-09-09

### <a name="new-web-experience-preview-for-azure-machine-learning-workspaces"></a>Azure Machine Learning çalışma alanları için yeni web deneyimi (önizleme)
Yeni web deneyimi, veri bilimcilerin ve veri mühendislerinin verileri hazırlama ve görselleştirmeden modelleri tek bir konumda eğitmeye ve dağıtmaya kadar uçtan uca makine öğrenimi yaşam döngüsünü tamamlamalarına olanak tanır.

![Azure Machine Learning çalışma alanı arabirimi (önizleme)](./media/azure-machine-learning-release-notes/new-ui-for-workspaces.jpg)

**Temel özellikler:**

Bu yeni Azure Machine Learning arabirimini kullanarak şunları yapabilirsiniz:
+ Not defterlerinizi yönetin veya Jupyter'a bağlantı kurun
+ [Otomatik ML deneylerini çalıştırın](tutorial-first-experiment-automated-ml.md)
+ [Yerel dosyalardan, veri depolarından, & web dosyalarından veri kümeleri oluşturma](how-to-create-register-datasets.md)
+ Model oluşturma için veri kümelerini & keşfetme
+ Modelleriniz için veri sürüklenmelerini izleme
+ Panodaki en son kaynakları görüntüleme

Bu sürüm sırasında aşağıdaki tarayıcılar desteklenir: Chrome, Firefox, Safari ve Microsoft Edge Preview.

**Bilinen sorunlar:**

1. "Bir şeyler ters gitti! Dağıtım devam ederken yığın dosyaları yükleme hatası" olur.

1. Not Defterleri ve Dosyalar'da dosyayı silemez veya yeniden adlandıramaz. Genel Önizleme sırasında güncelleştirme dosya işlemleri gerçekleştirmek için Not Defteri VM'deki Jupyter UI veya Terminal'i kullanabilirsiniz. Monte edilmiş bir ağ dosya sistemi olduğundan, Notebook VM'de yaptığınız tüm değişiklikler hemen Not Defteri Çalışma Alanına yansıtılır.

1. Notebook VM içine SSH için:
   1. VM kurulumu sırasında oluşturulan SSH anahtarlarını bulun. Veya, Azure Machine Learning çalışma alanında anahtarları bulun > açık Bilgi İşlem sekmesi> not defteri VM'i listede bulun > özelliklerini açın: anahtarları iletişim kutusundan kopyalayın.
   1. Bu ortak ve özel SSH anahtarlarını yerel makinenize aktarın.
   1. Bunları SSH için Notebook VM'de kullanın.

## <a name="2019-09-03"></a>2019-09-03
### <a name="azure-machine-learning-sdk-for-python-v1060"></a>Python v1.0.60 için Azure Machine Learning SDK

+ **Yeni özellikler**
  + Veri depolarınızda veya genel url'lerinizde tek veya birden çok dosyaya atıfta bulunan FileDataset tanıtıldı. Dosyalar herhangi bir biçimde olabilir. FileDataset, dosyaları bilgi işleminize indirme veya takma olanağı sağlar. FileDataset hakkında bilgi edinmek https://aka.ms/file-datasetiçin lütfen .
  + PythonScript Adımı, Adla Adım, Databricks Adım, DataTransferStep ve AzureBatch Adımı için Pipeline Yaml Desteği eklendi

+ **Hata düzeltmeleri ve iyileştirmeler**
  + **azureml-automl-core**
    + AutoArima artık yalnızca önizleme için önerilebilir bir boru hattıdır.
    + Tahmin için geliştirilmiş hata raporlaması.
    + Tahmin görevlerinde genel yerine özel özel durumlar kullanarak günlüğe kaydetmeyi geliştirin.
    + Toplam yineleme sayısından daha az olması max_concurrent_iterations üzerindeki denetim kaldırıldı.
    + AutoML modelleri artık AutoMLExceptions dönmek
    + Bu sürüm, otomatik makine öğrenme yerel çalıştırmalarının yürütme performansını artırır.
  + **azureml-core**
    + Bir sözlük `TabularDataset` ve `FileDataset` kayıt adlarına göre anahtarlanmış nesnelerin döndürüldişini sağlayan Dataset.get_all(çalışma alanı) ile tanıyın.

    ```py
    workspace = Workspace.from_config()
    all_datasets = Dataset.get_all(workspace)
    mydata = all_datasets['my-data']
    ```

    + Argüman `parition_format` olarak `Dataset.Tabular.from_delimited_files` tanıtın ve `Dataset.Tabular.from_parquet.files`. Her veri yolunun bölüm bilgileri belirtilen biçime göre sütunlara ayıklanır. '{column_name}' dize sütunu oluşturur ve '{column_name:yyyy/MM/dd/HH/mm/ss}, 'yyyy', 'MM', 'dd', 'HH', 'mm' ve 'ss' tarih türü için yıl, ay, gün, saat, dakika ve ikinci ayıklamak için kullanılır tarih sütunu oluşturur. partition_format, ilk bölüm anahtarının konumundan dosya yolunun sonuna kadar başlamalıdır. Örneğin, yol verilen '... /USA/2019/01/01/data.csv' bölümü ülke ve zamana göre olduğu yerde, partition_format='/{Country}/{PartitionDate:yyyy/MM/dd}/data.csv' değeri 'ABD' ve '2019-01-01' değerine sahip tarih sütunu 'Ülke' dize sütunu 'Ülke' oluşturur.
        ```py
        workspace = Workspace.from_config()
        all_datasets = Dataset.get_all(workspace)
        mydata = all_datasets['my-data']
        ```

    + Argüman `partition_format` olarak `Dataset.Tabular.from_delimited_files` tanıtın ve `Dataset.Tabular.from_parquet.files`. Her veri yolunun bölüm bilgileri belirtilen biçime göre sütunlara ayıklanır. '{column_name}' dize sütunu oluşturur ve '{column_name:yyyy/MM/dd/HH/mm/ss}, 'yyyy', 'MM', 'dd', 'HH', 'mm' ve 'ss' tarih türü için yıl, ay, gün, saat, dakika ve ikinci ayıklamak için kullanılır tarih sütunu oluşturur. partition_format, ilk bölüm anahtarının konumundan dosya yolunun sonuna kadar başlamalıdır. Örneğin, yol verilen '... /USA/2019/01/01/data.csv' bölümü ülke ve zamana göre olduğu yerde, partition_format='/{Country}/{PartitionDate:yyyy/MM/dd}/data.csv' değeri 'ABD' ve '2019-01-01' değerine sahip tarih sütunu 'Ülke' dize sütunu 'Ülke' oluşturur.
    + `to_csv_files`ve `to_parquet_files` yöntemler `TabularDataset`eklenmiştir. Bu yöntemler, verileri `TabularDataset` belirtilen `FileDataset` biçimdeki dosyalara dönüştürerek a ve a arasında dönüştürmeyi sağlar.
    + Model.package() tarafından oluşturulan bir Dockerfile kaydederken temel görüntü kayıt defterine otomatik olarak giriş yapın.
    + 'gpu_support' artık gerekli değildir; AML artık kullanılabilir olduğunda nvidia docker uzantısını otomatik olarak algılar ve kullanır. İleride yayınlanacak bir sürümde kaldırılır.
    + PipelineDrafts oluşturmak, güncelleştirmek ve kullanmak için destek eklendi.
    + Bu sürüm, otomatik makine öğrenme yerel çalıştırmalarının yürütme performansını artırır.
    + Kullanıcılar, run geçmişinden ölçümleri ada göre sorgulayabilir.
    + Tahmin görevlerinde genel yerine özel özel durumlar kullanarak günlüğe kaydetmeyi geliştirin.
  + **azureml-explain-model**
    + Yeni MimicWrapper'a feature_maps parametresi eklendi ve kullanıcıların ham özellik açıklamaları almasına olanak sağladı.
    + Dataset yüklemeleri artık açıklama yüklemesi için varsayılan olarak kapatılır ve upload_datasets=True ile yeniden etkinleştirilebilir
    + Açıklama listesi ve indirme işlevlerine "is_law" filtreleme parametreleri eklendi.
    + Hem `get_raw_explanation(feature_maps)` genel hem de yerel açıklama nesnelerine yöntem ekler.
    + Aşağıda desteklenen sürüm varsa yazdırılmış uyarı ile lightgbm için sürüm denetimi eklendi
    + Açıklamaları toplu işerken en iyi duruma getirilmiş bellek kullanımı
    + AutoML modelleri artık AutoMLExceptions dönmek
  + **azureml-boru hattı-çekirdek**
    + PipelineDrafts oluşturmak, güncelleştirmek ve kullanmak için eklenen destek - mutable boru hattı tanımlarını korumak ve bunları çalıştırmak için etkileşimli olarak kullanmak için kullanılabilir
  + **azureml-tren-automl**
    + GPU özellikli pytorch v1.1.0, :::no-loc text="cuda"::: toolkit 9.0, pytorch-transformers'ın belirli sürümlerini yüklemek için oluşturulan özellik, uzak python çalışma zamanı ortamında BERT/ XLNet'i etkinleştirmek için gereklidir.
  + **azureml-train-core**
    + Bazı hiperparametre uzay tanımı hatalarının sunucu tarafı yerine doğrudan sdk'da erken arızası.

### <a name="azure-machine-learning-data-prep-sdk-v1114"></a>Azure Machine Learning Data Hazırlık SDK v1.1.14
+ **Hata düzeltmeleri ve iyileştirmeler**
  + Ham yol ve kimlik bilgilerini kullanarak ADLS/ADLSGen2'ye yazma özelliği etkinleştirildi.
  + Çalışmama `include_path=True` nedeni olan bir `read_parquet`hata düzeltildi.
  + Özel `to_pandas_dataframe()` durum "Geçersiz özellik değeri: hostSecret" nedeniyle arıza düzeltildi.
  + Spark modunda DBFS'de dosyaların okunamadığını zedilen bir hata düzeltildi.

## <a name="2019-08-19"></a>2019-08-19

### <a name="azure-machine-learning-sdk-for-python-v1057"></a>Python v1.0.57 için Azure Machine Learning SDK
+ **Yeni özellikler**
  + AutomatedML tarafından tüketilmesi etkinleştirildi. `TabularDataset` Hakkında `TabularDataset`daha fazla bilgi https://aka.ms/azureml/howto/createdatasetsiçin, lütfen ziyaret edin.

+ **Hata düzeltmeleri ve iyileştirmeler**
  + **automl-istemci-çekirdek-nativeclient**
    + Eğitim ve/veya doğrulama etiketleri (y ve y_valid) pandalar veri çerçevesi biçiminde sağlandığında, ancak sayısal dizi olarak değil, yükseltilen hata düzeltildi.
    + Yalnızca veri ve `RawDataContext` `AutoMLBaseSettings` nesne gerektiren a oluşturmak için güncelleştirilmiş arabirim.
    +  AutoML kullanıcılarının tahmin yaparken yeterince uzun olmayan eğitim serilerini bırakmasına izin verin. - AutoML kullanıcılarının tahmin yaparken eğitim kümesinde bulunmayan test kümesinden taneler bırakmasına izin verin.
  + **masmavi-cli-ml**
    + Artık HEM Microsoft tarafından oluşturulan microsoft hem de müşteri sertifikası için AKS kümesinde dağıtılan puanlama bitiş noktası için TLS/SSL sertifikasını güncelleştirebilirsiniz.
  + **azureml-automl-core**
    + Otomatik ML'de, eksik etiketleri olan satırların düzgün şekilde kaldırılmadıkları bir sorun giderildi.
    + AutoML'de geliştirilmiş hata günlüğü; tam hata iletileri artık her zaman günlük dosyasına yazılır.
    + AutoML, paket sabitlemesini `azureml-defaults`, `azureml-explain-model`ve `azureml-dataprep`. AutoML artık paket uyuşmazlıkları konusunda `azureml-train-automl` (paket hariç) uyarmayacaktır.
    + Cv bölmelerinin `timeseries` eşit olmayan boyutta olduğu ve depo alanı hesaplamasının başarısız olduğu bir sorun giderildi.
    + Cross-Validation eğitim türü için topluluk yinelemesini çalıştırırken, tüm veri seti üzerinde eğitilmiş modelleri indirmekte sorun yaşıyorsak, model ağırlıkları ile oylama topluluğuna beslenen modeller arasında bir tutarsızlık yaşıyorduk.
    + Eğitim ve/veya doğrulama etiketleri (y ve y_valid) pandalar veri çerçevesi biçiminde sağlandığında, ancak sayısal dizi olarak değil, yükseltilen hata düzeltildi.
    + Giriş tablolarının Boolean sütunlarında Hiçbiri ile karşılaşıldığında tahmin görevleriyle ilgili sorun giderildi.
    + AutoML kullanıcılarının tahmin yaparken yeterince uzun olmayan eğitim serilerini bırakmasına izin verin. - AutoML kullanıcılarının tahmin yaparken eğitim kümesinde bulunmayan test kümesinden taneler bırakmasına izin verin.
  + **azureml-core**
    + blob_cache_timeout parametre siparişi ile ilgili sorun düzeltildi.
    + Sistem hatalarına dış sığdırmazlık ve dönüştürme özel durum türleri eklendi.
    + Uzaktan çalıştırmalar için Key Vault sırları için destek eklendi. Çalışma alanınızla ilişkili keyvault'tan sırları eklemek, almak ve listelemek için azureml.core.keyvault.Keyvault sınıfını ekleyin. Desteklenen işlemler şunlardır:
      + azureml.core.workspace.Workspace.get_default_keyvault()
      + azureml.core.keyvault.Keyvault.set_secret(isim, değer)
      + azureml.core.keyvault.Keyvault.set_secrets(secrets_dict)
      + azureml.core.keyvault.Keyvault.get_secret(isim)
      + azureml.core.keyvault.Keyvault.get_secrets(secrets_list)
      + azureml.core.keyvault.Keyvault.list_secrets()
    + Varsayılan keyvault'u elde etmek ve uzaktan çalıştırma sırasında sır almak için ek yöntemler:
      + azureml.core.workspace.Workspace.get_default_keyvault()
      + azureml.core.run.Run.get_secret(isim)
      + azureml.core.run.Run.get_secrets(secrets_list)
    + CLI komutunu göndermek için ek geçersiz kılma parametreleri eklendi.
    + API çağrılarının güvenilirliğini artırın, yeniden denemeleri ortak istekkitaplık durumlarına genişletiyor.
    + Gönderilen bir çalıştırmadan çalıştırma gönderme için destek ekleyin.
    + FileWatcher'da süresi dolan SAS belirteci sorunu düzeltildi ve bu da dosyaların ilk belirteçlerinin süresi dolduktan sonra yüklenmenin durdurulmasına neden oldu.
    + Dataset python SDK'da HTTP csv/tsv dosyalarının içe aktarılması desteklenir.
    + Workspace.setup() yöntemini devre-yitirme. Kullanıcılara gösterilen uyarı iletisi, bunun yerine create() veya get()/from_config() kullanarak önerilmektedir.
    + Özel özel python paketlerinin `whl`çalışma alanına yüklenmesini ve ortamı oluşturmak/somutlaştırmak için güvenli bir şekilde kullanılmasını sağlayan Environment.add_private_pip_wheel() eklendi.
    + Artık HEM Microsoft tarafından oluşturulan microsoft hem de müşteri sertifikası için AKS kümesinde dağıtılan puanlama bitiş noktası için TLS/SSL sertifikasını güncelleştirebilirsiniz.
  + **azureml-explain-model**
    + Yükleme deki açıklamalara model kimliği eklemek için parametre eklendi.
    + Bellekve yükleme açıklamalarına etiketleme eklendi. `is_raw`
    + Azureml-explain-model paketi için pytorch desteği ve testler eklendi.
  + **azureml-opendatasets**
    + Otomatik test ortamını algılamayı ve günlüğe kaydetmeyi destekleyin.
    + İlçe ve zip tarafından ABD nüfus almak için sınıflar eklendi.
  + **azureml-boru hattı-çekirdek**
    + Giriş ve çıkış bağlantı noktası tanımlarına etiket özelliği eklendi.
  + **azureml-telemetry**
    + Yanlış bir telemetri yapılandırması düzeltildi.
  + **azureml-tren-automl**
    + Hata, kurulum çalışması için "hatalar" alanında günlüğe kaydedilmedi ve bu nedenle üst çalıştırma "hataları" depolanmadı hata düzeltildi.
    + Otomatik ML'de, eksik etiketleri olan satırların düzgün şekilde kaldırılmadıkları bir sorun giderildi.
    + AutoML kullanıcılarının tahmin yaparken yeterince uzun olmayan eğitim serilerini bırakmasına izin verin.
    + AutoML kullanıcılarının tahmin yaparken eğitim kümesinde bulunmayan taneleri test kümesinden bırakmasına izin verin.
    + Şimdi AutoMLStep `automl` değişiklikler veya yeni config parametreleri eklemeler herhangi bir sorunları önlemek için arka uç config geçer.
    + AutoML Data Guardrail artık genel önizlemede. Kullanıcı eğitimden sonra bir Veri Koruma raporu (sınıflandırma/regresyon görevleri için) görecek ve ayrıca SDK API üzerinden erişebilecektir.
  + **azureml-train-core**
    + PyTorch Estimator meşale 1.2 desteği eklendi.
  + **azureml-widgets**
    + Sınıflandırma eğitimi için geliştirilmiş karışıklık matris grafikleri.

### <a name="azure-machine-learning-data-prep-sdk-v1112"></a>Azure Machine Learning Data Hazırlık SDK v1.1.12
+ **Yeni özellikler**
  + Dizeleri listeleri artık yöntemlere `read_*` giriş olarak geçirilebilir.

+ **Hata düzeltmeleri ve iyileştirmeler**
  + Spark'ta çalışırken performansı `read_parquet` önemli ölçüde artırıldı.
  + Belirsiz tarih `column_type_builder` biçimlerine sahip tek bir sütun durumunda başarısız olunan bir sorun giderildi.

### <a name="azure-portal"></a>Azure portal
+ **Önizleme Özelliği**
  + Günlük ve çıktı dosyası akışı artık çalışma ayrıntıları sayfaları için kullanılabilir. Önizleme geçişi açık olduğunda dosyalar güncelleştirmeleri gerçek zamanlı olarak aktarır.
  + Kotayı çalışma alanı düzeyinde ayarlama yeteneği önizlemede yayımlanır. AmlCompute kotaları abonelik düzeyinde ayrılmıştır, ancak şimdi bu kotayı çalışma alanları arasında dağıtmanıza ve adil paylaşım ve yönetim için ayırmanıza izin veririz. Çalışma alanınızın sol navigasyon çubuğundaki **Kullanımlar+Kotalar** çubuğuna tıklamanız ve **Kotaları Yapılandır** makarasını seçmeniz yeterlidir.

## <a name="2019-08-05"></a>2019-08-05

### <a name="azure-machine-learning-sdk-for-python-v1055"></a>Python v1.0.55 için Azure Machine Learning SDK

+ **Yeni özellikler**
  + Token tabanlı kimlik doğrulaması artık AKS'de dağıtılan puanlama bitiş noktasına yapılan aramalar için desteklenir. Geçerli anahtar tabanlı kimlik doğrulamasını desteklemeye devam edeceğiz ve kullanıcılar bu kimlik doğrulama mekanizmalarından birini aynı anda kullanabilir.
  + Sanal ağın (VNet) arkasındaki blob depolamayı veri deposu olarak kaydetme yeteneği.

+ **Hata düzeltmeleri ve iyileştirmeler**
  + **azureml-automl-core**
    + CV bölmeleri için doğrulama boyutunun küçük olduğu ve gerileme ve tahmin için kötü tahmin edilen gerçek grafiklere karşı sonuçlar doğurduğu bir hatayı düzeltir.
    + Uzak çalıştırmalarda tahmin görevlerinin günlüğe kaydolması iyileştirildi, şimdi kullanıcıya çalışma başarısız olduysa kapsamlı hata iletisi sağlandı.
    + İşlem öncesi `Timeseries` bayrağın Doğru olup olmadığı düzeltildi.
    + Bazı tahmin veri doğrulama hata iletileri daha işlem yapılabilir yaptı.
    + Özellikle proses yumurtlamaları arasında veri kümelerinin bırakılarak ve/veya tembelbir şekilde yüklenerek AutoML'nin daha az bellek tüketimi çalışır
  + **azureml-contrib-explain-model**
    + Kullanıcının model türü için varsayılan otomatik çıkarım mantığını geçersiz kılmasına izin vermek için açıklayıcılara model_task bayrak eklendi
    + Widget değişiklikleri: Otomatik olarak `contrib`yükler `nbextension` , artık yüklemek / etkinleştirmek - sadece küresel özellik önemi ile destek açıklaması (örneğin Permutative)
    + Pano değişiklikleri: - Özet sayfada çizime `beeswarm` ek olarak kutu çizimleri ve `beeswarm` keman çizimleri - 'Top -k' kaydırıcı değişikliğinde çizimin çok daha hızlı yeniden işlenmesi - üst-k'nin nasıl hesaplandığını açıklayan yararlı ileti - Veriler sağlanamadığında grafikler yerine yararlı özelleştirilebilir iletiler
  + **azureml-core**
    + Modelleri ve bunların bağımlılıklarını kapsülleyen Docker görüntüleri ve Docker dosyaları oluşturmak için Model.package() yöntemi eklendi.
    + Çevre nesneleri içeren InferenceConfigs'i kabul etmek için yerel web hizmetleri güncelleştirildi.
    + Sabit Model.register() geçersiz modeller üreten zaman '.' (geçerli dizin için) model_path parametresi olarak geçirilir.
    + Run.submit_child ekle, işlevler, gönderilen alt çalıştırmanın üst bölümü olarak çalıştır'ı belirtirken Deneme.submit'ü aynalar.
    + Model.register'dan Run.register_model'da yapılandırma seçeneklerini destekleyin.
    + Varolan kümede JAR işlerini çalıştırma yeteneği.
    + Şimdi instance_pool_id ve cluster_log_dbfs_path parametreleri destekliyoruz.
    + Bir Web hizmetine Model dağıtılırken Ortam nesnesi kullanmak için destek eklendi. Çevre nesnesi artık InferenceConfig nesnesinin bir parçası olarak sağlanabilir.
    + Yeni bölgeler için appinsifht haritalama ekleyin - centralus - westus - northcentralus
    + Tüm Datastore sınıflarında tüm öznitelikler için belgeler eklendi.
    + blob_cache_timeout parametresi `Datastore.register_azure_blob_container`eklendi
    + azureml.core.environment.Environment'a save_to_directory ve load_from_directory yöntemler eklendi.
    + CLI'ye "az ml environment download" ve "az ml environment register" komutları eklendi.
    + Çevre.add_private_pip_wheel yöntemi eklendi.
  + **azureml-explain-model**
    + Dataset hizmetini kullanarak Açıklamalar'a veri kümesi izleme eklendi (önizleme).
    + Genel açıklamalar 10k'tan 100'e akış yaparken varsayılan toplu iş boyutu azaltıldı.
    + Kullanıcının model türü için varsayılan otomatik çıkarım mantığını geçersiz kılmasına izin vermek için açıklayıcılara model_task bayrak eklendi.
  + **azureml-mlflow**
    + Iç içe dizinlerin yoksayıldığı mlflow.azureml.build_image'daki hata düzeltildi.
  + **azureml-boru hattı adımları**
    + Varolan Azure Veri Tuğlaları kümesinde JAR işlerini çalıştırma özelliği eklendi.
    + DatabricksStep adımı için destek instance_pool_id ve cluster_log_dbfs_path parametrelereklendi.
    + DatabricksStep adımında boru hattı parametreleri için destek eklendi.
  + **azureml-tren-automl**
    + Ensemble `docstrings` ile ilgili dosyalar için eklendi.
    + Dokümanlar için `max_cores_per_iteration` daha uygun bir dile güncelleştirildi ve`max_concurrent_iterations`
    + Uzak çalıştırmalarda tahmin görevlerinin günlüğe kaydolması iyileştirildi, şimdi kullanıcıya çalışma başarısız olduysa kapsamlı hata iletisi sağlandı.
    + Get_data boru `automlstep` hattı not defterinden kaldırıldı.
    + `dataprep` Destek `automlstep`başladı.

### <a name="azure-machine-learning-data-prep-sdk-v1110"></a>Azure Machine Learning Data Hazırlık SDK v1.1.10

+ **Yeni özellikler**
  + Artık belirli sütunlarda belirli denetçilerin (örneğin histogram, dağılım çizimi, vb.) yürütülmesini isteyebilirsiniz.
  + 'ye `append_columns`paralelleştirme bağımsız değişkeni eklendi True ise, veriler belleğe yüklenir, ancak yürütme paralel olarak çalışır; False ise, yürütme akış olacak ama tek iş parçacığı.

## <a name="2019-07-23"></a>2019-07-23

### <a name="azure-machine-learning-sdk-for-python-v1053"></a>Python v1.0.53 için Azure Machine Learning SDK

+ **Yeni özellikler**
  + Otomatik Makine Öğrenme artık uzaktan bilgi işlem hedefi üzerinde ONNX modelleri eğitim destekler
  + Azure Machine Learning artık önceki bir çalıştırma, denetim noktası veya model dosyalarından eğitime devam etme olanağı sağlar.
    + [Önceki bir çalıştırmadan eğitime devam etmek için tahmincileri](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-tensorflow-resume-training/train-tensorflow-resume-training.ipynb) nasıl kullanacağınızı öğrenin

+ **Hata düzeltmeleri ve iyileştirmeler**
  + **automl-istemci-çekirdek-nativeclient**
    + Dönüşümden sonra sütun türlerini loosing hakkında hata düzeltme (hata bağlantılı);
    + y_query başında (#459519) None(lar) içeren bir nesne türü ne kadar izin ver?
  + **masmavi-cli-ml**
    + CLI komutları "model dağıtma" ve "hizmet güncelleştirmesi" artık parametreleri, config dosyalarını veya ikisinin birleşimini kabul eder. Parametrelerin dosyalardaki özniteliklere göre önceliği vardır.
    + Model açıklaması artık kayıttan sonra güncelleştirilebilir
  + **azureml-automl-core**
    + NimbusML bağımlılığını 1.2.0 sürümüne (en son geçerli) güncelleştirin.
    + AutoML tahmincileri içinde kullanılacak NimbusML tahmincileri & boru hatları için destek ekleme.
    + Topluluk seçim prosedüründe, skorlar sabit kalsa bile gereksiz yere ortaya çıkan topluluğu büyüten bir hatayı düzeltme.
    + Tahminleri tahmin etmek için CV Bölmeleri'nde bazı başarılandırmaların yeniden kullanılmasını etkinleştirin. Bu, gecikmeler ve yuvarlanan pencereler gibi pahalı başarılar için kabaca n_cross_validations bir faktör tarafından çalıştırılanın kurulum çalışma süresini hızlandırıyor.
    + Zaman pandalar desteklenen zaman aralığı dışında ise bir sorunu giderme. Zaman pd'den azsa şimdi bir DataException yükseltiyoruz. Timestamp.min veya pd'den büyük. Zaman damgası.max
    + Tahmin şimdi onlar hizalanabilir eğer tren ve test kümelerinde farklı frekansları sağlar. Örneğin, "Üç aylık Ocak ayında başlayan" ve "üç aylık Ekim ayında başlayan" hizalanabilir.
    + Özellik "parametreleri" TimeSeriesTransformer eklendi.
    + Eski özel durum sınıflarını kaldırın.
    + Tahmin görevlerinde `target_lags` parametre artık tek bir tamsayı değerini veya tamsayılar listesini kabul eder. Sonda sağlanmışsa, yalnızca bir gecikme oluşturulur. Bir liste sağlanırsa, gecikmelerin benzersiz değerleri alınır. target_lags=[1, 2, 2, 4] bir, 2 ve 4'lük dönemlerin gecikmesi yaratacaktır.
    + Dönüşümden sonra sütun türlerini kaybetme yle ilgili hatayı düzeltme (hata bağlantılı);
    + Y_query'nin `model.forecast(X, y_query)`başlangıçta (#459519) None(lar) içeren bir nesne türü olmasını bekleyin.
    + Çıktıya `automl` beklenen değerleri ekleme
  + **azureml-contrib-datadrift**
    +  Verileri zenginleştirirken azureml-contrib-opendatasets yerine azureml opendatasets'e geçiş ve performans iyileştirmeleri gibi örnek not defterinde iyileştirmeler
  + **azureml-contrib-explain-model**
    + Azureml-contrib-explain-model paketinde ham özellik önemi için LIME açıklayıcısı için sabit dönüşümler bağımsız değişkeni
    + AzureML-contrib-explain-model paketi için görüntü açıklayıcısındaki görüntü açıklamalarına segmentasyonlar eklendi
    + LimeExplainer için scipy seyrek destek ekleyin
    + DecisionTreeExplainableModel'in yürütme süresini iyileştirmek için toplu olarak küresel açıklamalar akışı için açıklayıcıyı `batch_size` `include_local=False`taklit etmek için eklendi
  + **azureml-contrib-featureengineering**
    + arama için düzeltme set_featurizer_timeseries_params(): dikte değeri türü değişikliği `timeseries` ve null check - Featurizer için not defteri ekle
    + NimbusML bağımlılığını 1.2.0 sürümüne (en son geçerli) güncelleştirin.
  + **azureml-core**
    + AzureML CLI'ye DBFS veri depolarını ekleme özelliği eklendi
    + `target_path` Başlatıldığında boş bir klasörün oluşturulduğu datastore yüklemesi ile hata düzeltildi`/`
    + ServicePrincipalAuthentication'da sorun giderildi. `deepcopy`
    + CLI'ye "az ml çevre gösterisi" ve "az ml çevre listesi" komutları eklendi.
    + Ortamlar artık zaten oluşturulmuş bir base_image alternatif olarak bir base_dockerfile belirtme yi destekler.
    + Kullanılmayan RunConfiguration ayarı auto_prepare_environment amortismana katılmış olarak işaretlenmiştir.
    + Model açıklaması artık kayıttan sonra güncelleştirilebilir
    + Bugfix: Model ve Görüntü silme şimdi bir yukarı akış bağımlılığı nedeniyle başarısız olursa, onlara bağlı upstream nesneleri alma hakkında daha fazla bilgi sağlar.
    + Bazı ortamlar için çalışma alanı oluştururken oluşan dağıtımlar için boş süre yazdıran hata düzeltildi.
    + Geliştirilmiş çalışma alanı hata özel durumları oluşturur. Öyle ki, kullanıcılar "Çalışma alanı oluşturamıyorum. Bulamıyor..." ileti olarak ve bunun yerine gerçek oluşturma hatası bakın.
    + AKS web hizmetlerinde belirteç kimlik doğrulaması için destek ekleyin.
    + Nesnelere `get_token()` `Webservice` yöntem ekleyin.
    + Makine öğrenimi veri kümelerini yönetmek için CLI desteği eklendi.
    + `Datastore.register_azure_blob_container`şimdi isteğe `blob_cache_timeout` bağlı olarak bu veri deposu için önbellek sona ermesini etkinleştirmek için blobfuse montaj parametrelerini yapılandırAn bir değer (saniye cinsinden) alır. Varsayılan zaman aşımı değildir, yani bir blob okunduğunda, iş bitene kadar yerel önbellekte kalır. Çoğu iş bu ayarı tercih eder, ancak bazı işlerin büyük bir veri kümesinden düğümlerine sığmayacak kadar fazla veri okuması gerekir. Bu işler için, bu parametreyi alabilmeleri başarılı olmalarına yardımcı olur. Bu parametreyi ayarlarken dikkatli olmak: değeri çok düşük ayarlamak düşük performansa neden olabilir, çünkü bir çağda kullanılan verilerin yeniden kullanılmadan önce süresi dolabilir. Bu, tüm okumaların yerel önbellek yerine blob depolamadan (yani ağdan) yapılacağı anlamına gelir ve bu da eğitim sürelerini olumsuz etkiler.
    + Model açıklaması artık kayıttan sonra düzgün bir şekilde güncellenebilir
    + Model ve Görüntü silme artık silme işleminin başarısız olması nedeniyle bunlara bağlı olan yukarı akış nesneleri hakkında daha fazla bilgi sağlar
    + Azureml.mlflow kullanarak uzaktan çalıştırmaların kaynak kullanımını geliştirin.
  + **azureml-explain-model**
    + Azureml-contrib-explain-model paketinde ham özellik önemi için LIME açıklayıcısı için sabit dönüşümler bağımsız değişkeni
    + LimeExplainer için scipy seyrek destek eklemek
    + şekil lineer açıklayıcı sarıcı, hem de doğrusal modelleri açıklamak için tabular açıklayıcı başka bir düzey eklendi
    + model kitaplığında mimik açıklayıcısı için, include_local=Seyrek veri girişi için yanlış olduğunda sabit hata
    + çıktıya `automl` beklenen değerleri ekleme
    + ham özellik önemi elde etmek için sağlanan dönüşümler argümanı zaman sabit permütasyon özelliği önemi
    + ne `batch_size` zaman `include_local=False`açıklayıcı taklit eklendi , DecisionTreeExplainableModel yürütme süresini artırmak için toplu olarak küresel açıklamalar akışı için
    + model açıklayabilirlik kitaplığı için, tahmin için pandaların veri çerçevesi girişinin gerekli olduğu sabit karakutu açıklayıcıları
    + Bazen içinde `explanation.expected_values` yüzer bulunan bir liste yerine float döndürecek bir hata düzeltildi.
  + **azureml-mlflow**
    + mlflow.set_experiment(experiment_name) performansını artırmak
    + Mlflow tracking_uri için InteractiveLoginAuthentication kullanımıhata düzeltme
    + Azureml.mlflow kullanarak uzaktan çalıştırmaların kaynak kullanımını geliştirin.
    + Azureml-mlflow paketinin belgelerini geliştirin
    + Mlflow.log_artifacts("my_dir") yapılarını "<yapı-yolları>" yerine "my_dir/<artifon yolları>" altında kaydedeceği yama hatası
  + **azureml-opendatasets**
    + Yeni `pyarrow` `opendatasets` tanıtılan bellek sorunu nedeniyle eski sürümlere (<0.14.0) pin.
    + Azureml-contrib-opendatasets'i azureml-opendatasets'e taşıyın.
    + Açık veri kümesi sınıflarının Azure Machine Learning çalışma alanına kaydedilmesine izin verin ve AML Dataset özelliklerinden sorunsuz bir şekilde yararlanın.
    + NoaaIsdWeather'ı geliştirinSPARK olmayan sürümdeki performansı önemli ölçüde zenginleştirin.
  + **azureml-boru hattı adımları**
    + DBFS Datastore artık DatabricksStep'teki Giriş ler ve Çıktılar için desteklendi.
    + Giriş/çıktılarla ilgili olarak Azure Toplu İş Adımı için güncelleştirilmiş belgeler.
    + AzureBatchStep'te *delete_batch_job_after_finish* varsayılan değeri *delete_batch_job_after_finish değiştirildi.*
  + **azureml-telemetry**
    +  Azureml-contrib-opendatasets'i azureml-opendatasets'e taşıyın.
    + Açık veri kümesi sınıflarının Azure Machine Learning çalışma alanına kaydedilmesine izin verin ve AML Dataset özelliklerinden sorunsuz bir şekilde yararlanın.
    + NoaaIsdWeather'ı geliştirinSPARK olmayan sürümdeki performansı önemli ölçüde zenginleştirin.
  + **azureml-tren-automl**
    + Gerçek iade türünü yansıtmak ve önemli özellikleri alma yla ilgili ek notlar sağlamak için get_output belgeler güncelleştirildi.
    + NimbusML bağımlılığını 1.2.0 sürümüne (en son geçerli) güncelleştirin.
    + çıktıya `automl` beklenen değerleri ekleme
  + **azureml-train-core**
    + Dizeleri artık Otomatik Hiperparametre Tuning için işlem hedefi olarak kabul edilir
    + Kullanılmayan RunConfiguration ayarı auto_prepare_environment amortismana katılmış olarak işaretlenmiştir.

### <a name="azure-machine-learning-data-prep-sdk-v119"></a>Azure Machine Learning Data Hazırlık SDK v1.1.9

+ **Yeni özellikler**
  + Bir dosyayı doğrudan http veya https url'den okumak için destek eklendi.

+ **Hata düzeltmeleri ve iyileştirmeler**
  + Uzak bir kaynaktan parke veri seti okumaya çalışırken geliştirilmiş hata iletisi (şu anda desteklenmiyor).
  + ADLS Gen 2'de Parke dosyası biçimine yazarken ve yolda ADLS Gen 2 konteyner adını güncellerken bir hata düzeltildi.

## <a name="2019-07-09"></a>2019-07-09

### <a name="visual-interface"></a>Görsel Arayüz
+ **Önizleme özellikleri**
  + Görsel arabirimde "Execute R script" modülü eklendi.

### <a name="azure-machine-learning-sdk-for-python-v1048"></a>Python v1.0.48 için Azure Machine Learning SDK

+ **Yeni özellikler**
  + **azureml-opendatasets**
    + **azureml-contrib-opendatasets** artık **azureml-opendatasets**olarak kullanılabilir. Eski paket hala çalışabilir, ancak daha zengin özellikler ve iyileştirmeler için ileriye doğru hareket eden **azureml opendatasets** kullanmanızı öneririz.
    + Bu yeni paket, açık veri kümelerini Azure Machine Learning çalışma alanında Dataset olarak kaydetmenize ve Dataset'in sunduğu işlevlerden yararlanmanıza olanak tanır.
    + Ayrıca Pandas/SPARK veri çerçeveleri gibi açık veri kümelerini tüketme ve hava durumu gibi bazı veri kümeleri için konum birleştirmeleri gibi varolan yetenekleri de içerir.

+ **Önizleme özellikleri**
    + HyperDriveConfig artık bir ardışık etki hattı kullanarak hiperparametre atomasını desteklemek için bir parametre olarak boru hattı nesnesi kabul edebilir.

+ **Hata düzeltmeleri ve iyileştirmeler**
  + **azureml-tren-automl**
    + Dönüşümden sonra sütun türlerini kaybetme yle ilgili hata düzeltildi.
    + Hata, y_query başında None(lar) içeren bir nesne türü olmasını sağlayacak şekilde düzeltildi.
    + Puanlar sabit kalsa bile gereksiz yere ortaya çıkan topluluğu büyüten Ensemble seçim prosedüründeki sorun giderildi.
    + AutoMLStep'teki whitelist_models ve blacklist_models ayarlarıyla ilgili sorun giderildi.
    + AutoML Azure ML Pipelines bağlamında kullanıldığında ön işleme kullanımını engelleyen sorun giderildi.
  + **azureml-opendatasets**
    + Azureml-contrib-opendatasets'i azureml-opendatasets'e taşıdı.
    + Açık veri kümesi sınıflarının Azure Machine Learning çalışma alanına kaydedilmesine ve AML Dataset özelliklerinden sorunsuz bir şekilde yararlanmasına izin verin.
    + Geliştirilmiş NoaaIsdWeather olmayan SPARK sürümünde performansı önemli ölçüde zenginleştirmek.
  + **azureml-explain-model**
    + Yorumlanabilirlik nesneleri için güncelleştirilmiş çevrimiçi belgeler.
    + Model `batch_size` açıklayıcı kitaplığı `include_local=False`için DecisionTreeExplainableModel yürütme süresini artırmak için toplu olarak küresel açıklamalar akışı için açıklayıcı taklit etmek için eklendi.
    + Bazen içinde `explanation.expected_values` float olan bir liste yerine bir float döndürecek sorunu düzeltildi.
    + Model kitaplığında taklit açıklama için çıktıya `automl` beklenen değerler eklendi.
    + Ham özellik önemi elde etmek için dönüşümler argümanı verildiğinde sabit permütasyon özelliği önemi.
  + **azureml-core**
    + AzureML CLI'ye DBFS veri depolarına ekleme özelliği eklendi.
    + 'yle `target_path` başlanırsa `/`boş bir klasörün oluşturulduğu datastore yükleme sorunu düzeltildi
    + İki veri kümesinin karşılaştırılması etkinleştirilmiş.
    + Model ve Görüntü silme şimdi bir yukarı akış bağımlılığı nedeniyle silmek başarısız olursa onlara bağlı upstream nesneleri alma hakkında daha fazla bilgi sağlar.
    + kullanılmayan RunConfiguration ayarını auto_prepare_environment'da devre dışı.
  + **azureml-mlflow**
    + Azureml.mlflow kullanan uzaktan çalıştırmaların geliştirilmiş kaynak kullanımı.
    + Azureml-mlflow paketinin dokümantasyonu geliştirildi.
    + mlflow.log_artifacts("my_dir") yapıtları "artefakt-yollar" yerine "my_dir/artefakt-yolları" altında kaydedeceği sorun giderildi.
  + **azureml-boru hattı-çekirdek**
    + Tüm boru hattı adımları için hash_paths parametre amortismana alınır ve gelecekte kaldırılır. source_directory varsayılan içeriği haşlanır (.amlignore veya .gitignore'de listelenen dosyalar hariç)
    + Boru hatlarında bilgi işlem türüne özgü modülleri desteklemek, RunConfiguration entegrasyonu ve diğer değişikliklere hazırlanmak için Modül ve ModuleStep'i geliştirmeye devam edin.
  + **azureml-boru hattı adımları**
    + AzureBatchStep: Giriş/çıktılar la ilgili geliştirilmiş belgeler.
    + AzureBatchStep: Varsayılan değeri delete_batch_job_after_finish doğru değiştirildi.
  + **azureml-train-core**
    + Dizeleri artık Otomatik Hiperparametre Tuning için işlem hedefi olarak kabul edilir.
    + kullanılmayan RunConfiguration ayarını auto_prepare_environment'da devre dışı.
    + Amortismana alınan `conda_dependencies_file_path` parametreler `pip_requirements_file_path` ve `conda_dependencies_file` `pip_requirements_file` sırasıyla lehine.
  + **azureml-opendatasets**
    + NoaaIsdWeather'ı geliştirinSPARK olmayan sürümdeki performansı önemli ölçüde zenginleştirin.

### <a name="azure-machine-learning-data-prep-sdk-v118"></a>Azure Machine Learning Data Hazırlık SDK v1.1.8

+ **Yeni özellikler**
 + Veri akışı nesneleri artık üzerinde yinelenebilir ve bir dizi kayıt üretilebilir. Için belgelere `Dataflow.to_record_iterator`bakın.
  + Veri akışı nesneleri artık üzerinde yinelenebilir ve bir dizi kayıt üretilebilir. Için belgelere `Dataflow.to_record_iterator`bakın.

+ **Hata düzeltmeleri ve iyileştirmeler**
 + DataPrep SDK'nın sağlamlığı artırıldı.
 + Dize olmayan Sütun Dizinleri ile pandalar DataFrames geliştirilmiş işleme.
 + `to_pandas_dataframe` Datasets'teki performansı artırıldı.
 + Çok düğümlü bir ortamda çalıştırıldığında Datasets'in Spark yürütmesinin başarısız olduğu bir hata düzeltildi.
  + DataPrep SDK'nın sağlamlığı artırıldı.
  + Dize olmayan Sütun Dizinleri ile pandalar DataFrames geliştirilmiş işleme.
  + `to_pandas_dataframe` Datasets'teki performansı artırıldı.
  + Çok düğümlü bir ortamda çalıştırıldığında Datasets'in Spark yürütmesinin başarısız olduğu bir hata düzeltildi.

## <a name="2019-07-01"></a>2019-07-01

### <a name="azure-machine-learning-data-prep-sdk-v117"></a>Azure Machine Learning Data Hazırlık SDK v1.1.7

Azure Databricks kullanan bazı müşteriler için sorunlara neden olduğu için performansı artıran bir değişikliği geri aldık. Azure Databricks'te bir sorun yaşadıysanız, aşağıdaki yöntemlerden birini kullanarak sürüm 1.1.7'ye yükseltebilirsiniz:
1. Yükseltmek için bu komut dosyasını çalıştırın:`%sh /home/ubuntu/databricks/python/bin/pip install azureml-dataprep==1.1.7`
2. En son Data Prep SDK sürümünü yükleyecek olan kümeyi yeniden oluşturun.

## <a name="2019-06-25"></a>2019-06-25

### <a name="azure-machine-learning-sdk-for-python-v1045"></a>Python v1.0.45 için Azure Machine Learning SDK

+ **Yeni özellikler**
  + Azureml-explain-model paketinde açıklayıcıyı taklit etmek için karar ağacı vekil modeli ekleme
  + Inferencing görüntülerine yüklenecek bir CUDA sürümünü belirtme yeteneği. CUDA 9.0, 9.1 ve 10.0 desteği.
  + Azure ML eğitim üssü görüntüleri hakkında bilgi artık [Azure ML Containers GitHub Deposu](https://github.com/Azure/AzureML-Containers) ve [DockerHub'da](https://hub.docker.com/_/microsoft-azureml) mevcuttur
  + Boru hattı zamanlaması için CLI desteği eklendi. Daha fazla bilgi edinmek için "az ml boru hattı -h" çalıştırın
  + AKS web hizmeti dağıtım yapılandırmasına ve CLI'ye özel Kubernetes ad alanı parametresi eklendi.
  + Tüm boru hattı adımları için amortismana hash_paths parametresi
  + Model.register şimdi `child_paths` parametre kullanımı ile tek bir model olarak birden çok tek dosya kayıt destekler.

+ **Önizleme özellikleri**
    + Puanlama açıklayıcıları artık daha güvenilir serileştirme ve deserialization için isteğe bağlı olarak conda ve pip bilgilerini kaydedebilirsiniz.
    + Otomatik Özellik Seçici için Hata Düzeltme.
    + Yeni api için mlflow.azureml.build_image güncellendi, yeni uygulama tarafından maruz yamalı hatalar.

+ **Hata düzeltmeleri ve iyileştirmeler**
  + Azureml-core'dan bağımlılık kaldırıldı. `paramiko` Eski bilgi işlem hedef ekleme yöntemleri için amortisman uyarıları eklendi.
  + run.create_children performansını artırın
  + İkili sınıflandırıcılı mimik açıklayıcısında, şekil değerlerini ölçekleme için öğretmen olasılığı kullanıldığında olasılıkların sırasını düzeltin.
  + Otomatik makine öğrenimi için geliştirilmiş hata işleme ve ileti.
  + Otomatik makine öğrenimi için yineleme zaman aşım sorunu düzeltildi.
  + Otomatik makine öğrenimi için zaman serisi dönüşüm performansı artırıldı.

## <a name="2019-06-24"></a>2019-06-24

### <a name="azure-machine-learning-data-prep-sdk-v116"></a>Azure Machine Learning Data Hazırlık SDK v1.1.6

+ **Yeni özellikler**
  + Üst değerler ( )`SummaryFunction.TOPVALUES`ve alt`SummaryFunction.BOTTOMVALUES`değerler için özet işlevleri eklendi ( ). .

+ **Hata düzeltmeleri ve iyileştirmeler**
  + Önemli ölçüde performansını `read_pandas_dataframe`artırdı .
  + İkili dosyaların başarısız `get_profile()` olmasını gösteren bir Veri Akışı'nda neden olacak bir hata düzeltildi.
  + Telemetri koleksiyonunun programatik olarak etkinleştirilmesine/devre dışı bırakılmasına izin vermek için maruz kalır. `set_diagnostics_collection()`
  + Davranışını `get_profile()`değiştirdi. NaN değerleri artık Pandaların davranışıyla uyumlu olan Min, Mean, Std ve Sum için yoksayılır.


## <a name="2019-06-10"></a>2019-06-10

### <a name="azure-machine-learning-sdk-for-python-v1043"></a>Python v1.0.43 için Azure Machine Learning SDK

+ **Yeni özellikler**
  + Azure Machine Learning artık popüler makine öğrenimi ve veri analizi çerçevesi Scikit-learn için birinci sınıf destek sağlıyor. [ `SKLearn` Tahminci](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py)kullanarak, kullanıcılar scikit-learn modellerini kolayca eğitebilir ve dağıtabilirler.
    + [HyperDrive kullanarak Scikit-learn ile hiperparametre ayarı nasıl çalıştırılayarıştamayı](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-hyperparameter-tune-deploy-with-sklearn/train-hyperparameter-tune-deploy-with-sklearn.ipynb)öğrenin.
  + Yeniden kullanılabilir bilgi işlem birimlerini yönetmek için Modül ve ModuleVersion sınıfları ile birlikte boru hatlarında ModuleStep oluşturmak için destek eklendi.
  + ACI web hizmetleri artık güncelleştirmeler aracılığıyla kalıcı scoring_uri desteklemeyi destekliyor. scoring_uri IP'den FQDN'ye değişecektir. FQDN için Dns Ad Etiketi, dns_name_label deploy_configuration ayarlayarak yapılandırılabilir.
  + Otomatik makine öğrenme yeni özellikler:
    + Tahmin için STL featurizer
    + Özellik süpürme için KMeans kümeleme özelliği etkinleştirildi
  + AmlCompute Kota onayları sadece daha hızlı oldu! Kota isteklerinizi bir eşik içinde onaylamak için işlemi otomatikhale getirdik. Kotaların nasıl çalıştığı hakkında daha fazla bilgi için [kotaları nasıl yönetirecer öğrenin.](https://docs.microsoft.com/azure/machine-learning/how-to-manage-quotas)

+ **Önizleme özellikleri**
    + Azureml-mlflow paketi[(örnek dizüstü bilgisayarlar)](https://aka.ms/azureml-mlflow-examples)aracılığıyla [MLflow](https://mlflow.org) 1.0.0 izleme ile entegrasyon.
    + Jupyter not defterini bir koşu olarak gönderin. [API Başvuru Belgeleri](https://docs.microsoft.com/python/api/azureml-contrib-notebook/azureml.contrib.notebook?view=azure-ml-py)
    + Azureml-contrib-datadrift paketi[(örnek not defterleri)](https://aka.ms/azureml-datadrift-example)aracılığıyla [Veri Drift Dedektörü](https://docs.microsoft.com/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector(class)) Genel Önizlemesi . Veri Drift modeli doğruluğu zaman içinde bozulur üst nedenlerinden biridir. Üretimde modele sunulan veriler, modelin eğitildiği verilerden farklı olduğunda gerçekleşir. AML Veri Drift dedektörü, müşterinin veri sürüklenmesini izlemesine yardımcı olur ve sürüklenme algılandığında uyarı gönderir.

+ **Yeni değişiklikler**

+ **Hata düzeltmeleri ve iyileştirmeler**
  + RunConfiguration yük ve kaydet önceki davranış için tam geri uyumluluk ile tam bir dosya yolu belirten destekler.
  + ServicePrincipalAuthentication'da önbelleğe alma eklendi, varsayılan olarak kapatıldı.
  + Aynı metrik ad altında birden çok çizimin günlüğe kaydedilmesini etkinleştirin.
  + Model sınıfı artık azureml.core 'den`from azureml.core import Model`düzgün bir şekilde içe aktarılabilir ( ).
  + Boru hattı `hash_path` adımlarında parametre artık amortismana hazırdır. .amlignore veya .gitignore'de listelenen dosyalar dışında, yeni davranış, source_directory'ı tamamlamaktır.
  + Boru `get_all` hattı paketlerinde, çeşitli ve `get_all_*` yöntemler lehine `list` ve `list_*`, sırasıyla amortismana alınmıştır.
  + azureml.core.get_run artık özgün çalıştırma türünü döndürmeden önce sınıfların içe aktarılmasını gerektirmez.
  + WebService Update'e yapılan bazı çağrıların güncelleştirmeyi tetiklemediğini niçin sorun giderildi.
  + AKS web hizmetlerinde puanlama zaman ları 5ms ile 300000ms arasında olmalıdır. Max puanlama istekleri için scoring_timeout_ms izin 1 dk 5 dk çarptı.
  + LocalWebservice nesneleri artık `scoring_uri` `swagger_uri` sahip ve özellikleri.
  + Taşınmış çıktılar dizin oluşturma ve çıktılar dizin yükleme kullanıcı işleminin dışında. Her kullanıcı işleminde çalışacak sdk çalışma geçmişi etkin. Bu, dağıtılmış eğitim çalıştırmaları tarafından karşılaşılan bazı eşitleme sorunlarını gidermelidir.
  + Kullanıcı işlem adından yazılan azureml günlüğünün adı artık işlem adını (yalnızca dağıtılmış eğitim için) ve PID'yi içerir.

### <a name="azure-machine-learning-data-prep-sdk-v115"></a>Azure Machine Learning Data Hazırlık SDK v1.1.5

+ **Hata düzeltmeleri ve iyileştirmeler**
  + 2 basamaklı yıl biçimine sahip yorumlanmış datetime değerleri için, geçerli yıl aralığı Windows Mayıs Sürümü ile eşleşecek şekilde güncelleştirildi. Bu ürün 1930-2029 yılları arasında 1950-2049 olarak değiştirilmiştir.
  + Bir dosya ve `handleQuotedLineBreaks=True`ayarda `\r` okurken, yeni bir satır olarak kabul edilecektir.
  + Bazı durumlarda başarısız `read_pandas_dataframe` olan bir hata düzeltildi.
  + Geliştirilmiş performans `get_profile`.
  + Geliştirilmiş hata iletileri.

## <a name="2019-05-28"></a>2019-05-28

### <a name="azure-machine-learning-data-prep-sdk-v114"></a>Azure Machine Learning Data Hazırlık SDK v1.1.4

+ **Yeni özellikler**
  + Artık datetime değerlerini yeni sütunlara ayıklamak ve ayrıştırmak için aşağıdaki ifade dili işlevlerini kullanabilirsiniz.
    + `RegEx.extract_record()`datetime öğelerini yeni bir sütuna ayıklar.
    + `create_datetime()`ayrı tarih saati öğelerinden datetime nesneleri oluşturur.
  + Ararken, `get_profile()`artık değerlerin yaklaşık olduğunu açıkça belirtmek için nicel sütunların (est.) olarak etiketlendiğini görebilirsiniz.
  + Azure Blob Depolama'dan okurken artık ** globbing'i kullanabilirsiniz.
    + Örneğin.`dprep.read_csv(path='https://yourblob.blob.core.windows.net/yourcontainer/**/data/*.csv')`

+ **Hata düzeltmeleri**
  + Parke dosyasını uzak bir kaynaktan (Azure Blob) okumayla ilgili bir hata düzeltildi.

## <a name="2019-05-14"></a>2019-05-14

### <a name="azure-machine-learning-sdk-for-python-v1039"></a>Python v1.0.39 için Azure Machine Learning SDK
+ **Değişiklik**
  + Yapılandırmayı çalıştırma auto_prepare_environment seçeneği, otomatik olarak hazırlanarak varsayılan hale gelirken, amortismana sokuluyor.

## <a name="2019-05-08"></a>2019-05-08

### <a name="azure-machine-learning-data-prep-sdk-v113"></a>Azure Machine Learning Data Hazırlık SDK v1.1.3

+ **Yeni özellikler**
  + read_postgresql arayarak veya bir Datastore kullanarak PostgresSQL veritabanından okumak için destek eklendi.
    + Nasıl yapIlir kılavuzlarındaki örneklere bakın:
      + [Veri Alma not defteri](https://aka.ms/aml-data-prep-ingestion-nb)
      + [Datastore dizüstü bilgisayar](https://aka.ms/aml-data-prep-datastore-nb)

+ **Hata düzeltmeleri ve iyileştirmeler**
  + Sütun türü dönüştürme ile ilgili sorunlar düzeltildi:
  + Şimdi doğru bir boolean veya sayısal sütun boolean sütun dönüştürür.
  + Şimdi, tarih türü olarak bir tarih sütunu ayarlamaya çalışırken başarısız olmaz.
  + Geliştirilmiş JoinType türleri ve beraberindeki başvuru belgeleri. İki veri akışını birleştirirken, şimdi aşağıdaki türlerden birini belirtebilirsiniz:
    + YOK, MAÇ, IÇ, UNMATCHLEFT, LEFTANTI, LEFTOUTER, UNMATCHRIGHT, RIGHTANTI, RIGHTOUTER, FULLANTI, TAM.
  + Daha fazla tarih biçimini tanımak için veri türü çıkarTIsi geliştirildi.

## <a name="2019-05-06"></a>2019-05-06

### <a name="azure-portal"></a>Azure portal

Azure portalında artık şunları yapabilirsiniz:
+ Otomatik ML deneyleri oluşturma ve çalıştırma
+ Örnek Jupyter dizüstü bilgisayarlarınızı veya kendi dizüstü bilgisayarlarınızı denemek için bir Notebook VM oluşturun.
+ Otomatik Makine Öğrenimi, Görsel Arayüz ve Barındırılan Notebook VM'lerini içeren Azure Machine Learning çalışma alanında yepyeni Yazma bölümü (Önizleme)
    + Otomatik makine öğrenimini kullanarak otomatik olarak bir model oluşturun
    + Denemeleri çalıştırmak için Görsel Arabirimi sürükleyin ve bırak
    + Verileri keşfetmek, modeller oluşturmak ve hizmetleri dağıtmak için bir Notebook VM oluşturun.
+ Çalışma raporları ve çalıştırma ayrıntıları sayfalarında canlı grafik ve metrik güncelleme
+ Çalışma ayrıntıları sayfalarındagünlükler, çıktılar ve anlık görüntüler için dosya görüntüleyicigüncelleştirildi.
+ Denemeler sekmesinde yeni ve geliştirilmiş rapor oluşturma deneyimi.
+ Azure Machine Learning çalışma alanının Genel Bakış sayfasından config.json dosyasını indirme özelliği eklendi.
+ Azure Veri Tuğlaları çalışma alanından Azure Machine Learning çalışma alanı oluşturmayı destekleyin.

## <a name="2019-04-26"></a>2019-04-26

### <a name="azure-machine-learning-sdk-for-python-v1033"></a>Python v1.0.33 için Azure Machine Learning SDK
+ **Yeni özellikler**
  + _Workspace.create_ yöntemi şimdi CPU ve GPU kümeleri için varsayılan küme yapılandırmalarını kabul eder.
  + Çalışma alanı oluşturma başarısız olursa, bağlı kaynaklar temizlenir.
  + Varsayılan Azure Kapsayıcı Kayıt Defteri SKU temele geçti.
  + Azure Kapsayıcı Kayıt Defteri, çalıştırma veya görüntü oluşturma için gerektiğinde tembelce oluşturulur.
  + Eğitim için Ortamlar desteği çalışır.

### <a name="notebook-virtual-machine"></a>Notebook Sanal Makine 

Python'u kullanarak Azure Machine Learning SDK tarafından desteklenen tüm işlemleri gerçekleştirebileceğiniz, makine öğrenimi denemelerini programlayabileceğiniz, modelleri web uç noktaları olarak dağıtabileceğiniz ve Azure Machine Learning SDK tarafından desteklenen diğer tüm işlemleri gerçekleştirebileceğiniz Jupyter dizüstü bilgisayarlar için güvenli, kurumsal kullanıma hazır bir barındırma ortamı olarak Bir Notebook VM kullanın.Çeşitli özellikler sağlar:
+ [Quickly spin up a preconfigured notebook VM](tutorial-1st-experiment-sdk-setup.md) Azure Machine Learning SDK'nın ve ilgili paketlerin en son sürümüne sahip önceden yapılandırılmış bir dizüstü bilgisayar VM'sini hızla döndürün.
+ Erişim, HTTPS, Azure Active Directory kimlik doğrulaması ve yetkilendirme gibi kanıtlanmış teknolojiler aracılığıyla sağlanır.
+ Azure Machine Learning Workspace blob depolama hesabınızda dizüstü bilgisayarların ve kodların güvenilir bulut depolama sı.Çalışmanızı kaybetmeden dizüstü bilgisayarınızın VM'ini güvenle silebilirsiniz.
+ Azure Machine Learning özelliklerini keşfetmek ve denemek için önceden yüklenmiş örnek defterler.
+ Azure VM'lerin tam özelleştirme özellikleri, herhangi bir VM türü, herhangi bir paket, herhangi bir sürücü. 

## <a name="2019-04-26"></a>2019-04-26

### <a name="azure-machine-learning-sdk-for-python-v1033-released"></a>Python v1.0.33 için Azure Machine Learning SDK yayınlandı.

+ Azure ML Donanım Hızlandırılmış Modeller [FPGA'larda](how-to-deploy-fpga-web-service.md) genellikle kullanılabilir.
  + Artık [azureml-accel-models paketini şu şekilde kullanabilirsiniz:](how-to-deploy-fpga-web-service.md)
    + Desteklenen bir derin sinir ağının ağırlıklarını eğitin (ResNet 50, ResNet 152, DenseNet-121, VGG-16 ve SSD-VGG)
    + Desteklenen DNN ile transfer öğrenimini kullanma
    + Modeli Model Yönetim Hizmeti'ne kaydedin ve modeli konteynerleştirin
    + Modeli, Azure Kubernetes Hizmeti (AKS) kümesinde FPGA ile bir Azure VM'sine dağıtma
  + Kapsayıcıyı Azure [Veri Kutusu Kenarı](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview) sunucu aygıtına dağıtma
  + Bu [örnekle](https://github.com/Azure-Samples/aml-hardware-accelerated-models) gRPC bitiş noktası ile verilerinizi puanla

### <a name="automated-machine-learning"></a>Otomatik Makine Öğrenmesi

+ Performans optimizasyonu için :::no-loc text="featurizers"::: dinamik olarak eklemeyi etkinleştirmek için özellik süpürme. Yeni :::no-loc text="featurizers":::: iş katıştırma, kanıt ağırlığı, hedef kodlamalar, metin hedef kodlama, küme mesafesi
+ Akıllı CV otomatik ML içinde tren / geçerli böler işlemek için
+ Birkaç bellek optimizasyonu değişikliği ve çalışma zamanı performans geliştirme
+ Model açıklamasında performans artışı
+ Yerel çalıştırma için ONNX model dönüştürme
+ Eklenen Alt Örnekleme desteği
+ Çıkış ölçütleri tanımlanmadığında Akıllı Durdurma
+ Yığılmış topluluklar

+ Zaman Serileri Tahmini
  + Yeni tahmin tahmini fonksiyonu
  + Artık zaman serisi verilerinde yuvarlanma kökenli çapraz doğrulamayı kullanabilirsiniz
  + Zaman serilerini yapılandırmak için eklenen yeni işlevler
  + Yuvarlama penceresi toplu özelliklerini desteklemek için yeni işlevler eklendi
  + Deneme ayarlarında ülke kodu tanımlandığında yeni Tatil algılama ve featurizer

+ Azure Databricks
  + Etkin zaman serisi tahmini ve model açıklaması/yorumlanabilirlik yeteneği
  + Artık otomatik ML denemelerini iptal edebilir ve devam ettirebilirsiniz
  + Çok çekirdekli işleme için ek destek

### <a name="mlops"></a>MISPler
+ **Yerel dağıtım & hata ayıklama kapsayıcıları için**<br/> Artık bir ML modelini yerel olarak dağıtabilir ve beklendiği gibi çalıştıklarından emin olmak için puanlama dosyanıza ve bağımlılıklarınıza hızlı bir şekilde yineleyebilirsiniz.

+ **Tanıtılan InferenceConfig & Model.deploy()**<br/> Model dağıtımı artık RunConfig ile aynı giriş komut dosyasına sahip bir kaynak klasör belirtmeyi destekler.  Ayrıca, model dağıtımı tek bir komuta basitleştirilmiştir.

+ **Git başvuru takibi**<br/> Müşteriler, uçuça denetim izini korumaya yardımcı olduğu için bir süredir temel Git tümleştirme özelliklerini talep ediyor. Git ile ilgili meta veriler (repo, commit, clean state) için Azure ML'deki büyük kuruluşlar arasında izleme uyguladık. Bu bilgiler SDK ve CLI tarafından otomatik olarak toplanacaktır.

+ **Model profil leme & doğrulama hizmeti**<br/> Müşteriler sık sık çıkarım hizmetleriyle ilişkili bilgi işlemin düzgün boyutlandırma zorluğundan şikayet ederler. Model profil oluşturma hizmetimiz ile, müşteri örnek girdiler sağlayabilir ve dağıtım için en iyi boyutlandırmayı belirlemek için 16 farklı CPU / bellek yapılandırmaları arasında profil oluşturacağız.

+ **Çıkarım için kendi temel resminizi getirin**<br/> Başka bir yaygın şikayet deneme den çıkarım RE paylaşım bağımlılıkları hareket zorluk oldu. Yeni temel görüntü paylaşım özelliğimizle, artık deneme temel görüntülerinizi, bağımlılıklarınızı ve tüm çıkarımlarınızı yeniden kullanabilirsiniz. Bu, dağıtımları hızlandırmalı ve içten dış döngüye kadar olan boşluğu azaltmalıdır.

+ **Geliştirilmiş Swagger şema üretim deneyimi**<br/> Önceki swagger nesil yöntemi hata eğilimli ve otomatikleştirmek imkansız oldu. Dekoratörler aracılığıyla herhangi bir Python işlevinden swagger şemaüreten yeni bir in-line yolu var. Bu kodu açık kaynaklı yaptık ve şema oluşturma protokolümüz Azure ML platformuyla birleştirmiyor.

+ **Azure ML CLI genellikle kullanılabilir (GA)**<br/> Modeller artık tek bir CLI komutuyla dağıtılabilir. Kimsenin Jupyter dizüstü bilgisayardan ML modelini dağıtmadığı yönündeki ortak müşteri geri bildirimi aldık. [**CLI başvuru belgeleri**](https://aka.ms/azmlcli) güncelleştirildi.


## <a name="2019-04-22"></a>2019-04-22

Python v1.0.30 için Azure Machine Learning SDK yayınlandı.

Aynı [`PipelineEndpoint`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline_endpoint.pipelineendpoint?view=azure-ml-py) bitiş noktasını korurken yayımlanmış bir ardışık yeni bir sürümünü eklemek için tanıtıldı.

## <a name="2019-04-17"></a>2019-04-17

### <a name="azure-machine-learning-data-prep-sdk-v112"></a>Azure Machine Learning Data Hazırlık SDK v1.1.2

Not: Veri Hazırlama Python SDK `numpy` `pandas` artık yüklemeve paketleri. [Güncelleştirilmiş yükleme yönergelerine](https://github.com/Microsoft/AMLDataPrepDocs)bakın.

+ **Yeni özellikler**
  + Artık Pivot dönüşümü'ni kullanabilirsiniz.
    + Nasıl yapIlir kılavuzu: [Pivot notebook](https://aka.ms/aml-data-prep-pivot-nb)
  + Artık yerel işlevlerde normal ifadeleri kullanabilirsiniz.
    + Örnekler:
      + `dflow.filter(dprep.RegEx('pattern').is_match(dflow['column_name']))`
      + `dflow.assert_value('column_name', dprep.RegEx('pattern').is_match(dprep.value))`
  + Artık ifade `to_upper`  dilinde `to_lower`  kullanabilir ve işlevler bulabilirsiniz.
  + Artık bir veri profilinde her sütunun benzersiz değerlerinin sayısını görebilirsiniz.
  + Yaygın olarak kullanılan okuyucu adımlarından bazıları için artık `infer_column_types` bağımsız değişkeni geçebilirsiniz. `True`Ayarlanırsa, Veri Hazırlık sütun türlerini algılamayı ve otomatik olarak dönüştürmeyi dener.
    + `inference_arguments`şimdi küçümsüyor.
  + Artık arayabilirsiniz. `Dataflow.shape`

+ **Hata düzeltmeleri ve iyileştirmeler**
  + `keep_columns` şimdi ek bir isteğe bağlı bağımsız `validate_column_exists`değişken `keep_columns` kabul eder , hangi sonucu herhangi bir sütun içerecek olup olmadığını denetler.
  + Tüm okuyucu adımları (bir dosyadan okunan) `verify_exists`şimdi ek bir isteğe bağlı bağımsız değişken kabul.
  + Pandalar veri çerçevesi okuma ve veri profilleri alma geliştirilmiş performans.
  + Veri akışından tek bir adım dilimlemenin tek bir dizinle başarısız olduğu bir hata düzeltildi.

## <a name="2019-04-15"></a>2019-04-15

### <a name="azure-portal"></a>Azure portal
  + Artık varolan bir uzak bilgi işlem kümesinde varolan bir Komut Dosyası çalışmasını yeniden gönderebilirsiniz.
  + Artık, Denetim Hatları sekmesinde yeni parametreler içeren yayımlanmış bir ardışık işakte hattı çalıştırabilirsiniz.
  + Çalıştır ayrıntıları artık yeni bir Anlık Görüntü dosyası görüntüleyicidestekler. Belirli bir çalışma gönderdiğinde dizinin anlık görüntüsünü görüntüleyebilirsiniz. Çalıştırmayı başlatmak için gönderilen not defterini de indirebilirsiniz.
  + Artık Azure portalından üst çalıştırmaları iptal edebilirsiniz.

## <a name="2019-04-08"></a>2019-04-08

### <a name="azure-machine-learning-sdk-for-python-v1023"></a>Python v1.0.23 için Azure Machine Learning SDK

+ **Yeni özellikler**
  + Azure Machine Learning SDK artık Python 3.7'yi destekliyor.
  + Azure Machine Learning DNN Tahmincileri artık yerleşik çok sürümdesteği sağlıyor. Örneğin, `TensorFlow`  tahminci artık bir `framework_version` parametre kabul eder ve kullanıcılar '1.10' veya '1.12' sürümünü belirtebilir. Geçerli SDK sürümünüz tarafından desteklenen sürümlerin listesi `get_supported_versions()` için, istenen çerçeve sınıfını (örneğin, `TensorFlow.get_supported_versions()`) arayın.
  En son SDK sürümü tarafından desteklenen sürümlerin listesi için [DNN Tahminci belgelerine](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn?view=azure-ml-py)bakın.

### <a name="azure-machine-learning-data-prep-sdk-v111"></a>Azure Machine Learning Data Hazırlık SDK v1.1.1

+ **Yeni özellikler**
  + read_* dönüşümlerini kullanarak birden çok Datastore/DataPath/DataReference kaynağını okuyabilirsiniz.
  + Yeni bir sütun oluşturmak için sütunlar üzerinde aşağıdaki işlemleri gerçekleştirebilirsiniz: bölme, zemin, modulo, güç, uzunluk.
  + Veri Hazırlama artık Azure ML tanılama paketinin bir parçasıdır ve tanılama bilgilerini varsayılan olarak kaydeder.
    + Bunu kapatmak için, bu ortam değişkenini doğru ayarlayın: DISABLE_DPREP_LOGGER

+ **Hata düzeltmeleri ve iyileştirmeler**
  + Yaygın olarak kullanılan sınıflar ve işlevler için geliştirilmiş kod belgeleri.
  + Excel dosyalarını okumayan auto_read_file bir hata düzeltildi.
  + Klasörün üzerine read_pandas_dataframe yazma seçeneği eklendi.
  + Dotnetcore2 bağımlılık kurulumunun geliştirilmiş performansı ve Fedora 27/28 ve Ubuntu 1804 için destek eklendi.
  + Azure Blobs'tan okuma performansını artırdı.
  + Sütun türü algılama şimdi Uzun türü sütunları destekler.
  + Python tarih saati nesneleri yerine bazı tarih değerlerinin zaman damgası olarak görüntülendiği bir hata düzeltildi.
  + Bazı tür sayımlarının tamsayılar yerine çift olarak görüntülendiği bir hata düzeltildi.


## <a name="2019-03-25"></a>2019-03-25

### <a name="azure-machine-learning-sdk-for-python-v1021"></a>Python v1.0.21 için Azure Machine Learning SDK

+ **Yeni özellikler**
  + *Azureml.core.Run.create_children* yöntemi, tek bir çağrıyla birden çok alt çalıştırmanın düşük gecikmeli olarak oluşturulmasına olanak tanır.

### <a name="azure-machine-learning-data-prep-sdk-v110"></a>Azure Machine Learning Data Hazırlık SDK v1.1.0

+ **Yeni değişiklikler**
  + Veri Hazırlama Paketi kavramı küçümsülse ve artık desteklenmemiştir. Bir Pakette birden çok Veri akışını kalıcı olarak sürdürmek yerine, Veri akışlarını tek tek süre devam ettirebilirsiniz.
    + Nasıl yapIlir kılavuzu: [Veri akışları defterini açma ve kaydetme](https://aka.ms/aml-data-prep-open-save-dataflows-nb)

+ **Yeni özellikler**
  + Veri Hazırlığı artık belirli bir Anlam türüyle eşleşen sütunları tanıyabilir ve buna göre bölünebilir. Şu anda desteklenen STypes şunlardır: e-posta adresi, coğrafi koordinatlar (enlem & boylam), IPv4 ve IPv6 adresleri, ABD telefon numarası ve ABD posta kodu.
    + Nasıl yapilir kılavuzu: [Anlamsal Tipler not defteri](https://aka.ms/aml-data-prep-semantic-types-nb)
  + Veri Hazırlığı şimdi iki sayısal sütundan elde edilen bir sütun oluşturmak için aşağıdaki işlemleri destekler: çıkarma, çarpma, bölme ve modüle.
  + Yürütülürse Veri Akışının kayıt oluşturup oluşturmayacağını denetlemek için Veri Akışı'nı arayabilirsiniz. `verify_has_data()`

+ **Hata düzeltmeleri ve iyileştirmeler**
  + Artık sayısal sütun profilleri için histogramda kullanılacak kutu sayısı belirtebilirsiniz.
  + Dönüştürme `read_pandas_dataframe` şimdi DataFrame dize veya bayt-yazılı sütun adları olmasını gerektirir.
  + Sütun eksikse `fill_nulls` değerlerin doğru şekilde doldurulmadı dönüşümdeki bir hata düzeltildi.

## <a name="2019-03-11"></a>2019-03-11

### <a name="azure-machine-learning-sdk-for-python-v1018"></a>Python v1.0.18 için Azure Machine Learning SDK

 + **Değişiklik**
   + Azureml-tensorboard paketi azureml-contrib-tensorboard'un yerini alır.
   + Bu sürümle, oluştururken yönetilen işlem kümenizde (amlcompute) bir kullanıcı hesabı ayarlayabilirsiniz. Bu, bu özellikleri sağlama yapılandırmasında geçirerek yapılabilir. [SDK başvuru belgelerinde](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#provisioning-configuration-vm-size-----vm-priority--dedicated---min-nodes-0--max-nodes-none--idle-seconds-before-scaledown-none--admin-username-none--admin-user-password-none--admin-user-ssh-key-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--tags-none--description-none--remote-login-port-public-access--notspecified--)daha fazla ayrıntı bulabilirsiniz.

### <a name="azure-machine-learning-data-prep-sdk-v1017"></a>Azure Machine Learning Data Hazırlık SDK v1.0.17

+ **Yeni özellikler**
  + Şimdi ifade dilini kullanarak sonuç sütun oluşturmak için iki sayısal sütun eklemeyi destekler.

+ **Hata düzeltmeleri ve iyileştirmeler**
  + random_split için belge ve parametre denetimi geliştirildi.

## <a name="2019-02-27"></a>2019-02-27

### <a name="azure-machine-learning-data-prep-sdk-v1016"></a>Azure Machine Learning Data Hazırlık SDK v1.0.16

+ **Hata düzeltmesi**
  + API değişikliğinden kaynaklanan hizmet sorumlusu kimlik doğrulama sorunu düzeltildi.

## <a name="2019-02-25"></a>2019-02-25

### <a name="azure-machine-learning-sdk-for-python-v1017"></a>Python v1.0.17 için Azure Machine Learning SDK

+ **Yeni özellikler**

  + Azure Machine Learning artık popüler DNN çerçeve Zinciri için birinci sınıf destek sağlar. Sınıf [`Chainer`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) kullanıcıları kullanarak kolayca trainve Chainer modelleri dağıtmak.
    + [ChainerMN ile dağıtılmış eğitimi nasıl çalıştırılamayın](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/distributed-chainer/distributed-chainer.ipynb)
    + [HyperDrive'ı kullanarak Chainer ile hiperparametre atonlamayı nasıl çalıştırılanmayı](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-chainer/train-hyperparameter-tune-deploy-with-chainer.ipynb) öğrenin
  + Azure Machine Learning Pipelines, datastore değişikliklerine dayalı bir Ardışık Ardışık Yol Çalışması'nı tetikleme özelliği ni ekledi. Bu özelliği sergilemek için ardışık program [defteri](https://aka.ms/pl-schedule) güncelleştirilir.

+ **Hata düzeltmeleri ve iyileştirmeler**
  + Source_directory_data_store özelliğini [PythonScriptStep'e](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep?view=azure-ml-py)verilen [RunConfigurations'da](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.runconfiguration?view=azure-ml-py) istenen bir veri deposuna (blob depolama gibi) ayarlamak için Azure Machine Learning ardışık hatlarına destek ekledik. Varsayılan adımlar olarak, çok sayıda adım aynı anda yürütüldüğünde azaltma sorunlarıyla karşılaşabilecek olan, destek veri deposu olarak Azure Dosya deposunu kullanın.

### <a name="azure-portal"></a>Azure portal

+ **Yeni özellikler**
  + Raporlar için yeni sürükle ve bırak tablo düzenleyicisi deneyimi. Kullanıcılar bir sütunu kuyudan tabloönizlemesinin görüntüleneceği tablo alanına sürükleyebilir. Sütunlar yeniden düzenlenebilir.
  + Yeni Günlükler dosya görüntüleyici
  + Etkinlikler sekmesinden deneme çalıştırmaları, hesaplama, modeller, resimler ve dağıtımlara bağlantılar

### <a name="azure-machine-learning-data-prep-sdk-v1015"></a>Azure Machine Learning Data Hazırlık SDK v1.0.15

+ **Yeni özellikler**
  + Data Prep artık bir veri akışından dosya akışları yazmayı destekler. Ayrıca, yeni dosya adları oluşturmak için dosya akışı adlarını işleme olanağı sağlar.
    + Nasıl yapIlir kılavuzu: [Dosya Akışları ile Çalışma not defteri](https://aka.ms/aml-data-prep-file-stream-nb)

+ **Hata düzeltmeleri ve iyileştirmeler**
  + Büyük veri kümelerinde t-Digest'in geliştirilmiş performansı.
  + Data Prep artık bir DataPath'ten veri okumayı destekler.
  + Bir sıcak kodlama şimdi boolean ve sayısal sütunlar üzerinde çalışır.
  + Diğer çeşitli hata düzeltmeleri.

## <a name="2019-02-11"></a>2019-02-11

### <a name="azure-machine-learning-sdk-for-python-v1015"></a>Python v1.0.15 için Azure Machine Learning SDK

+ **Yeni özellikler**
  + Azure Machine Learning Pipelines, AzureBatchStep[(not defteri),](https://aka.ms/pl-azbatch)HyperDriveStep (not defteri) ve zaman tabanlı zamanlama işlevselliği[(not defteri)](https://aka.ms/pl-schedule)ekledi.
  +  DataTranferStep, PostgreSQL[(notebook)](https://aka.ms/pl-data-trans)için Azure SQL Server ve Azure veritabanı ile çalışacak şekilde güncelleştirildi.

+ **Değişiklik**
  + Lehine amortismana `PublishedPipeline.get_published_pipeline` `PublishedPipeline.get`uyrmandı.
  + Lehine amortismana `Schedule.get_schedule` `Schedule.get`uyrmandı.

### <a name="azure-machine-learning-data-prep-sdk-v1012"></a>Azure Machine Learning Data Hazırlık SDK v1.0.12

+ **Yeni özellikler**
  + Data Prep artık Datastore'u kullanarak Azure SQL veritabanından okumayı destekliyor.

+ **Değişiklik**
  + Büyük verilerdeki belirli işlemlerin bellek performansını artırdı.
  + `read_pandas_dataframe()`şimdi `temp_folder` belirtilmesi gerekir.
  + Üzerindeki `name` `ColumnProfile` özellik amortismana kaldırıldı - `column_name` bunun yerine kullanın.

## <a name="2019-01-28"></a>2019-01-28

### <a name="azure-machine-learning-sdk-for-python-v1010"></a>Python v1.0.10 için Azure Machine Learning SDK

+ **Değişiklikler**:
  + Azure ML SDK artık bağımlılık olarak azure-cli paketlerine sahip değildir. Özellikle, azure-cli-core ve azure-cli-profile bağımlılıkları azureml-core kaldırıldı. Değişiklikleri etkileyen kullanıcı şunlardır:
      + "Az giriş" yapıyorsanız ve azureml-sdk kullanıyorsanız, SDK tarayıcı veya cihaz kodu oturumaçını bir kez daha gerçekleştirecektir. "az giriş" tarafından oluşturulan herhangi bir kimlik bilgileri durumu kullanmaz.
    + "Az giriş" gibi Azure CLI kimlik doğrulaması için _azureml.core.authentication.AzureCliAuthentication_ sınıf'ı kullanın. Azure CLI kimlik doğrulaması için, azureml-sdk yüklediğiniz Python ortamında _pip yükleme azure-cli'yi_ yapın.
    + Otomasyon için bir servis ilkesini kullanarak "az giriş" yapıyorsanız, _azureml.core.authentication.ServicePrincipalAuthentication_ sınıf'ı kullanmanızı öneririz, çünkü azureml-sdk azure CLI tarafından oluşturulan kimlik bilgilerini kullanmaz.

+ **Hata düzeltmeleri**: Bu sürüm çoğunlukla küçük hata düzeltmeleri içerir

### <a name="azure-machine-learning-data-prep-sdk-v108"></a>Azure Machine Learning Data Hazırlık SDK v1.0.8

+ **Hata düzeltmeleri**
  + Veri profilleri alma performansını artırdı.
  + Hata raporlamaile ilgili küçük hatalar düzeltildi.

### <a name="azure-portal-new-features"></a>Azure portalı: yeni özellikler
+ Raporlar için yeni sürükle ve bırak grafik deneyimi. Kullanıcılar, bir sütunu veya özniteliği kuyudan, sistemin veri türüne bağlı olarak kullanıcı için uygun bir grafik türünü otomatik olarak seçeceği grafik alanına sürükleyebilir. Kullanıcılar grafik türünü diğer geçerli türlerle değiştirebilir veya ek öznitelikler ekleyebilir.

    Desteklenen Grafik Türleri:
    - Çizgi Grafiği
    - Histogram
    - Yığılmış Çubuk Grafiği
    - Kutu Çizimi
    - Dağılım Konusu
    - Kabarcık Arsa
+ Portal artık dinamik olarak deneyler için raporlar oluşturur. Bir kullanıcı bir deneme için bir çalıştırma gönderdiğinde, farklı çalıştırmalar arasında karşılaştırmaya izin vermek için oturum açmış ölçümler ve grafiklerle otomatik olarak bir rapor oluşturulur.

## <a name="2019-01-14"></a>2019-01-14

### <a name="azure-machine-learning-sdk-for-python-v108"></a>Python v1.0.8 için Azure Machine Learning SDK

+ **Hata düzeltmeleri**: Bu sürüm çoğunlukla küçük hata düzeltmeleri içerir

### <a name="azure-machine-learning-data-prep-sdk-v107"></a>Azure Machine Learning Data Hazırlık SDK v1.0.7

+ **Yeni özellikler**
  + Datastore geliştirmeleri [(Datastore nasıl yapılır kılavuzunda](https://aka.ms/aml-data-prep-datastore-nb)belgelenmiştir)
    + Azure Dosya Paylaşımı ve ADLS Datastores'a ölçeklendirme de dahil olmak üzere okuma ve yazma özelliği eklendi.
    + DataStores'ı kullanırken, Data Prep artık etkileşimli kimlik doğrulaması yerine hizmet temel kimlik doğrulamasını kullanmayı destekler.
    + Wasb ve wasbs url'leri için destek eklendi.

## <a name="2019-01-09"></a>2019-01-09

### <a name="azure-machine-learning-data-prep-sdk-v106"></a>Azure Machine Learning Data Hazırlık SDK v1.0.6

+ **Hata düzeltmeleri**
  + Kıvılcım'da herkese açık okunabilir Azure Blob kapsayıcılarından okuma ile hata düzeltildi

## <a name="2018-12-20"></a>2018-12-20

### <a name="azure-machine-learning-sdk-for-python-v106"></a>Python v1.0.6 için Azure Machine Learning SDK
+ **Hata düzeltmeleri**: Bu sürüm çoğunlukla küçük hata düzeltmeleri içerir

### <a name="azure-machine-learning-data-prep-sdk-v104"></a>Azure Machine Learning Data Hazırlık SDK v1.0.4

+ **Yeni özellikler**
  + `to_bool`işlev artık eşleşmemiş değerlerin Hata değerlerine dönüştürülmesini sağlar. Bu, yeni `to_bool` varsayılan uyumsuzluk davranışıdır ve `set_column_types`önceki varsayılan davranış eşleşmeyen değerleri False'a dönüştürmek içindi.
  + Ararken, `to_pandas_dataframe`sayısal sütunlarda null/missing değerlerini NaN olarak yorumlamak için yeni bir seçenek vardır.
  + Tür tutarlılığı sağlamak ve erken başarısız olmak için bazı ifadelerin dönüş türünü denetleme özelliği eklendi.
  + Artık json `parse_json` nesneleri olarak bir sütundaki değerleri ayrıştırmak için çağrıda bulunabilir ve bunları birden çok sütuna genişletebilirsiniz.

+ **Hata düzeltmeleri**
  + Python 3.5.2'de çöken `set_column_types` bir hata düzeltildi.
  + AML görüntüsü kullanarak Datastore'a bağlanırken çöken bir hata düzeltildi.

+ **Güncelleştirmeler**
  * Başlangıç eğitimleri, vaka çalışmaları ve nasıl yapılacağını öğrenmek için [örnek Defterler.](https://aka.ms/aml-data-prep-notebooks)

## <a name="2018-12-04-general-availability"></a>2018-12-04: Genel Uygunluk

Azure Machine Learning artık genel olarak kullanılabilir.

### <a name="azure-machine-learning-compute"></a>Azure Machine Learning İşlemi
Bu sürümle, [Azure Machine Learning Compute](how-to-set-up-training-targets.md#amlcompute)aracılığıyla yeni bir yönetilen işlem deneyimi duyuruyor. Bu bilgi işlem hedefi, Azure Machine Learning için Azure Toplu AI bilgi işlem yerini alır.

Bu işlem hedefi:
+ Model eğitimi ve toplu çıkarım/puanlama için kullanılır
+ Tek-to multi-düğüm hesaplama
+ Kullanıcı için küme yönetimi ve iş planlaması yapar mı
+ Varsayılan olarak otomatik ölçekler
+ Hem CPU hem de GPU kaynakları için destek
+ Düşük öncelikli VM'lerin daha düşük maliyetle kullanılmasını sağlar

Azure Machine Learning Compute, Azure portalı veya CLI kullanılarak Python'da oluşturulabilir. Çalışma alanınızın bölgesinde oluşturulmalıdır ve başka bir çalışma alanına eklenemez. Bu bilgi işlem hedefi, çalışmanız için bir Docker kapsayıcısı kullanır ve bağımlılıklarınızı tüm düğümlerinizde aynı ortamı çoğaltmak için paketler.

> [!Warning]
> Azure Machine Learning Compute'u kullanmak için yeni bir çalışma alanı oluşturmanızı öneririz. Varolan bir çalışma alanından Azure Machine Learning Compute oluşturmaya çalışan kullanıcıların bir hata görme olasılığı çok uzaktır. Çalışma alanınızdaki varolan işlem etkilenmeden çalışmaya devam etmelidir.

### <a name="azure-machine-learning-sdk-for-python-v102"></a>Python v1.0.2 için Azure Machine Learning SDK
+ **Yeni değişiklikler**
  + Bu sürümle, Azure Machine Learning'den bir VM oluşturma desteğini kaldırıyoruz. Varolan bir bulut VM'yi veya uzaktaki bir şirket içi sunucueklemeye devam edebilirsiniz.
  + Ayrıca, artık Azure Machine Learning Compute aracılığıyla desteklenmesi gereken BatchAI desteğini de kaldırıyoruz.

+ **Yeni**
  + Makine öğrenimi boru hatları için:
    + [TahminciAdım](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.estimator_step.estimatorstep?view=azure-ml-py)
    + [HyperDriveStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.hyper_drive_step.hyperdrivestep?view=azure-ml-py)
    + [MpiStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.mpi_step.mpistep?view=azure-ml-py)


+ **Güncel -leştirilmiş**
  + Makine öğrenimi boru hatları için:
    + [DatabricksStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.databricks_step.databricksstep?view=azure-ml-py) şimdi runconfig kabul eder
    + [DataTransferStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) artık bir SQL veri kaynağına ve
    + Yayımlanmış ardışık hatlar için zamanlamaoluşturmak ve güncelleştirmek için SDK'da işlevsellik zamanlama

<!--+ **Bugs fixed**-->

### <a name="azure-machine-learning-data-prep-sdk-v052"></a>Azure Machine Learning Data Hazırlık SDK v0.5.2
+ **Yeni değişiklikler**
  * `SummaryFunction.N`olarak `SummaryFunction.Count`değiştirildi.

+ **Hata Düzeltmeleri**
  * Uzaktan çalıştırmalarda veri depolarına okurken ve yazarken en son AML Run Belirteci'ni kullanın. Daha önce, AML Run Token Python'da güncelleştirilmişse, Veri Hazırlık çalışma zamanı güncelleştirilmiş AML Run Token ile güncelleştirilmez.
  * Ek daha net hata iletileri
  * to_spark_dataframe() artık Spark serileştirme `Kryo` kullandığında çökmeyecek
  * Değer Sayımı Denetçisi artık 1000'den fazla benzersiz değer gösterebilir
  * Özgün Veri akışının bir adı yoksa Rasgele Bölme artık başarısız olmuyor

+ **Daha fazla bilgi**
  * [Azure Machine Learning Veri Hazırlama SDK'sı](https://aka.ms/data-prep-sdk)

### <a name="docs-and-notebooks"></a>Dokümanlar ve not defterleri
+ ML Boru Hatları
  + Boru hatları, toplu iş kapsamları ve stil transferi örnekleriyle başlamak için yeni ve güncelleştirilmiş not defterleri:https://aka.ms/aml-pipeline-notebooks
  + [İlk ardışık hattınızı](how-to-create-your-first-pipeline.md) nasıl oluşturunuzu öğrenin
  + [Ardışık hatları kullanarak toplu tahminlerini nasıl çalıştırırışturmayı](how-to-use-parallel-run-step.md) öğrenin
+ Azure Machine Learning bilgi işlem hedefi
  + [Örnek not defterleri](https://aka.ms/aml-notebooks) artık yeni yönetilen bilgi işlem kullanmak üzere güncelleştirildi.
  + [Bu bilgi işlem hakkında bilgi edinin](how-to-set-up-training-targets.md#amlcompute)

### <a name="azure-portal-new-features"></a>Azure portalı: yeni özellikler
+ Portalda [Azure Machine Learning Compute](how-to-set-up-training-targets.md#amlcompute) türleri oluşturun ve yönetin.
+ Azure Machine Learning Compute için kota kullanımını izleyin ve [kota isteyin.](how-to-manage-quotas.md)
+ Azure Machine Learning İşlem küme durumunu gerçek zamanlı olarak görüntüleyin.
+ Azure Machine Learning Compute ve Azure Kubernetes Service oluşturma için sanal ağ desteği eklendi.
+ Yayımlanmış ardışık hatlarınızı varolan parametrelerle yeniden çalıştırın.
+ Sınıflandırma modelleri (asansör, kazanımlar, kalibrasyon, model açıklanabilirliği ile özellik önem grafiği) ve regresyon modelleri (artıklar ve model açıklanabilirliği ile özellik önem grafiği) için yeni [otomatik makine öğrenme grafikleri.](how-to-understand-automated-ml.md)
+ Boru hatları Azure portalında görüntülenebilir




## <a name="2018-11-20"></a>2018-11-20

### <a name="azure-machine-learning-sdk-for-python-v0180"></a>Python v0.1.80 için Azure Machine Learning SDK

+ **Yeni değişiklikler**
  * *azureml.train.widgets* namespace *azureml.widgets*taşındı .
  * *azureml.core.compute.AmlCompute* aşağıdaki sınıfları yıkıyor - *azureml.core.compute.BatchAICompute* ve *azureml.core.compute.DSVMCompute*. İkinci sınıf sonraki sürümlerde kaldırılacaktır. AmlCompute sınıfı artık daha kolay bir tanıma sahiptir ve yalnızca bir vm_size ve max_nodes ihtiyaç duyar ve bir iş gönderildiğinde kümenizi otomatik olarak 0'dan max_nodes ölçeklendirecektir. [Örnek not defterlerimiz](https://github.com/Azure/MachineLearningNotebooks/tree/master/training) bu bilgilerle güncellenmiştir ve size kullanım örnekleri vermelidir. Biz bu basitleştirme ve daha sonra sürümü gelmek için daha heyecan verici özellikleri çok gibi umuyoruz!

### <a name="azure-machine-learning-data-prep-sdk-v051"></a>Azure Machine Learning Data Hazırlık SDK v0.5.1

[Referans dokümanlarını](https://aka.ms/data-prep-sdk)okuyarak Veri Hazırlama SDK hakkında daha fazla bilgi edinin.
+ **Yeni Özellikler**
   * DataPrep paketlerini yürütmek ve veri kümesi veya veri akışı için veri profilini görüntülemek için yeni bir DataPrep CLI oluşturuldu
   * Kullanılabilirliği artırmak için Yeniden Tasarlanan SetColumnType API
   * auto_read_file için smart_read_file olarak yeniden adlandırıldı
   * Şimdi Veri Profili çarpık ve kurtosis içerir
   * Tabakalı örnekleme ile örnekleme yapabilir
   * CSV dosyaları içeren zip dosyalarından okuyabilirsiniz
   * Random Split ile veri kümelerini satır bazında bölebilir (örneğin, test-tren kümelerine)
   * Bir veri akışından veya veri profilinden tüm sütun veri türlerini arayarak`.dtypes`
   * Bir veri akışından veya veri profilinden satır sayısını arayarak alabilirsiniz`.row_count`

+ **Hata Düzeltmeleri**
   * Uzundan çifte dönüştürmeye düzeltildi
   * Herhangi bir ekleme sütunundan sonra düzeltme
   * Bazı durumlarda grupları algılatmayacak FuzzyGrouping ile ilgili bir sorun giderildi
   * Çok sütunlu sıralama sırasına saygı göstermek için sabit sıralama işlevi
   * Nasıl `pandas` işlenire benzer şekilde sabit ve/veya ifadeler
   * dbfs yolundan sabit okuma
   * Hata iletilerini daha anlaşılır hale getirdi
   * Artık bir AML belirteci kullanarak uzaktan işlem hedefi okurken başarısız
   * Artık Linux DSVM'de başarısız
   * Dize olmayan değerler dize yüklemlerinde olduğunda artık çökmeyok
   * Dataflow doğru başarısız olduğunda şimdi iddia hataları işler
   * Artık Azure Databricks'te dbutils monte edilmiş depolama konumlarını destekler

## <a name="2018-11-05"></a>2018-11-05

### <a name="azure-portal"></a>Azure portal
Azure Machine Learning için Azure portalı aşağıdaki güncelleştirmelere sahiptir:
  * Yayımlanmış ardışık hatlar için yeni bir **Pipelines** sekmesi.
  * Varolan bir HDInsight kümesini bilgi işlem hedefine eklemek için destek eklendi.

### <a name="azure-machine-learning-sdk-for-python-v0174"></a>Python v0.1.74 için Azure Machine Learning SDK

+ **Yeni değişiklikler**
  * *Workspace.compute_targets, datastores, experiments, images, models ve *webservices* yöntemleri yerine özellikleridir. Örneğin, *Workspace.compute_targets()* ile *Workspace.compute_targets'ı*değiştirin.
  * *Run.get_context* *Run.get_submitted_run'ı*amortismana katsın. İkinci yöntem sonraki sürümlerde kaldırılır.
  * *PipelineData* sınıfı artık datastore_name yerine bir parametre olarak bir veri deposu nesnesi bekliyor. Benzer şekilde, *Pipeline* default_datastore_name yerine default_datastore kabul eder.

+ **Yeni özellikler**
  * Azure Machine Learning Pipelines [örnek not defteri](https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline/pipeline-mpi-batch-prediction.ipynb) artık MPI adımlarını kullanıyor.
  * Jupyter dizüstü bilgisayarlar için RunDetails widget'ı, ardışık hattın görselleştirilmesini göstermek için güncelleştirilir.

### <a name="azure-machine-learning-data-prep-sdk-v040"></a>Azure Machine Learning Data Hazırlık SDK v0.4.0

+ **Yeni özellikler**
  * Veri Profiline Eklenen Tür Sayısı
  * Değer Sayımı ve Histogram artık kullanılabilir
  * Veri Profilinde daha fazla yüzdelik artış
  * Ortanca Özet'te mevcuttur
  * Python 3.7 artık desteklendi
  * Veri depolarını içeren bir veri akışını DataPrep paketine kaydettiğinizde, datastore bilgileri DataPrep paketinin bir parçası olarak kalıcı olacaktır
  * Datastore'a yazma artık desteklendi

+ **Hata düzeltildi**
  * 64 bit imzasız tamsayı taşmaları artık Linux'ta düzgün bir şekilde işlenir
  * smart_read düz metin dosyaları için yanlış metin etiketi düzeltildi
  * String sütun türü şimdi ölçümler görünümünde gösterir
  * Tür sayısı şimdi tek tek olanlar yerine tek FieldType eşlenen ValueKinds göstermek için sabittir
  * Write_to_csv, yol dize olarak sağlandığında artık başarısız olmaz
  * Değiştir'i kullanırken,"bul"u boş bırakmak artık başarısız olmaz

## <a name="2018-10-12"></a>2018-10-12

### <a name="azure-machine-learning-sdk-for-python-v0168"></a>Python v0.1.68 için Azure Machine Learning SDK

+ **Yeni özellikler**
  * Yeni çalışma alanı oluştururken çok kiracılı destek.

+ **Hatalar düzeltildi**
  * Web hizmetini dağıtırken pynacl kitaplık sürümünü sabitlemeniz gerekmez.

### <a name="azure-machine-learning-data-prep-sdk-v030"></a>Azure Machine Learning Data Hazırlık SDK v0.3.0

+ **Yeni özellikler**
  * Kullanıcıların bir Python dosyasının yürütülmesi için yol geçmesine olanak tanıyan yöntem transform_partition_with_file(script_path) eklendi

## <a name="2018-10-01"></a>2018-10-01

### <a name="azure-machine-learning-sdk-for-python-v0165"></a>Python v0.1.65 için Azure Machine Learning SDK
[Sürüm 0.1.65](https://pypi.org/project/azureml-sdk/0.1.65) yeni özellikler, daha fazla dokümantasyon, hata düzeltmeleri ve daha fazla [örnek not defteri](https://aka.ms/aml-notebooks)içerir.

Bilinen hatalar ve geçici geçici işler hakkında bilgi edinmek için [bilinen sorunların listesine](resource-known-issues.md) bakın.

+ **Yeni değişiklikler**
  * Workspace.experiments, Workspace.models, Workspace.compute_targets, Workspace.images, Workspace.web_services return dictionary, önceden döndürülen liste. [Azureml.core.Workspace](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py) API belgelerine bakın.

  * Otomatik Machine Learning, birincil ölçümlerden normalleştirilmiş ortalama kare hatasını kaldırdı.

+ **Hipersürücü**
  * Bayesian için çeşitli HyperDrive hata düzeltmeleri, Ölçümler aramaları almak için Performans iyileştirmeleri.
  * Tensorflow 1.10 yükseltme 1.9
  * Soğuk başlangıç için Docker görüntü optimizasyonu.
  * İşler artık 0'dan başka hata koduyla çıksalar bile doğru durumu bildiriyor.
  * SDK'da RunConfig öznitelik doğrulaması.
  * HyperDrive çalıştır nesnesi normal çalıştırmaya benzer şekilde iptal eder: herhangi bir parametreyi geçirmeye gerek yoktur.
  * Dağıtılmış çalıştırmalar ve HyperDrive çalıştırmaları için açılır değerlerin durumunu korumak için Widget geliştirmeleri.
  * TensorBoard ve diğer günlük dosyaları Parametre sunucusu için sabit desteği.
  * Hizmet tarafında Intel(R) MPI desteği.
  * BatchAI'de doğrulama sırasında dağıtılmış çalıştırma düzeltmesi için parametre atoması hata düzeltmesi.
  * Bağlam Yöneticisi şimdi birincil örneği tanımlar.

+ **Azure portal deneyimi**
  * log_table() ve log_row() Run ayrıntılarıyla desteklenir.
  * 1, 2 veya 3 sayısal sütunlu ve isteğe bağlı kategorik sütunlu tablolar ve satırlar için grafikleri otomatik olarak oluşturun.

+ **Otomatik Makine Öğrenimi**
  * Geliştirilmiş hata işleme ve dokümantasyon
  * Çalışma özelliği alma performansı sorunları düzeltildi.
  * Devam çalıştırma sorunu düzeltildi.
  * Yineleme :::no-loc text="ensembling"::: sorunları düzeltildi.
  * MAC OS'de sabit eğitim asılı hata.
  * Özel doğrulama senaryosunda makro ortalama PR/ROC eğrisinin alt örneklemesi.
  * Ek dizin mantığı kaldırıldı.
  * get_output API'den filtre kaldırıldı.

+ **İşlem hatları**
  * Bir yürütme çalışması ilk gerektirmeden, doğrudan bir ardışık yayımlamak için bir yöntem Pipeline.publish() eklendi.
  * Yayınlanan bir ardışık sistem hattından oluşturulan boru hattı çalışanlarını almak için PipelineRun.get_pipeline_runs() yöntemi eklendi.

+ **Beyin Dalgası Projesi**
  * FPPGA'larda bulunan yeni AI modelleri için güncelleştirilmiş destek.

### <a name="azure-machine-learning-data-prep-sdk-v020"></a>Azure Machine Learning Data Hazırlık SDK v0.2.0
[Sürüm 0.2.0](https://pypi.org/project/azureml-dataprep/0.2.0/) aşağıdaki özellikleri ve hata düzeltmelerini içerir:

+ **Yeni özellikler**
  * Tek sıcak kodlama desteği
  * Quantile dönüşümü için destek

+ **Hata düzeltildi:**
  * Herhangi bir Tornado sürümü ile çalışır, gerek Tornado sürümü düşürmek için
  * Değer, sadece ilk üç değer için değil, tüm değerler için sayar

## <a name="2018-09-public-preview-refresh"></a>2018-09 (Genel önizleme yenileme)

Azure Machine Learning'in yeni ve yenilenmiş sürümü: Bu sürüm hakkında daha fazla bilgi edinin:https://azure.microsoft.com/blog/what-s-new-in-azure-machine-learning-service/


## <a name="next-steps"></a>Sonraki adımlar

[Azure Machine Learning](overview-what-is-azure-ml.md)’e genel bakışı okuyun.
