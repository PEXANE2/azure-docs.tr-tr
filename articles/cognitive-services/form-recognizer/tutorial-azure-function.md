---
title: 'Öğretici: depolanan belgeleri işlemek için bir Azure Işlevi kullanma'
titleSuffix: Azure Cognitive Services
description: Bu kılavuzda bir Azure işlevinin, bir Azure Blob depolama kapsayıcısına yüklenen belgelerin işlenmesini tetiklemek için nasıl kullanılacağı gösterilmektedir.
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: tutorial
ms.date: 03/19/2021
ms.author: lajanuar
ms.openlocfilehash: bf455d9401593b5c09fa295e492368a2a5bee240
ms.sourcegitcommit: ed7376d919a66edcba3566efdee4bc3351c57eda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2021
ms.locfileid: "105048701"
---
# <a name="tutorial-use-an-azure-function-to-process-stored-documents"></a>Öğretici: depolanan belgeleri işlemek için bir Azure Işlevi kullanma

Form tanıyıcıyı, Azure Işlevleri ile oluşturulmuş otomatik veri işleme işlem hattının bir parçası olarak kullanabilirsiniz. Bu kılavuzda, Azure Blob depolama kapsayıcısına yüklenen belgeleri işlemek için bir Azure işlevinin nasıl kullanılacağı gösterilmektedir. Bu iş akışı, form tanıyıcı düzen hizmetini kullanarak depolanan belgelerden tablo verilerini ayıklar ve tablo verilerini Azure 'daki bir. csv dosyasına kaydeder. Daha sonra verileri Microsoft Power BI (burada kapsanmayan) kullanarak görüntüleyebilirsiniz.

> [!div class="mx-imgBorder"]
> ![Azure hizmeti iş akışı diyagramı](./media/tutorial-azure-function/workflow-diagram.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure Depolama hesabı oluşturma
> * Azure İşlevleri projesi oluşturma
> * Karşıya yüklenen formlardan düzen verilerini Ayıkla
> * Azure depolama 'ya düzen verileri yükleme

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services)
* <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer"  title="Form tanıyıcı kaynağı oluşturma form tanıyıcı "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> anahtarınızı ve uç noktanızı almak Için Azure Portal form tanıyıcı kaynağı oluşturun. Dağıtıldıktan sonra **Kaynağa Git ' e** tıklayın.
  * Uygulamanızı form tanıyıcı API 'sine bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
  * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.
