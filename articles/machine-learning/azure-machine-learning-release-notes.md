---
title: Yayındaki yenilikler nelerdir?
titleSuffix: Azure Machine Learning
description: Azure Machine Learning için en son güncelleştirmeler ve Machine Learning ve Data Prep Python SDK 'Ları hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
ms.author: jmartens
author: j-martens
ms.date: 03/10/2020
ms.openlocfilehash: 4a89816202953a15c4ad4586692dacd3f12993fa
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87013038"
---
# <a name="azure-machine-learning-release-notes"></a>Azure Machine Learning sürüm notları

Bu makalede Azure Machine Learning sürümleri hakkında bilgi edinin.  Tam SDK başvuru içeriği için Azure Machine Learning [**Python başvurusu için ana SDK**](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) sayfasına gidin.

Bilinen hatalar ve geçici çözümler hakkında bilgi edinmek için [bilinen sorunlar listesine](resource-known-issues.md) bakın.

## <a name="2020-07-20"></a>2020-07-20

### <a name="azure-machine-learning-sdk-for-python-v1100"></a>Python v 1.10.0 için SDK Azure Machine Learning

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **azureml-automl-core**
    + Otomatik ml kullanırken, bir yol otomatik Mlconfig nesnesine geçirilir ve zaten mevcut değilse otomatik olarak oluşturulur.
    + Kullanıcılar artık parametresini kullanarak tahmin görevleri için bir zaman serisi frekansı belirtebilir `freq` .
  + **azureml-automl-runtime**
    + Otomatik ml kullanırken, bir yol otomatik Mlconfig nesnesine geçirilir ve zaten mevcut değilse otomatik olarak oluşturulur.
    + Kullanıcılar artık parametresini kullanarak tahmin görevleri için bir zaman serisi frekansı belirtebilir `freq` .
    + Oto ve tahmin artık, test veya doğrulama kümesinin uzunluğunun giriş ufkından daha uzun olduğu ve bilinen y_pred değerinin tahmin bağlamı olarak kullanıldığı kullanım durumu için geçerli olan sıralı değerlendirmeyi desteklemektedir.
  + **azureml-core**
    + Bir çalıştırma içindeki veri deposundan hiçbir dosya indirildiyse uyarı iletileri yazdırılır.
    + İçin belge eklendi `skip_validation` `Datastore.register_azure_sql_database method` .
    + Otomatik olarak onaylanan özel uç nokta oluşturmak için kullanıcıların SDK v 1.10.0 veya üstüne yükseltilmesi gerekir. Bu, VNet 'in arkasında kullanılabilir olan Not defteri kaynağını içerir.
    + Çalışma alanını al yanıtında Not defteri bilgilerini kullanıma sunun.
    + Uzaktan çalıştırmada işlem hedeflerini listeleme ve işlem hedefini alma çağrılarına yönelik çağrılara sahip olan değişiklikler. İşlem hedefi ve liste çalışma alanı işlem hedeflerini almak için SDK işlevleri artık uzak çalışmalarla çalışacaktır.
    + Azureml. Core. image sınıfları için sınıf açıklamalarına kullanımdan kaldırma iletileri ekleyin.
    + Çalışma alanı özel uç noktası oluşturma başarısız olursa, özel durum oluştur ve çalışma alanını ve bağımlı kaynakları temizle.
    + Çalışma alanı güncelleştirme yönteminde çalışma alanı SKU yükseltmesi desteği.
  + **azureml-datadrift**
    + Matplotlib sürümünü 3.0.2 ' den 3.2.1 ' ye güncelleştiren Python 3,8 ' i destekler.
  + **azureml-dataprep**
    + Veya isteğiyle Web URL veri kaynakları desteği eklendi `Range` `Head` . 
    + Dosya veri kümesi bağlama ve indirme için iyileştirilmiş kararlılık.
  + **azureml-train-automl-client**
    + Setuptools 'tan kaldırılmasına ilişkin sorunlar düzeltildi `RequirementParseError` .
    + "Compute_target = ' yerel '" kullanılarak gönderilen yerel çalıştırmalar için Conda yerine Docker kullanın
    + Konsola yazdırılan yineleme süresi düzeltildi. Daha önce, yineleme süresi bazen çalışma bitiş saati eksi çalıştırma oluşturma süresi olarak yazdırılmıştır. Çalışma bitiş saati eksi çalıştırma başlangıç zamanına eşit olarak düzeltildi.
    + Otomatik ml kullanırken, bir yol otomatik Mlconfig nesnesine geçirilir ve zaten mevcut değilse otomatik olarak oluşturulur.
    + Kullanıcılar artık parametresini kullanarak tahmin görevleri için bir zaman serisi frekansı belirtebilir `freq` .
  + **azureml-train-automl-runtime**
    + En iyi model açıklamaları başarısız olduğunda, geliştirilmiş konsol çıkışı.
    + "Backlist_models" giriş parametresi "blocked_models" olarak yeniden adlandırıldı.
      + "Whitelist_models" giriş parametresi "allowed_models" olarak yeniden adlandırıldı.
    + Kullanıcılar artık parametresini kullanarak tahmin görevleri için bir zaman serisi frekansı belirtebilir `freq` .

  
## <a name="2020-07-06"></a>2020-07-06

### <a name="azure-machine-learning-sdk-for-python-v190"></a>Python v 1.9.0 için SDK Azure Machine Learning

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **azureml-automl-core**
    + Get_model_path (), otomatik ml otomatik olarak oluşturulan Puanlama betiğinde AZUREML_MODEL_DIR ortam değişkeniyle değiştirdi. Ayrıca, Init () sırasında oluşan sorunları izlemek için telemetri eklenmiştir.
    + , `enable_cache` Oto Mlconfig 'in bir parçası olarak belirtme özelliği kaldırıldı
    + Belirli bir tahmin çalıştırmaları sırasında çalışan hizmet hatalarıyla başarısız olabileceği bir hata düzeltildi
    + Sırasında belirli modeller etrafında geliştirilmiş hata işleme`get_output`
    + Y transformatörü ile sınıflandırma için fitted_model. fit (X, y) çağrısı düzeltildi
    + Foratama görevleri için özelleştirilmiş ileri sarma dolgusu etkinleştirildi
    + Bir dict biçiminde tahmin parametreleri yerine yeni bir Foredıseingparameters sınıfı kullanılacak
    + Geliştirilmiş hedef öteleme oto algılaması
    + BERT ile çok işlemli, çok GPU dağıtılmış bir şekilde sınırlı kullanılabilirlik eklendi
  + **azureml-automl-runtime**
    + Prophet artık, çarpma yerine ek mevsimsellik modellemesini de kapsar.
    + Kısa graa 'nın uzun yerlerdeki sıklıklardan farklı olması nedeniyle oluşan sorun düzeltildi, başarısız çalıştırmaların oluşmasına neden olur.
  + **azureml-contrib-oto ml-DNN-Vision**
    + Eğitim ve Puanlama için sistem/GPU istatistiklerini ve günlük ortalamaları toplayın
  + **azureml-contrib-mir**
    + Managedinferferklu Enable-App-Insights bayrağı için destek eklendi
  + **azureml-core**
    + Veri kaynağına geçerli işlem tarafından erişilemezse doğrulamanın atlanmasını sağlayarak bu API 'ye bir doğrulama parametresi.
      + TabularDataset. time_before (end_time, include_boundary = true, Validate = true)
      + TabularDataset. time_after (start_time, include_boundary = true, Validate = true)
      + TabularDataset. time_recent (time_delta, include_boundary = true, Validate = true)
      + TabularDataset. time_between (start_time, end_time, include_boundary = true, Validate = true)
    + Model listesi için çerçeve filtreleme desteği eklendi ve not defterine geri dönmek için NCD otomatik ml örneği eklendi
    + Veri deposu için. register_azure_blob_container ve veri deposu. register_azure_file_share (yalnızca SAS belirtecini destekleyen Seçenekler) için, alan için belge dizelerini, `sas_token` tipik okuma ve yazma senaryoları için en düşük izin gereksinimlerini içerecek şekilde güncelleştirdik.
    + Ws. get_mlflow_tracking_uri () içindeki _with_auth param 'ı kullanımdan kaldırma
  + **azureml-mlflow**
    + AzureML-MLflow ile yerel file://modellerini dağıtmaya yönelik destek ekleme
    + Ws. get_mlflow_tracking_uri () içindeki _with_auth param 'ı kullanımdan kaldırma
  + **azureml-opendatasets**
    + Son yayınlanan Covıd-19 izleme veri kümeleri artık SDK ile kullanılabilir
  + **azureml-pipeline-core**
    + "Azureml-Varsayılanlar", PIP bağımlılığının parçası olarak dahil edilmediğinden günlüğe çıkış uyarısı
    + Notun işlemesini geliştirme.
    + Ayrılmış dosyalar PipelineOutputFileDataset 'e ayrıştırılırken alıntı yapılan satır sonları için destek eklendi.
    + PipelineDataset sınıfı kullanım dışıdır. Daha fazla bilgi için bkz. https://aka.ms/dataset-deprecation. Veri kümesini ardışık düzen ile nasıl kullanacağınızı öğrenin, bkz https://aka.ms/pipeline-with-dataset ..
  + **azureml-pipeline-steps**
    + Belge, azureml-işlem hattı-işlem adımları için güncelleştirmeler.
    +  `load_yaml()`Kullanıcıların, yapılandırma geri kalanı ile veya ayrı bir dosyada ortamları satır içinde tanımlamasını sağlamak Için ParallelRunConfig 'e destek eklendi
  + **azureml-tren-oto ml-Client**.
    + , `enable_cache` Oto Mlconfig 'in bir parçası olarak belirtme özelliği kaldırıldı
  + **azureml-train-automl-runtime**
    + BERT ile çok işlemli, çok GPU dağıtılmış bir şekilde sınırlı kullanılabilirlik eklendi.
    + ADB tabanlı otomatik makine öğrenimi çalıştırmaları içinde uyumsuz paketlere yönelik hata işleme eklendi.
  + **azureml-widgets**
    + Belge, azureml-pencere öğeleri için güncelleştirmeler.

  
## <a name="2020-06-22"></a>2020-06-22

