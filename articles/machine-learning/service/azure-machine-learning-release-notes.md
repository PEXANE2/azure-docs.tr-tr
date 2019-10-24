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
ms.date: 08/19/2019
ms.custom: seodec18
ms.openlocfilehash: afad2648ec73b02d4e06ad55f850a518d2488f68
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2019
ms.locfileid: "72756049"
---
# <a name="azure-machine-learning-release-notes"></a>Azure Machine Learning sürüm notları

Bu makalede Azure Machine Learning sürümleri hakkında bilgi edinin.  Tam SDK başvuru içeriği için Azure Machine Learning [**Python başvurusu için ana SDK**](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) sayfasına gidin. 

Bilinen hatalar ve geçici çözümler hakkında bilgi edinmek için [bilinen sorunlar listesine](resource-known-issues.md) bakın.

## <a name="2019-10-21"></a>2019-10-21

### <a name="visual-interface-preview"></a>Görsel Arabirim (Önizleme)

+ Azure Machine Learning görsel arabirimi (Önizleme), [Azure Machine Learning işlem hatları](concept-ml-pipelines.md)üzerinde çalışacak şekilde kaldırıldı. Görsel arabirimde yazılan işlem hatları (daha önce denemeleri olarak bilinir) artık çekirdek Azure Machine Learning deneyimiyle tamamen tümleşiktir.
  + SDK varlıklarıyla Birleşik yönetim deneyimi
  + Görsel arabirim modelleri, işlem hatları ve uç noktalar için sürüm oluşturma ve izleme 
  + Yeniden tasarlanan kullanıcı arabirimi
  + Toplu iş ınırm dağıtımı eklendi
  + Inlekele işlem hedefleri için Azure Kubernetes hizmeti (AKS) desteği eklendi
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
  + **azureml-oto ml-çekirdek**
    + Her çalıştırma için bilgi işlem açıklamalarını değil, model açıklamalarını en iyi çalıştırmaya kısıtlama. Bu davranış yerel, uzak ve ADB için değişiklik yapıyor.
    + Kullanıcı arabirimi için isteğe bağlı model açıklamaları desteği eklendi
    + Psutil, bir oto ml bağımlılığı olarak eklenmiştir ve psutil, amlcompute içindeki bir Conda bağımlılığı olarak eklenmiştir.
    + Tahmin verileri ile ilgili sorun düzeltilme ve kayan pencere boyutları, bazı dizileri doğrusal algedeniz hatalarına neden olabilir
      + Tahmin çalıştırmalarının buluşsal belirlenen parametreleri için baskı çıkışı eklendi.
  + **azureml-contrib-datadrift**
    + Veri kümesi düzeyi değişikliklerini ilk bölümde değilse, çıkış ölçümleri oluşturulurken koruma eklendi.
  + **azureml-contrib-yorumlama**
    + azureml-contrib-açıkla-model paketi azureml-contrib-yorumlama olarak yeniden adlandırıldı
  + **azureml-çekirdek**
    + Veri kümelerinin kaydını silmek için API eklendi. `dataset.unregister_all_versions()`
    + Veri değiştirme süresini denetlemek için DataSet API 'SI eklendi. `dataset.data_changed_time`.
    + @No__t_4 işlem hattındaki `PythonScriptStep`, `EstimatorStep` ve Azure Machine Learning giriş olarak `FileDataset` ve `TabularDataset` olarak tükeiyor
    + Çok sayıda dosya içeren klasörler için `FileDataset.mount` performansı geliştirilmiştir
    + Çalışma ayrıntılarında bilinen hata önerilerine URL eklendi.
    + Çalıştırılmakta olan bir hata düzeltildi. bir çalıştırma çok fazla alt öğe içeriyorsa isteklerin başarısız olabileceği _ölçümleri al
    + Arcadia kümesinde kimlik doğrulaması için destek eklendi.
    + Deneme nesnesi oluşturma, çalışma geçmişi izlemenin Azure Machine Learning çalışma alanında denemeyi alır veya oluşturur. Deneme kimliği ve arşivlenen süre, oluşturma sırasında denemeler nesnesine doldurulur. Örnek: deneme = deneme (çalışma alanı, "yeni deneme") experiment_id = experiment.id Archive () ve yeniden etkinleştirme (), denemeyi UX içinde gösterilmeden veya bir çağrıda varsayılan olarak döndürülen bir deneyde çağrılabilen işlevlerdir denemeleri 'yi listelemek için. Arşivlenmiş bir deneyle aynı ada sahip yeni bir deneme oluşturulursa, yeni bir ad geçirerek yeniden etkinleştirme sırasında arşivlenmiş denemeyi yeniden adlandırabilirsiniz. Yalnızca belirli bir ada sahip bir etkin deneme olabilir. Örnek: experiment1 = deneme (çalışma alanı, "etkin deneme") experiment1. Archive () # arşivlenmiş ile aynı ada sahip yeni etkin denemeler oluşturun. experiment2. = Denemeler (çalışma alanı, "etkin deneme") experiment1. yeniden etkinleştirme (new_name = "önceki etkin deneme") deneme sırasında statik yöntem listesi () bir ad filtresi ve ViewType filtresi alabilir. ViewType değerleri şunlardır: "ACTIVE_ONLY", "ARCHIVED_ONLY" ve "ALL" örnek: archived_experiments = denemeler. List (Workspace, view_type = "ARCHIVED_ONLY") all_first_experiments = denemeler. List (çalışma alanı, ad = "Ilk deneme", view_type = "tümü")
    + Model dağıtımı ve hizmet güncelleştirmesi için ortamı kullanma desteği
  + **azureml-datadrift**
    + DataDriftDector sınıfının Show özniteliği, isteğe bağlı ' with_details ' bağımsız değişkenini desteklemez. Show özniteliği yalnızca veri DRFT katsayısını ve özellik sütunlarının veri kayması katkısını sunar.
    + Datadriftalgılayıcı özniteliği ' get_output ' davranış değişiklikleri:
      + Giriş parametresi start_time, end_time zorunlu değil, isteğe bağlıdır;
      + aynı çağırmada belirli bir run_id ile özel start_time ve/veya end_time nput, birbirini dışlamalı olduğundan değer hata özel durumuna neden olur 
      + Girişe özgü start_time ve/veya end_time 'e göre yalnızca zamanlanan çalıştırmaların sonuçları döndürülür; 
      + ' Daily_latest_only ' parametresi kullanım dışıdır.
    + Veri kümesi tabanlı veri Drçıkışları almayı destekler.
  + **azureml-açıkla-model**
    + AzureML daha sonra geri uyumluluk için eski paketi tutmak üzere, AzureML-açıkla-model paketini AzureML-yorumlama olarak yeniden adlandırır
    + ExplanationClient adresinden indirme sırasında varsayılan olarak gerileme yerine sınıflandırma görevine ayarlanmış ham açıklamaları içeren sabit bir hata düzeltildi
    + @No__t_1 kullanarak doğrudan oluşturulacak `ScoringExplainer` için destek ekleyin
  + **azureml-işlem hattı-çekirdek**
    + Büyük işlem hattı oluşturma performansı geliştirildi
  + **azureml-tren-çekirdek**
    + TensorFlow Estimator 'da TensorFlow 2,0 desteği eklendi
  + **azureml-eğitme-oto ml**
    + Düzenleme zaten bu işlemi yaptığı için, kurulum yineleme başarısız olduğunda üst öğe çalıştırması artık başarısız olmayacaktır.
    + Oto ml denemeleri için yerel Docker ve yerel-Conda desteği eklendi


## <a name="2019-10-08"></a>2019-10-08

### <a name="new-web-experience-preview-for-azure-machine-learning-workspaces"></a>Azure Machine Learning çalışma alanları için yeni Web deneyimi (Önizleme)

