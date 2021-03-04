---
title: 'Öğretici: Azure Data Factory form tanıyıcı kullanarak form verilerini toplu olarak ayıklama'
titleSuffix: Azure Cognitive Services
description: Form tanıyıcı modellerinin eğitimini ve çalışmasını tetiklemek ve büyük bir belge biriktirme listesini dijital olarak biçimlendirmek için Azure Data Factory etkinlikleri ayarlayın.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: tutorial
ms.date: 01/04/2021
ms.author: pafarley
ms.openlocfilehash: d0c95312e1794e2f78bbbef217ef5530a993146d
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102040915"
---
# <a name="tutorial-extract-form-data-in-bulk-by-using-azure-data-factory"></a>Öğretici: Azure Data Factory kullanarak form verilerini toplu olarak ayıklayın

Bu öğreticide, Azure hizmetlerini kullanarak büyük bir form toplu işini dijital medyaya alma hakkında bakacağız. Öğreticide, bir Azure SQL veritabanı 'na ait bir Azure Veri Gölü veri alımını nasıl otomatikleştirebileceğiniz gösterilmektedir. Birkaç tıklama ile modelleri hızlı bir şekilde eğitebilir ve yeni belgeleri işleyebilirsiniz.

## <a name="business-need"></a>İşletme gereksinimi

Çoğu kuruluş artık çeşitli biçimlerde (PDF, resim, video) sahip oldukları verilerin değerini biliyor. Bunlar, en iyi uygulamaları ve bu varlıkları dijital olarak ayırmak için en düşük maliyetli yöntemleri arıyor.

Ayrıca, müşterilerimiz çoğu istemci ve müşterinden gelen çeşitli form türlerine sahiptir. [Hızlı](./quickstarts/client-library.md)başlangıçlardan farklı olarak, bu öğreticide, meta veri odaklı bir yaklaşım kullanarak bir modeli yeni ve farklı form türleri ile otomatik olarak eğitme konusu gösterilmektedir. Verilen form türü için mevcut bir modeliniz yoksa, sistem bir tane oluşturur ve size model KIMLIĞI verir. 

İşletmeler, formlardan verileri ayıklayarak ve mevcut işletimsel sistemlerle ve veri ambarlarıyla birleştirerek müşteriler ve iş kullanıcılarına Öngörüler elde edebilir ve değer sunabilir.

Azure form tanıyıcı, kuruluşların verilerini kullanmasına, işlemleri otomatikleştirmesine (fatura ödemeleri, vergi işleme vb.), paradan ve zamandan tasarruf etmesine ve daha iyi veri doğruluğu avantajlarından yararlanmasına yardımcı olur.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Formlarınızı depolamak için Azure Data Lake ayarlayın.
> * Bir parametrization tablosu oluşturmak için Azure veritabanı kullanın.
> * Hassas kimlik bilgilerini depolamak için Azure Key Vault kullanın.
> * Form tanıyıcı modelinizi bir Azure Databricks not defterinde eğitme.
> * Bir Databricks Not defteri kullanarak form verilerinizi ayıklayın.
> * Azure Data Factory kullanarak form eğitimini ve ayıklamayı otomatikleştirin.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. [Ücretsiz bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services/).
* Azure aboneliğiniz olduktan sonra, <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer"  title=" "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> anahtarınızı ve uç noktanızı almak için Azure Portal bir form tanıyıcı kaynağı oluşturun. Kaynak dağıtıldıktan sonra **Kaynağa git**'i seçin.
    * Uygulamanızı form tanıyıcı API 'sine bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Bu hızlı başlangıçta, anahtarınızı ve uç noktanızı kodunuza yapıştırmanız gerekir.
    * Hizmeti denemek için ücretsiz fiyatlandırma katmanını (F0) kullanabilirsiniz. Daha sonra üretim için daha sonra ücretli bir katmana yükseltebilirsiniz.
