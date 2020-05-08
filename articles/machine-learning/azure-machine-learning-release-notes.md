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
ms.openlocfilehash: 40ea1bfe65c963dbbe78c8aca8f5e9e96fd986cb
ms.sourcegitcommit: a6d477eb3cb9faebb15ed1bf7334ed0611c72053
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82928899"
---
# <a name="azure-machine-learning-release-notes"></a>Azure Machine Learning sürüm notları

Bu makalede Azure Machine Learning sürümleri hakkında bilgi edinin.  Tam SDK başvuru içeriği için Azure Machine Learning [**Python başvurusu için ana SDK**](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) sayfasına gidin.

Bilinen hatalar ve geçici çözümler hakkında bilgi edinmek için [bilinen sorunlar listesine](resource-known-issues.md) bakın.

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
  + AmlCompute kümeleri, sağlama sırasında kümede yönetilen bir kimlik ayarlamayı desteklemektedir. Yalnızca sistem tarafından atanan bir kimlik veya Kullanıcı tarafından atanan bir kimlik kullanmak isteyip istemediğinizi belirtin ve ikinci durum durumunda bir IdentityId geçirin. Daha sonra, depolama veya ACR gibi çeşitli kaynaklara erişmek için kimlik denetimi, AmlCompute 'in bugün kullandığı belirteç tabanlı bir yaklaşım yerine, verileri güvenli bir şekilde erişmek için, işlem kimliğinin, verilere güvenli bir şekilde erişmesini sağlamak üzere kurulumunu yapabilirsiniz. Parametreler hakkında daha fazla bilgi için SDK başvurumuza göz atın.
  