[Yeni çalışma alanı portalındaki](http://ml.azure.com) deneme sekmesi, veri bilimcilerinin denemeleri daha performanslı bir şekilde izleyebilmesi için güncelleştirilmiştir. Aşağıdaki özellikleri inceleyebilirsiniz:
+ Denemeleri listenizi kolayca filtreleyip sıralamak için meta verileri deneyin
+ Öngörülerinizi görselleştirmenize ve karşılaştırmanıza olanak tanıyan Basitleştirilmiş ve performanslı deneme ayrıntıları sayfaları
+ Eğitim çalıştırmalarını anlamak ve izlemek için Ayrıntılar sayfalarını çalıştırmak üzere yeni tasarım

## <a name="2019-09-30"></a>2019-09-30

### <a name="azure-machine-learning-sdk-for-python-v1065"></a>Python v 1.0.65 için SDK Azure Machine Learning

  + **Yeni özellikler**
    + Seçkin ortamlar eklendi. Bu ortamlar, ortak makine öğrenimi görevlerinin kitaplıklarıyla önceden yapılandırılmıştır ve daha hızlı yürütülmek üzere Docker görüntüleri olarak önceden derlenerek önbelleğe alınır. Bu, varsayılan olarak çalışma alanının ortam listesinde ve "AzureML" önekiyle görünürler.
  
  + **azureml-eğitme-oto ml**
    + ADB ve HDI için ONNX dönüştürme desteği eklendi

+ **Önizleme özellikleri**  
  + **azureml-eğitme-oto ml**
    + Metin özelliği olarak desteklenen BERT ve BiLSTM (yalnızca Önizleme)
    + Sütun amacı ve transformatör parametreleri için desteklenen uygulanabilirlik özelleştirmesi (yalnızca Önizleme)
    + Kullanıcı eğitim sırasında model açıklamasını etkinleştirse desteklenen ham açıklamalar (yalnızca Önizleme)
    + Zaman serisi tahmin için Prophet, işlem hattı tahmini olarak eklendi (yalnızca Önizleme)
  
  + **azureml-contrib-datadrift**
    + Paketler, azureml-contrib-datadrift ile azureml-datadrift arasında yeniden konumlandırılır. contrib paketi sonraki bir sürümde kaldırılacaktır 

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **azureml-oto ml-çekirdek**
    + Cmlconfig ve oto Mlbasesettings için FeaturizationConfig sunuldu
      + Verilen sütun ve Özellik türüyle uygun şekilde sütun amacını geçersiz kıl
      + Transformatör parametrelerini geçersiz kıl
    + Explain_model () ve retrieve_model_explanations () için kullanımdan kaldırma iletisi eklendi
    + İşlem hattı olarak Prophet eklendi (yalnızca Önizleme)
    + Hedef lags, hareketli pencere boyutu ve en büyük ufuk için otomatik algılama desteği eklendi. Target_lags, target_rolling_window_size veya max_horizon ' Auto ' olarak ayarlanırsa, bu işlem, eğitim verilerine göre karşılık gelen parametrenin değerini tahmin etmek için uygulanır.
    + Veri kümesi bir Gren sütunu içerdiğinde, bu gren sayısal bir tür ve tren ve test kümesi arasında bir boşluk olduğunda sabit tahmin
    + Tahmin görevlerinde uzak çalıştırmada yinelenen dizin hakkında hata iletisi düzeltildi
    + Bir veri kümesinin kapalı olup olmadığını kontrol etmek için bir guardrayııl eklendi. Bu durumda, konsola bir guarddemiryolu iletisi yazılır.
  + **azureml-çekirdek**
    + Model nesnesi aracılığıyla depolamada SAS URL 'sini depolama sırasında modele alma özelliği eklendi. Ex: model. Get _sas_url ()
    + Gönderilen çalışma ile ilişkili veri kümelerini almak için `run.get_details()['datasets']` tanıtın
    + JSON satırları dosyalarından bir TabularDataset oluşturmak için API `Dataset.Tabular.from_json_lines_files` ekleyin. TabularDataset 'teki JSON satırları dosyalarında bu tablo verileri hakkında bilgi edinmek için lütfen belgeler için https://aka.ms/azureml-data ' ı ziyaret edin.
    + Supported_vmsizes () işlevine ek VM boyut alanları (işletim sistemi diski, GPU sayısı) eklendi
    + Çalışmayı, özel ve genel IP 'yi, bağlantı noktasını vb. göstermek için list_nodes () işlevine ek alanlar eklendi.
    + Küme sağlama sırasında yeni bir alan belirtme--remotelogin_port_public_access, bu durum, küme oluşturma sırasında SSH bağlantı noktasını açık veya kapalı olarak bırakmak isteyip istemediğinizi belirlemek için etkin veya devre dışı olarak ayarlanabilir. Bunu belirtmezseniz, küme bir sanal ağın içine dağıtıp dağıtdığınıza bağlı olarak bağlantı noktasını açar veya kapatır.
  + **azureml-açıkla-model**
    + Sınıflandırma senaryosunda açıklama çıkışları için geliştirilmiş belgeler.
    + Değerlendirme örnekleri için açıklamada tahmini y değerlerini karşıya yükleme özelliği eklendi. Daha kullanışlı görselleştirmelerin kilidini açar.
    + Temeldeki MimicExplainer almayı etkinleştirmek için Mimkıwrapper 'a Açıklama özelliği eklendi.
  + **azureml-işlem hattı-çekirdek**
    + Modülün açıklaması, ModuleVersion ve ModuleStep için Not defteri eklendi
  + **azureml-işlem hattı-adımlar**
    + AML işlem hattı aracılığıyla R betiği çalıştırmayı desteklemek için RScriptStep eklendi
    + AzureBatchStep içinde ayrıştırma, "SubscriptionID parametresi için atama belirtilmemiş" hata iletisine neden olan sabit meta veri parametreleri
  + **azureml-eğitme-oto ml**
    + Veri girişi biçimi olarak desteklenen training_data, validation_data, label_column_name, weight_column_name
    + Explain_model () ve retrieve_model_explanations () için kullanımdan kaldırma iletisi eklendi

  
## <a name="2019-09-16"></a>2019-09-16

### <a name="azure-machine-learning-sdk-for-python-v1062"></a>Python v 1.0.62 için SDK Azure Machine Learning

+ **Yeni özellikler**
  + Tabulardataset üzerinde zaman serisi nitelik kullanıma sunulmuştur. Bu nitelik, bir zaman aralığı veya en son veriler arasında tüm verileri almak gibi bir TabularDataset veri kümesi verilerinde kolay zaman damgası filtrelemesini mümkün bir şekilde sunar. Tabulardataset üzerinde bu zaman serisi nitelik hakkında bilgi edinmek için lütfen belgeler için https://aka.ms/azureml-data ' ı veya örnek bir not defteri için https://aka.ms/azureml-tsd-notebook ' i ziyaret edin. 
  + TabularDataset ve FileDataset ile eğitime etkin. Lütfen örnek bir not defteri için https://aka.ms/dataset-tutorial adresini ziyaret edin. 
  
  + **azureml-tren-çekirdek**
    + Pytorch tahmin aracı içinde nccl ve gloo desteği eklendi
  
+ **Hata düzeltmeleri ve geliştirmeleri**
  + **azureml-oto ml-çekirdek**
    + ' Lag_length ' ve LaggingTransformer.
    + Veri akışı biçiminde belirtilmişse giriş verilerinin doğru doğrulanması düzeltildi
    + JSON grafiğini oluşturmak ve yapıtlara yüklemek için fit_pipeline. Kopyala değiştirildi. 
    + Cytoscape kullanarak userrun altında grafik işlendi.
  + **azureml-çekirdek**
    + ADB kodundaki özel durum işlemeyi yeniden ziyaret ediyor ve yeni hata işleme uyarınca değişiklik yapma
    + Not defteri VM 'Leri için otomatik MSI kimlik doğrulaması eklendi.
    + Başarısız denemeler nedeniyle bozuk veya boş modellerin karşıya yüklenebildiği hatayı düzeltir.
    + @No__t_1 modu değiştiğinde `DataReference` adının değiştiği hata düzeltildi (örn. `as_upload`, `as_download` veya `as_mount` çağrılırken).
    + @No__t_2 ve `FileDataset.download` için `mount_point` ve `target_path` isteğe bağlı yapın.
    + Zaman damgası sütununun bulunamadığını belirten özel durum, zaman içinde ilgili API 'SI, hassas zaman damgası sütunu atanmamış veya atanan zaman damgası sütunları bırakılınca oluşturulur.
    + Zaman seri sütunları, türü tarih olan sütunla atanmalıdır, aksi takdirde özel durum beklenmektedir
    + ' With_timestamp_columns ' API 'sini atayan zaman damgası sütunları, daha önce atanan zaman damgası sütunlarını temizleyerek hiç bir değer ince/kaba zaman damgası sütun adı alabilir.
    + Büyük/küçük bir zaman damgası sütunu, bırakma listesindeki zaman damgası sütunu dışladıktan sonra veya with_time_stamp çağrısında zaman damgasına olmayan bir değere çağrı yapıldığında, sütunlardan
    + Sütunları tut listesine, sütun tut listesinde zaman damgası sütunu dahil edildiğinde veya with_time_stamp çağrısı yapılmamış bir Kullanıcı bildirimi ile, sütun tut listesinde yer alan bir durum ortaya çıkar. zaman damgası sütunlarını serbest bırakma değeri.
    + Kayıtlı bir modelin boyutu için günlük kaydı eklendi.
  + **azureml-açıkla-model**
    + "Paketleme" Python paketi yüklü olmadığında konsola sabitlenmiş uyarı düzeltildi: "desteklenen lightgbm sürümünden daha eski bir sürümü kullanıyor, lütfen 2.2.1 sürümüne yükseltin"
    + Birçok özelliğe sahip genel açıklamalar için parçalı yükleme modeli açıklaması düzeltildi
    + Eksik başlatma örnekleri çıkış açıklamasında açıklama düzeltildi
    + İki farklı model türü kullanarak açıklama istemcisiyle karşıya yüklenirken özelliklerde ayarlanan özelliklerde sabit hata düzeltildi
    + Puanlama açıklama. açıkla () için bir get_raw parametresi eklendi. bu nedenle, bir Puanlama açıklama hem mühendislik yapılan hem de ham değerleri döndürebilir.
  + **azureml-eğitme-oto ml**
    + Oto ml 'yi açıklayan ve oto ml 'nin SDK 'sı ile tümleşik ham açıklama desteğini destekleyen, oto ml 'nin SDK 'sı tarafından desteklenen genel API 'Ler modelde.
    + Uzak eğitim ortamlarından azureml varsayılan değerleri kaldırılıyor.
    + AzureDatabricks Code yolunda bulunan bir AzureFileCacheStore One tabanlı dosya olan varsayılan önbellek depolama konumu, bir.
    + Veri akışı biçiminde belirtilmişse giriş verilerinin doğru doğrulanması düzeltildi
  + **azureml-tren-çekirdek**
    + Source_directory_data_store kullanımdan kaldırıldı.
    + Azureml yüklü paket sürümlerini geçersiz kılma özelliği eklendi. 
    + Estimators 'da `environment_definition` parametreye dockerfile desteği eklendi.
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
   1. VM kurulumu sırasında oluşturulan SSH anahtarlarını bulun. Ya da Azure ML Azure portal 'ndaki anahtarları bulun > Işlem sekmesini açın > listedeki Not defteri sanal makinesini bulun > Bu özellikleri açın: iletişim kutusundan anahtarları kopyalayın.
   1. Bu ortak ve özel SSH anahtarlarını yerel makinenize aktarın.
   1. Bunları not defteri VM 'sine SSH için kullanın. 

## <a name="2019-09-03"></a>2019-09-03
### <a name="azure-machine-learning-sdk-for-python-v1060"></a>Python v 1.0.60 için SDK Azure Machine Learning

+ **Yeni özellikler**
  + Veri mağazalarınızın veya genel URL 'lerdeki tek veya birden çok dosyaya başvuran FileDataset kullanıma sunulmuştur. Dosyalar herhangi bir biçimde olabilir. Dosya veri kümesi size dosyaları indirme veya işlem için bağlama yeteneği sağlar. FileDataset hakkında bilgi edinmek için lütfen https://aka.ms/file-dataset ' ı ziyaret edin.
  + PythonScript Step, adla Step, Databricks Step, DataTransferStep ve AzureBatch Step için işlem hattı YAML desteği eklendi

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **azureml-oto ml-çekirdek**
    + Oto, yalnızca önizleme için bir önerimme tablosu ardışık düzeni.
    + Tahmin için geliştirilmiş hata raporlaması.
    + Tahmin görevlerinde genel yerine özel özel durumlar kullanılarak günlüğe kaydetme geliştirildi.
    + Max_concurrent_iterations üzerindeki denetim, toplam yineleme sayısından az olacak şekilde kaldırıldı.
    + Oto ml modelleri artık oto Mlexceptions döndürüyor
    + Bu sürüm, otomatik makine öğrenimi yerel çalıştırmalarının yürütme performansını geliştirir.
  + **azureml-çekirdek**
    + Veri kümesini tanıtın. Al _All (çalışma alanı). Bu, kayıt adı tarafından girilen `TabularDataset` ve `FileDataset` nesnelerinin bir sözlüğünü döndürür. 
    
    ```py 
    workspace = Workspace.from_config() 
    all_datasets = Dataset.get_all(workspace) 
    mydata = all_datasets['my-data'] 
    ```
    
    + @No__t_1 ve `Dataset.Tabular.from_parquet.files` `parition_format` bağımsız değişken olarak tanıtın. Her veri yolunun bölüm bilgileri, belirtilen biçime göre sütunlarda ayıklanır. ' {column_name} ' dize sütunu oluşturuyor ve ' {column_name: yyyy/aa/gg/HH/mm/ss} ' DateTime sütunu oluşturuyor; burada ' yyyy ', ' AA ', ' gg ', ' HH ', ' mm ' ve ' ss ', DateTime türü için Year, month, Day, Hour, Minute ve Second 'ı ayıklamak için kullanılır. Partition_format, dosya yolunun sonuna kadar ilk bölüm anahtarının konumundan başlamalıdır. Örneğin, '. ' yolu veriliyor. /USA/2019/01/01/data.csv '; bölüm ülke ve zamana göre, partition_format = '/{Country}/{PartitionDate: yyyy/aa/gg}/Data. csv ', ' USA ' değeri ve ' 2019-01-01 ' değerli ' PartitionDate ' DateTime sütunuyla ' Country ' dize sütununu oluşturur.
    + `to_csv_files` ve `to_parquet_files` yöntemleri `TabularDataset` eklenmiştir. Bu yöntemler, verileri belirtilen biçimdeki dosyalara dönüştürerek bir `TabularDataset` ve `FileDataset` arasında dönüştürmeyi etkinleştirir.
    + Model. Package () tarafından oluşturulan bir Dockerfile dosyasını kaydederken otomatik olarak temel görüntü kayıt defterinde oturum açın.
    + ' gpu_support ' artık gerekli değildir; AzureML artık kullanılabilir olduğunda NVIDIA Docker uzantısını otomatik olarak algılar ve kullanır. Daha sonraki bir sürümde kaldırılacak.
    + PipelineDrafts oluşturma, güncelleştirme ve kullanma desteği eklendi.
    + Bu sürüm, otomatik makine öğrenimi yerel çalıştırmalarının yürütme performansını geliştirir.
    + Kullanıcılar, çalışma geçmişinden ada göre ölçümleri sorgulayabilir.
    + Tahmin görevlerinde genel yerine özel özel durumlar kullanılarak günlüğe kaydetme geliştirildi.
  + **azureml-açıkla-model**
    + Yeni MimicWrapper 'a feature_maps parametresi eklendi ve kullanıcıların ham özellik açıklamalarını almalarına izin veriliyor.
    + Açıklama yüklemesi için veri kümesi yüklemeleri artık varsayılan olarak kapalıdır ve upload_datasets = true ile yeniden etkinleştirilebilir
    + Açıklama listesine ve indirme işlevlerine "is_law" parametresi ekleniyor.
    + Hem genel hem de yerel açıklama nesnelerine `get_raw_explanation(feature_maps)` yöntem ekler.
    + Desteklenen sürüm altında, yazdırılmış uyarı ile birlikte lightgbm için sürüm denetimi eklendi
    + Açıklamaları toplu işleme alırken İyileştirilmiş bellek kullanımı
    + Oto ml modelleri artık oto Mlexceptions döndürüyor
  + **azureml-işlem hattı-çekirdek**
    + PipelineDrafts oluşturma, güncelleştirme ve kullanma desteği eklendi-kesilebilir işlem hattı tanımlarını sürdürmek ve bunları çalıştırmak için etkileşimli olarak kullanmak için kullanılabilir
  + **azureml-eğitme-oto ml**
    + Uzak Python çalışma zamanı ortamında BERT/XLNet ' i etkinleştirmek için gereken, GPU özellikli pytorch v 1.1.0, CUDA Toolkit 9,0, pytorch-dönüştürücüler 'ın belirli sürümlerini yüklemek için özelliği oluşturuldu.
  + **azureml-tren-çekirdek**
    + Bazı hiper parametre alanı tanımı hatalarının sunucu tarafı yerine doğrudan SDK 'da erken hatası.

### <a name="azure-machine-learning-data-prep-sdk-v1114"></a>Azure Machine Learning Data Prep SDK v 1.1.14
+ **Hata düzeltmeleri ve geliştirmeleri**
  + Ham yol ve kimlik bilgileri kullanılarak ADLS/ADLSGen2 'a yazma özelliği etkinleştirildi.
  + @No__t_0 `read_parquet` için çalışmamasının neden olduğu bir hata düzeltildi.
  + "Geçersiz özellik değeri: hostSecret" özel durumuna neden olan hata `to_pandas_dataframe()` düzeltildi.
  + Spark modundaki DBFS üzerinde dosyaların okunmamasına neden olan bir hata düzeltildi.
  
## <a name="2019-08-19"></a>2019-08-19

### <a name="azure-machine-learning-sdk-for-python-v1057"></a>Python v 1.0.57 için SDK Azure Machine Learning
+ **Yeni özellikler**
  + Etkinleştirilen `TabularDataset`, AutomatedML tarafından tüketilmelidir. @No__t_0 hakkında daha fazla bilgi edinmek için lütfen https://aka.ms/azureml/howto/createdatasets ziyaret edin.
  
+ **Hata düzeltmeleri ve geliştirmeleri**
  + **Oto ml-Client-Core-NativeClient**
    + Bir hata düzeltildi, eğitim ve/veya doğrulama etiketleri (y ve y_valid), Pandas dataframe biçiminde sağlandığı halde bir sayısal tuş y dizisi olarak değil, ortaya çıktı.
    + Yalnızca verileri ve `AutoMLBaseSettings` nesnesini gerektirecek `RawDataContext` oluşturmak için arabirim güncelleştirildi.
    +  Oto ml kullanıcılarına tahmin sırasında yeterince uzun olmayan eğitim serisini bırakmaya izin verin. -Oto ml kullanıcılarına, tahmin sırasında eğitim kümesinde mevcut olmayan test kümesinden graıns 'yi bırakması izin verin.
  + **Azure-CLI-ml**
    + Artık, Microsoft tarafından oluşturulan ve müşteri sertifikası için AKS kümesinde dağıtılan Puanlama uç noktası için SSL sertifikasını güncelleştirebilirsiniz.
  + **azureml-oto ml-çekirdek**
    + Eksik etiketlere sahip satırların düzgün şekilde kaldırılmadığı, oto ml 'deki bir sorun düzeltildi.
    + Oto ml 'de geliştirilmiş hata günlüğü; Tam hata iletileri artık her zaman günlük dosyasına yazılır.
    + Oto, `azureml-defaults`, `azureml-explain-model` ve `azureml-dataprep` dahil olmak üzere paket sabitleme paketini güncelleştirmiştir. Oto, paket uyuşmazlıkları (`azureml-train-automl` paketi hariç) üzerinde artık uyarı vermez.
    + Zaman serisi 'te CV bölünmesinin eşit olmadığı, küme hesaplamasının başarısız olmasına neden olan bir sorun düzeltildi.
    + Çapraz doğrulama eğitim türü için ensebir yineleme çalıştırırken, veri kümesinin tamamında eğitilen modelleri indirirken sorun yaşadığımızda, model ağırlıkları ve oylama 'ye beslendiği modeller arasında bir tutarsızlık vardı topluluğu.
    + Bir hata düzeltildi, eğitim ve/veya doğrulama etiketleri (y ve y_valid), Pandas dataframe biçiminde sağlandığı halde bir sayısal tuş y dizisi olarak değil, ortaya çıktı.
    + Giriş tablolarının Boole sütunlarında hiç karşılaşılmadığı zaman tahmin görevlerinde sorun düzeltildi.
    + Oto ml kullanıcılarına tahmin sırasında yeterince uzun olmayan eğitim serisini bırakmaya izin verin. -Oto ml kullanıcılarına, tahmin sırasında eğitim kümesinde mevcut olmayan test kümesinden graıns 'yi bırakması izin verin.
  + **azureml-çekirdek**
    + Blob_cache_timeout parametresi sıralaması ile ilgili sorun düzeltildi.
    + Dış sığdırma ve özel durum türlerini sistem hatalarına ekledi.
    + Uzak çalıştırmalar için Key Vault gizli dizileri için destek eklendi. Çalışma alanınız ile ilişkili anahtar kasasından gizli dizi eklemek, almak ve listelemek için bir azureml. Core. keykasa. Keykasası sınıfı ekleyin. Desteklenen işlemler şunlardır:
      + azureml. Core. Workspace. Workspace. Get _default_keykasa()
      + azureml. Core. keykasa. Keykasası. set _secret (ad, değer)
      + azureml. Core. keykasa. Keykasası. _gizli dizileri ayarla (secrets_dict)
      + azureml. Core. keykasa. Keykasası. Get _secret (ad)
      + azureml. Core. keykasa. Keykasası. Get _gizlilikler (secrets_list)
      + azureml. Core. keykasa. Keykasa. list_secrets ()
    + Varsayılan anahtar kasasını elde etmek ve Uzaktan çalıştırma sırasında gizli dizileri almak için ek Yöntemler:
      + azureml. Core. Workspace. Workspace. Get _default_keykasa()
      + azureml. Core. Run. Run. Get _secret (ad)
      + azureml. Core. Run. Run. Get _gizlilikler (secrets_list)
    + Gönderme-Hyperdrive CLı komutuna ek geçersiz kılma parametreleri eklendi.
    + API çağrılarının güvenilirliğini artırmak, yaygın istekler Kitaplığı özel durumlarına yeniden denemeler genişletmektedir.
    + Gönderilen bir çalışmadan çalıştırma göndermek için destek ekleyin.
    + Dosya izleyicisinden, ilk belirtecinin süresi dolduktan sonra karşıya yükleme işlemini durdurmasına neden olan, süresi dolan son SAS belirteç sorunu düzeltildi.
    + Veri kümesi Python SDK 'sında HTTP CSV/TSV dosyalarını içeri aktarma destekleniyor.
    + Workspace. Setup () yöntemi kullanımdan kaldırıldı. Kullanıcılara gösterilen uyarı iletisi, bunun yerine Create () veya Get ()/from_config () kullanılmasını önerir.
    + Ortam eklendi. özel özel Python paketlerinin (. WHL) çalışma alanına yüklenmesini sağlayan ve ortamı derlemek/denemek için güvenli bir şekilde kullanarak, _private_pip_wheel () ekleyin.
    + Artık, Microsoft tarafından oluşturulan ve müşteri sertifikası için AKS kümesinde dağıtılan Puanlama uç noktası için SSL sertifikasını güncelleştirebilirsiniz.
  + **azureml-açıkla-model**
    + Karşıya yükleme ile ilgili açıklamaları bir model KIMLIĞI eklemek için parametresi eklendi.
    + Bellekteki açıklamaları etiketleyerek ve karşıya yükleme `is_raw` eklendi.
    + Azureml-açıkla-model paketi için pytorch desteği ve Testleri eklendi.
  + **azureml-OPENDATASET 'ler**
    + Otomatik test ortamını algılayıp günlüğe kaydetmeyi destekler.
    + Ülke ve ZIP tarafından nüfusu almak için sınıflar eklendi.
  + **azureml-işlem hattı-çekirdek**
    + Giriş ve çıkış bağlantı noktası tanımlarına etiket özelliği eklendi.
  + **azureml-telemetri**
    + Hatalı telemetri yapılandırması düzeltildi.
  + **azureml-eğitme-oto ml**
    + Kurulum hatasında hata düzeltildi, hata, kurulum çalıştırmasının "hatalar" alanında oturum açmamıştı ve bu nedenle üst çalıştırmada "hatalar" olarak depolanmadı.
    + Eksik etiketlere sahip satırların düzgün şekilde kaldırılmadığı, oto ml 'deki bir sorun düzeltildi.
    + Oto ml kullanıcılarına tahmin sırasında yeterince uzun olmayan eğitim serisini bırakmaya izin verin.
    + Oto ml kullanıcılarına, tahmin sırasında eğitim kümesinde mevcut olmayan test kümesinden graıns 'yi bırakması için izin verin.
    + Şimdi, yeni yapılandırma parametrelerinin değişiklikleri veya eklemeleri üzerinde herhangi bir sorun yaşamamak için, oto ml config 'ten arka uca geçiş yapar.
    + Oto Data Guardkıl artık genel önizlemeye sunuldu. Kullanıcı eğitim sonrasında (sınıflandırma/gerileme görevleri için) bir veri Guardcıl raporu görür ve ayrıca SDK API aracılığıyla buna erişebilir.
  + **azureml-tren-çekirdek**
    + PyTorch Estimator 'da Torch 1,2 desteği eklenmiştir.
  + **azureml-pencere öğeleri**
    + Sınıflandırma eğitimi için geliştirilmiş karışıklık matrisi grafikleri.

### <a name="azure-machine-learning-data-prep-sdk-v1112"></a>Azure Machine Learning Data Prep SDK v 1.1.12
+ **Yeni özellikler**
  + Dizelerin listesi artık `read_*` yöntemlere girdi olarak geçirilebilir.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + @No__t_0 performansı Spark 'ta çalışırken önemli ölçüde iyileştirilmiştir.
  + Belirsiz tarih biçimlerine sahip tek bir sütun olması durumunda `column_type_builder` başarısız olduğu bir sorun düzeltildi.

### <a name="azure-portal"></a>Azure portalı
+ **Önizleme özelliği**
  + Günlük ve çıkış dosyası akışı artık çalışma ayrıntıları sayfaları için kullanılabilir. Dosyalar, önizleme geçişi açık olduğunda güncelleştirmeleri gerçek zamanlı olarak akışa alır.
  + Çalışma alanı düzeyinde kota ayarlama özelliği önizleme aşamasında serbest bırakılır. AmlCompute kotaları abonelik düzeyinde ayrılır, ancak artık bu kotayı çalışma alanları arasında dağıtmanıza ve bunları dengeli paylaşım ve idare için ayırmaya izin veririz. Çalışma alanınızın sol gezinti çubuğunda **kullanımlar + kotalar** dikey penceresine ve **kotaları Yapılandır** sekmesini seçmeniz yeterlidir. Bu bir çoklu çalışma alanı olduğundan, çalışma alanı düzeyinde kotalar ayarlayabilmek için bir abonelik yöneticisi olmanız gerektiğini unutmayın çalışmasını.

## <a name="2019-08-05"></a>2019-08-05

### <a name="azure-machine-learning-sdk-for-python-v1055"></a>Python v 1.0.55 için SDK Azure Machine Learning

+ **Yeni özellikler**
  + Belirteç tabanlı kimlik doğrulaması artık AKS üzerinde dağıtılan Puanlama uç noktasına yapılan çağrılar için desteklenir. Geçerli anahtar tabanlı kimlik doğrulamasını desteklemeye devam edeceğiz ve kullanıcılar aynı anda bu kimlik doğrulama mekanizmalarından birini kullanabilir.
  + Sanal ağın (VNet) arkasındaki bir blob depolamayı bir veri deposu olarak kaydetme yeteneği.
  
+ **Hata düzeltmeleri ve geliştirmeleri**
  + **azureml-oto ml-çekirdek**
    + CV bölme için doğrulama boyutunun küçük olduğu ve gerileme ve tahmin için gerçek grafik ile sonuçları elde eden bir hatayı düzeltir.
    + Uzak çalışmalarla ilgili tahmin görevlerinin günlüğe kaydedilmesi, çalışma başarısız olursa, artık kullanıcı kapsamlı bir hata iletisiyle sunulmaktadır.
    + Ön işleme bayrağı true ise timeseries 'in düzeltilen sorunları.
    + Bazı tahmin verileri doğrulama hata iletileri daha fazla işlem yapılabilir.
    + , Özellikle işlem kaynakları arasında yer aldığı veri kümelerinin düşürümine ve/veya geç yüklenmesine göre, oto 'nin azaltılmış bellek tüketimi
  + **azureml-contrib-açıkla-model**
    + Kullanıcının model türü için varsayılan otomatik çıkarım mantığını geçersiz kılmasına izin vermek için explainers 'e model_task bayrağı eklendi
    + Pencere öğesi değişiklikleri: yalnızca genel özellik önem derecesine sahip contrib ile otomatik olarak yüklenir, daha fazla nbextension yükleme/etkinleştirme-destek açıklaması (ör. permütasyon)
    + Pano değişiklikleri:-Box çizimleri ve keman çizimleri Özet sayfasındaki beessıcak çizimi, ' top-k ' kaydırıcısının çok daha hızlı bir şekilde rerendering, bu arada veriler sağlanmadı
  + **azureml-çekirdek**
    + Modelleri ve bunların bağımlılıklarını kapsülleyen Docker görüntüleri ve Dockerfiles oluşturmak için model. Package () yöntemi eklendi.
    + Ortam nesneleri içeren ınenceconfigs 'leri kabul etmek için yerel WebServices güncelleştirildi.
    + Sabit model. Register (), '. ' olduğunda geçersiz modeller üretmiyor (geçerli dizin için) model_path parametresi olarak geçirilir.
    + Run. submit_child öğesini ekleyin, işlev, gönderilen alt öğenin üst öğesi olarak çalıştırmayı belirtirken denemeler. gönderim yansıtır.
    + Modelden yapılandırma seçeneklerini destekler. Run. register_model içinde kaydolun.
    + Mevcut kümede JAR işlerini çalıştırma özelliği.
    + Artık instance_pool_id ve cluster_log_dbfs_path parametrelerini destekleme.
    + Bir model bir Web WebService 'a dağıtıldığında bir ortam nesnesi kullanma desteği eklendi. Ortam nesnesi artık ınısenceconfig nesnesinin bir parçası olarak sağlanıyor olabilir.
    + Yeni bölgeler için appınsıfht Mapping ekleme-tek merkezde ABD-westus-Kuzeydoğu ABD
    + Tüm veri deposu sınıflarında tüm öznitelikler için belge eklendi.
    + @No__t_0 için blob_cache_timeout parametresi eklendi.
    + Save_to_directory ve load_from_directory yöntemleri azureml. Core. Environment. Environment öğesine eklendi.
    + CLı 'ya "az ml Environment Download" ve "az ml Environment Register" komutları eklendi.
    + Ortam eklendi. _private_pip_wheel metodunu ekleyin.
  + **azureml-açıkla-model**
    + DataSet hizmeti (Önizleme) kullanılarak açıklamaları olan veri kümesi izleme eklendi.
    + 10.000 'den 100 'e genel açıklamalar akışı yapılırken varsayılan toplu iş boyutu azalır.
    + Kullanıcının model türü için varsayılan otomatik çıkarım mantığını geçersiz kılmasına izin vermek için explainers 'e model_task bayrağı eklendi.
  + **azureml-mlflow**
    + İç içe dizinler göz ardı edildiği mlflow. azureml. build_image öğesinde hata düzeltildi.
  + **azureml-işlem hattı-adımlar**
    + Mevcut Azure Databricks kümesinde JAR işlerini çalıştırma özelliği eklendi.
    + DatabricksStep Step için support instance_pool_id ve cluster_log_dbfs_path Parameters eklendi.
    + DatabricksStep adımında işlem hattı parametreleri için destek eklendi.
  + **azureml-eğitme-oto ml**
    + İlişkili dosyalar için docstrings eklendi.
    + Belgeler `max_cores_per_iteration` ve `max_concurrent_iterations` için daha uygun dile güncelleştirildi
    + Uzak çalışmalarla ilgili tahmin görevlerinin günlüğe kaydedilmesi, çalışma başarısız olursa, artık kullanıcı kapsamlı bir hata iletisiyle sunulmaktadır.
    + Get_Data, ardışık düzen AutoSize Not defterinden kaldırıldı.
    + Oto mlstep 'te dataprep desteği başlatıldı.

### <a name="azure-machine-learning-data-prep-sdk-v1110"></a>Azure Machine Learning Data Prep SDK v 1.1.10

+ **Yeni özellikler**
  + Artık belirli bir sütunda belirli Denetçiler (örn. histogram, dağılım çizimi vb.) yürütmeyi isteyebilirsiniz.
  + @No__t_0 için bir paralel hale getirmek bağımsız değişkeni eklendi. Doğru ise, veriler belleğe yüklenir ancak yürütme paralel olarak çalıştırılır; Yanlış ise, yürütme akış, ancak tek iş parçacıklı olur.

## <a name="2019-07-23"></a>2019-07-23

### <a name="azure-machine-learning-sdk-for-python-v1053"></a>Python v 1.0.53 için SDK Azure Machine Learning

+ **Yeni özellikler**
  + Otomatik Machine Learning artık uzak işlem hedefinde ONNX modellerini eğitimini destekliyor
  + Azure Machine Learning, artık önceki bir çalıştırma, kontrol noktası veya model dosyalarından eğitim verme olanağı sağlar.
    + Daha [önceki bir çalıştırınızdan eğitime geçmek için tahmini](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-tensorflow-resume-training/train-tensorflow-resume-training.ipynb) 'ı nasıl kullanacağınızı öğrenin

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **Oto ml-Client-Core-NativeClient**
    + Dönüşümden sonra sütun türlerini gevtürme (hata bağlantısı) ile ilgili hatayı düzeltir; 
    + Y_query, Begin (#459519) sırasında None (öğeleri) içeren bir nesne türü olmasını sağlar.
  + **Azure-CLI-ml**
    + CLı komutları "model dağıtımı" ve "hizmet güncelleştirmesi" artık parametreleri, yapılandırma dosyalarını veya ikisinin birleşimini kabul eder. Parametrelerin, dosyalardaki özniteliklerin önceliği vardır.
    + Model açıklaması artık kayıt sonrasında güncelleştirilebilen
  + **azureml-oto ml-çekirdek**
    + 1\.2.0 sürümüne NimbusML bağımlılığını güncelleştir (geçerli en son).
    + Nimbus ml tahmini için destek eklemek, oto ml tahmini içinde kullanılacak işlem hatları &.
    + Bir hatayı düzeltme, sonuçta elde edilen ve sonuç olarak büyümekte olan bir hata düzeltiliyor.
    + Tahmin görevleri için CV bölmeler genelinde bazı özellikler yeniden kullanımını etkinleştirin. Bu, n_cross_validations ve hareketli pencereler gibi pahalı özellikler için kabaca bir faktör olan kurulum 'un çalışma süresini hızlandırır.
    + Zaman Pandas desteklenen zaman aralığı dışında bir sorun adresleme. Şimdi PD 'den küçükse bir DataException tetikliyoruz. Timestamp. Min veya PD 'den büyük. Timestamp. Max
    + Tahmin şimdi, eğiteseler tren ve test kümelerinde farklı sıklıklara izin veriyor. Örneğin, "Ocak ayında başlayan üç aylık dönem" ve "Ekim ayında başlayan" üç aylık olarak hizalanabilir.
    + "Parameters" özelliği TimeSeriesTransformer öğesine eklendi.
    + Eski özel durum sınıflarını kaldırın.
    + Tahmin görevlerinde `target_lags` parametresi artık tek bir tamsayı değeri ya da tamsayılar listesini kabul eder. Tamsayı sağlanmışsa, yalnızca bir gecikme oluşturulur. Bir liste sağlanmışsa, lags 'nin benzersiz değerleri alınacaktır. target_lags = [1, 2, 2, 4] bir, 2 ve 4 dönem için lags oluşturacak.
    + Dönüşümden sonra sütun türlerini kaybetme hakkındaki hatayı düzeltir (hata bağlandı);
    + @No__t_0 ' de, y_query ' nin Begin (#459519) sırasında hiçbiri içeren bir nesne türü olmasını sağlar.
    + Beklenen değerleri, oto ml çıktısına Ekle
  + **azureml-contrib-datadrift**
    +  Örnek Not defteri geliştirmeleri; azureml, verileri zenginleştirmeden azureml-contrib-OPENDATASET ve performans iyileştirmeleri yerine azureml-OPENDATASET 'e geçiş
  + **azureml-contrib-açıkla-model**
    + Azureml-contrib-açıkla-model paketindeki ham özellik için LIME açıklama için sabit dönüşümler bağımsız değişkeni
    + segmentations for AzureML-contrib-açıkla-model paketi için Image açıklama içindeki görüntü açıklamaları eklendi
    + LimeExplainer için SciPy seyrek desteği ekleme
    + include_local = false olduğunda açıklama 'ın yürütme süresini geliştirmek için toplu iş içindeki genel açıklamaları akışa almak için batch_size ekleyin
  + **azureml-contrib-featureengineering**
    + Set_featurizer_timeseries_params () çağrısı için çözüm: dict değer türü değişikliği ve null denetimi-zaman serisi için Not defteri özelliği Ekle
    + 1\.2.0 sürümüne NimbusML bağımlılığını güncelleştir (geçerli en son).
  + **azureml-çekirdek**
    + AzureML türünde DBFS veri depoları ekleme özelliği eklendi 
    + @No__t_0 `/` başlatıldıktan sonra boş bir klasör oluşturulduğunda veri deposu yüklemesine hata düzeltildi
    + Serviceprincıpalauthentication içinde sabit ayrıntılı kopyalama sorunu düzeltildi.
    + CLı 'ya "az ml Environment Show" ve "az ml Environment List" komutları eklendi.
    + Ortamlar artık zaten derlenmiş bir base_image için alternatif olarak bir base_dockerfile belirtmeyi desteklemektedir.
    + Kullanılmayan RunConfiguration ayarı auto_prepare_environment kullanım dışı olarak işaretlendi.
    + Model açıklaması artık kayıt sonrasında güncelleştirilebilen
    + Bugdüzeltmesini: model ve resim silme işlemi, yukarı akış bağımlılığı nedeniyle silme başarısız olursa, bunlara bağlı yukarı akış nesnelerini alma hakkında daha fazla bilgi sağlamaktadır.
    + Bazı ortamlar için bir çalışma alanı oluşturulurken gerçekleşen dağıtımlar için boş süre yazdırılan hata düzeltildi.
    + İyileştirilmiş çalışma alanı hata özel durumları oluşturma. Kullanıcılar "çalışma alanı oluşturulamıyor. Bulunamıyor... " ileti olarak ve bunun yerine gerçek oluşturma hatası ' nı görürsünüz.
    + AKS WebServices içinde belirteç kimlik doğrulaması desteği ekleyin. 
    + @No__t_1 nesnelerine `get_token()` yöntemi ekleyin.
    + Machine Learning veri kümelerini yönetmek için CLı desteği eklendi.
    + `Datastore.register_azure_blob_container` artık isteğe bağlı olarak, bu veri deposu için önbellek süre sonunu etkinleştirmek üzere blobsigortası 'nin Mount parametrelerini yapılandıran bir `blob_cache_timeout` değeri (saniye cinsinden) alır. Varsayılan değer zaman aşımı değildir, yani bir blob okunarak iş bitene kadar yerel önbellekte kalır. Çoğu iş bu ayarı tercih eder, ancak bazı işlerin, düğümlerine sığacak kadar büyük bir veri kümesinden daha fazla veri okuması gerekir. Bu işler için, bu parametrenin ayarlanması başarılı olur. Bu parametreyi ayarlarken dikkatli olmanız gerekir: değeri çok düşük olarak ayarlamak, bir dönem içinde kullanılan verilerin yeniden kullanılmadan önce süresinin dolmasına neden olabilir. Bu, tüm okumaların, eğitim sürelerini olumsuz yönde etkileyen yerel önbellek yerine blob depolamadan (ağ) yapılacağı anlamına gelir.
    + Model açıklaması artık kayıt sonrasında düzgün şekilde güncelleştirilebilen olabilir
    + Model ve resim silme artık, silinmesine bağlı yukarı akış nesneleri hakkında daha fazla bilgi sağlar ve bu da silmenin başarısız olmasına neden olur
    + Azureml. mlflow kullanarak uzak çalıştırmaların kaynak kullanımını geliştirme.
  + **azureml-açıkla-model**
    + Azureml-contrib-açıkla-model paketindeki ham özellik için LIME açıklama için sabit dönüşümler bağımsız değişkeni
    + LimeExplainer için SciPy seyrek desteği ekleme
    + Doğrusal modelleri açıklayan şekil doğrusal açıklama sarmalayıcı ve tablo açıklama 'e başka bir düzey eklendi
    + Model kitaplığı 'nda açıklama benzeklik için, include_local = false olduğunda, seyrek veri girişi için hata düzeltildi
    + Beklenen değerleri, oto ml çıktısına Ekle
    + ham Özellik önem derecesi almak için dönüşümler bağımsız değişkeni sağlandığında düzeltilen permütasyon özelliği önem derecesi
    + include_local = false olduğunda açıklama 'ın yürütme süresini geliştirmek için toplu iş içindeki genel açıklamaları akışa almak için batch_size ekleyin
    + Model explainability kitaplığı için, Pandas dataframe girişinin tahmin için gerekli olduğu sabit kara Box explainers
    + @No__t_0, zaman içinde float içeren bir liste yerine float döndüren bir hata düzeltildi.
  + **azureml-mlflow**
    + Mlflow 'un performansını geliştirir. set _deneme (experiment_name)
    + Mlflow tracking_uri için ınteractiveloginauthentication kullanımda olan hatayı düzeltir
    + Azureml. mlflow kullanarak uzak çalıştırmaların kaynak kullanımını geliştirme.
    + Azureml-mlflow paketinin belgelerini geliştirme
    + Mlflow. log _yapay öğelerinden ("my_dir") yapıları, "< yapıt-Paths > yerine" my_dir/< yapıt-Paths > "altına kaydedebileceği Patch hatası
  + **azureml-OPENDATASET 'ler**
    + Yeni eklenen bellek sorunu nedeniyle OPENDATASET 'in pyarrow satırını eski sürümlere (< 0.14.0) sabitleyin.
    +  Azureml-contrib-OPENDATASET veri kümelerini azureml-OPENDATASET 'e taşıyın. -Açık veri kümesi sınıflarının AML çalışma alanına kaydolmasına ve AML veri kümesi özelliklerinden sorunsuz bir şekilde yararlanmasını sağlar. -SPARK olmayan sürümde önemli performansı önemli ölçüde zenginleştirin.
  + **azureml-işlem hattı-adımlar**
    + DBFS veri deposu artık DatabricksStep içindeki girişler ve çıkışlar için desteklenir.
    + Giriş/çıkışlara göre Azure Batch adım için güncelleştirilmiş belgeler.
    + AzureBatchStep ' de, *delete_batch_job_after_finish* varsayılan değeri *true*olarak değiştirilmiştir.
  + **azureml-telemetri**
    +  Azureml-contrib-OPENDATASET veri kümelerini azureml-OPENDATASET 'e taşıyın. -Açık veri kümesi sınıflarının AML çalışma alanına kaydolmasına ve AML veri kümesi özelliklerinden sorunsuz bir şekilde yararlanmasını sağlar. -SPARK olmayan sürümde önemli performansı önemli ölçüde zenginleştirin.
  + **azureml-eğitme-oto ml**
    + Get_output hakkındaki belgeler, gerçek dönüş türünü yansıtacak ve anahtar özelliklerini alma hakkında ek notlar sağlayacak şekilde güncelleştirildi.
    + 1\.2.0 sürümüne NimbusML bağımlılığını güncelleştir (geçerli en son).
    + Beklenen değerleri, oto ml çıktısına Ekle
  + **azureml-tren-çekirdek**
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
  + **azureml-OPENDATASET 'ler**
    + **azureml-contrib-OPENDATASET** 'ler artık **azureml-OPENDATASET**olarak kullanılabilir. Eski paket çalışmaya devam edebilir, ancak daha zengin özellikler ve geliştirmeler için, daha zengin bir şekilde veri taşımaya yönelik **azureml ile openopendataset** kullanmanızı öneririz.
    + Bu yeni paket, AML çalışma alanında açık veri kümelerini kayıt defteri olarak kaydetmenize ve veri kümesinin sunduğu işlevleri kullanmanıza olanak sağlar.
    + Ayrıca, açık veri kümelerini Pandas/SPARK dataframes olarak kullanma gibi mevcut özellikleri ve hava durumu gibi bazı veri kümelerinde konum birleştirmelerini da içerir.

+ **Önizleme özellikleri**
    + HyperDriveConfig, işlem hattı nesnesini bir işlem hattı kullanarak hiper parametre ayarlamayı destekleyecek bir parametre olarak kabul edebilir.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **azureml-eğitme-oto ml**
    + Dönüşümden sonra sütun türlerini kaybetme hakkında hata düzeltildi.
    + Y_query 'ın başlangıcında None (s) içeren bir nesne türü olması için hata düzeltildi. 
    + Son olarak, sonuç olarak elde edilen puanlar sabit kaldığı halde ortaya çıkacak şekilde büyümekte olan, Ensebir seçim yordamında sorun düzeltildi.
    + Whitelist_models ve blacklist_models ayarlarındaki sorun, oto Mlstep içinde düzeltildi.
    + Azure ML işlem hatları bağlamında, oto ml kullanıldığında ön işleme kullanımını önleyen sorun düzeltildi.
  + **azureml-OPENDATASET 'ler**
    + Azureml-contrib-OPENDATASET 'leri azureml-OPENDATASET 'e taşındı.
    + AML çalışma alanına kaydedilecek ve AML veri kümesi özelliklerinden sorunsuz bir şekilde yararlanan açık veri kümesi sınıflarının kullanımına izin verilir.
    + SPARK olmayan sürümde gelişmiş Noaaısdhava durumu performansı önemli ölçüde geliştirildi.
  + **azureml-açıkla-model**
    + Yorumlenebilirlik nesneleri için çevrimiçi belgeler güncelleştirildi.
    + DecisionTreeExplainableModel yürütme süresini geliştirmek üzere toplu iş içindeki genel açıklamaları akışa almak için include_local = false olduğunda, batch_size için açıklama eklendi.
    + @No__t_0, zaman içinde float içeren bir liste yerine float döndüren sorun düzeltildi.
    + Açıkla model kitaplığı 'nda açıklama for benzeme için beklenen değerler oto ml çıktısına eklendi.
    + Ham Özellik önem derecesi almak için dönüşümler bağımsız değişkeni sağlandığında düzeltilen permütasyon özelliği önem derecesi.
    + İnclude_local = false olduğunda açıklama, model explainability kitaplığı için DecisionTreeExplainableModel yürütme süresini geliştirmek üzere toplu iş içindeki genel açıklamaları akışa almak için batch_size eklendi.
  + **azureml-çekirdek**
    + AzureML türünde DBFS veri depoları ekleme özelliği eklendi.
    + @No__t_0 `/` başlatıldığında boş bir klasör oluşturulduğunda, veri deposu yüklemesinin sorunu düzeltildi.
    + İki veri kümesinin etkinleştirilmiş karşılaştırması.
    + Model ve resim silme işlemi şimdi, yukarı akış bağımlılığı nedeniyle silme başarısız olursa, bunlara bağlı yukarı akış nesnelerini alma hakkında daha fazla bilgi sağlamaktadır.
    + Auto_prepare_environment ' de kullanılmayan RunConfiguration ayarı kullanım dışı bırakıldı.
  + **azureml-mlflow**
    + Azureml. mlflow kullanan uzak çalıştırmaların geliştirilmiş kaynak kullanımı.
    + Azureml-mlflow paketinin belgeleri geliştirilmiştir.
    + Mlflow. log _yapay öğelerinden ("my_dir") yapıtları "yapıt-Paths" yerine "my_dir/yapıt-Paths" altına kaydedebileceği sorun düzeltildi.
  + **azureml-işlem hattı-çekirdek**
    + Tüm işlem hattı adımları için hash_paths parametresi kullanım dışıdır ve gelecekte kaldırılacaktır. Varsayılan olarak source_directory içeriği karma hale getirilir (. amlignore veya. gitignore içinde listelenen dosyalar hariç)
    + İşlem türü belirli modülleri desteklemek için modül ve ModuleStep 'i geliştirmeye devam etmek, RunConfiguration tümleştirmesi için hazırlık ve ardışık düzen kullanımlarında kullanımının kilidini açmak için daha fazla değişiklik yapmak.
  + **azureml-işlem hattı-adımlar**
    + AzureBatchStep: giriş/çıkışlarla ilgili olarak Iyileştirilmiş belgeler.
    + AzureBatchStep: delete_batch_job_after_finish varsayılan değeri true olarak değiştirildi.
  + **azureml-tren-çekirdek**
    + Dizeler artık otomatik hiper parametre ayarlama için işlem hedefi olarak kabul edilir.
    + Auto_prepare_environment ' de kullanılmayan RunConfiguration ayarı kullanım dışı bırakıldı.
    + Kullanım dışı bırakılan parametreler `conda_dependencies_file_path` ve `pip_requirements_file_path` sırasıyla `conda_dependencies_file` ve `pip_requirements_file`.
  + **azureml-OPENDATASET 'ler**
    + SPARK olmayan sürümde önemli performansı önemli ölçüde zenginleştirin.

### <a name="azure-machine-learning-data-prep-sdk-v118"></a>Azure Machine Learning Data Prep SDK v 1.1.8

+ **Yeni özellikler**
 + Veri akışı nesneleri artık, bir dizi kayıt üreten şekilde tekrarlandırılır. @No__t_0 için belgelere bakın.

+ **Hata düzeltmeleri ve geliştirmeleri**
 + DataPrep SDK 'sının sağlamlık artışı artırılmıştır.
 + Dize olmayan sütun dizinleriyle Pandas Dataframe 'ler için geliştirilmiş işlem.
 + Veri kümelerinde `to_pandas_dataframe` performansı geliştirildi.
 + Çok düğümlü bir ortamda çalıştırıldığında veri kümelerinin Spark yürütmesinin başarısız olduğu bir hata düzeltildi.

## <a name="2019-07-01"></a>2019-07-01

### <a name="azure-machine-learning-data-prep-sdk-v117"></a>Azure Machine Learning Data Prep SDK v 1.1.7

Azure Databricks kullanan bazı müşterilerle ilgili sorunlara neden olduğundan, performansı destekleyen bir değişikliği geri döndürüyoruz. Azure Databricks bir sorun yaşıyorsanız, aşağıdaki yöntemlerden birini kullanarak sürüm 1.1.7 sürümüne yükseltebilirsiniz:
1. Yükseltmek için bu betiği çalıştırın: `%sh /home/ubuntu/databricks/python/bin/pip install azureml-dataprep==1.1.7`
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
  + Model. Register artık birden çok bireysel dosyanın `child_paths` parametresi kullanımıyla tek bir model olarak kaydedilmesini desteklemektedir.
  
+ **Önizleme özellikleri**
    + Puanlama explainers artık daha güvenilir serileştirme ve seri durumdan çıkarma için Conda ve PIP bilgilerini isteğe bağlı olarak kaydedebilir.
    + Otomatik Özellik Seçicisi için hata düzeltilme.
    + Yeni uygulama tarafından kullanıma sunulan yeni API 'ye ve düzeltme eki uygulanmış hatalara mlflow. azureml. build_image güncelleştirildi.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Azureml-Core ' dan paramıko bağımlılığı kaldırıldı. Eski işlem hedefi iliştirme yöntemlerine yönelik kullanımdan kaldırma uyarıları eklendi.
  + Run. create_children performansını geliştirme
  + İkili sınıflandırıcıyla benzeme açıklama, şekil değerlerini ölçeklendirmek için öğretmen olasılığı kullanıldığında olasılıkların sırasını düzeltir.
  + Otomatik makine öğrenimi için geliştirilmiş hata işleme ve ileti. 
  + Otomatik makine öğrenimi için yineleme zaman aşımı sorunu düzeltildi.
  + Otomatik makine öğrenimi için zaman serisi dönüştürme performansı geliştirildi.

## <a name="2019-06-24"></a>2019-06-24

### <a name="azure-machine-learning-data-prep-sdk-v116"></a>Azure Machine Learning Data Prep SDK v 1.1.6

+ **Yeni özellikler**
  + En üstteki değerler (`SummaryFunction.TOPVALUES`) ve alt değerler (`SummaryFunction.BOTTOMVALUES`) için Özet işlevler eklendi.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + @No__t_0 performansı önemli ölçüde geliştirildi.
  + İkili dosyaları işaret eden bir veri akışında `get_profile()` hatasına neden olan bir hata düzeltildi.
  + Telemetri koleksiyonunun programlı etkinleştirilmesi/devre dışı bırakılması için izin verilen `set_diagnostics_collection()` sunulur.
  + @No__t_0 davranışı değiştirildi. NaN değerleri artık, Pandas davranışıyla hizalanan min, ortalama, STD ve Sum için yok sayılır.


## <a name="2019-06-10"></a>2019-06-10

### <a name="azure-machine-learning-sdk-for-python-v1043"></a>Python v 1.0.43 için SDK Azure Machine Learning

+ **Yeni özellikler**
  + Azure Machine Learning artık popüler makine öğrenimi ve veri analizi çerçevesi Scikit-öğrenme için birinci sınıf destek sağlamaktadır. Kullanıcılar, [`SKLearn` tahmin aracı](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py)kullanarak scikit-öğrenme modellerini kolayca eğitebilir ve dağıtabilir.
    + [Hyperparameter ayarlamayı Scikit ile çalıştırmayı öğrenin-HyperDrive kullanarak öğrenin](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-hyperparameter-tune-deploy-with-sklearn/train-hyperparameter-tune-deploy-with-sklearn.ipynb).
  + Yeniden kullanılabilir işlem birimlerini yönetmek için modül ve ModuleVersion sınıflarıyla birlikte işlem hatlarında ModuleStep oluşturma desteği eklendi.
  + ACI WebServices artık güncelleştirmeler aracılığıyla kalıcı scoring_uri destekliyor. Scoring_uri, IP 'den FQDN 'ye değişecektir. FQDN için DNS ad etiketi, deploy_configuration üzerinde dns_name_label ayarlanarak yapılandırılabilir. 
  + Otomatik makine öğrenimi yeni özellikler:
    + Tahmin için STL featurizer
    + KNE anlama özelliği, özellik için kümeleme etkin
  + AmlCompute kota onayları daha hızlı hale geldi! Artık bir eşik içindeki kota isteklerinizi onaylama işlemini otomatik hale aldık. Kotaların nasıl çalıştığı hakkında daha fazla bilgi için [kotaları yönetme hakkında](https://docs.microsoft.com/azure/machine-learning/service/how-to-manage-quotas)bilgi edinin.

+ **Önizleme özellikleri**
    + Azureml-mlflow paketi ([örnek Not defterleri](https://aka.ms/azureml-mlflow-examples)) aracılığıyla [mlflow](https://mlflow.org) 1.0.0 izleme ile tümleştirme.
    + Jupyter Not defterini çalıştırma olarak gönderme. [API başvuru belgeleri](https://docs.microsoft.com/python/api/azureml-contrib-notebook/azureml.contrib.notebook?view=azure-ml-py)
    + Azureml-contrib-datadrift paketi ([örnek Not defterleri](https://aka.ms/azureml-datadrift-example)) aracılığıyla [veri kayması algılayıcısının](https://docs.microsoft.com/python/api/azureml-contrib-datadrift/azureml.contrib.datadrift?view=azure-ml-py) genel önizlemesi. Veri kayması, model doğruluğunun zamana göre düşmesinin en önemli nedenlerinden biridir. Üretimde modele sunulan veriler, modelin eğitilen verilerden farklı olduğunda gerçekleşir. AML Data değişikliklerini algılayıcısı, müşterinin veri kayması 'nı izlemelerine yardımcı olur ve her bir sürücü algılandığında uyarı gönderir. 

+ **Son değişiklikler**

+ **Hata düzeltmeleri ve geliştirmeleri**
  + RunConfiguration yükleme ve kaydetme, önceki davranış için tam yedekleme ile tam bir dosya yolu belirtmeyi destekler.
  + Serviceprincıpalauthentication içindeki önbelleğe alma, varsayılan olarak kapalıdır.
  + Aynı ölçüm adı altında birden çok çizim günlüğünü etkinleştirin.
  + Model sınıfı artık azureml. Core 'dan (`from azureml.core import Model`) daha uygun bir şekilde alınabilir.
  + İşlem hattı adımlarında `hash_path` parametresi artık kullanım dışıdır. Yeni davranış,. amlignore veya. gitignore içinde listelenen dosyalar hariç karma tamamlanma source_directory.
  + İşlem hattı paketlerinde, çeşitli `get_all` ve `get_all_*` yöntemleri sırasıyla `list` ve `list_*` kullanım dışı bırakılmıştır.
  + azureml. Core. Get _run artık özgün çalışma türünü döndürmeden önce sınıfların içeri aktarılmasını gerektirmez.
  + Bazı WebService güncelleştirmesine yapılan çağrıların bir güncelleştirmeyi tetikleyemediği bir sorun düzeltildi.
  + AKS WebServices üzerinde Puanlama zaman aşımı 5 MS ile 300000ms arasında olmalıdır. Puanlama istekleri için izin verilen maksimum scoring_timeout_ms, 1 dakikadan 5 dakika arasında tamponlanır.
  + LocalWebservice nesnelerinde artık `scoring_uri` ve `swagger_uri` özellikleri var.
  + Taşınan çıkış çıkışları dizin oluşturma ve çıkış Dizin yüklemesi Kullanıcı işleminin dışına çıktı. Her Kullanıcı işleminde çalıştırılacak çalışma geçmişi SDK 'sını etkinleştirdi. Bu, dağıtılmış eğitim çalıştırmaları tarafından karşılaşılan bazı eşitleme sorunlarını çözmelidir.
  + Kullanıcı işlemi adından yazılan azureml günlüğünün adı artık işlem adını (yalnızca dağıtılmış eğitim için) ve PID 'yi içerir.

### <a name="azure-machine-learning-data-prep-sdk-v115"></a>Azure Machine Learning Data Prep SDK v 1.1.5

+ **Hata düzeltmeleri ve geliştirmeleri**
  + 2 basamaklı yıl biçimine sahip yorumlanan tarih saat değerleri için geçerli yıl aralığı Windows 'un yayını ile eşleşecek şekilde güncelleştirilmiştir. Aralık 1930-2029 ' den 1950-2049 ' e değiştirilmiştir.
  + Bir dosya içinde okurken ve `handleQuotedLineBreaks=True` ' ı ayarlarken, `\r` yeni bir satır olarak kabul edilir.
  + Bazı durumlarda `read_pandas_dataframe` başarısız olmasına neden olan bir hata düzeltildi.
  + @No__t_0 performansı geliştirildi.
  + Geliştirilmiş hata iletileri.

## <a name="2019-05-28"></a>2019-05-28

### <a name="azure-machine-learning-data-prep-sdk-v114"></a>Azure Machine Learning Data Prep SDK v 1.1.4

+ **Yeni özellikler**
  + Artık DateTime değerlerini yeni sütunlara ayıklamak ve ayrıştırmak için aşağıdaki ifade dili işlevlerini kullanabilirsiniz.
    + `RegEx.extract_record()` DateTime öğelerini yeni bir sütuna ayıklar.
    + `create_datetime()`, ayrı DateTime öğelerinden DateTime nesneleri oluşturur.
  + @No__t_0 çağırırken, artık değerlerin yaklaşık olarak olduğunu açıkça göstermek için, bu quantile sütunlarının (EST.) olarak etiketlenmiş olduğunu görebilirsiniz.
  + Artık, Azure Blob depolamadan okurken * * glob kullanabilirsiniz.
    + DomainName.  `dprep.read_csv(path='https://yourblob.blob.core.windows.net/yourcontainer/**/data/*.csv')`

+ **Hata düzeltmeleri**
  + Uzak kaynaktan (Azure blob) bir Parquet dosyası okunmayla ilgili bir hata düzeltildi.

## <a name="2019-05-14"></a>2019-05-14

### <a name="azure-machine-learning-sdk-for-python-v1039"></a>Python v 1.0.39 için SDK Azure Machine Learning
+ **Değişikliklerine**
  + Yapılandırma auto_prepare_environment seçeneğini Çalıştır seçeneği, otomatik hazırlama varsayılan olarak devre dışı bırakılmıştır.

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
+ Otomatik Machine Learning, görsel arabirim ve barındırılan Not defteri VM 'Leri içeren Machine Learning hizmeti çalışma alanında yeni yazma bölümü (Önizleme)
    + Otomatik makine öğrenimi kullanarak otomatik olarak model oluşturma 
    + Denemeleri çalıştırmak için sürükle ve bırak görsel arabirimini kullanın
    + Verileri araştırmak, modeller oluşturmak ve hizmetleri dağıtmak için bir not defteri VM 'si oluşturun.
+ Çalışma raporları ve çalışma ayrıntıları sayfalarında canlı grafik ve ölçüm güncelleştirmesi
+ Çalışma ayrıntıları sayfalarındaki Günlükler, çıktılar ve anlık görüntüler için dosya Görüntüleyicisi güncelleştirildi.
+ Denemeleri sekmesinde yeni ve geliştirilmiş rapor oluşturma deneyimi. 
+ Azure Machine Learning çalışma alanının genel bakış sayfasından config. json dosyasını indirme özelliği eklendi.
+ Azure Databricks çalışma alanından hizmet çalışma alanı oluşturma desteği Machine Learning 

## <a name="2019-04-26"></a>2019-04-26

### <a name="azure-machine-learning-sdk-for-python-v1033"></a>Python v 1.0.33 için SDK Azure Machine Learning
+ **Yeni özellikler**
  + _Workspace. Create_ YÖNTEMI artık CPU ve GPU kümeleri için varsayılan küme yapılandırmasını kabul eder.
  + Çalışma alanı oluşturma başarısız olursa, bağımlı kaynaklar temizlenir.
  + Varsayılan Azure Container Registry SKU 'SU temel olarak değiştirildi.
  + Azure Container Registry, çalıştırma veya görüntü oluşturma için gerektiğinde geç oluşturulur.
  + Eğitim çalıştırmaları için ortamlar desteği.

### <a name="notebook-virtual-machine"></a>Not defteri sanal makinesi 

Machine Learning denemeleri 'i programtabileceğiniz, modelleri Web uç noktaları olarak dağıtmak ve Python kullanarak Azure Machine Learning SDK tarafından desteklenen tüm diğer işlemleri gerçekleştirmek için bir not defteri VM 'yi, Jupyter Not defterleri için güvenli, kurumsal kullanıma yönelik bir barındırma ortamı olarak kullanın. Çeşitli yetenekler sağlar:
+ Azure Machine Learning SDK ve ilgili paketlerin en son sürümüne sahip olan  adlı [önceden yapılandırılmış bir not DEFTERI VM 'Sini hızlıca](tutorial-1st-experiment-sdk-setup.md)çalıştırın.
+ Erişim, HTTPS, Azure Active Directory kimlik doğrulaması ve yetkilendirme gibi kanıtlanmış teknolojiler aracılığıyla güvenli hale getirilir.
+ Azure Machine Learning Çalışma Alanı BLOB depolama hesabınızda Not defterlerinin ve kodun güvenilir bulut depolaması. Çalışmanızı kaybetmeden Not defteri sanal makinenizin güvenli bir şekilde silinmesini sağlayabilirsiniz.
+ Azure Machine Learning özellikleri araştırmak ve denemek için önceden yüklenmiş örnek Not defterleri.
+ Azure VM 'lerinin tam özelleştirme özellikleri, herhangi bir VM türü, herhangi bir paket, herhangi bir sürücü. 

## <a name="2019-04-26"></a>2019-04-26

### <a name="azure-machine-learning-sdk-for-python-v1033-released"></a>Python v 1.0.33 için SDK Azure Machine Learning.

+ [Fpgas](concept-accelerate-with-fpgas.md) ÜZERINDE Azure ML hızlandırılmış donanım modelleri genel kullanıma sunulmuştur.
  + Artık, şunları yapmak için [azureml-Accel-modeller paketini kullanabilirsiniz](how-to-deploy-fpga-web-service.md) :
    + Desteklenen bir derin sinir ağının ağırlıklarını (ResNet 50, ResNet 152, DenseNet-121, VGG-16 ve SSD-VGG) eğitin
    + Desteklenen DNN ile aktarım öğrenimi kullanma
    + Modeli Model Yönetimi hizmeti ile kaydedin ve modeli Kapsayıcılı yapın
    + Azure Kubernetes Service (AKS) kümesinde bir FPGA ile modeli bir Azure VM 'ye dağıtma
  + Kapsayıcıyı bir [Azure Data Box Edge](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview) sunucusu cihazına dağıtma
  + Bu [örnekle](https://github.com/Azure-Samples/aml-hardware-accelerated-models) GRPC uç noktasıyla verilerinizi puan edin

### <a name="automated-machine-learning"></a>Otomatik Machine Learning

+ Performansı en iyi duruma getirmek için dinamik olarak dinamik bir şekilde ekleme özelliği sağlar. Yeni Özellikler: iş katıştırıcıları, kanıt ağırlığı, hedef kodlamalar, metin hedefi kodlama, küme uzaklığı
+ Otomatik ML içinde tren/geçerli bölmeler işlemek için akıllı CV
+ Birkaç bellek iyileştirme değişikliği ve çalışma zamanı performans iyileştirmesi
+ Model açıklamasında performans iyileştirmesi
+ Yerel çalıştırma için ONNX model dönüştürmesi
+ Alt örnekleme desteği eklendi
+ Hiçbir çıkış ölçütü tanımlanmadığı zaman akıllı durduruluyor
+ Yığılmış Kümelemeler

+ Zaman serisi tahmin
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

[@No__t_1](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline_endpoint.pipelineendpoint?view=azure-ml-py) , aynı uç noktayı koruyarak yayımlanmış bir işlem hattının yeni bir sürümünü eklemek için tanıtılmıştı.

## <a name="2019-04-17"></a>2019-04-17

### <a name="azure-machine-learning-data-prep-sdk-v112"></a>Azure Machine Learning Data Prep SDK v 1.1.2

Note: Data Prep Python SDK artık `numpy` ve `pandas` paketlerini yüklemecektir. Bkz. [güncelleştirilmiş yükleme yönergeleri](https://aka.ms/aml-data-prep-installation).

+ **Yeni özellikler**
  + Artık Pivot dönüşümünü kullanabilirsiniz.
    + Nasıl yapılır Kılavuzu: [Özet Not defteri](https://aka.ms/aml-data-prep-pivot-nb)
  + Artık yerel işlevlerde normal ifadeleri kullanabilirsiniz.
    + Örnekler:
      + `dflow.filter(dprep.RegEx('pattern').is_match(dflow['column_name']))`
      + `dflow.assert_value('column_name', dprep.RegEx('pattern').is_match(dprep.value))`
  + Artık ifade dilinde  functions `to_lower` `to_upper`  and kullanabilirsiniz.
  + Artık bir veri profilindeki her bir sütunun benzersiz değerlerinin sayısını görebilirsiniz.
  + Yaygın olarak kullanılan okuyucu adımlarının bazıları için artık `infer_column_types` bağımsız değişkenini geçirebilirsiniz. @No__t_0 olarak ayarlandıysa veri hazırlığı, sütun türlerini algılamaya ve otomatik olarak dönüştürmeye çalışır.
    + `inference_arguments` artık kullanım dışıdır.
  + Artık `Dataflow.shape` ' a çağrı yapabilirsiniz.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + `keep_columns`  now, `keep_columns` sonucunun herhangi bir sütun içerecekse kontrol eden ek bir isteğe bağlı bağımsız değişkeni `validate_column_exists` kabul eder.
  + Tüm okuyucu adımları (dosyadan okunan) artık `verify_exists` ek bir isteğe bağlı bağımsız değişkeni kabul eder.
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
  + Azure Machine Learning DNN estimators artık yerleşik çoklu sürüm desteği sağlıyor. Örneğin, `TensorFlow`  estimator artık bir `framework_version` parametresini kabul eder ve kullanıcılar ' 1,10 ' veya ' 1,12 ' sürümünü belirtebilir. Geçerli SDK sürümleriniz tarafından desteklenen sürümlerin bir listesi için, istenen Framework sınıfında `get_supported_versions()` çağırın (örneğin, `TensorFlow.get_supported_versions()`).
  En son SDK sürümü tarafından desteklenen sürümlerin bir listesi için [DNN Estimator belgelerine](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn?view=azure-ml-py)bakın.

### <a name="azure-machine-learning-data-prep-sdk-v111"></a>Azure Machine Learning Data Prep SDK v 1.1.1

+ **Yeni özellikler**
  + Read_ * dönüşümler kullanarak birden çok veri deposu/veri yolu/DataReference kaynağını okuyabilirsiniz.
  + Yeni bir sütun oluşturmak için sütunlar üzerinde aşağıdaki işlemleri gerçekleştirebilirsiniz: bölme, kat, modül, güç, uzunluk.
  + Veri hazırlığı artık Azure ML tanılama paketinin bir parçasıdır ve tanılama bilgilerini varsayılan olarak günlüğe kaydeder.
    + Bunu kapatmak için, bu ortam değişkenini true olarak ayarlayın: DISABLE_DPREP_LOGGER

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Yaygın olarak kullanılan sınıflar ve işlevler için geliştirilmiş kod belgeleri.
  + Auto_read_file içinde Excel dosyalarını okuyameyen bir hata düzeltildi.
  + Read_pandas_dataframe içindeki klasörün üzerine yazma seçeneği eklendi.
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

+ **Son değişiklikler**
  + Veri hazırlama paketinin kavramı kullanım dışı bırakılmış ve artık desteklenmiyor. Tek bir pakette birden çok veri akışını kalıcı hale getirmek yerine, veri akışlarını ayrı ayrı kalıcı hale getirebilirsiniz.
    + Nasıl yapılır Kılavuzu: [veri akışları Not defterini açma ve kaydetme](https://aka.ms/aml-data-prep-open-save-dataflows-nb)

+ **Yeni özellikler**
  + Veri hazırlama artık belirli bir anlam türüyle eşleşen sütunları tanıyabilir ve buna uygun şekilde ayrılabilir. Şu anda desteklenen STypes 'ler şunlardır: e-posta adresi, Coğrafi Koordinatlar (Enlem & Boylam), IPv4 ve IPv6 adresleri, ABD telefon numarası ve ABD ZIP kodu.
    + Nasıl yapılır Kılavuzu: [anlamsal türler Not defteri](https://aka.ms/aml-data-prep-semantic-types-nb)
  + Veri hazırlığı artık iki sayısal sütundan bir sonuç sütunu oluşturmak için aşağıdaki işlemleri destekler: çıkarma, çarpma, bölme ve mod.
  + Veri akışının yürütüldüğünde kayıt oluşturulup oluşturulmayacağını denetlemek için bir veri akışında `verify_has_data()` çağırabilirsiniz.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Artık sayısal sütun profilleri için bir histogram içinde kullanılacak bölme sayısını belirtebilirsiniz.
  + @No__t_0 Transform artık veri çerçevesinin dize veya bayt türünde sütun adlarına sahip olmasını gerektirir.
  + Sütun eksik ise değerlerin doğru doldurulmadığını `fill_nulls` dönüşümde bir hata düzeltildi.

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

  + Azure Machine Learning artık popüler DNN Framework Chainer için birinci sınıf destek sağlar. [@No__t_1](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) sınıf kullanıcıları, Chainer modellerini kolayca eğitebilir ve dağıtabilir.
    + [Chaeylemsizlik ile dağıtılmış eğitime nasıl çalıştırılacağını](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/distributed-chainer/distributed-chainer.ipynb) öğrenin
    + [Hyperparameter ayarlamayı hiper sürücü kullanarak Chainer ile çalıştırma](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-chainer/train-hyperparameter-tune-deploy-with-chainer.ipynb) hakkında bilgi edinin
  + Azure Machine Learning işlem hatları ekleme özelliği, veri deposu değişikliklerine göre bir işlem hattı çalıştırmasını tetikler. İşlem hattı [zamanlama Not defteri](https://aka.ms/pl-schedule) , bu özelliği göstermek için güncelleştirilir.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Source_directory_data_store özelliğini, [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep?view=azure-ml-py)Için sağlanan [runconfigurations](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.runconfiguration?view=azure-ml-py) 'lar üzerinde istenen veri deposuna (BLOB depolama gibi) ayarlamaya yönelik destek Azure Machine Learning işlem hatları ekledik. Varsayılan olarak, çok sayıda adım eşzamanlı olarak yürütüldüğünde sorunları azaltma sorunlarını ortadan kaldırarak Azure dosya deposunu, yedekleme veri deposu olarak kullanır.

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
  +  DataTranferStep, Azure SQL Server ve PostgreSQL için Azure veritabanı ([Not defteri](https://aka.ms/pl-data-trans)) ile çalışacak şekilde güncelleştirildi.

+ **Değişikliklerine**
  + @No__t_1 kullanım için kullanım dışı `PublishedPipeline.get_published_pipeline`.
  + @No__t_1 kullanım için kullanım dışı `Schedule.get_schedule`.

### <a name="azure-machine-learning-data-prep-sdk-v1012"></a>Azure Machine Learning Data Prep SDK v 1.0.12

+ **Yeni özellikler**
  + Veri hazırlama artık, veri deposu kullanarak bir Azure SQL veritabanından okumayı destekler.
 
+ **Değişikliklerine**
  + Büyük veriler üzerinde belirli işlemlerin bellek performansı geliştirilmiştir.
  + `read_pandas_dataframe()` artık `temp_folder` belirtilmesini gerektirir.
  + @No__t_1 `name` özelliği kullanım dışı bırakıldı. bunun yerine `column_name` kullanın.

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
  + `to_bool` işlev artık eşleşmeyen değerlerin hata değerlerine dönüştürülmesine izin veriyor. Bu, `to_bool` ve `set_column_types` için yeni varsayılan uyumsuzluk davranışıdır, ancak önceki varsayılan davranış eşleşmeyen değerleri yanlış olarak dönüştürmelidir.
  + @No__t_0 çağrılırken, sayısal sütunlardaki null/eksik değerleri NaN olarak yorumlamak için yeni bir seçenek vardır.
  + Tür tutarlılığı sağlamak ve erken yük devretmek için bazı ifadelerin dönüş türünü denetleme özelliği eklendi.
  + Artık, bir sütundaki değerleri JSON nesneleri olarak ayrıştırmak ve birden çok sütuna genişletmek için `parse_json` çağırabilirsiniz.

+ **Hata düzeltmeleri**
  + Python 3.5.2 'te `set_column_types` kilitlenen bir hata düzeltildi.
  + AML görüntüsü kullanılarak veri deposuna bağlanılırken kilitlenen hata düzeltildi.

+ **Güncelleştirmeler**
  * Başlarken öğreticileri, örnek olay incelemeleri ve nasıl yapılır kılavuzlarından oluşan [örnek Not defterleri](https://aka.ms/aml-data-prep-notebooks) .

## <a name="2018-12-04-general-availability"></a>2018-12-04: genel kullanılabilirlik

Azure Machine Learning genel kullanıma sunulmuştur.

### <a name="azure-machine-learning-compute"></a>Azure Machine Learning Işlem
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
+ **Son değişiklikler**
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
+ **Son değişiklikler** 
  * `SummaryFunction.N` `SummaryFunction.Count` olarak yeniden adlandırıldı.
  
+ **Hata düzeltmeleri**
  * Uzak çalışmalardaki veri depolarından okuma ve yazma yaparken en son AML çalıştırma belirtecini kullanın. Daha önce, AML çalıştırma belirteci Python 'da güncelleştirilirse, Data Prep çalışma zamanı güncelleştirilmiş AML çalıştırma belirteciyle güncellenmez.
  * Ek daha net hata iletileri
  * Spark `Kryo` serileştirme kullandığında to_spark_dataframe () artık kilitlenmeyecektir
  * Değer sayısı denetçisi artık 1000 ' den fazla benzersiz değer gösteriyor olabilir
  * Özgün veri akışının adı yoksa rastgele bölme artık başarısız olmaz  

+ **Daha fazla bilgi**
  * [Azure Machine Learning Veri Hazırlama SDK'sı](https://aka.ms/data-prep-sdk)

### <a name="docs-and-notebooks"></a>Belgeler ve Not defterleri
+ ML işlem hatları
  + İşlem hatları, toplu iş kapsamları ve stil aktarma örnekleri ile çalışmaya başlama için yeni ve güncelleştirilmiş Not defterleri: https://aka.ms/aml-pipeline-notebooks
  + [İlk işlem hattınızı oluşturmayı](how-to-create-your-first-pipeline.md) öğrenin
  + [İşlem hatlarını kullanarak toplu tahmine dayalı tahminleri nasıl çalıştıracağınızı](how-to-run-batch-predictions.md) öğrenin
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

+ **Son değişiklikler** 
  * *azureml. tren. pencere* öğesi ad alanı, *azureml. pencere öğeleri*'ne taşındı.
  * *azureml. Core. COMPUTE. AmlCompute* aşağıdaki sınıfları kullanımdan kaldırır- *azureml. Core. COMPUTE. Batchaıute* ve *azureml. Core. COMPUTE. dsvmcompute*. Sonraki sürümlerde ikinci sınıf kaldırılacak. AmlCompute sınıfının şimdi daha kolay bir tanımı vardır ve yalnızca bir vm_size ve max_nodes gerekir ve bir iş gönderildiğinde kümenizi otomatik olarak 0 ' dan max_nodes ' ye ölçeklendirecektir. [Örnek Not defterleriniz](https://github.com/Azure/MachineLearningNotebooks/tree/master/training) bu bilgilerle güncelleştirildi ve size kullanım örnekleri vermelidir. Bu basitleştirme ve çok sayıda daha heyecan verici özelliğin daha sonraki bir sürümde gelmesini umuyoruz!

### <a name="azure-machine-learning-data-prep-sdk-v051"></a>Azure Machine Learning Data Prep SDK v 0.5.1 

[Başvuru belgelerini](https://aka.ms/data-prep-sdk)okuyarak veri hazırlığı SDK 'sı hakkında daha fazla bilgi edinin.
+ **Yeni özellikler**
   * DataPrep paketlerini yürütmek ve bir veri kümesi veya veri akışı için veri profilini görüntülemek üzere yeni bir DataPrep CLı oluşturuldu
   * Kullanılabilirliği artırmak için SetColumnType API yeniden tasarlandı
   * Smart_read_file, auto_read_file olarak yeniden adlandırıldı
   * Artık veri profilinde skew ve basıklığını içerir
   * , Bağlı örneklemeyle örnek oluşturabilir
   * CSV dosyaları içeren ZIP dosyalarından okuyabilir
   * Veri kümeleri satır temelinde rastgele bölme ile bölebilir (örneğin, test-tren kümelerine)
   * Bir veri akışından veya bir veri profilinden tüm sütun veri türlerini `.dtypes` çağırarak alabilir
   * @No__t_0 çağırarak bir veri akışından veya bir veri profilinden satır sayısı alabilir

+ **Hata düzeltmeleri**
   * Uzun-Double dönüşümü düzeltildi 
   * Herhangi bir sütundan sonra düzeltilen onaylama 
   * Belirsizlik Gruplandırmayla ilgili bir sorun düzeltildi ve bazı durumlarda grupları algılayamayacağı
   * Çok sütunlu sıralama sırasına göre sabit sıralama işlevi
   * @No__t_0 nasıl işleyeceğinize benzer sabit ve/veya ifadeler
   * DBFS yolundan düzeltilen okuma
   * Hata iletileri daha fazla anlaşılır hale getirilir 
   * Artık AML belirtecini kullanarak uzak işlem hedefinde okurken artık başarısız olmaz
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

+ **Son değişiklikler** 
  * \* Workspace. compute_targets, datamağazalar, denemeleri, görüntüler, modeller ve *WebServices* , Yöntemler yerine özelliklerdir. Örneğin, Workspace. *compute_targets ()* çalışma alanı *. compute_targets*ile değiştirin.
  * *Çalıştır. Get _context* itiraz *Çalıştır. Get _submitted_run*. İkinci yöntem sonraki sürümlerde kaldırılacak.
  * *Pipelinedata* sınıfı artık datastore_name yerine bir parametre olarak bir veri deposu nesnesi bekliyor. Benzer şekilde, işlem *hattı* default_datastore_name yerine default_datastore kabul eder.

+ **Yeni özellikler**
  * Azure Machine Learning ardışık düzen [örnek Not defteri](https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline/pipeline-mpi-batch-prediction.ipynb) artık MPI adımlarını kullanır.
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
  * Smart_read içindeki düz metin dosyaları için hatalı metin etiketi düzeltildi
  * Dize sütunu türü artık ölçümler görünümünde gösteriliyor
  * Tür sayısı şimdi tek tek bir bağımsız olarak tek bir 1.
  * Yol bir dize olarak sağlandığında Write_to_csv artık başarısız olur
  * Değiştirme kullanılırken, "bul" boş bırakılması artık başarısız olmayacaktır 

## <a name="2018-10-12"></a>2018-10-12

### <a name="azure-machine-learning-sdk-for-python-v0168"></a>Python v 0.1.68 için SDK Azure Machine Learning

+ **Yeni özellikler**
  * Yeni çalışma alanı oluşturulurken çok kiracılı destek.

+ **Düzeltilen hatalar**
  * Web hizmetini dağıtmaya yönelik olarak pynacl kitaplığı sürümünü Sabitlemeye gerek kalmaz.

### <a name="azure-machine-learning-data-prep-sdk-v030"></a>Azure Machine Learning Data Prep SDK v 0.3.0

+ **Yeni özellikler**
  * Transform_partition_with_file (script_path) yöntemi eklendi, bu, kullanıcıların yürütülecek bir Python dosyasının yolunu geçmesine izin verir

## <a name="2018-10-01"></a>2018-10-01

### <a name="azure-machine-learning-sdk-for-python-v0165"></a>Python v 0.1.65 için SDK Azure Machine Learning
[Sürüm 0.1.65](https://pypi.org/project/azureml-sdk/0.1.65) yeni özellikler, daha fazla belge, hata düzeltmeleri ve daha fazla [örnek Not defteri](https://aka.ms/aml-notebooks)içerir.

Bilinen hatalar ve geçici çözümler hakkında bilgi edinmek için [bilinen sorunlar listesine](resource-known-issues.md) bakın.

+ **Son değişiklikler**
  * Workspace. denemeleri, Workspace. modeller, Workspace. compute_targets, Workspace. Images, Workspace. Web _services, daha önce döndürülen liste sözlüğünü döndürüyor. Bkz. [azureml. Core. Workspace](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py) API 'si belgeleri.

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
  * Ensembling yineleme sorunları düzeltildi.
  * MAC OS 'ta hata ile asılı eğitim hatası.
  * Altörnekleme makrosu ortalama PR/ROC eğrisi özel doğrulama senaryosunda.
  * Ek dizin mantığı kaldırıldı.
  * Get_output API 'sinden filtre kaldırıldı.

+ **İşlem hatları**
  * Önce bir yürütme çalıştırması gerekmeden bir işlem hattını doğrudan yayımlamak için bir yöntem işlem hattı. Publish () eklendi.   
  * Yayımlanmış bir işlem hattından oluşturulan işlem hattı çalıştırmalarını getirmek için bir ardışık düzen eylemsizlik. Get _pipeline_çalıştırmaları () eklendi.

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

Azure Machine Learning yeni, yenilenmiş bir sürümü: Bu sürüm hakkında daha fazla bilgi edinin: https://azure.microsoft.com/blog/what-s-new-in-azure-machine-learning-service/


## <a name="next-steps"></a>Sonraki adımlar

[Azure Machine Learning](../service/overview-what-is-azure-ml.md)’e genel bakışı okuyun.
