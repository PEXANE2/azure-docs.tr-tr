---
title: 'Hızlı başlangıç: Python form tanıyıcısı kullanarak fatura verilerini ayıklama'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, verileri faturalardan ayıklamak için Python ile REST API formu tanıyıcıyı kullanacaksınız
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 11/12/2020
ms.author: pafarley
ms.custom: devx-track-python
ms.openlocfilehash: 7c6b5406728817c3dd700ec285d9af77c9d3cf60
ms.sourcegitcommit: 1bf144dc5d7c496c4abeb95fc2f473cfa0bbed43
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "96009322"
---
# <a name="quickstart-extract-invoice-data-using-the-form-recognizer-rest-api-with-python"></a>Hızlı başlangıç: Python ile REST API form tanıyıcı kullanarak fatura verilerini ayıklama

Bu hızlı başlangıçta, faturalardaki ilgili bilgileri ayıklamak ve tanımlamak için Python ile Azure form tanıyıcı REST API kullanacaksınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/cognitive-services/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlayabilmeniz için şunları yapmanız gerekir:
- [Python](https://www.python.org/downloads/) yüklendi (örneği yerel olarak çalıştırmak istiyorsanız).
- Bir fatura belgesi. Bu hızlı başlangıç için [örnek fatura](../media/sample-invoice.jpg) kullanabilirsiniz.

> [!NOTE]
> Bu hızlı başlangıçta yerel bir dosya kullanılır. Bunun yerine URL tarafından erişilen bir fatura belgesi kullanmak için [başvuru belgelerine](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeInvoiceAsync)bakın.

## <a name="create-a-form-recognizer-resource"></a>Form tanıyıcı kaynağı oluşturma

[!INCLUDE [create resource](../includes/create-resource.md)]

## <a name="analyze-an-invoice"></a>Fatura çözümleme

Bir faturayı çözümlemeye başlamak için aşağıdaki Python betiğini kullanarak **[faturayı çözümle](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-2/operations/5ed8c9843c2794cbb1a96291)** API 'sini çağırın. Betiği çalıştırmadan önce Şu değişiklikleri yapın:

1. `<Endpoint>`Form tanıyıcı aboneliğiniz ile edindiğiniz uç noktayla değiştirin.
1. `<subscription key>`Önceki adımdan kopyaladığınız abonelik anahtarıyla değiştirin.
1. `<path to your invoice>`Bir örnek faturanızda kaydettiğiniz yerel yol ile değiştirin.

    ```python
        ########### Python Form Recognizer Async Invoice #############
    
        import json
        import time
        from requests import get, post
        
        # Endpoint URL
        endpoint = r"<Endpoint>"
        apim_key = "<subscription key>"
        post_url = endpoint + "/formrecognizer/v2.1-preview.2/prebuilt/invoice/analyze"
        source = r"<path to your invoice>"
        
        headers = {
            # Request headers
            'Content-Type': '<file type>',
            'Ocp-Apim-Subscription-Key': apim_key,
        }
        
        params = {
            "includeTextDetails": True
            "locale": "en-US"
        }
        
        with open(source, "rb") as f:
            data_bytes = f.read()
        
        try:
            resp = post(url = post_url, data = data_bytes, headers = headers, params = params)
            if resp.status_code != 202:
                print("POST analyze failed:\n%s" % resp.text)
                quit()
            print("POST analyze succeeded:\n%s" % resp.headers)
            get_url = resp.headers["operation-location"]
        except Exception as e:
            print("POST analyze failed:\n%s" % str(e))
            quit()
    ```

1. Kodu. Kopyala uzantılı bir dosyaya kaydedin. Örneğin, *form-recognizer-invoice.py*.
1. Bir komut istemi penceresi açın.
1. İstemde, örneği çalıştırmak için `python` komutunu kullanın. Örneğin, `python form-recognizer-invoice.py`.

`202 (Success)`Komut dosyasının konsola yazdırabileceği bir **işlem konumu** üst bilgisi içeren bir yanıt alırsınız. Bu üst bilgi, zaman uyumsuz işlemin durumunu sorgulamak ve sonuçları almak için kullanabileceğiniz bir işlem KIMLIĞI içerir. Aşağıdaki örnek değerinde, sonraki dize `operations/` Işlem kimliğidir.

## <a name="get-the-invoice-results"></a>Fatura sonuçlarını al

**Faturayı çözümle** API 'sini çağırdıktan sonra, işlemin durumunu ve ayıklanan verileri almak Için **[Fatura sonucunu al](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-2/operations/5ed8c9acb78c40a2533aee83)** API 'sini çağırın. Aşağıdaki kodu Python betiğinizin altına ekleyin. Bu işlem KIMLIĞI değerini yeni bir API çağrısında kullanır. Bu betik, sonuçlar kullanılabilir olana kadar, API 'YI düzenli aralıklarla çağırır. Bir saniye veya daha fazla Aralık öneririz.

```python
n_tries = 10
n_try = 0
wait_sec = 6
while n_try < n_tries:
    try:
        resp = get(url = get_url, headers = {"Ocp-Apim-Subscription-Key": apim_key})
        resp_json = json.loads(resp.text)
        if resp.status_code != 200:
            print("GET Invoice results failed:\n%s" % resp_json)
            quit()
        status = resp_json["status"]
        if status == "succeeded":
            print("Invoice Analysis succeeded:\n%s" % resp_json)
            quit()
        if status == "failed":
            print("Invoice Analysis failed:\n%s" % resp_json)
            quit()
        # Analysis still running. Wait and retry.
        time.sleep(wait_sec)
        n_try += 1     
    except Exception as e:
        msg = "GET analyze results failed:\n%s" % str(e)
        print(msg)
        quit()
```

1. Betiği kaydedin.
1. `python`Örneği çalıştırmak için komutunu yeniden kullanın. Örneğin, `python form-recognizer-invoice.py`.

### <a name="examine-the-response"></a>Yanıtı inceleme

Komut dosyası, **faturayı çözümle** işlemi tamamlanana kadar konsola gönderilen yanıtları yazdıracaktır. Daha sonra, ayıklanan metin verilerini JSON biçiminde yazdıracaktır. `"readResults"`Alan, faturadan çıkarılan her metin satırını içerir, `"pageResults"` faturada çıkarılan tablolar ve seçimler işaretlerini içerir ve `"documentResults"` alan, faturanın en ilgili bölümleri için anahtar/değer bilgilerini içerir.

Aşağıdaki fatura belgesine ve buna karşılık gelen JSON çıktısına bakın:

* [Örnek fatura](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/curl/form-recognizer/sample-invoice.pdf)
* [Örnek fatura JSON çıkışı](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/curl/form-recognizer/sample-invoice-output.json)


### <a name="sample-python-script-to-extract-invoice-or-a-batch-of-invoices-into-a-csv-file"></a>Bir CSV dosyasına fatura veya bir fatura toplu işi çıkarmak için örnek Python betiği

Bu örnek Python betiği, fatura API 'sini kullanmaya başlamanızı gösterir. Bir parametre veya klasör olarak tek fatura ile çalışabilir ve ".invoice.json" JSON dosyasını ve ayıklanan değerler sonuçlarıyla bir CSV dosyası _invoiceResutls.csv_ çıktısını alır. Bir klasör üzerinde çalışırken, tüm "PDF", "jpg", "JPEG", "png", "BMP", "tif", "TIFF" dosyalarını tarar ve bunları API ile çalıştırır. 
 
```python
########### Python Form Recognizer Async Invoice #############

import json
import time
import os
import ntpath
import sys
from requests import get, post
import csv


def analyzeInvoice(filename):
    invoiceResultsFilename = filename + ".invoice.json"

    # do not run analyze if .invoice.json file is present on disk
    if os.path.isfile(invoiceResultsFilename):
        with open(invoiceResultsFilename) as json_file:
            return json.load(json_file)

    # Endpoint URL
    #endpoint = r"<Endpoint>"
    #apim_key = "<subscription key>"
    post_url = endpoint + "/formrecognizer/v2.1-preview.2/prebuilt/invoice/analyze"
    headers = {
        # Request headers
        'Content-Type': 'application/octet-stream',
        'Ocp-Apim-Subscription-Key': apim_key,
    }

    params = {
        "includeTextDetails": True
    }

    with open(filename, "rb") as f:
        data_bytes = f.read()

    try:
        resp = post(url = post_url, data = data_bytes, headers = headers, params = params)
        if resp.status_code != 202:
            print("POST analyze failed:\n%s" % resp.text)
            return None
        print("POST analyze succeeded: %s" % resp.headers["operation-location"])
        get_url = resp.headers["operation-location"]
    except Exception as e:
        print("POST analyze failed:\n%s" % str(e))
        return None

    n_tries = 50
    n_try = 0
    wait_sec = 6

    while n_try < n_tries:
        try:
            resp = get(url = get_url, headers = {"Ocp-Apim-Subscription-Key": apim_key})
            resp_json = json.loads(resp.text)
            if resp.status_code != 200:
                print("GET Invoice results failed:\n%s" % resp_json)
                return None
            status = resp_json["status"]
            if status == "succeeded":
                print("Invoice analysis succeeded.")
                with open(invoiceResultsFilename, 'w') as outfile:
                    json.dump(resp_json, outfile, indent=4)
                return resp_json
            if status == "failed":
                print("Analysis failed:\n%s" % resp_json)
                return None
            # Analysis still running. Wait and retry.
            time.sleep(wait_sec)
            n_try += 1     
        except Exception as e:
            msg = "GET analyze results failed:\n%s" % str(e)
            print(msg)
            return None

    return resp_json

def parseInvoiceResults(resp_json):
    docResults = resp_json["analyzeResult"]["documentResults"]
    invoiceResult = {}
    for docResult in docResults:
        for fieldName, fieldValue in sorted(docResult["fields"].items()):
            valueFields = list(filter(lambda item: ("value" in item[0]) and ("valueString" not in item[0]), fieldValue.items()))
            invoiceResult[fieldName] = fieldValue["text"]            
            if len(valueFields) == 1:
                print("{0:26} : {1:50}      NORMALIZED VALUE: {2}".format(fieldName , fieldValue["text"], valueFields[0][1]))
                invoiceResult[fieldName + "_normalized"] = valueFields[0][1]
            else:
                print("{0:26} : {1}".format(fieldName , fieldValue["text"]))
    print("")
    return invoiceResult

def main(argv):
    if (len(argv)  != 2):
        print("ERROR: Please provide invoice filename or root directory with invoice PDFs/images as an argument to the python script")
        return

    # list of invoice to analyze
    invoiceFiles = []
    csvPostfix = '-invoiceResults.csv'
    if os.path.isfile(argv[1]):
        # Single invoice
        invoiceFiles.append(argv[1])
        csvFileName = argv[1] + csvPostfix
    else:
        # Folder of invoices
        supportedExt = ['.pdf', '.jpg','.jpeg','.tif','.tiff','.png','.bmp']
        invoiceDirectory = argv[1]
        csvFileName = os.path.join(invoiceDirectory, os.path.basename(os.path.abspath(invoiceDirectory)) + csvPostfix)
        for root, directories, filenames in os.walk(invoiceDirectory):
            for invoiceFilename in filenames:
                ext = os.path.splitext(invoiceFilename)[-1].lower()
                if ext in supportedExt:
                    fullname = os.path.join(root, invoiceFilename)
                    invoiceFiles.append(fullname)
                    
    with open(csvFileName, mode='w', newline='\n', encoding='utf-8') as csv_file:
        fieldnames = ['Filename',
                      'FullFilename','InvoiceTotal','InvoiceTotal_normalized','AmountDue','AmountDue_normalized','SubTotal','SubTotal_normalized','TotalTax','TotalTax_normalized','CustomerName','VendorName',
                      'InvoiceId','CustomerId','PurchaseOrder','InvoiceDate','InvoiceDate_normalized','DueDate','DueDate_normalized',
                      'VendorAddress','VendorAddressRecipient','BillingAddress','BillingAddressRecipient','ShippingAddress','ShippingAddressRecipient','CustomerAddress','CustomerAddressRecipient','ServiceAddress','ServiceAddressRecipient','RemittanceAddress','RemittanceAddressRecipient', 'ServiceStartDate','ServiceStartDate_normalized','ServiceEndDate','ServiceEndDate_normalized','PreviousUnpaidBalance','PreviousUnpaidBalance_normalized']
        writer = csv.DictWriter(csv_file, fieldnames=fieldnames)
        writer.writeheader()
        counter = 0
        for invoiceFullFilename in invoiceFiles:
            counter = counter + 1
            invoiceFilename = ntpath.basename(invoiceFullFilename)
            print("----- Processing {0}/{1} : {2} -----".format(counter, len(invoiceFiles),invoiceFullFilename))
            
            resp_json = analyzeInvoice(invoiceFullFilename)

            if (resp_json is not None):
                invoiceResults = parseInvoiceResults(resp_json)
                invoiceResults["FullFilename"] = invoiceFullFilename
                invoiceResults["Filename"] = invoiceFilename
                writer.writerow(invoiceResults)
                    
if __name__ == '__main__':
    main(sys.argv)
```

1. Kodu. Kopyala uzantılı bir dosyaya kaydedin. Örneğin, *form-recognizer-invoice-to-CSV.py*.
1. `<Endpoint>`Form tanıyıcı aboneliğiniz ile edindiğiniz uç noktayla değiştirin.
1. `<subscription key>`Önceki adımdan kopyaladığınız abonelik anahtarıyla değiştirin.
1. Bir komut istemi penceresi açın.
1. İstemde, örneği çalıştırmak için `python` komutunu kullanın. Örneğin, `python form-recognizer-invoice.py {file name or folder name}` Python betiği parametre olarak tek bir fatura veya klasör ile çalışabilir ve ".invoice.json" JSON dosyasını ve faturalardan "-invoiceResults.csv" CSV dosyasına ayıklanan değerleri sonuçlara dönüştürür. Bir klasör üzerinde çalışırken, tüm "PDF", "jpg", "JPEG", "png", "BMP", "tif", "TIFF" dosyalarını tarar ve bunları API ile çalıştırır.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, içeriği faturalardan çıkarmak için Python ile birlikte REST API bir form tanıyıcı kullandınız. Sonra, form tanıyıcı API 'sini daha ayrıntılı incelemek için başvuru belgelerine bakın.

> [!div class="nextstepaction"]
> [REST API başvuru belgeleri](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeInvoiceAsync)

   