* Aynı türde en az beş form kümesi. İdeal olarak, bu iş akışı büyük belge kümelerini desteklemeye yöneliktir. Eğitim veri kümenizi birlikte yerleştirmeye yönelik ipuçları ve seçenekler için bkz. [eğitim veri kümesi oluşturma](./build-training-data-set.md) . Bu öğreticide, [örnek veri kümesinin](https://go.microsoft.com/fwlink/?linkid=2128080)eğitme klasöründeki dosyaları kullanabilirsiniz.

## <a name="project-architecture"></a>Proje mimarisi 

Bu proje, bir Azure Data Lake depolama hesabındaki belgelerden verileri eğiteleyen, çözümleyen ve ayıklayarak Python not defterlerini tetiklemek için bir dizi Azure Data Factory işlem hattı temsil eder.

Form tanıyıcı REST API giriş olarak bazı parametreler gerektiriyor. Güvenlik nedenleriyle, bu parametrelerden bazıları bir Azure Anahtar Kasası 'nda depolanır. Depolama Blobu klasör adı gibi diğer, daha az hassas parametreler, bir Azure SQL veritabanında Parametreleştirme tablosunda depolanır.

Analiz edilecek her form türü için, veri mühendisleri veya veri bilimcileri, parametre tablosunun bir satırını dolduracaktır. Daha sonra, algılanan form türleri listesini yinelemek ve form tanıyıcı modellerini eğitme veya yeniden eğitebilmeniz için ilgili parametreleri bir Databricks not defterine geçirmek üzere Azure Data Factory kullanır. Burada bir Azure işlevi de kullanılabilir.

Azure Databricks Not defteri daha sonra, form verilerini ayıklamak için eğitilen modelleri kullanır. Bu verileri bir Azure SQL veritabanına aktarır.

:::image type="content" source="./media/tutorial-bulk-processing/architecture.png" alt-text="Proje mimarisini gösteren diyagram.":::


## <a name="set-up-azure-data-lake"></a>Azure Data Lake ayarlama

Form kapsamınız şirket içi ortamınızda veya bir FTP sunucusunda olabilir. Bu öğretici bir Azure Data Lake Storage 2. hesabındaki formları kullanır. Dosyalarınızı Azure Data Factory, Azure Depolama Gezgini veya AzCopy kullanarak aktarabilirsiniz. Eğitim ve Puanlama veri kümeleri farklı kapsayıcılarda olabilir, ancak tüm form türleri için eğitim veri kümelerinin aynı kapsayıcıda olması gerekir. (Farklı klasörlerde olabilir.)

Yeni bir Data Lake oluşturmak için [Azure Data Lake Storage 2. ile kullanmak üzere depolama hesabı oluşturma](../../storage/blobs/create-data-lake-storage-account.md)bölümündeki yönergeleri izleyin.

## <a name="create-a-parameterization-table"></a>Parametreleştirme tablosu oluşturma

Daha sonra, bir Azure SQL veritabanında meta veri tablosu oluşturacağız. Bu tablo, REST API, form tanıyıcı tarafından gereken hassas olmayan verileri içerir. Veri kümesinde yeni bir tür form olduğunda, bu tabloya yeni bir kayıt ekleyecek ve eğitim ve Puanlama işlem hatlarını tetikleyeceğiz. (Bu işlem hatlarını daha sonra uygulayacağız.)

Bu alanlar tabloda kullanılacaktır:

* **form_description**. Eğitimin bir parçası olarak gerekli değildir. Bu alan, modeli eğititireceğimiz form türünün bir açıklamasını sağlar (örneğin, "istemci A formları," "otel B formları").
* **training_container_name**. Eğitim veri kümesini depoladığınız depolama hesabı kapsayıcısının adı. **Scoring_container_name** aynı kapsayıcı olabilir.
* **training_blob_root_folder**. Depolama hesabındaki, modelin eğitimi için dosyaları depolayacağımız klasör.
* **scoring_container_name**. Anahtar/değer çiftlerini ayıklamak istediğimiz dosyaları depoladığınız depolama hesabı kapsayıcısının adı. **Training_container_name** aynı kapsayıcı olabilir.
* **scoring_input_blob_folder**. Depolama hesabındaki, verileri Ayıklanacak dosyaları depolayacağımız klasör.
* **model_id**. Yeniden eğitebilmemiz istediğimiz modelin KIMLIĞI. İlk çalıştırma için değer-1 ' e ayarlanmalıdır, bu da eğitim Not defterinin eğitebileceği yeni bir özel model oluşturmasına neden olur. Eğitim Not defteri yeni model KIMLIĞINI Azure Data Factory örneğine döndürür. Saklı yordam etkinliğini kullanarak bu değeri Azure SQL veritabanında güncelleştireceğiz.

  Yeni bir form türünü almak istediğinizde, modeli eğitmadan önce model KIMLIĞINI-1 ' e el ile sıfırlamanız gerekir.

* **file_type**. Desteklenen form türleri `application/pdf` ,, `image/jpeg` `image/png` ve `image/tif` .

  Başka dosya türlerinde formunuz varsa, yeni bir form türünü eğitedığınızda bu değeri değiştirmeniz ve **model_id** gerekir.
* **form_batch_group_id**. Zaman içinde, aynı modele göre eğitmeniz gereken birden çok form türüne sahip olabilirsiniz. **Form_batch_group_id** alanı, belirli bir modelle eğitim verilen tüm form türlerini belirtmenize olanak tanır.

### <a name="create-the-table"></a>Tabloyu oluşturma


[Bir Azure SQL veritabanı oluşturun](https://ms.portal.azure.com/#create/Microsoft.SQLDatabase)ve sonra gerekli tabloyu oluşturmak IÇIN bu SQL betiğini [sorgu Düzenleyicisi](../../azure-sql/database/connect-query-portal.md) 'nde çalıştırın:


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

Eğitilen sonrasında **model_id** otomatik olarak güncelleştiren yordamı oluşturmak için bu betiği çalıştırın.

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

### <a name="create-an-azure-key-vault"></a>Azure Anahtar Kasası oluşturma

[Key Vault kaynağı oluşturun](https://ms.portal.azure.com/#create/Microsoft.KeyVault). Sonra, oluşturulduktan sonra Key Vault kaynağına gidin ve **Ayarlar** bölümünde, parametreleri eklemek için **gizli** dizileri seçin.

Yeni bir pencere görüntülenir. **Oluştur/içeri aktar**' ı seçin. Parametrenin adını ve değerini girip **Oluştur**' u seçin. Bu adımları aşağıdaki parametreler için doldurun:

* **Biliveserviceendpoint**. Form tanıyıcı API 'nizin uç nokta URL 'SI.
* **Biliveservicesubscriptionkey**. Form tanıyıcı hizmetiniz için erişim anahtarı. 
* **StorageAccountName**. Eğitim veri kümesinin ve içindeki anahtar/değer çiftlerini ayıklamak istediğiniz formların depolanacağı depolama hesabı. Bu öğeler farklı hesaplarınızda, her hesap adını ayrı bir gizli dizi olarak girin. Eğitim veri kümelerinin tüm form türleri için aynı kapsayıcıda olması gerektiğini unutmayın. Farklı klasörlerde olabilirler.
* **Storageaccountsaskey**. Depolama hesabının paylaşılan erişim imzası (SAS). SAS URL 'sini almak için depolama kaynağına gidin. **Depolama Gezgini** sekmesinde, kapsayıcınıza gidin, sağ tıklayın ve **paylaşılan erişim imzasını al**' ı seçin. Depolama hesabının kendisi için değil, kapsayıcınıza yönelik SAS almak önemlidir. **Okuma** ve **Listeleme** izinlerinin seçili olduğundan emin olun ve ardından **Oluştur**' u seçin. Sonra **URL** bölümündeki değeri kopyalayın. Şu biçimde olmalıdır: `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>` .

## <a name="train-your-form-recognizer-model-in-a-databricks-notebook"></a>Form tanıyıcı modelinizi bir Databricks not defterinde eğitme

Form tanıyıcı hizmetiyle etkileşim kuran Python kodunu depolamak ve çalıştırmak için Azure Databricks kullanacaksınız.

### <a name="create-a-notebook-in-databricks"></a>Databricks 'te Not defteri oluşturma

Azure portal [bir Azure Databricks kaynağı oluşturun](https://ms.portal.azure.com/#create/Microsoft.Databricks) . Oluşturulduktan sonra kaynağa gidin ve çalışma alanını açın.

### <a name="create-a-secret-scope-backed-by-azure-key-vault"></a>Azure Key Vault tarafından desteklenen gizli bir kapsam oluşturun


Yukarıda oluşturduğunuz Azure Anahtar Kasası 'nda bulunan gizli dizileri oluşturmak için, Databricks içinde bir gizli dizi kapsamı oluşturmanız gerekir. Buradaki adımları izleyin: [Azure Key Vault ile desteklenen gizli dizi kapsamı oluşturma](/azure/databricks/security/secrets/secret-scopes#--create-an-azure-key-vault-backed-secret-scope).

### <a name="create-a-databricks-cluster"></a>Databricks kümesi oluşturma

Küme, Databricks hesaplama kaynakları koleksiyonudur. Bir küme oluşturmak için:

1. Sol bölmede, **kümeler** düğmesini seçin.
1. **Kümeler** sayfasında **küme oluştur**' u seçin.
1. **Küme oluştur** sayfasında, bir küme adı belirtin ve **Databricks Runtime sürüm** listesinde **7,2 (Scala 2,12, Spark 3.0.0)** öğesini seçin.
1. **Küme Oluştur**’u seçin.

### <a name="write-a-settings-notebook"></a>Bir ayarlar Not defteri yazın

Artık Python Not defterleri eklemeye hazırsınız. İlk olarak, **Ayarlar** adlı bir not defteri oluşturun. Bu not defteri, Parametreleştirme tablonuzdaki değerleri betikteki değişkenlere atayacaktır. Azure Data Factory, değerleri daha sonra parametreler olarak ' a geçirecek. Ayrıca, anahtar kasasındaki gizli dizilerle değişkenlere değerler atayacağız. 

1. **Ayarlar** Not defterini oluşturmak Için **çalışma alanı** düğmesini seçin. Yeni sekmesinde, açılan listeyi seçin ve ardından **Oluştur** ve sonra **Not defteri**' ni seçin.
1. Açılır pencerede, Not defteri için bir ad girin ve varsayılan dil olarak **Python** ' ı seçin. Databricks kümenizi seçip **Oluştur**' u seçin.
1. İlk not defteri hücresinde, Azure Data Factory geçirilen parametreleri alıyoruz:

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

1. İkinci hücrede Key Vault gizli dizileri alır ve bunları değişkenlere atayacağız:

    ```python 
    cognitive_service_subscription_key = dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "CognitiveserviceSubscriptionKey")
    cognitive_service_endpoint = dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "CognitiveServiceEndpoint")
    
    training_storage_account_name = dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "StorageAccountName")
    storage_account_sas_key= dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "StorageAccountSasKey")  
    
    ScoredFile = file_to_score_name+ "_output.json"
    training_storage_account_url="https://"+training_storage_account_name+".blob.core.windows.net/"+training_container_name+storage_account_sas_key 
    ```

### <a name="write-a-training-notebook"></a>Eğitim Not defteri yazma

Artık **Ayarlar** Not defterini tamamladığımıza göre, modeli eğitebilmeniz için bir not defteri oluşturuyoruz. Yukarıda belirtildiği gibi, bir Azure Data Lake Storage 2. hesabındaki bir klasörde depolanan dosyaları kullanacağız (**training_blob_root_folder**). Klasör adı bir değişken olarak geçirildi. Her form türü kümesi aynı klasörde olacaktır. Parametre tablosunda döngü yaparken, tüm form türlerini kullanarak modeli eğtireceğiz. 

1. **Trainformtanıyıcı** adlı bir not defteri oluşturun. 
1. İlk hücrede **Ayarlar** Not defterini çalıştırın:

    ```python
    %run "./Settings"
    ```

1. Sonraki hücrede, ayarlar dosyasından değişkenler atayın ve her form türü için modeli dinamik olarak eğitme, bu kodu [rest hızlı](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/FormRecognizer/rest/python-train-extract.md#get-training-results%20)başlangıca uygulayın.

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
        # Request headers.
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
    if model_id=="-1": # If you don't already have a model you want to retrain. In this case, we create a model and use it to extract the key/value pairs.
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
    else :# If you already have a model you want to retrain, we reuse it and (re)train with the new form types.  
      try:
        get_url =post_url+r"/"+model_id
          
      except Exception as e:
          print("POST model failed:\n%s" % str(e))
          quit()
    ```

1. Eğitim sürecinin son adımı, eğitimin bir JSON biçiminde elde edilmesine neden olur:

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

## <a name="extract-form-data-by-using-a-notebook"></a>Bir not defteri kullanarak form verilerini ayıklama

### <a name="mount-the-azure-data-lake-storage"></a>Azure Data Lake depolamayı bağlama

Bir sonraki adım, eğitilen modeli kullanarak yaptığımız birçok formu puanlandırıyoruz. Azure Data Lake Storage hesabını Databricks 'e bağlayacağız ve alma işlemi sırasında takmaya başvuracağız.

Eğitim aşamasında yaptığımız gibi, formlardan anahtar/değer çiftleri ayıklanmasını çağırmak için Azure Data Factory kullanacağız. Parametre tablosunda belirtilen klasörlerdeki formların üzerinde döngü göndereceğiz.

1. Depolama hesabını Databricks 'e bağlamak için Not defteri oluşturun. **Mount veri** Gölü çağırın. 
1. Önce **Ayarlar** Not defterini çağırmanız gerekir:

    ```python
    %run "./Settings"
    ```

1. İkinci hücrede, hassas parametrelerin değişkenlerini tanımlayın ve bu Key Vault gizli dizilerimizden elde edeceğiz:

    ```python
    cognitive_service_subscription_key = dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "CognitiveserviceSubscriptionKey")
    cognitive_service_endpoint = dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "CognitiveServiceEndpoint")
    
    scoring_storage_account_name = dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "StorageAccountName")
    scoring_storage_account_sas_key= dbutils.secrets.get(scope = "FormRecognizer_SecretScope", key = "StorageAccountSasKey")  
    
    scoring_mount_point = "/mnt/"+scoring_container_name 
    scoring_source_str = "wasbs://{container}@{storage_acct}.blob.core.windows.net/".format(container=scoring_container_name, storage_acct=scoring_storage_account_name) 
    scoring_conf_key = "fs.azure.sas.{container}.{storage_acct}.blob.core.windows.net".format(container=scoring_container_name, storage_acct=scoring_storage_account_name)
    
    ```

1. Daha önce bağlanmış olması durumunda depolama hesabını çıkarmasını deneyin:

    ```python
    try:
      dbutils.fs.unmount(scoring_mount_point) # Use this to unmount as needed
    except:
      print("{} already unmounted".format(scoring_mount_point))
    
    ```

1. Depolama hesabını bağlama:


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
    > Yalnızca eğitim depolama hesabını bağladık. Bu durumda, eğitim dosyaları ve içindeki anahtar/değer çiftlerini ayıklamak istediğimiz dosyalar aynı depolama hesabıdır. Puanlama ve eğitim depolama hesaplarınız farklıysa, her iki depolama hesabını da bağlamanız gerekir. 

### <a name="write-the-scoring-notebook"></a>Puanlama Not defterini yazma

Artık bir Puanlama Not defteri oluşturuyoruz. Eğitim not defterinde yaptığımız gibi bir şey yapacağız: yeni bağlandığımız Azure Data Lake Storage hesaptaki klasörlerde depolanan dosyaları kullanacağız. Klasör adı bir değişken olarak geçirilir. Belirtilen klasördeki tüm formların üzerinde döngü yapılacak ve anahtar/değer çiftlerini onlardan ayıklayacağız. 

1. Bir not defteri oluşturun ve bunu **Scoreformtanıyıcı** olarak çağırın. 
1. **Ayarları** ve **bağlamadatalake** not defterlerini çalıştırın:

    ```python
    %run "./Settings"
    %run "./MountDataLake"
    ```

1. [Çözümle](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeWithCustomForm) API 'sini çağıran bu kodu ekleyin:

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
        # Request headers.
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

1. Sonraki hücrede, anahtar/değer çiftinin ayıklanmasına ilişkin sonuçları elde edeceğiz. Bu hücre sonucun çıktısını alacak. Azure SQL veritabanı veya Azure Cosmos DB daha fazla işleyebilmemiz için JSON biçiminde sonuç istiyoruz. Bu nedenle, sonucu bir. JSON dosyasına yazacak. Çıkış dosyası adı, "_output.json" ile birleştirilmiş puanlanmış dosyanın adı olacaktır. Dosya, kaynak dosyayla aynı klasörde depolanır.

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

## <a name="automate-training-and-scoring-by-using-azure-data-factory"></a>Azure Data Factory kullanarak eğitimi ve Puanlama otomatikleştirme

Kalan tek adım eğitim ve Puanlama işlemlerini otomatikleştirmek için Azure Data Factory hizmetini ayarlamanıza olanak sağlar. İlk olarak, [Veri Fabrikası oluşturma](../../data-factory/quickstart-create-data-factory-portal.md#create-a-data-factory)bölümündeki adımları izleyin. Azure Data Factory kaynağını oluşturduktan sonra üç işlem hattı oluşturmanız gerekir: bir eğitim için biri ve skor için iki. (Daha sonra açıklayacağız.)

### <a name="training-pipeline"></a>Eğitim işlem hattı

Eğitim işlem hattındaki ilk etkinlik, Azure SQL veritabanındaki Parametreleştirme tablosundaki değerleri okumak ve döndürmek için bir aramadır. Tüm eğitim veri kümeleri aynı depolama hesabında ve kapsayıcıda (ancak muhtemelen farklı klasörlerde) olacaktır. Bu nedenle, varsayılan öznitelik değeri **ilk satırı yalnızca** arama etkinliği ayarlarında tutulacağız. Modeli eğitmek için her form türü için, **training_blob_root_folder** içindeki tüm dosyaları kullanarak modeli eğtireceğiz.

:::image type="content" source="./media/tutorial-bulk-processing/training-pipeline.png" alt-text="Data Factory bir eğitim işlem hattını gösteren ekran görüntüsü.":::

Saklı yordam iki parametre alır: **model_id** ve **form_batch_group_id**. Databricks Not defterinden model KIMLIĞINI döndürecek kod `dbutils.notebook.exit(model_id)` . Data Factory içindeki saklı yordam etkinliğinde kodu okuma kodu `@activity('GetParametersFromMetadaTable').output.firstRow.form_batch_group_id` .

### <a name="scoring-pipelines"></a>Puanlama işlem hatları

Anahtar/değer çiftlerini ayıklamak için, Parametreleştirme tablosundaki tüm klasörleri tarayacağız. Her klasör için, içindeki tüm dosyaların anahtar/değer çiftlerini ayıklayacağız. Azure Data Factory, iç içe geçmiş ForEach döngülerini desteklememektedir. Bunun yerine, iki işlem hattı oluşturacağız. İlk işlem hattı, Parametreleştirme tablosundan arama yapılır ve Klasörler listesini ikinci işlem hattına parametre olarak iletmektir.

:::image type="content" source="./media/tutorial-bulk-processing/scoring-pipeline-1a.png" alt-text="Data Factory ilk Puanlama ardışık düzenini gösteren ekran görüntüsü.":::

:::image type="content" source="./media/tutorial-bulk-processing/scoring-pipeline-1b.png" alt-text="Data Factory ilk Puanlama ardışık düzenine ilişkin ayrıntıları gösteren ekran görüntüsü.":::

İkinci işlem hattı, klasördeki dosyaların listesini almak ve bu dosyayı Puanlama Databricks not defterine bir parametre olarak geçirmek için GetMeta etkinliğini kullanır.

:::image type="content" source="./media/tutorial-bulk-processing/scoring-pipeline-2a.png" alt-text="Data Factory ikinci Puanlama ardışık düzenini gösteren ekran görüntüsü.":::

:::image type="content" source="./media/tutorial-bulk-processing/scoring-pipeline-2b.png" alt-text="Data Factory ikinci Puanlama ardışık düzenine ilişkin ayrıntıları gösteren ekran görüntüsü.":::

### <a name="specify-a-degree-of-parallelism"></a>Paralellik derecesini belirtme

Eğitim ve Puanlama işlem hatlarında, birden fazla formu eşzamanlı olarak işleyebilmeniz için paralellik derecesini belirtebilirsiniz.

Azure Data Factory ardışık düzeninde paralellik derecesini ayarlamak için:

1. **Foreach** etkinliğini seçin.
1. **Sıralı** kutuyu temizleyin.
1. **Yığın sayısı** kutusunda paralellik derecesini ayarlayın. Puanlama için en fazla 15 toplu iş sayısı önerilir.

:::image type="content" source="./media/tutorial-bulk-processing/parallelism.png" alt-text="Azure Data Factory Puanlama etkinliğinin paralellik yapılandırmasını gösteren ekran görüntüsü.":::

## <a name="how-to-use-the-pipeline"></a>İşlem hattını kullanma

Artık, form kapsamınızı dijital olarak ayırmak ve üzerine bazı analizler çalıştırmak için otomatik bir işlem hattınız vardır. Mevcut bir depolama klasörüne tanıdık bir türün yeni biçimlerini eklediğinizde, Puanlama işlem hatlarını yeniden çalıştırmanız yeterlidir. Yeni formlar için çıkış dosyaları da dahil olmak üzere tüm çıkış dosyalarınızı güncelleştirirler. 

Yeni bir türden yeni formlar eklerseniz, uygun kapsayıcıya bir eğitim veri kümesi de yüklemeniz gerekir. Daha sonra, yeni belgelerin ve eğitim veri kümesinin konumlarını girerek parameterleştirme tablosuna yeni bir satır ekleyeceksiniz. Form için yeni bir modelin eğitililmesi gerektiğini belirtmek üzere **model_ID** için-1 değerini girin. Ardından Azure Data Factory 'de Eğitim işlem hattını çalıştırın. İşlem hattı tablodan okur, bir modeli eğitme ve tablodaki model KIMLIĞI üzerine yazacak. Daha sonra, çıktı dosyalarını yazmaya başlamak için Puanlama işlem hatlarını çağırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, form tanıyıcı modellerinin eğitimini ve çalışmasını tetikleyip büyük bir dosya biriktirme listesi oluşturmak için Azure Data Factory işlem hatlarını ayarlarsınız. Ardından, bununla ne yapabileceğinizi görmek için form tanıyıcı API 'sini inceleyin.

* [Form tanıyıcı REST API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-2/operations/AnalyzeBusinessCardAsync)