* Çözümlenecek yerel PDF belgesi. Bu [örnek belgeyi](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/curl/form-recognizer/sample-layout.pdf) kullanmak için indirebilirsiniz.
* [Python 3.8. x](https://www.python.org/downloads/) yüklendi.
* [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) yüklendi.
* [Azure Functions Core Tools](../../azure-functions/functions-run-local.md?tabs=windows%2ccsharp%2cbash#install-the-azure-functions-core-tools) yüklendi.
* Aşağıdaki Uzantılar yüklü Visual Studio Code:
  * [Azure Işlevleri uzantısı](/azure/developer/python/tutorial-vs-code-serverless-python-01#visual-studio-code-python-and-the-azure-functions-extension)
  * [Python uzantısı](https://code.visualstudio.com/docs/python/python-tutorial#_install-visual-studio-code-and-the-python-extension)

## <a name="create-an-azure-storage-account"></a>Azure Depolama hesabı oluşturma

Azure portal [bir Azure depolama hesabı oluşturun](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) . Hesap türü olarak **StorageV2** öğesini seçin.

Sol bölmede **CORS** sekmesini seçin ve varsa mevcut CORS ilkesini kaldırın.

Dağıtıldıktan sonra, **Test** ve **output** adlı iki boş BLOB depolama kapsayıcısı oluşturun.

## <a name="create-an-azure-functions-project"></a>Azure İşlevleri projesi oluşturma

Visual Studio Code’u açın. Azure Işlevleri uzantısını yüklediyseniz, sol gezinti bölmesinde bir Azure logosu görmeniz gerekir. Kalem simgesini seçin. Yeni bir proje oluşturun ve sorulduğunda projeyi içeren **coa_new** yerel bir klasör oluşturun.

![VSCode işlev oluştur düğmesi](./media/tutorial-azure-function/vs-code-create-function.png)


Bir dizi ayarı yapılandırmanız istenir:
* **Bir dil seçin** isteminde Python ' ı seçin.
* **Şablon seçin** Isteminde Azure Blob depolama tetikleyicisi ' ni seçin. Ardından varsayılan tetikleyicisine bir ad verin.
* **Ayar seç** isteminde yeni yerel uygulama ayarları oluşturmayı kabul edin.
* Oluşturduğunuz depolama hesabıyla Azure aboneliğinizi seçin. Ardından, depolama kapsayıcısının adını girmeniz gerekir (Bu durumda, `test/{name}` )
* Projeyi geçerli pencerede açmayı tercih edin. 

![VSCode istem oluşturma örneği](./media/tutorial-azure-function/vs-code-prompt.png)

Bu adımları tamamladığınızda, vscode, bir *\_ \_ init \_ \_ . Kopyala* Python betiğine sahip yeni bir Azure işlevi projesi ekler. Bu betik, **Test** depolama kapsayıcısına bir dosya yüklendiğinde tetiklenir, ancak hiçbir işlem yapmaz.

## <a name="test-the-function"></a>İşlevi test etme

Temel işlevi çalıştırmak için F5 tuşuna basın. VSCode, ile arabirim oluşturulacak bir depolama hesabı seçmenizi ister. Oluşturduğunuz depolama hesabını seçin ve devam edin.

Azure Depolama Gezgini açın ve **Test** kapsayıcısına örnek bir PDF belgesi yükleyin. Ardından VSCode terminalini kontrol edin. Betik, PDF karşıya yüklemesi tarafından tetiklenen günlüğe kaydedilecek.

![VSCode Terminal testi](./media/tutorial-azure-function/vs-code-terminal-test.png)


Devam etmeden önce betiği durdurun.

## <a name="add-form-processing-code"></a>Form işleme kodu ekle

Daha sonra, form tanıyıcı hizmetini çağırmak ve yükleme belgelerini form tanıyıcı [Düzen API](concept-layout.md)'sini kullanarak ayrıştırmak için Python betiğine kendi kodunuzu ekleyeceksiniz.

VSCode 'da işlevin *requirements.txt* dosyasına gidin. Bu, betiğinizin bağımlılıklarını tanımlar. Aşağıdaki Python paketlerini dosyasına ekleyin:

```
cryptography
azure-functions
azure-storage-blob
azure-identity
requests
pandas
numpy
```

Ardından, *\_ \_ init \_ \_ . Kopyala* betiğini açın. Aşağıdaki `import` deyimlerini ekleyin:

```Python
import logging
from azure.storage.blob import BlobServiceClient
import azure.functions as func
import json
import time
from requests import get, post
import os
from collections import OrderedDict
import numpy as np
import pandas as pd
```

Oluşturulan `main` işlevi olduğu gibi bırakabilirsiniz. Özel kodunuzu bu işlevin içine ekleyeceksiniz.

```python
# This part is automatically generated
def main(myblob: func.InputStream):
    logging.info(f"Python blob trigger function processed blob \n"
    f"Name: {myblob.name}\n"
    f"Blob Size: {myblob.length} bytes")
```

Aşağıdaki kod bloğu, karşıya yüklenen belgedeki form tanıyıcıyı [Çözümle düzen](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-2/operations/AnalyzeLayoutAsync) API 'sini çağırır. Uç nokta ve anahtar değerlerinizi girin. 


# <a name="version-20"></a>[sürüm 2,0](#tab/2-0)

```Python
# This is the call to the Form Recognizer endpoint
    endpoint = r"Your Form Recognizer Endpoint"
    apim_key = "Your Form Recognizer Key"
    post_url = endpoint + "/formrecognizer/v2.0/Layout/analyze"
    source = myblob.read()

    headers = {
    # Request headers
    'Content-Type': 'application/pdf',
    'Ocp-Apim-Subscription-Key': apim_key,
        }

    text1=os.path.basename(myblob.name)
```

# <a name="version-21-preview"></a>[sürüm 2,1 Önizleme](#tab/2-1)

```Python
# This is the call to the Form Recognizer endpoint
    endpoint = r"Your Form Recognizer Endpoint"
    apim_key = "Your Form Recognizer Key"
    post_url = endpoint + "/formrecognizer/v2.1-preview.3/Layout/analyze"
    source = myblob.read()

    headers = {
    # Request headers
    'Content-Type': 'application/pdf',
    'Ocp-Apim-Subscription-Key': apim_key,
        }

    text1=os.path.basename(myblob.name)
```
---

> [!IMPORTANT]
> Azure portala gidin. **Önkoşul** bölümünde oluşturduğunuz form tanıyıcı kaynağı başarıyla dağıtıldı, **sonraki adımlar** altındaki **Kaynağa Git** düğmesine tıklayın. Anahtar ve uç noktanızı kaynağın **anahtar ve uç nokta** sayfasında, **kaynak yönetimi** altında bulabilirsiniz. 
>
> İşiniz bittiğinde kodu koddan kaldırmayı unutmayın ve hiçbir zaman herkese açık bir şekilde nakletmeyin. Üretim için, kimlik bilgilerinizi depolamak ve bunlara erişmek için güvenli bir yol kullanmayı düşünün. Daha fazla bilgi için bilişsel [Hizmetler güvenlik](../cognitive-services-security.md) makalesine bakın.

Sonra, hizmeti sorgulamak ve döndürülen verileri almak için kod ekleyin. 


```Python
resp = requests.post(url = post_url, data = source, headers = headers)
    if resp.status_code != 202:
        print("POST analyze failed:\n%s" % resp.text)
        quit()
    print("POST analyze succeeded:\n%s" % resp.headers)
    get_url = resp.headers["operation-location"]

    wait_sec = 25
    
    time.sleep(wait_sec)
    # The layout API is async therefore the wait statement
    
    resp =requests.get(url = get_url, headers = {"Ocp-Apim-Subscription-Key": apim_key})
    
    resp_json = json.loads(resp.text)
    
    
    status = resp_json["status"]
    
    
    if status == "succeeded":
        print("Layout Analysis succeeded:\n%s")
        results=resp_json
    else:
        print("GET Layout results failed:\n%s")
        quit()

    results=resp_json
```

Ardından, Azure depolama **Çıkış** kapsayıcısına bağlanmak için aşağıdaki kodu ekleyin. Depolama hesabı adı ve anahtarı için kendi değerlerinizi girin. Azure portal, depolama kaynağınızın **erişim tuşları** sekmesinde anahtarı alabilir.

```Python
# This is the connection to the blob storage, with the Azure Python SDK
    blob_service_client = BlobServiceClient.from_connection_string("DefaultEndpointsProtocol=https;AccountName="Storage Account Name";AccountKey="storage account key";EndpointSuffix=core.windows.net")
    container_client=blob_service_client.get_container_client("output")
```

Aşağıdaki kod, döndürülen form tanıyıcı yanıtını ayrıştırır, bir. csv dosyası oluşturur ve bunu **Çıkış** kapsayıcısına yükler. 


> [!IMPORTANT]
> Muhtemelen kendi form belgelerinizin yapısıyla eşleşmesi için bu kodu düzenlemeniz gerekir.

```Python
# The code below is how I extract the json format into tabular data 
    # Please note that you need to adjust the code below to your form structure
    # It probably won't work out-of-box for your specific form
    pages = results["analyzeResult"]["pageResults"]

    def make_page(p):
        res=[]
        res_table=[]
        y=0
        page = pages[p]
        for tab in page["tables"]:
            for cell in tab["cells"]:
                res.append(cell)
                res_table.append(y)
            y=y+1
    
        res_table=pd.DataFrame(res_table)
        res=pd.DataFrame(res)
        res["table_num"]=res_table[0]
        h=res.drop(columns=["boundingBox","elements"])
        h.loc[:,"rownum"]=range(0,len(h))
        num_table=max(h["table_num"])
        return h, num_table, p

    h, num_table, p= make_page(0)   

    for k in range(num_table+1):
        new_table=h[h.table_num==k]
        new_table.loc[:,"rownum"]=range(0,len(new_table))
        row_table=pages[p]["tables"][k]["rows"]
        col_table=pages[p]["tables"][k]["columns"]
        b=np.zeros((row_table,col_table))
        b=pd.DataFrame(b)
        s=0
        for i,j in zip(new_table["rowIndex"],new_table["columnIndex"]):
            b.loc[i,j]=new_table.loc[new_table.loc[s,"rownum"],"text"]
            s=s+1

```

Son olarak, kod bloğu ayıklanan tablo ve metin verilerini blob Storage öğesine yükler.

```Python
    # Here is the upload to the blob storage
    tab1_csv=b.to_csv(header=False,index=False,mode='w')
    name1=(os.path.splitext(text1)[0]) +'.csv'
    container_client.upload_blob(name=name1,data=tab1_csv)
```

## <a name="run-the-function"></a>İşlevi çalıştırma

İşlevi yeniden çalıştırmak için F5 'e basın. **Test** depolama KAPSAYıCıSıNA örnek PDF formunu yüklemek için Azure Depolama Gezgini kullanın. Bu eylem betiği çalışacak şekilde tetiklemelidir ve ardından **çıktı** kapsayıcısında elde edilen. csv dosyasını (tablo olarak görüntülenir) görmeniz gerekir.

İçerdiği verilerin zengin görselleştirmelerini oluşturmak için, bu kapsayıcıyı Power BI bağlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, karşıya yüklenen PDF belgelerini otomatik olarak işlemek ve içeriklerinin daha kolay bir biçimde çıktısını almak için Python 'da yazılmış bir Azure Işlevini nasıl kullanacağınızı öğrendiniz. Sonra, verileri göstermek için Power BI kullanmayı öğrenin.

> [!div class="nextstepaction"]
> [Microsoft Power BI](https://powerbi.microsoft.com/integrations/azure-table-storage/)

* [Form Tanıma nedir?](overview.md)
* [Düzen API 'si](concept-layout.md) hakkında daha fazla bilgi edinin