+ **Yeni değişiklikler**
  + AmlCompute kümeleri, çalışma tabanlı oluşturma sürecinin iki haftada kullanım dışı bırakıldığımızda bir önizleme özelliği destekliyordu. Her zaman Amlcompute sınıfını kullanarak kalıcı işlem hedefleri oluşturmaya devam edebilirsiniz, ancak Run config içinde işlem hedefi olarak "amlcompute" tanımlayıcısını belirtmeyle ilgili yaklaşım yakın gelecekte desteklenmeyecektir. 

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **azureml-automl-runtime**
    + Sütundaki benzersiz değerlerin sayısını hesaplarken hashable türü desteğini etkinleştirin.
  + **azureml-core**
    + TabularDataset kullanılarak Azure Blob depolama 'dan okurken iyileştirilmiş kararlılık.
    + `grant_workspace_msi` Parametresi için geliştirilmiş belgeler `Datastore.register_azure_blob_store`.
    + Bağımsız değişkenin bir `datastore.upload` `/` veya `\`ile sona erdiği, karşıya yüklenemedik ile ilgili hata düzeltildi. `src_dir`
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
    + İzin verilmeyen target_rolling_window_size değerlere ayarlanamaz 2
  + **azureml-automl-runtime**
    + Yinelenen zaman damgaları bulunduğunda gösterilen hata iletisi geliştirildi.
    + İzin verilmeyen target_rolling_window_size değerlere ayarlanamaz 2.
    + Öteleme imputation hatası düzeltildi. Bu sorun, bir seriyi önemli ölçüde çıkarmak için gereken sayıda gözlemden kaynaklandı. "Mevsimsel hale getirilmiş" veriler, öteleme uzunluğunu belirlemede kısmi bir oto bağıntı işlevini (PACF) hesaplamak için kullanılır.
    + Uygun bir yapılandırmaya göre tahmin görevleri için sütun amacı özelleştirmesi etkin. Tahmin görevleri için sayısal ve kategorik olarak sütun amacı desteklenmektedir.
    + Kaldırma yapılandırmasına göre tahmin görevleri için bırakma sütununun etkin bir şekilde özelleştirilmesi.
    + Uygun yapılandırmaya göre tahmin görevleri için imputation özelleştirmesini etkinleştirdi. Hedef sütun için imputation sabit değer ve ortalama, ortanca, most_frequent ve eğitim verileri için imputation sabit değeri artık desteklenmektedir.
  + **azureml-contrib-pipeline-steps**
    + ParallelRunConfig 'e geçirilecek dize işlem adlarını kabul et
  + **azureml-core**
    +  Ortam nesnesinin bir kopyasını oluşturmak için Environment. Clone (new_name) API 'SI eklendi
    +  Environment. Docker. base_dockerfile FilePath öğesini kabul eder. Bir dosya çözümleyemezse, içerik base_dockerfile ortam özelliği olarak okunacak
    + Kullanıcı ortamında el ile bir değer ayarladığında base_image ve base_dockerfile için birbirini dışlayan değerleri otomatik olarak sıfırlayın. Docker
    + Ortamın Kullanıcı veya AzureML tarafından yönetilip yönetilmediğini belirten RSection öğesine user_managed bayrak eklendi.
    + Veri kümesi: Unicode karakterler içeren veri yolunda sabit veri kümesi indirme hatası.
    + Veri kümesi: Azure Machine Learning Işlem sırasında en düşük disk alanı gereksinimini karşılamak için geliştirilmiş veri kümesi bağlama önbelleği mekanizması, düğümün kullanılamamasına ve işin iptal edilmesine yol açan, bu sayede
    + Veri kümesi: zaman serisi 'e dayalı veri erişimine erişimi hızlandırmak için kullanılan bir Pandas dataframe olarak bir zaman serisi veri kümesine eriştiğinizde zaman serisi sütunu için bir dizin ekleyeceğiz.  Daha önce, dizine zaman damgası sütunuyla aynı ad veriliyordu. Bu, gerçek zaman damgası sütunu olan ve dizine ait olan kullanıcıları karışyordu. Artık, bir sütun olarak kullanılmadığından bu dizine belirli bir ad vermeyiz. 
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
    +  Varsayılan oto ml denemesi zaman aşımı, 6 güne değiştiriliyor.
  + **azureml-train-automl-runtime**
    + Eğitim sonrası işlemleri etrafında ek telemetri eklendi.
    + seyrek bir oto e2e desteği eklendi
  + **azureml-opendatasets**
    + Service Monitor için ek telemetri eklendi.
    + Kararlılığı artırmak için Blobun için frontkapısını etkinleştirin 

## <a name="2020-03-23"></a>2020-03-23

### <a name="azure-machine-learning-sdk-for-python-v120"></a>Python v 1.2.0 için SDK Azure Machine Learning

+ **Yeni değişiklikler**
  + Python 2,7 için bırakma desteği

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **Azure-CLI-ml**
    + CLı 'daki `az ml model/computetarget/service` komutlara "--Subscription-ID" ekler
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
    + İçin `support_multi_line` `Dataset.Tabular.from_delimited_files`yeni bir parametre eklendi: varsayılan olarak (`support_multi_line=False`), tırnak alan değerleri de dahil olmak üzere tüm satır sonları bir kayıt kesmesi olarak yorumlanır. Bu şekilde verilerin okunması, birden fazla CPU çekirdeği üzerinde paralel yürütme için daha hızlı ve iyileştirilir. Ancak, yanlış hizalanmış alan değerleriyle daha fazla kayıt oluşmasına neden olabilir. Bu, ayrılmış dosyaların tırnak `True` içine alınmış satır sonlarını içermesinin bilindiğinde olarak ayarlanmalıdır.
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
  + **Anlamsal sürüm oluşturma 2.0.0**
    + Sürüm 1,1 ' den başlayarak Azure ML Python SDK 'Sı 2.0.0 anlam sürümü oluşturma. [Burada daha fazla bilgi edinin](https://semver.org/). Sonraki tüm sürümler yeni numaralandırma düzenini ve anlamsal sürüm oluşturma sözleşmesini takip edecektir. 

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **Azure-CLI-ml**
    + Tutarlılık için uç nokta CLı komut adını ' az ml Endpoint aks ' iken ' az ml Endpoint gerçek ' olarak değiştirin.
    + kararlı ve deneysel dal CLı için CLı yükleme yönergelerini güncelleştirme
    + Tek örnekli profil oluşturma bir öneri üretmek için düzeltildi ve temel SDK 'da kullanıma sunuldu.
  + **azureml-automl-core**
    + Toplu Işlem modu çıkarımı (birden çok satır bir kez alınıyor), oto ml ONNX modelleri için etkinleştirildi
    + Veri kümelerinde sıklık algılaması geliştirildi, veri yok veya düzensiz veri noktaları içeriyor
    + Baskın sıklığa uyulmayan veri noktalarını kaldırma özelliği eklendi.
    + Oluşturucunun girişi, karşılık gelen sütunlar için imputation seçeneklerini uygulamak üzere seçeneklerin bir listesini alacak şekilde değiştirildi.
    + Hata günlüğü geliştirildi.
  + **azureml-automl-runtime**
    + Eğitim kümesinde mevcut olmayan gren test kümesinde görünmediğinde oluşan hata ile ilgili sorun düzeltildi
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
    + Dağıtılmış AzureML WebServices, şimdi `INFO` günlüğe kaydedilecek. Bu, dağıtılan hizmette `AZUREML_LOG_LEVEL` ortam değişkeni ayarlanarak denetlenebilir.
    + Python SDK ' işlem hatları ' yerine ' API ' uç noktasını kullanmak için bulma hizmetini kullanır.
    + Tüm SDK çağrılarında yeni yollara değiştirin.
    + Yapılan çağrıların ModelManagementService 'e yeni Birleşik bir yapıya yönlendirilmesi değiştirildi.
      + Çalışma alanı güncelleştirme yöntemi genel olarak kullanılabilir hale getirilir.
      + Kullanıcının görüntü derlemesi için işlem güncelleştirmesine izin vermek üzere çalışma alanı güncelleştirme yönteminde image_build_compute parametresi eklendi.
    + Eski profil oluşturma iş akışına kullanımdan kaldırma iletileri eklendi. Sabit profil oluşturma CPU ve bellek sınırları.
    + R işlerini çalıştırmak için ortamın parçası olarak RSection eklendi.
    + Veri kümesinin kaynağına `Dataset.mount` erişilemezse veya veri içermiyorsa hata vermek için öğesine doğrulama eklendi.
    + Bir `--grant-workspace-msi-access` VNET 'In arkasındaki blob kapsayıcısını kaydetmenizi sağlayacak Azure Blob kapsayıcısını kaydettirmek Için VERI deposu CLI için ek bir parametre olarak eklenmiştir.
    + Tek örnekli profil oluşturma bir öneri üretmek için düzeltildi ve temel SDK 'da kullanıma sunuldu.
    + Sorun aks.py _deploy düzeltildi.
    + Sessiz depolama hatalarından kaçınmak için karşıya yüklenen modellerin bütünlüğünü doğrular.
    + Kullanıcı, WebServices için anahtarlar yeniden oluşturulurken kimlik doğrulama anahtarı için bir değer belirtebilir.
    + Büyük harfler veri kümesinin giriş adı olarak kullanılamayan hata düzeltildi.
  + **azureml-varsayılanlar**
    + `azureml-dataprep`, ' nin `azureml-defaults`parçası olarak yüklenir. Veri kümelerini bağlamak için işlem hedeflerine el ile dataprep [sigortası] yüklemesi gerekmez.
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
    + , `AutoMLStep` `azureml-pipeline-steps` Paketini pakete taşıdı. `AutoMLStep` İçinde `azureml-train-automl-runtime`kullanım dışı.
    + Veri kümesi için PythonScriptStep girişi olarak belge örneği eklendi
  + **azureml-tensorboard**
    + , TensorFlow 2,0 desteği için azureml-tensorboard güncelleştirildi
    + Bir Işlem örneğinde özel bir Tensorboard bağlantı noktası kullanırken doğru bağlantı noktası numarasını göster
  + **azureml-train-automl-client**
    + Belirli paketlerin uzak çalışmalerdeki yanlış sürümlere yüklenebildiği bir sorun düzeltildi.
    + Özel uygulanabilirlik yapılandırmasını filtreleyen sorunu geçersiz kılan sabit Korturizationconfig.
  + **azureml-train-automl-runtime**
    + Uzak çalışmadaki sıklık algılamasıyla ilgili sorun düzeltildi
    + , `AutoMLStep` `azureml-pipeline-steps` Paketin içine taşınır. `AutoMLStep` İçinde `azureml-train-automl-runtime`kullanım dışı.
  + **azureml-train-core**
    + PyTorch Estimator 'da PyTorch sürüm 1,4 ' i destekleme
  
## <a name="2020-03-02"></a>2020-03-02

### <a name="azure-machine-learning-sdk-for-python-v112rc0-pre-release"></a>Python v 1.1.2 RC0 için SDK Azure Machine Learning (ön sürüm)

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **azureml-automl-core**
    + Toplu Işlem modu çıkarımı (birden çok satır bir kez alınıyor), oto ml ONNX modelleri için etkinleştirildi
    + Veri kümelerinde sıklık algılaması geliştirildi, veri yok veya düzensiz veri noktaları içeriyor
    + Baskın frequrncy ile uyumlu olmayan veri noktalarını kaldırma özelliği eklendi.
  + **azureml-automl-runtime**
    + Eğitim kümesinde mevcut olmayan gren test kümesinde görünmediğinde oluşan hata ile ilgili sorun düzeltildi
    + Tahmin hizmeti üzerinde Puanlama sırasında y_query gereksinimi kaldırıldı
  + **azureml-contrib-mir**
    + Erişim belirtecini almak için Mwebservice sınıfına işlevsellik ekler
  + **azureml-core**
    + Dağıtılmış AzureML WebServices, şimdi `INFO` günlüğe kaydedilecek. Bu, dağıtılan hizmette `AZUREML_LOG_LEVEL` ortam değişkeni ayarlanarak denetlenebilir.
    + Çalışma alanına kayıtlı `Dataset.get_all` tüm veri kümelerini döndürmek için yineleme ' i düzeltir.
    + Veri kümesi oluşturma API 'lerinin `path` bağımsız değişkenine geçersiz tür geçirildiğinde hata iletisini geliştirebilirsiniz.
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
    + Öğesini `AutoMLStep` öğesine taşıdı `azureml-pipeline-steps package`. `AutoMLStep` İçinde `azureml-train-automl-runtime`kullanım dışı.
  + **azureml-train-automl-client**
    + Belirli paketlerin uzak çalışmalerdeki yanlış sürümlere yüklenebildiği bir sorun düzeltildi.
  + **azureml-train-automl-runtime**
    + Uzak çalışmadaki sıklık algılamasıyla ilgili sorun düzeltildi
    + Öğesini `AutoMLStep` öğesine taşıdı `azureml-pipeline-steps package`. `AutoMLStep` İçinde `azureml-train-automl-runtime`kullanım dışı.
  + **azureml-train-core**
    + Öğesini `AutoMLStep` öğesine taşıdı `azureml-pipeline-steps package`. `AutoMLStep` İçinde `azureml-train-automl-runtime`kullanım dışı.

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
    + Bir `--grant-workspace-msi-access` VNET 'In arkasındaki blob kapsayıcısını kaydetmenize olanak sağlayacak Azure Blob kapsayıcısını kaydettirmek Için VERI deposu CLI için ek bir parametre olarak eklenmiştir
    + Tek örnekli profil oluşturma bir öneri üretmek için düzeltildi ve temel SDK 'da kullanıma sunuldu.
    + Sorun aks.py _deploy düzeltildi
    + Sessiz depolama hatalarından kaçınmak için karşıya yüklenen modellerin bütünlüğünü doğrular.
  + **azureml-interpret**
    + azureml-yorumlama için azureml stili özel durumları eklendi
    + keras modelleri için sabit DeepScoringExplainer serileştirmesi
  + **azureml-pipeline-core**
    + Ardışık düzen toplu işlem Puanlama Not defteri artık ParallelRunStep kullanıyor
  + **azureml-pipeline-steps**
    + , `AutoMLStep` `azureml-pipeline-steps` Paketin içine taşınır. `AutoMLStep` İçinde `azureml-train-automl-runtime`kullanım dışı.
  + **azureml-contrib-pipeline-steps**
    + ParallelRunStep öğesine side_inputs isteğe bağlı parametre eklendi. Bu parametre, kapsayıcıya klasörü bağlamak için kullanılabilir. Şu anda desteklenen türler DataReference ve PipelineData.
  + **azureml-tensorboard**
    + , TensorFlow 2,0 desteği için azureml-tensorboard güncelleştirildi
  + **azureml-train-automl-client**
    + Özel uygulanabilirlik yapılandırmasını filtreleyen sorunu geçersiz kılan sabit Korturizationconfig.
  + **azureml-train-automl-runtime**
    + , `AutoMLStep` `azureml-pipeline-steps` Paketin içine taşınır. `AutoMLStep` İçinde `azureml-train-automl-runtime`kullanım dışı.
  + **azureml-train-core**
    + PyTorch Estimator 'da PyTorch sürüm 1,4 ' i destekleme
  
## <a name="2020-02-04"></a>2020-02-04

### <a name="azure-machine-learning-sdk-for-python-v110rc0-pre-release"></a>Python v 1.1.0 RC0 için SDK Azure Machine Learning (ön sürüm)

+ **Yeni değişiklikler**
  + **Anlamsal sürüm oluşturma 2.0.0**
    + Sürüm 1,1 ' den başlayarak Azure ML Python SDK 'Sı 2.0.0 anlam sürümü oluşturma. [Burada daha fazla bilgi edinin](https://semver.org/). Sonraki tüm sürümler yeni numaralandırma düzenini ve anlamsal sürüm oluşturma sözleşmesini takip edecektir. 
  
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
  + Veri kümesi: veriler ile `on_error` `None`doldurmak `out_of_range_datetime` yerine `to_pandas_dataframe` hata değerleri olduğunda, için iki seçenek ekleyin ve başarısız olur.
  + Çalışma alanı: hassas `hbi_workspace` verilerle daha fazla şifrelemeyi sağlayan ve çalışma alanlarında gelişmiş tanılamayı devre dışı bırakan çalışma alanları Için bayrak eklendi. Ayrıca, çalışma alanınızı sağlarken aboneliğinizde bir Cosmos DB örneği oluşturan bir çalışma alanı oluştururken `cmk_keyvault` ve `resource_cmk_uri` parametrelerini belirterek ilişkili Cosmos DB örneği için kendi anahtarlarınızı getirme desteği ekledik. [Buradan daha fazla bilgi edinin.](https://docs.microsoft.com/azure/machine-learning/concept-enterprise-security#azure-cosmos-db)

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **azureml-automl-runtime**
    + 3.5.4 altındaki Python sürümlerinde oto ml çalıştırıldığında bir TypeError oluşturulmasına neden olan bir gerileme düzeltildi.
  + **azureml-core**
    + İle `./` başlayan göreli `datastore.upload_files` yolun kullanılamadı olması durumunda düzeltilen hata.
    + Tüm Image class codepaths için kullanımdan kaldırma iletileri eklendi
    + Bir pasta bölgesi için Model Yönetimi URL yapımı düzeltildi.
    + Azure Işlevleri için source_dir kullanan modellerin paketlenemediğinde sorun düzeltildi.    
    + Bir görüntüyü AzureML çalışma alanı kapsayıcısı kayıt defterine göndermek için [Environment. build_local ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) seçeneğine bir seçenek eklendi
    + SDK, Azure SYNAPSE üzerinde yeni belirteç kitaplığını yeniden uyumlu bir şekilde kullanacak şekilde güncelleştirildi.
  + **azureml-interpret**
    + İndirilmek üzere hiçbir açıklamalar kullanılamadığı zaman düzeltilmeyen hata düzeltildi. Artık bir özel durum ve eşleşen davranışı başka bir yerde yükseltir.
  + **azureml-pipeline-steps**
    + `DatasetConsumptionConfig`Bir `EstimatorStep`içinde kullanılacaksa, `Estimator` `inputs` `Estimator` parametresi parametresi öğesine geçirilmeyecektir.
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
    + Model ve hizmet nesnelerine CreatedBy bilgileri eklendi. Aracılığıyla <var>erişilebilir. created_by
    + Sabit Containerımage. Run (), Docker kapsayıcısının HTTP bağlantı noktasını doğru ayarlamamıştı.
    + CLI `azureml-dataprep` için `az ml dataset register` isteğe bağlı yap komutu
    + Alternatif bir okuyucuya doğru `TabularDataset.to_pandas_dataframe` bir şekilde geri dönebileceği ve bir uyarının yazdırılacağı bir hata düzeltildi.
  + **azureml-açıkla-model**
    + -Community-yorumlamaya yönelik Shap bağımlılığını yorumlamak için ertele
  + **azureml-pipeline-core**
    + Ardışık düzende adım olarak `NotebookRunnerStep`yerel bir not defteri çalıştırmak için yeni işlem hattı adımı eklendi.
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
  + **azureml-contrib-veri kümesi**
    + Etiketlenmiş veri kümelerinin takılabileceği bir hata düzeltildi.
  + **azureml-core**
    + İçin `Environment.from_existing_conda_environment(name, conda_environment_name)`hata düzeltildi. Kullanıcı, Yerel ortamın tam çoğaltması olan bir ortam örneği oluşturabilir
    + Zaman serisiyle ilgili veri kümesi yöntemleri varsayılan olarak `include_boundary=True` olarak değiştirildi.
  + **azureml-train-automl-client**
    + Çıktıyı göster değeri false olarak ayarlandığında doğrulama sonuçlarının yazdırılmayacağı sorun düzeltildi.


## <a name="2019-11-25"></a>2019-11-25

### <a name="azure-machine-learning-sdk-for-python-v1076"></a>Python v 1.0.76 için SDK Azure Machine Learning

+ **Yeni değişiklikler**
  + Azureml-eğitme-oto ml yükseltme sorunları
    + Azureml-tren-oto ml>= 1.0.76 'ten azureml-tren-oto ml<1.0.76 'e yükseltmek kısmi yüklemelere neden olabilir ve bazı oto ml içeri aktarımlarının başarısız olmasına neden olabilir. Bu sorunu çözmek için konumunda https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/automl_setup.cmdbulunan kurulum betiğini çalıştırabilirsiniz. Ya da doğrudan PIP kullanıyorsanız şunları yapabilirsiniz:
      + "PIP install--Upgrade azureml-tren-oto"
      + "PIP install--Ignore-yüklü azureml-tren-oto ml-Client"
    + ya da yükseltmeden önce eski sürümü kaldırabilirsiniz
      + "PIP kaldırma azureml-eğitme-oto ml"
      + "PIP yüklemesi azureml-eğitme-oto ml"

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **azureml-automl-runtime**
    + Artık İkili Sınıflandırma görevleri için Ortalama skaler ölçümleri hesaplarken, oto 'nin her ikisi de doğru ve yanlış sınıfları hesaba götürecektir.
    + AzureML-oto ml-Core içindeki makine öğrenimi ve eğitim kodu, yeni bir AzureML-oto ml-Runtime paketine taşındı.
  + **azureml-contrib-veri kümesi**
    + Etiketli bir `to_pandas_dataframe` veri kümesinde, indirme seçeneğiyle çağrılırken, artık mevcut dosyaların üzerine yazılıp yazılmayacağını belirtebilirsiniz.
    + Çağrılırken `keep_columns` veya `drop_columns` bir timeseries, etiket veya resim sütunu atıldığı zaman, ilgili yetenekler veri kümesi için de bırakılır.
    + Nesne algılama görevi için pytorch yükleyicisindeki bir sorun düzeltildi.
  + **azureml-contrib-interpret**
    + Contrib-yorumlanan açıklama panosu pencere öğesi, interpret_community ' deki yeni bir başvuruya başvuracak şekilde değiştirildi
    + Yorumlama-Community 'nin güncelleştirilmiş sürümü 0.2.0 olarak
  + **azureml-core**
    + Performansını geliştirir `workspace.datasets`.
    + Kullanıcı adı ve parola kimlik doğrulaması kullanarak Azure SQL veritabanı veri deposunu kaydetme özelliği eklendi
    + Göreli yollardan RunConfigurations yükleme için çözüm.
    + Çağrılırken `keep_columns` veya `drop_columns` bir zaman serisi sütununun bırakılmasının sonucu olarak, ilgili yetenekler veri kümesi için de bırakılır.
  + **azureml-interpret**
    + yorumlama-Community 'nin güncelleştirilmiş sürümü 0.2.0 olarak
  + **azureml-pipeline-steps**
    + Azure Machine Learning ardışık `runconfig_pipeline_params` düzen adımları için desteklenen değerler belgelenmiştir.
  + **azureml-pipeline-core**
    + İşlem hattı komutları için JSON biçiminde çıktıyı indirmek için CLı seçeneği eklendi.
  + **azureml-eğitme-oto ml**
    + AzureML-tren-oto ml 'yi 2 paket, bir istemci paketi AzureML-tren-oto ml-Client ve bir ML eğitim paketi AzureML-tren-oto ml-Runtime olarak Böl
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
    + **azureml-contrib-dataset**
      + Azureml-contrib-DataSet içeri aktardıktan sonra etiketli bir veri `Dataset.Labeled.from_json_lines` kümesi oluşturmak `._Labeled` için yerine öğesini çağırabilirsiniz.
      + Etiketli bir `to_pandas_dataframe` veri kümesinde, indirme seçeneğiyle çağrılırken, artık mevcut dosyaların üzerine yazılıp yazılmayacağını belirtebilirsiniz.
      + Çağrılırken `keep_columns` veya `drop_columns` bir zaman serisi, etiket veya resim sütunu atıldığı zaman, ilgili yetenekler veri kümesi için de bırakılır.
      + Çağrılırken `dataset.to_torchvision()`PyTorch yükleyicisindeki sorunlar düzeltildi.

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
    + Otomatik olarak oluşturulan IML dağıtım dosyasına psutil bir Conda bağımlılığı olarak ekleme.
  + **azureml-contrib-mir**
    + Azure depolama 'da ortadan kaldırma değişikliği, AzureML 'nın başarısız olmasına neden olur.
  + **azureml-core**
    + Azure Işlevleri üzerinde dağıtılan modellerin 500s 'yi üretmesine neden olan bir hatayı düzeltir.
    + Amlignore dosyasının anlık görüntülere uygulanmadığı bir sorun düzeltildi.
    + Belirli bir amlcompute üzerinde çalışan ve sıraya alınmış çalıştırmalar için bir Oluşturucu döndüren yeni bir API amlcompute. get_active_runs eklendi.
    + AKS türleri için MLC 'ye Load Balancer türü eklendi.
    + Artifacts_client içindeki run.py ve download_artifacts_from_prefix download_files append_prefix bool parametresi eklendi. Bu bayrak, kaynak FilePath öğesini seçmeli olarak düzleştirmek için kullanılır, bu nedenle yalnızca dosya veya klasör adı output_directory eklenir
    + Veri kümesi kullanımıyla ilgili `run_config.yml` kaldırma sorununu düzeltir.
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

Konumundaki [https://ml.azure.com](https://ml.azure.com) işbirliğine dayalı çalışma alanı giriş sayfası, Azure Machine Learning Studio (Önizleme) olarak geliştirilmiş ve yeniden markalı.

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

R için Azure Machine Learning SDK, Python SDK `reticulate` 'sına bağlamak için paketini kullanır. Doğrudan Python 'a bağlayarak, R için SDK, seçtiğiniz herhangi bir R ortamından Python SDK 'sında uygulanan temel nesnelere ve yöntemlere erişmenizi sağlar.

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
    + Azure Machine Learning Işlem sırasında yerel SSD 'nizin disk şifrelemesi için Önizleme desteği yayınlıyoruz. Aboneliğinizi bu özelliği kullanmak için daha sonra listelenen bir teknik destek bileti sağlayın.
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
    + 2 yıl ve 1 ay gibi karmaşık sıklıklara yönelik destek eklendi. -Sıklık tespit edilemez hata iletisi eklendi.
    + Model dağıtım hatasını çözümlemek için otomatik olarak oluşturulan Conda env öğesine azureml-varsayılanlar ekleyin
    + Azure Machine Learning ardışık düzeninde ara verilerin tablo veri kümesine dönüştürülmesine ve içinde `AutoMLStep`kullanılmasına izin verin.
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
    + Desteklemeyen bir AmlCompute işlem hedefi için uyarı eklendi
    + Azureml-contrib-gbdt paketine LightGMB Estimator eklendi
  + [**azureml-core**](https://docs.microsoft.com/python/api/azureml-core)
    + CLı artık model paketlemeyi destekliyor.
    + Kullanım dışı veri kümesi API 'Leri için kullanımdan kaldırma uyarısı ekleyin. Bkz https://aka.ms/tabular-dataset. veri kümesi API 'si değişiklik bildirimi.
    + Veri [`Dataset.get_by_id`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset%28class%29#get-by-id-workspace--id-) kümesi kayıtlıysa kayıt adını ve sürümünü döndürecek şekilde değiştirin.
    + Bağımsız değişken olarak ScriptRunConfig ile birlikte bulunan bir hatayı düzelttikten sonra, deneme çalıştırması göndermek için tekrar tekrar kullanılamaz.
    + Çalıştırma sırasında alınan veri kümeleri izlenir ve çalıştırma ayrıntıları sayfasında ya da çalışma tamamlandıktan sonra öğesini çağırarak [`run.get_details()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29#get-details--) görünür.
    + Azure Machine Learning ardışık düzeninde ara verilerin tablo veri kümesine dönüştürülmesine ve içinde [`AutoMLStep`](/python/api/azureml-train-automl-runtime/azureml.train.automl.runtime.automlstep)kullanılmasına izin verin.
    + Bir ınenceconfig örneği olmadan desteklenen modellerin (ONNX, scikit-öğren ve TensorFlow) dağıtımı ve paketlenmesi için destek eklendi.
    + SDK ve CLı 'de hizmet dağıtımı (acı ve AKS) için üzerine yazma bayrağı eklendi. Sağlanmışsa, zaten adlı hizmet varsa mevcut hizmetin üzerine yazar. Hizmet yoksa, yeni hizmet oluşturacaktır.
    +  Modeller, Onnx ve TensorFlow olmak üzere iki yeni çerçeve ile kaydedilebilir. Model kaydı, model için örnek giriş verilerini, örnek çıkış verilerini ve kaynak yapılandırmasını kabul eder.
    + MySQL için Azure veritabanı için yeni veri deposu eklendi. DataTransferStep içinde MySQL için Azure veritabanı 'nın Azure Machine Learning işlem hatları 'nda kullanılması için örnek eklenmiştir.
    + Etiketleri çalıştırmalardan kaldırmak için denemeleri eklenen işlevden etiket ekleme ve kaldırma işlevselliği eklendi
    + SDK ve CLı 'de hizmet dağıtımı (acı ve AKS) için üzerine yazma bayrağı eklendi. Sağlanmışsa, zaten adlı hizmet varsa mevcut hizmetin üzerine yazar. Hizmet yoksa, yeni hizmet oluşturacaktır.
  + [**azureml-datadrift**](https://docs.microsoft.com/python/api/azureml-datadrift)
    + Öğesinden `azureml-contrib-datadrift` öğesine taşındı`azureml-datadrift`
    + Değişikliklerini ve diğer istatistiksel ölçüler için zaman serisi veri kümelerini izleme desteği eklendi
    + Yeni yöntemler `create_from_model()` ve `create_from_dataset()` [`DataDriftDetector`](https://docs.microsoft.com/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector(class)) sınıfı. `create()` Yöntem kullanım dışı olacaktır.
    + Azure Machine Learning Studio 'daki Python ve Kullanıcı arabirimindeki görselleştirmelerin ayarlamaları.
    + Günlük veri kümesi izleyicilerine ek olarak haftalık ve aylık izleme zamanlamasını destekler.
    + Veri kümesi izleyicilerinin geçmiş verilerini çözümlemek için veri izleme ölçümlerinin geri doldurmasını destekler.
    + Çeşitli hata düzeltmeleri
  + [**azureml-pipeline-core**](https://docs.microsoft.com/python/api/azureml-pipeline-core)
    + işlem hattı `yaml` dosyasından bir Azure Machine Learning işlem hattı çalıştırması göndermek için azureml-dataprep artık gerekli değildir.
  + [**azureml-eğitme-oto ml**](/python/api/azureml-train-automl-runtime/)
    + Model dağıtım hatasını çözümlemek için otomatik olarak oluşturulan Conda env öğesine azureml-varsayılanlar ekleyin
    + Oto ml uzak eğitimi artık, çıkarım için eğitim env 'nin yeniden kullanımına izin vermek üzere azureml-varsayılanlar içerir.
  + **azureml-train-core**
    + Tahmin aracı 'da [`PyTorch`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch) pytorch 1,3 desteği eklendi

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
    + Psutil bir bağımlılığı olarak eklendi ve `automl` psutil, amlcompute içindeki bir Conda bağımlılığı olarak eklenmiştir.
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
    + `PythonScriptStep`, `EstimatorStep`Ve `HyperDriveStep` Azure Machine Learning işlem `FileDataset` hattında `TabularDataset` giriş olarak tükeme ve olarak
    + `FileDataset.mount` Çok sayıda dosya içeren klasörler için performansı geliştirilmiştir
    + Azure Machine Learning işlem hattındaki [filedataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.filedataset) ve [Tabulardataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset) ' i [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep), [estimatorstep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.estimatorstep)ve [HyperDriveStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.hyperdrivestep) öğesine giriş olarak tükeiyor.
    + FileDataset performansı. [Mount ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.filedataset#mount-mount-point-none----kwargs-) çok sayıda dosya içeren klasörler için geliştirildi
    + Çalışma ayrıntılarında bilinen hata önerilerine URL eklendi.
    + Çalıştırılmakta olan bir hata düzeltildi. bir çalıştırma çok fazla alt öğe içeriyorsa isteklerin başarısız olduğu get_metrics
    + [Çalıştırılmakta](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run#get-metrics-name-none--recursive-false--run-type-none--populate-false-) olan bir hata düzeltildi. bir çalıştırma çok fazla alt öğe içeriyorsa isteklerin başarısız olduğu get_metrics
    + Arcadia kümesinde kimlik doğrulaması için destek eklendi.
    + Deneme nesnesi oluşturma, çalışma geçmişi izlemenin Azure Machine Learning çalışma alanında denemeyi alır veya oluşturur. Deneme KIMLIĞI ve arşivlenen süre, oluşturma sırasında denemeler nesnesine doldurulur. Örnek: deneme = deneme (çalışma alanı, "yeni deneme") experiment_id = experiment.id Archive () ve yeniden etkinleştirme (), denemeyi UX içinde gösterilmeye veya bir List denemeleri çağrısında varsayılan olarak döndürülen bir deneyde çağrılabilir işlevlerdir. Arşivlenmiş bir deneyle aynı ada sahip yeni bir deneme oluşturulursa, yeni bir ad geçirerek yeniden etkinleştirme sırasında arşivlenmiş denemeyi yeniden adlandırabilirsiniz. Yalnızca belirli bir ada sahip bir etkin deneme olabilir. Örnek: experiment1 = deneme (çalışma alanı, "etkin deneme") experiment1. Archive () # arşivlenmiş ile aynı ada sahip yeni etkin denemeler oluşturun. experiment2. = Denemeler (çalışma alanı, "etkin deneme") experiment1. yeniden etkinleştirme (new_name = "önceki etkin deneme") deneme sırasında statik yöntem listesi () bir ad filtresi ve ViewType filtresi alabilir. ViewType değerleri şunlardır: "ACTIVE_ONLY", "ARCHIVED_ONLY" ve "ALL" örnek: archived_experiments = denemeler. List (Workspace, view_type = "ARCHIVED_ONLY") all_first_experiments = denemeler. List (çalışma alanı, ad = "Ilk deneme", view_type = "tümü")
    + Model dağıtımı ve hizmet güncelleştirmesi için ortamı kullanma desteği
  + **azureml-datadrift**
    + DataDriftDector sınıfının Show özniteliği, isteğe bağlı ' with_details ' bağımsız değişkenini desteklemez. Show özniteliği yalnızca veri DRFT katsayısını ve özellik sütunlarının veri kayması katkısını sunar.
    + Datadriftalgılayıcı özniteliği ' get_output ' davranış değişiklikleri:
      + Giriş parametresi start_time, end_time zorunlu yerine isteğe bağlıdır;
      + Aynı çağırma içindeki belirli bir run_id giriş özel start_time ve/veya end_time, karşılıklı olarak dışlandığı için değer hata özel durumuna neden olur
      + Girişe özgü start_time ve/veya end_time göre yalnızca zamanlanan çalıştırmaların sonuçları döndürülür;
      + ' Daily_latest_only ' parametresi kullanım dışıdır.
    + Veri kümesi tabanlı veri Drçıkışları almayı destekler.
  + **azureml-açıkla-model**
    + AzureML daha sonra geri uyumluluk için eski paketi tutmak üzere, AzureML-açıkla-model paketini AzureML-yorumlama olarak yeniden adlandırır
    + ExplanationClient `automl` adresinden indirme sırasında varsayılan olarak gerileme yerine sınıflandırma görevine ayarlanmış ham açıklamalar ile düzeltilen hata
    + Kullanarak doğrudan oluşturulacak `ScoringExplainer` desteği ekle`MimicWrapper`
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
    + [Datadriftalgılayıcı](https://docs.microsoft.com/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector.datadriftdetector) sınıfının Show özniteliği, ' with_details ' isteğe bağlı bağımsız değişkenini desteklemez. Show özniteliği yalnızca veri DRFT katsayısını ve özellik sütunlarının veri kayması katkısını sunar.
    + Datadriftalgılayıcı işlevi [get_output]https://docs.microsoft.com/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector.datadriftdetector#get-output-start-time-none--end-time-none--run-id-none-) davranış değişiklikleri:
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
+ Öngörülerinizi görselleştirmenize ve karşılaştırmanıza olanak tanıyan Basitleştirilmiş ve performanslı deneme ayrıntıları sayfaları
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
    + İşlem hattı olarak tahmin için `timeseries` Prophet eklendi (yalnızca Önizleme)

  + **azureml-contrib-datadrift**
    + Paketler, azureml-contrib-datadrift ile azureml-datadrift arasında yeniden konumlandırılır. `contrib` paket sonraki bir sürümde kaldırılacak

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
    + Gönderilen `run.get_details()['datasets']` çalıştırma ile ilişkili veri kümelerini almak için tanıtın
    + JSON satırları `Dataset.Tabular.from_json_lines_files` dosyalarından bir TabularDataset oluşturmak için API ekleyin. TabularDataset 'teki JSON satırları dosyalarında bu tablo verileri hakkında bilgi edinmek için lütfen belgeler için https://aka.ms/azureml-data ziyaret edin.
    + Supported_vmsizes () işlevine ek VM boyut alanları (işletim sistemi diski, GPU sayısı) eklendi
    + Çalışmayı, özel ve genel IP 'yi, bağlantı noktasını vb. göstermek için list_nodes () işlevine ek alanlar eklendi.
    + Küme sağlama sırasında yeni bir alan belirtme--remotelogin_port_public_access, küme oluşturma sırasında SSH bağlantı noktasını açık veya kapalı bırakmak istediğinize bağlı olarak, etkin veya devre dışı olarak ayarlanabilir. Bunu belirtmezseniz, küme bir sanal ağın içine dağıtıp dağıtdığınıza bağlı olarak bağlantı noktasını açar veya kapatır.
  + **azureml-açıkla-model**
  + **[azureml-core](https://docs.microsoft.com/python/api/azureml-core/azureml.core)**
    + Model nesnesi aracılığıyla depolamada SAS URL 'sini depolama sırasında modele alma özelliği eklendi. Ex: model. [get_sas_url ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model#get-sas-urls--)
    + Çalıştırmayı tanıtın. gönderilen çalışma ile ilişkili veri kümelerini almak için [' DataSet '] [get_details](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29#get-details--)
    + API `Dataset.Tabular`ekleyin. JSON satırları dosyalarından bir TabularDataset oluşturmak için [from_json_lines_files ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory#from-json-lines-files-path--validate-true--include-path-false--set-column-types-none--partition-format-none-) . TabularDataset 'teki JSON satırları dosyalarında bu tablo verileri hakkında bilgi edinmek için lütfen belgeler için https://aka.ms/azureml-data ziyaret edin.
    + [Supported_vmsizes ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#supported-vmsizes-workspace--location-none-) IŞLEVINE ek VM boyut alanları (Işletim sistemi diski, GPU sayısı) eklendi
    + Çalışmayı, özel ve genel IP 'yi, bağlantı noktasını vb. göstermek için [list_nodes ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#list-nodes--) işlevine ek alanlar eklendi.
    + [Küme oluşturma](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#provisioning-configuration-vm-size-----vm-priority--dedicated---min-nodes-0--max-nodes-none--idle-seconds-before-scaledown-none--admin-username-none--admin-user-password-none--admin-user-ssh-key-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--tags-none--description-none--remote-login-port-public-access--notspecified--) `--remotelogin_port_public_access` sırasında SSH bağlantı noktasını açık veya kapalı bırakmak istediğinize bağlı olarak, etkin veya devre dışı olarak ayarlanabilir bir yeni alan belirtme özelliği. Bunu belirtmezseniz, küme bir sanal ağın içine dağıtıp dağıtdığınıza bağlı olarak bağlantı noktasını açar veya kapatır.
  + **azureml-açıkla-model**
    + Sınıflandırma senaryosunda açıklama çıkışları için geliştirilmiş belgeler.
    + Değerlendirme örnekleri için açıklamada tahmini y değerlerini karşıya yükleme özelliği eklendi. Daha kullanışlı görselleştirmelerin kilidini açar.
    + Temeldeki MimicExplainer almayı etkinleştirmek için Mimkıwrapper 'a Açıklama özelliği eklendi.
  + **azureml-pipeline-core**
    + Modülün açıklaması, ModuleVersion ve ModuleStep için Not defteri eklendi
  + **azureml-pipeline-steps**
    + AML işlem hattı aracılığıyla R betiği çalıştırmayı desteklemek için RScriptStep eklendi.
    + AzureBatchStep içinde ayrıştırma, "SubscriptionID parametresi için atama belirtilmemiş" hata iletisine neden olan sabit meta veri parametreleri.
  + **azureml-eğitme-oto ml**
    + Desteklenen training_data, validation_data, label_column_name, veri girişi biçimi olarak weight_column_name
    + Explain_model () ve retrieve_model_explanations () için kullanımdan kaldırma iletisi eklendi
  + **[azureml-pipeline-core](https://docs.microsoft.com/python/api/azureml-pipeline-core)**
    + [Module](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.module(class)), [Moduleversion](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.moduleversion) ve [modulestep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.modulestep)'i anlatmak için bir [Not defteri](https://aka.ms/pl-modulestep) eklendi.
  + **[azureml-pipeline-steps](https://docs.microsoft.com/python/api/azureml-pipeline-steps)**
    + AML işlem hattı aracılığıyla R betiği çalıştırmayı desteklemek için [Rscriptstep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.rscriptstep) eklendi.
    + [AzureBatchStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.azurebatchstep) içinde ayrıştırma, "SubscriptionID parametresi için atama belirtilmemiş" hata iletisine neden olan sabit meta veri parametreleri.
  + **[azureml-eğitme-oto ml](/python/api/azureml-train-automl-runtime/)**
    + Training_data, validation_data, label_column_name weight_column_name veri girişi biçimi olarak desteklenir.
    + [Explain_model ()](/python/api/azureml-train-automl-runtime/azureml.train.automl.runtime.automlexplainer#explain-model-fitted-model--x-train--x-test--best-run-none--features-none--y-train-none----kwargs-) ve [retrieve_model_explanations ()](/python/api/azureml-train-automl-runtime/azureml.train.automl.runtime.automlexplainer#retrieve-model-explanation-child-run-)için kullanımdan kaldırma iletisi eklendi.


## <a name="2019-09-16"></a>2019-09-16

### <a name="azure-machine-learning-sdk-for-python-v1062"></a>Python v 1.0.62 için SDK Azure Machine Learning

+ **Yeni özellikler**
  + TabularDataset üzerinde `timeseries` nitelik tanıtılmıştır. Bu nitelik, bir zaman aralığı veya en son veriler arasında tüm verileri almak gibi bir TabularDataset veri kümesi verilerinde kolay zaman damgası filtrelemesini mümkün bir şekilde sunar. TabularDataset üzerinde `timeseries` nitelik hakkında bilgi edinmek için lütfen belgeler veya https://aka.ms/azureml-data https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datasets-tutorial/timeseries-datasets/tabular-timeseries-dataset-filtering.ipynb örnek bir not defteri için ziyaret edin.
  + TabularDataset ve FileDataset ile eğitime etkin. Lütfen örnek https://aka.ms/dataset-tutorial bir not defteri için ziyaret edin.

  + **azureml-train-core**
      + Pytorch tahmin aracı 'da eklendi `Nccl` ve desteklenir `Gloo`

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **azureml-automl-core**
    + ' Lag_length ' ve LaggingTransformer oto ml ayarı kullanım dışı bırakıldı.
    + Veri akışı biçiminde belirtilmişse giriş verilerinin doğru doğrulanması düzeltildi
    + JSON grafiğini oluşturmak ve yapıtlara yüklemek için fit_pipeline. Kopyala değiştirildi.
    + `userrun` Kullanılarak `Cytoscape`grafik işlendi.
  + **azureml-core**
    + ADB kodundaki özel durum işlemeyi yeniden ziyaret ediyor ve yeni hata işleme uyarınca değişiklik yapma
    + Not defteri VM 'Leri için otomatik MSI kimlik doğrulaması eklendi.
    + Başarısız denemeler nedeniyle bozuk veya boş modellerin karşıya yüklenebildiği hatayı düzeltir.
    + `DataReference` Mod değiştiğinde `DataReference` adın değiştiği hata düzeltildi (örn., veya `as_upload` `as_download` `as_mount`çağrılırken).
    + Ve `mount_point` `FileDataset.download`için `target_path` yapın ve `FileDataset.mount` için isteğe bağlıdır.
    + Zaman damgası sütununun bulunamadığını belirten özel durum, zaman içinde ilgili API 'SI, hassas zaman damgası sütunu atanmamış veya atanan zaman damgası sütunları bırakılınca oluşturulur.
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
    + Oto ml 'yi `automl` açıklayan ve oto ml modelleri için özel ml açıklamalarını desteklemeye yönelik daha yeni yollardan, oto ml 'yi açıklayan ve SDK ile tümleşik ham açıklama desteğini açıklayan, oto ml 'Den ortak API 'ler tanıtılmıştır.
    + Uzak eğitim ortamlarından azureml varsayılan değerleri kaldırılıyor.
    + Azure Databricks kod yolu üzerinde bir AzureFileCacheStore One tabanlı dosya olan varsayılan önbellek depolama konumu, oto ml için bir tane olarak değiştirildi.
    + Veri akışı biçiminde belirtilmişse giriş verilerinin doğru doğrulanması düzeltildi
  + **azureml-train-core**
    + Kullanımdan kaldırılması source_directory_data_store geri döndürüldü.
    + Azureml yüklü paket sürümlerini geçersiz kılma özelliği eklendi.
    + Estimators 'daki `environment_definition` parametreye dockerfile desteği eklendi.
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

Bu yayın sırasında, aşağıdaki tarayıcılar desteklenir: Chrome, Firefox, Safari ve Microsoft Edge Preview.

**Bilinen sorunlar:**

1. "Bir sorun oluştu!" hata görürseniz tarayıcınızı yenileyin. Dağıtım devam ederken öbek dosyaları yüklenirken hata oluştu.

1. Not defterlerindeki ve dosyalardaki dosya silinemez veya yeniden adlandırılamaz. Genel Önizleme sırasında güncelleştirme dosyası işlemlerini gerçekleştirmek için, dizüstü bilgisayar VM 'de Jupyter Kullanıcı arabirimini veya terminali kullanabilirsiniz. Bağlı bir ağ dosya sistemi olduğundan, Not defteri VM 'de yaptığınız tüm değişiklikler not defteri çalışma alanında hemen yansıtılır.

1. Not defteri sanal makinesine SSH eklemek için:
   1. VM kurulumu sırasında oluşturulan SSH anahtarlarını bulun. Ya da Azure Machine Learning çalışma alanındaki anahtarları bulun > Işlem sekmesini açın > listedeki Not defteri sanal makinesini bulun > bu özellikleri açın: iletişim kutusundan anahtarları kopyalayın.
   1. Bu ortak ve özel SSH anahtarlarını yerel makinenize aktarın.
   1. Bunları not defteri VM 'sine SSH için kullanın.

## <a name="2019-09-03"></a>2019-09-03
### <a name="azure-machine-learning-sdk-for-python-v1060"></a>Python v 1.0.60 için SDK Azure Machine Learning

+ **Yeni özellikler**
  + Veri mağazalarınızın veya genel URL 'lerdeki tek veya birden çok dosyaya başvuran FileDataset kullanıma sunulmuştur. Dosyalar herhangi bir biçimde olabilir. Dosya veri kümesi size dosyaları indirme veya işlem için bağlama yeteneği sağlar. FileDataset hakkında bilgi edinmek için lütfen adresini https://aka.ms/file-datasetziyaret edin.
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
    + Bir sözlüğü `TabularDataset` ve `FileDataset` kayıt adına göre anahtarlanan nesneler döndüren DataSet. get_all (Workspace) öğesini tanıtın.

    ```py
    workspace = Workspace.from_config()
    all_datasets = Dataset.get_all(workspace)
    mydata = all_datasets['my-data']
    ```

    + Ve `parition_format` `Dataset.Tabular.from_parquet.files`için `Dataset.Tabular.from_delimited_files` bağımsız değişken olarak tanıtın. Her veri yolunun bölüm bilgileri, belirtilen biçime göre sütunlarda ayıklanır. ' {column_name} ' dize sütunu oluşturuyor ve ' {column_name: yyyy/aa/gg/HH/mm/ss} ' DateTime sütunu oluşturuyor; burada ' yyyy ', ' AA ', ' gg ', ' HH ', ' mm ' ve ' ss ', DateTime türü için Year, month, Day, Hour, Minute ve Second 'ı ayıklamak için kullanılır. Partition_format, dosya yolunun sonuna kadar ilk bölüm anahtarının konumundan başlamalıdır. Örneğin, '. ' yolu veriliyor. /USA/2019/01/01/data.csv '; bölüm ülke ve zamana göre olduğunda partition_format = '/{Country}/{PartitionDate: yyyy/aa/gg}/Data. csv ', ' USA ' değeri ve ' 2019-01-01 ' değerli ' PartitionDate ' DateTime sütunuyla ' Country ' dize sütununu oluşturur.
        ```py
        workspace = Workspace.from_config()
        all_datasets = Dataset.get_all(workspace)
        mydata = all_datasets['my-data']
        ```

    + Ve `partition_format` `Dataset.Tabular.from_parquet.files`için `Dataset.Tabular.from_delimited_files` bağımsız değişken olarak tanıtın. Her veri yolunun bölüm bilgileri, belirtilen biçime göre sütunlarda ayıklanır. ' {column_name} ' dize sütunu oluşturuyor ve ' {column_name: yyyy/aa/gg/HH/mm/ss} ' DateTime sütunu oluşturuyor; burada ' yyyy ', ' AA ', ' gg ', ' HH ', ' mm ' ve ' ss ', DateTime türü için Year, month, Day, Hour, Minute ve Second 'ı ayıklamak için kullanılır. Partition_format, dosya yolunun sonuna kadar ilk bölüm anahtarının konumundan başlamalıdır. Örneğin, '. ' yolu veriliyor. /USA/2019/01/01/data.csv '; bölüm ülke ve zamana göre olduğunda partition_format = '/{Country}/{PartitionDate: yyyy/aa/gg}/Data. csv ', ' USA ' değeri ve ' 2019-01-01 ' değerli ' PartitionDate ' DateTime sütunuyla ' Country ' dize sütununu oluşturur.
    + `to_csv_files`ve `to_parquet_files` yöntemleri öğesine `TabularDataset`eklenmiştir. Bu yöntemler, verileri belirtilen biçimdeki `TabularDataset` dosyalara dönüştürerek `FileDataset` , ve arasında dönüştürmeyi etkinleştirir.
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
    + Hem genel `get_raw_explanation(feature_maps)` hem de yerel açıklama nesnelerine yöntem ekler.
    + Desteklenen sürüm altında, yazdırılmış uyarı ile birlikte lightgbm için sürüm denetimi eklendi
    + Açıklamaları toplu işleme alırken İyileştirilmiş bellek kullanımı
    + Oto ml modelleri artık oto Mlexceptions döndürüyor
  + **azureml-pipeline-core**
    + PipelineDrafts oluşturma, güncelleştirme ve kullanma desteği eklendi-kesilebilir işlem hattı tanımlarını sürdürmek ve bunları çalıştırmak için etkileşimli olarak kullanmak için kullanılabilir
  + **azureml-eğitme-oto ml**
    + Uzak Python çalışma zamanı ortamında BERT/XLNet ' i etkinleştirmek için gereken GPU :::no-loc text="cuda"::: özellikli pytorch v 1.1.0, araç seti 9,0, pytorch-dönüştürücüler 'ın belirli sürümlerini yüklemek için özelliği oluşturuldu.
  + **azureml-train-core**
    + Bazı hiper parametre alanı tanımı hatalarının sunucu tarafı yerine doğrudan SDK 'da erken hatası.

### <a name="azure-machine-learning-data-prep-sdk-v1114"></a>Azure Machine Learning Data Prep SDK v 1.1.14
+ **Hata düzeltmeleri ve geliştirmeleri**
  + Ham yol ve kimlik bilgileri kullanılarak ADLS/ADLSGen2 'a yazma özelliği etkinleştirildi.
  + İçin `include_path=True` `read_parquet`çalışmamasından kaynaklanan bir hata düzeltildi.
  + " `to_pandas_dataframe()` Geçersiz özellik değeri: hostsecret" özel durumu nedeniyle oluşan hata düzeltildi.
  + Spark modundaki DBFS üzerinde dosyaların okunmamasına neden olan bir hata düzeltildi.

## <a name="2019-08-19"></a>2019-08-19

### <a name="azure-machine-learning-sdk-for-python-v1057"></a>Python v 1.0.57 için SDK Azure Machine Learning
+ **Yeni özellikler**
  + AutomatedML tarafından tüketilebilmesi için etkinleştirildi `TabularDataset` . Hakkında `TabularDataset`daha fazla bilgi edinmek için lütfen https://aka.ms/azureml/howto/createdatasetsadresini ziyaret edin.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **Oto ml-Client-Core-NativeClient**
    + Hata düzeltildi, eğitim ve/veya doğrulama etiketleri (y ve y_valid), Pandas dataframe biçiminde sağlandığı halde bir sayısal tuş y dizisi olarak değil, ortaya çıktı.
    + Yalnızca verileri ve `AutoMLBaseSettings` nesneyi gerektirecek `RawDataContext` şekilde oluşturmak için arabirimi güncelleştirildi.
    +  Oto ml kullanıcılarına tahmin sırasında yeterince uzun olmayan eğitim serisini bırakmaya izin verin. -Oto ml kullanıcılarına, tahmin sırasında eğitim kümesinde mevcut olmayan test kümesinden graıns 'yi bırakması izin verin.
  + **Azure-CLI-ml**
    + Artık, Microsoft tarafından oluşturulan ve müşteri sertifikası için AKS kümesinde dağıtılan Puanlama uç noktası için TLS/SSL sertifikasını güncelleştirebilirsiniz.
  + **azureml-automl-core**
    + Eksik etiketlere sahip satırların düzgün şekilde kaldırılmadığı, oto ml 'deki bir sorun düzeltildi.
    + Oto ml 'de geliştirilmiş hata günlüğü; Tam hata iletileri artık her zaman günlük dosyasına yazılır.
    + Oto, `azureml-defaults` `azureml-explain-model`, ve `azureml-dataprep`dahil olmak üzere paket sabitlenmesini güncelleştirmiştir. Oto, paket uyuşmazlıkları ( `azureml-train-automl` paket hariç) üzerinde artık uyarı vermez.
    + CV 'in eşit olmadığı `timeseries` bir sorun düzeltildi.
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
    + Özel özel Python paketlerinin `whl`çalışma alanına yüklenmesini sağlayan ortam. add_private_pip_wheel () eklendi ve ortamı derlemek/denemek için güvenli bir şekilde kullanma.
    + Artık, Microsoft tarafından oluşturulan ve müşteri sertifikası için AKS kümesinde dağıtılan Puanlama uç noktası için TLS/SSL sertifikasını güncelleştirebilirsiniz.
  + **azureml-açıkla-model**
    + Karşıya yükleme ile ilgili açıklamaları bir model KIMLIĞI eklemek için parametresi eklendi.
    + Bellekteki `is_raw` açıklamaları etiketleme ve karşıya yükleme eklendi.
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
    + Şimdi, yeni yapılandırma parametrelerinin değişiklikleri `automl` veya eklemeleri üzerinde herhangi bir sorun yaşamamak için config üzerinden arka uca geçiş yapar.
    + Oto Data Guardkıl artık genel önizlemeye sunuldu. Kullanıcı eğitim sonrasında (sınıflandırma/gerileme görevleri için) bir veri Guardcıl raporu görür ve ayrıca SDK API aracılığıyla buna erişebilir.
  + **azureml-train-core**
    + PyTorch Estimator 'da Torch 1,2 desteği eklenmiştir.
  + **azureml-widgets**
    + Sınıflandırma eğitimi için geliştirilmiş karışıklık matrisi grafikleri.

### <a name="azure-machine-learning-data-prep-sdk-v1112"></a>Azure Machine Learning Data Prep SDK v 1.1.12
+ **Yeni özellikler**
  + Dizelerin listesi artık `read_*` yöntemlere giriş olarak geçirilebilir.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + , Spark 'ta `read_parquet` çalışırken performansı önemli ölçüde iyileştirildi.
  + Belirsiz tarih biçimleri içeren `column_type_builder` tek bir sütun olması durumunda başarısız olan bir sorun düzeltildi.

### <a name="azure-portal"></a>Azure portal
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
    + Eğer ön işleme `Timeseries` bayrağının doğru olması durumunda oluşan sorunlar düzeltildi.
    + Bazı tahmin verileri doğrulama hata iletileri daha fazla işlem yapılabilir.
    + , Özellikle işlem kaynakları arasında yer aldığı veri kümelerinin düşürümine ve/veya geç yüklenmesine göre, oto 'nin azaltılmış bellek tüketimi
  + **azureml-contrib-açıkla-model**
    + Kullanıcının model türü için varsayılan otomatik çıkarım mantığını geçersiz kılmasına izin vermek üzere explainers 'e model_task bayrağı eklendi
    + Pencere öğesi değişiklikleri: yalnızca genel `contrib`Özellik önem derecesine `nbextension` sahip (ör. permütasyon), ile otomatik olarak yüklenir, daha fazla yükleme/etkinleştirme desteği açıklaması yoktur
    + Pano değişiklikleri:-Box çizimleri ve keman çizimleri, Özet `beeswarm` sayfa üzerine çizmenin yanı sıra, ' top `beeswarm` -k ' kaydırıcısının daha hızlı bir şekilde rerendering
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
    + Blob_cache_timeout parametresi eklendi `Datastore.register_azure_blob_container`.
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
    + İlişkili `docstrings` dosyalar için eklendi.
    + Belgeler, ve için `max_cores_per_iteration` daha uygun dile güncelleştirildi`max_concurrent_iterations`
    + Uzak çalışmalarla ilgili tahmin görevlerinin günlüğe kaydedilmesi, çalışma başarısız olursa, artık kullanıcı kapsamlı bir hata iletisiyle sunulmaktadır.
    + Ardışık Düzen `automlstep` not defterinden get_Data kaldırıldı.
    + ' De `dataprep` `automlstep`destek başlatıldı.

### <a name="azure-machine-learning-data-prep-sdk-v1110"></a>Azure Machine Learning Data Prep SDK v 1.1.10

+ **Yeni özellikler**
  + Artık belirli bir sütunda belirli Denetçiler (örn. histogram, dağılım çizimi vb.) yürütmeyi isteyebilirsiniz.
  + İçin `append_columns`bir paralel hale getirmek bağımsız değişkeni eklendi. Doğru ise, veriler belleğe yüklenir ancak yürütme paralel olarak çalıştırılır; Yanlış ise, yürütme akış, ancak tek iş parçacıklı olur.

## <a name="2019-07-23"></a>2019-07-23

### <a name="azure-machine-learning-sdk-for-python-v1053"></a>Python v 1.0.53 için SDK Azure Machine Learning

+ **Yeni özellikler**
  + Otomatik Machine Learning artık uzak işlem hedefinde ONNX modellerini eğitimini destekliyor
  + Azure Machine Learning, artık önceki bir çalıştırma, kontrol noktası veya model dosyalarından eğitim verme olanağı sağlar.
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
    + Bir hatayı düzeltme, sonuçta elde edilen ve sonuç olarak büyümekte olan bir hata düzeltiliyor.
    + Tahmin görevleri için CV bölmeler genelinde bazı özellikler yeniden kullanımını etkinleştirin. Bu, zaman aşımı ve sıralı pencereler gibi pahalı bir n_cross_validations faktörü için, kurulum 'un çalışma süresini kabaca hızlandırır.
    + Zaman Pandas desteklenen zaman aralığı dışında bir sorun adresleme. Şimdi PD 'den küçükse bir DataException tetikliyoruz. Timestamp. Min veya PD 'den büyük. Timestamp. Max
    + Tahmin şimdi, eğiteseler tren ve test kümelerinde farklı sıklıklara izin veriyor. Örneğin, "Ocak ayında başlayan üç aylık dönem" ve "Ekim ayında başlayan" üç aylık olarak hizalanabilir.
    + "Parameters" özelliği TimeSeriesTransformer öğesine eklendi.
    + Eski özel durum sınıflarını kaldırın.
    + Tahmin görevlerinde, `target_lags` parametresi artık tek bir tamsayı değeri ya da tamsayılar listesini kabul eder. Tamsayı sağlanmışsa, yalnızca bir gecikme oluşturulur. Bir liste sağlanmışsa, lags 'nin benzersiz değerleri alınacaktır. target_lags = [1, 2, 2, 4] bir, 2 ve 4 dönem için lags oluşturacak.
    + Dönüşümden sonra sütun türlerini kaybetme hakkındaki hatayı düzeltir (hata bağlandı);
    + ' `model.forecast(X, y_query)`De, y_query begin (#459519) sırasında hiçbirini içeren bir nesne türü olmasını sağlar.
    + `automl` Çıkışa beklenen değerleri ekleyin
  + **azureml-contrib-datadrift**
    +  Örnek Not defteri geliştirmeleri; azureml, verileri zenginleştirmeden azureml-contrib-OPENDATASET ve performans iyileştirmeleri yerine azureml-OPENDATASET 'e geçiş
  + **azureml-contrib-açıkla-model**
    + Azureml-contrib-açıkla-model paketindeki ham özellik için LIME açıklama için sabit dönüşümler bağımsız değişkeni
    + AzureML-contrib-açıkla-model paketi için Image açıklama içindeki görüntü açıklamaları segmentations eklendi
    + LimeExplainer için SciPy seyrek desteği ekleme
    + DecisionTreeExplainableModel `batch_size` yürütme süresini geliştirmek üzere `include_local=False`toplu işlerle ilgili genel açıklamalar için açıklama taklit 'e eklendi
  + **azureml-contrib-featureengineering**
    + Set_featurizer_timeseries_params () çağrısı için çözüm: dict değer türü değişikliği ve null denetimi-koratizer `timeseries` için Not defteri ekleme
    + 1.2.0 sürümüne NimbusML bağımlılığını güncelleştir (geçerli en son).
  + **azureml-core**
    + AzureML türünde DBFS veri depoları ekleme özelliği eklendi
    + İle `target_path` başlatılmışsa boş bir klasörün oluşturulduğu veri deposu yüklemesine hata düzeltildi`/`
    + Serviceprincıpalauthentication içindeki sorun düzeltildi `deepcopy` .
    + CLı 'ya "az ml Environment Show" ve "az ml Environment List" komutları eklendi.
    + Ortamlar artık önceden oluşturulmuş bir base_image alternatif olarak bir base_dockerfile belirtmeyi desteklemektedir.
    + Kullanılmayan RunConfiguration ayarı auto_prepare_environment kullanım dışı olarak işaretlendi.
    + Model açıklaması artık kayıt sonrasında güncelleştirilebilen
    + Bugdüzeltmesini: model ve resim silme işlemi, yukarı akış bağımlılığı nedeniyle silme başarısız olursa, bunlara bağlı yukarı akış nesnelerini alma hakkında daha fazla bilgi sağlamaktadır.
    + Bazı ortamlar için bir çalışma alanı oluşturulurken gerçekleşen dağıtımlar için boş süre yazdırılan hata düzeltildi.
    + İyileştirilmiş çalışma alanı hata özel durumları oluşturma. Kullanıcılar "çalışma alanı oluşturulamıyor. Bulunamıyor... " ileti olarak ve bunun yerine gerçek oluşturma hatası ' nı görürsünüz.
    + AKS WebServices içinde belirteç kimlik doğrulaması desteği ekleyin.
    + `Webservice` Nesnelere yöntem ekleyin. `get_token()`
    + Machine Learning veri kümelerini yönetmek için CLı desteği eklendi.
    + `Datastore.register_azure_blob_container`Artık isteğe bağlı olarak `blob_cache_timeout` , bu veri deposu için önbellek süre sonunu etkinleştirmek üzere blobsigortası 'nin Mount parametrelerini yapılandıran bir değer (saniye cinsinden) alır. Varsayılan değer zaman aşımı değildir, yani bir blob okunarak iş bitene kadar yerel önbellekte kalır. Çoğu iş bu ayarı tercih eder, ancak bazı işlerin, düğümlerine sığacak kadar büyük bir veri kümesinden daha fazla veri okuması gerekir. Bu işler için, bu parametrenin ayarlanması başarılı olur. Bu parametreyi ayarlarken dikkatli olmanız gerekir: değeri çok düşük olarak ayarlamak, bir dönem içinde kullanılan verilerin yeniden kullanılmadan önce süresinin dolmasına neden olabilir. Bu, tüm okumaların, eğitim sürelerini olumsuz yönde etkileyen yerel önbellek yerine blob depolamadan (ağ) yapılacağı anlamına gelir.
    + Model açıklaması artık kayıt sonrasında düzgün şekilde güncelleştirilebilen olabilir
    + Model ve resim silme artık, silinmesine bağlı yukarı akış nesneleri hakkında daha fazla bilgi sağlar ve bu da silmenin başarısız olmasına neden olur
    + Azureml. mlflow kullanarak uzak çalıştırmaların kaynak kullanımını geliştirme.
  + **azureml-açıkla-model**
    + Azureml-contrib-açıkla-model paketindeki ham özellik için LIME açıklama için sabit dönüşümler bağımsız değişkeni
    + LimeExplainer için SciPy seyrek desteği ekleme
    + Doğrusal modelleri açıklayan şekil doğrusal açıklama sarmalayıcı ve tablo açıklama 'e başka bir düzey eklendi
    + Model kitaplığı 'nda açıklama benzeklik için, include_local = false olduğunda sabit hata, seyrek veri girişi için
    + `automl` çıkışa beklenen değerleri ekleyin
    + ham Özellik önem derecesi almak için dönüşümler bağımsız değişkeni sağlandığında düzeltilen permütasyon özelliği önem derecesi
    + DecisionTreeExplainableModel `batch_size` yürütme süresini geliştirmek üzere `include_local=False`toplu işlerle ilgili genel açıklamalar için açıklama taklit 'e eklendi
    + Model explainability kitaplığı için, Pandas dataframe girişinin tahmin için gerekli olduğu sabit kara Box explainers
    + Bazen içinde float içeren `explanation.expected_values` bir liste yerine float döndüren bir hata düzeltildi.
  + **azureml-mlflow**
    + Mlflow 'un performansını geliştirme. set_experiment (experiment_name)
    + Mlflow tracking_uri için ınteractiveloginauthentication kullanımda olan hatayı düzeltir
    + Azureml. mlflow kullanarak uzak çalıştırmaların kaynak kullanımını geliştirme.
    + Azureml-mlflow paketinin belgelerini geliştirme
    + Mlflow. log_artifacts ("my_dir") içindeki yapıtları "my_dir/<yapıt-Paths>" yerine "<yapıt-Paths>" altına kaydedebilecekleri düzeltme eki hatası
  + **azureml-opendatasets**
    + Yeni eklenen bellek sorunu nedeniyle eski sürümlere (<0.14.0) sabitleme `pyarrow` `opendatasets`
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
    + `automl` çıkışa beklenen değerleri ekleyin
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
    + Son olarak, sonuç olarak elde edilen puanlar sabit kaldığı halde ortaya çıkacak şekilde büyümekte olan, Ensebir seçim yordamında sorun düzeltildi.
    + Whitelist_models sorun düzeltildi ve blacklist_models ayarları, oto Mlstep.
    + Azure ML işlem hatları bağlamında, oto ml kullanıldığında ön işleme kullanımını önleyen sorun düzeltildi.
  + **azureml-opendatasets**
    + Azureml-contrib-OPENDATASET 'leri azureml-OPENDATASET 'e taşındı.
    + Azure Machine Learning çalışma alanına kaydedilecek ve AML veri kümesi özelliklerinden sorunsuz bir şekilde yararlanan açık veri kümesi sınıflarının kullanımına izin verilir.
    + SPARK olmayan sürümde gelişmiş Noaaısdhava durumu performansı önemli ölçüde geliştirildi.
  + **azureml-açıkla-model**
    + Yorumlenebilirlik nesneleri için çevrimiçi belgeler güncelleştirildi.
    + Model `batch_size` explainability kitaplığı için DecisionTreeExplainableModel `include_local=False`yürütme süresini geliştirmek üzere toplu işlerle ilgili genel açıklamalar için açıklama benzemesine eklenmiştir.
    + Bazen içinde float içeren `explanation.expected_values` bir liste yerine float döndüren sorun düzeltildi.
    + Açıklama modeli kitaplığındaki taklit `automl` açıklama için çıktıya beklenen değerler eklendi.
    + Ham Özellik önem derecesi almak için dönüşümler bağımsız değişkeni sağlandığında düzeltilen permütasyon özelliği önem derecesi.
  + **azureml-core**
    + AzureML türünde DBFS veri depoları ekleme özelliği eklendi.
    + İle `target_path` `/`başlatılmışsa boş bir klasörün oluşturulduğu veri deposu yükleme sorunu düzeltildi.
    + İki veri kümesinin etkinleştirilmiş karşılaştırması.
    + Model ve resim silme işlemi şimdi, yukarı akış bağımlılığı nedeniyle silme başarısız olursa, bunlara bağlı yukarı akış nesnelerini alma hakkında daha fazla bilgi sağlamaktadır.
    + Auto_prepare_environment kullanılmamış RunConfiguration ayarı kullanım dışı bırakıldı.
  + **azureml-mlflow**
    + Azureml. mlflow kullanan uzak çalıştırmaların geliştirilmiş kaynak kullanımı.
    + Azureml-mlflow paketinin belgeleri geliştirilmiştir.
    + Mlflow. log_artifacts ("my_dir"), yapıtları "yapıt-Paths" yerine "my_dir/Artifact-Paths" altına kaydedebileceği sorun düzeltildi.
  + **azureml-pipeline-core**
    + Tüm işlem hattı adımları için parametre hash_paths kullanım dışıdır ve gelecekte kaldırılacaktır. Source_directory varsayılan olarak, karma hale getirilir (. amlignore veya. gitignore ' de listelenen dosyalar hariç)
    + İşlem türüne özgü modül kullanımının kilidini açmak için, RunConfiguration tümleştirmesi ve diğer değişikliklere hazırlanmak üzere işlem türü özel modülleri desteklemek için modülü ve ModuleStep 'i sürekli iyileştirme.
  + **azureml-pipeline-steps**
    + AzureBatchStep: giriş/çıkışlarla ilgili olarak Iyileştirilmiş belgeler.
    + AzureBatchStep: delete_batch_job_after_finish varsayılan değeri true olarak değiştirildi.
  + **azureml-train-core**
    + Dizeler artık otomatik hiper parametre ayarlama için işlem hedefi olarak kabul edilir.
    + Auto_prepare_environment kullanılmamış RunConfiguration ayarı kullanım dışı bırakıldı.
    + Kullanım dışı `conda_dependencies_file_path` bırakılan `pip_requirements_file_path` parametreler ve `conda_dependencies_file` `pip_requirements_file` sırasıyla.
  + **azureml-opendatasets**
    + SPARK olmayan sürümde önemli performansı önemli ölçüde zenginleştirin.

### <a name="azure-machine-learning-data-prep-sdk-v118"></a>Azure Machine Learning Data Prep SDK v 1.1.8

+ **Yeni özellikler**
 + Veri akışı nesneleri artık, bir dizi kayıt üreten şekilde tekrarlandırılır. İçin `Dataflow.to_record_iterator`belgelerine bakın.
  + Veri akışı nesneleri artık, bir dizi kayıt üreten şekilde tekrarlandırılır. İçin `Dataflow.to_record_iterator`belgelerine bakın.

+ **Hata düzeltmeleri ve geliştirmeleri**
 + DataPrep SDK 'sının sağlamlık artışı artırılmıştır.
 + Dize olmayan sütun dizinleriyle Pandas Dataframe 'ler için geliştirilmiş işlem.
 + Veri kümelerinde performansı `to_pandas_dataframe` geliştirildi.
 + Çok düğümlü bir ortamda çalıştırıldığında veri kümelerinin Spark yürütmesinin başarısız olduğu bir hata düzeltildi.
  + DataPrep SDK 'sının sağlamlık artışı artırılmıştır.
  + Dize olmayan sütun dizinleriyle Pandas Dataframe 'ler için geliştirilmiş işlem.
  + Veri kümelerinde performansı `to_pandas_dataframe` geliştirildi.
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
  + Model. Register artık birden çok bireysel dosyanın `child_paths` parametresini kullanan tek bir model olarak kaydedilmesini desteklemektedir.

+ **Önizleme özellikleri**
    + Puanlama explainers artık daha güvenilir serileştirme ve seri durumdan çıkarma için Conda ve PIP bilgilerini isteğe bağlı olarak kaydedebilir.
    + Otomatik Özellik Seçicisi için hata düzeltilme.
    + Yeni uygulama tarafından kullanıma sunulan yeni API 'ye ve düzeltme eki uygulanmış hatalara mlflow. azureml. build_image güncelleştirildi.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Azureml `paramiko` -Core ' dan bağımlılığı kaldırıldı. Eski işlem hedefi iliştirme yöntemlerine yönelik kullanımdan kaldırma uyarıları eklendi.
  + Çalıştırmanın performansını geliştirir. create_children
  + İkili sınıflandırıcıyla benzeme açıklama, şekil değerlerini ölçeklendirmek için öğretmen olasılığı kullanıldığında olasılıkların sırasını düzeltir.
  + Otomatik makine öğrenimi için geliştirilmiş hata işleme ve ileti.
  + Otomatik makine öğrenimi için yineleme zaman aşımı sorunu düzeltildi.
  + Otomatik makine öğrenimi için zaman serisi dönüştürme performansı geliştirildi.

## <a name="2019-06-24"></a>2019-06-24

### <a name="azure-machine-learning-data-prep-sdk-v116"></a>Azure Machine Learning Data Prep SDK v 1.1.6

+ **Yeni özellikler**
  + Top values (`SummaryFunction.TOPVALUES`) ve Bottom values (`SummaryFunction.BOTTOMVALUES`) için özet işlevleri eklendi.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Performansı önemli ölçüde iyileştirildi `read_pandas_dataframe`.
  + İkili dosyaları işaret eden bir veri `get_profile()` akışının başarısız olmasına neden olan bir hata düzeltildi.
  + Telemetri `set_diagnostics_collection()` koleksiyonunun programlı bir şekilde etkinleştirilmesi/devre dışı bırakılmasına olanak tanımak için sunulur.
  + Davranışı değiştirildi `get_profile()`. NaN değerleri artık, Pandas davranışıyla hizalanan min, ortalama, STD ve Sum için yok sayılır.


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
  + Model sınıfı artık azureml. Core (`from azureml.core import Model`) öğesinden daha uygun bir şekilde alınabilir.
  + İşlem hattı adımlarında, `hash_path` parametre artık kullanım dışıdır. Yeni davranış,. amlignore veya. gitignore içinde listelenen dosyalar hariç karma tamamlanma source_directory.
  + İşlem hattı paketlerinde, çeşitli `get_all` ve `get_all_*` yöntemleri sırasıyla `list` ve `list_*`için kullanım dışı bırakılmıştır.
  + azureml. Core. get_run artık özgün çalışma türünü döndürmeden önce sınıfların içeri aktarılmasını gerektirmez.
  + Bazı WebService güncelleştirmesine yapılan çağrıların bir güncelleştirmeyi tetikleyemediği bir sorun düzeltildi.
  + AKS WebServices üzerinde Puanlama zaman aşımı 5 MS ile 300000ms arasında olmalıdır. Puanlama istekleri için izin verilen en büyük scoring_timeout_ms, 1 dakikadan 5 dakika arasında tamponlanır.
  + LocalWebservice nesnelerinin artık ve `scoring_uri` `swagger_uri` özellikleri vardır.
  + Taşınan çıkış çıkışları dizin oluşturma ve çıkış Dizin yüklemesi Kullanıcı işleminin dışına çıktı. Her Kullanıcı işleminde çalıştırılacak çalışma geçmişi SDK 'sını etkinleştirdi. Bu, dağıtılmış eğitim çalıştırmaları tarafından karşılaşılan bazı eşitleme sorunlarını çözmelidir.
  + Kullanıcı işlemi adından yazılan azureml günlüğünün adı artık işlem adını (yalnızca dağıtılmış eğitim için) ve PID 'yi içerir.

### <a name="azure-machine-learning-data-prep-sdk-v115"></a>Azure Machine Learning Data Prep SDK v 1.1.5

+ **Hata düzeltmeleri ve geliştirmeleri**
  + 2 basamaklı yıl biçimine sahip yorumlanan tarih saat değerleri için geçerli yıl aralığı Windows 'un yayını ile eşleşecek şekilde güncelleştirilmiştir. Aralık 1930-2029 ' den 1950-2049 ' e değiştirilmiştir.
  + Bir dosya ve ayarı `handleQuotedLineBreaks=True` `\r` okurken yeni bir satır olarak kabul edilir.
  + Bazı durumlarda başarısız olmasına neden `read_pandas_dataframe` olan bir hata düzeltildi.
  + Performansı geliştirildi `get_profile`.
  + Geliştirilmiş hata iletileri.

## <a name="2019-05-28"></a>2019-05-28

### <a name="azure-machine-learning-data-prep-sdk-v114"></a>Azure Machine Learning Data Prep SDK v 1.1.4

+ **Yeni özellikler**
  + Artık DateTime değerlerini yeni sütunlara ayıklamak ve ayrıştırmak için aşağıdaki ifade dili işlevlerini kullanabilirsiniz.
    + `RegEx.extract_record()`DateTime öğelerini yeni bir sütuna ayıklar.
    + `create_datetime()`ayrı DateTime öğelerinden DateTime nesneleri oluşturur.
  + ' I `get_profile()`çağırırken, değerlerin yakın olduğunu açıkça göstermek için, bu quantile sütunlarının (EST.) olarak etiketlenmiş olduğunu görebilirsiniz.
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

### <a name="azure-portal"></a>Azure portal

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
+ Azure Machine Learning çalışma alanının genel bakış sayfasından config. json dosyasını indirme özelliği eklendi.
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
+ [Quickly spin up a preconfigured notebook VM](tutorial-1st-experiment-sdk-setup.md) Azure Machine Learning SDK ve ilgili paketlerin en son sürümüne sahip olan önceden yapılandırılmış bir not defteri VM 'sini hızlıca çalıştırın.
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

+ Performans iyileştirmesi için dinamik ekleme :::no-loc text="featurizers"::: özelliğini etkinleştirme özelliği. Yeni :::no-loc text="featurizers":::: iş katıştırıtları, kanıt ağırlığı, hedef kodlamalar, metin hedefi kodlama, küme uzaklığı
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

Aynı [`PipelineEndpoint`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline_endpoint.pipelineendpoint?view=azure-ml-py) uç noktayı koruyarak yayımlanmış bir işlem hattının yeni bir sürümünü eklemeyi tanıtmıştı.

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
  + Artık `to_upper`  ifade dilinde `to_lower`  işlevleri kullanabilirsiniz.
  + Artık bir veri profilindeki her bir sütunun benzersiz değerlerinin sayısını görebilirsiniz.
  + Yaygın olarak kullanılan bazı okuyucu adımları için artık `infer_column_types` bağımsız değişkeni geçirebilirsiniz. Olarak `True`ayarlanırsa, veri hazırlığı sütun türlerini algılamaya ve otomatik olarak dönüştürmeye çalışır.
    + `inference_arguments`Artık kullanım dışıdır.
  + Artık çağırabilirsiniz `Dataflow.shape`.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + `keep_columns` Artık ek bir isteğe bağlı bağımsız `validate_column_exists`değişkeni kabul eder, bu da sonucun `keep_columns` herhangi bir sütun içerecekse olup olmadığını denetler.
  + Tüm okuyucu adımları (dosyadan okunan) artık ek bir isteğe bağlı bağımsız değişkeni `verify_exists`kabul eder.
  + Pandas dataframe 'ten okuma ve veri Profilleri alma performansı geliştirildi.
  + Bir veri akışından tek bir adımı Dilimleme tek bir dizin ile başarısız olan bir hata düzeltildi.

## <a name="2019-04-15"></a>2019-04-15

### <a name="azure-portal"></a>Azure portal
  + Artık var olan bir uzak işlem kümesinde çalışan bir betiği yeniden gönderebilirsiniz.
  + Artık, işlem hatları sekmesinde yeni parametrelerle yayınlanmış bir işlem hattı çalıştırabilirsiniz.
  + Çalışma ayrıntıları artık yeni bir Snapshot dosya görüntüleyicisini destekliyor. Belirli bir çalıştırma gönderdiğinizde dizinin anlık görüntüsünü görüntüleyebilirsiniz. Çalıştırmaya başlamak için gönderilen Not defterini de indirebilirsiniz.
  + Artık Azure portal üst çalıştırmaları iptal edebilirsiniz.

## <a name="2019-04-08"></a>2019-04-08

### <a name="azure-machine-learning-sdk-for-python-v1023"></a>Python v 1.0.23 için SDK Azure Machine Learning

+ **Yeni özellikler**
  + Azure Machine Learning SDK artık Python 3,7 ' i desteklemektedir.
  + Azure Machine Learning DNN estimators artık yerleşik çoklu sürüm desteği sağlıyor. Örneğin, `TensorFlow`  tahmin aracı şimdi bir `framework_version` parametreyi kabul eder ve kullanıcılar ' 1,10 ' veya ' 1,12 ' sürümünü belirtebilir. Geçerli SDK sürümleriniz tarafından desteklenen sürümlerin bir listesi için, istenen Framework sınıfını `get_supported_versions()` (örneğin, `TensorFlow.get_supported_versions()`) çağırın.
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
  + Veri akışının yürütüldüğünde `verify_has_data()` kayıt oluşturulup oluşturulmayacağını denetlemek Için bir veri akışı üzerinde arama yapabilirsiniz.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Artık sayısal sütun profilleri için bir histogram içinde kullanılacak bölme sayısını belirtebilirsiniz.
  + `read_pandas_dataframe` Dönüştürme artık veri çerçevesinin dize veya bayt türünde sütun adlarına sahip olmasını gerektirir.
  + Bir hata `fill_nulls` düzeltildi, ancak sütun eksik ise değerler doğru doldurulmadı.

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

  + Azure Machine Learning artık popüler DNN Framework Chainer için birinci sınıf destek sağlar. [`Chainer`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) Sınıf kullanıcıları, Chainer modellerini kolayca eğitebilir ve dağıtabilir.
    + [Chaeylemsizlik ile dağıtılmış eğitime nasıl çalıştırılacağını](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/chainer/training/distributed-chainer/distributed-chainer.ipynb) öğrenin
    + [Hyperparameter ayarlamayı hiper sürücü kullanarak Chainer ile çalıştırma](https://github.com/Azure/MachineLearningNotebooks/blob/b881f78e4658b4e102a72b78dbd2129c24506980/how-to-use-azureml/ml-frameworks/chainer/deployment/train-hyperparameter-tune-deploy-with-chainer/train-hyperparameter-tune-deploy-with-chainer.ipynb) hakkında bilgi edinin
  + Azure Machine Learning işlem hatları, veri deposu değişikliklerine göre bir işlem hattı çalıştırmasını tetikleyebilme özelliği eklendi. İşlem hattı [zamanlama Not defteri](https://aka.ms/pl-schedule) , bu özelliği göstermek için güncelleştirilir.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Source_directory_data_store özelliğini, [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep?view=azure-ml-py)Için sağlanan [runconfigurations](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.runconfiguration?view=azure-ml-py) 'lar üzerinde istenen veri deposuna (BLOB depolama gibi) ayarlamaya yönelik Azure Machine Learning işlem hatlarında destek ekledik. Varsayılan olarak, çok sayıda adım eşzamanlı olarak yürütüldüğünde sorunları azaltma sorunlarını ortadan kaldırarak Azure dosya deposunu, yedekleme veri deposu olarak kullanır.

### <a name="azure-portal"></a>Azure portal

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
  +  DataTranferStep, Azure SQL Server ve PostgreSQL için Azure veritabanı ([Not defteri](https://aka.ms/pl-data-trans)) ile çalışacak şekilde güncelleştirildi.

+ **Değişikliklerine**
  + Kullanım `PublishedPipeline.get_published_pipeline` dışı bırakıldı `PublishedPipeline.get`.
  + Kullanım `Schedule.get_schedule` dışı bırakıldı `Schedule.get`.

### <a name="azure-machine-learning-data-prep-sdk-v1012"></a>Azure Machine Learning Data Prep SDK v 1.0.12

+ **Yeni özellikler**
  + Veri hazırlama artık, veri deposu kullanarak bir Azure SQL veritabanından okumayı destekler.

+ **Değişikliklerine**
  + Büyük veriler üzerinde belirli işlemlerin bellek performansı geliştirilmiştir.
  + `read_pandas_dataframe()`Şimdi belirtilmesini `temp_folder` gerektirir.
  + `name` Özelliği `ColumnProfile` kullanım dışı bırakıldı- `column_name` bunun yerine kullanım dışıdır.

## <a name="2019-01-28"></a>2019-01-28

### <a name="azure-machine-learning-sdk-for-python-v1010"></a>Python v 1.0.10 için SDK Azure Machine Learning

+ **Değişiklikler**:
  + Azure ML SDK artık bağımlılık olarak Azure-CLI paketlerine sahip değildir. Özellikle, Azure-CLI-Core ve Azure-CLI-profile bağımlılıkları azureml-Core ' dan kaldırılmıştır. Bunlar, değişiklikleri etkileyen Kullanıcı ' lardır:
      + "Az Login" ve ardından azureml-SDK kullanıyorsanız, SDK tarayıcı veya cihaz kodu günlüğünü bir kez daha zaman kullanacaktır. "Az Login" tarafından oluşturulan herhangi bir kimlik bilgisi durumunu kullanmaz.
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
  + `to_bool`işlev artık eşleşmeyen değerlerin hata değerlerine dönüştürülmesine izin veriyor. Bu, ve `to_bool` `set_column_types`için yeni varsayılan uyumsuzluk davranışıdır, ancak önceki varsayılan davranış eşleşmeyen değerleri yanlış olarak dönüştürmelidir.
  + Çağrılırken `to_pandas_dataframe`, sayısal sütunlardaki null/eksik değerleri Nan olarak yorumlamak için yeni bir seçenek vardır.
  + Tür tutarlılığı sağlamak ve erken yük devretmek için bazı ifadelerin dönüş türünü denetleme özelliği eklendi.
  + Artık bir sütundaki değerleri `parse_json` JSON nesneleri olarak ayrıştırmaya ve bunları birden çok sütuna genişletmeye çağrı yapabilirsiniz.

+ **Hata düzeltmeleri**
  + Python 3.5.2 `set_column_types` 'te kilitlenen bir hata düzeltildi.
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
  * `SummaryFunction.N`, olarak `SummaryFunction.Count`yeniden adlandırıldı.

+ **Hata düzeltmeleri**
  * Uzak çalışmalardaki veri depolarından okuma ve yazma yaparken en son AML çalıştırma belirtecini kullanın. Daha önce, AML çalıştırma belirteci Python 'da güncelleştirilirse, Data Prep çalışma zamanı güncelleştirilmiş AML çalıştırma belirteciyle güncellenmez.
  * Ek daha net hata iletileri
  * Spark serileştirme kullandığında `Kryo` to_spark_dataframe () artık kilitlenmeyecektir
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
  * *azureml. Core. COMPUTE. AmlCompute* aşağıdaki sınıfları kullanımdan kaldırır- *azureml. Core. COMPUTE. Batchaıute* ve *azureml. Core. COMPUTE. dsvmcompute*. Sonraki sürümlerde ikinci sınıf kaldırılacak. AmlCompute sınıfının şimdi daha kolay bir tanımı vardır ve yalnızca bir vm_size ve max_nodes ihtiyacı vardır ve bir iş gönderildiğinde kümenizi otomatik olarak 0 ' dan max_nodes ölçeklendirecektir. [Örnek Not defterleriniz](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training) bu bilgilerle güncelleştirildi ve size kullanım örnekleri vermelidir. Bu basitleştirme ve çok sayıda daha heyecan verici özelliğin daha sonraki bir sürümde gelmesini umuyoruz!

### <a name="azure-machine-learning-data-prep-sdk-v051"></a>Azure Machine Learning Data Prep SDK v 0.5.1

[Başvuru belgelerini](https://aka.ms/data-prep-sdk)okuyarak veri hazırlığı SDK 'sı hakkında daha fazla bilgi edinin.
+ **Yeni özellikler**
   * DataPrep paketlerini yürütmek ve bir veri kümesi veya veri akışı için veri profilini görüntülemek üzere yeni bir DataPrep CLı oluşturuldu
   * Kullanılabilirliği artırmak için SetColumnType API yeniden tasarlandı
   * Auto_read_file smart_read_file yeniden adlandırıldı
   * Artık veri profilinde skew ve basıklığını içerir
   * , Bağlı örneklemeyle örnek oluşturabilir
   * CSV dosyaları içeren ZIP dosyalarından okuyabilir
   * Veri kümeleri satır temelinde rastgele bölme ile bölebilir (örneğin, test-tren kümelerine)
   * ' İ çağırarak bir veri akışından veya bir veri profilinden tüm sütun veri türlerini alabilir.`.dtypes`
   * ' İ çağırarak bir veri akışından veya bir veri profilinden satır sayısı alabilir.`.row_count`

+ **Hata düzeltmeleri**
   * Uzun-Double dönüşümü düzeltildi
   * Herhangi bir sütundan sonra düzeltilen onaylama
   * Belirsizlik Gruplandırmayla ilgili bir sorun düzeltildi ve bazı durumlarda grupları algılayamayacağı
   * Çok sütunlu sıralama sırasına göre sabit sıralama işlevi
   * Onları nasıl `pandas` işleyeceğinize benzer sabit ve/veya ifadeler
   * DBFS yolundan düzeltilen okuma
   * Hata iletileri daha fazla anlaşılır hale getirilir
   * Artık bir AML belirteci kullanılarak uzak işlem hedefinde okurken artık başarısız olmaz
   * Artık Linux DSVM 'de başarısız oluyor
   * Dize olmayan değerler dize yüklemelerinde artık kilitlenmeyen
   * Artık veri akışı doğru bir şekilde başarısız olduğunda onaylama hatalarını işler
   * Artık Azure Databricks üzerinde dbutils bağlı depolama konumlarını destekliyor

## <a name="2018-11-05"></a>2018-11-05

### <a name="azure-portal"></a>Azure portal
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
  * MAC OS 'ta hata ile asılı eğitim hatası.
  * Altörnekleme makrosu ortalama PR/ROC eğrisi özel doğrulama senaryosunda.
  * Ek dizin mantığı kaldırıldı.
  * Get_output API 'sinden filtre kaldırıldı.

+ **İşlem hatları**
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
