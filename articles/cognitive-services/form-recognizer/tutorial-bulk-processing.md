---
title: 'Öğretici: Azure Data Factory form tanıyıcı kullanarak form verilerini toplu olarak ayıklama'
titleSuffix: Azure Cognitive Services
description: Büyük bir belge biriktirme listesini dijital olarak biçimlendirmek için form tanıyıcı modellerinin eğitimini ve çalışmasını tetiklemek üzere Azure Data Factory etkinlikleri ayarlayın.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: tutorial
ms.date: 01/04/2021
ms.author: pafarley
ms.openlocfilehash: 6faa612f55b4114b4242c48d43aae9aac8c56582
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101700006"
---
# <a name="tutorial-extract-form-data-in-bulk-using-azure-data-factory"></a>Öğretici: Azure Data Factory kullanarak form verilerini toplu olarak ayıklama

Bu öğreticide, Azure hizmetlerini kullanarak büyük bir form toplu işini dijital medyaya alma hakkında bakacağız. Bu öğreticide, bir Azure SQL veritabanı 'na bir belge Azure Data Lake veri alımı otomatik hale getirme işlemi gösterilmektedir. Birkaç tıklama ile modelleri hızlı bir şekilde eğitebilir ve yeni belgeleri işleyebilirsiniz.

## <a name="business-need"></a>İşletme gereksinimi

Çoğu kuruluş artık değerli verilerin farklı biçimlerde (PDF, resimler, videolar) nasıl olduğunu fark ediyor. Bunlar, en iyi uygulamaları ve bu varlıkları dijital olarak ayırmak için en düşük maliyetli yöntemleri arıyor.

Ayrıca, müşterilerimiz genellikle birçok istemci ve müşterisi tarafından gelen farklı türlerde formlara sahiptir. [Hızlı](./quickstarts/client-library.md)başlangıçlardan farklı olarak, bu öğreticide, meta veri odaklı bir yaklaşım kullanarak bir modeli yeni ve farklı form türleri ile otomatik olarak eğitme konusu gösterilmektedir. Verilen form türü için mevcut bir modeliniz yoksa, sistem sizin için bir tane oluşturur ve size model KIMLIĞI sağlar. 

İşletmeler, formlardan verileri ayıklayarak ve mevcut işletimsel sistemlerle ve veri ambarlarıyla birleştirerek müşteriler ve iş kullanıcılarına Öngörüler elde edebilir ve değer sunabilir.

Azure form tanıyıcı sayesinde kuruluşların verilerini, işlemleri otomatikleştirmesini (fatura ödemeleri, vergi işleme vb.), paradan ve zamandan tasarruf etmesini ve daha iyi veri doğruluğu tadını çıkardık.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Formlarınızı depolamak için Azure Data Lake ayarlama
> * Bir Azure veritabanı kullanarak parametrization tablosu oluşturma
> * Hassas kimlik bilgilerini depolamak için Azure Key Vault kullanma
> * Form tanıyıcı modelinizi bir Databricks not defterinde eğitme
> * Databricks Not defteri kullanarak form verilerinizi ayıklama
> * Azure Data Factory ile form eğitimini ve ayıklamayı otomatikleştirin

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services/)
* Azure aboneliğiniz olduktan sonra <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer"  title=" bir form tanıyıcı kaynağı oluşturun "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> Azure Portal anahtarınızı ve uç noktanızı almak için bir form tanıyıcı kaynağı oluşturun. Dağıtıldıktan sonra **Kaynağa Git**' i seçin.
    * Uygulamanızı form tanıyıcı API 'sine bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.
