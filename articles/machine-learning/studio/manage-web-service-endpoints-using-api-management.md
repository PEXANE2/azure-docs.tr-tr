---
title: API Yönetimi'ni kullanarak web hizmetlerini yönetme
titleSuffix: ML Studio (classic) - Azure
description: API Yönetimi'ni kullanarak AzureML web hizmetlerinin nasıl yönetilendiğini gösteren bir kılavuz. Kullanıcı erişimini, kullanımı azaltmayı ve pano izlemesini tanımlayarak REST API uç noktalarınızı yönetin.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.date: 11/03/2017
ms.openlocfilehash: 7064101c21c11b48d8616dbeaa2fd9075660fd3b
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80473460"
---
# <a name="manage-azure-machine-learning-studio-classic-web-services-using-api-management"></a>API Yönetimi'ni kullanarak Azure Machine Learning Studio (klasik) web hizmetlerini yönetme

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

## <a name="overview"></a>Genel Bakış
Bu kılavuz, Azure Machine Learning Studio (klasik) web hizmetlerinizi yönetmek için API Yönetimi'ni kullanmaya nasıl hızlı bir şekilde başalacağınız gösterilmektedir.

## <a name="what-is-azure-api-management"></a>Azure API Management nedir?
Azure API Yönetimi, kullanıcı erişimini, kullanımı azaltmayı ve pano izlemeyi tanımlayarak REST API uç noktalarınızı yönetmenize olanak tanıyan bir Azure hizmetidir. Daha fazla ayrıntı için [Azure API yönetim sitesine](https://azure.microsoft.com/services/api-management/) bakın. Azure API Yönetimi'ne başlamak için [alma ve yayımlama kılavuzuna](/azure/api-management/import-and-publish)bakın. Bu kılavuzun dayandığı bu diğer kılavuz, bildirim yapılandırmaları, katman fiyatlandırması, yanıt işleme, kullanıcı kimlik doğrulaması, ürün oluşturma, geliştirici abonelikleri ve kullanım panosu dahil olmak üzere daha fazla konuyu kapsar.

## <a name="prerequisites"></a>Ön koşullar
Bu kılavuzu tamamlamak için şunları yapmanız gerekir:

* Bir Azure hesabı.
* AzureML hesabı.
* Web hizmeti olarak dağıtılan bir AzureML denemesiiçin çalışma alanı, hizmet ve api_key. AzureML denemesini nasıl oluşturabilirsiniz hakkında ayrıntılı bilgi için [Studio quickstart'a](create-experiment.md)bakın. Bir Stüdyo (klasik) denemesini web hizmeti olarak nasıl dağıtılabildiğiniz hakkında bilgi için, AzureML denemesini web hizmeti olarak nasıl dağıtılanın nasıl dağıtılanın ayrıntıları için [Stüdyo dağıtımına](deploy-a-machine-learning-web-service.md) bakın. Alternatif olarak, Ek A'nın basit bir AzureML denemesini oluşturma ve test etme ve bunu bir web hizmeti olarak dağıtma yönergeleri vardır.

## <a name="create-an-api-management-instance"></a>API Management örneği oluşturma

Azure Machine Learning web hizmetinizi bir API Yönetimi örneğiyle yönetebilirsiniz.

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. **+ Kaynak oluştur**’u seçin.
3. Arama kutusunda "API yönetimi" yazın ve ardından "API yönetimi" kaynağını seçin.
4. **Oluştur'u**tıklatın.
5. **Ad** değeri benzersiz bir URL oluşturmak için kullanılır (bu örnekte "demoazureml" kullanılır).
6. Hizmet örneğiniz için **Abonelik,** **Kaynak grubu**ve **Konum'u** seçin.
7. **Kuruluş adı** için bir değer belirtin (bu örnekte "demoazureml" kullan).
8. Yönetici **e-postanızı** girin - bu e-posta API Yönetim sisteminden gelen bildirimler için kullanılacaktır.
9. **Oluştur'u**tıklatın.

Yeni bir hizmetin oluşturulması 30 dakika kadar sürebilir.

![oluşturma-hizmet](./media/manage-web-service-endpoints-using-api-management/create-service.png)


## <a name="create-the-api"></a>API oluşturma
Hizmet örneği oluşturulduktan sonra, bir sonraki adım API'yi oluşturmaktır. Bir API, istemci uygulamasından çağrılabilen işlemler grubundan oluşur. API işlemleri mevcut web hizmetlerine taşınır. Bu kılavuz, varolan AzureML RRS ve BES web hizmetlerine proxy olan API'ler oluşturur.

API oluşturmak için:

1. Azure portalında oluşturduğunuz hizmet örneğini açın.
2. Sol gezinti bölmesinde **API'leri**seçin.

   ![api-management-menu](./media/manage-web-service-endpoints-using-api-management/api-management.png)

1. **API Ekle'yi**tıklatın.
2. Bir **Web API adı** girin (bu örnekte "AzureML Demo API" kullanır).
3. **Web hizmeti URL'si**için , enter "`https://ussouthcentral.services.azureml.net`".
4. **Web API URL soneki girin". Bu, müşterilerin hizmet örneğine istek göndermek için kullanacağı URL'nin son bölümü olur (bu örnekte "azureml-demo" kullanılır).
5. **Web API URL düzeni**için **HTTPS'yi**seçin.
6. **Ürünler**için **Başlangıç'ı**seçin.
7. **Kaydet**'e tıklayın.


## <a name="add-the-operations"></a>İşlemleri ekleme

İşlemler publisher portalındaki bir API'ye eklenir ve yapılandırılır. Yayımcı portalına erişmek için API Yönetimi hizmetiniz için Azure portalındaki **Publisher portalını** tıklatın, **API'leri**, **İşlemleri**seçin ve ardından **İşlem Ekle'yi**tıklatın.

![ekleme işlemi](./media/manage-web-service-endpoints-using-api-management/add-an-operation.png)

**Yeni işlem** penceresi görüntülenir ve **İmza** sekmesi varsayılan olarak seçilir.

## <a name="add-rrs-operation"></a>RRS İşlemi Ekle
Önce AzureML RRS hizmeti için bir işlem oluşturun:

1. HTTP **fiili**için **POST'u**seçin.
2. URL **şablonu**için`/workspaces/{workspace}/services/{service}/execute?api-version={apiversion}&details={details}`" yazın" yazın.
3. Bir **Görüntü adı** girin (bu örnekte "RRS Execute" kullanır).

   ![add-rrs-operasyon-imza](./media/manage-web-service-endpoints-using-api-management/add-rrs-operation-signature.png)

4. **Soldaki Yanıtlar** > **EKLE'ye** tıklayın ve **200 Ok'u**seçin.
5. Bu işlemi kaydetmek için **Kaydet'i** tıklatın.

   ![add-rrs-operasyon-yanıt](./media/manage-web-service-endpoints-using-api-management/add-rrs-operation-response.png)

## <a name="add-bes-operations"></a>BES İşlemleri Ekle

> [!NOTE]
> RRS işlemini eklemek için olanlara çok benzedikleri için ekran görüntüleri BES işlemleri için burada yer almaz.

### <a name="submit-but-not-start-a-batch-execution-job"></a>Toplu Yürütme işi gönderme (ancak başlatmama)

1. API'ye BIR BES işlemi eklemek için **işlem ekle'yi** tıklatın.
2. HTTP **fiili**için **POST'u**seçin.
3. URL **şablonu**için`/workspaces/{workspace}/services/{service}/jobs?api-version={apiversion}`" yazın" yazın.
4. Bir **Görüntü adı** girin (bu örnekte "BES Gönder" kullanır).
5. **Soldaki Yanıtlar** > **EKLE'ye** tıklayın ve **200 Ok'u**seçin.
6. **Kaydet**'e tıklayın.

### <a name="start-a-batch-execution-job"></a>Toplu Yürütme işini başlatma

1. API'ye BIR BES işlemi eklemek için **işlem ekle'yi** tıklatın.
2. HTTP **fiili**için **POST'u**seçin.
3. HTTP **fiili**için ,`/workspaces/{workspace}/services/{service}/jobs/{jobid}/start?api-version={apiversion}`type " ".
4. Bir **Görüntü adı** girin (bu örnekte "BES Start" kullanır).
6. **Soldaki Yanıtlar** > **EKLE'ye** tıklayın ve **200 Ok'u**seçin.
7. **Kaydet**'e tıklayın.

### <a name="get-the-status-or-result-of-a-batch-execution-job"></a>Toplu Yürütme işinin durumunu veya sonucunu alma

1. API'ye BIR BES işlemi eklemek için **işlem ekle'yi** tıklatın.
2. HTTP **fiili**için **GET'i**seçin.
3. URL **şablonu**için`/workspaces/{workspace}/services/{service}/jobs/{jobid}?api-version={apiversion}`" yazın" yazın.
4. Bir **Görüntü adı** girin (bu örnekte "BES Durumu" kullanır).
6. **Soldaki Yanıtlar** > **EKLE'ye** tıklayın ve **200 Ok'u**seçin.
7. **Kaydet**'e tıklayın.

### <a name="delete-a-batch-execution-job"></a>Toplu Yürütme işini silme

1. API'ye BIR BES işlemi eklemek için **işlem ekle'yi** tıklatın.
2. HTTP **fiili için** **DELETE'i**seçin.
3. URL **şablonu**için`/workspaces/{workspace}/services/{service}/jobs/{jobid}?api-version={apiversion}`" yazın" yazın.
4. Bir **Görüntü adı** girin (bu örnekte "BES Delete") bulunur.
5. **Soldaki Yanıtlar** > **EKLE'ye** tıklayın ve **200 Ok'u**seçin.
6. **Kaydet**'e tıklayın.

## <a name="call-an-operation-from-the-developer-portal"></a>Geliştirici portalından bir işlem çağrısı

İşlemler, bir API'nin işlemlerini görüntülemek ve test etmek için uygun bir yol sağlayan Geliştirici portalından doğrudan çağrılabilir. Bu adımda, **AzureML Demo API'sine**eklenen **RRS Yürütme** yöntemini çağırırsınız. 

1. **Geliştirici portalına**tıklayın.

   ![geliştirici portalı](./media/manage-web-service-endpoints-using-api-management/developer-portal.png)

2. Üst menüden **API'leri** tıklatın ve ardından kullanılabilir işlemleri görmek için **AzureML Demo API'sini** tıklatın.

   ![demoazureml-api](./media/manage-web-service-endpoints-using-api-management/demoazureml-api.png)

3. İşlem için **RRS Execute'ı** seçin. **Deneyin'e**tıklayın.

   ![deneyin-it](./media/manage-web-service-endpoints-using-api-management/try-it.png)

4. **İstek parametreleri**için, **çalışma alanınızı** ve **hizmetinizi**yazın, **"apiversion**için 2.0" yazın ve **ayrıntılar**için "doğru" yazın. **Çalışma alanınızı** ve **hizmetinizi** AzureML web hizmeti panosunda bulabilirsiniz (bkz. Ek A'daki **web hizmetini test edin).**

   **İstek üstbilgisi**için **üstbilgi ekle'yi** tıklatın ve "İçerik Türü" ve "uygulama/json" yazın. **Üstbilgi** yeniden ekle'yi tıklatın ve "Yetkilendirme" ve * \<"Hizmet\>API-KEY'inizi*taşıyan" yazın. API-KEY'inizi AzureML web hizmeti panosunda bulabilirsiniz (bkz. Ek A'daki **web hizmetini test edin).**

   **İstek gövdesi**için `{"Inputs": {"input1": {"ColumnNames": ["Col2"], "Values": [["This is a good day"]]}}, "GlobalParameters": {}}`, yazın.

   ![azureml-demo-api](./media/manage-web-service-endpoints-using-api-management/azureml-demo-api.png)

5. **Gönder**’e tıklayın.

   ![gönderme](./media/manage-web-service-endpoints-using-api-management/send.png)

Bir işlem çağrıldıktan sonra, geliştirici portalı arka uç hizmetinden **Istenen URL'yi,** Yanıt **durumunu,** Yanıt **üstbilgisini**ve yanıt **içeriğini**görüntüler.

![yanıt durumu](./media/manage-web-service-endpoints-using-api-management/response-status.png)

## <a name="appendix-a---creating-and-testing-a-simple-azureml-web-service"></a>Ek A - Basit bir AzureML web hizmeti oluşturma ve test etme
### <a name="creating-the-experiment"></a>Denemeoluşturma
Aşağıda basit bir AzureML denemesi oluşturma ve web hizmeti olarak dağıtma adımları verilmiştir. Web hizmeti rasgele metin sütunu olarak alır ve tamsayılar olarak temsil edilen bir dizi özellik döndürür. Örnek:

| Metin | Haşdi Metin |
| --- | --- |
| Bugün güzel bir gün. |1 1 2 2 0 2 0 1 |

İlk olarak, seçtiğiniz bir tarayıcıyı [https://studio.azureml.net/](https://studio.azureml.net/) kullanarak şu adresine gidin: ve oturum açmak için kimlik bilgilerinizi girin. Ardından, yeni bir boş deneme oluşturun.

![arama-deneme şablonları](./media/manage-web-service-endpoints-using-api-management/search-experiment-templates.png)

**SimpleFeatureHashingExperiment**olarak yeniden adlandırın. **Kaydedilen Veri Kümelerini** genişletin ve **Amazon'daki Kitap İncelemelerini** denemenize sürükleyin.

![basit özellik-karma-deney](./media/manage-web-service-endpoints-using-api-management/simple-feature-hashing-experiment.png)

**Veri Dönüşümü** ve **Düzenlemeyi** genişletin ve **Dataset'teki Select Sütunlarını** denemenize sürükleyin. **Kitap Yorumlarını Amazon'dan** **Veri Kümesi'ndeki Sütunları Seçin'e**bağlayın.

![Kitap incelemeleri veri seti modülünü Proje Sütunları modülüne bağlama](./media/manage-web-service-endpoints-using-api-management/project-columns.png)

**Dataset'te Sütunları Seç'i** tıklatın ve ardından **Sütun Seçici'yi Başlat'ı** tıklatın ve **Col2'yi**seçin. Bu değişiklikleri uygulamak için onay işaretini tıklatın.

![Sütun adlarını kullanarak sütunları seçme](./media/manage-web-service-endpoints-using-api-management/select-columns.png)

**Metin Analizini** genişletin ve **Özellik Karmasını** denemenin üzerine sürükleyin. Karma özelliğine **veri kümesindeki Sütunları Seç'i** bağlayın. **Feature Hashing**

![connect-project-columns](./media/manage-web-service-endpoints-using-api-management/connect-project-columns.png)

**Hashing bitboyutu** **için** 3 yazın. Bu, 8 (23) sütun oluşturur.

![karma-bit boyutu](./media/manage-web-service-endpoints-using-api-management/hashing-bitsize.png)

Bu noktada, denemeyi sınamak için **Çalıştır'ı** tıklatmak isteyebilirsiniz.

![Çalıştırmak](./media/manage-web-service-endpoints-using-api-management/run.png)

### <a name="create-a-web-service"></a>Web hizmeti oluşturma
Şimdi bir web hizmeti oluşturun. **Web Hizmeti'ni** genişletin ve **Girişi** denemenize sürükleyin. **Girişi** **Özellik Karmasına**Bağlayın. Ayrıca **çıktıyı** denemenize sürükleyin. **Çıkışı** **Özellik Karmasına**Bağlayın.

![çıkış-to-özellik-karma](./media/manage-web-service-endpoints-using-api-management/output-to-feature-hashing.png)

**Web hizmetini yayımla'yı**tıklatın.

![publish-web-service](./media/manage-web-service-endpoints-using-api-management/publish-web-service.png)

Denemeyi yayınlamak için **Evet'i** tıklatın.

![evet-to-yayın](./media/manage-web-service-endpoints-using-api-management/yes-to-publish.png)

### <a name="test-the-web-service"></a>Web hizmetini test etme
AzureML web hizmeti RSS (istek/yanıt hizmeti) ve BES (toplu yürütme hizmeti) uç noktalarından oluşur. RSS senkron infaz içindir. BES, eşzamanlı iş yürütme içindir. Web hizmetinizi aşağıdaki örnek Python kaynağıyla test etmek için Python için Azure SDK'sını indirmeniz ve yüklemeniz gerekebilir (bkz. [Python nasıl yüklenir).](/azure/developer/python/azure-sdk-install)

Ayrıca aşağıdaki örnek kaynak için denemeçalışma **alanı,** **hizmet**ve **api_key** gerekir. Web hizmeti panosundaki denemeniz için **İstek/Yanıt** veya **Toplu Yürütme'yi** tıklatarak çalışma alanını ve hizmeti bulabilirsiniz.

![bul-çalışma alanı ve hizmet](./media/manage-web-service-endpoints-using-api-management/find-workspace-and-service.png)

**Denemenizi** web hizmeti panosunda tıklayarak api_key bulabilirsiniz.

![bul-api-anahtar](./media/manage-web-service-endpoints-using-api-management/find-api-key.png)

#### <a name="test-rrs-endpoint"></a>TEST RRS bitiş noktası
##### <a name="test-button"></a>Test düğmesi
RRS bitiş noktasını test etmenin kolay bir yolu, web hizmeti panosunda **Test'i** tıklatmaktır.

![test](./media/manage-web-service-endpoints-using-api-management/test.png)

Yazın Bu **col2**için **iyi bir gün.** Onay işaretini tıklatın.

![veri girin](./media/manage-web-service-endpoints-using-api-management/enter-data.png)

Böyle bir şey göreceksiniz

![örnek çıktı](./media/manage-web-service-endpoints-using-api-management/sample-output.png)

##### <a name="sample-code"></a>Örnek Kod
RRS'nizi test etmenin başka bir yolu da istemci kodunuzdur. Panoda **İstek/yanıt'ı** tıklatıp en altına kaydırırsanız, C#, Python ve R için örnek kod görürsünüz. Ayrıca, uri isteği, üstbilgi ve gövde de dahil olmak üzere RRS isteğinin sözdizimini de görürsünüz.

Bu kılavuz, çalışan bir Python örneğini gösterir. Çalışma **alanı,** **hizmet**ve denemenizin **api_key** ile değiştirmeniz gerekir.

    import urllib2
    import json
    workspace = "<REPLACE WITH YOUR EXPERIMENT'S WEB SERVICE WORKSPACE ID>"
    service = "<REPLACE WITH YOUR EXPERIMENT'S WEB SERVICE SERVICE ID>"
    api_key = "<REPLACE WITH YOUR EXPERIMENT'S WEB SERVICE API KEY>"
    data = {
    "Inputs": {
        "input1": {
            "ColumnNames": ["Col2"],
            "Values": [ [ "This is a good day" ] ]
        },
    },
    "GlobalParameters": { }
    }
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace + "/services/" + service + "/execute?api-version=2.0&details=true"
    headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}
    body = str.encode(json.dumps(data))
    try:
        req = urllib2.Request(url, body, headers)
        response = urllib2.urlopen(req)
        result = response.read()
        print "result:" + result
            except urllib2.HTTPError, error:
        print("The request failed with status code: " + str(error.code))
        print(error.info())
        print(json.loads(error.read()))

#### <a name="test-bes-endpoint"></a>TEST BES bitiş noktası
Panoda **Toplu Yürütme'yi** tıklatın ve en alta doğru ilerleyin. C#, Python ve R için örnek kodu görürsünüz. Ayrıca, BES'in iş gönderme, iş başlatma, bir işin durumunu veya sonuçlarını alma ve bir işi silme isteklerinin sözdizimini de görürsünüz.

Bu kılavuz, çalışan bir Python örneğini gösterir. Denemenizin **çalışma alanı,** **hizmet**ve **api_key** ile değiştirmeniz gerekir. Ayrıca, **depolama hesabı adını,** depolama **hesabı anahtarını**ve **depolama kapsayıcı adını**değiştirmeniz gerekir. Son olarak, **giriş dosyasının** konumunu ve **çıktı dosyasının**konumunu değiştirmeniz gerekir.

    import urllib2
    import json
    import time
    from azure.storage import *
    workspace = "<REPLACE WITH YOUR WORKSPACE ID>"
    service = "<REPLACE WITH YOUR SERVICE ID>"
    api_key = "<REPLACE WITH THE API KEY FOR YOUR WEB SERVICE>"
    storage_account_name = "<REPLACE WITH YOUR AZURE STORAGE ACCOUNT NAME>"
    storage_account_key = "<REPLACE WITH YOUR AZURE STORAGE KEY>"
    storage_container_name = "<REPLACE WITH YOUR AZURE STORAGE CONTAINER NAME>"
    input_file = "<REPLACE WITH THE LOCATION OF YOUR INPUT FILE>" # Example: C:\\mydata.csv
    output_file = "<REPLACE WITH THE LOCATION OF YOUR OUTPUT FILE>" # Example: C:\\myresults.csv
    input_blob_name = "mydatablob.csv"
    output_blob_name = "myresultsblob.csv"
    def printHttpError(httpError):
    print("The request failed with status code: " + str(httpError.code))
    print(httpError.info())
    print(json.loads(httpError.read()))
    return
    def saveBlobToFile(blobUrl, resultsLabel):
    print("Reading the result from " + blobUrl)
    try:
        response = urllib2.urlopen(blobUrl)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    with open(output_file, "wb+") as f:
        f.write(response.read())
    print(resultsLabel + " have been written to the file " + output_file)
    return
    def processResults(result):
    first = True
    results = result["Results"]
    for outputName in results:
        result_blob_location = results[outputName]
        sas_token = result_blob_location["SasBlobToken"]
        base_url = result_blob_location["BaseLocation"]
        relative_url = result_blob_location["RelativeLocation"]
        print("The results for " + outputName + " are available at the following Azure Storage location:")
        print("BaseLocation: " + base_url)
        print("RelativeLocation: " + relative_url)
        print("SasBlobToken: " + sas_token)
        if (first):
            first = False
            url3 = base_url + relative_url + sas_token
            saveBlobToFile(url3, "The results for " + outputName)
    return

    def invokeBatchExecutionService():
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace +"/services/" + service +"/jobs"
    blob_service = BlobService(account_name=storage_account_name, account_key=storage_account_key)
    print("Uploading the input to blob storage...")
    data_to_upload = open(input_file, "r").read()
    blob_service.put_blob(storage_container_name, input_blob_name, data_to_upload, x_ms_blob_type="BlockBlob")
    print "Uploaded the input to blob storage"
    input_blob_path = "/" + storage_container_name + "/" + input_blob_name
    connection_string = "DefaultEndpointsProtocol=https;AccountName=" + storage_account_name + ";AccountKey=" + storage_account_key
    payload =  {
        "Input": {
            "ConnectionString": connection_string,
            "RelativeLocation": input_blob_path
        },
        "Outputs": {
            "output1": { "ConnectionString": connection_string, "RelativeLocation": "/" + storage_container_name + "/" + output_blob_name },
        },
        "GlobalParameters": {
        }
    }
        body = str.encode(json.dumps(payload))
    headers = { "Content-Type":"application/json", "Authorization":("Bearer " + api_key)}
    print("Submitting the job...")
    # submit the job
    req = urllib2.Request(url + "?api-version=2.0", body, headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    result = response.read()
    job_id = result[1:-1] # remove the enclosing double-quotes
    print("Job ID: " + job_id)
    # start the job
    print("Starting the job...")
    req = urllib2.Request(url + "/" + job_id + "/start?api-version=2.0", "", headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    url2 = url + "/" + job_id + "?api-version=2.0"

    while True:
        print("Checking the job status...")
        # If you are using Python 3+, replace urllib2 with urllib.request in the following code
        req = urllib2.Request(url2, headers = { "Authorization":("Bearer " + api_key) })
        try:
            response = urllib2.urlopen(req)
        except urllib2.HTTPError, error:
            printHttpError(error)
            return
        result = json.loads(response.read())
        status = result["StatusCode"]
        print "status:" + status
        if (status == 0 or status == "NotStarted"):
            print("Job " + job_id + " not yet started...")
        elif (status == 1 or status == "Running"):
            print("Job " + job_id + " running...")
        elif (status == 2 or status == "Failed"):
            print("Job " + job_id + " failed!")
            print("Error details: " + result["Details"])
            break
        elif (status == 3 or status == "Cancelled"):
            print("Job " + job_id + " cancelled!")
            break
        elif (status == 4 or status == "Finished"):
            print("Job " + job_id + " finished!")
            processResults(result)
            break
        time.sleep(1) # wait one second
    return
    invokeBatchExecutionService()
