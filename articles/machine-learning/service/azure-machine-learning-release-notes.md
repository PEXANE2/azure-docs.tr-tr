---
title: Yayındaki yenilikler nelerdir?
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmeti için en son güncelleştirmeler ve Machine Learning ve Data Prep Python SDK 'Ları hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
ms.author: jmartens
author: j-martens
ms.date: 07/25/2019
ms.custom: seodec18
ms.openlocfilehash: 684a84431c8348ddafab8cefbe831c2b58c3cee6
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69639939"
---
# <a name="azure-machine-learning-service-release-notes"></a>Azure Machine Learning hizmeti sürüm notları

Bu makalede, Azure Machine Learning hizmet sürümleri hakkında bilgi edinin.  Tam SDK başvuru içeriği için Azure Machine Learning [**Python başvurusu için ana SDK**](https://aka.ms/aml-sdk) sayfasına gidin.

Bkz: [bilinen sorunların listesi](resource-known-issues.md) bilinen hataların ve geçici çözümleri hakkında bilgi edinmek için.

## <a name="2019-08-19"></a>2019-08-19

### <a name="azure-machine-learning-sdk-for-python-v1057"></a>Python v 1.0.57 için SDK Azure Machine Learning
+ **Yeni Özellikler**
  + Automatedml tarafından tüketilebilmesi için etkinleştirildi `TabularDataset` . Hakkında `TabularDataset`daha fazla bilgi edinmek için lütfen https://aka.ms/azureml/howto/createdatasets adresini ziyaret edin.
  
+ **Hata düzeltmeleri ve geliştirmeleri**
  + **Oto ml-Client-Core-NativeClient**
    + Bir hata düzeltildi, eğitim ve/veya doğrulama etiketleri (y ve y_valid), Pandas dataframe biçiminde sağlandığı halde bir sayısal tuş y dizisi olarak değil, ortaya çıktı.
    + Yalnızca verileri `RawDataContext` `AutoMLBaseSettings` ve nesneyi gerektirecek şekilde oluşturmak için arabirimi güncelleştirildi.
    +  Oto ml kullanıcılarına tahmin sırasında yeterince uzun olmayan eğitim serisini bırakmaya izin verin. -Oto ml kullanıcılarına, tahmin sırasında eğitim kümesinde mevcut olmayan test kümesinden graıns 'yi bırakması izin verin.
  + **Azure-CLI-ml**
    + Artık, Microsoft tarafından oluşturulan ve müşteri sertifikası için AKS kümesinde dağıtılan Puanlama uç noktası için SSL sertifikasını güncelleştirebilirsiniz.
  + **azureml-oto ml-çekirdek**
    + Eksik etiketlere sahip satırların düzgün şekilde kaldırılmadığı, oto ml 'deki bir sorun düzeltildi.
    + Oto ml 'de geliştirilmiş hata günlüğü; Tam hata iletileri artık her zaman günlük dosyasına yazılır.
    + Oto, `azureml-defaults` `azureml-explain-model`, ve`azureml-dataprep`dahil olmak üzere paket sabitlenmesini güncelleştirmiştir. Oto, paket uyuşmazlıkları ( `azureml-train-automl` paket hariç) üzerinde artık uyarı vermez.
    + Zaman serisi 'te CV bölünmesinin eşit olmadığı, küme hesaplamasının başarısız olmasına neden olan bir sorun düzeltildi.
    + Çapraz doğrulama eğitim türü için ensebir yineleme çalıştırırken, veri kümesinin tamamında eğitilen modelleri indirirken sorun yaşadığımızda, model ağırlıkları ve oylama 'ye beslendiği modeller arasında bir tutarsızlık vardı topluluğu.
    + Bir hata düzeltildi, eğitim ve/veya doğrulama etiketleri (y ve y_valid), Pandas dataframe biçiminde sağlandığı halde bir sayısal tuş y dizisi olarak değil, ortaya çıktı.
    + Giriş tablolarının Boole sütunlarında hiç karşılaşılmadığı zaman tahmin görevlerinde sorun düzeltildi.
    + Oto ml kullanıcılarına tahmin sırasında yeterince uzun olmayan eğitim serisini bırakmaya izin verin. -Oto ml kullanıcılarına, tahmin sırasında eğitim kümesinde mevcut olmayan test kümesinden graıns 'yi bırakması izin verin.
  + **azureml-çekirdek**
    + Blob_cache_timeout parametresi sıralaması ile ilgili sorun düzeltildi.
    + Dış sığdırma ve özel durum türlerini sistem hatalarına ekledi.
    + Uzak çalıştırmalar için Key Vault gizli dizileri için destek eklendi. Çalışma alanınızda ilişkili olan anahtar kasasından gizli dizileri eklemek, almak ve listelemek için bir azureml. Core. keykasa. Keykasası sınıfı ekleyin. Desteklenen işlemler şunlardır:
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
    + Dosya izleyicisinden, ilk belirtecinin süresi dolduktan sonra karşıya yükleme durdurmasına neden olan, süresi dolan son SAS belirteç sorunu düzeltildi.
    + Veri kümesi Python SDK 'sında HTTP CSV/TSV dosyalarını içeri aktarma destekleniyor.
    + Workspace. Setup () yöntemi kullanımdan kaldırıldı. Kullanıcılara gösterilen uyarı iletisi, bunun yerine Create () veya Get ()/from_config () kullanmayı önerir.
    + Ortam eklendi. özel özel Python paketlerinin (. WHL) çalışma alanına yüklenmesini sağlayan ve ortamı derlemek/denemek için güvenli bir şekilde kullanarak, _private_pip_wheel () ekleyin.
    + Artık, Microsoft tarafından oluşturulan ve müşteri sertifikası için AKS kümesinde dağıtılan Puanlama uç noktası için SSL sertifikasını güncelleştirebilirsiniz.
  + **azureml-açıkla-model**
    + Karşıya yükleme ile ilgili açıklamaları bir model KIMLIĞI eklemek için parametresi eklendi.
    + Bellekteki `is_raw` açıklamaları etiketleme ve karşıya yükleme eklendi.
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
  + **azureml-tren-çekirdek**
    + PyTorch Estimator 'da Torch 1,2 desteği eklenmiştir.
  + **azureml-pencere öğeleri**
    + Sınıflandırma eğitimi için geliştirilmiş karışıklık matrisi grafikleri.

### <a name="azure-portal"></a>Azure Portal
+ **Önizleme özelliği**
  + Günlük ve çıkış dosyası akışı artık çalışma ayrıntıları sayfaları için kullanılabilir. Dosyalar, önizleme geçişi açık olduğunda güncelleştirmeleri gerçek zamanlı olarak akışa alır.

## <a name="2019-08-05"></a>2019-08-05

### <a name="azure-machine-learning-sdk-for-python-v1055"></a>Python v 1.0.55 için SDK Azure Machine Learning

+ **Yeni Özellikler**
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
    + Pencere öğesi değişiklikleri: Yalnızca genel özellik önem derecesine sahip contrib, başka bir nbextension install/Enable-support açıklaması olmadan otomatik olarak yüklenir (örn. permütasyon)
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
    + Blob_cache_timeout parametresi `Datastore.register_azure_blob_container`eklendi.
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
    + Belgeler, ve için `max_cores_per_iteration` daha uygun dile güncelleştirildi`max_concurrent_iterations`
    + Uzak çalışmalarla ilgili tahmin görevlerinin günlüğe kaydedilmesi, çalışma başarısız olursa, artık kullanıcı kapsamlı bir hata iletisiyle sunulmaktadır.
    + Get_Data, ardışık düzen AutoSize Not defterinden kaldırıldı.
    + Oto mlstep 'te dataprep desteği başlatıldı.

### <a name="azure-machine-learning-data-prep-sdk-v1110"></a>Azure Machine Learning Data Prep SDK v 1.1.10

+ **Yeni Özellikler**
  + Artık belirli bir sütunda belirli Denetçiler (örn. histogram, dağılım çizimi vb.) yürütmeyi isteyebilirsiniz.
  + İçin `append_columns`bir paralel hale getirmek bağımsız değişkeni eklendi. Doğru ise, veriler belleğe yüklenir ancak yürütme paralel olarak çalıştırılır; Yanlış ise, yürütme akış, ancak tek iş parçacıklı olur.

## <a name="2019-07-23"></a>2019-07-23

### <a name="azure-machine-learning-sdk-for-python-v1053"></a>Python v 1.0.53 için SDK Azure Machine Learning

+ **Yeni Özellikler**
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
    + Tahmin görevlerinde, `target_lags` parametresi artık tek bir tamsayı değeri ya da tamsayılar listesini kabul eder. Tamsayı sağlanmışsa, yalnızca bir gecikme oluşturulur. Bir liste sağlanmışsa, lags 'nin benzersiz değerleri alınacaktır. target_lags = [1, 2, 2, 4] bir, 2 ve 4 dönem için lags oluşturacak.
    + Dönüşümden sonra sütun türlerini kaybetme hakkındaki hatayı düzeltir (hata bağlandı);
    + ' `model.forecast(X, y_query)`De, y_query ' ın Begin (#459519) sırasında None (öğeleri) içeren bir nesne türü olmasını sağlar.
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
    + İle `target_path` başlatılmışsa boş bir klasörün oluşturulduğu veri deposu yüklemesine hata düzeltildi`/`
    + Serviceprincıpalauthentication içinde sabit ayrıntılı kopyalama sorunu düzeltildi.
    + CLı 'ya "az ml Environment Show" ve "az ml Environment List" komutları eklendi.
    + Ortamlar artık zaten derlenmiş bir base_image için alternatif olarak bir base_dockerfile belirtmeyi desteklemektedir.
    + Kullanılmayan RunConfiguration ayarı auto_prepare_environment kullanım dışı olarak işaretlendi.
    + Model açıklaması artık kayıt sonrasında güncelleştirilebilen
    + Hata düzeltmesi Model ve resim silme işlemi şimdi, yukarı akış bağımlılığı nedeniyle silme başarısız olursa, bunlara bağlı yukarı akış nesnelerini alma hakkında daha fazla bilgi sağlamaktadır.
    + Bazı ortamlar için bir çalışma alanı oluşturulurken gerçekleşen dağıtımlar için boş süre yazdırılan hata düzeltildi.
    + İyileştirilmiş çalışma alanı hata özel durumları oluşturma. Kullanıcılar "çalışma alanı oluşturulamıyor. Bulunamıyor... " ileti olarak ve bunun yerine gerçek oluşturma hatası ' nı görürsünüz.
    + AKS WebServices içinde belirteç kimlik doğrulaması desteği ekleyin. 
    + Nesnelere`Webservice` yöntem ekleyin. `get_token()`
    + Machine Learning veri kümelerini yönetmek için CLı desteği eklendi.
    + `Datastore.register_azure_blob_container`Artık isteğe bağlı olarak `blob_cache_timeout` , bu veri deposu için önbellek süre sonunu etkinleştirmek üzere blobsigortası 'nin Mount parametrelerini yapılandıran bir değer (saniye cinsinden) alır. Varsayılan değer zaman aşımı değildir, yani bir blob okunarak iş bitene kadar yerel önbellekte kalır. Çoğu iş bu ayarı tercih eder, ancak bazı işlerin, düğümlerine sığacak kadar büyük bir veri kümesinden daha fazla veri okuması gerekir. Bu işler için, bu parametrenin ayarlanması başarılı olur. Bu parametreyi ayarlarken dikkatli olmanız gerekir: değeri çok düşük olarak ayarlamak, bir dönem içinde kullanılan verilerin yeniden kullanılmadan önce süresinin dolmasına neden olabilir. Bu, tüm okumaların, eğitim sürelerini olumsuz yönde etkileyen yerel önbellek yerine blob depolamadan (ağ) yapılacağı anlamına gelir.
    + Model açıklaması artık kayıt sonrasında düzgün şekilde güncelleştirilebilen olabilir
    + Model ve resim silme artık, silinmesine bağlı yukarı akış nesneleri hakkında daha fazla bilgi sağlar ve bu da silmenin başarısız olmasına neden olur
    + Azureml. mlflow kullanarak uzak çalıştırmaların kaynak kullanımını geliştirme.
  + **azureml-açıkla-model**
    + Azureml-contrib-açıkla-model paketindeki ham özellik için LIME açıklama için sabit dönüşümler bağımsız değişkeni
    + LimeExplainer için SciPy seyrek desteği ekleme
    + Doğrusal modelleri açıklayan, Shap doğrusal açıklama sarmalayıcı ve tabular açıklama 'e başka bir düzey eklendi
    + Model kitaplığı 'nda açıklama benzeklik için, include_local = false olduğunda, seyrek veri girişi için hata düzeltildi
    + beklenen değerleri, oto ml çıktısına Ekle
    + ham Özellik önem derecesi almak için dönüşümler bağımsız değişkeni sağlandığında düzeltilen permütasyon özelliği önem derecesi
    + include_local = false olduğunda açıklama 'ın yürütme süresini geliştirmek için toplu iş içindeki genel açıklamaları akışa almak için batch_size ekleyin
    + Model explainability kitaplığı için, Pandas dataframe girişinin tahmin için gerekli olduğu sabit kara Box explainers
    + Bazen içinde float içeren `explanation.expected_values` bir liste yerine float döndüren bir hata düzeltildi.
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
    + beklenen değerleri, oto ml çıktısına Ekle
  + **azureml-tren-çekirdek**
    + Dizeler artık otomatik hiper parametre ayarlama için işlem hedefi olarak kabul edilir
    + Kullanılmayan RunConfiguration ayarı auto_prepare_environment kullanım dışı olarak işaretlendi.

### <a name="azure-machine-learning-data-prep-sdk-v119"></a>Azure Machine Learning Data Prep SDK v 1.1.9

+ **Yeni Özellikler**
  + Bir dosyayı doğrudan http veya https URL 'sinden okumak için destek eklendi.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Uzak bir kaynaktan bir Parquet veri kümesini okumaya çalışırken (Şu anda desteklenmeyen), geliştirilmiş hata iletisi.
  + ADLS Gen 2 ' de Parquet dosya biçimine yazarken hata düzeltildi ve yoldaki ADLS Gen 2 kapsayıcı adı güncelleştiriliyor.

## <a name="2019-07-09"></a>2019-07-09

### <a name="visual-interface"></a>Görsel arabirim
+ **Önizleme özellikleri**
  + Görsel arabirimde "R betiği Yürüt" modülü eklendi.

### <a name="azure-machine-learning-sdk-for-python-v1048"></a>Python v 1.0.48 için SDK Azure Machine Learning

+ **Yeni Özellikler**
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
    + Bazen içinde float içeren `explanation.expected_values` bir liste yerine float döndüren sorun düzeltildi.
    + Açıkla model kitaplığı 'nda açıklama for benzeme için beklenen değerler oto ml çıktısına eklendi.
    + Ham Özellik önem derecesi almak için dönüşümler bağımsız değişkeni sağlandığında düzeltilen permütasyon özelliği önem derecesi.
    + İnclude_local = false olduğunda açıklama, model explainability kitaplığı için DecisionTreeExplainableModel yürütme süresini geliştirmek üzere toplu iş içindeki genel açıklamaları akışa almak için batch_size eklendi.
  + **azureml-çekirdek**
    + AzureML türünde DBFS veri depoları ekleme özelliği eklendi.
    + `target_path` İle`/`başlatılmışsa boş bir klasörün oluşturulduğu veri deposu yükleme sorunu düzeltildi.
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
    + AzureBatchStep: Giriş/çıkışlarla ilgili olarak geliştirilmiş belgeler.
    + AzureBatchStep: Delete_batch_job_after_finish varsayılan değeri true olarak değiştirildi.
  + **azureml-tren-çekirdek**
    + Dizeler artık otomatik hiper parametre ayarlama için işlem hedefi olarak kabul edilir.
    + Auto_prepare_environment ' de kullanılmayan RunConfiguration ayarı kullanım dışı bırakıldı.
    + Kullanım dışı `conda_dependencies_file_path` bırakılan `pip_requirements_file_path` parametreler ve `conda_dependencies_file` sırasıyla`pip_requirements_file` .
  + **azureml-OPENDATASET 'ler**
    + SPARK olmayan sürümde önemli performansı önemli ölçüde zenginleştirin.

### <a name="azure-machine-learning-data-prep-sdk-v118"></a>Azure Machine Learning Data Prep SDK v 1.1.8

+ **Yeni Özellikler**
 + Veri akışı nesneleri artık, bir dizi kayıt üreten şekilde tekrarlandırılır. İçin `Dataflow.to_record_iterator`belgelerine bakın.

+ **Hata düzeltmeleri ve geliştirmeleri**
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

+ **Yeni Özellikler**
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
  + Azureml-Core ' dan paramıko bağımlılığı kaldırıldı. Eski işlem hedefi iliştirme yöntemlerine yönelik kullanımdan kaldırma uyarıları eklendi.
  + Run. create_children performansını geliştirme
  + İkili sınıflandırıcıyla benzeme açıklama, Shap değerlerini ölçeklendirmek için öğretmen olasılığı kullanıldığında olasılıkların sırasını düzeltir
  + Otomatik makine öğrenimi için geliştirilmiş hata işleme ve ileti. 
  + Otomatik makine öğrenimi için yineleme zaman aşımı sorunu düzeltildi.
  + Otomatik makine öğrenimi için zaman serisi dönüştürme performansı geliştirildi.

## <a name="2019-06-24"></a>2019-06-24

### <a name="azure-machine-learning-data-prep-sdk-v116"></a>Azure Machine Learning Data Prep SDK v 1.1.6

+ **Yeni Özellikler**
  + Top values (`SummaryFunction.TOPVALUES`) ve Bottom values (`SummaryFunction.BOTTOMVALUES`) için özet işlevleri eklendi.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Performansı önemli ölçüde iyileştirildi `read_pandas_dataframe`.
  + İkili dosyaları işaret eden bir veri `get_profile()` akışının başarısız olmasına neden olan bir hata düzeltildi.
  + Telemetri `set_diagnostics_collection()` koleksiyonunun programlı bir şekilde etkinleştirilmesi/devre dışı bırakılmasına olanak tanımak için sunulur.
  + Davranışı `get_profile()`değiştirildi. NaN değerleri artık, Pandas davranışıyla hizalanan min, ortalama, STD ve Sum için yok sayılır.


## <a name="2019-06-10"></a>2019-06-10

### <a name="azure-machine-learning-sdk-for-python-v1043"></a>Python v 1.0.43 için SDK Azure Machine Learning

+ **Yeni Özellikler**
  + Azure Machine Learning artık popüler makine öğrenimi ve veri analizi çerçevesi Scikit-öğrenme için birinci sınıf destek sağlamaktadır. Kullanıcılar, [ `SKLearn` tahmin aracı](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py)kullanarak scikit-öğrenme modellerini kolayca eğitebilir ve dağıtabilir.
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

+ **Bozucu değişiklikler**

+ **Hata düzeltmeleri ve geliştirmeleri**
  + RunConfiguration yükleme ve kaydetme, önceki davranış için tam yedekleme ile tam bir dosya yolu belirtmeyi destekler.
  + Serviceprincıpalauthentication içindeki önbelleğe alma, varsayılan olarak kapalıdır.
  + Aynı ölçüm adı altında birden çok çizim günlüğünü etkinleştirin.
  + Model sınıfı artık azureml. Core (`from azureml.core import Model`) öğesinden daha uygun bir şekilde alınabilir.
  + İşlem hattı adımlarında, `hash_path` parametre artık kullanım dışıdır. Yeni davranış,. amlignore veya. gitignore içinde listelenen dosyalar hariç karma tamamlanma source_directory.
  + İşlem hattı paketlerinde, çeşitli `get_all` ve `get_all_*` yöntemleri sırasıyla `list` ve `list_*`için kullanım dışı bırakılmıştır.
  + azureml. Core. Get _run artık özgün çalışma türünü döndürmeden önce sınıfların içeri aktarılmasını gerektirmez.
  + Bazı WebService güncelleştirmesine yapılan çağrıların bir güncelleştirmeyi tetikleyemediği bir sorun düzeltildi.
  + AKS WebServices üzerinde Puanlama zaman aşımı 5 MS ile 300000ms arasında olmalıdır. Puanlama istekleri için izin verilen maksimum scoring_timeout_ms, 1 dakikadan 5 dakika arasında tamponlanır.
  + Localwebservice nesnelerinin artık ve `scoring_uri` `swagger_uri` özellikleri vardır.
  + Taşınan çıkış çıkışları dizin oluşturma ve çıkış Dizin yüklemesi Kullanıcı işleminin dışına çıktı. Her Kullanıcı işleminde çalıştırılacak çalışma geçmişi SDK 'sını etkinleştirdi. Bu, dağıtılmış eğitim çalıştırmaları tarafından karşılaşılan bazı eşitleme sorunlarını çözmelidir.
  + Kullanıcı işlemi adından yazılan azureml günlüğünün adı artık işlem adını (yalnızca dağıtılmış eğitim için) ve PID 'yi içerir.

### <a name="azure-machine-learning-data-prep-sdk-v115"></a>Azure Machine Learning Data Prep SDK v 1.1.5

+ **Hata düzeltmeleri ve geliştirmeleri**
  + 2 basamaklı yıl biçimine sahip yorumlanan tarih saat değerleri için geçerli yıl aralığı Windows 'un yayını ile eşleşecek şekilde güncelleştirilmiştir. Aralık 1930-2029 ' den 1950-2049 ' e değiştirilmiştir.
  + Bir dosya ve ayarı `handleQuotedLineBreaks=True` `\r` okurken yeni bir satır olarak kabul edilir.
  + Bazı durumlarda başarısız olmasına neden `read_pandas_dataframe` olan bir hata düzeltildi.
  + Performansı `get_profile`geliştirildi.
  + Geliştirilmiş hata iletileri.

## <a name="2019-05-28"></a>2019-05-28

### <a name="azure-machine-learning-data-prep-sdk-v114"></a>Azure Machine Learning Data Prep SDK v 1.1.4

+ **Yeni Özellikler**
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
  + Yapılandırma auto_prepare_environment seçeneğini Çalıştır seçeneği, otomatik hazırlama varsayılan olarak devre dışı bırakılmıştır.

## <a name="2019-05-08"></a>2019-05-08

### <a name="azure-machine-learning-data-prep-sdk-v113"></a>Azure Machine Learning Data Prep SDK v 1.1.3

+ **Yeni Özellikler**
  + Read_postgresql çağırarak veya bir veri deposu kullanarak bir PostgresSQL veritabanından okuma desteği eklendi.
    + Bkz. nasıl yapılır kılavuzlarındaki örnekler:
      + [Veri alımı Not defteri](https://aka.ms/aml-data-prep-ingestion-nb)
      + [Veri deposu Not defteri](https://aka.ms/aml-data-prep-datastore-nb)

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Sütun türü dönüştürmesi ile ilgili sorunlar düzeltildi:
  + Şimdi Boolean veya sayısal bir sütunu Boole sütununa doğru bir şekilde dönüştürür.
  + Tarih sütunu tarih türü olarak ayarlamaya çalışırken artık başarısız olmaz.
  + Geliştirilmiş JoinType türleri ve başvuru belgeleri. İki veri akışı birleştirilirken, artık bu tür birleştirme türlerinden birini belirtebilirsiniz:
    + NONE, MATCH, INNER, UNMATCHLEFT, LEFTANTI, LEFTOUTER, UNMATCHRIGHT, SOLTANTI, TAM OLARAK, FULLANTI, TAM.
  + Daha fazla tarih biçimini tanımak için veri türü artırılmıştır.

## <a name="2019-05-06"></a>2019-05-06

### <a name="azure-portal"></a>Azure portal

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
+ Azure Machine Learning hizmeti çalışma alanının genel bakış sayfasından config. json dosyasını indirme özelliği eklendi.
+ Azure Databricks çalışma alanından hizmet çalışma alanı oluşturma desteği Machine Learning 

## <a name="2019-04-26"></a>2019-04-26

### <a name="azure-machine-learning-sdk-for-python-v1033"></a>Python v 1.0.33 için SDK Azure Machine Learning
+ **Yeni Özellikler**
  + _Workspace. Create_ YÖNTEMI artık CPU ve GPU kümeleri için varsayılan küme yapılandırmasını kabul eder.
  + Çalışma alanı oluşturma başarısız olursa, bağımlı kaynaklar temizlenir.
  + Varsayılan Azure Container Registry SKU 'SU temel olarak değiştirildi.
  + Azure Container Registry, çalıştırma veya görüntü oluşturma için gerektiğinde geç oluşturulur.
  + Eğitim çalıştırmaları için ortamlar desteği.

### <a name="notebook-virtual-machine"></a>Not defteri sanal makinesi 

Machine Learning denemeleri 'i programtabileceğiniz, modelleri Web uç noktaları olarak dağıtmak ve Python kullanarak Azure Machine Learning SDK tarafından desteklenen tüm diğer işlemleri gerçekleştirmek için bir not defteri VM 'yi, Jupyter Not defterleri için güvenli, kurumsal kullanıma yönelik bir barındırma ortamı olarak kullanın. Çeşitli yetenekler sağlar:
+ [](tutorial-1st-experiment-sdk-setup.md) Azure Machine Learning SDK ve ilgili paketlerin en son sürümüne sahip olan önceden yapılandırılmış bir not defteri VM 'sini hızlıca çalıştırın.
+ Erişim, HTTPS, Azure Active Directory kimlik doğrulaması ve yetkilendirme gibi kanıtlanmış teknolojiler aracılığıyla güvenli hale getirilir.
+ Azure Machine Learning Çalışma Alanı BLOB depolama hesabınızda Not defterlerinin ve kodun güvenilir bulut depolaması. Çalışmanızı kaybetmeden Not defteri sanal makinenizin güvenli bir şekilde silinmesini sağlayabilirsiniz.
+ Azure Machine Learning hizmeti özelliklerini incelemek ve denemek için önceden yüklenmiş örnek Not defterleri.
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

Aynı uç noktayı koruyarak yayımlanmış bir işlem hattının yeni bir sürümünü eklemeyi [tanıtmıştı.`PipelineEndpoint`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline_endpoint.pipelineendpoint?view=azure-ml-py)

## <a name="2019-04-17"></a>2019-04-17

### <a name="azure-machine-learning-data-prep-sdk-v112"></a>Azure Machine Learning Data Prep SDK v 1.1.2

Not: Data Prep Python SDK artık yüklenemez `numpy` ve `pandas` paketlerdir. Bkz. [güncelleştirilmiş yükleme yönergeleri](https://aka.ms/aml-data-prep-installation).

+ **Yeni Özellikler**
  + Artık Pivot dönüşümünü kullanabilirsiniz.
    + Nasıl yapılır Kılavuzu: [Özet Not defteri](https://aka.ms/aml-data-prep-pivot-nb)
  + Artık yerel işlevlerde normal ifadeleri kullanabilirsiniz.
    + Örnekler:
      + `dflow.filter(dprep.RegEx('pattern').is_match(dflow['column_name']))`
      + `dflow.assert_value('column_name', dprep.RegEx('pattern').is_match(dprep.value))`
  + Artık `to_upper`ifade dilinde   işlevleri kullanabilirsiniz `to_lower`.
  + Artık bir veri profilindeki her bir sütunun benzersiz değerlerinin sayısını görebilirsiniz.
  + Yaygın olarak kullanılan bazı okuyucu adımları için artık `infer_column_types` bağımsız değişkeni geçirebilirsiniz. Olarak `True`ayarlanırsa, veri hazırlığı sütun türlerini algılamaya ve otomatik olarak dönüştürmeye çalışır.
    + `inference_arguments`Artık kullanım dışıdır.
  + Artık çağırabilirsiniz `Dataflow.shape`.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + `keep_columns` Artık ek bir isteğe bağlı bağımsız `validate_column_exists`değişkeni kabul eder, bu da `keep_columns` sonucun herhangi bir sütun içerecekse olup olmadığını denetler.
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

+ **Yeni Özellikler**
  + Azure Machine Learning SDK artık Python 3,7 ' i desteklemektedir.
  + Azure Machine Learning DNN estimators artık yerleşik çoklu sürüm desteği sağlıyor. Örneğin, `TensorFlow`  tahmin aracı şimdi bir `framework_version` parametreyi kabul eder ve kullanıcılar ' 1,10 ' veya ' 1,12 ' sürümünü belirtebilir. Geçerli SDK sürümleriniz tarafından desteklenen sürümlerin bir listesi için, istenen Framework sınıfını `get_supported_versions()` (örneğin, `TensorFlow.get_supported_versions()`) çağırın.
  En son SDK sürümü tarafından desteklenen sürümlerin bir listesi için [DNN Estimator belgelerine](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn?view=azure-ml-py)bakın.

### <a name="azure-machine-learning-data-prep-sdk-v111"></a>Azure Machine Learning Data Prep SDK v 1.1.1

+ **Yeni Özellikler**
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

+ **Yeni Özellikler**
  + *Azureml. Core. Run. create_children* yöntemi, tek bir çağrıda birden çok alt çalıştırmanın düşük gecikmeli oluşturulmasına izin verir.

### <a name="azure-machine-learning-data-prep-sdk-v110"></a>Azure Machine Learning Data Prep SDK v 1.1.0

+ **Bozucu değişiklikler**
  + Veri hazırlama paketinin kavramı kullanım dışı bırakılmış ve artık desteklenmiyor. Tek bir pakette birden çok veri akışını kalıcı hale getirmek yerine, veri akışlarını ayrı ayrı kalıcı hale getirebilirsiniz.
    + Nasıl yapılır Kılavuzu: [Veri akışları Not defteri açılıyor ve kaydediliyor](https://aka.ms/aml-data-prep-open-save-dataflows-nb)

+ **Yeni Özellikler**
  + Veri hazırlama artık belirli bir anlam türüyle eşleşen sütunları tanıyabilir ve buna uygun şekilde ayrılabilir. Şu anda desteklenen STypes 'ler şunlardır: e-posta adresi, Coğrafi Koordinatlar (Enlem & Boylam), IPv4 ve IPv6 adresleri, ABD telefon numarası ve ABD ZIP kodu.
    + Nasıl yapılır Kılavuzu: [Anlamsal türler Not defteri](https://aka.ms/aml-data-prep-semantic-types-nb)
  + Veri hazırlığı artık iki sayısal sütundan bir sonuç sütunu oluşturmak için aşağıdaki işlemleri destekler: çıkarma, çarpma, bölme ve mod.
  + Veri akışının yürütüldüğünde `verify_has_data()` kayıt oluşturulup oluşturulmayacağını denetlemek için bir veri akışı üzerinde arama yapabilirsiniz.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Artık sayısal sütun profilleri için bir histogram içinde kullanılacak bölme sayısını belirtebilirsiniz.
  + `read_pandas_dataframe` Dönüştürme artık veri çerçevesinin dize veya bayt türünde sütun adlarına sahip olmasını gerektirir.
  + Bir hata `fill_nulls` düzeltildi, ancak sütun eksik ise değerler doğru doldurulmadı.

## <a name="2019-03-11"></a>2019-03-11

### <a name="azure-machine-learning-sdk-for-python-v1018"></a>Python v 1.0.18 için SDK Azure Machine Learning

 + **Değişikliklerine**
   + Azureml-tensorboard paketi, azureml-contrib-tensorboard yerini alır.
   + Bu sürümle birlikte, yönetilen işlem kümenizde (amlcompute), oluştururken bir kullanıcı hesabı ayarlayabilirsiniz. Bu, sağlama yapılandırmasında bu özellikler geçirerek yapılabilir. Daha fazla ayrıntı için [SDK başvuru belgelerine](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py#provisioning-configuration-vm-size-----vm-priority--dedicated---min-nodes-0--max-nodes-none--idle-seconds-before-scaledown-none--admin-username-none--admin-user-password-none--admin-user-ssh-key-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--tags-none--description-none-)ulaşabilirsiniz.

### <a name="azure-machine-learning-data-prep-sdk-v1017"></a>Azure Machine Learning Data Prep SDK v 1.0.17

+ **Yeni Özellikler**
  + Artık ifade dilini kullanarak sonuç sütunu oluşturmak için iki sayısal sütun eklenmesini destekler.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Random_split için belge ve parametre denetimi geliştirildi.
  
## <a name="2019-02-27"></a>2019-02-27

### <a name="azure-machine-learning-data-prep-sdk-v1016"></a>Azure Machine Learning Data Prep SDK v 1.0.16

+ **Hata onarımı**
  + Bir API değişikliği nedeniyle oluşan bir hizmet sorumlusu kimlik doğrulama sorunu düzeltildi.

## <a name="2019-02-25"></a>2019-02-25

### <a name="azure-machine-learning-sdk-for-python-v1017"></a>Python v 1.0.17 için SDK Azure Machine Learning

+ **Yeni Özellikler**

  + Azure Machine Learning artık popüler DNN Framework Chainer için birinci sınıf destek sağlar. [`Chainer`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) Sınıf kullanıcıları, Chainer modellerini kolayca eğitebilir ve dağıtabilir.
    + [Chaeylemsizlik ile dağıtılmış eğitime nasıl çalıştırılacağını](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/distributed-chainer/distributed-chainer.ipynb) öğrenin
    + [Hyperparameter ayarlamayı hiper sürücü kullanarak Chainer ile çalıştırma](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-chainer/train-hyperparameter-tune-deploy-with-chainer.ipynb) hakkında bilgi edinin
  + Azure Machine Learning işlem hatları ekleme özelliği, veri deposu değişikliklerine göre bir işlem hattı çalıştırmasını tetikler. İşlem hattı [zamanlama Not defteri](https://aka.ms/pl-schedule) , bu özelliği göstermek için güncelleştirilir.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Source_directory_data_store özelliğini, [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep?view=azure-ml-py)Için sağlanan [runconfigurations](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.runconfiguration?view=azure-ml-py) 'lar üzerinde istenen veri deposuna (BLOB depolama gibi) ayarlamaya yönelik destek Azure Machine Learning işlem hatları ekledik. Varsayılan olarak, çok sayıda adım eşzamanlı olarak yürütüldüğünde sorunları azaltma sorunlarını ortadan kaldırarak Azure dosya deposunu, yedekleme veri deposu olarak kullanır.

### <a name="azure-portal"></a>Azure portal

+ **Yeni Özellikler**
  + Raporlar için yeni Sürükle ve bırak tablosu Düzenleyicisi deneyimi. Kullanıcılar bir sütunu, tablonun önizlemesinin görüntüleneceği tablo alanına kadar iyi sürükleyebilirsiniz. Sütunlar yeniden düzenlenebilir.
  + Yeni günlük dosyası Görüntüleyici
  + Etkinlik sekmesinden deneme çalıştırmaları, işlem, modeller, görüntüler ve dağıtımlara bağlantılar

### <a name="azure-machine-learning-data-prep-sdk-v1015"></a>Azure Machine Learning Data Prep SDK v 1.0.15

+ **Yeni Özellikler**
  + Veri hazırlığı artık bir veri akışından dosya akışlarının yazılmasını desteklemektedir. Ayrıca, yeni dosya adları oluşturmak için dosya akışı adlarını değiştirme olanağı da sağlar.
    + Nasıl yapılır Kılavuzu: [Dosya akışları not defteriyle çalışma](https://aka.ms/aml-data-prep-file-stream-nb)

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Büyük veri kümelerinde t-Digest performansı geliştirildi.
  + Veri hazırlığı artık verileri bir veri yolundan okumayı destekliyor.
  + Artık bir etkin kodlama, Boolean ve sayısal sütunlarda çalışır.
  + Diğer çeşitli hata düzeltmeleri.

## <a name="2019-02-11"></a>2019-02-11

### <a name="azure-machine-learning-sdk-for-python-v1015"></a>Python v 1.0.15 için SDK Azure Machine Learning

+ **Yeni Özellikler**
  + AzureBatchStep ([Not defteri](https://aka.ms/pl-azbatch)), HyperDriveStep (Not defteri) ve zaman tabanlı zamanlama işlevselliği ([Not defteri](https://aka.ms/pl-schedule)) eklenen Azure Machine Learning işlem hatları.
  +  DataTranferStep, Azure SQL Server ve PostgreSQL için Azure veritabanı ([Not defteri](https://aka.ms/pl-data-trans)) ile çalışacak şekilde güncelleştirildi.

+ **Değişikliklerine**
  + Kullanım dışı bırakıldı `PublishedPipeline.get_published_pipeline`. `PublishedPipeline.get`
  + Kullanım dışı bırakıldı `Schedule.get_schedule`. `Schedule.get`

### <a name="azure-machine-learning-data-prep-sdk-v1012"></a>Azure Machine Learning Data Prep SDK v 1.0.12

+ **Yeni Özellikler**
  + Veri hazırlama artık, veri deposu kullanarak bir Azure SQL veritabanından okumayı destekler.
 
+ **Değişikliklerine**
  + Büyük veriler üzerinde belirli işlemlerin bellek performansı geliştirilmiştir.
  + `read_pandas_dataframe()`Şimdi belirtilmesini `temp_folder` gerektirir.
  + Özelliği kullanım dışı bırakıldı- `column_name` bunun yerine kullanım dışıdır. `ColumnProfile` `name`

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
  
### <a name="azure-portal-new-features"></a>Azure portalı: yeni özellikler
+ Raporlar için yeni sürükleme ve bırakma grafik deneyimi. Kullanıcılar bir sütunu ya da özniteliği, sistemin veri türüne göre otomatik olarak uygun bir grafik türü seçlebileceği grafik alanına sürükleyebilirsiniz. Kullanıcılar grafik türünü diğer geçerli türler olarak değiştirebilir veya ek öznitelikler ekleyebilir.

    Desteklenen grafik türleri:
    - Çizgi Grafiği
    - Çubuk grafik
    - Yığılmış çubuk grafik
    - Kutu çizimi
    - Çizim Dağılım
    - Kabarcık çizimi
+ Portal artık denemeleri için raporları dinamik olarak oluşturur. Bir Kullanıcı bir denemeye bir çalıştırma gönderdiğinde, farklı çalışma genelinde karşılaştırmaya izin vermek için günlüğe kaydedilen ölçümler ve grafiklerle birlikte otomatik olarak bir rapor oluşturulur. 

## <a name="2019-01-14"></a>2019-01-14

### <a name="azure-machine-learning-sdk-for-python-v108"></a>Python v 1.0.8 için SDK Azure Machine Learning

+ **Hata düzeltmeleri**: Bu sürüm genellikle küçük hata düzeltmeleri içerir

### <a name="azure-machine-learning-data-prep-sdk-v107"></a>Azure Machine Learning Data Prep SDK v 1.0.7

+ **Yeni Özellikler**
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

+ **Yeni Özellikler**
  + `to_bool`işlev artık eşleşmeyen değerlerin hata değerlerine dönüştürülmesine izin veriyor. Bu, ve `to_bool` `set_column_types`için yeni varsayılan uyumsuzluk davranışıdır, ancak önceki varsayılan davranış eşleşmeyen değerleri yanlış olarak dönüştürmelidir.
  + Çağrılırken `to_pandas_dataframe`, sayısal sütunlardaki null/eksik değerleri Nan olarak yorumlamak için yeni bir seçenek vardır.
  + Tür tutarlılığı sağlamak ve erken yük devretmek için bazı ifadelerin dönüş türünü denetleme özelliği eklendi.
  + Artık bir sütundaki değerleri `parse_json` JSON nesneleri olarak ayrıştırmaya ve bunları birden çok sütuna genişletmeye çağrı yapabilirsiniz.

+ **Hata düzeltmeleri**
  + Python 3.5.2 `set_column_types` 'te kilitlenen bir hata düzeltildi.
  + AML görüntüsü kullanılarak veri deposuna bağlanılırken kilitlenen hata düzeltildi.

+ **Güncelleştirmeler**
  * Başlarken öğreticileri, örnek olay incelemeleri ve nasıl yapılır kılavuzlarından oluşan [örnek Not defterleri](https://aka.ms/aml-data-prep-notebooks) .

## <a name="2018-12-04-general-availability"></a>2018-12-04: Genel Erişilebilirlik

Azure Machine Learning hizmeti genel kullanıma sunulmuştur.

### <a name="azure-machine-learning-compute"></a>Azure Machine Learning işlem
Bu sürümle birlikte [Azure Machine Learning işlem](how-to-set-up-training-targets.md#amlcompute)aracılığıyla yeni bir yönetilen işlem deneyimi duyuruyoruz. Bu işlem hedefi Azure Machine Learning Azure Batch AI işlem yerini alır. 

Bu işlem hedefi:
+ Model eğitimi ve toplu iş çıkarımı/Puanlama için kullanılır
+ Tek-çok düğümlü işlem
+ Kullanıcı için küme yönetimi ve iş zamanlaması yapar
+ Varsayılan olarak otomatik ölçekler
+ Hem CPU hem de GPU kaynakları için destek 
+ Düşük öncelikli VM 'lerin daha düşük maliyetli olarak kullanılmasına izin verebilir

Azure Machine Learning işlem, Azure portal veya CLI kullanarak Python'da oluşturulabilir. Çalışma alanınızın bölgesinde oluşturulmalıdır ve herhangi bir çalışma alanına bağlı olamaz. Bu işlem hedefi, çalıştırmanız için bir Docker kapsayıcısı kullanır ve bağımlılıklarınızı, tüm düğümleriniz genelinde aynı ortamı çoğaltmak üzere paketleyebilirsiniz.

> [!Warning]
> Azure Machine Learning işlem kullanmak için yeni bir çalışma alanı oluşturmanızı öneririz. Kullanıcılar Azure Machine Learning işlem mevcut bir çalışma alanından oluşturulmaya çalışılırken bir hata görebilirsiniz uzak bir fırsat yoktur. Mevcut işlem çalışma alanınızdaki etkilenmeyen çalışmaya devam etmesi gerekir.

### <a name="azure-machine-learning-sdk-for-python-v102"></a>Azure Machine SDK için Python v1.0.2 Learning
+ **Bozucu değişiklikler**
  + Bu sürümle birlikte, Azure Machine Learning ile VM oluşturmaya yönelik destek kaldırıyoruz. Veya uzak bir şirket içi sunucusu hala var olan bir buluta VM ekleyebilirsiniz. 
  + Ayrıca, tüm Azure Machine Learning işlem artık desteklenmelidir BatchAI, desteği kaldırıyoruz.

+ **Yeni**
  + Machine learning işlem hatları için:
    + [EstimatorStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.estimator_step.estimatorstep?view=azure-ml-py)
    + [HyperDriveStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.hyper_drive_step.hyperdrivestep?view=azure-ml-py)
    + [MpiStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.mpi_step.mpistep?view=azure-ml-py)


+ **Güncelleştirildi**
  + Machine learning işlem hatları için:
    + [DatabricksStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.databricks_step.databricksstep?view=azure-ml-py) artık runconfig kabul eder
    + [DataTransferStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) için ve SQL veri kaynağı artık kopyalar
    + Zamanlama oluşturma ve güncelleştirme zamanlamaları yayımlanan işlem hatları çalıştırmak için SDK'sındaki işlevleri

<!--+ **Bugs fixed**-->

### <a name="azure-machine-learning-data-prep-sdk-v052"></a>Azure Machine Learning veri hazırlama SDK v0.5.2
+ **Bozucu değişiklikler** 
  * `SummaryFunction.N` yeniden adlandırıldı `SummaryFunction.Count`.
  
+ **Hata düzeltmeleri**
  * En son AML çalıştırma okuma ve veri depoları için uzaktan çalıştırmalar yazarken belirteci kullanın. Python'da AML çalıştırma belirteci güncelleştirilir, daha önce veri hazırlığı çalışma zamanı güncelleştirilmiş AML çalıştırma belirteci ile güncelleştirilmez.
  * NET ek hata iletileri
  * Spark serileştirme kullandığında `Kryo` to_spark_dataframe () artık kilitlenmeyecektir
  * Inspector değer sayısı 1000'den fazla benzersiz değerler artık gösterebilirsiniz
  * Özgün veri akışı bir adı yoksa rastgele bir bölünmüş artık başarısız  

+ **Daha fazla bilgi**
  * [Azure Machine Learning Veri Hazırlama SDK'sı](https://aka.ms/data-prep-sdk)

### <a name="docs-and-notebooks"></a>Belgeleri ve Not Defterleri
+ ML işlem hatları
  + İşlem hatları, batch kapsamlar ve stil ile çalışmaya başlama için yeni ve güncelleştirilmiş not defterlerini örnekler aktarın: https://aka.ms/aml-pipeline-notebooks
  + Bilgi edinmek için nasıl [ilk işlem hattınızı oluşturun](how-to-create-your-first-pipeline.md)
  + Bilgi edinmek için nasıl [komut zincirlerini kullanarak batch Öngörüler çalıştırın](how-to-run-batch-predictions.md)
+ Azure Machine Learning işlem hedefi
  + [Örnek Not defterleri](https://aka.ms/aml-notebooks) artık yeni yönetilen işlem kullanacak şekilde güncelleştirilir.
  + [Bu işlem hakkında bilgi edinin](how-to-set-up-training-targets.md#amlcompute)

### <a name="azure-portal-new-features"></a>Azure portalı: yeni özellikler
+ Oluşturma ve yönetme [Azure Machine Learning işlem](how-to-set-up-training-targets.md#amlcompute) portalında türleri.
+ Kota kullanımını izlemek ve [isteği kota](how-to-manage-quotas.md) Azure Machine Learning işlemi için.
+ Işlem kümesi durumunu gerçek zamanlı olarak görüntüleyin Azure Machine Learning.
+ Azure Machine Learning işlem ve Azure Kubernetes hizmeti oluşturmak için sanal ağ desteği eklendi.
+ Mevcut parametrelerle yayınlanmış işlem hatlarınızı yeniden çalıştırın.
+ Yeni [machine learning grafikleri otomatik](how-to-understand-automated-ml.md) sınıflandırma modellerini (lift, Kazanç, ayarlama, özellik önem modeli explainability grafikle) ve regresyon modellerini (Kalanlar ve modeli ile özellik önem grafiği explainability). 
+ İşlem hatları Azure portalında görüntülenebilir




## <a name="2018-11-20"></a>2018-11-20

### <a name="azure-machine-learning-sdk-for-python-v0180"></a>Azure Machine SDK için Python v0.1.80 Learning

+ **Bozucu değişiklikler** 
  * *azureml.Train.widgets* ad alanı taşındığı *azureml.widgets*.
  * *azureml.core.compute.AmlCompute* aşağıdaki sınıflar - kullanımdan kaldırıldı *azureml.core.compute.BatchAICompute* ve *azureml.core.compute.DSVMCompute*. İkinci sınıfı sonraki sürümlerde kaldırılacak. AmlCompute sınıfı artık daha kolay bir tanıma sahip yalnızca bir vm_size ve max_nodes gerekir ve bir iş gönderildiğinde kümenize 0 max_nodes otomatik olarak ölçeklendirir. [Örnek Not defterleriniz](https://github.com/Azure/MachineLearningNotebooks/tree/master/training) bu bilgilerle güncelleştirildi ve size kullanım örnekleri vermelidir. Bu basitleştirme ve çok daha heyecan verici özelliklerin bir sonraki sürümde gelir gibi umuyoruz!

### <a name="azure-machine-learning-data-prep-sdk-v051"></a>Azure Machine Learning veri hazırlama SDK v0.5.1 

Veri hazırlığı SDK'sı hakkında daha fazla bilgi edinmek [başvuru belgeleri](https://aka.ms/data-prep-sdk).
+ **Yeni Özellikler**
   * DataPrep paketleri çalıştırmak ve bir veri kümesi veya veri akışı veri profilini görüntülemek için yeni bir DataPrep CLI oluşturdunuz
   * Kullanılabilirliği iyileştirmek için tasarlanan SetColumnType API
   * Yeniden adlandırılan smart_read_file auto_read_file için
   * Şimdi veri profilinde sapması ve basıklık içerir
   * İle stratified örnekleme örnek oluşturabilirsiniz
   * CSV dosyaları içeren zip dosyaları okuyabilir
   * Veri kümeleri satır temelinde rastgele bölme ile bölebilir (örneğin, test-tren kümelerine)
   * ' İ çağırarak bir veri akışından veya bir veri profilinden tüm sütun veri türlerini alabilir.`.dtypes`
   * ' İ çağırarak bir veri akışından veya bir veri profilinden satır sayısı alabilir.`.row_count`

+ **Hata düzeltmeleri**
   * Uzun çift dönüştürme sabit 
   * Sabit sütun ekleyin, sonra onaylama 
   * Grupları nereden bazı durumlarda algılaması değil, FuzzyGrouping ile bir sorun düzeltildi
   * Birden çok sütunu sıralamayı cevaben sabit bir sıralama işlevi
   * Onları nasıl `pandas` işleyeceğinize benzer sabit ve/veya ifadeler
   * Dbfs yolundan sabit okuma
   * Hata iletileri daha anlaşılır hale 
   * AML belirteci kullanarak uzak işlem hedefte okurken şimdi artık başarısız
   * Artık üzerinde Linux DSVM'sini artık başarısız
   * Dize olmayan değerler dize koşullarda şimdi artık çöküyor
   * Artık doğru şekilde veri akışı başarısız olursa, onaylama hatalarını ele alır
   * Artık Azure Databricks'te dbutils bağlı depolama konumları destekler

## <a name="2018-11-05"></a>2018-11-05

### <a name="azure-portal"></a>Azure portal 
Azure Machine Learning hizmeti için Azure portalı aşağıdaki güncelleştirmeler bulunur:
  * Yeni bir **işlem hatları** yayımlanan işlem hatları için sekmesinde.
  * İşlem hedefi olarak var olan bir HDInsight kümesine ekleme desteği eklendi.

### <a name="azure-machine-learning-sdk-for-python-v0174"></a>Azure Machine SDK için Python v0.1.74 Learning

+ **Bozucu değişiklikler** 
  * \* Workspace. compute_targets, datamağazalar, denemeleri, görüntüler, modeller ve *WebServices* , Yöntemler yerine özelliklerdir. Örneğin, *Workspace.compute_targets()* ile *Workspace.compute_targets*.
  * *Run.get_context* kullanımdan kaldırıldı *Run.get_submitted_run*. İkinci yöntem, sonraki sürümlerde kaldırılacak.
  * *PipelineData* sınıfı bir parametre yerine olarak datastore_name bir veri deposu nesnesi artık bekliyor. Benzer şekilde, *işlem hattı* default_datastore_name yerine default_datastore kabul eder.

+ **Yeni Özellikler**
  * Azure Machine Learning işlem hatlarını [örnek not defteri](https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline/pipeline-mpi-batch-prediction.ipynb) artık MPI adımları kullanır.
  * Jupyter not defterleri için RunDetails pencere, işlem hattının görselleştirilmesi göstermek için güncelleştirilir.

### <a name="azure-machine-learning-data-prep-sdk-v040"></a>Azure Machine Learning veri hazırlama SDK v0.4.0 
 
+ **Yeni Özellikler**
  * Veri profili türü sayısı eklendi 
  * Değer sayısı ve Histogram kullanıma sunuldu
  * Daha fazla yüzdebirliklerini veri profilinde
  * ORTANCA özetleme kullanılabilir
  * Python 3.7 artık desteklenmektedir
  * Veri depoları DataPrep paketi içeren bir veri akışı kaydettiğinizde, veri deposu bilgileri DataPrep paketinin bir parçası sürdürülecek
  * Veri deposu yazma artık desteklenmektedir 
        
+ **Hata düzeltildi**
  * 64-bit işaretsiz tamsayı taşmaları artık Linux üzerinde düzgün şekilde işlendi
  * Düz metin dosyalarında smart_read hatalı sabit metin etiketi
  * Dize sütun türü artık ölçümleri görünümünde gösterilir
  * Türü sayısı artık tek tek olanları yerine tek FieldType eşlenen ValueKinds gösterecek şekilde düzeltildi
  * Bir dize olarak yol sağlandığında Write_to_csv artık başarısız
  * Replace kullanırken, "boş bulma" bırakarak artık başarısız olur 

## <a name="2018-10-12"></a>2018-10-12

### <a name="azure-machine-learning-sdk-for-python-v0168"></a>Azure Machine SDK için Python v0.1.68 Learning

+ **Yeni Özellikler**
  * Yeni çalışma alanı oluşturulurken çok kiracılı destek.

+ **Düzeltilen hatalar**
  * Web hizmetini dağıtmaya yönelik olarak pynacl kitaplığı sürümünü Sabitlemeye gerek kalmaz.

### <a name="azure-machine-learning-data-prep-sdk-v030"></a>Azure Machine Learning veri hazırlama SDK v0.3.0

+ **Yeni Özellikler**
  * Kullanıcıların yürütmek için bir Python dosyası yolunda geçirilecek yöntemi transform_partition_with_file(script_path) eklendi

## <a name="2018-10-01"></a>2018-10-01

### <a name="azure-machine-learning-sdk-for-python-v0165"></a>Azure Machine SDK için Python v0.1.65 Learning
[Sürüm 0.1.65](https://pypi.org/project/azureml-sdk/0.1.65) yeni özellikler, daha fazla belge, hata düzeltmeleri ve daha fazlasını içeren [örnek not defterleri](https://aka.ms/aml-notebooks).

Bkz: [bilinen sorunların listesi](resource-known-issues.md) bilinen hataların ve geçici çözümleri hakkında bilgi edinmek için.

+ **Bozucu değişiklikler**
  * Workspace.experiments, Workspace.models, Workspace.compute_targets, Workspace.images, Workspace.web_services dönüş sözlük, daha önce listesi döndürdü. Bkz: [azureml.core.Workspace](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py) API belgeleri.

  * Machine Learning otomatik normalleştirilmiş Ortalama kare hata birincil ölçümleri kaldırıldı.

+ **HyperDrive**
  * Bayes çeşitli HyperDrive hata düzeltmeleri, ölçümleri çağrıları için performans geliştirmeleri alın. 
  * Tensorflow 1.10 1.9 sürümüne yükseltme 
  * Docker görüntü iyileştirmesi hazırlıksız başlatma için. 
  * İşler artık 0 dışında bir hata koduyla çıksa bile doğru durumu raporlar. 
  * Öznitelik doğrulamada SDK RunConfig. 
  * Çalıştırma HyperDrive nesnesi iptal normal çalışmaya benzer destekler: herhangi bir parametre geçirin gerek yoktur. 
  * Dağıtılmış çalıştırır ve HyperDrive çalıştırmalar için aşağı açılan değerleri durumunu korumak için pencere öğesi geliştirmeleri. 
  * Parametre sunucusu için sabit, TensorBoard ve diğer günlük dosyalarını destekler. 
  * Hizmet tarafı Intel(r) MPI desteği. 
  * Hata düzeltmesi, için parametre ayarlama için düzeltme çalışma BatchAI doğrulama sırasında dağıtılmış. 
  * Bağlam Yöneticisi artık birincil tanıtır. 

+ **Azure portal deneyimi**
  * Çalıştırma ayrıntıları log_table() ve log_row() desteklenir. 
  * 1, 2 veya 3 sayısal sütunlu ve isteğe bağlı bir kategorik sütunlu tablolar ve satırlar için otomatik olarak grafikler oluşturun.

+ **Otomatik makine öğrenimi**
  * Geliştirilmiş hata işleme ve belgeler 
  * Çalıştırma özelliği alımı performans sorunlarını düzelttik. 
  * Sabit çalışma sorunu devam edin. 
  * Ensembling yineleme sorunlar düzeltildi.
  * MAC OS sabit eğitim asılı hata.
  * Aşağı örnekleme makrosu ortalama çekme isteği/ROC eğrisi özel doğrulama senaryosunda.
  * Ek dizin mantıksal kaldırıldı.
  * Filtre get_output API ' kaldırıldı.

+ **İşlem hatları**
  * Bir yöntem bir işlem hattı yürütme ilk çalıştırma gerek kalmadan doğrudan yayımlamak için Pipeline.publish() eklendi.   
  * Yayımlanmış bir işlem hattından oluşturulan işlem hattı çalıştırmalarını getirmek için bir ardışık düzen eylemsizlik. Get _pipeline_çalıştırmaları () eklendi.

+ **Project Brainwave**
  * Yeni AI modelleri FPGA üzerinde kullanılabilir güncelleştirilmiş destek.

### <a name="azure-machine-learning-data-prep-sdk-v020"></a>Azure Machine Learning veri hazırlama SDK v0.2.0
[Sürüm 0.2.0](https://pypi.org/project/azureml-dataprep/0.2.0/) , aşağıdaki özellikleri ve hata düzeltmelerini içerir:

+ **Yeni Özellikler**
  * Bir sık erişimli kodlaması için destek
  * Quantile dönüştürme için destek
   
+ **Hata düzeltildi:**
  * Herhangi bir hortum sürümü ile çalışır hortum sürümünüzü düşürme gerekmez.
  * Değer için tüm değerleri, yalnızca ilk üç sayar.

## <a name="2018-09-public-preview-refresh"></a>2018-09 (genel Önizleme yenileme)

Azure Machine Learning yeni, yenilenmiş bir sürümü: Bu sürüm hakkında daha fazla bilgi edinin: https://azure.microsoft.com/blog/what-s-new-in-azure-machine-learning-service/


## <a name="next-steps"></a>Sonraki adımlar

[Azure Machine Learning hizmeti](../service/overview-what-is-azure-ml.md)için genel bakışı okuyun.