* Aynı türde en az beş form kümesi. İdeal olarak, bu iş akışı büyük belge kümelerini desteklemeye yöneliktir. Eğitim veri kümesini birlikte yerleştirmeye yönelik ipuçları ve seçenekler için bkz. [eğitim verileri kümesi oluşturma](./build-training-data-set.md) . Bu öğreticide, [örnek veri kümesinin](https://go.microsoft.com/fwlink/?linkid=2128080) **eğitme** klasörü altındaki dosyaları kullanabilirsiniz.

## <a name="project-architecture"></a>Proje mimarisi 

Bu proje, bir Azure Data Lake depolama hesabındaki belgelerden verileri eğiteleyen, çözümleyen ve ayıklayarak Python not defterlerini tetiklemek için bir dizi Azure Data Factory işlem hattı temsil eder.

Form tanıyıcı REST AP, giriş olarak bazı parametreler gerektirir. Güvenlik nedenleriyle, bu parametrelerden bazıları Azure Key Vault depolanır, ancak Depolama Blobu klasör adı gibi daha az hassas parametreler, bir Azure SQL veritabanında Parametreleştirme tablosunda depolanır.

Analiz edilecek form türü için, veri mühendisleri veya veri bilimcileri, parametre tablosunun bir satırını dolduracaktır. Ardından, algılanan form türleri listesini yinelemek ve form tanıyıcı modellerini eğitme veya yeniden eğitebilmeniz için ilgili parametreleri bir Databricks not defterine geçirmek üzere Azure Data Factory kullanırlar. Burada bir Azure işlevi de kullanılabilir.

Azure Databricks Not defteri daha sonra, form verilerini ayıklamak için eğitilen modelleri kullanır ve bu tarihi bir Azure SQL veritabanına aktarır.

:::image type="content" source="./media/tutorial-bulk-processing/architecture.png" alt-text="Proje mimarisi":::


## <a name="set-up-azure-data-lake"></a>Azure Data Lake ayarlama

Form kapsamınız şirket içi ortamınızda veya bir FTP sunucusunda olabilir. Bu öğretici Azure Data Lake Gen 2 depolama hesabındaki formları kullanır. Dosyalarınızı Azure Data Factory, Azure Depolama Gezgini veya AzCopy kullanarak aktarabilirsiniz. Eğitim ve Puanlama veri kümeleri farklı kapsayıcılarda olabilir, ancak tüm form türleri için eğitim veri kümelerinin aynı kapsayıcıda olması gerekir (ancak farklı klasörlerde yer alabilir).

Yeni bir Data Lake oluşturmak için, [Azure Data Lake Storage 2. birlikte kullanmak üzere depolama hesabı oluşturma](../../storage/blobs/create-data-lake-storage-account.md)bölümündeki yönergeleri izleyin.

## <a name="create-a-parameterization-table"></a>Parametreleştirme tablosu oluşturma

Daha sonra, bir Azure SQL veritabanında meta veri tablosu oluşturacağız. Bu tablo, REST API, form tanıyıcı tarafından gereken hassas olmayan verileri içerir. Veri kümizdeki yeni bir tür form olduğunda, bu tabloya yeni bir kayıt ekleyeceğiz ve eğitim ve Puanlama işlem hattını tetikleyeceğiz (daha sonra uygulanması için).

Tabloda aşağıdaki alanlar kullanılacaktır:

* **form_description**: Bu alan, eğitimin bir parçası olarak gerekli değildir. Modeli eğititireceğimiz form türünün bir açıklamasını sağlar (örneğin, "istemci A formları," "otel B formları").
* **training_container_name**: Bu alan, eğitim veri kümesini depoladığınız depolama hesabı kapsayıcısı adıdır. **Scoring_container_name** aynı kapsayıcı olabilir.
* **training_blob_root_folder**: depolama hesabındaki, modelin eğitimi için dosyaları depolayacağımız klasör.
* **scoring_container_name**: Bu alan, ' den anahtar değer çiftlerini ayıklamak istediğimiz dosyaları depoladığınız depolama hesabı kapsayıcısı adıdır. **Training_container_name** aynı kapsayıcı olabilir.
* **scoring_input_blob_folder**: depolama hesabındaki, verileri Ayıklanacak dosyaları depolayacağımız klasör.
* **model_id**: yeniden eğitebilmemiz ISTEDIĞIMIZ modelin kimliği. İlk çalıştırma için değer-1 ' e ayarlanmalıdır, bu da eğitim Not defterinin eğitebileceği yeni bir özel model oluşturmasına neden olur. Eğitim Not defteri yeni oluşturulan model KIMLIĞINI Azure Data Factory örneğine döndürür ve saklı yordam etkinliğini kullanarak bu değeri Azure SQL veritabanında güncelleştiririz.

  Yeni bir form türünü almak istediğinizde, modeli eğitmek için model KIMLIĞINI-1 ' e el ile sıfırlamanız gerekir.

* **file_type**: desteklenen form türleri,, `application/pdf` `image/jpeg` `image/png` ve `image/tif` .

  Farklı dosya türlerine sahip formunuz varsa, yeni bir form türünü eğitmek için bu değeri değiştirmeniz ve **model_id** gerekir.
* **form_batch_group_id**: zaman içinde, aynı modele göre eğiteettiğiniz birden çok form türüne sahip olabilirsiniz. **Form_batch_group_id** , belirli bir modeli kullanarak eğitime yapılan tüm form türlerini belirtmenizi sağlayacak.

### <a name="create-the-table"></a>Tabloyu oluşturma

[Bir Azure SQL veritabanı oluşturun](https://ms.portal.azure.com/#create/Microsoft.SQLDatabase)ve ardından gerekli tabloyu oluşturmak için [sorgu DÜZENLEYICISI](../../azure-sql/database/connect-query-portal.md) 'nde aşağıdaki SQL betiğini çalıştırın.

```sql
CREATE TABLE dbo.ParamFormRecogniser(
    form_description varchar(50) NULL,
  training_container_name varchar(50) NOT NULL,
    training_blob_root_folder varchar(50) NULL,
    scoring_container_name varchar(50) NOT NULL,
    scoring_input_blob_folder varchar(50) NOT NULL,
    scoring_output_blob_folder varchar(50) NOT NULL,
    model_id varchar(50) NULL,
    file_type varchar(50) NULL
) ON PRIMARY
GO
```

Eğitilen bir kez **model_id** otomatik olarak güncelleştirme yordamını oluşturmak için aşağıdaki betiği çalıştırın.

```SQL
CREATE PROCEDURE [dbo].[update_model_id] ( @form_batch_group_id  varchar(50),@model_id varchar(50))
AS
BEGIN 
    UPDATE [dbo].[ParamFormRecogniser]   
        SET [model_id] = @model_id  
    WHERE form_batch_group_id =@form_batch_group_id
END
```

## <a name="use-azure-key-vault-to-store-sensitive-credentials"></a>Hassas kimlik bilgilerini depolamak için Azure Key Vault kullanma

Güvenlik nedenleriyle, belirli hassas bilgileri Azure SQL veritabanındaki Parametreleştirme tablosunda depolamak istemiyorum. Gizli parametreleri Azure Key Vault gizli dizi olarak depolayacağız.

### <a name="create-an-azure-key-vault"></a>Azure Key Vault oluşturma

[Key Vault kaynağı oluşturun](https://ms.portal.azure.com/#create/Microsoft.KeyVault). Ardından, oluşturulduktan sonra Key Vault kaynağına gidin ve **Ayarlar** bölümünde, parametreleri eklemek için **gizli** dizileri seçin.

Yeni bir pencere görünür, **Oluştur/içeri aktar**' ı seçin. Parametrenin adını ve değerini girip oluştur ' a tıklayın. Bunu aşağıdaki parametreler için yapın:

* **Biliveserviceendpoint**: form tanıyıcı API 'nizin uç nokta URL 'si.
* **Biliveservicesubscriptionkey**: form tanıyıcı hizmetiniz için erişim anahtarı. 
* **StorageAccountName**: eğitim veri kümesinin ve formların içinden anahtar-değer çiftlerini ayıklamak istediğimiz depolama hesabı. Bunlar farklı hesaplarınızda, hesap adlarının her birini ayrı parolalar olarak girin. Eğitim veri kümelerinin tüm form türleri için aynı kapsayıcıda olması gerektiğini unutmayın, ancak farklı klasörlerde yer alabilir.
* **Storageaccountsaskey**: depolama hesabının paylaşılan erişim IMZASı (SAS). SAS URL 'sini almak için depolama kaynağına gidin ve **Depolama Gezgini** sekmesini seçin. Kapsayıcınıza gidin, sağ tıklayın ve **paylaşılan erişim Imzasını al**' ı seçin. Depolama hesabının kendisi için değil, kapsayıcınıza yönelik SAS almak önemlidir. **Okuma** ve **Listeleme** izinlerinin işaretli olduğundan emin olun ve **Oluştur**' a tıklayın. Sonra **URL** bölümündeki değeri kopyalayın. Şu biçimde olmalıdır: `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>`.

## <a name="train-your-form-recognizer-model-in-a-databricks-notebook"></a>Form tanıyıcı modelinizi bir Databricks not defterinde eğitme

Form tanıyıcı hizmetiyle etkileşim kuran Python kodunu depolamak ve çalıştırmak için Azure Databricks kullanacaksınız.

### <a name="create-a-notebook-in-databricks"></a>Databricks 'te Not defteri oluşturma

Azure portal [bir Azure Databricks kaynağı oluşturun](https://ms.portal.azure.com/#create/Microsoft.Databricks) . Oluşturulduktan sonra kaynağa gidin ve çalışma alanını başlatın.

### <a name="create-a-secret-scope-backed-by-azure-key-vault"></a>Azure Key Vault tarafından desteklenen gizli bir kapsam oluşturun

Yukarıda oluşturduğumuz Azure Key Vault sırlara başvurmak için Databricks içinde gizli bir kapsam oluşturmanız gerekir. [Azure Key Vault ile desteklenen gizli dizi kapsamı oluşturma](/azure/databricks/security/secrets/secret-scopes#--create-an-azure-key-vault-backed-secret-scope)bölümündeki adımları izleyin.

### <a name="create-a-databricks-cluster"></a>Databricks kümesi oluşturma

Küme, Databricks hesaplama kaynakları koleksiyonudur. Bir küme oluşturmak için:

1. Yan çubukta **kümeler** düğmesine tıklayın.
1. **Kümeler** sayfasında, **küme oluştur**' a tıklayın.
1. **Küme oluştur** sayfasında, bir küme adı belirtin ve Databricks Runtime sürüm açılır penceresinde **7,2 (Scala 2,12, Spark 3.0.0)** seçeneğini belirleyin.
1. **Create Cluster** (Küme Oluştur) öğesine tıklayın.

### <a name="write-a-settings-notebook"></a>Bir ayarlar Not defteri yazın

Artık Python Not defterleri eklemeye hazırsınız. İlk olarak, **Ayarlar** adlı bir not defteri oluşturun; Bu not defteri, Parametreleştirme tablonuzdaki değerleri betikteki değişkenlere atayacaktır. Değerler daha sonra Azure Data Factory göre parametre olarak geçirilir. Ayrıca, Key Vault dizilerinizden değişkenlere değerler atayacağız. 

1. **Ayarlar** Not defterini oluşturmak için, **çalışma alanı** düğmesine tıklayın, yeni sekmesinde, açılan listeye tıklayın ve **Oluştur** ve sonra **Not defteri**' ni seçin.
1. Açılır pencerede, not defterine vermek istediğiniz adı girin ve varsayılan dil olarak **Python** ' ı seçin. Databricks kümenizi seçip **Oluştur**' u seçin.
1. İlk not defteri hücresinde, Azure Data Factory tarafından geçirilen parametreleri alıyoruz.

    ```python 
    dbutils.widgets.text("form_batch_group_id", "","")
    dbutils.widgets.get("form_batch_group_id")
    form_batch_group_id = getArgument("form_batch_group_id")
    
    dbutils.widgets.text("model_id", "","")
    dbutils.widgets.get("model_id")
    model_id = getArgument("model_id")
    
    dbutils.widgets.text("training_container_name", "","")
    dbutils.widgets.get("training_container_name")
    training_container_name = getArgument("training_container_name")
    
    dbutils.widgets.text("training_blob_root_folder", "","")
    dbutils.widgets.get("training_blob_root_folder")
    training_blob_root_folder=  getArgument("training_blob_root_folder")
    
    dbutils.widgets.text("scoring_container_name", "","")
    dbutils.widgets.get("scoring_container_name")
    scoring_container_name=  getArgument("scoring_container_name")
    
    dbutils.widgets.text("scoring_input_blob_folder", "","")
    dbutils.widgets.get("scoring_input_blob_folder")
    scoring_input_blob_folder=  getArgument("scoring_input_blob_folder")
    
    
    dbutils.widgets.text("file_type", "","")
    dbutils.widgets.get("file_type")
    file_type = getArgument("file_type")
    
    dbutils.widgets.text("file_to_score_name", "","")
    dbutils.widgets.get("file_to_score_name")
    file_to_score_name=  getArgument("file_to_score_name")
    ```

1. İkinci hücrede Key Vault parolaları alır ve değişkenlere atar.

    ```python 
    cognitive_service_subscription_key = dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "CognitiveserviceSubscriptionKey")
    cognitive_service_endpoint = dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "CognitiveServiceEndpoint")
    
    training_storage_account_name = dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "StorageAccountName")
    storage_account_sas_key= dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "StorageAccountSasKey")  
    
    ScoredFile = file_to_score_name+ "_output.json"
    training_storage_account_url="https://"+training_storage_account_name+".blob.core.windows.net/"+training_container_name+storage_account_sas_key 
    ```

### <a name="write-a-training-notebook"></a>Eğitim Not defteri yazma

Artık **Ayarlar** Not defterini tamamladığımıza göre, modeli eğitebilmeniz için bir not defteri oluşturuyoruz. Yukarıda belirtildiği gibi, Azure Data Lake Gen 2 depolama hesabındaki bir klasörde depolanan dosyaları kullanacağız (**training_blob_root_folder**). Klasör adı bir değişken olarak geçirildi. Her form türü kümesi aynı klasörde olacak ve parametre tablosunda döngü yaptığımız için modeli, tüm form türlerini kullanarak eğtireceğiz. 

1. **Trainformtanıyıcı** adlı yeni bir not defteri oluşturun. 
1. İlk hücrede ayarlar Not defterini yürütün:

    ```python
    %run "./Settings"
    ```

1. Sonraki hücrede, **Ayarlar** dosyasından değişkenler atayın ve her form türü için modeli dinamik olarak eğitme, bu kodu [rest hızlı](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/FormRecognizer/rest/python-train-extract.md#get-training-results%20)başlangıca uygulayın.

    ```python
    import json
    import time
    from requests import get, post
    
    post_url = cognitive_service_endpoint + r"/formrecognizer/v2.0/custom/models"
    source = training_storage_account_url
    prefix= training_blob_root_folder
    
    includeSubFolders=True
    useLabelFile=False
    headers = {
        # Request headers
        'Content-Type': file_type,
        'Ocp-Apim-Subscription-Key': cognitive_service_subscription_key,
    }
    body =     {
        "source": source
        ,"sourceFilter": {
            "prefix": prefix,
            "includeSubFolders": includeSubFolders
       },
    }
    if model_id=="-1": # if you don't already have a model you want to retrain. In this case, we create a model and use it to extract the key-value pairs
      try:
          resp = post(url = post_url, json = body, headers = headers)
          if resp.status_code != 201:
              print("POST model failed (%s):\n%s" % (resp.status_code, json.dumps(resp.json())))
              quit()
          print("POST model succeeded:\n%s" % resp.headers)
          get_url = resp.headers["location"]
          model_id=get_url[get_url.index('models/')+len('models/'):]     
          
      except Exception as e:
          print("POST model failed:\n%s" % str(e))
          quit()
    else :# if you already have a model you want to retrain, we reuse it and (re)train with the new form types.  
      try:
        get_url =post_url+r"/"+model_id
          
      except Exception as e:
          print("POST model failed:\n%s" % str(e))
          quit()
    ```

1. Eğitim sürecinin son adımı, eğitimin bir JSON biçiminde elde edilmesine neden olur.

    ```python
    n_tries = 10
    n_try = 0
    wait_sec = 5
    max_wait_sec = 5
    while n_try < n_tries:
        try:
            resp = get(url = get_url, headers = headers)
            resp_json = resp.json()
            print (resp.status_code)
            if resp.status_code != 200:
                print("GET model failed (%s):\n%s" % (resp.status_code, json.dumps(resp_json)))
                n_try += 1
                quit()
            model_status = resp_json["modelInfo"]["status"]
            print (model_status)
            if model_status == "ready":
                print("Training succeeded:\n%s" % json.dumps(resp_json))
                n_try += 1
                quit()
            if model_status == "invalid":
                print("Training failed. Model is invalid:\n%s" % json.dumps(resp_json))
                n_try += 1
                quit()
            # Training still running. Wait and retry.
            time.sleep(wait_sec)
            n_try += 1
            wait_sec = min(2*wait_sec, max_wait_sec)     
            print (n_try)
        except Exception as e:
            msg = "GET model failed:\n%s" % str(e)
            print(msg)
            quit()
    print("Train operation did not complete within the allocated time.")
    ```

## <a name="extract-form-data-using-a-notebook"></a>Not defteri kullanarak form verilerini ayıklama

### <a name="mount-the-azure-data-lake-storage"></a>Azure Data Lake depolamayı bağlama

Bir sonraki adım, eğitilen modeli kullanmakta olduğumuz farklı formlara puan vereceğiz. Azure Data Lake depolama hesabını Databricks 'e bağlayacağız ve bir işlem sırasında bağlama işleminde başvuracağız.

Eğitim aşamasında olduğu gibi, formlardan anahtar-değer çiftleri ayıklanmasını çağırmak için Azure Data Factory kullanacağız. Parametre tablosunda belirtilen klasörlerdeki formların üzerinde döngü göndereceğiz.

1. Depolama hesabını Databricks 'e bağlamak için Not defterini oluşturalım. It **Bağlamaverileri** Gölü için arama yapacağız. 
1. Önce **Ayarlar** Not defterini çağırmanız gerekir:

    ```python
    %run "./Settings"
    ```

1. İkinci hücrede, hassas parametrelerin değişkenlerini tanımlayacağız, bu da Key Vault gizli dizilerimizden elde edeceğiz.

    ```python
    cognitive_service_subscription_key = dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "CognitiveserviceSubscriptionKey")
    cognitive_service_endpoint = dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "CognitiveServiceEndpoint")
    
    scoring_storage_account_name = dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "StorageAccountName")
    scoring_storage_account_sas_key= dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "StorageAccountSasKey")  
    
    scoring_mount_point = "/mnt/"+scoring_container_name 
    scoring_source_str = "wasbs://{container}@{storage_acct}.blob.core.windows.net/".format(container=scoring_container_name, storage_acct=scoring_storage_account_name) 
    scoring_conf_key = "fs.azure.sas.{container}.{storage_acct}.blob.core.windows.net".format(container=scoring_container_name, storage_acct=scoring_storage_account_name)
    
    ```

1. Daha sonra, daha önce bağlanmış olması durumunda depolama hesabını çıkaracağız.

    ```python
    try:
      dbutils.fs.unmount(scoring_mount_point) # Use this to unmount as needed
    except:
      print("{} already unmounted".format(scoring_mount_point))
    
    ```

1. Son olarak, depolama hesabını bağlayacağız.


    ```python
    try: 
      dbutils.fs.mount( 
        source = scoring_source_str, 
        mount_point = scoring_mount_point, 
        extra_configs = {scoring_conf_key: scoring_storage_account_sas_key} 
      ) 
    except Exception as e: 
      print("ERROR: {} already mounted. Run previous cells to unmount first".format(scoring_mount_point))
    
    ```

    > [!NOTE]
    > Bu durumda yalnızca eğitim depolama hesabını &mdash; , eğitim dosyalarını ve içindeki anahtar-değer çiftlerini ayıklamak istediğimiz dosyaları aynı depolama hesabında bağladık. Puanlama ve eğitim depolama hesaplarınız farklıysa, her iki depolama hesabını da buraya bağlamanız gerekecektir. 

### <a name="write-the-scoring-notebook"></a>Puanlama Not defterini yazma

Artık bir Puanlama Not defteri oluşturuyoruz. Eğitim not defterine benzer şekilde, yeni bağlandığımız Azure Data Lake depolama hesabındaki klasörlerde depolanan dosyaları kullanacağız. Klasör adı bir değişken olarak geçirilir. Belirtilen klasördeki tüm formların üzerinde döngü yapılacak ve anahtar-değer çiftlerini onlardan ayıklayacağız. 

1. Yeni bir not defteri oluşturun ve bunu **Scoreformtanıyıcı** olarak çağırın. 
1. **Ayarları** ve **bağlamadatalake** not defterlerini yürütün.

    ```python
    %run "./Settings"
    %run "./MountDataLake"
    ```

1. Ardından, [Çözümle](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeWithCustomForm) API 'sini çağıran aşağıdaki kodu ekleyin.

    ```python
    ########### Python Form Recognizer Async Analyze #############
    import json
    import time
    from requests import get, post 
    
    
    #prefix= TrainingBlobFolder
    post_url = cognitive_service_endpoint + "/formrecognizer/v2.0/custom/models/%s/analyze" % model_id
    source = r"/dbfs/mnt/"+scoring_container_name+"/"+scoring_input_blob_folder+"/"+file_to_score_name
    output = r"/dbfs/mnt/"+scoring_container_name+"/scoringforms/ExtractionResult/"+os.path.splitext(os.path.basename(source))[0]+"_output.json"
    
    params = {
        "includeTextDetails": True
    }
    
    headers = {
        # Request headers
        'Content-Type': file_type,
        'Ocp-Apim-Subscription-Key': cognitive_service_subscription_key,
    }
    
    with open(source, "rb") as f:
        data_bytes = f.read()
    
    try:
        resp = post(url = post_url, data = data_bytes, headers = headers, params = params)
        if resp.status_code != 202:
            print("POST analyze failed:\n%s" % json.dumps(resp.json()))
            quit()
        print("POST analyze succeeded:\n%s" % resp.headers)
        get_url = resp.headers["operation-location"]
    except Exception as e:
        print("POST analyze failed:\n%s" % str(e))
        quit() 
    ```

1. Sonraki hücrede, anahtar-değer çiftinin ayıklanmasına ilişkin sonuçları elde edeceğiz. Bu hücre sonucun çıktısını alacak. JSON biçimindeki sonucun Azure SQL veritabanı veya Cosmos DB üzerinde daha fazla işlem yapacağından, sonucu bir. JSON dosyasına yazacağız. Çıkış dosyası adı, "_output.json" ile birleştirilmiş puanlanmış dosyanın adı olacaktır. Dosya, kaynak dosyayla aynı klasörde depolanır.

    ```python
    n_tries = 10
    n_try = 0
    wait_sec = 5
    max_wait_sec = 5
    while n_try < n_tries:
       try:
           resp = get(url = get_url, headers = {"Ocp-Apim-Subscription-Key": cognitive_service_subscription_key})
           resp_json = resp.json()
           if resp.status_code != 200:
               print("GET analyze results failed:\n%s" % json.dumps(resp_json))
               n_try += 1
               quit()
           status = resp_json["status"]
           if status == "succeeded":
               print("Analysis succeeded:\n%s" % json.dumps(resp_json))
               n_try += 1
               quit()
           if status == "failed":
               print("Analysis failed:\n%s" % json.dumps(resp_json))
               n_try += 1
               quit()
           # Analysis still running. Wait and retry.
           time.sleep(wait_sec)
           n_try += 1
           wait_sec = min(2*wait_sec, max_wait_sec)     
       except Exception as e:
           msg = "GET analyze results failed:\n%s" % str(e)
           print(msg)
           n_try += 1
           print("Analyze operation did not complete within the allocated time.")
           quit()
    
    ```
1. Dosya yazma yordamını yeni bir hücrede yapın:

    ```python
    import requests
    file = open(output, "w")
    file.write(str(resp_json))
    file.close()
    ```

## <a name="automate-training-and-scoring-with-azure-data-factory"></a>Eğitim ve Puanlama Azure Data Factory ile otomatikleştirin

Kalan tek adım, eğitim ve Puanlama işlemlerini otomatikleştirmek için Azure Data Factory (ADF) hizmetini ayarlamanıza olanak sağlar. İlk olarak, [Veri Fabrikası oluşturma](../../data-factory/quickstart-create-data-factory-portal.md#create-a-data-factory)bölümündeki adımları izleyin. ADF kaynağını oluşturduktan sonra üç işlem hattı oluşturmanız gerekir: bir eğitim için biri ve skor için iki (aşağıda açıklanmıştır).

### <a name="training-pipeline"></a>Eğitim işlem hattı

Eğitim işlem hattındaki ilk etkinlik, Azure SQL veritabanındaki Parametreleştirme tablosundaki değerleri okumak ve döndürmek için bir aramadır. Tüm eğitim veri kümeleri aynı depolama hesabında ve kapsayıcıda (ancak muhtemelen farklı klasörlerde) olacağı için, arama etkinlik ayarlarında varsayılan değer **ilk satır yalnızca** özniteliğini saklayacağız. Modeli eğitmek için her form türü için, **training_blob_root_folder** içindeki tüm dosyaları kullanarak modeli eğtireceğiz.

:::image type="content" source="./media/tutorial-bulk-processing/training-pipeline.png" alt-text="Data Factory 'de Eğitim işlem hattı":::

Saklı yordam iki parametre alır: **model_id** ve **form_batch_group_id**. Databricks Not defterinden model KIMLIĞINI döndüren kod `dbutils.notebook.exit(model_id)` ve Veri Fabrikası içindeki saklı yordam etkinliğinde kodu okuma kodu `@activity('GetParametersFromMetadaTable').output.firstRow.form_batch_group_id` .

### <a name="scoring-pipelines"></a>Puanlama işlem hatları

Anahtar-değer çiftlerini ayıklamak için, Parametreleştirme tablosundaki tüm klasörleri tarayacağız ve her klasör için içindeki tüm dosyaların anahtar-değer çiftlerini çıkaracağız. Bugün, ADF, iç içe geçmiş ForEach döngülerini desteklemez. Bunun yerine, iki işlem hattı oluşturacağız. İlk işlem hattı, Parametreleştirme tablosundan arama yapılır ve Klasörler listesini ikinci işlem hattına parametre olarak iletmektir.

:::image type="content" source="./media/tutorial-bulk-processing/scoring-pipeline-1a.png" alt-text="Data Factory 'de ilk Puanlama işlem hattı":::

:::image type="content" source="./media/tutorial-bulk-processing/scoring-pipeline-1b.png" alt-text="Data Factory 'de ilk Puanlama ardışık düzeni, Ayrıntılar":::

İkinci işlem hattı, klasördeki dosyaların listesini almak ve bu dosyayı Puanlama Databricks not defterine bir parametre olarak geçirmek için GetMeta etkinliğini kullanır.

:::image type="content" source="./media/tutorial-bulk-processing/scoring-pipeline-2a.png" alt-text="Data Factory 'de ikinci Puanlama işlem hattı":::

:::image type="content" source="./media/tutorial-bulk-processing/scoring-pipeline-2b.png" alt-text="Data Factory 'de ikinci Puanlama işlem hattı, Ayrıntılar":::

### <a name="specify-a-degree-of-parallelism"></a>Paralellik derecesini belirtme

Eğitim ve Puanlama işlem hatları içinde, birden fazla formu aynı anda işlemek için paralellik derecesini belirtebilirsiniz.

ADF ardışık düzeninde paralellik derecesini ayarlamak için:

* Foreach etkinliğini seçin.
* **Sıralı** kutunun işaretini kaldırın.
* **Yığın sayısı** metin kutusunda paralellik derecesini ayarlayın. Puanlama için en fazla 15 toplu iş sayısı önerilir.

:::image type="content" source="./media/tutorial-bulk-processing/parallelism.png" alt-text="ADF 'de puanlama etkinliği için paralellik yapılandırması":::

## <a name="how-to-use"></a>Nasıl kullanılır?

Artık, form kapsamınızı dijital olarak ayırmak ve üzerine bazı analizler çalıştırmak için otomatik bir işlem hattınız vardır. Var olan bir depolama klasörüne tanıdık bir türün yeni biçimlerini eklediğinizde, Puanlama işlem hatlarını yeniden çalıştırmanız yeterlidir ve yeni formlar için çıkış dosyaları da dahil olmak üzere tüm çıkış dosyalarınızı güncelleştirirler. 

Yeni bir türden yeni formlar eklerseniz, uygun kapsayıcıya bir eğitim veri kümesi de yüklemeniz gerekir. Ardından, yeni belgelerin ve eğitim veri kümesinin konumlarını girerek parameterleştirme tablosuna yeni bir satır ekleyin. Yeni bir modelin Bu formlar için eğitilmeli olduğunu belirtmek üzere **model_ID** için-1 değerini girin. Ardından, ADF 'de Eğitim işlem hattını çalıştırın. Tablodan okur, bir modeli eğitme ve tablodaki model KIMLIĞI üzerine yazacak. Ardından, çıkış dosyalarını yazmaya başlamak için Puanlama işlem hatlarını çağırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, büyük bir dosya biriktirme listesini dijital olarak biçimlendirmek için form tanıyıcı modellerinin eğitimini ve çalışmasını tetikleyen Azure Data Factory işlem hatlarını ayarlarsınız. Ardından, bununla ne yapabileceğinizi görmek için form tanıyıcı API 'sini inceleyin.

* [Form tanıyıcı REST API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-2/operations/AnalyzeBusinessCardAsync)