### <a name="azure-machine-learning-sdk-for-python-v180"></a>Python v 1.8.0 için SDK Azure Machine Learning
  
  + **Önizleme özellikleri**
    + **azureml-contrib-eşitliği** `azureml-contrib-fairness`Paket, açık kaynak eşitliği değerlendirmesi ve unfairness azaltma paketi [Fairlearn](https://fairlearn.github.io) ve Azure Machine Learning Studio arasında tümleştirme sağlar. Bu paket özellikle, model eşitliği değerlendirme panoları 'nın bir AzureML çalışmasının parçası olarak karşıya yüklenmesini ve Azure Machine Learning Studio 'da gösterilmesini sağlar

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **Azure-CLI-ml**
    + Başlatma kapsayıcısı günlüklerini alma desteği.
    + ComputeInstance yönetmek için yeni CLı komutları eklendi
  + **azureml-automl-core**
    + Kullanıcılar artık, timeseries görevleri için yığın zenginit yinelemesinin büyük olasılıkla uyabilecek olduğunu belirten bir uyarı vererek etkinleştirebiliyor.
    + `azureml.automl.core.shared.exceptions.CacheStoreCorruptedException`Önbellek deposu içeriği oynanmışsa oluşturulan yeni bir kullanıcı özel durumu eklendi
  + **azureml-automl-runtime**
    + Kullanıcı, bir şekilde devre dışı bırakıldığında sınıf Dengeleme özelliği artık etkinleştirilmeyecektir.  
  + **azureml-contrib-IOP**
    + CmAks işlem türü destekleniyor. Eğitim işi için kendi AKS kümenizi çalışma alanına ekleyebilirsiniz.
  + **azureml-contrib-notebook**
    + Belge, azureml-contrib-Not defteri paketine yönelik iyileştirmeler.
  + **azureml-contrib-pipeline-steps**
    + Belge, azureml-contrib--işlem hattı-adımlar paketine yönelik iyileştirmeler.
  + **azureml-core**
    + Müşterinin çalışma alanı bağlantı kaynağı üzerinde çalışması için set_connection, get_connection, list_connections, delete_connection işlevleri ekleyin
    + Belgeler azureml-Coore/azureml. Exceptions paketine yönelik güncelleştirmeler.
    + Azureml-çekirdek paketine belge güncelleştirmeleri.
    + ComputeInstance sınıfında belge güncelleştirmeleri.
    + Belge azureml-Core/azureml. Core. COMPUTE paketine yönelik iyileştirmeler.
    + Azureml, sanal sunucudan WebService ile ilgili sınıflar için iyileştirmeler.
    + Profil oluşturma verilerini depolamak için Kullanıcı tarafından seçilen veri deposunu destekleme
    + Model listesi API 'SI için Expand ve page_count özelliği eklendi
    + Üzerine yazma özelliğini kaldıran düzeltilen hata, gönderilen çalıştırmanın seri kaldırma hatasıyla başarısız olmasına neden olur.
    + Tek bir dosyaya başvuran bir dosya veri kümesi indirilirken veya bağlanırken tutarsız bir klasör yapısı düzeltildi.
    + Parquet dosyalarının bir veri kümesini yükleme to_spark_dataframe artık daha hızlıdır ve tüm Parquet ve Spark SQL veri türlerini destekler.
    + Başlatma kapsayıcısı günlüklerini alma desteği.
    + Parallel ml çalıştırmaları artık paralel çalıştırma adımının alt çalıştırması olarak işaretlenir.
  + **azureml-datadrift**
    + Belge, azureml-contrib-Not defteri paketine yönelik iyileştirmeler.
  + **azureml-dataprep**
    + Parquet dosyalarının bir veri kümesini yükleme to_spark_dataframe artık daha hızlıdır ve tüm Parquet ve Spark SQL veri türlerini destekler.
    + To_pandas_dataframe için OutOfMemory sorunu durumunda daha iyi bellek işleme.
  + **azureml-interpret**
    + Yükseltilmiş azureml-topluluk sürüm 0,12 ' i kullanacak şekilde yorumlayın. *
  + **azureml-mlflow**
    + Belge için azureml-mlflow geliştirmeleri.
    + MLFlow ile AML modeli kayıt defteri desteği ekler.
  + **azureml-opendatasets**
    + Python 3,8 için destek eklendi
  + **azureml-pipeline-core**
    + `PipelineDataset`Uygulamasının, bir iç sınıf olduğundan emin olmak için güncelleştirilmiş belgeleri.
    + Bir bağımsız değişken için birden çok değeri kabul etmek üzere ParallelRunStep Updates, örneğin: "--group_column_names", "Sütun1", "Col2", "Col3"
    + İşlem hatlarında, ara veri kullanımı için passthru_automl_config gereksinimi, oto Mlstep ile kaldırıldı.
  + **azureml-pipeline-steps**
    + Belge, azureml-işlem hattı adımları paketine yönelik iyileştirmeler.
    + İşlem hatlarında, ara veri kullanımı için passthru_automl_config gereksinimi, oto Mlstep ile kaldırıldı.
  + **azureml-telemetry**
    + Belge için azureml-telemetri geliştirmeleri.
  + **azureml-train-automl-client**
    + `experiment.submit()`Bir nesne üzerinde iki kez çağrılan bir hata düzeltildi `AutoMLConfig` , farklı davranış ile sonuçlandı.
    + Kullanıcılar artık, timeseries görevleri için yığın zenginit yinelemesinin büyük olasılıkla uyabilecek olduğunu belirten bir uyarı vererek etkinleştirebiliyor.
    + Hizmet Kullanıcı hatası oluşturursa UserErrorException 'yı yükseltmek için, oto ml çalıştırma davranışı değiştirildi
    + Uzaktan işlem hedefinde bir oto ml denemesi gerçekleştirirken azureml_automl. log ' ın oluşturulmamasına veya eksik Günlükler oluşmasına neden olan bir hatayı düzeltir.
    + İmletilmiş sınıflarla sınıflandırma veri kümeleri için, özellik Sweeper, alt örneklenen veriler için bu değeri belirlerse, ağırlık dengelemesi, sınıflandırma görevinin performansını belirli bir eşikle artırır.
    + Parallel ml çalıştırmaları artık paralel çalıştırma adımının alt çalıştırması olarak işaretlenir.
  + **azureml-train-automl-runtime**
    + Hizmet Kullanıcı hatası oluşturursa UserErrorException 'yı yükseltmek için, oto ml çalıştırma davranışı değiştirildi
    + Parallel ml çalıştırmaları artık paralel çalıştırma adımının alt çalıştırması olarak işaretlenir.

  
## <a name="2020-06-08"></a>2020-06-08

### <a name="azure-machine-learning-sdk-for-python-v170"></a>Python v 1.7.0 için SDK Azure Machine Learning

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **Azure-CLI-ml**
    + CLI komutlarını ve paket bağımlılıklarını temizleyerek model profili oluşturma işleminin mir contrib 'den kaldırılması tamamlandı, model profil oluşturma çekirdek içinde kullanılabilir.
    + En düşük Azure CLI sürümünü 2.3.0 sürümüne yükseltir
  + **azureml-automl-core**
    + Özel Transformatör parametreleri nedeniyle fit_transform () adımında daha iyi özel durum iletisi.
    + Otomatikleştirilmiş ML 'de BERT gibi derin öğrenme transformatörü modelleri için birden çok dil desteği ekleyin.
    + Kullanım dışı lag_length parametresini belgelerden kaldırın.
    + Tahmin parametreleri belgeleri geliştirildi. Lag_length parametresi kullanım dışı bırakıldı.
  + **azureml-automl-runtime**
    + Tahmin/test sırasında kategorik sütunlardan biri boş olduğunda oluşan hata düzeltildi.
    + Geri dönüş özellikleri etkinleştirildiğinde ve veriler kısa grasınlar içerdiğinde oluşan çalıştırma başarısızlıklarını düzeltir.
    + Lags veya sıralı pencereler ' Auto ' olarak ayarlandığında yinelenen zaman dizini hata iletisiyle ilgili sorun düzeltildi.
    + Geriye doğru özelliklerini içeren veri kümelerinde Prophet ve ARIMA modelleriyle ilgili sorun düzeltildi.
    + Tahmin görevlerinde 1677-09-21 veya daha sonra tarih ve saat içinde 2262-04-11 öncesi tarih desteği eklendi. Geliştirilmiş hata iletileri.
    + Tahmin parametreleri belgeleri geliştirildi. Lag_length parametresi kullanım dışı bırakıldı.
    + Özel Transformatör parametreleri nedeniyle fit_transform () adımında daha iyi özel durum iletisi.
    + Otomatikleştirilmiş ML 'de BERT gibi derin öğrenme transformatörü modelleri için birden çok dil desteği ekleyin.
    + Bazı OSErrors ile sonuçlanan önbellek işlemleri Kullanıcı hatası oluşturacak.
    + Eğitim ve doğrulama verilerinde aynı sayıda ve sütun kümesi olduğundan emin olmak için denetimler eklendi
    + Veriler tırnak işaretleri içerdiğinde otomatik olarak oluşturulan otomatik ml Puanlama betiği ile ilgili sorun düzeltildi
    + Prophet modeli içeren, oto ml Prophet ve Ensembled modelleriyle ilgili açıklamaları etkinleştirme.
    + Son bir müşteri sorunu, sınıf Dengeleme mantığı düzgün bir şekilde etkinleştirilmediğinde bile, mesajların, sınıf dengelemesi ve bir arada olması halinde mesajlar hakkında bir canlı site hatası ortaya çıkardık. Bu PR ile bu Günlükler/iletiler kaldırılıyor.
  + **azureml-CLI-ortak**
    + CLI komutlarını ve paket bağımlılıklarını temizleyerek model profili oluşturma işleminin mir contrib 'den kaldırılması tamamlandı, model profil oluşturma çekirdek içinde kullanılabilir.
  + **azureml-contrib-reinforcementlearning**
    + Yük testi aracı
  + **azureml-core**
    + Script_run_config. Kopyala üzerinde belge değişiklikleri
    + Çalıştırma gönderme-işlem hattı CLı çıkışını yazdırmayla ilgili bir hatayı düzeltir
    + Azureml-Core/azureml. Data 'da belge geliştirmeleri
    + Igetconf komutunu kullanarak depolama hesabı alma sorunu giderilir
    + Geliştirilmiş register_azure_blob_container ve register_azure_file_share belgeleri
  + **azureml-datadrift**
    + Veri kümesi DRFT izleyicileri devre dışı bırakmak ve etkinleştirmek için geliştirilmiş uygulama
  + **azureml-interpret**
    + Açıklama istemcisinde, yapıtlardan karşıya yükleme sırasında JSON serileştirmesinden önce NaNs veya INF dosyalarını kaldırın
    + Birçok özellik ve sınıf ile genel açıklamalar için bellek hatalarını artırmak üzere yorumlama-topluluk ' nin en son sürümüne güncelleştirin
    + .NET Kullanıcı arabirimindeki ek özellikleri etkinleştirmek için açıklama karşıya yükleme true_ys isteğe bağlı parametre ekleyin
    + Download_model_explanations () ve list_model_explanations () performansını iyileştirme
    + Hata ayıklamaya yardımcı olmak için dizüstü bilgisayarlarda küçük tnak 'lar
  + **azureml-opendatasets**
    + azureml-OPENDATASET 'ler için azureml-dataprep sürüm 1.4.0 veya üzeri gerekir. Daha düşük sürüm algılanırsa uyarı eklendi
  + **azureml-pipeline-core**
    + Bu değişiklik, kullanıcının modül çağrılırken moduleVersion öğesine isteğe bağlı bir runconfig sağlamasına izin verir. Publish_python_script.
    + Node hesabını etkinleştir, azureml. Pipeline içindeki ParallelRunStep içindeki bir işlem hattı parametresi olabilir. adımlarda
  + **azureml-pipeline-steps**
    + Bu değişiklik, kullanıcının modül çağrılırken moduleVersion öğesine isteğe bağlı bir runconfig sağlamasına izin verir. Publish_python_script.
  + **azureml-train-automl-client**
    + Otomatikleştirilmiş ML 'de BERT gibi derin öğrenme transformatörü modelleri için birden çok dil desteği ekleyin.
    + Kullanım dışı lag_length parametresini belgelerden kaldırın.
    + Tahmin parametreleri belgeleri geliştirildi. Lag_length parametresi kullanım dışı bırakıldı.
  + **azureml-train-automl-runtime**
    + Prophet modeli içeren, oto ml Prophet ve Ensembled modelleriyle ilgili açıklamaları etkinleştirme.
    + Azureml-tren-oto ml-* paketlerine yönelik belgeler.
  + **azureml-train-core**
    + PyTorch Estimator 'da TensorFlow sürüm 2,1 'i destekleme
    + Azureml-tren-çekirdek paketindeki geliştirmeler.
  
## <a name="2020-05-26"></a>2020-05-26

### <a name="azure-machine-learning-sdk-for-python-v160"></a>Python v 1.6.0 için SDK Azure Machine Learning

+ **Yeni özellikler**
  + **azureml-automl-runtime**
    + Otomatik ml tahmini artık, modeli yeniden eğitmek zorunda kalmadan önceden belirtilmiş maksimum ufkın ötesinde müşterilerin tahminini desteklemektedir. Tahmin hedefi, belirtilen maksimum ufuk süresinden daha ileride olduğunda, tahmin () işlevi, özyinelemeli işlem modunu kullanarak daha sonraki bir tarihe kadar puan tahminleri yapmaya devam edecektir. Yeni özelliğin çizimi için, lütfen "tahmin-tahmin-işlevi" Not defterinin "en büyük ufuk süresinden daha fazla tahmin" bölümüne [bakın.](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning)
  
  + **azureml-pipeline-steps**
    + ParallelRunStep artık serbest bırakılır ve **azureml-işlem hattı-adımlar** paketinin bir parçasıdır. Azureml için mevcut ParallelRunStep **-contrib-Pipeline-Steps** paketi kullanım dışıdır. Genel Önizleme sürümündeki değişiklikler:
      + `run_max_try`Belirli bir toplu iş için Run metoduna maksimum çağrıyı denetlemek için isteğe bağlı yapılandırılabilir parametre eklendi, varsayılan değer 3 ' tür.
      + Artık hiçbir PipelineParameters otomatik olarak oluşturulmamış. Aşağıdaki yapılandırılabilir değerler, açıkça bir PipelineParameter olarak ayarlanabilir.
        + mini_batch_size
        + node_count
        + process_count_per_node
        + logging_level
        + run_invocation_timeout
        + run_max_try
      + Process_count_per_node için varsayılan değer 1 ' e değiştirilir. Kullanıcı daha iyi performans için bu değeri ayarlamanız gerekir. En iyi uygulama, GPU veya CPU düğümü sayısı olarak ayarlanmalıdır.
      + ParallelRunStep hiçbir paket eklemez, kullanıcının ortam tanımında **azureml-Core** ve **azureml-dataprep [Pandas, sigortası]** paketleri içermesi gerekir. User_managed_dependencies ile özel Docker görüntüsü kullanılıyorsa, kullanıcının görüntüye Conda yüklemesi gerekir.
      
+ **Yeni değişiklikler**
  + **azureml-pipeline-steps**
    + , Bir loadmlconfig için geçerli bir giriş türü olarak azureml. dprep. Dataflow kullanımı kullanım dışı bırakıldı
  + **azureml-train-automl-client**
    + , Bir loadmlconfig için geçerli bir giriş türü olarak azureml. dprep. Dataflow kullanımı kullanım dışı bırakıldı

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **azureml-automl-core**
    + İstenen kullanıcının istemciyi indirgeme sırasında bir uyarının yazdırılacağı hata düzeltildi `get_output` .
    + Mac henüz sürüm 10 ' da kullanılamadığından cudatoolkit = 9.0 'a ait olacak şekilde güncelleştirildi.
    + Uzaktan işlem sırasında eğitilen phrophet ve xgboost modelleriyle kısıtlamalar kaldırılıyor.
    + Oto ml 'de geliştirilmiş günlük
    + Tahmin görevlerinde özel özellikler için hata işleme geliştirildi.
    + Kullanıcılara tahmin oluşturmak için sırasında geride özellikleri ekleme izni veren işlevsellik eklenmiştir.
    + Kullanıcı hatasını doğru şekilde görüntülemesi için hata iletisi güncelleştirmeleri.
    + Training_data birlikte kullanılacak cv_split_column_names desteği
    + Özel durum iletisini günlüğe kaydetmeyi ve izleme geri 'yi güncelleştirin.
  + **azureml-automl-runtime**
    + Eksik imputations değerini tahmin etmek için guardrayları etkinleştirin.
    + Oto ml 'de geliştirilmiş günlük
    + Veri hazırlığı özel durumları için ayrıntılı hata işleme eklendi
    + Uzaktan işlem sırasında eğitilen phrophet ve xgboost modelleriyle kısıtlamalar kaldırılıyor.
    + `azureml-train-automl-runtime`ve `azureml-automl-runtime` için,, ve için bağımlılıklar güncelleştirildi `pytorch` `scipy` `cudatoolkit` . Artık `pytorch==1.4.0` , ve ' yi destekliyoruz `scipy>=1.0.0,<=1.3.1` `cudatoolkit==10.1.243` .
    + Tahmin görevlerinde özel özellikler için hata işleme geliştirildi.
    + Tahmin veri kümesi frekansı algılama mekanizması geliştirilmiştir.
    + Bazı veri kümelerinde Prophet modeli eğitiminde sorun düzeltildi.
    + Tahmin sırasında maksimum ufkın otomatik algılanması geliştirilmiştir.
    + Kullanıcılara tahmin oluşturmak için sırasında geride özellikleri ekleme izni veren işlevsellik eklenmiştir.
    +  Tahmin modeline yeniden eğitim olmadan eğitilen UFOnun ötesinde tahminler sağlamaya olanak tanımak için tahmin işlevine işlevsellik ekler.
    + Training_data birlikte kullanılacak cv_split_column_names desteği
  + **azureml-contrib-oto ml-DNN-tahmin**
    + Oto ml 'de geliştirilmiş günlük
  + **azureml-contrib-mir**
    + Managedinferferferde Windows Hizmetleri için destek eklendi
    + MIR COMPUTE, Tekmodelirwebservice sınıfı-Temizleme, contrib-MIR paketine yerleştirilmiş olan eski MIR iş akışlarını kaldırın
  + **azureml-contrib-pipeline-steps**
    + YAML desteği için küçük bir çözüm
    + ParallelRunStep genel kullanılabilirlik-azureml. contrib. Pipeline içinde kullanıma sunuldu. adımlarda kullanımdan kaldırma bildirimi vardır ve azureml. Pipeline öğesine taşınır. adımlara
  + **azureml-contrib-reinforcementlearning**
    + RL yük testi aracı
    + RL tahmin aracı 'da akıllı varsayılanlar vardır
  + **azureml-core**
    + MIR COMPUTE, Tekmodelirwebservice sınıfı-Temizleme, contrib-MIR paketine yerleştirilmiş olan eski MIR iş akışlarını kaldırın
    + Profil oluşturma hatası durumunda kullanıcıya sağlanan bilgiler düzeltildi: eklenen istek kimliği ve iletiyi daha anlamlı olacak şekilde yeniden paketleyin. Profil oluşturmak için yeni profil oluşturma iş akışı eklendi
    + Veri kümesi yürütme hataları durumunda önemli ölçüde geliştirilmiş hata metni.
    + Çalışma alanı özel bağlantısı CLı desteği eklendi.
    + `invalid_lines` `Dataset.Tabular.from_json_lines_files` Geçersiz JSON içeren satırların nasıl işleneceğini belirtmeye izin veren için isteğe bağlı bir parametre eklendi.
    + Sonraki sürümde işlem tabanlı işlem oluşturmayı kullanımdan kaldıracağız. Kalıcı bir işlem hedefi olarak gerçek bir Amlcompute kümesi oluşturmanızı ve küme adını çalıştırma yapılandırmanızda işlem hedefi olarak kullanmanızı öneririz. Örnek Not defteri buraya bakın: aka.ms/amlcomputenb
    + Veri kümesi yürütme hataları durumunda önemli ölçüde geliştirilmiş hata iletileri.
  + **azureml-dataprep**
    + Pyarrow sürümünü daha açık bir şekilde yükseltmek için uyarı yapıldı.
    + Veri akışı yürütme hatası durumunda geliştirilmiş hata işleme ve ileti döndürüldü.
  + **azureml-interpret**
    + Belge, azureml için güncelleştirme paketi.
    + En son SKLE uyumlu olacak şekilde düzeltilen düzeltme paketleri ve Not defterleri
  + **azureml-opendatasets**
    + hiçbir veri döndürülmediğinde, None döndürün.
    + To_pandas_dataframe performansını geliştirir.
  + **azureml-pipeline-core**
    + YAML 'den yüklemenin bozuk olduğu ParallelRunStep için hızlı düzelme
    + ParallelRunStep genel kullanılabilirlik-azureml. contrib. Pipeline içinde kullanıma sunuldu. adımlarda kullanımdan kaldırma bildirimi vardır ve azureml. Pipeline öğesine taşınır. adımlar-yeni özellikler şunlardır: 1. Veri kümeleri, PipelineParameter 2 olarak. Yeni parametre run_max_retry 3. Yapılandırılabilir append_row çıkış dosyası adı
  + **azureml-pipeline-steps**
    + Giriş verileri için geçerli bir tür olarak kullanımdan kaldırılan azureml. dprep. Dataflow.
    + YAML 'den yüklemenin bozuk olduğu ParallelRunStep için hızlı düzelme
    + ParallelRunStep, genel kullanılabilirlik-azureml. contrib. Pipeline ' de kullanıma sunuldu. adımlarda kullanımdan kaldırma bildirimi vardır ve azureml. Pipeline öğesine taşınır. adımlar-yeni özellikler şunlardır:
      + Pipelineparametresi olarak veri kümeleri
      + Yeni parametre run_max_retry
      + Yapılandırılabilir append_row çıkış dosyası adı
  + **azureml-telemetry**
    + Özel durum iletisini günlüğe kaydetmeyi ve izleme geri 'yi güncelleştirin.
  + **azureml-train-automl-client**
    + Oto ml 'de geliştirilmiş günlük
    + Kullanıcı hatasını doğru şekilde görüntülemesi için hata iletisi güncelleştirmeleri.
    + Training_data birlikte kullanılacak cv_split_column_names desteği
    + Giriş verileri için geçerli bir tür olarak kullanımdan kaldırılan azureml. dprep. Dataflow.
    + Mac henüz sürüm 10 ' da kullanılamadığından cudatoolkit = 9.0 'a ait olacak şekilde güncelleştirildi.
    + Uzaktan işlem sırasında eğitilen phrophet ve xgboost modelleriyle kısıtlamalar kaldırılıyor.
    + `azureml-train-automl-runtime`ve `azureml-automl-runtime` için,, ve için bağımlılıklar güncelleştirildi `pytorch` `scipy` `cudatoolkit` . Artık `pytorch==1.4.0` , ve ' yi destekliyoruz `scipy>=1.0.0,<=1.3.1` `cudatoolkit==10.1.243` .
    + Kullanıcılara tahmin oluşturmak için sırasında geride özellikleri ekleme izni veren işlevsellik eklenmiştir.
  + **azureml-train-automl-runtime**
    + Oto ml 'de geliştirilmiş günlük
    + Veri hazırlığı özel durumları için ayrıntılı hata işleme eklendi
    + Uzaktan işlem sırasında eğitilen phrophet ve xgboost modelleriyle kısıtlamalar kaldırılıyor.
    + `azureml-train-automl-runtime`ve `azureml-automl-runtime` için,, ve için bağımlılıklar güncelleştirildi `pytorch` `scipy` `cudatoolkit` . Artık `pytorch==1.4.0` , ve ' yi destekliyoruz `scipy>=1.0.0,<=1.3.1` `cudatoolkit==10.1.243` .
    + Kullanıcı hatasını doğru şekilde görüntülemesi için hata iletisi güncelleştirmeleri.
    + Training_data birlikte kullanılacak cv_split_column_names desteği
  + **azureml-train-core**
    + Yeni bir HyperDrive özel durum kümesi eklendi. azureml. tren. Hyperdrive artık ayrıntılı özel durumlar oluşturacak.
  + **azureml-widgets**
    + AzureML, Jupyıterlab içinde gösterilmiyor
  

## <a name="2020-05-11"></a>2020-05-11

### <a name="azure-machine-learning-sdk-for-python-v150"></a>Python v 1.5.0 için SDK Azure Machine Learning

+ **Yeni özellikler**
  + **Önizleme özellikleri**
    + **azureml-contrib-reinforcementlearning**
        + Azure Machine Learning, [Ray](https://ray.io) Framework kullanarak pekiştirmeye dayalı Learning için Önizleme desteğini serbest bırakdır. , `ReinforcementLearningEstimator` Azure Machine Learning 'DAKI GPU ve CPU işlem hedefleri genelinde pekiştirmeye dayalı Learning aracılarının eğitimine izin vermez.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **Azure-CLI-ml**
    + Önceki bir PR 'de yanlışlıkla kalan bir uyarı günlüğünü düzeltir. Günlük hata ayıklama için kullanıldı ve yanlışlıkla geride bırakılmış.
    + Hata çözme: istemcilere profil oluşturma sırasında kısmen hata hakkında bilgi bildirme
  + **azureml-automl-core**
    + Veri kümelerinin birden çok zaman serisi olduğunda, zaman serisine paralel sığdırma 'yı etkinleştirerek, hızlı bir şekilde, Prophet/oto ARIMA modelini hızlandırın. Bu yeni özellikten yararlanabilmek için, oto Mlconfig içindeki "max_cores_per_iteration =-1" (yani, tüm kullanılabilir CPU çekirdeklerini kullanarak) ayarlamanız önerilir.
    + Konsol arabirimindeki guardrayları yazdırırken KeyError 'ı çözme
    + Experimentation_timeout_hours için düzeltilen hata iletisi
    + Oto ml için kullanım dışı TensorFlow modelleri.
  + **azureml-automl-runtime**
    + Experimentation_timeout_hours için düzeltilen hata iletisi
    + Önbellek deposundan serisini kaldırma girişimi sırasında sınıflandırılmayan özel durum düzeltildi
    + Veri kümelerinin birden çok zaman serisi olduğunda, zaman serisine paralel sığdırma 'yı etkinleştirerek, hızlı bir şekilde, Prophet/oto ARIMA modelini hızlandırın.
    + Test/tahmin kümesinin eğitim kümesinden bir tanesi içermediği veri kümelerinde etkin sıralı pencere ile tahmin düzeltildi.
    + Eksik verilerin işlenmesi geliştirildi
    + Zaman içinde hizalı olmayan veri kümelerinde tahmin sırasındaki tahmin aralıklarıyla ilgili sorun düzeltildi.
    + Tahmin görevleri için veri şeklinin daha iyi doğrulanması eklendi.
    + Sıklık algılaması geliştirildi.
    + Tahmin görevleri için çapraz doğrulama alanları üretilemez, daha iyi hata iletisi oluşturuldu.
    + Eksik olan guardkııl değerini doğru yazdırmak için konsol arabirimini düzeltir.
    + Oto Mlconfig içinde cv_split_indices girişte veri türü denetimleri zorlanıyor.
  + **azureml-CLI-ortak**
    + Hata çözme: istemcilere profil oluşturma sırasında kısmen hata hakkında bilgi bildirme
  + **azureml-contrib-mir**
    + Şu anda dağıtılmış olan MIR düzeltmesi ve Kullanıcı tarafından belirtilen en son sürüm hakkında bilgi aktaran bir azureml. contrib. mir. RevisionStatus sınıfı ekler. Bu sınıf, ' deployment_status ' özniteliği altındaki Mwebservice nesnesine dahil edilmiştir.
    + Mwebservice türü ve onun alt sınıfı Singlemodelirwebservice olan WebServices üzerinde güncelleştirmeye izin vermez.
  + **azureml-contrib-reinforcementlearning**
    + Ray 0.8.3 için destek eklendi
    + AmlWindowsCompute yalnızca bağlı depolama olarak Azure dosyalarını destekler
    + Health_check_timeout_seconds health_check_timeout yeniden adlandırıldı
    + Bazı sınıf/yöntem açıklamaları düzeltildi.
  + **azureml-core**
    + Uskamu ve Çin bulutlarında > blob dönüştürmeleri etkinleştirildi.
    + Okuyucu rollerinin kullanmasına izin vermek için hatayı düzeltir az ml çalıştırma bilgilerini almak için CLı komutlarını çalıştır
    + Giriş veri kümeleri ile Azure ML uzak çalıştırmaları sırasında gereksiz günlük kaydı kaldırıldı.
    + RCranPackage artık CRAN paket sürümü için "Version" parametresini destekliyor.
    + Hata çözme: istemcilere profil oluşturma sırasında kısmen hata hakkında bilgi bildirme
    + Azureml-Core için Avrupa stili kayan işleme eklendi.
    + Azure ML SDK 'da çalışma alanı özel bağlantı özellikleri etkinleştirildi.
    + Kullanarak bir TabularDataset oluştururken `from_delimited_files` , Boolean bağımsız değişkenini ayarlayarak boş değerlerin yok veya boş dize olarak yüklenip yüklenmeyeceğini belirtebilirsiniz `empty_as_string` .
    + Veri kümeleri için Avrupa stili kayan işleme eklendi.
    + Veri kümesi bağlama hatalarında geliştirilmiş hata iletileri.
  + **azureml-datadrift**
    + SDK 'daki veri drift sonuçları sorgusunda, en düşük, en yüksek ve ortalama Özellik ölçümlerini ayırt etmediğiniz bir hata oluştu ve bu değer yinelenen değerlere neden olur. Bu hatayı, hedef veya taban çizgisini ölçüm adlarına önek olarak çağırarak düzelttik. Önce: yinelenen min, Max, ortalama. Sonra: target_min, target_max, target_mean, baseline_min, baseline_max, baseline_mean.
  + **azureml-dataprep**
    + Veri teslimi için gerekli .NET bağımlılıklarını güvence altına alırken, yazma kısıtlı Python ortamlarının işlenmesini geliştirme.
    + Baştaki boş kayıtları olan dosyada sabit veri akışı oluşturma.
    + `to_partition_iterator`Şuna benzer şekilde hata işleme seçenekleri eklendi `to_pandas_dataframe` .
  + **azureml-interpret**
    + Windows sınırını azaltma olasılığını azaltmak için daha fazla açıklama yolu uzunluğu sınırları
    + Doğrusal bir yedek model kullanılarak açıklama benzeimiyle oluşturulan seyrek açıklamalar için bugdüzeltmesini.
  + **azureml-opendatasets**
    + MNIST 'nin sütunlarının sorunu, int olması gereken dize olarak ayrıştırılır.
  + **azureml-pipeline-core**
    + Bir ModuleStep içine gömülü bir modül kullanılırken regenerate_outputs seçeneğe izin verme.
  + **azureml-train-automl-client**
    + Oto ml için kullanım dışı TensorFlow modelleri.
    + Kullanıcıları, desteklenmeyen algoritmaların yerel modda listelemesine izin verir
    + Belge, Cmlconfig için düzeltir.
    + Oto Mlconfig içinde cv_split_indices girişte veri türü denetimleri zorlanıyor.
    + Show_output başarısız olan oto ml çalıştırmasına sorun düzeltildi
  + **azureml-train-automl-runtime**
    + Model indirme zaman aşımını başarıyla iade durumundan çıkarılan yineleme sırasında bir hatayı düzeltme.
  + **azureml-train-core**
    + Azureml. DNN. nccl sınıfında yazım hatası 'ı düzeltir.
    + PyTorch Estimator 'da PyTorch sürüm 1,5 ' i destekleme
    + Eğitim çerçevesi tahmini 'ı kullanırken Framework görüntüsünün Fairfax bölgesinde getirilmediği sorunu çözme

  
## <a name="2020-05-04"></a>2020-05-04
**Yeni Not defteri deneyimi**

Artık Azure Machine Learning doğrudan stüdyo Web deneyiminin içinde Machine Learning not defterlerini ve dosyalarını oluşturabilir, düzenleyebilir ve paylaşabilirsiniz. [Azure Machine Learning Python SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) ' da bulunan tüm sınıfları ve yöntemleri bu not defterleri içinde [kullanmaya başlayın.](https://docs.microsoft.com/azure/machine-learning/how-to-run-jupyter-notebooks)

**Sunulan yeni özellikler:**

+ VS Code tarafından kullanılan geliştirilmiş Düzenleyici (Monako Düzenleyicisi) 
+ UI/UX geliştirmeleri
+ Hücre araç çubuğu
+ Yeni Not defteri araç çubuğu ve Işlem denetimleri
+ Not defteri durum çubuğu 
+ Satır içi çekirdek değiştirme
+ R desteği
+ Erişilebilirlik ve yerelleştirme geliştirmeleri
+ Komut paleti
+ Ek klavye kısayolları
+ Gönderil
+ İyileştirilmiş performans ve güvenilirlik

Studio 'dan aşağıdaki Web tabanlı yazma araçlarına erişin:
    
| Web tabanlı araç  |     Açıklama  | Sürüm | 
|---|---|---|
| Azure ML Studio Not defterleri   |     Not defteri dosyaları için birinci sınıf yazma ve Azure ML Python SDK 'da bulunan tüm işlemleri destekleme. | Temel & kurumsal  |   

## <a name="2020-04-27"></a>2020-04-27

### <a name="azure-machine-learning-sdk-for-python-v140"></a>Python v 1.4.0 için SDK Azure Machine Learning

+ **Yeni özellikler**
  + AmlCompute kümeleri, sağlama sırasında kümede yönetilen bir kimlik ayarlamayı desteklemektedir. Yalnızca sistem tarafından atanan bir kimlik veya Kullanıcı tarafından atanan bir kimlik kullanmak isteyip istemediğinizi belirtin ve ikinci durum durumunda bir IdentityId geçirin. Daha sonra, depolama veya ACR gibi çeşitli kaynaklara erişim izinleri ayarlayarak, AmlCompute 'in bugün kullandığı belirteç tabanlı bir yaklaşım yerine, verilerin güvenli bir şekilde veriye erişmesi için kullanılması gerekir. Parametreler hakkında daha fazla bilgi için SDK başvurumuza göz atın.
  

+ **Yeni değişiklikler**
  + AmlCompute kümeleri, çalışma tabanlı oluşturma sürecinin iki haftada kullanım dışı bırakıldığımızda bir önizleme özelliği destekliyordu. Her zaman Amlcompute sınıfını kullanarak kalıcı işlem hedefleri oluşturmaya devam edebilirsiniz, ancak Run config içinde işlem hedefi olarak "amlcompute" tanımlayıcısını belirtmeyle ilgili yaklaşım yakın gelecekte desteklenmeyecektir. 

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **azureml-automl-runtime**
    + Sütundaki benzersiz değerlerin sayısını hesaplarken hashable türü desteğini etkinleştirin.
  + **azureml-core**
    + TabularDataset kullanılarak Azure Blob depolama 'dan okurken iyileştirilmiş kararlılık.
    + Parametresi için geliştirilmiş belgeler `grant_workspace_msi` `Datastore.register_azure_blob_store` .
    + `datastore.upload` `src_dir` Veya ile biten bağımsız değişkeni desteklemek için ile hata düzeltildi `/` `\` .
    + Erişim anahtarı veya SAS belirteci olmayan bir Azure Blob depolama veri deposuna yüklenmeye çalışıldığında işlem yapılabilir hata iletisi eklendi.
  + **azureml-interpret**
    + Karşıya yüklenen açıklamaları üzerindeki görselleştirme verilerinin dosya boyutuna üst sınır eklendi.
  + **azureml-train-automl-client**
    + Label_column_name & weight_column_name parametrelerin String türünde olması için açıkça denetleniyor.
  + **azureml-contrib-pipeline-steps**
    + ParallelRunStep artık veri kümesini ardışık düzen parametresi olarak destekliyor. Kullanıcı, örnek veri kümesiyle işlem hattı oluşturabilir ve yeni işlem hattı çalıştırması için aynı türdeki (dosya veya tablo) giriş veri kümesini değiştirebilir.

  
## <a name="2020-04-13"></a>2020-04-13

### <a name="azure-machine-learning-sdk-for-python-v130"></a>Python v 1.3.0 için SDK Azure Machine Learning

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **azureml-automl-core**
    + Eğitim sonrası işlemleri etrafında ek telemetri eklendi.
    + 100 'den daha uzun bir süre boyunca Toplam kareler (CSS) eğitimi kullanarak otomatik ARıMA eğitimini hızlandırır. Kullanılan uzunluğun,/src/azureml-automl-Core/azureml/automl/Core/Shared/Constants.py adresindeki Timeseriesınternal sınıfında ARIMA_TRIGGER_CSS_TRAINING_LENGTH bir sabit olarak depolandığını unutmayın.
    + Tahmin çalıştırmalarının Kullanıcı günlüğü geliştirildi, şu anda çalışmakta olan aşama hakkında daha fazla bilgi günlükte gösterilsin
    + İzin verilmeyen target_rolling_window_size 2 ' den küçük değerlere ayarlanamaz
  + **azureml-automl-runtime**
    + Yinelenen zaman damgaları bulunduğunda gösterilen hata iletisi geliştirildi.
    + İzin verilmeyen target_rolling_window_size 2 ' den küçük değerlere ayarlanamaz.
    + Öteleme imputation hatası düzeltildi. Bu sorun, bir seriyi önemli ölçüde çıkarmak için gereken sayıda gözlemden kaynaklandı. "Mevsimsel hale getirilmiş" veriler, öteleme uzunluğunu belirlemede kısmi bir oto bağıntı işlevini (PACF) hesaplamak için kullanılır.
    + Uygun bir yapılandırmaya göre tahmin görevleri için sütun amacı özelleştirmesi etkin. Tahmin görevleri için sayısal ve kategorik olarak sütun amacı desteklenmektedir.
    + Kaldırma yapılandırmasına göre tahmin görevleri için bırakma sütununun etkin bir şekilde özelleştirilmesi.
    + Uygun yapılandırmaya göre tahmin görevleri için imputation özelleştirmesini etkinleştirdi. Hedef sütun ve ortalama, ortanca, most_frequent ve sabit değer imputation eğitim verileri için imputation sabit değeri artık desteklenmektedir.
  + **azureml-contrib-pipeline-steps**
    + ParallelRunConfig 'e geçirilecek dize işlem adlarını kabul et
  + **azureml-core**
    +  Ortam nesnesinin bir kopyasını oluşturmak için Environment. Clone (new_name) API 'SI eklendi
    +  Environment.docker. base_dockerfile, FilePath 'i kabul eder. Bir dosya çözümleyemezse, içerik base_dockerfile ortam özelliği olarak okunacak
    + Environment.docker 'da Kullanıcı el ile bir değer ayarladığında base_image ve base_dockerfile için birbirini dışlayan değerleri otomatik olarak Sıfırla
    + Ortamın Kullanıcı veya AzureML tarafından yönetilip yönetilmediğini belirten RSection öğesine user_managed bayrak eklendi.
    + Veri kümesi: Unicode karakterler içeren veri yolunda sabit veri kümesi indirme hatası.
    + Veri kümesi: Azure Machine Learning Işlem sırasında en düşük disk alanı gereksinimini karşılamak için geliştirilmiş veri kümesi bağlama önbelleği mekanizması, düğümün kullanılamamasına ve işin iptal edilmesine yol açan, bu sayede
    + Veri kümesi: zaman serisi veri çerçevesi olarak bir zaman serisi veri kümesine eriştiğinizde zaman serisi sütunu için bir dizin ekler. Bu, zaman serisi tabanlı veri erişimine erişimi hızlandırmak için kullanılır.  Daha önce, dizine zaman damgası sütunuyla aynı ad veriliyordu. Bu, gerçek zaman damgası sütunu olan ve dizine ait olan kullanıcıları karışyordu. Artık, bir sütun olarak kullanılmadığından bu dizine belirli bir ad vermeyiz. 
    + Veri kümesi: bağımsız bulutu 'nda sabit veri kümesi kimlik doğrulama sorunu.
    + Veri kümesi: `Dataset.to_spark_dataframe` Azure PostgreSQL veri depolarından oluşturulan veri kümelerinde hata düzeltildi.
  + **azureml-interpret**
    + Yerel önem değerleri seyrek ise görselleştirmelere genel puanlar eklendi
    + Azureml güncelleştirildi-topluluk 0,9 ' i kullanmak için yorumlayın. *
    + Seyrek değerlendirme verisi içeren açıklama indirilirken sorun düzeltildi
    + Oto ml 'de açıklama nesnesinin seyrek biçim desteği eklendi
  + **azureml-pipeline-core**
    + İşlem hatları 'nda işlem hedefi olarak ComputeInstance desteği
  + **azureml-train-automl-client**
    + Eğitim sonrası işlemleri etrafında ek telemetri eklendi.
    + Erken durdurulduğunda gerileme düzeltildi
    + Giriş verileri için geçerli bir tür olarak kullanımdan kaldırılan azureml. dprep. Dataflow.
    +  Varsayılan oto denemesi süresi altı güne kadar değiştiriliyor.
  + **azureml-train-automl-runtime**
    + Eğitim sonrası işlemleri etrafında ek telemetri eklendi.
    + seyrek bir oto e2e desteği eklendi
  + **azureml-opendatasets**
    + Service Monitor için ek telemetri eklendi.
    + Kararlılığı artırmak için Blobun ön kapısını etkinleştirin 

## <a name="2020-03-23"></a>2020-03-23

### <a name="azure-machine-learning-sdk-for-python-v120"></a>Python v 1.2.0 için SDK Azure Machine Learning

+ **Yeni değişiklikler**
  + Python 2,7 için bırakma desteği

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **Azure-CLI-ml**
    + CLı 'daki komutlara "--Subscription-ID" ekler `az ml model/computetarget/service`
    + ACı dağıtımı için müşteri tarafından yönetilen anahtar (CMK) vault_url, key_name ve key_version geçirme desteği ekleme
  + **azureml-automl-core** 
    + X ve y veri tahmin görevleri için sabit değer ile özelleştirilmiş imputation etkinleştirildi.
    + ' Deki sorunu, kullanıcıya hata iletilerini göstermeye yönelik olarak düzeltildi.    
  + **azureml-automl-runtime**
    + Yalnızca bir satırla bir satır içeren veri kümelerinde tahminle ilgili sorun düzeltildi
    + Tahmin görevlerinin gerektirdiği bellek miktarını azaltmıştır.
    + Zaman sütunu yanlış biçimde olduğunda daha iyi hata iletileri eklendi.
    + X ve y veri tahmin görevleri için sabit değer ile özelleştirilmiş imputation etkinleştirildi.
  + **azureml-core**
    + Ortam değişkenlerinden ServicePrincipal yükleme desteği eklendi: AZUREML_SERVICE_PRINCIPAL_ID, AZUREML_SERVICE_PRINCIPAL_TENANT_ID ve AZUREML_SERVICE_PRINCIPAL_PASSWORD
    + İçin yeni bir parametre `support_multi_line` eklendi `Dataset.Tabular.from_delimited_files` : varsayılan olarak ( `support_multi_line=False` ), tırnak alan değerleri de dahil olmak üzere tüm satır sonları bir kayıt kesmesi olarak yorumlanır. Bu şekilde verilerin okunması, birden fazla CPU çekirdeği üzerinde paralel yürütme için daha hızlı ve iyileştirilir. Ancak, yanlış hizalanmış alan değerleriyle daha fazla kayıt oluşmasına neden olabilir. Bu `True` , ayrılmış dosyaların tırnak içine alınmış satır sonlarını içermesinin bilindiğinde olarak ayarlanmalıdır.
    + Azure Machine Learning CLı ADLS 2. kaydolma özelliği eklendi
    + ' Fine_grain_timestamp ' parametresi ' timestamp ' ve ' coarse_grain_timestamp ' parametresi, TabularDataset içindeki with_timestamp_columns () yöntemi için ' partition_timestamp ' olarak yeniden adlandırıldı ve parametrelerin kullanımını daha iyi yansıtmaktadır.
    + En fazla deneme adı uzunluğu 255 olarak artar.
  + **azureml-interpret**
    + Toplu olarak güncelleştirildi-0,7-Community 'e yorumlayın. *
  + **azureml-SDK**
    + Yayın öncesi ve kararlı sürümlerde düzeltme eki uygulama için uyumlu sürüm tilde olan bağımlılıklara değiştirme.


## <a name="2020-03-11"></a>2020-03-11

### <a name="azure-machine-learning-sdk-for-python-v115"></a>Python v 1.1.5 için SDK Azure Machine Learning

+ **Özellik kullanımdan kaldırıldı**
  + **Python 2,7**
    + Python 2,7 ' i desteklemeye yönelik son sürüm

+ **Yeni değişiklikler**
  + **Anlamsal Sürüm Oluşturma 2.0.0**
    + Sürüm 1,1 ' den başlayarak Azure ML Python SDK 'Sı 2.0.0 anlam sürümü oluşturma. [Buradan daha fazla bilgi edinin](https://semver.org/). Sonraki tüm sürümler yeni numaralandırma düzenini ve anlamsal sürüm oluşturma sözleşmesini takip edecektir. 

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **Azure-CLI-ml**
    + Tutarlılık için uç nokta CLı komut adını ' az ml Endpoint aks ' iken ' az ml Endpoint Real Time ' olarak değiştirin.
    + kararlı ve deneysel dal CLı için CLı yükleme yönergelerini güncelleştirme
    + Tek örnekli profil oluşturma bir öneri üretmek için düzeltildi ve temel SDK 'da kullanıma sunuldu.
  + **azureml-automl-core**
    + Toplu Işlem modu çıkarımı (birden çok satır bir kez alınıyor), oto ml ONNX modelleri için etkinleştirildi
    + Veri kümelerinde sıklık algılaması geliştirildi, veri yok veya düzensiz veri noktaları içeriyor
    + Baskın sıklığa uyulmayan veri noktalarını kaldırma özelliği eklendi.
    + Oluşturucunun girişi, karşılık gelen sütunlar için imputation seçeneklerini uygulamak üzere seçeneklerin bir listesini alacak şekilde değiştirildi.
    + Hata günlüğü geliştirildi.
  + **azureml-automl-runtime**
    + Bir Gren test kümesinde görünen Eğitim kümesinde mevcut değilse oluşan hata ile ilgili sorun düzeltildi
    + Tahmin hizmeti üzerinde Puanlama sırasında y_query gereksinimi kaldırıldı
    + Veri kümesi uzun zaman aralıkları olan kısa grak içeriyorsa, tahminle ilgili sorun düzeltildi.
    + Otomatik maks. ufuk açık olduğunda ve tarih sütunu dizeler biçimindeki tarihleri içerdiğinde sorun düzeltildi. Tarihe dönüştürme mümkün olmadığında uygun dönüştürme ve hata iletileri eklendi
    + Filecacheser için ara verileri serileştirmek ve seri durumdan çıkarmak için yerel sayısal tuş y ve SciPy kullanma (yerel otomatik ml çalıştırmaları için kullanılır)
    + Başarısız olan alt çalıştırmaların çalışma durumunda takılmasına neden olan bir hata düzeltildi.
    + Artırılan hız artar.
    + Puanlama sırasında sıklık denetimi düzeltildi, artık tahmin görevleri, tren ve test kümesi arasında katı sıklık denkliğine gerek yoktur.
    + Oluşturucunun girişi, karşılık gelen sütunlar için imputation seçeneklerini uygulamak üzere seçeneklerin bir listesini alacak şekilde değiştirildi.
    + Öteleme türü seçimiyle ilgili düzeltilen hatalar.
    + Veri kümelerinde oluşan sınıflandırılmayan hata düzeltildi, tek satır ile grak.
    + Sıklık algılama yavaşlamasıyla ilgili sorun düzeltildi.
    + , Eğitim hatasının bir AttributeError ile değiştirilmesini gerçek sebebini sağlayan, oto ml özel durum işlemede oluşan bir hatayı düzeltir.
  + **azureml-CLI-ortak**
    + Tek örnekli profil oluşturma bir öneri üretmek için düzeltildi ve temel SDK 'da kullanıma sunuldu.
  + **azureml-contrib-mir**
    + Erişim belirtecini almak için Mwebservice sınıfına işlevsellik ekler
    + Mwebservice sırasında varsayılan olarak Mwebservice için belirteç kimlik doğrulamasını kullanın. Run () çağrı-yalnızca çağrı başarısız olursa Yenile
    + Mir WebService dağıtımı artık, sırasıyla [Ds2v2, A2v2 ve F16] yerine uygun SKU 'Lar [Standard_DS2_v2, Standard_F16, Standard_A2_v2] gerektirir.
  + **azureml-contrib-pipeline-steps**
    + ParallelRunStep öğesine side_inputs isteğe bağlı parametre eklendi. Bu parametre, kapsayıcıya klasörü bağlamak için kullanılabilir. Şu anda desteklenen türler DataReference ve PipelineData.
    + ParallelRunConfig içinde geçirilen parametrelerin, ardışık düzen parametreleri şimdi geçirerek üzerine yazılabilir. Desteklenen yeni işlem hattı parametreleri aml_mini_batch_size, aml_error_threshold, aml_logging_level, aml_run_invocation_timeout (aml_node_count ve aml_process_count_per_node zaten önceki sürümün bir parçası).
  + **azureml-core**
    + Dağıtılmış AzureML WebServices, şimdi günlüğe kaydedilecek `INFO` . Bu, `AZUREML_LOG_LEVEL` dağıtılan hizmette ortam değişkeni ayarlanarak denetlenebilir.
    + Python SDK ' işlem hatları ' yerine ' API ' uç noktasını kullanmak için bulma hizmetini kullanır.
    + Tüm SDK çağrılarında yeni yollara değiştirin.
    + Yapılan çağrıların ModelManagementService 'e yeni Birleşik bir yapıya yönlendirilmesi değiştirildi.
      + Çalışma alanı güncelleştirme yöntemi genel olarak kullanılabilir hale getirilir.
      + Kullanıcının görüntü derlemesi için işlem güncelleştirmesine izin vermek üzere çalışma alanı güncelleştirme yönteminde image_build_compute parametresi eklendi.
    + Eski profil oluşturma iş akışına kullanımdan kaldırma iletileri eklendi. Sabit profil oluşturma CPU ve bellek sınırları.
    + R işlerini çalıştırmak için ortamın parçası olarak RSection eklendi.
    + Veri `Dataset.mount` kümesinin kaynağına erişilemezse veya veri içermiyorsa hata vermek için öğesine doğrulama eklendi.
    + `--grant-workspace-msi-access`VNET 'in arkasındaki blob kapsayıcısını kaydetmenizi sağlayacak Azure Blob kapsayıcısını kaydettirmek Için veri deposu CLI için ek bir parametre olarak eklenmiştir.
    + Tek örnekli profil oluşturma bir öneri üretmek için düzeltildi ve temel SDK 'da kullanıma sunuldu.
    + Sorun aks.py _deploy düzeltildi.
    + Sessiz depolama hatalarından kaçınmak için karşıya yüklenen modellerin bütünlüğünü doğrular.
    + Kullanıcı, WebServices için anahtarlar yeniden oluşturulurken kimlik doğrulama anahtarı için bir değer belirtebilir.
    + Büyük harfler veri kümesinin giriş adı olarak kullanılamayan hata düzeltildi.
  + **azureml-varsayılanlar**
    + `azureml-dataprep`, ' nin parçası olarak yüklenir `azureml-defaults` . Veri kümelerini bağlamak için işlem hedeflerine veri Prep [sigortası] el ile yüklemek artık gerekli değildir.
  + **azureml-interpret**
    + Toplu olarak güncelleştirildi-0,6-Community 'e yorumlayın. *
    + Azureml, yorumlamaya (0.5.0) göre güncelleştirildi-Community
    + Azureml-yorumlama için azureml stili özel durumları eklendi
    + Keras modelleri için sabit DeepScoringExplainer serileştirmesi
  + **azureml-mlflow**
    + Azureml. mlflow için bağımsız bulutlar desteği ekleyin
  + **azureml-pipeline-core**
    + Ardışık düzen toplu işlem Puanlama Not defteri artık ParallelRunStep kullanıyor
    + Bağımsız değişken listesini değiştirirken PythonScriptStep sonuçlarının yanlış şekilde yeniden kullanılabildiği bir hata düzeltildi
    + Üzerinde parse_ * yöntemleri çağrılırken sütunları ayarlama özelliği eklendi`PipelineOutputFileDataset`
  + **azureml-pipeline-steps**
    + , `AutoMLStep` `azureml-pipeline-steps` Paketini pakete taşıdı. İçinde kullanım `AutoMLStep` dışı `azureml-train-automl-runtime` .
    + Veri kümesi için PythonScriptStep girişi olarak belge örneği eklendi
  + **azureml-tensorboard**
    + , TensorFlow 2,0 desteği için azureml-tensorboard güncelleştirildi
    + Bir Işlem örneğinde özel bir Tensorboard bağlantı noktası kullanırken doğru bağlantı noktası numarasını göster
  + **azureml-train-automl-client**
    + Belirli paketlerin uzak çalışmalerdeki yanlış sürümlere yüklenebildiği bir sorun düzeltildi.
    + Özel uygulanabilirlik yapılandırmasını filtreleyen sorunu geçersiz kılan sabit Korturizationconfig.
  + **azureml-train-automl-runtime**
    + Uzak çalışmadaki sıklık algılamasıyla ilgili sorun düzeltildi
    + , `AutoMLStep` Paketin içine taşınır `azureml-pipeline-steps` . İçinde kullanım `AutoMLStep` dışı `azureml-train-automl-runtime` .
  + **azureml-train-core**
    + PyTorch Estimator 'da PyTorch sürüm 1,4 ' i destekleme
  
## <a name="2020-03-02"></a>2020-03-02

### <a name="azure-machine-learning-sdk-for-python-v112rc0-pre-release"></a>Python v 1.1.2 RC0 için SDK Azure Machine Learning (ön sürüm)

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **azureml-automl-core**
    + Toplu Işlem modu çıkarımı (birden çok satır bir kez alınıyor), oto ml ONNX modelleri için etkinleştirildi
    + Veri kümelerinde sıklık algılaması geliştirildi, veri yok veya düzensiz veri noktaları içeriyor
    + Baskın sıklığa uyulmayan veri noktalarını kaldırma özelliği eklendi.
  + **azureml-automl-runtime**
    + Bir Gren test kümesinde görünen Eğitim kümesinde mevcut değilse oluşan hata ile ilgili sorun düzeltildi
    + Tahmin hizmeti üzerinde Puanlama sırasında y_query gereksinimi kaldırıldı
  + **azureml-contrib-mir**
    + Erişim belirtecini almak için Mwebservice sınıfına işlevsellik ekler
  + **azureml-core**
    + Dağıtılmış AzureML WebServices, şimdi günlüğe kaydedilecek `INFO` . Bu, `AZUREML_LOG_LEVEL` dağıtılan hizmette ortam değişkeni ayarlanarak denetlenebilir.
    + `Dataset.get_all`Çalışma alanına kayıtlı tüm veri kümelerini döndürmek için yineleme ' i düzeltir.
    + `path`Veri kümesi oluşturma API 'lerinin bağımsız değişkenine geçersiz tür geçirildiğinde hata iletisini geliştirebilirsiniz.
    + Python SDK ' işlem hatları ' yerine ' API ' uç noktasını kullanmak için bulma hizmetini kullanır.
    + Tüm SDK çağrılarında yeni yollara Değiştir
    + ModelManagementService 'e yapılan çağrıların yeni bir birleştirilmiş yapıya yönlendirilmesini değiştirir
      + Çalışma alanı güncelleştirme yöntemi genel olarak kullanılabilir hale getirilir.
      + Kullanıcının görüntü derlemesi için işlem güncelleştirmesine izin vermek üzere çalışma alanı güncelleştirme yönteminde image_build_compute parametresi eklendi
    +  Eski profil oluşturma iş akışına kullanımdan kaldırma iletileri eklendi. Sabit profil oluşturma CPU ve bellek sınırları
  + **azureml-interpret**
    + azureml güncelleştirme-topluluk 0,6. *
  + **azureml-mlflow**
    + Azureml. mlflow için bağımsız bulutlar desteği ekleyin
  + **azureml-pipeline-steps**
    + Öğesini `AutoMLStep` öğesine taşıdı `azureml-pipeline-steps package` . İçinde kullanım `AutoMLStep` dışı `azureml-train-automl-runtime` .
  + **azureml-train-automl-client**
    + Belirli paketlerin uzak çalışmalerdeki yanlış sürümlere yüklenebildiği bir sorun düzeltildi.
  + **azureml-train-automl-runtime**
    + Uzak çalışmadaki sıklık algılamasıyla ilgili sorun düzeltildi
    + Öğesini `AutoMLStep` öğesine taşıdı `azureml-pipeline-steps package` . İçinde kullanım `AutoMLStep` dışı `azureml-train-automl-runtime` .
  + **azureml-train-core**
    + Öğesini `AutoMLStep` öğesine taşıdı `azureml-pipeline-steps package` . İçinde kullanım `AutoMLStep` dışı `azureml-train-automl-runtime` .

## <a name="2020-02-18"></a>2020-02-18

### <a name="azure-machine-learning-sdk-for-python-v111rc0-pre-release"></a>Python v 1.1.1 RC0 için SDK Azure Machine Learning (ön sürüm)

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **Azure-CLI-ml**
    + Tek örnekli profil oluşturma bir öneri üretmek için düzeltildi ve temel SDK 'da kullanıma sunuldu.
  + **azureml-automl-core**
    + Hata günlüğü geliştirildi.
  + **azureml-automl-runtime**
    + Veri kümesi uzun zaman aralıkları olan kısa grak içeriyorsa, tahminle ilgili sorun düzeltildi.
    + Otomatik maks. ufuk açık olduğunda ve tarih sütunu dizeler biçimindeki tarihleri içerdiğinde sorun düzeltildi. Date 'e dönüştürme mümkün değilse uygun dönüştürme ve hatalı hata ekledik
    + Filecacheser için ara verileri serileştirmek ve seri durumdan çıkarmak için yerel sayısal tuş y ve SciPy kullanma (yerel otomatik ml çalıştırmaları için kullanılır)
    + Başarısız olan alt çalıştırmaların çalışma durumunda takılmasına neden olan bir hata düzeltildi.
  + **azureml-CLI-ortak**
    + Tek örnekli profil oluşturma bir öneri üretmek için düzeltildi ve temel SDK 'da kullanıma sunuldu.
  + **azureml-core**
    + `--grant-workspace-msi-access`VNET 'in arkasındaki blob kapsayıcısını kaydetmenizi sağlayacak Azure Blob kapsayıcısını kaydettirmek Için veri deposu CLI için ek bir parametre olarak eklendi
    + Tek örnekli profil oluşturma bir öneri üretmek için düzeltildi ve temel SDK 'da kullanıma sunuldu.
    + Sorun aks.py _deploy düzeltildi
    + Sessiz depolama hatalarından kaçınmak için karşıya yüklenen modellerin bütünlüğünü doğrular.
  + **azureml-interpret**
    + azureml-yorumlama için azureml stili özel durumları eklendi
    + keras modelleri için sabit DeepScoringExplainer serileştirmesi
  + **azureml-pipeline-core**
    + Ardışık düzen toplu işlem Puanlama Not defteri artık ParallelRunStep kullanıyor
  + **azureml-pipeline-steps**
    + , `AutoMLStep` Paketin içine taşınır `azureml-pipeline-steps` . İçinde kullanım `AutoMLStep` dışı `azureml-train-automl-runtime` .
  + **azureml-contrib-pipeline-steps**
    + ParallelRunStep öğesine side_inputs isteğe bağlı parametre eklendi. Bu parametre, kapsayıcıya klasörü bağlamak için kullanılabilir. Şu anda desteklenen türler DataReference ve PipelineData.
  + **azureml-tensorboard**
    + , TensorFlow 2,0 desteği için azureml-tensorboard güncelleştirildi
  + **azureml-train-automl-client**
    + Özel uygulanabilirlik yapılandırmasını filtreleyen sorunu geçersiz kılan sabit Korturizationconfig.
  + **azureml-train-automl-runtime**
    + , `AutoMLStep` Paketin içine taşınır `azureml-pipeline-steps` . İçinde kullanım `AutoMLStep` dışı `azureml-train-automl-runtime` .
  + **azureml-train-core**
    + PyTorch Estimator 'da PyTorch sürüm 1,4 ' i destekleme
  
## <a name="2020-02-04"></a>2020-02-04

### <a name="azure-machine-learning-sdk-for-python-v110rc0-pre-release"></a>Python v 1.1.0 RC0 için SDK Azure Machine Learning (ön sürüm)

+ **Yeni değişiklikler**
  + **Anlamsal Sürüm Oluşturma 2.0.0**
    + Sürüm 1,1 ' den başlayarak Azure ML Python SDK 'Sı 2.0.0 anlam sürümü oluşturma. [Buradan daha fazla bilgi edinin](https://semver.org/). Sonraki tüm sürümler yeni numaralandırma düzenini ve anlamsal sürüm oluşturma sözleşmesini takip edecektir. 
  
+ **Hata düzeltmeleri ve geliştirmeleri**
  + **azureml-automl-runtime**
    + Artırılan hız artar.
    + Puanlama sırasında sıklık denetimi düzeltildi, şimdi tahmin görevlerinde, tren ve test kümesi arasında katı sıklık denkliğine gerek yoktur.
  + **azureml-core**
    + Kullanıcı, WebServices için anahtarlar yeniden oluşturulurken kimlik doğrulama anahtarı için bir değer belirtebilir.
  + **azureml-interpret**
    + Azureml, yorumlamaya (0.5.0) göre güncelleştirildi-Community
  + **azureml-pipeline-core**
    + Bağımsız değişken listesini değiştirirken PythonScriptStep sonuçlarının yanlış şekilde yeniden kullanılabildiği bir hata düzeltildi
  + **azureml-pipeline-steps**
    + Veri kümesi için PythonScriptStep girişi olarak belge örneği eklendi
  + **azureml-contrib-pipeline-steps**
    + ParallelRunConfig içinde geçirilen parametrelerin, ardışık düzen parametreleri şimdi geçirerek üzerine yazılabilir. Desteklenen yeni işlem hattı parametreleri aml_mini_batch_size, aml_error_threshold, aml_logging_level, aml_run_invocation_timeout (aml_node_count ve aml_process_count_per_node zaten önceki sürümün bir parçası).
  
## <a name="2020-01-21"></a>2020-01-21

### <a name="azure-machine-learning-sdk-for-python-v1085"></a>Python v 1.0.85 için SDK Azure Machine Learning

+ **Yeni özellikler**
  + **azureml-core**
    + Belirli bir çalışma alanı ve abonelik içindeki AmlCompute kaynakları için geçerli çekirdek kullanımı ve kota sınırlamasını alın
  
  + **azureml-contrib-pipeline-steps**
    + Önceki adımdan parallelrunstep öğesine ara sonuç olarak tablolu veri kümesini geçirmesi için kullanıcıyı etkinleştirin

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **azureml-automl-runtime**
    + Dağıtılan tahmin hizmeti isteğindeki y_query sütununun gereksinimi kaldırıldı. 
    + ' Y_query ', Dominick 'in turuncu Snot defteri hizmet isteği bölümünden kaldırılmıştır.
    + Tarih saat sütunları ile veri kümeleri üzerinde çalışan dağıtılan modellerde tahmin edilmesini engellediği hata düzeltildi.
    + Hem ikili hem de birden çok Lass sınıflandırması için bir sınıflandırma ölçümü olarak Matthews bağıntı katsayısı eklendi.
  + **azureml-contrib-interpret**
    + Metin açıklaması, kısa bir süre içinde yayımlanacak yorumlama-metin deposuna taşındığı için explainers-contrib-yorumlamaya kaldırılan Text.
  + **azureml-core**
    + Veri kümesi: dosya veri kümesi kullanımları artık Python env 'de yüklenecek bir sayısal tuş a ve Pandas 'a bağlı değildir.
    + Yerel Docker kapsayıcısının durumunu denetlemek için LocalWebservice. wait_for_deployment () değiştirildi. bu durum, başarısız bir dağıtımı raporlamak için gereken süreyi büyük ölçüde azaltır.
    + Localwebservice. Reload () içinde kullanılan bir iç özelliğin başlatılması, Service nesnesi LocalWebservice () Oluşturucusu kullanılarak mevcut bir dağıtımdan oluşturulduğunda düzeltildi.
    + Açıklama için düzenlenmiş hata iletisi.
    + Get_access_token (), erişim belirtecini, zaman damgasından sonra Yenile, zaman damgası ve belirteç türü için zaman aşımı ' nı döndüren AksServiceAccessToken nesnesini döndürecek yeni bir yöntem eklendi. 
    + Yeni yöntem bu yöntemin döndürdüğü tüm bilgileri döndürdüğünden, AksWebservice içindeki mevcut get_token () yöntemi kullanımdan kaldırıldı.
    + Az ml Service Get-Access-Token komutunun çıkışı değiştirildi. ' RefreshAfter ' olarak accessToken ve refreshBy belirteci yeniden adlandırıldı. Expıryon ve tokenType özellikleri eklendi.
    + Sabit get_active_runs
  + **azureml-açıkla-model**
    + Shap 0.33.0 ve yorumlayın-Community 0,4. * olarak güncelleştirildi
  + **azureml-interpret**
    + Shap 0.33.0 ve yorumlayın-Community 0,4. * olarak güncelleştirildi
  + **azureml-train-automl-runtime**
    + Hem ikili hem de birden çok Lass sınıflandırması için bir sınıflandırma ölçümü olarak Matthews bağıntı katsayısı eklendi.
    + Koddan ön işleme bayrağını kullanımdan kaldırma ve değişen özellikler, varsayılan olarak açık hale getirildi

## <a name="2020-01-06"></a>2020-01-06

### <a name="azure-machine-learning-sdk-for-python-v1083"></a>Python v 1.0.83 için SDK Azure Machine Learning

+ **Yeni özellikler**
  + Veri kümesi: `on_error` `out_of_range_datetime` `to_pandas_dataframe` veriler ile doldurmak yerine hata değerleri olduğunda, Için iki seçenek ekleyin ve başarısız olur `None` .
  + Çalışma alanı: `hbi_workspace` hassas verilerle daha fazla şifrelemeyi sağlayan ve çalışma alanlarında gelişmiş tanılamayı devre dışı bırakan çalışma alanları için bayrak eklendi. Ayrıca, çalışma alanınızı `cmk_keyvault` `resource_cmk_uri` sağlarken aboneliğinizde bir Cosmos DB örneği oluşturan bir çalışma alanı oluştururken ve parametrelerini belirterek ilişkili Cosmos DB örneği için kendi anahtarlarınızı getirme desteği ekledik. [Buradan daha fazla bilgi edinin.](https://docs.microsoft.com/azure/machine-learning/concept-enterprise-security#azure-cosmos-db)

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **azureml-automl-runtime**
    + 3.5.4 altındaki Python sürümlerinde oto ml çalıştırıldığında bir TypeError oluşturulmasına neden olan bir gerileme düzeltildi.
  + **azureml-core**
    + `datastore.upload_files`İle başlayan göreli yolun kullanılamadı olması durumunda düzeltilen hata `./` .
    + Tüm Image class codepaths için kullanımdan kaldırma iletileri eklendi
    + Azure Çin 21Vianet bölgesi için Model Yönetimi URL yapımı düzeltildi.
    + Azure Işlevleri için source_dir kullanan modellerin paketlenemediğinde sorun düzeltildi.    
    + Bir görüntüyü AzureML çalışma alanı kapsayıcısı kayıt defterine göndermek için [Environment. build_local ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) seçeneğine bir seçenek eklendi
    + SDK, Azure SYNAPSE üzerinde yeni belirteç kitaplığını yeniden uyumlu bir şekilde kullanacak şekilde güncelleştirildi.
  + **azureml-interpret**
    + İndirilmek üzere hiçbir açıklamalar kullanılamadığı zaman düzeltilmeyen hata düzeltildi. Artık bir özel durum ve eşleşen davranışı başka bir yerde yükseltir.
  + **azureml-pipeline-steps**
    + `DatasetConsumptionConfig` `Estimator` `inputs` Bir içinde kullanılacaksa, parametresi parametresi öğesine `Estimator` geçirilmeyecektir `EstimatorStep` .
  + **azureml-SDK**
    + Tam oto ml paketi yüklenmeden, uzak oto ml 'nin gönderilmesine olanak tanımak için, azureml-SDK paketine, oto ml istemcisi eklenmiştir.
  + **azureml-train-automl-client**
    + Oto ml çalıştırmaları için konsol çıktısındaki hizalama düzeltildi
    + Uzak amlcompute 'e hatalı Pandas sürümünün yüklenebildiği bir hata düzeltildi.

## <a name="2019-12-23"></a>2019-12-23

### <a name="azure-machine-learning-sdk-for-python-v1081"></a>Python v 1.0.81 için SDK Azure Machine Learning

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **azureml-contrib-interpret**
    + -Community-yorumlamaya yönelik Shap bağımlılığını yorumlamak için ertele
  + **azureml-core**
    + İşlem hedefi artık karşılık gelen dağıtım yapılandırma nesnelerine bir parametre olarak belirtilebilir. Bu, SDK nesnesine değil, dağıtılacak işlem hedefinin adıdır.
    + Model ve hizmet nesnelerine CreatedBy bilgileri eklendi. Aracılığıyla erişilebilir <var> . created_by
    + Sabit Containerımage. Run (), Docker kapsayıcısının HTTP bağlantı noktasını doğru ayarlamamıştı.
    + `azureml-dataprep`CLI için isteğe bağlı yap `az ml dataset register` komutu
    + `TabularDataset.to_pandas_dataframe`Alternatif bir okuyucuya doğru bir şekilde geri dönebileceği ve bir uyarının yazdırılacağı bir hata düzeltildi.
  + **azureml-açıkla-model**
    + -Community-yorumlamaya yönelik Shap bağımlılığını yorumlamak için ertele
  + **azureml-pipeline-core**
    + `NotebookRunnerStep`Ardışık düzende adım olarak yerel bir not defteri çalıştırmak için yeni işlem hattı adımı eklendi.
    + Publishedhatlarının, zamanlamalar ve PipelineEndpoints için kullanım dışı get_all işlevleri kaldırıldı
  + **azureml-train-automl-client**
    + Data_script, bir oto girişi olarak bir giriş olarak kullanımdan kaldırıldı.


## <a name="2019-12-09"></a>2019-12-09

### <a name="azure-machine-learning-sdk-for-python-v1079"></a>Python v 1.0.79 için SDK Azure Machine Learning

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **azureml-automl-core**
    + Korturizationconfig günlüğe kaydedilecek şekilde kaldırıldı
      + Günlüğe kaydetme, yalnızca "Auto"/"off"/"özelleştirilmiş" günlüğüne kaydedilecek şekilde güncelleştirildi.
  + **azureml-automl-runtime**
    + Pandas desteği eklendi. Seriler ve Pandas. Sütun veri türünü saptamak için kategorik. Daha önce yalnızca desteklenen sayısal tuş takımı. ndarray
      + Kategorik dtype 'ı doğru şekilde işlemek için ilgili kod değişiklikleri eklendi.
    + Tahmin işlevi arabirimi geliştirildi: y_pred parametresi isteğe bağlı olarak yapıldı. -Docstrings geliştirilmiştir.
  + **azureml-contrib-dataset**
    + Etiketlenmiş veri kümelerinin takılabileceği bir hata düzeltildi.
  + **azureml-core**
    + İçin hata düzeltildi `Environment.from_existing_conda_environment(name, conda_environment_name)` . Kullanıcı, Yerel ortamın tam çoğaltması olan bir ortam örneği oluşturabilir
    + Zaman serisiyle ilgili veri kümesi yöntemleri `include_boundary=True` Varsayılan olarak olarak değiştirildi.
  + **azureml-train-automl-client**
    + Çıktıyı göster değeri false olarak ayarlandığında doğrulama sonuçlarının yazdırılmayacağı sorun düzeltildi.


## <a name="2019-11-25"></a>2019-11-25

### <a name="azure-machine-learning-sdk-for-python-v1076"></a>Python v 1.0.76 için SDK Azure Machine Learning

+ **Yeni değişiklikler**
  + Azureml-eğitme-oto ml yükseltme sorunları
    + Azureml-tren-oto ml>= 1.0.76 'ten azureml-tren-oto ml<1.0.76 'e yükseltmek kısmi yüklemelere neden olabilir ve bazı oto ml içeri aktarımlarının başarısız olmasına neden olabilir. Bu sorunu çözmek için konumunda bulunan kurulum betiğini çalıştırabilirsiniz https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/automl_setup.cmd . Ya da doğrudan PIP kullanıyorsanız şunları yapabilirsiniz:
      + "PIP install--Upgrade azureml-tren-oto"
      + "PIP install--Ignore-yüklü azureml-tren-oto ml-Client"
    + ya da yükseltmeden önce eski sürümü kaldırabilirsiniz
      + "PIP kaldırma azureml-eğitme-oto ml"
      + "PIP yüklemesi azureml-eğitme-oto ml"

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **azureml-automl-runtime**
    + Artık İkili Sınıflandırma görevleri için Ortalama skaler ölçümleri hesaplarken, oto 'nin her ikisi de doğru ve yanlış sınıfları hesaba götürecektir.
    + AzureML-oto ml-Core içindeki makine öğrenimi ve eğitim kodu, yeni bir AzureML-oto ml-Runtime paketine taşındı.
  + **azureml-contrib-dataset**
    + `to_pandas_dataframe`Etiketli bir veri kümesinde, indirme seçeneğiyle çağrılırken, artık mevcut dosyaların üzerine yazılıp yazılmayacağını belirtebilirsiniz.
    + Çağrılırken `keep_columns` veya `drop_columns` bir zaman serisi, etiket veya resim sütunu atıldığı zaman, ilgili yetenekler veri kümesi için de bırakılır.
    + Nesne algılama görevi için pytorch yükleyicisindeki bir sorun düzeltildi.
  + **azureml-contrib-interpret**
    + Contrib-yorumlanan açıklama panosu pencere öğesi, interpret_community ' deki yeni bir başvuruya başvuracak şekilde değiştirildi
    + Yorumlama-Community 'nin güncelleştirilmiş sürümü 0.2.0 olarak
  + **azureml-core**
    + Performansını geliştirir `workspace.datasets` .
    + Kullanıcı adı ve parola kimlik doğrulaması kullanarak Azure SQL veritabanı veri deposunu kaydetme özelliği eklendi
    + Göreli yollardan RunConfigurations yükleme için çözüm.
    + Çağrılırken `keep_columns` veya `drop_columns` bir zaman serisi sütununun bırakılmasının sonucu olarak, ilgili yetenekler veri kümesi için de bırakılır.
  + **azureml-interpret**
    + yorumlama-Community 'nin güncelleştirilmiş sürümü 0.2.0 olarak
  + **azureml-pipeline-steps**
    + `runconfig_pipeline_params`Azure Machine Learning ardışık düzen adımları için desteklenen değerler belgelenmiştir.
  + **azureml-pipeline-core**
    + İşlem hattı komutları için JSON biçiminde çıktıyı indirmek için CLı seçeneği eklendi.
  + **azureml-eğitme-oto ml**
    + AzureML-tren-oto ml 'yi iki pakete ayırın, bir istemci paketi AzureML-tren-oto ml-Client ve bir ML eğitim paketi AzureML-tren-oto ml-Runtime
  + **azureml-train-automl-client**
    + Herhangi bir makine öğrenimi bağımlılığı yerel olarak yüklemeye gerek kalmadan, oto ml denemeleri göndermek için bir ince istemci eklenmiştir.
    + Uzak çalışmalarındaki otomatik olarak algılanan lags, pencere boyutları ve maxhayvan Horizons sabit günlüğü.
  + **azureml-train-automl-runtime**
    + Makine öğrenimi ve çalışma zamanı bileşenlerini istemciden yalıtmak için yeni bir oto ml paketi eklendi.
  + **azureml-contrib-tren-rl**
    + SDK 'da pekiştirmeye dayalı Learning desteği eklendi.
    + RL SDK 'da AmlWindowsCompute desteği eklendi.


## <a name="2019-11-11"></a>2019-11-11

### <a name="azure-machine-learning-sdk-for-python-v1074"></a>Python v 1.0.74 için SDK Azure Machine Learning

  + **Önizleme özellikleri**
    + **azureml-contrib-veri kümesi**
      + Azureml-contrib-DataSet içeri aktardıktan sonra `Dataset.Labeled.from_json_lines` `._Labeled` etiketli bir veri kümesi oluşturmak için yerine öğesini çağırabilirsiniz.
      + `to_pandas_dataframe`Etiketli bir veri kümesinde, indirme seçeneğiyle çağrılırken, artık mevcut dosyaların üzerine yazılıp yazılmayacağını belirtebilirsiniz.
      + Çağrılırken `keep_columns` veya `drop_columns` bir zaman serisi, etiket veya resim sütunu atıldığı zaman, ilgili yetenekler veri kümesi için de bırakılır.
      + Çağrılırken PyTorch yükleyicisindeki sorunlar düzeltildi `dataset.to_torchvision()` .

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **Azure-CLI-ml**
    + Model profili oluşturma, önizleme CLı 'ya eklendi.
    + Azure depolama 'da ortadan kaldırma değişikliği, AzureML 'nın başarısız olmasına neden olur.
    + AKS türleri için MLC 'ye Load Balancer türü eklendi
  + **azureml-automl-core**
    + Eksik değerlere ve birden çok grak 'a sahip olan zaman serisinde en fazla on üst sıra tespit eden sorun düzeltildi.
    + Çapraz doğrulama bölmelerini oluşturma sırasında hatalarla ilgili sorun düzeltildi.
    + Bu bölümü, sürüm notları:-tahmin verileri kümelerindeki kısa gratların yönetilmesi ile Iyileştirilmiş bir ileti ile birlikte markın biçimindeki bir iletiyle değiştirin.
    + Günlüğe kaydetme sırasında bazı Kullanıcı bilgilerinin maskelendiği sorun düzeltildi. -Tahmin çalıştırmaları sırasında hata günlüğü geliştirildi.
    + Psutil, otomatik olarak oluşturulmuş IML dağıtım dosyasına bir Conda bağımlılığı olarak ekleniyor.
  + **azureml-contrib-mir**
    + Azure depolama 'da ortadan kaldırma değişikliği, AzureML 'nın başarısız olmasına neden olur.
  + **azureml-core**
    + Azure Işlevleri üzerinde dağıtılan modellerin 500s 'yi üretmesine neden olan bir hatayı düzeltir.
    + Amlignore dosyasının anlık görüntülere uygulanmadığı bir sorun düzeltildi.
    + Belirli bir amlcompute üzerinde çalışan ve sıraya alınmış çalıştırmalar için bir Oluşturucu döndüren yeni bir API amlcompute. get_active_runs eklendi.
    + AKS türleri için MLC 'ye Load Balancer türü eklendi.
    + Artifacts_client içindeki run.py ve download_artifacts_from_prefix download_files append_prefix bool parametresi eklendi. Bu bayrak, kaynak FilePath öğesini seçmeli olarak düzleştirmek için kullanılır, bu nedenle yalnızca dosya veya klasör adı output_directory eklenir
    + Veri kümesi kullanımıyla ilgili kaldırma sorununu düzeltir `run_config.yml` .
    + Çağrılırken `keep_columns` veya `drop_columns` bir zaman serisi sütununun bırakılmasının sonucu olarak, ilgili yetenekler veri kümesi için de bırakılır.
  + **azureml-interpret**
    + Yorumlama-topluluk sürümü 0.1.0.3 olarak güncelleştirildi
  + **azureml-eğitme-oto ml**
    + Automl_step doğrulama sorunlarını yazdıramama sorunu düzeltildi.
    + Modelin ortamında bağımlılıkları yerel olarak eksik olsa bile, sabit register_model başarılı olur.
    + Bazı uzak çalışmaların Docker etkin olmadığı bir sorun düzeltildi.
    + Yerel çalıştırmanın erken başarısız olmasına neden olan özel durumun günlüğe kaydedilmesini ekleyin.
  + **azureml-train-core**
    + Otomatikleştirilmiş hyperparameter ayarlama en iyi alt çalıştırmalarının hesaplamasında resume_from çalıştırmaları göz önünde bulundurun.
  + **azureml-pipeline-core**
    + Ardışık düzen bağımsız değişkeninde düzeltilen parametre işleme.
    + Ardışık düzen açıklaması ve adım türü YAML parametresi eklendi.
    + İşlem hattı adımı için yeni YAML biçimi ve eski biçim için kullanımdan kaldırma uyarısı eklendi.



## <a name="2019-11-04"></a>2019-11-04

### <a name="web-experience"></a>Web deneyimi

Konumundaki işbirliğine dayalı çalışma alanı giriş sayfası, [https://ml.azure.com](https://ml.azure.com) Azure Machine Learning Studio (Önizleme) olarak geliştirilmiş ve yeniden markalı.

Studio 'dan veri kümeleri, işlem hatları, modeller, uç noktaları ve daha fazlası gibi Azure Machine Learning varlıkları eğitebilir, test edebilir, dağıtabilir ve yönetebilirsiniz.

Studio 'dan aşağıdaki Web tabanlı yazma araçlarına erişin:

| Web tabanlı araç | Açıklama | Sürüm |
|-|-|-|
| Not defteri VM (Önizleme) | Tam olarak yönetilen bulut tabanlı iş istasyonu | Temel & kurumsal |
| [Otomatik makine öğrenimi](tutorial-first-experiment-automated-ml.md) (Önizleme) | Machine Learning modeli geliştirmeyi otomatikleştirmek için kod deneyimi yok | Enterprise |
| [Tasarımcı](concept-designer.md) (Önizleme) | Daha önce tasarımcı olarak bilinen sürükle ve bırak makine öğrenimi modelleme aracı | Enterprise |


### <a name="azure-machine-learning-designer-enhancements"></a>Azure Machine Learning tasarımcı geliştirmeleri

+ Daha önce görsel arabirim olarak bilinirdi 
+    Özellik Mühendisliği, çapraz doğrulama ve veri dönüştürme dahil olmak üzere recommenders, sınıflandırıcılar ve eğitim yardımcı programları içeren 11 yeni [Modül](algorithm-module-reference/module-reference.md) .

### <a name="r-sdk"></a>R SDK 
 
Veri bilimcileri ve AI geliştiricileri, Azure Machine Learning ile makine öğrenimi iş akışlarını derlemek ve çalıştırmak için [R için Azure Machine Learning SDK 'sını](tutorial-1st-r-experiment.md) kullanır.

R için Azure Machine Learning SDK, `reticulate` Python SDK 'sına bağlamak için paketini kullanır. Doğrudan Python 'a bağlayarak, R için SDK, seçtiğiniz herhangi bir R ortamından Python SDK 'sında uygulanan temel nesnelere ve yöntemlere erişmenizi sağlar.

SDK 'nın ana özellikleri şunlardır:

+    Makine öğrenmesi denemelerinizi izlemek, günlüğe kaydetmek ve düzenlemek için bulut kaynaklarını yönetme.
+    GPU hızlandırılmış model eğitimi de dahil olmak üzere bulut kaynaklarını kullanarak modelleri eğitme.
+    Modellerinizi Azure Container Instances (ACI) ve Azure Kubernetes hizmeti (AKS) üzerinde WebServices olarak dağıtın.

Tüm belgeler için [paket Web sitesine](https://azure.github.io/azureml-sdk-for-r) bakın.

### <a name="azure-machine-learning-integration-with-event-grid"></a>Event Grid ile tümleştirme Azure Machine Learning 

Azure Machine Learning artık Event Grid için bir kaynak sağlayıcıdır, Azure portal veya Azure CLı aracılığıyla makine öğrenimi olaylarını yapılandırabilirsiniz. Kullanıcılar çalıştırma tamamlama, model kaydı, model dağıtımı ve veri DRI için olaylar oluşturabilir. Bu olaylar, tüketim için Event Grid tarafından desteklenen olay işleyicilerine yönlendirilebilir. Daha fazla bilgi için bkz. Machine Learning olay [şeması](https://docs.microsoft.com/azure/event-grid/event-schema-machine-learning) ve [öğretici](how-to-use-event-grid.md) makaleleri.

## <a name="2019-10-31"></a>2019-10-31

### <a name="azure-machine-learning-sdk-for-python-v1072"></a>Python v 1.0.72 için SDK Azure Machine Learning

+ **Yeni özellikler**
  + Veri kümesi izleyicileri ve zaman içinde diğer istatistiksel değişiklikler için zaman serisi veri kümelerini izlemeye izin veren, [**azureml-datadrift**](https://docs.microsoft.com/python/api/azureml-datadrift) paketi aracılığıyla veri kümesi izleyicileri eklendi. Uyarılar ve olaylar, DRIP algılanırsa veya veriler üzerinde başka koşullar karşılanıyorsa tetiklenebilir. Ayrıntılar için [belgelerimizi](https://aka.ms/datadrift) inceleyin.
  + Azure Machine Learning ' de iki yeni sürüm (aynı zamanda SKU da denir) duyuruldu. Bu sürümle birlikte, artık temel veya kurumsal Azure Machine Learning çalışma alanı oluşturabilirsiniz. Tüm mevcut çalışma alanları temel sürüme varsayılan olarak uygulanır ve Azure portal veya Studio 'ya giderek çalışma alanını dilediğiniz zaman yükseltebilirsiniz. Azure portal temel veya kurumsal bir çalışma alanı oluşturabilirsiniz. Daha fazla bilgi edinmek için lütfen [belgelerimizi](https://docs.microsoft.com/azure/machine-learning/how-to-manage-workspace) okuyun. SDK 'dan çalışma alanınızın sürümü çalışma alanı nesnenizin "SKU" özelliği kullanılarak belirlenebilir.
  + Ayrıca, Azure Machine Learning Işlem için geliştirmeler yaptık. artık, kümeleriniz için (Toplam düğümler, çalışan düğümler, toplam çekirdek kota gibi) ölçümleri, hata ayıklama için tanılama günlüklerini görüntülemenin yanı sıra Azure Izleyici 'de görüntüleyebilirsiniz. Ayrıca, kümenizde çalışmakta olan veya sıraya alınmış çalıştırmaları ve kümenizdeki çeşitli düğümlerin IP 'Leri gibi ayrıntıları da görüntüleyebilirsiniz. Bunları portalda veya SDK ya da CLı 'de karşılık gelen işlevleri kullanarak görüntüleyebilirsiniz.

  + **Önizleme özellikleri**
    + Azure Machine Learning Işlem sırasında yerel SSD 'nizin disk şifrelemesi için Önizleme desteği yayınlıyoruz. Aboneliğinizin bu özelliği kullanmasına izin vermek için lütfen bir teknik destek bileti yükseltin.
    + Azure Machine Learning Batch çıkarımı genel önizlemesi. Azure Machine Learning Batch çıkarımı zamana duyarlı olmayan büyük çıkarım işlerini hedefler. Batch çıkarımı, zaman uyumsuz uygulamalar için paralellik verimlilik ile uygun maliyetli çıkarım işlem Ölçeklendirmesi sağlar. Büyük veri koleksiyonları üzerinde yüksek aktarım hızı, yangın ve unutma çıkarımı için iyileştirilmiştir.
    + [**azureml-contrib-dataset**](https://docs.microsoft.com/python/api/azureml-contrib-dataset)
        + Etiketli veri kümesi için etkinleştirilen işlevler
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

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **Azure-CLI-ml**
    + CLı artık model paketlemeyi destekliyor.
    + Veri kümesi CLı eklendi. Daha fazla bilgi için:`az ml dataset --help`
    + Bir ınenceconfig örneği olmadan desteklenen modellerin (ONNX, scikit-öğren ve TensorFlow) dağıtımı ve paketlenmesi için destek eklendi.
    + SDK ve CLı 'de hizmet dağıtımı (acı ve AKS) için üzerine yazma bayrağı eklendi. Sağlanmışsa, zaten adlı hizmet varsa mevcut hizmetin üzerine yazar. Hizmet yoksa, yeni hizmet oluşturacaktır.
    + Modeller, Onnx ve TensorFlow olmak üzere iki yeni çerçeve ile kaydedilebilir. -Model kaydı, örnek giriş verilerini, örnek çıkış verilerini ve model için kaynak yapılandırmasını kabul eder.
  + **azureml-automl-core**
    + Bir yineleme eğitimi, yalnızca çalışma zamanı kısıtlamaları ayarlanmakta olduğunda bir alt işlemde çalışır.
    + Belirtilen bir max_horizon verilen makinede bellek sorununa neden olup olmayacağını denetlemek için tahmin görevleri için bir guardkıl eklendi. Bu işlem, bir guarddemiryolu iletisi görüntülenir.
    + İki yıl ve bir ay gibi karmaşık sıklıklara yönelik destek eklendi. -Sıklık tespit edilemez hata iletisi eklendi.
    + Model dağıtım hatasını çözümlemek için otomatik olarak oluşturulan Conda env öğesine azureml-varsayılanlar ekleyin
    + Azure Machine Learning ardışık düzeninde ara verilerin tablo veri kümesine dönüştürülmesine ve içinde kullanılmasına izin verin `AutoMLStep` .
    + Akış için sütun amacı güncelleştirmesi uygulandı.
    + Akış için ımputer ve HashOneHotEncoder için transformatör parametresi güncelleştirmesi uygulandı.
    + Geçerli veri boyutu ve gereken en düşük veri boyutu doğrulama hata iletilerine eklendi.
    + Her bir doğrulama katalanında en az iki örnek sağlamak için çapraz doğrulama için gereken en düşük veri boyutu güncelleştirildi.
  + **azureml-CLI-ortak**
    + CLı artık model paketlemeyi destekliyor.
    + Modeller, Onnx ve TensorFlow olmak üzere iki yeni çerçeve ile kaydedilebilir.
    + Model kaydı, model için örnek giriş verilerini, örnek çıkış verilerini ve kaynak yapılandırmasını kabul eder.
  + **azureml-contrib-gbdt**
    + Not defteri için yayın kanalı düzeltildi
    + Desteklemediğimiz AmlCompute işlem hedefi için bir uyarı eklendi
    + Azureml-contrib-gbdt paketine LightGMB Estimator eklendi
  + [**azureml-core**](https://docs.microsoft.com/python/api/azureml-core)
    + CLı artık model paketlemeyi destekliyor.
    + Kullanım dışı veri kümesi API 'Leri için kullanımdan kaldırma uyarısı ekleyin. Bkz. veri kümesi API 'SI değişiklik bildirimi https://aka.ms/tabular-dataset .
    + [`Dataset.get_by_id`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset%28class%29#get-by-id-workspace--id-)Veri kümesi kayıtlıysa kayıt adını ve sürümünü döndürecek şekilde değiştirin.
    + Bağımsız değişken olarak ScriptRunConfig ile birlikte bulunan bir hatayı düzelttikten sonra, deneme çalıştırması göndermek için tekrar tekrar kullanılamaz.
    + Çalıştırma sırasında alınan veri kümeleri izlenir ve çalıştırma ayrıntıları sayfasında ya da [`run.get_details()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29#get-details--) çalışma tamamlandıktan sonra öğesini çağırarak görünür.
    + Azure Machine Learning ardışık düzeninde ara verilerin tablo veri kümesine dönüştürülmesine ve içinde kullanılmasına izin verin [`AutoMLStep`](/python/api/azureml-train-automl-runtime/azureml.train.automl.runtime.automlstep) .
    + Bir ınenceconfig örneği olmadan desteklenen modellerin (ONNX, scikit-öğren ve TensorFlow) dağıtımı ve paketlenmesi için destek eklendi.
    + SDK ve CLı 'de hizmet dağıtımı (acı ve AKS) için üzerine yazma bayrağı eklendi. Sağlanmışsa, zaten adlı hizmet varsa mevcut hizmetin üzerine yazar. Hizmet yoksa, yeni hizmet oluşturacaktır.
    +  Modeller, Onnx ve TensorFlow olmak üzere iki yeni çerçeve ile kaydedilebilir. Model kaydı, model için örnek giriş verilerini, örnek çıkış verilerini ve kaynak yapılandırmasını kabul eder.
    + MySQL için Azure veritabanı için yeni veri deposu eklendi. DataTransferStep içinde MySQL için Azure veritabanı 'nın Azure Machine Learning işlem hatları 'nda kullanılması için örnek eklenmiştir.
    + Etiketleri çalıştırmalardan kaldırmak için denemeleri eklenen işlevden etiket ekleme ve kaldırma işlevselliği eklendi
    + SDK ve CLı 'de hizmet dağıtımı (acı ve AKS) için üzerine yazma bayrağı eklendi. Sağlanmışsa, zaten adlı hizmet varsa mevcut hizmetin üzerine yazar. Hizmet yoksa, yeni hizmet oluşturacaktır.
  + [**azureml-datadrift**](https://docs.microsoft.com/python/api/azureml-datadrift)
    + Öğesinden `azureml-contrib-datadrift` öğesine taşındı`azureml-datadrift`
    + Değişikliklerini ve diğer istatistiksel ölçüler için zaman serisi veri kümelerini izleme desteği eklendi
    + Yeni yöntemler `create_from_model()` ve `create_from_dataset()` [`DataDriftDetector`](https://docs.microsoft.com/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector(class)) sınıfı. `create()`Yöntem kullanım dışı olacaktır.
    + Azure Machine Learning Studio 'daki Python ve Kullanıcı arabirimindeki görselleştirmelerin ayarlamaları.
    + Günlük veri kümesi izleyicilerine ek olarak haftalık ve aylık izleme zamanlamasını destekler.
    + Veri kümesi izleyicilerinin geçmiş verilerini çözümlemek için veri izleme ölçümlerinin geri doldurmasını destekler.
    + Çeşitli hata düzeltmeleri
  + [**azureml-pipeline-core**](https://docs.microsoft.com/python/api/azureml-pipeline-core)
    + işlem hattı dosyasından bir Azure Machine Learning işlem hattı çalıştırması göndermek için azureml-dataprep artık gerekli değildir `yaml` .
  + [**azureml-eğitme-oto ml**](/python/api/azureml-train-automl-runtime/)
    + Model dağıtım hatasını çözümlemek için otomatik olarak oluşturulan Conda env öğesine azureml-varsayılanlar ekleyin
    + Oto ml uzak eğitimi artık, çıkarım için eğitim env 'nin yeniden kullanımına izin vermek üzere azureml-varsayılanlar içerir.
  + **azureml-train-core**
    + Tahmin aracı 'da pytorch 1,3 desteği eklendi [`PyTorch`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch)

## <a name="2019-10-21"></a>2019-10-21

### <a name="visual-interface-preview"></a>Görsel Arabirim (Önizleme)

+ Azure Machine Learning görsel arabirimi (Önizleme), [Azure Machine Learning işlem hatları](concept-ml-pipelines.md)üzerinde çalışacak şekilde kaldırıldı. Görsel arabirimde yazılan işlem hatları (daha önce denemeleri olarak bilinir) artık çekirdek Azure Machine Learning deneyimiyle tamamen tümleşiktir.
  + SDK varlıklarıyla Birleşik yönetim deneyimi
  + Görsel arabirim modelleri, işlem hatları ve uç noktalar için sürüm oluşturma ve izleme
  + Yeniden tasarlanan kullanıcı Arabirimi
  + Toplu çıkarım dağıtımı eklendi
  + Çıkarım işlem hedefleri için Azure Kubernetes hizmeti (AKS) desteği eklendi
  + Yeni Python-adım işlem hattı yazma iş akışı
  + Görsel yazma araçları için yeni [giriş sayfası](https://ml.azure.com)

+ **Yeni modüller**
  + Matematik işlemini Uygula
  + SQL dönüşümünü Uygula
  + Klip değerleri
  + Verileri özetleme
  + SQL veritabanından al

## <a name="2019-10-14"></a>2019-10-14

### <a name="azure-machine-learning-sdk-for-python-v1069"></a>Python v 1.0.69 için SDK Azure Machine Learning

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **azureml-automl-core**
    + Her çalıştırma için bilgi işlem açıklamalarını değil, model açıklamalarını en iyi çalıştırmaya kısıtlama. Bu davranış yerel, uzak ve ADB için değişiklik yapıyor.
    + Kullanıcı arabirimi için isteğe bağlı model açıklamaları desteği eklendi
    + Psutil bir bağımlılığı olarak eklendi `automl` ve psutil, amlcompute içindeki bir Conda bağımlılığı olarak eklenmiştir.
    + Tahmin verileri ile ilgili sorun düzeltilme ve kayan pencere boyutları, bazı dizileri doğrusal algedeniz hatalarına neden olabilir
      + Tahmin çalıştırmalarının buluşsal belirlenen parametreleri için baskı çıkışı eklendi.
  + **azureml-contrib-datadrift**
    + Veri kümesi düzeyi değişikliklerini ilk bölümde değilse, çıkış ölçümleri oluşturulurken koruma eklendi.
  + **azureml-contrib-interpret**
    + azureml-contrib-açıkla-model paketi azureml-contrib-yorumlama olarak yeniden adlandırıldı
  + **azureml-core**
    + Veri kümelerinin kaydını silmek için API eklendi. `dataset.unregister_all_versions()`
    + azureml-contrib-açıkla-model paketi, azureml-contrib-yorumlama olarak yeniden adlandırıldı.
  + **[azureml-core](https://docs.microsoft.com/python/api/azureml-core)**
    + Veri kümelerinin kaydını silmek için API eklendi. veri kümesi. [unregister_all_versions ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.abstract_datastore.abstractdatastore#unregister--).
    + Veri değiştirme süresini denetlemek için DataSet API 'SI eklendi. `dataset.data_changed_time`.
    + `FileDataset` `TabularDataset` `PythonScriptStep` , `EstimatorStep` Ve `HyperDriveStep` Azure Machine Learning işlem hattında giriş olarak tükeme ve olarak
    + `FileDataset.mount`Çok sayıda dosya içeren klasörler için performansı geliştirilmiştir
    + Azure Machine Learning işlem hattındaki [filedataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.filedataset) ve [Tabulardataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset) ' i [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep), [estimatorstep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.estimatorstep)ve [HyperDriveStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.hyperdrivestep) öğesine giriş olarak tükeiyor.
    + FileDataset performansı. [Mount ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.filedataset#mount-mount-point-none----kwargs-) çok sayıda dosya içeren klasörler için geliştirildi
    + Çalışma ayrıntılarında bilinen hata önerilerine URL eklendi.
    + Çalıştırılmakta olan bir hata düzeltildi. bir çalıştırma çok fazla alt öğe içeriyorsa isteklerin başarısız olduğu get_metrics
    + [Çalıştırılmakta](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run#get-metrics-name-none--recursive-false--run-type-none--populate-false-) olan bir hata düzeltildi. bir çalıştırma çok fazla alt öğe içeriyorsa isteklerin başarısız olduğu get_metrics
    + Arcadia kümesinde kimlik doğrulaması için destek eklendi.
    + Deneme nesnesi oluşturma, çalışma geçmişi izlemenin Azure Machine Learning çalışma alanında denemeyi alır veya oluşturur. Deneme KIMLIĞI ve arşivlenen süre, oluşturma sırasında denemeler nesnesine doldurulur. Örnek: deneme = deneme (çalışma alanı, "yeni deneme") experiment_id = experiment.id Archive () ve yeniden etkinleştirme (), denemeyi UX içinde gösterilmeye veya bir List denemeleri çağrısında varsayılan olarak döndürülen bir deneyde çağrılabilir işlevlerdir. Arşivlenmiş bir deneyle aynı ada sahip yeni bir deneme oluşturulursa, yeni bir ad geçirerek yeniden etkinleştirme sırasında arşivlenmiş denemeyi yeniden adlandırabilirsiniz. Yalnızca belirli bir ada sahip bir etkin deneme olabilir. Örnek: experiment1 = deneme (çalışma alanı, "etkin deneme") experiment1. Archive () # arşivlenmiş ile aynı ada sahip yeni etkin denemeler oluşturun. experiment2. = Denemeler (çalışma alanı, "etkin deneme") experiment1. yeniden etkinleştirme (new_name = "önceki etkin deneme") deneme sırasında statik yöntem listesi () bir ad filtresi ve ViewType filtresi alabilir. ViewType değerleri şunlardır: "ACTIVE_ONLY", "ARCHIVED_ONLY" ve "ALL" örnek: archived_experiments = denemeler. List (Workspace, view_type = "ARCHIVED_ONLY") all_first_experiments = denemeler. List (çalışma alanı, ad = "Ilk deneme", view_type = "tümü")
    + Model dağıtımı ve hizmet güncelleştirmesi için ortamı kullanma desteği
  + **azureml-datadrift**
    + DataDriftDector sınıfının Show özniteliği artık ' with_details ' isteğe bağlı bağımsız değişkenini desteklemez. Show özniteliği yalnızca veri DRFT katsayısını ve özellik sütunlarının veri kayması katkısını sunar.
    + Datadriftalgılayıcı özniteliği ' get_output ' davranış değişiklikleri:
      + Giriş parametresi start_time, end_time zorunlu yerine isteğe bağlıdır;
      + Aynı çağırma içindeki belirli bir run_id giriş özel start_time ve/veya end_time, karşılıklı olarak dışlandığı için değer hata özel durumuna neden olur
      + Girişe özgü start_time ve/veya end_time göre yalnızca zamanlanan çalıştırmaların sonuçları döndürülür;
      + ' Daily_latest_only ' parametresi kullanım dışıdır.
    + Veri kümesi tabanlı veri Drçıkışları almayı destekler.
  + **azureml-açıkla-model**
    + AzureML daha sonra geri uyumluluk için eski paketi tutmak üzere, AzureML-açıkla-model paketini AzureML-yorumlama olarak yeniden adlandırır
    + `automl`ExplanationClient adresinden indirme sırasında varsayılan olarak gerileme yerine sınıflandırma görevine ayarlanmış ham açıklamalar ile düzeltilen hata
    + `ScoringExplainer`Kullanarak doğrudan oluşturulacak desteği ekle`MimicWrapper`
  + **azureml-pipeline-core**
    + Büyük işlem hattı oluşturma performansı geliştirildi
  + **azureml-train-core**
    + TensorFlow Estimator 'da TensorFlow 2,0 desteği eklendi
  + **azureml-eğitme-oto ml**
    + [Deneme](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment) nesnesi oluşturma, çalışma geçmişi izlemenin Azure Machine Learning çalışma alanında denemeyi alır veya oluşturur. Deneme KIMLIĞI ve arşivlenen süre, oluşturma sırasında denemeler nesnesine doldurulur. Örnek:

        ```py
        experiment = Experiment(workspace, "New Experiment")
        experiment_id = experiment.id
        ```
        [Archive ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment#archive--) ve [yeniden etkinleştirme ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment#reactivate-new-name-none-) , bir deneyinin UX içinde gösterilmesi veya bir List denemeleri çağrısında varsayılan olarak döndürülmesi için bir denemede çağrılabilecek işlevlerdir. Arşivlenmiş bir deneyle aynı ada sahip yeni bir deneme oluşturulursa, yeni bir ad geçirerek yeniden etkinleştirme sırasında arşivlenmiş denemeyi yeniden adlandırabilirsiniz. Yalnızca belirli bir ada sahip bir etkin deneme olabilir. Örnek:

        ```py
        experiment1 = Experiment(workspace, "Active Experiment")
        experiment1.archive()
        # Create new active experiment with the same name as the archived.
        experiment2 = Experiment(workspace, "Active Experiment")
        experiment1.reactivate(new_name="Previous Active Experiment")
        ```
        Deneme sırasında statik yöntem [listesi ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment#list-workspace--experiment-name-none--view-type--activeonly---tags-none-) bir ad filtresi ve ViewType filtresi alabilir. ViewType değerleri şunlardır "ACTIVE_ONLY", "ARCHIVED_ONLY" ve "ALL". Örnek:

        ```py
        archived_experiments = Experiment.list(workspace, view_type="ARCHIVED_ONLY")
        all_first_experiments = Experiment.list(workspace, name="First Experiment", view_type="ALL")
        ```
    + Model dağıtımı ve hizmet güncelleştirmesi için ortamı kullanma desteği.
  + **[azureml-datadrift](https://docs.microsoft.com/python/api/azureml-datadrift)**
    + [Datadriftalgılayıcı](https://docs.microsoft.com/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector.datadriftdetector) sınıfının Show özniteliği artık ' with_details ' isteğe bağlı bağımsız değişkenini desteklemez. Show özniteliği yalnızca veri DRFT katsayısını ve özellik sütunlarının veri kayması katkısını sunar.
    + Datadriftalgılayıcı işlevi [get_output] https://docs.microsoft.com/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector.datadriftdetector#get-output-start-time-none--end-time-none--run-id-none-) davranış değişiklikleri:
      + Giriş parametresi start_time, end_time zorunlu yerine isteğe bağlıdır;
      + Aynı çağırma içindeki belirli bir run_id giriş özel start_time ve/veya end_time, karşılıklı olarak dışlandığı için değer hata özel durumuna neden olur;
      + Girişe özgü start_time ve/veya end_time göre yalnızca zamanlanan çalıştırmaların sonuçları döndürülür;
      + ' Daily_latest_only ' parametresi kullanım dışıdır.
    + Veri kümesi tabanlı veri Drçıkışları almayı destekler.
  + **azureml-açıkla-model**
    + Daha önce geri uyumluluk için eski paketi tutmak üzere, AzureML-açıkla-model paketini AzureML-yorumlama olarak yeniden adlandırır.
    + ExplanationClient adresinden indirme sırasında varsayılan olarak gerileme yerine sınıflandırma görevine ayarlanmış ham açıklamaları içeren sabit bir hata düzeltildi.
    + [ScoringExplainer](/python/api/azureml-interpret/azureml.interpret.scoring.scoring_explainer.scoringexplainer?view=azure-ml-py) için Mmıısarmalayıcı kullanılarak doğrudan oluşturulacak destek ekleme
  + **[azureml-pipeline-core](https://docs.microsoft.com/python/api/azureml-pipeline-core)**
    + Büyük işlem hattı oluşturma performansı geliştirildi.
  + **[azureml-train-core](https://docs.microsoft.com/python/api/azureml-train-core)**
    + [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow) Estimator 'Da tensorflow 2,0 desteği eklendi.
  + **[azureml-eğitme-oto ml](/python/api/azureml-train-automl-runtime/)**
    + Düzenleme zaten bu işlemi yaptığı için, kurulum yineleme başarısız olduğunda üst öğe çalıştırması artık başarısız olmayacaktır.
    + Oto ml denemeleri için yerel Docker ve yerel-Conda desteği eklendi
    + Oto ml denemeleri için yerel Docker ve Local-Conda desteği eklendi.


## <a name="2019-10-08"></a>2019-10-08

### <a name="new-web-experience-preview-for-azure-machine-learning-workspaces"></a>Azure Machine Learning çalışma alanları için yeni Web deneyimi (Önizleme)

[Yeni çalışma alanı portalındaki](https://ml.azure.com) deneme sekmesi, veri bilimcilerinin denemeleri daha performanslı bir şekilde izleyebilmesi için güncelleştirilmiştir. Aşağıdaki özellikleri inceleyebilirsiniz:
+ Denemeleri listenizi kolayca filtreleyip sıralamak için meta verileri deneyin
+ Öngörülerinizi görselleştirmenize ve karşılaştırmanıza imkan tanıyan Basitleştirilmiş ve performanslı deneme ayrıntıları sayfaları
+ Eğitim çalıştırmalarını anlamak ve izlemek için Ayrıntılar sayfalarını çalıştırmak üzere yeni tasarım

## <a name="2019-09-30"></a>2019-09-30

### <a name="azure-machine-learning-sdk-for-python-v1065"></a>Python v 1.0.65 için SDK Azure Machine Learning

  + **Yeni özellikler**
    + Seçkin ortamlar eklendi. Bu ortamlar, ortak makine öğrenimi görevlerinin kitaplıklarıyla önceden yapılandırılmıştır ve daha hızlı yürütülmek üzere Docker görüntüleri olarak önceden derlenerek önbelleğe alınır. Bu, varsayılan olarak çalışma alanının ortam listesinde ve "AzureML" önekiyle görünürler.
    + Seçkin ortamlar eklendi. Bu ortamlar, ortak makine öğrenimi görevlerinin kitaplıklarıyla önceden yapılandırılmıştır ve daha hızlı yürütülmek üzere Docker görüntüleri olarak önceden derlenerek önbelleğe alınır. Bu, varsayılan olarak [çalışma alanının](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace%28class%29)ortam listesinde ve "AzureML" önekiyle görünürler.

  + **azureml-eğitme-oto ml**
  + **[azureml-eğitme-oto ml](/python/api/azureml-train-automl-runtime/)**
    + ADB ve HDI için ONNX dönüştürme desteği eklendi

+ **Önizleme özellikleri**
  + **azureml-eğitme-oto ml**
  + **[azureml-eğitme-oto ml](/python/api/azureml-train-automl-runtime/)**
    + Metin özelliği olarak desteklenen BERT ve BiLSTM (yalnızca Önizleme)
    + Sütun amacı ve transformatör parametreleri için desteklenen uygulanabilirlik özelleştirmesi (yalnızca Önizleme)
    + Kullanıcı eğitim sırasında model açıklamasını etkinleştirse desteklenen ham açıklamalar (yalnızca Önizleme)
    + İşlem hattı olarak tahmin için Prophet eklendi `timeseries` (yalnızca Önizleme)

  + **azureml-contrib-datadrift**
    + Paketler, azureml-contrib-datadrift ile azureml-datadrift arasında yeniden konumlandırılır. `contrib`paket sonraki bir sürümde kaldırılacak

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **azureml-automl-core**
    + Cmlconfig ve oto Mlbasesettings için FeaturizationConfig sunuldu
    + [Cmlconfig](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig) ve oto Mlbasesettings Için FeaturizationConfig sunuldu
      + Verilen sütun ve Özellik türüyle uygun şekilde sütun amacını geçersiz kıl
      + Transformatör parametrelerini geçersiz kıl
    + Explain_model () ve retrieve_model_explanations () için kullanımdan kaldırma iletisi eklendi
    + İşlem hattı olarak Prophet eklendi (yalnızca Önizleme)
    + Explain_model () ve retrieve_model_explanations () için kullanımdan kaldırma iletisi eklendi.
    + Prophet bir işlem hattı olarak eklendi (yalnızca Önizleme).
    + Hedef lags, hareketli pencere boyutu ve en büyük ufuk için otomatik algılama desteği eklendi. Target_lags, target_rolling_window_size veya max_horizon ' Auto ' olarak ayarlandıysa, bu işlem, eğitim verilerine göre karşılık gelen parametrenin değerini tahmin etmek için uygulanır.
    + Veri kümesi bir Gren sütunu içerdiğinde, bu gren sayısal bir tür ve tren ve test kümesi arasında bir boşluk olduğunda sabit tahmin
    + Tahmin görevlerinde uzak çalıştırmada yinelenen dizin hakkında hata iletisi düzeltildi
    + Veri kümesi bir Gren sütunu içerdiğinde, bu gren sayısal bir tür ve tren ve test kümesi arasında bir boşluk olduğunda sabit tahmin.
    + Tahmin görevlerinde uzak çalıştırmada yinelenen dizin hakkında hata iletisi düzeltildi.
    + Bir veri kümesinin kapalı olup olmadığını kontrol etmek için bir guardrayııl eklendi. Bu durumda, konsola bir guarddemiryolu iletisi yazılır.
  + **azureml-core**
    + Model nesnesi aracılığıyla depolamada SAS URL 'sini depolama sırasında modele alma özelliği eklendi. Ex: model. get_sas_url ()
    + `run.get_details()['datasets']`Gönderilen çalıştırma ile ilişkili veri kümelerini almak için tanıtın
    + `Dataset.Tabular.from_json_lines_files`JSON satırları dosyalarından bir TabularDataset oluşturmak IÇIN API ekleyin. TabularDataset 'teki JSON satırları dosyalarında bu tablo verileri hakkında bilgi edinmek için lütfen https://aka.ms/azureml-data belgeler için ziyaret edin.
    + Supported_vmsizes () işlevine ek VM boyut alanları (işletim sistemi diski, GPU sayısı) eklendi
    + Çalışmayı, özel ve genel IP 'yi, bağlantı noktasını vb. göstermek için list_nodes () işlevine ek alanlar eklendi.
    + Küme sağlama sırasında yeni bir alan belirtme--remotelogin_port_public_access, küme oluşturma sırasında SSH bağlantı noktasını açık veya kapalı bırakmak istediğinize bağlı olarak, etkin veya devre dışı olarak ayarlanabilir. Bunu belirtmezseniz, küme bir sanal ağın içine dağıtıp dağıtdığınıza bağlı olarak bağlantı noktasını açar veya kapatır.
  + **azureml-açıkla-model**
  + **[azureml-core](https://docs.microsoft.com/python/api/azureml-core/azureml.core)**
    + Model nesnesi aracılığıyla depolamada SAS URL 'sini depolama sırasında modele alma özelliği eklendi. Ex: model. [get_sas_url ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model#get-sas-urls--)
    + Çalıştırmayı tanıtın. gönderilen çalışma ile ilişkili veri kümelerini almak için [' DataSet '] [get_details](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29#get-details--)
    + API ekleyin `Dataset.Tabular` .[ ](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory#from-json-lines-files-path--validate-true--include-path-false--set-column-types-none--partition-format-none-)JSON satırları dosyalarından bir TabularDataset oluşturmak için from_json_lines_files (). TabularDataset 'teki JSON satırları dosyalarında bu tablo verileri hakkında bilgi edinmek için lütfen https://aka.ms/azureml-data belgeler için ziyaret edin.
    + [Supported_vmsizes ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#supported-vmsizes-workspace--location-none-) IŞLEVINE ek VM boyut alanları (Işletim sistemi diski, GPU sayısı) eklendi
    + Çalışmayı, özel ve genel IP 'yi, bağlantı noktasını vb. göstermek için [list_nodes ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#list-nodes--) işlevine ek alanlar eklendi.
    + [Küme](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#provisioning-configuration-vm-size-----vm-priority--dedicated---min-nodes-0--max-nodes-none--idle-seconds-before-scaledown-none--admin-username-none--admin-user-password-none--admin-user-ssh-key-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--tags-none--description-none--remote-login-port-public-access--notspecified--) `--remotelogin_port_public_access` oluşturma sırasında SSH bağlantı noktasını açık veya kapalı bırakmak istediğinize bağlı olarak, etkin veya devre dışı olarak ayarlanabilir bir yeni alan belirtme özelliği. Bunu belirtmezseniz, küme bir sanal ağın içine dağıtıp dağıtdığınıza bağlı olarak bağlantı noktasını açar veya kapatır.
  + **azureml-açıkla-model**
    + Sınıflandırma senaryosunda açıklama çıkışları için geliştirilmiş belgeler.
    + Değerlendirme örnekleri için açıklamada tahmini y değerlerini karşıya yükleme özelliği eklendi. Daha kullanışlı görselleştirmelerin kilidini açar.
    + Temeldeki MimicExplainer almayı etkinleştirmek için Mimkıwrapper 'a Açıklama özelliği eklendi.
  + **azureml-pipeline-core**
    + Modül, ModuleVersion ve ModuleStep 'i anlatmak için Not defteri eklendi
  + **azureml-pipeline-steps**
    + AML işlem hattı aracılığıyla R betiği çalıştırmayı desteklemek için RScriptStep eklendi.
    + AzureBatchStep içinde ayrıştırma, "SubscriptionID parametresi için atama belirtilmemiş" hata iletisine neden olan sabit meta veri parametreleri.
  + **azureml-eğitme-oto ml**
    + Desteklenen training_data, validation_data, label_column_name, veri girişi biçimi olarak weight_column_name
    + Explain_model () ve retrieve_model_explanations () için kullanımdan kaldırma iletisi eklendi
  + **[azureml-pipeline-core](https://docs.microsoft.com/python/api/azureml-pipeline-core)**
    + [Modül](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.module(class)), [Moduleversion ve [modulestep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.modulestep)' i anlatmak için bir [Not defteri](https://aka.ms/pl-modulestep) eklendi.
  + **[azureml-pipeline-steps](https://docs.microsoft.com/python/api/azureml-pipeline-steps)**
    + AML işlem hattı aracılığıyla R betiği çalıştırmayı desteklemek için [Rscriptstep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.rscriptstep) eklendi.
    + [AzureBatchStep parametresi için atama "hata iletisine neden olan" hata iletisine neden olan "içinde ayrıştırma için sabit meta veri parametreleri belirtilmemiş".
  + **[azureml-eğitme-oto ml](/python/api/azureml-train-automl-runtime/)**
    + Training_data, validation_data, label_column_name weight_column_name veri girişi biçimi olarak desteklenir.
    + [Explain_model ()](/python/api/azureml-train-automl-runtime/azureml.train.automl.runtime.automlexplainer#explain-model-fitted-model--x-train--x-test--best-run-none--features-none--y-train-none----kwargs-) ve [retrieve_model_explanations ()](/python/api/azureml-train-automl-runtime/azureml.train.automl.runtime.automlexplainer#retrieve-model-explanation-child-run-)için kullanımdan kaldırma iletisi eklendi.


## <a name="2019-09-16"></a>2019-09-16

### <a name="azure-machine-learning-sdk-for-python-v1062"></a>Python v 1.0.62 için SDK Azure Machine Learning

+ **Yeni özellikler**
  + `timeseries`TabularDataset üzerinde nitelik tanıtılmıştır. Bu nitelik, bir zaman aralığı veya en son veriler arasında tüm verileri almak gibi bir TabularDataset veri kümesi verilerinde kolay zaman damgası filtrelemesini mümkün bir şekilde sunar. `timeseries`TabularDataset üzerinde nitelik hakkında bilgi edinmek için lütfen https://aka.ms/azureml-data Belgeler veya https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datasets-tutorial/timeseries-datasets/tabular-timeseries-dataset-filtering.ipynb örnek bir not defteri için ziyaret edin.
  + TabularDataset ve FileDataset ile eğitime etkin. Lütfen https://aka.ms/dataset-tutorial örnek bir not defteri için ziyaret edin.

  + **azureml-train-core**
      + `Nccl` `Gloo` Pytorch tahmin aracı 'da eklendi ve desteklenir

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **azureml-automl-core**
    + ' Lag_length ' ve LaggingTransformer oto ml ayarı kullanım dışı bırakıldı.
    + Veri akışı biçiminde belirtilmişse giriş verilerinin doğru doğrulanması düzeltildi
    + JSON grafiğini oluşturmak ve yapıtlara yüklemek için fit_pipeline. Kopyala değiştirildi.
    + Kullanılarak grafik işlendi `userrun` `Cytoscape` .
  + **azureml-core**
    + ADB kodundaki özel durum işlemeyi yeniden ziyaret ediyor ve yeni hata işleme uyarınca değişiklik yapma
    + Not defteri VM 'Leri için otomatik MSI kimlik doğrulaması eklendi.
    + Başarısız denemeler nedeniyle bozuk veya boş modellerin karşıya yüklenebildiği hatayı düzeltir.
    + `DataReference`Mod değiştiğinde adın değiştiği hata düzeltildi `DataReference` (örn., `as_upload` veya çağrılırken `as_download` `as_mount` ).
    + `mount_point` `target_path` Ve için yapın ve için isteğe bağlıdır `FileDataset.mount` `FileDataset.download` .
    + Zaman damgası sütununun bulunamadığı bir özel durum, zaman içinde ilgili API 'nin atanmış ince zaman damgası sütunu olmadan çağrılması veya atanan zaman damgası sütunları bırakılmaması durumunda oluşturulur.
    + Zaman seri sütunları, türü tarih olan sütunla atanmalıdır, aksi takdirde özel durum beklenmektedir
    + ' With_timestamp_columns ' API 'sini atayan zaman damgası sütunları, daha önce atanan zaman damgası sütunlarını temizleyerek hiçbir değer ince/kaba zaman damgası sütun adı alabilir.
    + Büyük/küçük bir zaman damgası sütunu, bırakma listesindeki zaman damgası sütunu dışladıktan sonra ya da zaman damgası sütunlarını serbest bırakmak için None değeri ile with_time_stamp çağrısı yapıldıktan sonra yapılabilirse Kullanıcı için belirtilene,
    + Sütunları tut listesinde, sütun tut listesinde zaman damgası sütunu dahil edildiğinde veya çağrı zaman damgası sütunlarını serbest bırakmak için None değeri olan bir değer olan with_time_stamp bir Kullanıcı göstergesi olan sütun tut listesine dahil edilmediği zaman, özel durum atılır.
    + Kayıtlı bir modelin boyutu için günlük kaydı eklendi.
  + **azureml-açıkla-model**
    + "Paketleme" Python paketi yüklü olmadığında konsola sabitlenmiş uyarı düzeltildi: "desteklenen lightgbm sürümünden daha eski bir sürümü kullanıyor, lütfen 2.2.1 sürümüne yükseltin"
    + Birçok özelliğe sahip genel açıklamalar için parçalı yükleme modeli açıklaması düzeltildi
    + Eksik başlatma örnekleri çıkış açıklamasında açıklama düzeltildi
    + İki farklı model türü kullanarak açıklama istemcisiyle karşıya yüklenirken özelliklerde ayarlanan özelliklerde sabit hata düzeltildi
    + Puanlama açıklama. açıkla () için bir get_raw parametresi eklendi, bu nedenle bir Puanlama açıklama hem mühendislik hem de ham değerleri döndürebilir.
  + **azureml-eğitme-oto ml**
    + Oto ml 'yi açıklayan ve oto ml modelleri için özel ml açıklamalarını desteklemeye yönelik `automl` daha yeni yollardan, oto ml 'yi açıklayan ve SDK Ile tümleşik ham açıklama desteğini açıklayan, oto ml 'den ortak API 'ler tanıtılmıştır.
    + Uzak eğitim ortamlarından azureml varsayılan değerleri kaldırılıyor.
    + Azure Databricks kod yolu üzerinde bir AzureFileCacheStore One tabanlı dosya olan varsayılan önbellek depolama konumu, oto ml için bir tane olarak değiştirildi.
    + Veri akışı biçiminde belirtilmişse giriş verilerinin doğru doğrulanması düzeltildi
  + **azureml-train-core**
    + Kullanımdan kaldırılması source_directory_data_store geri döndürüldü.
    + Azureml yüklü paket sürümlerini geçersiz kılma özelliği eklendi.
    + Estimators 'daki parametreye dockerfile desteği eklendi `environment_definition` .
    + Estimators 'da Basitleştirilmiş dağıtılmış eğitim parametreleri.

         ```py
        from azureml.train.dnn import TensorFlow, Mpi, ParameterServer
        ```

## <a name="2019-09-09"></a>2019-09-09

### <a name="new-web-experience-preview-for-azure-machine-learning-workspaces"></a>Azure Machine Learning çalışma alanları için yeni Web deneyimi (Önizleme)
Yeni Web deneyimi, veri bilimcilerinin ve veri mühendislerinin kendi uçtan uca makine öğrenimi yaşam döngüsünü tamamlamalarını ve modelleri tek bir konumda eğitmek ve dağıtmak için verileri görselleştirmesini ve dağıtmalarını sağlar.

![Azure Machine Learning çalışma alanı kullanıcı arabirimi (Önizleme)](./media/azure-machine-learning-release-notes/new-ui-for-workspaces.jpg)

**Temel Özellikler:**

Bu yeni Azure Machine Learning arabirimini kullanarak şu anda şunları yapabilirsiniz:
+ Not defterlerinizi yönetin veya Jupyıter 'a bağlayın
+ [Otomatik ML denemeleri çalıştırma](tutorial-first-experiment-automated-ml.md)
+ [Yerel dosyalardan, veri depolarından, & Web dosyalarından veri kümeleri oluşturma](how-to-create-register-datasets.md)
+ Model oluşturma için veri kümelerini hazırlama & hazırla
+ Modelleriniz için veri kayması izleme
+ Bir panodan son kaynakları görüntüleme

Bu sürümün sırasında, aşağıdaki tarayıcılar desteklenir: Chrome, Firefox, Safari ve Microsoft Edge Preview.

**Bilinen sorunlar:**

1. "Bir sorun oluştu!" hata görürseniz tarayıcınızı yenileyin. Dağıtım devam ederken öbek dosyaları yüklenirken hata oluştu.

1. Not defterlerindeki ve dosyalardaki dosya silinemez veya yeniden adlandırılamaz. Genel Önizleme sırasında, güncelleştirme dosyası işlemlerini gerçekleştirmek için, dizüstü bilgisayar VM 'de Jupyter Kullanıcı arabirimini veya terminali kullanabilirsiniz. Bağlı bir ağ dosya sistemi tüm değişiklikleri olduğundan, Not defteri VM 'de yaptığınız değişiklikler not defteri çalışma alanında hemen yansıtılır.

1. Not defteri sanal makinesine SSH eklemek için:
   1. VM kurulumu sırasında oluşturulan SSH anahtarlarını bulun. Ya da Azure Machine Learning çalışma alanındaki anahtarları bulun > Işlem sekmesini açın > listedeki Not defteri sanal makinesini bulun > özelliklerini açın: anahtarları iletişim kutusundan kopyalayın.
   1. Bu ortak ve özel SSH anahtarlarını yerel makinenize aktarın.
   1. Bunları not defteri VM 'sine SSH için kullanın.

## <a name="2019-09-03"></a>2019-09-03
### <a name="azure-machine-learning-sdk-for-python-v1060"></a>Python v 1.0.60 için SDK Azure Machine Learning

+ **Yeni özellikler**
  + Veri mağazalarınızın veya genel URL 'lerdeki tek veya birden çok dosyaya başvuran FileDataset kullanıma sunulmuştur. Dosyalar herhangi bir biçimde olabilir. Dosya veri kümesi size dosyaları indirme veya işlem için bağlama yeteneği sağlar. FileDataset hakkında bilgi edinmek için lütfen adresini ziyaret edin https://aka.ms/file-dataset .
  + PythonScript Step, adla Step, Databricks Step, DataTransferStep ve AzureBatch Step için işlem hattı YAML desteği eklendi

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **azureml-automl-core**
    + Oto, yalnızca önizleme için bir önerimme tablosu ardışık düzeni.
    + Tahmin için geliştirilmiş hata raporlaması.
    + Tahmin görevlerinde genel yerine özel özel durumlar kullanılarak günlüğe kaydetme geliştirildi.
    + Max_concurrent_iterations denetim, toplam yineleme sayısından daha az olacak şekilde kaldırıldı.
    + Oto ml modelleri artık oto Mlexceptions döndürüyor
    + Bu sürüm, otomatik makine öğrenimi yerel çalıştırmalarının yürütme performansını geliştirir.
  + **azureml-core**
    + Bir sözlüğü `TabularDataset` ve `FileDataset` kayıt adına göre anahtarlanan nesneler döndüren Dataset. get_all (Workspace) öğesini tanıtın.

    ```py
    workspace = Workspace.from_config()
    all_datasets = Dataset.get_all(workspace)
    mydata = all_datasets['my-data']
    ```

    + `parition_format`Ve için bağımsız değişken olarak tanıtın `Dataset.Tabular.from_delimited_files` `Dataset.Tabular.from_parquet.files` . Her veri yolunun bölüm bilgileri, belirtilen biçime göre sütunlarda ayıklanır. ' {column_name} ' dize sütunu oluşturuyor ve ' {column_name: yyyy/aa/gg/HH/mm/ss} ' DateTime sütunu oluşturuyor; burada ' yyyy ', ' AA ', ' gg ', ' HH ', ' mm ' ve ' ss ', DateTime türü için Year, month, Day, Hour, Minute ve Second 'ı ayıklamak için kullanılır. Partition_format, dosya yolunun sonuna kadar ilk bölüm anahtarının konumundan başlamalıdır. Örneğin, '. ' yolu veriliyor. /USA/2019/01/01/data.csv ' bölümü ülkeye ve zamana göre, partition_format = '/{Country}/{PartitionDate: yyyy/aa/gg}/data.csv ', ' ABD ' değeri ve ' PartitionDate ' tarih sütunu ' 2019-01-01 ' değerine sahip ' Country ' dize sütununu oluşturur.
        ```py
        workspace = Workspace.from_config()
        all_datasets = Dataset.get_all(workspace)
        mydata = all_datasets['my-data']
        ```

    + `partition_format`Ve için bağımsız değişken olarak tanıtın `Dataset.Tabular.from_delimited_files` `Dataset.Tabular.from_parquet.files` . Her veri yolunun bölüm bilgileri, belirtilen biçime göre sütunlarda ayıklanır. ' {column_name} ' dize sütunu oluşturuyor ve ' {column_name: yyyy/aa/gg/HH/mm/ss} ' DateTime sütunu oluşturuyor; burada ' yyyy ', ' AA ', ' gg ', ' HH ', ' mm ' ve ' ss ', DateTime türü için Year, month, Day, Hour, Minute ve Second 'ı ayıklamak için kullanılır. Partition_format, dosya yolunun sonuna kadar ilk bölüm anahtarının konumundan başlamalıdır. Örneğin, '. ' yolu veriliyor. /USA/2019/01/01/data.csv ' bölümü ülkeye ve zamana göre, partition_format = '/{Country}/{PartitionDate: yyyy/aa/gg}/data.csv ', ' ABD ' değeri ve ' PartitionDate ' tarih sütunu ' 2019-01-01 ' değerine sahip ' Country ' dize sütununu oluşturur.
    + `to_csv_files`ve `to_parquet_files` yöntemleri öğesine eklenmiştir `TabularDataset` . Bu yöntemler `TabularDataset` `FileDataset` , verileri belirtilen biçimdeki dosyalara dönüştürerek, ve arasında dönüştürmeyi etkinleştirir.
    + Model. Package () tarafından oluşturulan bir Dockerfile dosyasını kaydederken otomatik olarak temel görüntü kayıt defterinde oturum açın.
    + ' gpu_support ' artık gerekli değildir; AML artık kullanılabilir olduğunda NVIDIA Docker uzantısını otomatik olarak algılar ve kullanır. Daha sonraki bir sürümde kaldırılacak.
    + PipelineDrafts oluşturma, güncelleştirme ve kullanma desteği eklendi.
    + Bu sürüm, otomatik makine öğrenimi yerel çalıştırmalarının yürütme performansını geliştirir.
    + Kullanıcılar, çalışma geçmişinden ada göre ölçümleri sorgulayabilir.
    + Tahmin görevlerinde genel yerine özel özel durumlar kullanılarak günlüğe kaydetme geliştirildi.
  + **azureml-açıkla-model**
    + Yeni MimicWrapper 'a feature_maps parametresi eklendi ve kullanıcıların ham özellik açıklamalarını almalarına izin veriliyor.
    + Açıklama yüklemesi için veri kümesi yüklemeleri artık varsayılan olarak kapalıdır ve upload_datasets = true ile yeniden etkinleştirilebilir
    + Açıklama listesine ve indirme işlevlerine "is_law" filtre eklendi.
    + `get_raw_explanation(feature_maps)`Hem genel hem de yerel açıklama nesnelerine yöntem ekler.
    + Desteklenen sürüm altında, yazdırılmış uyarı ile birlikte lightgbm için sürüm denetimi eklendi
    + Açıklamaları toplu işleme alırken İyileştirilmiş bellek kullanımı
    + Oto ml modelleri artık oto Mlexceptions döndürüyor
  + **azureml-pipeline-core**
    + PipelineDrafts oluşturma, güncelleştirme ve kullanma desteği eklendi-kesilebilir işlem hattı tanımlarını sürdürmek ve bunları çalıştırmak için etkileşimli olarak kullanmak için kullanılabilir
  + **azureml-eğitme-oto ml**
    + :::no-loc text="cuda":::Uzak Python çalışma zamanı ortamında BERT/XLNet ' i etkinleştirmek için gereken GPU özellikli pytorch v 1.1.0, araç seti 9,0, pytorch-dönüştürücüler 'ın belirli sürümlerini yüklemek için özelliği oluşturuldu.
  + **azureml-train-core**
    + Bazı hiper parametre alanı tanımı hatalarının sunucu tarafı yerine doğrudan SDK 'da erken hatası.

### <a name="azure-machine-learning-data-prep-sdk-v1114"></a>Azure Machine Learning Data Prep SDK v 1.1.14
+ **Hata düzeltmeleri ve geliştirmeleri**
  + Ham yol ve kimlik bilgileri kullanılarak ADLS/ADLSGen2 'a yazma özelliği etkinleştirildi.
  + İçin çalışmamasından kaynaklanan bir hata düzeltildi `include_path=True` `read_parquet` .
  + `to_pandas_dataframe()`"Geçersiz özellik değeri: hostSecret" özel durumu nedeniyle oluşan hata düzeltildi.
  + Spark modundaki DBFS üzerinde dosyaların okunmamasına neden olan bir hata düzeltildi.

## <a name="2019-08-19"></a>2019-08-19

### <a name="azure-machine-learning-sdk-for-python-v1057"></a>Python v 1.0.57 için SDK Azure Machine Learning
+ **Yeni özellikler**
  + `TabularDataset`AutomatedML tarafından tüketilebilmesi için etkinleştirildi. Hakkında daha fazla bilgi edinmek için `TabularDataset` lütfen adresini ziyaret edin https://aka.ms/azureml/howto/createdatasets .

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **Oto ml-Client-Core-NativeClient**
    + Hata düzeltildi, eğitim ve/veya doğrulama etiketleri (y ve y_valid), Pandas dataframe biçiminde sağlandığı halde bir sayısal tuş y dizisi olarak değil, ortaya çıktı.
    + `RawDataContext`Yalnızca verileri ve nesneyi gerektirecek şekilde oluşturmak için arabirimi güncelleştirildi `AutoMLBaseSettings` .
    +  Oto ml kullanıcılarına tahmin sırasında yeterince uzun olmayan eğitim serisini bırakmaya izin verin. -Oto ml kullanıcılarına, tahmin sırasında eğitim kümesinde mevcut olmayan test kümesinden graıns 'yi bırakması izin verin.
  + **Azure-CLI-ml**
    + Artık, Microsoft tarafından oluşturulan ve müşteri sertifikası için AKS kümesinde dağıtılan Puanlama uç noktası için TLS/SSL sertifikasını güncelleştirebilirsiniz.
  + **azureml-automl-core**
    + Eksik etiketlere sahip satırların düzgün şekilde kaldırılmadığı, oto ml 'deki bir sorun düzeltildi.
    + Oto ml 'de geliştirilmiş hata günlüğü; Tam hata iletileri artık her zaman günlük dosyasına yazılır.
    + Oto,, ve dahil olmak üzere paket sabitlenmesini güncelleştirmiştir `azureml-defaults` `azureml-explain-model` `azureml-dataprep` . Oto, paket uyuşmazlıkları (paket hariç) üzerinde artık uyarı vermez `azureml-train-automl` .
    + `timeseries`CV 'in eşit olmadığı bir sorun düzeltildi.
    + Çapraz doğrulama eğitim türü için ensebir yineleme çalıştırırken, veri kümesinin tamamında eğitilen modelleri indirirken sorun yaşadığımızda, model ağırlıkları ve oylama 'nin içine beslendiği modeller arasında bir tutarsızlık vardı.
    + Hata düzeltildi, eğitim ve/veya doğrulama etiketleri (y ve y_valid), Pandas dataframe biçiminde sağlandığı halde bir sayısal tuş y dizisi olarak değil, ortaya çıktı.
    + Giriş tablolarının Boole sütunlarında hiç karşılaşılmadığı zaman tahmin görevlerinde sorun düzeltildi.
    + Oto ml kullanıcılarına tahmin sırasında yeterince uzun olmayan eğitim serisini bırakmaya izin verin. -Oto ml kullanıcılarına, tahmin sırasında eğitim kümesinde mevcut olmayan test kümesinden graıns 'yi bırakması izin verin.
  + **azureml-core**
    + Blob_cache_timeout parametre sıralaması ile ilgili sorun düzeltildi.
    + Dış sığdırma ve özel durum türlerini sistem hatalarına ekledi.
    + Uzak çalıştırmalar için Key Vault gizli dizileri için destek eklendi. Çalışma alanınız ile ilişkili anahtar kasasından gizli dizi eklemek, almak ve listelemek için bir azureml. Core. keykasa. Keykasası sınıfı ekleyin. Desteklenen işlemler şunlardır:
      + azureml. Core. Workspace. Workspace. get_default_keyvault ()
      + azureml. Core. keykasa. Keykasası. set_secret (ad, değer)
      + azureml. Core. keykasa. Keykasa. set_secrets (secrets_dict)
      + azureml. Core. keykasa. Keykasası. get_secret (ad)
      + azureml. Core. keykasa. Keykasa. get_secrets (secrets_list)
      + azureml. Core. keykasa. Keykasası. list_secrets ()
    + Varsayılan anahtar kasasını elde etmek ve Uzaktan çalıştırma sırasında gizli dizileri almak için ek Yöntemler:
      + azureml. Core. Workspace. Workspace. get_default_keyvault ()
      + azureml. Core. Run. Run. get_secret (ad)
      + azureml. Core. Run. Run. get_secrets (secrets_list)
    + Gönderme-Hyperdrive CLı komutuna ek geçersiz kılma parametreleri eklendi.
    + API çağrılarının güvenilirliğini artırmak, yaygın istekler Kitaplığı özel durumlarına yeniden denemeler genişletmektedir.
    + Gönderilen bir çalışmadan çalıştırma göndermek için destek ekleyin.
    + Dosya izleyicisinden, ilk belirtecinin süresi dolduktan sonra karşıya yükleme işlemini durdurmasına neden olan, süresi dolan son SAS belirteç sorunu düzeltildi.
    + Veri kümesi Python SDK 'sında HTTP CSV/TSV dosyalarını içeri aktarma destekleniyor.
    + Workspace. Setup () yöntemi kullanımdan kaldırıldı. Kullanıcılara gösterilen uyarı iletisi, bunun yerine Create () veya Get ()/from_config () kullanılmasını önerir.
    + Özel özel Python paketlerinin çalışma alanına yüklenmesini sağlayan ortam. add_private_pip_wheel () eklendi `whl` ve ortamı derlemek/denemek için güvenli bir şekilde kullanma.
    + Artık, Microsoft tarafından oluşturulan ve müşteri sertifikası için AKS kümesinde dağıtılan Puanlama uç noktası için TLS/SSL sertifikasını güncelleştirebilirsiniz.
  + **azureml-açıkla-model**
    + Karşıya yükleme ile ilgili açıklamaları bir model KIMLIĞI eklemek için parametresi eklendi.
    + `is_raw`Bellekteki açıklamaları etiketleme ve karşıya yükleme eklendi.
    + Azureml-açıkla-model paketi için pytorch desteği ve Testleri eklendi.
  + **azureml-opendatasets**
    + Otomatik test ortamını algılayıp günlüğe kaydetmeyi destekler.
    + Ülke ve ZIP tarafından nüfusu almak için sınıflar eklendi.
  + **azureml-pipeline-core**
    + Giriş ve çıkış bağlantı noktası tanımlarına etiket özelliği eklendi.
  + **azureml-telemetry**
    + Hatalı telemetri yapılandırması düzeltildi.
  + **azureml-eğitme-oto ml**
    + Kurulum hatasında hata düzeltildi, hata, kurulum çalıştırmasının "hatalar" alanında oturum açmamıştı ve bu nedenle üst çalıştırmada "hatalar" olarak depolanmadı.
    + Eksik etiketlere sahip satırların düzgün şekilde kaldırılmadığı, oto ml 'deki bir sorun düzeltildi.
    + Oto ml kullanıcılarına tahmin sırasında yeterince uzun olmayan eğitim serisini bırakmaya izin verin.
    + Oto ml kullanıcılarına, tahmin sırasında eğitim kümesinde mevcut olmayan test kümesinden graıns 'yi bırakması için izin verin.
    + Şimdi `automl` , yeni yapılandırma parametrelerinin değişiklikleri veya eklemeleri üzerinde herhangi bir sorun yaşamamak için config üzerinden arka uca geçiş yapar.
    + Oto Data Guardkıl artık genel önizlemeye sunuldu. Kullanıcı eğitim sonrasında (sınıflandırma/gerileme görevleri için) bir veri Guardcıl raporu görür ve ayrıca SDK API aracılığıyla buna erişebilir.
  + **azureml-train-core**
    + PyTorch Estimator 'da Torch 1,2 desteği eklenmiştir.
  + **azureml-widgets**
    + Sınıflandırma eğitimi için geliştirilmiş karışıklık matrisi grafikleri.

### <a name="azure-machine-learning-data-prep-sdk-v1112"></a>Azure Machine Learning Data Prep SDK v 1.1.12
+ **Yeni özellikler**
  + Dizelerin listesi artık yöntemlere giriş olarak geçirilebilir `read_*` .

+ **Hata düzeltmeleri ve geliştirmeleri**
  + , `read_parquet` Spark 'ta çalışırken performansı önemli ölçüde iyileştirildi.
  + `column_type_builder`Belirsiz tarih biçimleri içeren tek bir sütun olması durumunda başarısız olan bir sorun düzeltildi.

### <a name="azure-portal"></a>Azure portalı
+ **Önizleme özelliği**
  + Günlük ve çıkış dosyası akışı artık çalışma ayrıntıları sayfaları için kullanılabilir. Dosyalar, önizleme geçişi açık olduğunda güncelleştirmeleri gerçek zamanlı olarak akışa alır.
  + Çalışma alanı düzeyinde kota ayarlama özelliği önizleme aşamasında serbest bırakılır. AmlCompute kotaları abonelik düzeyinde ayrılır, ancak artık bu kotayı çalışma alanları arasında dağıtmanıza ve bunları dengeli paylaşım ve idare için ayırmaya izin veririz. Çalışma alanınızın sol gezinti çubuğunda bulunan **kullanımlar + kotalar** dikey penceresine ve **kotaları Yapılandır** sekmesini seçmeniz yeterlidir. Bu bir çoklu çalışma alanı işlemi olduğundan, çalışma alanı düzeyinde kotalar ayarlayabilmek için bir abonelik yöneticisi olmanız gerektiğini unutmayın.

## <a name="2019-08-05"></a>2019-08-05

### <a name="azure-machine-learning-sdk-for-python-v1055"></a>Python v 1.0.55 için SDK Azure Machine Learning

+ **Yeni özellikler**
  + Belirteç tabanlı kimlik doğrulaması artık AKS üzerinde dağıtılan Puanlama uç noktasına yapılan çağrılar için desteklenir. Geçerli anahtar tabanlı kimlik doğrulamasını desteklemeye devam edeceğiz ve kullanıcılar aynı anda bu kimlik doğrulama mekanizmalarından birini kullanabilir.
  + Sanal ağın (VNet) arkasındaki bir blob depolamayı bir veri deposu olarak kaydetme yeteneği.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **azureml-automl-core**
    + CV bölme için doğrulama boyutunun küçük olduğu ve gerileme ve tahmin için gerçek grafik ile sonuçları elde eden bir hatayı düzeltir.
    + Uzak çalışmalarla ilgili tahmin görevlerinin günlüğe kaydedilmesi, çalışma başarısız olursa, artık kullanıcı kapsamlı bir hata iletisiyle sunulmaktadır.
    + `Timeseries`Eğer ön işleme bayrağının doğru olması durumunda oluşan sorunlar düzeltildi.
    + Bazı tahmin verileri doğrulama hata iletileri daha fazla işlem yapılabilir.
    + , Özellikle işlem kaynakları arasında yer aldığı veri kümelerinin düşürümine ve/veya geç yüklenmesine göre, oto 'nin azaltılmış bellek tüketimi
  + **azureml-contrib-açıkla-model**
    + Kullanıcının model türü için varsayılan otomatik çıkarım mantığını geçersiz kılmasına izin vermek üzere explainers 'e model_task bayrağı eklendi
    + Pencere öğesi değişiklikleri: `contrib` yalnızca genel özellik önem derecesine sahip (ör. permütasyon), Ile otomatik olarak yüklenir, daha fazla `nbextension` yükleme/etkinleştirme desteği açıklaması yoktur
    + Pano değişiklikleri:-Box çizimleri ve keman çizimleri, `beeswarm` Özet sayfa üzerine çizmenin yanı sıra `beeswarm` , ' top-k ' kaydırıcısının daha hızlı bir şekilde rerendering
  + **azureml-core**
    + Modelleri ve bunların bağımlılıklarını kapsülleyen Docker görüntüleri ve Dockerfiles oluşturmak için model. Package () yöntemi eklendi.
    + Ortam nesneleri içeren ınenceconfigs 'leri kabul etmek için yerel WebServices güncelleştirildi.
    + Sabit model. Register (), '. ' olduğunda geçersiz modeller üretmiyor (geçerli dizin için) model_path parametresi olarak geçirilir.
    + Run. submit_child, işlev, gönderilen alt öğenin üst öğesi olarak çalıştırmayı belirtirken deneme. Gönder 'i yansıtır.
    + Modelden yapılandırma seçeneklerini destekleme. Run içinde kaydolun. register_model.
    + Mevcut kümede JAR işlerini çalıştırma özelliği.
    + Artık instance_pool_id ve cluster_log_dbfs_path parametrelerini destekliyor.
    + Bir model bir Web WebService 'a dağıtıldığında bir ortam nesnesi kullanma desteği eklendi. Ortam nesnesi artık ınısenceconfig nesnesinin bir parçası olarak sağlanıyor olabilir.
    + Yeni bölgeler için appınsıfht Mapping ekleme-tek merkezde ABD-westus-Kuzeydoğu ABD
    + Tüm veri deposu sınıflarında tüm öznitelikler için belge eklendi.
    + Blob_cache_timeout parametresi eklendi `Datastore.register_azure_blob_container` .
    + Azureml. Core. Environment. Environment öğesine save_to_directory ve load_from_directory yöntemleri eklendi.
    + CLı 'ya "az ml Environment Download" ve "az ml Environment Register" komutları eklendi.
    + Environment. add_private_pip_wheel yöntemi eklendi.
  + **azureml-açıkla-model**
    + DataSet hizmeti (Önizleme) kullanılarak açıklamaları olan veri kümesi izleme eklendi.
    + 10.000 'den 100 'e genel açıklamalar akışı yapılırken varsayılan toplu iş boyutu azalır.
    + Kullanıcının model türü için varsayılan otomatik çıkarım mantığını geçersiz kılmasına izin vermek üzere explainers 'e model_task bayrak eklendi.
  + **azureml-mlflow**
    + Mlflow. azureml öğesinde hata düzeltildi. iç içe dizinlerin yoksayıldığı build_image.
  + **azureml-pipeline-steps**
    + Mevcut Azure Databricks kümesinde JAR işlerini çalıştırma özelliği eklendi.
    + DatabricksStep Step için destek instance_pool_id ve cluster_log_dbfs_path parametreleri eklendi.
    + DatabricksStep adımında işlem hattı parametreleri için destek eklendi.
  + **azureml-eğitme-oto ml**
    + `docstrings`İlişkili dosyalar için eklendi.
    + Belgeler, ve için daha uygun dile güncelleştirildi `max_cores_per_iteration``max_concurrent_iterations`
    + Uzak çalışmalarla ilgili tahmin görevlerinin günlüğe kaydedilmesi, çalışma başarısız olursa, artık kullanıcı kapsamlı bir hata iletisiyle sunulmaktadır.
    + Ardışık düzen Not defterinden get_data kaldırıldı `automlstep` .
    + `dataprep`' De destek başlatıldı `automlstep` .

### <a name="azure-machine-learning-data-prep-sdk-v1110"></a>Azure Machine Learning Data Prep SDK v 1.1.10

+ **Yeni özellikler**
  + Artık belirli bir sütunda belirli Denetçiler (örn. histogram, dağılım çizimi vb.) yürütmeyi isteyebilirsiniz.
  + İçin bir paralel hale getirmek bağımsız değişkeni eklendi `append_columns` . Doğru ise, veriler belleğe yüklenir ancak yürütme paralel olarak çalıştırılır; Yanlış ise, yürütme akış, ancak tek iş parçacıklı olur.

## <a name="2019-07-23"></a>2019-07-23

### <a name="azure-machine-learning-sdk-for-python-v1053"></a>Python v 1.0.53 için SDK Azure Machine Learning

+ **Yeni özellikler**
  + Otomatik Machine Learning artık uzak işlem hedefinde ONNX modellerini eğitimini destekliyor
  + Azure Machine Learning, artık önceki bir çalıştırma, kontrol noktası veya model dosyalarından eğitim verme olanağı sunar.
    + Daha [önceki bir çalıştırınızdan eğitime geçmek için tahmini](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/tensorflow/training/train-tensorflow-resume-training/train-tensorflow-resume-training.ipynb) 'ı nasıl kullanacağınızı öğrenin

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **Oto ml-Client-Core-NativeClient**
    + Dönüşümden sonra sütun türlerini gevtürme (hata bağlantısı) ile ilgili hatayı düzeltir;
    + Y_query, Begin (#459519) sırasında hiçbirini içeren bir nesne türü olmasını sağlar.
  + **Azure-CLI-ml**
    + CLı komutları "model dağıtımı" ve "hizmet güncelleştirmesi" artık parametreleri, yapılandırma dosyalarını veya ikisinin birleşimini kabul eder. Parametrelerin, dosyalardaki özniteliklerin önceliği vardır.
    + Model açıklaması artık kayıt sonrasında güncelleştirilebilen
  + **azureml-automl-core**
    + 1.2.0 sürümüne NimbusML bağımlılığını güncelleştir (geçerli en son).
    + NimbusML tahmini için destek eklemek, oto ml tahmini içinde kullanılacak işlem hatları &.
    + Puan sabitinden sonra, sonuçta elde edilen bir şekilde büyümekte olan bir hatayı düzeltme.
    + Tahmin görevleri için CV bölmelerini genelinde bazı özellikler kullanımını etkinleştirin. Bu, zaman aşımı ve sıralı pencereler gibi pahalı bir n_cross_validations faktörü için, kurulum 'un çalışma süresini kabaca hızlandırır.
    + Zaman Pandas desteklenen zaman aralığı dışında bir sorun adresleme. Şimdi PD 'den küçükse bir DataException tetikliyoruz. Timestamp. Min veya PD 'den büyük. Timestamp. Max
    + Tahmin şimdi, eğiteseler tren ve test kümelerinde farklı sıklıklara izin veriyor. Örneğin, "Ocak ayında başlayan üç aylık dönem" ve "Ekim ayında başlayan" üç aylık olarak hizalanabilir.
    + "Parameters" özelliği TimeSeriesTransformer öğesine eklendi.
    + Eski özel durum sınıflarını kaldırın.
    + Tahmin görevlerinde, `target_lags` parametresi artık tek bir tamsayı değeri ya da tamsayılar listesini kabul eder. Tamsayı sağlanmışsa, yalnızca bir gecikme oluşturulur. Bir liste sağlanmışsa, lags 'nin benzersiz değerleri alınacaktır. target_lags = [1, 2, 2, 4] bir, 2 ve 4 dönem için lags oluşturacak.
    + Dönüşümden sonra sütun türlerini kaybetme hakkındaki hatayı düzeltir (hata bağlandı);
    + ' De `model.forecast(X, y_query)` , y_query Begin (#459519) sırasında hiçbirini içeren bir nesne türü olmasını sağlar.
    + Çıkışa beklenen değerleri ekleyin `automl`
  + **azureml-contrib-datadrift**
    +  Örnek Not defteri geliştirmeleri; azureml, verileri zenginleştirmeden azureml-contrib-OPENDATASET ve performans iyileştirmeleri yerine azureml-OPENDATASET 'e geçiş
  + **azureml-contrib-açıkla-model**
    + Azureml-contrib-açıkla-model paketindeki ham özellik için LIME açıklama için sabit dönüşümler bağımsız değişkeni
    + AzureML-contrib-açıkla-model paketi için Image açıklama içindeki görüntü açıklamaları segmentations eklendi
    + LimeExplainer için SciPy seyrek desteği ekleme
    + `batch_size` `include_local=False` DecisionTreeExplainableModel yürütme süresini geliştirmek üzere toplu işlerle ilgili genel açıklamalar için açıklama taklit 'e eklendi
  + **azureml-contrib-featureengineering**
    + Set_featurizer_timeseries_params () çağrısı için çözüm: dict değer türü değişikliği ve null denetimi-koratizer için Not defteri ekleme `timeseries`
    + 1.2.0 sürümüne NimbusML bağımlılığını güncelleştir (geçerli en son).
  + **azureml-core**
    + AzureML türünde DBFS veri depoları ekleme özelliği eklendi
    + İle başlatılmışsa boş bir klasörün oluşturulduğu veri deposu yüklemesine hata düzeltildi `target_path``/`
    + `deepcopy`Serviceprincıpalauthentication içindeki sorun düzeltildi.
    + CLı 'ya "az ml Environment Show" ve "az ml Environment List" komutları eklendi.
    + Ortamlar artık önceden oluşturulmuş bir base_image alternatif olarak bir base_dockerfile belirtmeyi desteklemektedir.
    + Kullanılmayan RunConfiguration ayarı auto_prepare_environment kullanım dışı olarak işaretlendi.
    + Model açıklaması artık kayıt sonrasında güncelleştirilebilen
    + Bugdüzeltmesini: model ve resim silme işlemi, yukarı akış bağımlılığı nedeniyle silme başarısız olursa, bunlara bağlı yukarı akış nesnelerini alma hakkında daha fazla bilgi sağlamaktadır.
    + Bazı ortamlar için bir çalışma alanı oluşturulurken gerçekleşen dağıtımlar için boş süre yazdırılan hata düzeltildi.
    + İyileştirilmiş çalışma alanı hata özel durumları oluşturma. Kullanıcılar "çalışma alanı oluşturulamıyor. Bulunamıyor... " ileti olarak ve bunun yerine gerçek oluşturma hatası ' nı görürsünüz.
    + AKS WebServices içinde belirteç kimlik doğrulaması desteği ekleyin.
    + `get_token()`Nesnelere yöntem ekleyin `Webservice` .
    + Machine Learning veri kümelerini yönetmek için CLı desteği eklendi.
    + `Datastore.register_azure_blob_container`Artık isteğe bağlı olarak `blob_cache_timeout` , bu veri deposu için önbellek süre sonunu etkinleştirmek üzere blobsigortası 'nin Mount parametrelerini yapılandıran bir değer (saniye cinsinden) alır. Varsayılan değer zaman aşımı değildir, yani bir blob okunarak iş bitene kadar yerel önbellekte kalır. Çoğu iş bu ayarı tercih eder, ancak bazı işlerin, düğümlerine sığacak kadar büyük bir veri kümesinden daha fazla veri okuması gerekir. Bu işler için, bu parametrenin ayarlanması başarılı olur. Bu parametreyi ayarlarken dikkatli olmanız gerekir: değeri çok düşük olarak ayarlamak, bir dönem içinde kullanılan verilerin yeniden kullanılmadan önce süresinin dolmasına neden olabilir. Bu, tüm okumaların, eğitim sürelerini olumsuz yönde etkileyen yerel önbellek yerine blob depolamadan (ağ) yapılacağı anlamına gelir.
    + Model açıklaması artık kayıt sonrasında düzgün şekilde güncelleştirilebilen olabilir
    + Model ve resim silme artık, bunlara bağlı yukarı akış nesneleri hakkında daha fazla bilgi sağlar ve bu da silmenin başarısız olmasına neden olur
    + Azureml. mlflow kullanarak uzak çalıştırmaların kaynak kullanımını geliştirme.
  + **azureml-açıkla-model**
    + Azureml-contrib-açıkla-model paketindeki ham özellik için LIME açıklama için sabit dönüşümler bağımsız değişkeni
    + LimeExplainer için SciPy seyrek desteği ekleme
    + Doğrusal modelleri açıklayan şekil doğrusal açıklama sarmalayıcı ve tablo açıklama 'e başka bir düzey eklendi
    + Model kitaplığı 'nda açıklama benzeklik için, include_local = false olduğunda sabit hata, seyrek veri girişi için
    + çıkışa beklenen değerleri ekleyin `automl`
    + ham Özellik önem derecesi almak için dönüşümler bağımsız değişkeni sağlandığında düzeltilen permütasyon özelliği önem derecesi
    + `batch_size` `include_local=False` DecisionTreeExplainableModel yürütme süresini geliştirmek üzere toplu işlerle ilgili genel açıklamalar için açıklama taklit 'e eklendi
    + Model explainability kitaplığı için, Pandas dataframe girişinin tahmin için gerekli olduğu sabit kara Box explainers
    + `explanation.expected_values`Bazen içinde float içeren bir liste yerine float döndüren bir hata düzeltildi.
  + **azureml-mlflow**
    + Mlflow 'un performansını geliştirme. set_experiment (experiment_name)
    + Mlflow tracking_uri için ınteractiveloginauthentication kullanımda olan hatayı düzeltir
    + Azureml. mlflow kullanarak uzak çalıştırmaların kaynak kullanımını geliştirme.
    + Azureml-mlflow paketinin belgelerini geliştirme
    + Mlflow. log_artifacts ("my_dir") içindeki yapıtları "my_dir/<yapıt-Paths>" yerine "<yapıt-Paths>" altına kaydedebilecekleri düzeltme eki hatası
  + **azureml-opendatasets**
    + `pyarrow` `opendatasets` Yeni eklenen bellek sorunu nedeniyle eski sürümlere (<0.14.0) sabitleme.
    + Azureml-contrib-OPENDATASET veri kümelerini azureml-OPENDATASET 'e taşıyın.
    + Açık veri kümesi sınıflarının Azure Machine Learning çalışma alanına kaydolmasına ve AML veri kümesi özelliklerinden sorunsuz bir şekilde yararlanmasını sağlar.
    + SPARK olmayan sürümde önemli performansı önemli ölçüde zenginleştirin.
  + **azureml-pipeline-steps**
    + DBFS veri deposu artık DatabricksStep içindeki girişler ve çıkışlar için desteklenir.
    + Giriş/çıkışlara göre Azure Batch adım için güncelleştirilmiş belgeler.
    + AzureBatchStep içinde *delete_batch_job_after_finish* varsayılan değeri *true*olarak değiştirildi.
  + **azureml-telemetry**
    +  Azureml-contrib-OPENDATASET veri kümelerini azureml-OPENDATASET 'e taşıyın.
    + Açık veri kümesi sınıflarının Azure Machine Learning çalışma alanına kaydolmasına ve AML veri kümesi özelliklerinden sorunsuz bir şekilde yararlanmasını sağlar.
    + SPARK olmayan sürümde önemli performansı önemli ölçüde zenginleştirin.
  + **azureml-eğitme-oto ml**
    + Get_output hakkındaki belgeler, gerçek dönüş türünü yansıtacak şekilde güncelleştirildi ve anahtar özelliklerini alma hakkında ek notlar sağlar.
    + 1.2.0 sürümüne NimbusML bağımlılığını güncelleştir (geçerli en son).
    + çıkışa beklenen değerleri ekleyin `automl`
  + **azureml-train-core**
    + Dizeler artık otomatik hiper parametre ayarlama için işlem hedefi olarak kabul edilir
    + Kullanılmayan RunConfiguration ayarı auto_prepare_environment kullanım dışı olarak işaretlendi.

### <a name="azure-machine-learning-data-prep-sdk-v119"></a>Azure Machine Learning Data Prep SDK v 1.1.9

+ **Yeni özellikler**
  + Bir dosyayı doğrudan http veya https URL 'sinden okumak için destek eklendi.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Uzak bir kaynaktan bir Parquet veri kümesini okumaya çalışırken (Şu anda desteklenmeyen), geliştirilmiş hata iletisi.
  + ADLS Gen 2 ' de Parquet dosya biçimine yazarken hata düzeltildi ve yoldaki ADLS Gen 2 kapsayıcı adı güncelleştiriliyor.

## <a name="2019-07-09"></a>2019-07-09

### <a name="visual-interface"></a>Görsel arabirim
+ **Önizleme özellikleri**
  + Görsel arabirimde "R betiği Yürüt" modülü eklendi.

### <a name="azure-machine-learning-sdk-for-python-v1048"></a>Python v 1.0.48 için SDK Azure Machine Learning

+ **Yeni özellikler**
  + **azureml-opendatasets**
    + **azureml-contrib-OPENDATASET** 'ler artık **azureml-OPENDATASET**olarak kullanılabilir. Eski paket çalışmaya devam edebilir, ancak daha zengin özellikler ve geliştirmeler için, daha zengin bir şekilde veri taşımaya yönelik **azureml ile openopendataset** kullanmanızı öneririz.
    + Bu yeni paket, açık veri kümelerini Azure Machine Learning çalışma alanında veri kümesi olarak kaydetmenize ve veri kümesinin sunduğu işlevleri kullanmanıza olanak tanır.
    + Ayrıca, açık veri kümelerini Pandas/SPARK dataframes olarak kullanma gibi mevcut özellikleri ve hava durumu gibi bazı veri kümelerinde konum birleştirmelerini da içerir.

+ **Önizleme özellikleri**
    + HyperDriveConfig, işlem hattı nesnesini bir işlem hattı kullanarak hiper parametre ayarlamayı destekleyecek bir parametre olarak kabul edebilir.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **azureml-eğitme-oto ml**
    + Dönüşümden sonra sütun türlerini kaybetme hakkında hata düzeltildi.
    + Y_query başlangıcında None (s) içeren bir nesne türü olması için hata düzeltildi.
    + Puanlar sabit kaldığı halde ortaya çıkan elde edilen bir şekilde büyümekte olan ensebir seçim yordamında sorun düzeltildi.
    + List_models izin verme ve oto Mlstep içindeki list_models ayarları engellenme sorunu düzeltildi.
    + Azure ML işlem hatları bağlamında, oto ml kullanıldığında ön işleme kullanımını önleyen sorun düzeltildi.
  + **azureml-opendatasets**
    + Azureml-contrib-OPENDATASET 'leri azureml-OPENDATASET 'e taşındı.
    + Azure Machine Learning çalışma alanına kaydedilecek ve AML veri kümesi özelliklerinden sorunsuz bir şekilde yararlanan açık veri kümesi sınıflarının kullanımına izin verilir.
    + SPARK olmayan sürümde gelişmiş Noaaısdhava durumu performansı önemli ölçüde geliştirildi.
  + **azureml-açıkla-model**
    + Yorumlenebilirlik nesneleri için çevrimiçi belgeler güncelleştirildi.
    + `batch_size` `include_local=False` Model explainability kitaplığı için DecisionTreeExplainableModel yürütme süresini geliştirmek üzere toplu işlerle ilgili genel açıklamalar için açıklama benzemesine eklenmiştir.
    + `explanation.expected_values`Bazen içinde float içeren bir liste yerine float döndüren sorun düzeltildi.
    + `automl`Açıklama modeli kitaplığındaki taklit açıklama için çıktıya beklenen değerler eklendi.
    + Ham Özellik önem derecesi almak için dönüşümler bağımsız değişkeni sağlandığında düzeltilen permütasyon özelliği önem derecesi.
  + **azureml-core**
    + AzureML türünde DBFS veri depoları ekleme özelliği eklendi.
    + İle başlatılmışsa boş bir klasörün oluşturulduğu veri deposu yükleme sorunu düzeltildi `target_path` `/` .
    + İki veri kümesinin etkinleştirilmiş karşılaştırması.
    + Model ve resim silme işlemi şimdi, yukarı akış bağımlılığı nedeniyle silme başarısız olursa, bunlara bağlı yukarı akış nesnelerini alma hakkında daha fazla bilgi sağlamaktadır.
    + Auto_prepare_environment kullanılmamış RunConfiguration ayarı kullanım dışı bırakıldı.
  + **azureml-mlflow**
    + Azureml. mlflow kullanan uzak çalıştırmaların geliştirilmiş kaynak kullanımı.
    + Azureml-mlflow paketinin belgeleri geliştirilmiştir.
    + Mlflow. log_artifacts ("my_dir"), yapıtları "yapıt-Paths" yerine "my_dir/Artifact-Paths" altına kaydedebileceği sorun düzeltildi.
  + **azureml-pipeline-core**
    + Tüm işlem hattı adımları için parametre hash_paths kullanım dışıdır ve gelecekte kaldırılacaktır. Source_directory varsayılan olarak, karma hale getirilir (. amlignore veya. gitignore ' de listelenen dosyalar hariç)
    + İşlem türüne özgü modülleri, işlem türüne özgü modül kullanımının kilidini ardışık düzen kullanımına açmak için hazırlamak üzere işlem türüne özgü modülleri desteklemek için modülü ve ModuleStep 'i sürekli iyileştirme.
  + **azureml-pipeline-steps**
    + AzureBatchStep: giriş/çıkışlarla ilgili olarak Iyileştirilmiş belgeler.
    + AzureBatchStep: delete_batch_job_after_finish varsayılan değeri true olarak değiştirildi.
  + **azureml-train-core**
    + Dizeler artık otomatik hiper parametre ayarlama için işlem hedefi olarak kabul edilir.
    + Auto_prepare_environment kullanılmamış RunConfiguration ayarı kullanım dışı bırakıldı.
    + Kullanım dışı bırakılan parametreler `conda_dependencies_file_path` ve `pip_requirements_file_path` `conda_dependencies_file` `pip_requirements_file` sırasıyla.
  + **azureml-opendatasets**
    + SPARK olmayan sürümde önemli performansı önemli ölçüde zenginleştirin.

### <a name="azure-machine-learning-data-prep-sdk-v118"></a>Azure Machine Learning Data Prep SDK v 1.1.8

+ **Yeni özellikler**
 + Veri akışı nesneleri artık, bir dizi kayıt üreten şekilde tekrarlandırılır. İçin belgelerine bakın `Dataflow.to_record_iterator` .
  + Veri akışı nesneleri artık, bir dizi kayıt üreten şekilde tekrarlandırılır. İçin belgelerine bakın `Dataflow.to_record_iterator` .

+ **Hata düzeltmeleri ve geliştirmeleri**
 + DataPrep SDK 'sının sağlamlık artışı artırılmıştır.
 + Dize olmayan sütun dizinleriyle Pandas Dataframe 'ler için geliştirilmiş işlem.
 + Veri kümelerinde performansı geliştirildi `to_pandas_dataframe` .
 + Çok düğümlü bir ortamda çalıştırıldığında veri kümelerinin Spark yürütmesinin başarısız olduğu bir hata düzeltildi.
  + DataPrep SDK 'sının sağlamlık artışı artırılmıştır.
  + Dize olmayan sütun dizinleriyle Pandas Dataframe 'ler için geliştirilmiş işlem.
  + Veri kümelerinde performansı geliştirildi `to_pandas_dataframe` .
  + Çok düğümlü bir ortamda çalıştırıldığında veri kümelerinin Spark yürütmesinin başarısız olduğu bir hata düzeltildi.

## <a name="2019-07-01"></a>2019-07-01

### <a name="azure-machine-learning-data-prep-sdk-v117"></a>Azure Machine Learning Data Prep SDK v 1.1.7

Azure Databricks kullanan bazı müşterilerle ilgili sorunlara neden olduğundan, performansı destekleyen bir değişikliği geri döndürüyoruz. Azure Databricks bir sorun yaşıyorsanız, aşağıdaki yöntemlerden birini kullanarak sürüm 1.1.7 sürümüne yükseltebilirsiniz:
1. Yükseltmek için bu betiği çalıştırın:`%sh /home/ubuntu/databricks/python/bin/pip install azureml-dataprep==1.1.7`
2. En son Data Prep SDK sürümünü yükleyecek olan kümeyi yeniden oluşturun.

## <a name="2019-06-25"></a>2019-06-25

### <a name="azure-machine-learning-sdk-for-python-v1045"></a>Python v 1.0.45 için SDK Azure Machine Learning

+ **Yeni özellikler**
  + Açıklama, azureml-açıkla-model paketindeki taklit etmek için karar ağacı yedek modeli ekleme
  + Inyaze resimleri üzerine yüklenecek CUDA sürümünü belirtme özelliği. CUDA 9,0, 9,1 ve 10,0 desteği.
  + Azure ML eğitimi temel görüntüleriyle ilgili bilgiler artık [Azure ML kapsayıcıları GitHub deposunda](https://github.com/Azure/AzureML-Containers) ve [dockerhub](https://hub.docker.com/_/microsoft-azureml) 'da sunulmaktadır
  + Ardışık düzen zamanlaması için CLı desteği eklendi. Daha fazla bilgi için "az ml işlem hattı-h" çalıştırın
  + AKS WebService dağıtım yapılandırmasına ve CLı 'ye özel Kubernetes ad alanı parametresi eklendi.
  + Tüm işlem hattı adımları için kullanım dışı hash_paths parametresi
  + Model. Register artık birden çok bireysel dosyanın parametresini kullanan tek bir model olarak kaydedilmesini desteklemektedir `child_paths` .

+ **Önizleme özellikleri**
    + Puanlama explainers artık daha güvenilir serileştirme ve seri durumdan çıkarma için Conda ve PIP bilgilerini isteğe bağlı olarak kaydedebilir.
    + Otomatik Özellik Seçicisi için hata düzeltilme.
    + Yeni uygulama tarafından kullanıma sunulan yeni API 'ye ve düzeltme eki uygulanmış hatalara mlflow. azureml. build_image güncelleştirildi.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + `paramiko`Azureml-Core ' dan bağımlılığı kaldırıldı. Eski işlem hedefi iliştirme yöntemlerine yönelik kullanımdan kaldırma uyarıları eklendi.
  + Çalıştırmanın performansını geliştirir. create_children
  + İkili sınıflandırıcıyla benzeme açıklama, şekil değerlerini ölçeklendirmek için öğretmen olasılığı kullanıldığında olasılıkların sırasını düzeltir.
  + Otomatik makine öğrenimi için geliştirilmiş hata işleme ve ileti.
  + Otomatik makine öğrenimi için yineleme zaman aşımı sorunu düzeltildi.
  + Otomatik makine öğrenimi için zaman serisi dönüştürme performansı geliştirildi.

## <a name="2019-06-24"></a>2019-06-24

### <a name="azure-machine-learning-data-prep-sdk-v116"></a>Azure Machine Learning Data Prep SDK v 1.1.6

+ **Yeni özellikler**
  + Top values ( `SummaryFunction.TOPVALUES` ) ve Bottom values () için özet işlevleri eklendi `SummaryFunction.BOTTOMVALUES` .

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Performansı önemli ölçüde iyileştirildi `read_pandas_dataframe` .
  + `get_profile()`İkili dosyaları işaret eden bir veri akışının başarısız olmasına neden olan bir hata düzeltildi.
  + `set_diagnostics_collection()`Telemetri koleksiyonunun programlı bir şekilde etkinleştirilmesi/devre dışı bırakılmasına olanak tanımak için sunulur.
  + Davranışı değiştirildi `get_profile()` . NaN değerleri artık, Pandas davranışıyla hizalanan min, ortalama, STD ve Sum için yok sayılır.


## <a name="2019-06-10"></a>2019-06-10

### <a name="azure-machine-learning-sdk-for-python-v1043"></a>Python v 1.0.43 için SDK Azure Machine Learning

+ **Yeni özellikler**
  + Azure Machine Learning artık popüler makine öğrenimi ve veri analizi çerçevesi Scikit-öğrenme için birinci sınıf destek sağlamaktadır. Kullanıcılar, [ `SKLearn` tahmin aracı](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py)kullanarak scikit-öğrenme modellerini kolayca eğitebilir ve dağıtabilir.
    + [Hyperparameter ayarlamayı Scikit ile çalıştırmayı öğrenin-HyperDrive kullanarak öğrenin](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/scikit-learn/training/train-hyperparameter-tune-deploy-with-sklearn/train-hyperparameter-tune-deploy-with-sklearn.ipynb).
  + Yeniden kullanılabilir işlem birimlerini yönetmek için modül ve ModuleVersion sınıflarıyla birlikte işlem hatlarında ModuleStep oluşturma desteği eklendi.
  + ACI WebServices artık güncelleştirmeler aracılığıyla kalıcı scoring_uri destekliyor. Scoring_uri IP 'den FQDN 'ye değişir. FQDN için DNS ad etiketi, deploy_configuration dns_name_label ayarlanarak yapılandırılabilir.
  + Otomatik makine öğrenimi yeni özellikler:
    + Tahmin için STL featurizer
    + KNE anlama özelliği, özellik için kümeleme etkin
  + AmlCompute kota onayları daha hızlı hale geldi! Artık bir eşik içindeki kota isteklerinizi onaylama işlemini otomatik hale aldık. Kotaların nasıl çalıştığı hakkında daha fazla bilgi için [kotaları yönetme hakkında](https://docs.microsoft.com/azure/machine-learning/how-to-manage-quotas)bilgi edinin.

+ **Önizleme özellikleri**
    + Azureml-mlflow paketi ([örnek Not defterleri](https://aka.ms/azureml-mlflow-examples)) aracılığıyla [mlflow](https://mlflow.org) 1.0.0 izleme ile tümleştirme.
    + Jupyter Not defterini çalıştırma olarak gönderme. [API başvuru belgeleri](https://docs.microsoft.com/python/api/azureml-contrib-notebook/azureml.contrib.notebook?view=azure-ml-py)
    + Azureml-contrib-datadrift paketi ([örnek Not defterleri](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/monitor-models/data-drift)) aracılığıyla [veri kayması algılayıcısının](https://docs.microsoft.com/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector(class)) genel önizlemesi. Veri kayması, model doğruluğunun zamana göre düşmesinin en önemli nedenlerinden biridir. Üretimde modele sunulan veriler, modelin eğitilen verilerden farklı olduğunda gerçekleşir. AML Data değişikliklerini algılayıcısı, müşterinin veri kayması 'nı izlemelerine yardımcı olur ve her bir sürücü algılandığında uyarı gönderir.

+ **Yeni değişiklikler**

+ **Hata düzeltmeleri ve geliştirmeleri**
  + RunConfiguration yükleme ve kaydetme, önceki davranış için tam yedekleme ile tam bir dosya yolu belirtmeyi destekler.
  + Serviceprincıpalauthentication içindeki önbelleğe alma, varsayılan olarak kapalıdır.
  + Aynı ölçüm adı altında birden çok çizim günlüğünü etkinleştirin.
  + Model sınıfı artık azureml. Core () öğesinden daha uygun bir şekilde alınabilir. `from azureml.core import Model`
  + İşlem hattı adımlarında, `hash_path` parametre artık kullanım dışıdır. Yeni davranış,. amlignore veya. gitignore içinde listelenen dosyalar hariç karma tamamlanma source_directory.
  + İşlem hattı paketlerinde, çeşitli `get_all` ve `get_all_*` yöntemleri sırasıyla ve için kullanım dışı bırakılmıştır `list` `list_*` .
  + azureml. Core. get_run artık özgün çalışma türünü döndürmeden önce sınıfların içeri aktarılmasını gerektirmez.
  + Bazı WebService güncelleştirmesine yapılan çağrıların bir güncelleştirmeyi tetikleyemediği bir sorun düzeltildi.
  + AKS WebServices üzerinde Puanlama zaman aşımı 5 MS ile 300000 MS arasında olmalıdır. Puanlama istekleri için izin verilen en büyük scoring_timeout_ms, 1 dakikadan 5 dakika arasında tamponlanır.
  + LocalWebservice nesnelerinin artık `scoring_uri` ve `swagger_uri` özellikleri vardır.
  + Taşınan çıkış çıkışları dizin oluşturma ve çıkış Dizin yüklemesi Kullanıcı işleminin dışına çıktı. Her Kullanıcı işleminde çalıştırılacak çalışma geçmişi SDK 'sını etkinleştirdi. Bu, dağıtılmış eğitim çalıştırmaları tarafından karşılaşılan bazı eşitleme sorunlarını çözmelidir.
  + Kullanıcı işlemi adından yazılan azureml günlüğünün adı artık işlem adını (yalnızca dağıtılmış eğitim için) ve PID 'yi içerir.

### <a name="azure-machine-learning-data-prep-sdk-v115"></a>Azure Machine Learning Data Prep SDK v 1.1.5

+ **Hata düzeltmeleri ve geliştirmeleri**
  + 2 basamaklı yıl biçimine sahip yorumlanan tarih saat değerleri için geçerli yıl aralığı Windows 'un yayını ile eşleşecek şekilde güncelleştirilmiştir. Aralık 1930-2029 ' den 1950-2049 ' e değiştirilmiştir.
  + Bir dosya ve ayarı okurken `handleQuotedLineBreaks=True` `\r` Yeni bir satır olarak kabul edilir.
  + Bazı durumlarda başarısız olmasına neden olan bir hata düzeltildi `read_pandas_dataframe` .
  + Performansı geliştirildi `get_profile` .
  + Geliştirilmiş hata iletileri.

## <a name="2019-05-28"></a>2019-05-28

### <a name="azure-machine-learning-data-prep-sdk-v114"></a>Azure Machine Learning Data Prep SDK v 1.1.4

+ **Yeni özellikler**
  + Artık DateTime değerlerini yeni sütunlara ayıklamak ve ayrıştırmak için aşağıdaki ifade dili işlevlerini kullanabilirsiniz.
    + `RegEx.extract_record()`DateTime öğelerini yeni bir sütuna ayıklar.
    + `create_datetime()`ayrı DateTime öğelerinden DateTime nesneleri oluşturur.
  + `get_profile()`' I çağırırken, değerlerin yakın olduğunu açıkça göstermek için, bu quantile sütunlarının (EST.) olarak etiketlenmiş olduğunu görebilirsiniz.
  + Artık, Azure Blob depolamadan okurken * * glob kullanabilirsiniz.
    + DomainName.`dprep.read_csv(path='https://yourblob.blob.core.windows.net/yourcontainer/**/data/*.csv')`

+ **Hata düzeltmeleri**
  + Uzak kaynaktan (Azure blob) bir Parquet dosyası okunmayla ilgili bir hata düzeltildi.

## <a name="2019-05-14"></a>2019-05-14

### <a name="azure-machine-learning-sdk-for-python-v1039"></a>Python v 1.0.39 için SDK Azure Machine Learning
+ **Değişikliklerine**
  + Yapılandırmayı Çalıştır auto_prepare_environment seçeneği, otomatik hazırlama varsayılan olarak devre dışı bırakılmıştır.

## <a name="2019-05-08"></a>2019-05-08

### <a name="azure-machine-learning-data-prep-sdk-v113"></a>Azure Machine Learning Data Prep SDK v 1.1.3

+ **Yeni özellikler**
  + Read_postgresql çağırarak veya bir veri deposu kullanarak bir PostgresSQL veritabanından okuma desteği eklendi.
    + Bkz. nasıl yapılır kılavuzlarındaki örnekler:
      + [Veri alımı Not defteri](https://aka.ms/aml-data-prep-ingestion-nb)
      + [Veri deposu Not defteri](https://aka.ms/aml-data-prep-datastore-nb)

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Sütun türü dönüştürmesi ile ilgili sorunlar düzeltildi:
  + Şimdi Boolean veya sayısal bir sütunu Boole sütununa doğru bir şekilde dönüştürür.
  + Tarih sütunu tarih türü olarak ayarlamaya çalışırken artık başarısız olmaz.
  + Geliştirilmiş JoinType türleri ve başvuru belgeleri. İki veri akışı birleştirilirken, artık bu tür birleştirme türlerinden birini belirtebilirsiniz:
    + NONE, MATCH, ıNNER, UNMATCHLEFT, LEFTANTı, LEFTOUTER, UNMATCHRIGHT, SOLTANTı, TAM OLARAK, FULLANTI, TAM.
  + Daha fazla tarih biçimini tanımak için veri türü artırılmıştır.

## <a name="2019-05-06"></a>2019-05-06

### <a name="azure-portal"></a>Azure portalı

Azure portal, şimdi şunları yapabilirsiniz:
+ Otomatik ML denemeleri oluşturma ve çalıştırma
+ Örnek Jupyter not defterlerini veya kendinizinkini denemek için bir not defteri VM 'si oluşturun.
+ Azure Machine Learning çalışma alanında, otomatik Machine Learning, görsel arabirim ve barındırılan Not defteri VM 'Leri içeren yeni yazma bölümü (Önizleme)
    + Otomatik makine öğrenimi kullanarak otomatik olarak model oluşturma
    + Denemeleri çalıştırmak için sürükle ve bırak görsel arabirimini kullanın
    + Verileri araştırmak, modeller oluşturmak ve hizmetleri dağıtmak için bir not defteri VM 'si oluşturun.
+ Çalışma raporları ve çalışma ayrıntıları sayfalarında canlı grafik ve ölçüm güncelleştirmesi
+ Çalışma ayrıntıları sayfalarındaki Günlükler, çıktılar ve anlık görüntüler için dosya Görüntüleyicisi güncelleştirildi.
+ Denemeleri sekmesinde yeni ve geliştirilmiş rapor oluşturma deneyimi.
+ Dosya config.jsAzure Machine Learning çalışma alanının genel bakış sayfasından indirme özelliği eklendi.
+ Azure Databricks çalışma alanından Azure Machine Learning çalışma alanı oluşturma desteği.

## <a name="2019-04-26"></a>2019-04-26

### <a name="azure-machine-learning-sdk-for-python-v1033"></a>Python v 1.0.33 için SDK Azure Machine Learning
+ **Yeni özellikler**
  + _Workspace. Create_ YÖNTEMI artık CPU ve GPU kümeleri için varsayılan küme yapılandırmasını kabul eder.
  + Çalışma alanı oluşturma başarısız olursa, bağımlı kaynaklar temizlenir.
  + Varsayılan Azure Container Registry SKU 'SU temel olarak değiştirildi.
  + Azure Container Registry, çalıştırma veya görüntü oluşturma için gerektiğinde geç oluşturulur.
  + Eğitim çalıştırmaları için ortamlar desteği.

### <a name="notebook-virtual-machine"></a>Not defteri sanal makinesi 

Machine Learning denemeleri 'i programtabileceğiniz, modelleri Web uç noktaları olarak dağıtmak ve Python kullanarak Azure Machine Learning SDK tarafından desteklenen tüm diğer işlemleri gerçekleştirmek için bir not defteri VM 'yi, Jupyter Not defterleri için güvenli, kurumsal kullanıma yönelik bir barındırma ortamı olarak kullanın.Çeşitli yetenekler sağlar:
+ [Önceden yapılandırılmış bir not DEFTERI VM](tutorial-1st-experiment-sdk-setup.md)   'sini hızla çalıştırın Bu, Azure Machine Learning SDK ve ilgili paketlerin en son sürümüne sahiptir.
+ Erişim, HTTPS, Azure Active Directory kimlik doğrulaması ve yetkilendirme gibi kanıtlanmış teknolojiler aracılığıyla güvenli hale getirilir.
+ Azure Machine Learning Çalışma Alanı BLOB depolama hesabınızda Not defterlerinin ve kodun güvenilir bulut depolaması.Çalışmanızı kaybetmeden Not defteri sanal makinenizin güvenli bir şekilde silinmesini sağlayabilirsiniz.
+ Azure Machine Learning özellikleri araştırmak ve denemek için önceden yüklenmiş örnek Not defterleri.
+ Azure VM 'lerinin tam özelleştirme özellikleri, herhangi bir VM türü, herhangi bir paket, herhangi bir sürücü. 

## <a name="2019-04-26"></a>2019-04-26

### <a name="azure-machine-learning-sdk-for-python-v1033-released"></a>Python v 1.0.33 için SDK Azure Machine Learning.

+ [Fpgas](how-to-deploy-fpga-web-service.md) ÜZERINDE Azure ML hızlandırılmış donanım modelleri genel kullanıma sunulmuştur.
  + Artık, şunları yapmak için [azureml-Accel-modeller paketini kullanabilirsiniz](how-to-deploy-fpga-web-service.md) :
    + Desteklenen bir derin sinir ağının ağırlıklarını (ResNet 50, ResNet 152, DenseNet-121, VGG-16 ve SSD-VGG) eğitin
    + Desteklenen DNN ile aktarım öğrenimi kullanma
    + Modeli Model Yönetimi hizmeti ile kaydedin ve modeli Kapsayıcılı yapın
    + Azure Kubernetes Service (AKS) kümesinde bir FPGA ile modeli bir Azure VM 'ye dağıtma
  + Kapsayıcıyı bir [Azure Data Box Edge](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview) sunucusu cihazına dağıtma
  + Bu [örnekle](https://github.com/Azure-Samples/aml-hardware-accelerated-models) GRPC uç noktasıyla verilerinizi puan edin

### <a name="automated-machine-learning"></a>Otomatik Makine Öğrenmesi

+ Performans iyileştirmesi için dinamik ekleme özelliğini etkinleştirme özelliği :::no-loc text="featurizers"::: . Yeni :::no-loc text="featurizers"::: : iş katıştırıtları, kanıt ağırlığı, hedef kodlamalar, metin hedefi kodlama, küme uzaklığı
+ Otomatik ML içinde tren/geçerli bölmeler işlemek için akıllı CV
+ Birkaç bellek iyileştirme değişikliği ve çalışma zamanı performans iyileştirmesi
+ Model açıklamasında performans iyileştirmesi
+ Yerel çalıştırma için ONNX model dönüştürmesi
+ Alt örnekleme desteği eklendi
+ Hiçbir çıkış ölçütü tanımlanmadığı zaman akıllı durduruluyor
+ Yığılmış Kümelemeler

+ Zaman Serileri Tahmini
  + Yeni tahmin tahmini işlevi
  + Artık zaman serisi verilerinde sıralı kaynak çapraz doğrulamayı kullanabilirsiniz
  + Zaman serisi lags 'yi yapılandırmak için yeni işlevler eklendi
  + Sıralı pencere toplu özelliklerini desteklemek için yeni işlevler eklendi
  + Deneme ayarlarında ülke kodu tanımlandığında yeni tatil algılama ve korleştirici

+ Azure Databricks
  + Etkin zaman serisi tahmin ve model explainabilty/ıntertability özelliği
  + Artık iptal ve devam edebilirsiniz (devam) otomatik ML denemeleri
  + Çok birimli işleme desteği eklendi

### <a name="mlops"></a>MLOps
+ **Puanlama kapsayıcıları için yerel dağıtım & hata ayıklaması**<br/> Artık bir ML modelini yerel olarak dağıtabilir ve beklenen şekilde davrandıklarından emin olmak için Puanlama dosyanıza ve bağımlılıklarınızla hızla yineleyebilirsiniz.

+ **Sunulan ınenceconfig & model. deploy ()**<br/> Model dağıtımı artık, bir RunConfig ile aynı olan bir giriş betiği ile kaynak klasörü belirtmeyi destekliyor.  Ayrıca, model dağıtımı tek bir komuta basitleştirilmiştir.

+ **Git başvurusu izleme**<br/> Müşteriler, uçtan uca bir denetim izinin devam etmesine yardımcı olduğundan bir süre için temel git tümleştirme özellikleri istiyor. Git ile ilgili meta veriler (depo, işleme, temiz durum) için Azure ML 'deki önemli varlıklar genelinde izlemeyi uyguladık. Bu bilgiler SDK ve CLı tarafından otomatik olarak toplanır.

+ **Model profil oluşturma & doğrulama hizmeti**<br/> Müşteriler, çıkarım hizmeti ile ilişkili işlemi doğru şekilde boyutlandırma zorluğunu sıklıkla şikayet ediyor. Model profil oluşturma hizmetimizde müşteri, örnek girişler sağlayabilir ve dağıtım için en iyi boyutlandırmayı belirlemede 16 farklı CPU/bellek yapılandırmasında profil göndereceğiz.

+ **Çıkarımı için kendi taban görüntünüzü getirin**<br/> Diğer bir yaygın şikayet, deneme ' den çıkarımı YENIDEN paylaşma bağımlılıklarından geçiş yaparken zorluk görmekte. Yeni temel görüntü paylaşma olansunumuzdan, artık deneme temel görüntülerini, bağımlılıklarını ve tümünü, çıkarım için yeniden kullanabilirsiniz. Bu, dağıtımları hızlandırmalı ve Inner 'ten dıştaki döngüye kadar olan boşluğu azaltmalıdır.

+ **Geliştirilmiş Swagger şema oluşturma deneyimi**<br/> Önceki Swagger oluşturma yönteminiz hata durumunda ve otomatikleştirilmesine neden oldu. Dekoratörler aracılığıyla herhangi bir Python işlevinden Swagger şemaları oluşturmanın yeni bir çevrimiçi yoludur. Bu kodu açık bölümliyoruz ve şema oluşturma protokoliz Azure ML platformuna bağlı değil.

+ **Azure ML CLI genel olarak kullanılabilir (GA)**<br/> Modeller artık tek bir CLı komutuyla dağıtılabilir. Bir Jupyter Not defterinden ML modeli dağıttığı yaygın müşteri görüşleri sunuyoruz. [**CLI başvuru belgeleri**](https://aka.ms/azmlcli) güncelleştirildi.


## <a name="2019-04-22"></a>2019-04-22

Python v 1.0.30 için SDK Azure Machine Learning.

[`PipelineEndpoint`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline_endpoint.pipelineendpoint?view=azure-ml-py)Aynı uç noktayı koruyarak yayımlanmış bir işlem hattının yeni bir sürümünü eklemek için sunulmuştur.

## <a name="2019-04-17"></a>2019-04-17

### <a name="azure-machine-learning-data-prep-sdk-v112"></a>Azure Machine Learning Data Prep SDK v 1.1.2

Note: Data Prep Python SDK artık yüklenemez `numpy` ve `pandas` paketlerdir. Bkz. [güncelleştirilmiş yükleme yönergeleri](https://github.com/Microsoft/AMLDataPrepDocs).

+ **Yeni özellikler**
  + Artık Pivot dönüşümünü kullanabilirsiniz.
    + Nasıl yapılır Kılavuzu: [Özet Not defteri](https://aka.ms/aml-data-prep-pivot-nb)
  + Artık yerel işlevlerde normal ifadeleri kullanabilirsiniz.
    + Örnekler:
      + `dflow.filter(dprep.RegEx('pattern').is_match(dflow['column_name']))`
      + `dflow.assert_value('column_name', dprep.RegEx('pattern').is_match(dprep.value))`
  + Artık `to_upper`   `to_lower`   ifade dilinde işlevleri kullanabilirsiniz.
  + Artık bir veri profilindeki her bir sütunun benzersiz değerlerinin sayısını görebilirsiniz.
  + Yaygın olarak kullanılan bazı okuyucu adımları için artık `infer_column_types` bağımsız değişkeni geçirebilirsiniz. Olarak ayarlanırsa `True` , veri hazırlığı sütun türlerini algılamaya ve otomatik olarak dönüştürmeye çalışır.
    + `inference_arguments`Artık kullanım dışıdır.
  + Artık çağırabilirsiniz `Dataflow.shape` .

+ **Hata düzeltmeleri ve geliştirmeleri**
  + `keep_columns` Artık ek bir isteğe bağlı bağımsız değişkeni kabul eder `validate_column_exists` , bu da sonucun `keep_columns` herhangi bir sütun içerecekse olup olmadığını denetler.
  + Tüm okuyucu adımları (dosyadan okunan) artık ek bir isteğe bağlı bağımsız değişkeni kabul eder `verify_exists` .
  + Pandas dataframe 'ten okuma ve veri Profilleri alma performansı geliştirildi.
  + Bir veri akışından tek bir adımı Dilimleme tek bir dizin ile başarısız olan bir hata düzeltildi.

## <a name="2019-04-15"></a>2019-04-15

### <a name="azure-portal"></a>Azure portalı
  + Artık var olan bir uzak işlem kümesinde çalışan bir betiği yeniden gönderebilirsiniz.
  + Artık, işlem hatları sekmesinde yeni parametrelerle yayınlanmış bir işlem hattı çalıştırabilirsiniz.
  + Çalışma ayrıntıları artık yeni bir Snapshot dosya görüntüleyicisini destekliyor. Belirli bir çalıştırma gönderdiğinizde dizinin anlık görüntüsünü görüntüleyebilirsiniz. Çalıştırmaya başlamak için gönderilen Not defterini de indirebilirsiniz.
  + Artık Azure portal üst çalıştırmaları iptal edebilirsiniz.

## <a name="2019-04-08"></a>2019-04-08

### <a name="azure-machine-learning-sdk-for-python-v1023"></a>Python v 1.0.23 için SDK Azure Machine Learning

+ **Yeni özellikler**
  + Azure Machine Learning SDK artık Python 3,7 ' i desteklemektedir.
  + Azure Machine Learning DNN estimators artık yerleşik çoklu sürüm desteği sağlıyor. Örneğin, `TensorFlow`   tahmin aracı şimdi bir parametreyi kabul eder `framework_version` ve kullanıcılar ' 1,10 ' veya ' 1,12 ' sürümünü belirtebilir. Geçerli SDK sürümleriniz tarafından desteklenen sürümlerin bir listesi için, `get_supported_versions()` istenen Framework sınıfını (örneğin, `TensorFlow.get_supported_versions()` ) çağırın.
  En son SDK sürümü tarafından desteklenen sürümlerin bir listesi için [DNN Estimator belgelerine](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn?view=azure-ml-py)bakın.

### <a name="azure-machine-learning-data-prep-sdk-v111"></a>Azure Machine Learning Data Prep SDK v 1.1.1

+ **Yeni özellikler**
  + Read_ * dönüşümler kullanarak birden çok veri deposu/veri yolu/DataReference kaynağını okuyabilirsiniz.
  + Yeni bir sütun oluşturmak için sütunlar üzerinde aşağıdaki işlemleri gerçekleştirebilirsiniz: bölme, kat, modül, güç, uzunluk.
  + Veri hazırlığı artık Azure ML tanılama paketinin bir parçasıdır ve tanılama bilgilerini varsayılan olarak günlüğe kaydeder.
    + Bunu kapatmak için, bu ortam değişkenini true olarak ayarlayın: DISABLE_DPREP_LOGGER

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Yaygın olarak kullanılan sınıflar ve işlevler için geliştirilmiş kod belgeleri.
  + Excel dosyalarını okuyameyen auto_read_file bir hata düzeltildi.
  + Read_pandas_dataframe klasörünün üzerine yazma seçeneği eklendi.
  + Dotnetcore2 bağımlılık yüklemesinin performansı geliştirilmiştir ve Fedora 27/28 ve Ubuntu 1804 desteği eklenmiştir.
  + Azure Bloblarından okuma performansı geliştirildi.
  + Sütun türü algılama artık uzun türdeki sütunları destekliyor.
  + Bazı tarih değerlerinin Python DateTime nesneleri yerine zaman damgası olarak görüntülendiği bir hata düzeltildi.
  + Bazı tür sayımlarının tamsayılar yerine Double olarak görüntülendiği bir hata düzeltildi.


## <a name="2019-03-25"></a>2019-03-25

### <a name="azure-machine-learning-sdk-for-python-v1021"></a>Python v 1.0.21 için SDK Azure Machine Learning

+ **Yeni özellikler**
  + *Azureml. Core. Run. create_children* yöntemi, tek bir çağrıda birden çok alt çalıştırmanın düşük gecikmeli oluşturulmasına izin verir.

### <a name="azure-machine-learning-data-prep-sdk-v110"></a>Azure Machine Learning Data Prep SDK v 1.1.0

+ **Yeni değişiklikler**
  + Veri hazırlama paketinin kavramı kullanım dışı bırakılmış ve artık desteklenmiyor. Tek bir pakette birden çok veri akışını kalıcı hale getirmek yerine, veri akışlarını ayrı ayrı kalıcı hale getirebilirsiniz.
    + Nasıl yapılır Kılavuzu: [veri akışları Not defterini açma ve kaydetme](https://aka.ms/aml-data-prep-open-save-dataflows-nb)

+ **Yeni özellikler**
  + Veri hazırlama artık belirli bir anlam türüyle eşleşen sütunları tanıyabilir ve buna uygun şekilde ayrılabilir. Şu anda desteklenen STypes 'ler şunlardır: e-posta adresi, Coğrafi Koordinatlar (Enlem & Boylam), IPv4 ve IPv6 adresleri, ABD telefon numarası ve ABD ZIP kodu.
    + Nasıl yapılır Kılavuzu: [anlamsal türler Not defteri](https://aka.ms/aml-data-prep-semantic-types-nb)
  + Veri hazırlığı artık iki sayısal sütundan bir sonuç sütunu oluşturmak için aşağıdaki işlemleri destekler: çıkarma, çarpma, bölme ve mod.
  + Veri `verify_has_data()` akışının yürütüldüğünde kayıt oluşturulup oluşturulmayacağını denetlemek için bir veri akışı üzerinde arama yapabilirsiniz.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Artık sayısal sütun profilleri için bir histogram içinde kullanılacak bölme sayısını belirtebilirsiniz.
  + `read_pandas_dataframe`Dönüştürme artık veri çerçevesinin dize veya bayt türünde sütun adlarına sahip olmasını gerektirir.
  + Bir hata düzeltildi `fill_nulls` , ancak sütun eksik ise değerler doğru doldurulmadı.

## <a name="2019-03-11"></a>2019-03-11

### <a name="azure-machine-learning-sdk-for-python-v1018"></a>Python v 1.0.18 için SDK Azure Machine Learning

 + **Değişikliklerine**
   + Azureml-tensorboard paketi, azureml-contrib-tensorboard yerini alır.
   + Bu sürümle birlikte, yönetilen işlem kümenizde (amlcompute), oluştururken bir kullanıcı hesabı ayarlayabilirsiniz. Bu, sağlama yapılandırmasında bu özellikler geçirerek yapılabilir. Daha fazla ayrıntı için [SDK başvuru belgelerine](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#provisioning-configuration-vm-size-----vm-priority--dedicated---min-nodes-0--max-nodes-none--idle-seconds-before-scaledown-none--admin-username-none--admin-user-password-none--admin-user-ssh-key-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--tags-none--description-none--remote-login-port-public-access--notspecified--)ulaşabilirsiniz.

### <a name="azure-machine-learning-data-prep-sdk-v1017"></a>Azure Machine Learning Data Prep SDK v 1.0.17

+ **Yeni özellikler**
  + Artık ifade dilini kullanarak sonuç sütunu oluşturmak için iki sayısal sütun eklenmesini destekler.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Random_split için belge ve parametre denetimi geliştirildi.

## <a name="2019-02-27"></a>2019-02-27

### <a name="azure-machine-learning-data-prep-sdk-v1016"></a>Azure Machine Learning Data Prep SDK v 1.0.16

+ **Hata onarımı**
  + Bir API değişikliği nedeniyle oluşan bir hizmet sorumlusu kimlik doğrulama sorunu düzeltildi.

## <a name="2019-02-25"></a>2019-02-25

### <a name="azure-machine-learning-sdk-for-python-v1017"></a>Python v 1.0.17 için SDK Azure Machine Learning

+ **Yeni özellikler**

  + Azure Machine Learning artık popüler DNN Framework Chainer için birinci sınıf destek sağlar. [`Chainer`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py)Sınıf kullanıcıları, Chainer modellerini kolayca eğitebilir ve dağıtabilir.
    + [Chaeylemsizlik ile dağıtılmış eğitime nasıl çalıştırılacağını](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/chainer/training/distributed-chainer/distributed-chainer.ipynb) öğrenin
    + [Hyperparameter ayarlamayı hiper sürücü kullanarak Chainer ile çalıştırma](https://github.com/Azure/MachineLearningNotebooks/blob/b881f78e4658b4e102a72b78dbd2129c24506980/how-to-use-azureml/ml-frameworks/chainer/deployment/train-hyperparameter-tune-deploy-with-chainer/train-hyperparameter-tune-deploy-with-chainer.ipynb) hakkında bilgi edinin
  + Azure Machine Learning işlem hatları, veri deposu değişikliklerine göre bir işlem hattı çalıştırmasını tetikleyebilme özelliği eklendi. İşlem hattı [zamanlama Not defteri](https://aka.ms/pl-schedule) , bu özelliği göstermek için güncelleştirilir.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Source_directory_data_store özelliğini, [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep?view=azure-ml-py)Için sağlanan [runconfigurations](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.runconfiguration?view=azure-ml-py) 'lar üzerinde istenen veri deposuna (BLOB depolama gibi) ayarlamaya yönelik Azure Machine Learning işlem hatlarında destek ekledik. Varsayılan olarak, çok sayıda adım eşzamanlı olarak yürütüldüğünde sorunları azaltma sorunlarını ortadan kaldırarak Azure dosya deposunu, yedekleme veri deposu olarak kullanır.

### <a name="azure-portal"></a>Azure portalı

+ **Yeni özellikler**
  + Raporlar için yeni Sürükle ve bırak tablosu Düzenleyicisi deneyimi. Kullanıcılar bir sütunu, tablonun önizlemesinin görüntüleneceği tablo alanına kadar iyi sürükleyebilirsiniz. Sütunlar yeniden düzenlenebilir.
  + Yeni günlük dosyası Görüntüleyici
  + Etkinlik sekmesinden deneme çalıştırmaları, işlem, modeller, görüntüler ve dağıtımlara bağlantılar

### <a name="azure-machine-learning-data-prep-sdk-v1015"></a>Azure Machine Learning Data Prep SDK v 1.0.15

+ **Yeni özellikler**
  + Veri hazırlığı artık bir veri akışından dosya akışlarının yazılmasını desteklemektedir. Ayrıca, yeni dosya adları oluşturmak için dosya akışı adlarını değiştirme olanağı da sağlar.
    + Nasıl yapılır Kılavuzu: [dosya akışları not defteriyle çalışma](https://aka.ms/aml-data-prep-file-stream-nb)

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Büyük veri kümelerinde t-Digest performansı geliştirildi.
  + Veri hazırlığı artık verileri bir veri yolundan okumayı destekliyor.
  + Artık bir etkin kodlama, Boolean ve sayısal sütunlarda çalışır.
  + Diğer çeşitli hata düzeltmeleri.

## <a name="2019-02-11"></a>2019-02-11

### <a name="azure-machine-learning-sdk-for-python-v1015"></a>Python v 1.0.15 için SDK Azure Machine Learning

+ **Yeni özellikler**
  + AzureBatchStep ([Not defteri](https://aka.ms/pl-azbatch)), HyperDriveStep (Not defteri) ve zaman tabanlı zamanlama işlevselliği ([Not defteri](https://aka.ms/pl-schedule)) eklenen Azure Machine Learning işlem hatları.
  +  DataTranferStep, Azure SQL veritabanı ve PostgreSQL için Azure veritabanı ([Not defteri](https://aka.ms/pl-data-trans)) ile çalışacak şekilde güncelleştirildi.

+ **Değişikliklerine**
  + Kullanım dışı bırakıldı `PublishedPipeline.get_published_pipeline` `PublishedPipeline.get` .
  + Kullanım dışı bırakıldı `Schedule.get_schedule` `Schedule.get` .

### <a name="azure-machine-learning-data-prep-sdk-v1012"></a>Azure Machine Learning Data Prep SDK v 1.0.12

+ **Yeni özellikler**
  + Veri hazırlığı artık Azure SQL veritabanı 'ndan veri deposu kullanarak okumayı desteklemektedir.

+ **Değişikliklerine**
  + Büyük veriler üzerinde belirli işlemlerin bellek performansı geliştirilmiştir.
  + `read_pandas_dataframe()`Şimdi `temp_folder` belirtilmesini gerektirir.
  + `name`Özelliği `ColumnProfile` kullanım dışı bırakıldı- `column_name` bunun yerine kullanım dışıdır.

## <a name="2019-01-28"></a>2019-01-28

### <a name="azure-machine-learning-sdk-for-python-v1010"></a>Python v 1.0.10 için SDK Azure Machine Learning

+ **Değişiklikler**:
  + Azure ML SDK artık bağımlılık olarak Azure-CLI paketlerine sahip değildir. Özellikle, Azure-CLI-Core ve Azure-CLI-profile bağımlılıkları azureml-Core ' dan kaldırılmıştır. Bunlar, değişiklikleri etkileyen Kullanıcı ' lardır:
      + "Az Login" ve ardından azureml-SDK kullanıyorsanız SDK, tarayıcı veya cihaz kodu oturum açmaları bir kez daha zaman kullanacaktır. "Az Login" tarafından oluşturulan herhangi bir kimlik bilgisi durumunu kullanmaz.
    + "Az Login" kullanımı gibi Azure CLı kimlik doğrulaması için, _azureml. Core. Authentication. Azureclıauthentication_ sınıfını kullanın. Azure CLı kimlik doğrulaması için, _Azure-CLI_ 'yi, azureml-SDK yüklediğiniz Python ortamında Azure-CLI ' yi yükleme.
    + Otomasyon için bir hizmet sorumlusu kullanarak "az oturum açma" işlemi yapıyorsanız, azureml-SDK, Azure CLı tarafından oluşturulan kimlik bilgileri durumunu kullanmayacağından, _azureml. Core. Authentication. Serviceprincıpalauthentication_ sınıfını kullanmanızı öneririz.

+ **Hata düzeltmeleri**: Bu sürüm genellikle küçük hata düzeltmeleri içerir

### <a name="azure-machine-learning-data-prep-sdk-v108"></a>Azure Machine Learning Data Prep SDK v 1.0.8

+ **Hata düzeltmeleri**
  + Veri profillerinin alınması performansı geliştirildi.
  + Hata raporlama ile ilgili küçük hatalar düzeltildi.

### <a name="azure-portal-new-features"></a>Azure portal: yeni özellikler
+ Raporlar için yeni sürükleme ve bırakma grafik deneyimi. Kullanıcılar bir sütunu ya da özniteliği, sistemin veri türüne göre otomatik olarak uygun bir grafik türü seçlebileceği grafik alanına sürükleyebilirsiniz. Kullanıcılar grafik türünü diğer geçerli türler olarak değiştirebilir veya ek öznitelikler ekleyebilir.

    Desteklenen grafik türleri:
    - Çizgi grafik
    - Histogram
    - Yığılmış çubuk grafik
    - Kutu çizimi
    - Dağılım çizimi
    - Kabarcık çizimi
+ Portal artık denemeleri için raporları dinamik olarak oluşturur. Bir Kullanıcı bir denemeye bir çalıştırma gönderdiğinde, farklı çalışma genelinde karşılaştırmaya izin vermek için günlüğe kaydedilen ölçümler ve grafiklerle birlikte otomatik olarak bir rapor oluşturulur.

## <a name="2019-01-14"></a>2019-01-14

### <a name="azure-machine-learning-sdk-for-python-v108"></a>Python v 1.0.8 için SDK Azure Machine Learning

+ **Hata düzeltmeleri**: Bu sürüm genellikle küçük hata düzeltmeleri içerir

### <a name="azure-machine-learning-data-prep-sdk-v107"></a>Azure Machine Learning Data Prep SDK v 1.0.7

+ **Yeni özellikler**
  + Veri deposu geliştirmeleri ( [veri deposu nasıl yapılır Kılavuzu](https://aka.ms/aml-data-prep-datastore-nb)'nda belgelenmiştir)
    + Azure dosya paylaşımında ve ADLS veri depolarından okuma ve yazma özelliği ölçeği artırma özelliği eklendi.
    + Veri hazırlığı, Datamağazaların kullanıldığı sırada etkileşimli kimlik doğrulaması yerine hizmet sorumlusu kimlik doğrulamasının kullanılmasını desteklemektedir.
    + ,, Ve için ve diğer URL 'ler için destek eklendi.

## <a name="2019-01-09"></a>2019-01-09

### <a name="azure-machine-learning-data-prep-sdk-v106"></a>Azure Machine Learning Data Prep SDK v 1.0.6

+ **Hata düzeltmeleri**
  + Spark 'ta ortak okunabilir Azure Blob kapsayıcılarından okuma ile düzeltilen hata

## <a name="2018-12-20"></a>2018-12-20

### <a name="azure-machine-learning-sdk-for-python-v106"></a>Python v 1.0.6 için SDK Azure Machine Learning
+ **Hata düzeltmeleri**: Bu sürüm genellikle küçük hata düzeltmeleri içerir

### <a name="azure-machine-learning-data-prep-sdk-v104"></a>Azure Machine Learning Data Prep SDK v 1.0.4

+ **Yeni özellikler**
  + `to_bool`işlev artık eşleşmeyen değerlerin hata değerlerine dönüştürülmesine izin veriyor. Bu, ve için yeni varsayılan uyumsuzluk davranışıdır `to_bool` `set_column_types` , ancak önceki varsayılan davranış eşleşmeyen değerleri yanlış olarak dönüştürmelidir.
  + Çağrılırken `to_pandas_dataframe` , sayısal sütunlardaki null/eksik değerleri Nan olarak yorumlamak için yeni bir seçenek vardır.
  + Tür tutarlılığı sağlamak ve erken yük devretmek için bazı ifadelerin dönüş türünü denetleme özelliği eklendi.
  + Artık `parse_json` bir sütundaki DEĞERLERI JSON nesneleri olarak ayrıştırmaya ve bunları birden çok sütuna genişletmeye çağrı yapabilirsiniz.

+ **Hata düzeltmeleri**
  + Python 3.5.2 'te kilitlenen bir hata düzeltildi `set_column_types` .
  + AML görüntüsü kullanılarak veri deposuna bağlanılırken kilitlenen hata düzeltildi.

+ **Güncelleştirmeler**
  * Başlarken öğreticileri, örnek olay incelemeleri ve nasıl yapılır kılavuzlarından oluşan [örnek Not defterleri](https://aka.ms/aml-data-prep-notebooks) .

## <a name="2018-12-04-general-availability"></a>2018-12-04: genel kullanılabilirlik

Azure Machine Learning genel kullanıma sunulmuştur.

### <a name="azure-machine-learning-compute"></a>Azure Machine Learning İşlemi
Bu sürümle birlikte [Azure Machine Learning işlem](how-to-set-up-training-targets.md#amlcompute)aracılığıyla yeni bir yönetilen işlem deneyimi duyuruyoruz. Bu işlem hedefi Azure Machine Learning Azure Batch AI işlem yerini alır.

Bu işlem hedefi:
+ Model eğitimi ve toplu iş çıkarımı/Puanlama için kullanılır
+ Tek-çok düğümlü işlem
+ Kullanıcı için küme yönetimi ve iş zamanlaması yapar
+ Varsayılan olarak otomatik ölçekler
+ Hem CPU hem de GPU kaynakları için destek
+ Düşük öncelikli VM 'lerin daha düşük maliyetli olarak kullanılmasına izin verebilir

Azure Machine Learning Işlem, Python 'da Azure portal veya CLı kullanılarak oluşturulabilir. Çalışma alanınızın bölgesinde oluşturulması gerekir ve başka hiçbir çalışma alanına iliştirilemez. Bu işlem hedefi, çalıştırmanız için bir Docker kapsayıcısı kullanır ve bağımlılıklarınızı, tüm düğümleriniz genelinde aynı ortamı çoğaltmak üzere paketleyebilirsiniz.

> [!Warning]
> Azure Machine Learning Işlem kullanmak için yeni bir çalışma alanı oluşturmanızı öneririz. Mevcut bir çalışma alanından Azure Machine Learning Işlem oluşturmaya çalışan kullanıcıların bir hata görmesini sağlayan bir uzak şans vardır. Çalışma alanınızdaki mevcut işlem etkilenmeden çalışmaya devam etmelidir.

### <a name="azure-machine-learning-sdk-for-python-v102"></a>Python v 1.0.2 için SDK Azure Machine Learning
+ **Yeni değişiklikler**
  + Bu sürümle, Azure Machine Learning VM oluşturma desteğini kaldırdık. Mevcut bir bulut VM 'sini veya uzak bir şirket içi sunucuyu ekleyebilirsiniz.
  + Batchaı desteğini de kaldırdık. bu Işlem şu anda Azure Machine Learning Işlem aracılığıyla desteklenmelidir.

+ **Yeni**
  + Makine öğrenimi ardışık düzenleri için:
    + [EstimatorStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.estimator_step.estimatorstep?view=azure-ml-py)
    + [HyperDriveStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.hyper_drive_step.hyperdrivestep?view=azure-ml-py)
    + [MpiStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.mpi_step.mpistep?view=azure-ml-py)


+ **Güncellendi**
  + Makine öğrenimi ardışık düzenleri için:
    + [Databricksstep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.databricks_step.databricksstep?view=azure-ml-py) şimdi runconfig 'i kabul ediyor
    + [Datatransferstep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) artık SQL veri kaynağından ve bu kaynaktan kopyalar
    + Yayınlanmış işlem hatlarını çalıştırmaya yönelik zamanlamalar oluşturmak ve güncelleştirmek için SDK 'daki işlevselliği zamanlayın

<!--+ **Bugs fixed**-->

### <a name="azure-machine-learning-data-prep-sdk-v052"></a>Azure Machine Learning Data Prep SDK v 0.5.2
+ **Yeni değişiklikler**
  * `SummaryFunction.N`, olarak yeniden adlandırıldı `SummaryFunction.Count` .

+ **Hata Düzeltmeleri**
  * Uzak çalışmalardaki veri depolarından okuma ve yazma yaparken en son AML çalıştırma belirtecini kullanın. Daha önce, AML çalıştırma belirteci Python 'da güncelleştirilirse, Data Prep çalışma zamanı güncelleştirilmiş AML çalıştırma belirteciyle güncellenmez.
  * Ek daha net hata iletileri
  * Spark serileştirme kullandığında to_spark_dataframe () artık kilitlenmeyecektir `Kryo`
  * Değer sayısı denetçisi artık 1000 ' den fazla benzersiz değer gösteriyor olabilir
  * Özgün veri akışının adı yoksa rastgele bölme artık başarısız olmaz

+ **Daha fazla bilgi**
  * [Azure Machine Learning Veri Hazırlama SDK'sı](https://aka.ms/data-prep-sdk)

### <a name="docs-and-notebooks"></a>Belgeler ve Not defterleri
+ ML işlem hatları
  + İşlem hatları, toplu iş kapsamları ve stil aktarma örnekleri ile çalışmaya başlama için yeni ve güncelleştirilmiş Not defterleri:https://aka.ms/aml-pipeline-notebooks
  + [İlk işlem hattınızı oluşturmayı](how-to-create-your-first-pipeline.md) öğrenin
  + [İşlem hatlarını kullanarak toplu tahmine dayalı tahminleri nasıl çalıştıracağınızı](how-to-use-parallel-run-step.md) öğrenin
+ Azure Machine Learning işlem hedefi
  + [Örnek Not defterleri](https://aka.ms/aml-notebooks) artık yeni yönetilen işlem kullanacak şekilde güncelleştirilir.
  + [Bu işlem hakkında bilgi edinin](how-to-set-up-training-targets.md#amlcompute)

### <a name="azure-portal-new-features"></a>Azure portal: yeni özellikler
+ Portalda [Azure Machine Learning işlem](how-to-set-up-training-targets.md#amlcompute) türleri oluşturun ve yönetin.
+ Azure Machine Learning Işlem için kota kullanımını ve [istek kotasını](how-to-manage-quotas.md) izleyin.
+ Işlem kümesi durumunu gerçek zamanlı olarak görüntüleyin Azure Machine Learning.
+ Azure Machine Learning Işlem ve Azure Kubernetes hizmeti oluşturmak için sanal ağ desteği eklenmiştir.
+ Mevcut parametrelerle yayınlanmış işlem hatlarınızı yeniden çalıştırın.
+ Sınıflandırma modelleri için yeni [otomatik makine öğrenimi grafikleri](how-to-understand-automated-ml.md) (yükseltme, kazanç, ayarlama, model explainability ile özellik önem grafiği) ve regresyon modelleri (model explainability ile birlikte kalan özellikler ve özellik önemi grafiği).
+ İşlem hatları Azure portal görüntülenebilir




## <a name="2018-11-20"></a>2018-11-20

### <a name="azure-machine-learning-sdk-for-python-v0180"></a>Python v 0.1.80 için SDK Azure Machine Learning

+ **Yeni değişiklikler**
  * *azureml. tren. pencere* öğesi ad alanı, *azureml. pencere öğeleri*'ne taşındı.
  * *azureml. Core. COMPUTE. AmlCompute* aşağıdaki sınıfları kullanımdan kaldırır- *azureml.core.compute.Batchaıute* ve *azureml. Core. COMPUTE. dsvmcompute*. Sonraki sürümlerde ikinci sınıf kaldırılacak. AmlCompute sınıfının şimdi daha kolay bir tanımı vardır ve yalnızca bir vm_size ve max_nodes ihtiyacı vardır ve bir iş gönderildiğinde kümenizi otomatik olarak 0 ' dan max_nodes ölçeklendirecektir. [Örnek Not defterleriniz](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training) bu bilgilerle güncelleştirildi ve size kullanım örnekleri vermelidir. Bu basitleştirme ve çok sayıda daha heyecan verici özelliğin daha sonraki bir sürümde gelmesini umuyoruz!

### <a name="azure-machine-learning-data-prep-sdk-v051"></a>Azure Machine Learning Data Prep SDK v 0.5.1

[Başvuru belgelerini](https://aka.ms/data-prep-sdk)okuyarak veri hazırlığı SDK 'sı hakkında daha fazla bilgi edinin.
+ **Yeni Özellikler**
   * DataPrep paketlerini yürütmek ve bir veri kümesi veya veri akışı için veri profilini görüntülemek üzere yeni bir DataPrep CLı oluşturuldu
   * Kullanılabilirliği artırmak için SetColumnType API yeniden tasarlandı
   * Auto_read_file smart_read_file yeniden adlandırıldı
   * Artık veri profilinde skew ve basıklığını içerir
   * , Bağlı örneklemeyle örnek oluşturabilir
   * CSV dosyaları içeren ZIP dosyalarından okuyabilir
   * Veri kümeleri satır temelinde rastgele bölme ile bölebilir (örneğin, test-tren kümelerine)
   * ' İ çağırarak bir veri akışından veya bir veri profilinden tüm sütun veri türlerini alabilir.`.dtypes`
   * ' İ çağırarak bir veri akışından veya bir veri profilinden satır sayısı alabilir.`.row_count`

+ **Hata Düzeltmeleri**
   * Uzun-Double dönüşümü düzeltildi
   * Herhangi bir sütundan sonra düzeltilen onaylama
   * Belirsizlik Gruplandırmayla ilgili bir sorun düzeltildi ve bazı durumlarda grupları algılayamayacağı
   * Çok sütunlu sıralama sırasına göre sabit sıralama işlevi
   * Onları nasıl işleyeceğinize benzer sabit ve/veya ifadeler `pandas`
   * DBFS yolundan düzeltilen okuma
   * Hata iletileri daha fazla anlaşılır hale getirilir
   * Artık bir AML belirteci kullanılarak uzak işlem hedefinde okurken artık başarısız olmaz
   * Artık Linux DSVM 'de başarısız oluyor
   * Dize olmayan değerler dize yüklemelerinde artık kilitlenmeyen
   * Artık veri akışı doğru bir şekilde başarısız olduğunda onaylama hatalarını işler
   * Artık Azure Databricks üzerinde dbutils bağlı depolama konumlarını destekliyor

## <a name="2018-11-05"></a>2018-11-05

### <a name="azure-portal"></a>Azure portalı
Azure Machine Learning için Azure portal aşağıdaki güncelleştirmelere sahiptir:
  * Yayımlanan işlem hatları için yeni bir işlem **hattı** sekmesi.
  * Mevcut bir HDInsight kümesini işlem hedefi olarak ekleme desteği eklendi.

### <a name="azure-machine-learning-sdk-for-python-v0174"></a>Python v 0.1.74 için SDK Azure Machine Learning

+ **Yeni değişiklikler**
  * * Çalışma alanı. compute_targets, datamağazalar, denemeleri, görüntüler, modeller ve *WebServices* , Yöntemler yerine özelliklerdir. Örneğin, Workspace. *compute_targets ()* *çalışma alanı. compute_targets*ile değiştirin.
  * *Çalıştır. Get_context* *Run. get_submitted_run*öğesini kullanımdan kaldırır. İkinci yöntem sonraki sürümlerde kaldırılacak.
  * *Pipelinedata* sınıfı artık datastore_name değil, bir veri deposu nesnesini bir parametre olarak bekliyor. Benzer şekilde, işlem *hattı* default_datastore_name yerine default_datastore kabul eder.

+ **Yeni özellikler**
  * Azure Machine Learning ardışık düzen [örnek Not defteri](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/machine-learning-pipelines/intro-to-pipelines) artık MPI adımlarını kullanır.
  * Jupi Not defterleri için RunDetails pencere öğesi, işlem hattının görselliğini gösterecek şekilde güncelleştirilir.

### <a name="azure-machine-learning-data-prep-sdk-v040"></a>Azure Machine Learning Data Prep SDK v 0.4.0

+ **Yeni özellikler**
  * Tür sayısı veri profiline eklendi
  * Değer sayısı ve histogram artık kullanılabilir
  * Veri profilinde daha fazla yüzdebirlik değeri
  * Ortanca, özette kullanılabilir
  * Python 3,7 artık destekleniyor
  * Veri depolarını içeren bir veri akışını bir DataPrep paketine kaydettiğinizde, veri deposu bilgileri DataPrep paketinin bir parçası olarak kalıcı olur
  * Veri deposuna yazma artık destekleniyor

+ **Hata düzeltildi**
  * 64-bit işaretsiz tamsayı taşmaları artık Linux üzerinde düzgün şekilde işlendi
  * Smart_read düz metin dosyaları için yanlış metin etiketi düzeltildi
  * Dize sütunu türü artık ölçümler görünümünde gösteriliyor
  * Tür sayısı şimdi tek tek bir bağımsız olarak tek bir 1.
  * Yol bir dize olarak sağlandığında Write_to_csv artık başarısız olmaz
  * Değiştirme kullanılırken, "bul" boş bırakılması artık başarısız olmayacaktır

## <a name="2018-10-12"></a>2018-10-12

### <a name="azure-machine-learning-sdk-for-python-v0168"></a>Python v 0.1.68 için SDK Azure Machine Learning

+ **Yeni özellikler**
  * Yeni çalışma alanı oluşturulurken çok kiracılı destek.

+ **Düzeltilen hatalar**
  * Web hizmetini dağıtmaya yönelik olarak pynacl kitaplığı sürümünü Sabitlemeye gerek kalmaz.

### <a name="azure-machine-learning-data-prep-sdk-v030"></a>Azure Machine Learning Data Prep SDK v 0.3.0

+ **Yeni özellikler**
  * Kullanıcıların yürütülecek Python dosyasının yolunda geçmesine izin veren transform_partition_with_file (script_path) yöntemi eklendi

## <a name="2018-10-01"></a>2018-10-01

### <a name="azure-machine-learning-sdk-for-python-v0165"></a>Python v 0.1.65 için SDK Azure Machine Learning
[Sürüm 0.1.65](https://pypi.org/project/azureml-sdk/0.1.65) yeni özellikler, daha fazla belge, hata düzeltmeleri ve daha fazla [örnek Not defteri](https://aka.ms/aml-notebooks)içerir.

Bilinen hatalar ve geçici çözümler hakkında bilgi edinmek için [bilinen sorunlar listesine](resource-known-issues.md) bakın.

+ **Yeni değişiklikler**
  * Workspace. denemeleri, Workspace. modeller, Workspace. compute_targets, Workspace. Images, Workspace. web_services Return Dictionary, daha önce döndürülen liste. Bkz. [azureml. Core. Workspace](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py) API 'si belgeleri.

  * Otomatik Machine Learning, birincil ölçülerden normalleştirilmiş ortalama kare hatası kaldırdı.

+ **Hiper sürücü**
  * Bayeme için çeşitli hiper sürücü hata düzeltmeleri, Get ölçüm çağrılarına yönelik performans iyileştirmeleri.
  * TensorFlow 1,10 yükseltme 1,9
  * Soğuk başlangıç için Docker görüntü iyileştirmesi.
  * İşler artık 0 dışında bir hata koduyla çıksa bile doğru durumu raporlar.
  * SDK 'da RunConfig öznitelik doğrulaması.
  * Hiper sürücü çalıştırma nesnesi, normal çalıştırmaya benzer bir şekilde iptali destekler: herhangi bir parametre geçirmeye gerek yoktur.
  * Dağıtılmış çalıştırmalar ve hiper sürücü çalıştırmaları için açılan değerlerin durumunu korumak üzere pencere öğesi geliştirmeleri.
  * TensorBoard ve diğer günlük dosyaları, parametre sunucusu için düzeltildi.
  * Hizmet tarafında Intel (R) MPı desteği.
  * Batchaı 'de doğrulama sırasında dağıtılmış çalışma düzeltmesi için parametre ayarlamaya bugdüzeltilme.
  * Bağlam Yöneticisi artık birincil örneği tanımlıyor.

+ **Azure portal deneyim**
  * log_table () ve log_row (), çalıştırma ayrıntılarında desteklenir.
  * 1, 2 veya 3 sayısal sütunlu ve isteğe bağlı bir kategorik sütunlu tablolar ve satırlar için otomatik olarak grafikler oluşturun.

+ **Otomatik Machine Learning**
  * Geliştirilmiş hata işleme ve belgeler
  * Çalışma özelliği alma performans sorunlarını düzeltildi.
  * Devam eden çalışma sorunu düzeltildi.
  * Düzeltilen :::no-loc text="ensembling"::: yineleme sorunları.
  * MacOS 'ta sistemin yanıt vermemesine neden olan bir hata düzeltildi.
  * Altörnekleme makrosu ortalama PR/ROC eğrisi özel doğrulama senaryosunda.
  * Ek dizin mantığı kaldırıldı.
  * Get_output API 'sinden filtre kaldırıldı.

+ **Pipelines**
  * Önce bir yürütme çalıştırması gerekmeden bir işlem hattını doğrudan yayımlamak için bir yöntem işlem hattı. Publish () eklendi.
  * Yayımlanmış bir ardışık düzen tarafından oluşturulan işlem hattı çalıştırmalarını getirmek için ardışık düzen eylemsizlik. get_pipeline_runs () yöntemi eklendi.

+ **Proje Brainwave**
  * FPGAs 'de kullanılabilen yeni AI modelleri için destek güncelleştirildi.

### <a name="azure-machine-learning-data-prep-sdk-v020"></a>Azure Machine Learning Data Prep SDK v 0.2.0
[Sürüm 0.2.0](https://pypi.org/project/azureml-dataprep/0.2.0/) , aşağıdaki özellikleri ve hata düzeltmelerini içerir:

+ **Yeni özellikler**
  * Tek yönlü kodlama desteği
  * Quantile dönüştürme desteği

+ **Hata düzeltildi:**
  * Her Tornado sürümü ile birlikte çalışarak, Tornado sürümünüzü indirgemeniz gerekmez
  * Yalnızca en üstteki üç değil tüm değerler için değer sayıları

## <a name="2018-09-public-preview-refresh"></a>2018-09 (genel önizlemeyi Yenile)

Azure Machine Learning yeni, yenilenmiş bir sürümü: Bu sürüm hakkında daha fazla bilgi edinin:https://azure.microsoft.com/blog/what-s-new-in-azure-machine-learning-service/


## <a name="next-steps"></a>Sonraki adımlar

[Azure Machine Learning](overview-what-is-azure-ml.md)’e genel bakışı okuyun.
