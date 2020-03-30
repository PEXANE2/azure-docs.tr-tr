---
title: Azure Monitör HTTP Veri Toplayıcı API | Microsoft Dokümanlar
description: REST API'yi arayabilen herhangi bir istemciden Bir Log Analytics çalışma alanına POST JSON verilerini eklemek için Azure Monitörü HTTP Veri Toplayıcı API'sini kullanabilirsiniz. Bu makalede, API'nin nasıl kullanılacağı açıklanmaktadır ve farklı programlama dilleri kullanarak verilerin nasıl yayımlandırılacaya da sahip olduğu örnekler bulunmaktadır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/01/2019
ms.openlocfilehash: f12e9e90b99a055945c34398ff5351334c344253
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77666761"
---
# <a name="send-log-data-to-azure-monitor-with-the-http-data-collector-api-public-preview"></a>GÜNLÜK verilerini HTTP Veri Toplayıcı API 'si (genel önizleme) ile Azure Monitörü'ne gönderme
Bu makalede, bir REST API istemcisinden Azure Monitor'a günlük verileri göndermek için HTTP Veri Toplayıcı API'sını nasıl kullanacağınızı gösterir.  Komut dosyanız veya uygulamanız tarafından toplanan verileri nasıl biçimlendirecek, bir isteğe dahil etmek ve bu isteğin Azure Monitor tarafından nasıl yetkilendirilmiş olduğunu açıklar.  PowerShell, C#ve Python için örnekler verilmiştir.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

> [!NOTE]
> Azure Monitörü HTTP Veri Toplayıcı API'si genel önizlemededir.

## <a name="concepts"></a>Kavramlar
REST API'yi arayabilen herhangi bir istemciden Azure Monitor'daki Log Analytics çalışma alanına günlük verileri göndermek için HTTP Veri ToplayıcıAPI'ni kullanabilirsiniz.  Bu, Azure Otomasyonu'nda yönetim verilerini Azure'dan veya başka bir buluttan toplayan bir runbook veya günlük verilerini birleştirmek ve çözümlemek için Azure Monitor'u kullanan alternatif bir yönetim sistemi olabilir.

Log Analytics çalışma alanındaki tüm veriler belirli bir kayıt türüne sahip bir kayıt olarak depolanır.  Verilerinizi HTTP Veri ToplayıcıAPI'ne göndermek için JSON'da birden çok kayıt olarak biçimlendirin.  Verileri gönderdiğiniz zaman, istek yükündeki her kayıt için depoda ayrı bir kayıt oluşturulur.


![HTTP Veri Toplayıcıgenel bakış](media/data-collector-api/overview.png)



## <a name="create-a-request"></a>İstek oluşturma
HTTP Veri Toplayıcı API'sini kullanmak için, JavaScript Nesne Gösterimi'nde (JSON) gönderilecek verileri içeren bir POST isteği oluşturursunuz.  Sonraki üç tablo, her istek için gereken öznitelikleri listeler. Her özniteliği daha sonra makalede daha ayrıntılı olarak açıklarız.

### <a name="request-uri"></a>İstek URI'si
| Öznitelik | Özellik |
|:--- |:--- |
| Yöntem |POST |
| URI |https://\<CustomerId\>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01 |
| İçerik türü |uygulama/json |

### <a name="request-uri-parameters"></a>URI parametreleri isteyin
| Parametre | Açıklama |
|:--- |:--- |
| CustomerID |Log Analytics çalışma alanı için benzersiz tanımlayıcı. |
| Kaynak |API kaynak adı: /api/logs. |
| API Sürümü |Bu istekle birlikte kullanılacak API sürümü. Şu anda, 2016-04-01. |

### <a name="request-headers"></a>İstek üst bilgileri
| Üst bilgi | Açıklama |
|:--- |:--- |
| Yetkilendirme |Yetki imzası. Makalenin ilerleyen saatlerinde, HMAC-SHA256 üstbilgisinin nasıl oluşturulabileceği hakkında bilgi edinebilirsiniz. |
| Günlük Türü |Gönderilen verilerin kayıt türünü belirtin. Yalnızca harfleri, sayıları içerebilir ve (_) alt çizebilir ve 100 karakteri geçemez. |
| x-ms-tarih |İsteğin RFC 1123 biçiminde işlendiği tarih. |
| x-ms-AzureResourceId | Verilerin ilişkilendirilmesi gereken Azure kaynağının kaynak kimliği. Bu, [_ResourceId](log-standard-properties.md#_resourceid) özelliğini doldurur ve verilerin [kaynak bağlam](design-logs-deployment.md#access-mode) sorgularına dahil edilmesine izin verir. Bu alan belirtilmemişse, veriler kaynak bağlam sorgularına dahil edilmez. |
| zaman-oluşturulan alan | Veri öğesinin zaman damgasını içeren verilerdeki bir alanın adı. Bir alan belirtirseniz, içeriği **TimeGenerated**için kullanılır. Bu alan belirtilmemişse, **TimeGenerated** için varsayılan değer iletinin yutulduğu zamandır. İleti alanının içeriği ISO 8601 formatında YYYY-MM-DDThh:mm:ssZ'yi takip etmelidir. |

## <a name="authorization"></a>Yetkilendirme
Azure Monitörü HTTP Veri ToplayıcıAPI'ne yapılan herhangi bir istek bir yetkilendirme üstbilgisi içermelidir. İsteğin kimliğini doğrulamak için, isteği yapan çalışma alanı için birincil veya ikincil anahtarla isteği imzalamanız gerekir. Sonra, bu imzayı isteğin bir parçası olarak geçirin.   

Yetkilendirme üstbilgisinin biçimi aşağıda veda edin:

```
Authorization: SharedKey <WorkspaceID>:<Signature>
```

*WorkspaceID,* Log Analytics çalışma alanının benzersiz tanımlayıcısıdır. *İmza,* istekten oluşturulmuş ve [SHA256 algoritması](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx)kullanılarak hesaplanan [Karma tabanlı İleti Kimlik Doğrulama Kodudur (HMAC).](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) Daha sonra Base64 kodlaması kullanarak kodlarsınız.

**SharedKey** imza dizesini kodlamak için bu biçimi kullanın:

```
StringToSign = VERB + "\n" +
                  Content-Length + "\n" +
               Content-Type + "\n" +
                  x-ms-date + "\n" +
                  "/api/logs";
```

İmza dizesinin bir örneği aşağıda verilmiştir:

```
POST\n1024\napplication/json\nx-ms-date:Mon, 04 Apr 2016 08:00:00 GMT\n/api/logs
```

İmza dizeniz olduğunda, UTF-8 kodlanmış dizedeki HMAC-SHA256 algoritmasını kullanarak kodlayın ve sonucu Base64 olarak kodlayın. Bu biçimi kullanın:

```
Signature=Base64(HMAC-SHA256(UTF8(StringToSign)))
```

Sonraki bölümlerdeki örnekler, yetkilendirme üstbilgisini oluşturmanıza yardımcı olacak örnek koduna sahiptir.

## <a name="request-body"></a>İstek gövdesi
Mesajın gövdesi JSON'da olmalı. Aşağıdaki biçimde özellik adı ve değer çiftleri ile bir veya daha fazla kayıt içermelidir. Özellik adı yalnızca harfleri, sayıları ve alt alt alt tümlemelerini (_) içerebilir.

```json
[
    {
        "property 1": "value1",
        "property 2": "value2",
        "property 3": "value3",
        "property 4": "value4"
    }
]
```

Aşağıdaki biçimi kullanarak birden çok kaydı tek bir istekte toplu olarak toplu olarak ayırabilirsiniz. Tüm kayıtlar aynı kayıt türünde olmalıdır.

```json
[
    {
        "property 1": "value1",
        "property 2": "value2",
        "property 3": "value3",
        "property 4": "value4"
    },
    {
        "property 1": "value1",
        "property 2": "value2",
        "property 3": "value3",
        "property 4": "value4"
    }
]
```

## <a name="record-type-and-properties"></a>Kayıt türü ve özellikleri
Azure Monitörü HTTP Veri Toplayıcı API'si aracılığıyla veri gönderdiğinde özel bir kayıt türü tanımlarsınız. Şu anda, diğer veri türleri ve çözümleri tarafından oluşturulan varolan kayıt türlerine veri yazamazsınız. Azure Monitor gelen verileri okur ve girdiğiniz değerlerin veri türleri ile eşleşen özellikler oluşturur.

Veri Toplayıcı API'sindeki her istek, kayıt türü için adı olan bir **Log-Type** üstbilgisi içermelidir. **_CL** sonek, girdiğiniz ada otomatik olarak eklenerek diğer günlük türlerinden özel bir günlük olarak ayırt edilir. Örneğin, **MyNewRecordType**adını girerseniz, Azure Monitor **türü MyNewRecordType_CL**bir kayıt oluşturur. Bu, kullanıcı tarafından oluşturulan tür adları ile geçerli veya gelecekteki Microsoft çözümlerinde sevk edilenler arasında çakışma olmamasını sağlamaya yardımcı olur.

Azure Monitor, bir özelliğin veri türünü tanımlamak için özellik adına bir sonek ekler. Bir özellik null değeri içeriyorsa, özellik bu kayda dahil edilmez. Bu tablo, özellik veri türünü ve ilgili sonekleri listeler:

| Özellik veri türü | Soneki |
|:--- |:--- |
| Dize |_s |
| Boole |_b |
| Çift |_d |
| Tarih/saat |_t |
| GUID (dize olarak depolanır) |_g |

Azure Monitor'un her özellik için kullandığı veri türü, yeni kaydın kayıt türünün zaten var olup olmadığına bağlıdır.

* Kayıt türü yoksa, Azure Monitor, yeni kaydın her özelliğiiçin veri türünü belirlemek için JSON türü çıkarımını kullanarak yeni bir tane oluşturur.
* Kayıt türü varsa, Azure Monitor varolan özellikleri temel alan yeni bir kayıt oluşturmaya çalışır. Yeni kayıttaki bir özelliğin veri türü eşleşmiyorsa ve varolan türe dönüştürülemezse veya kayıt var olmayan bir özellik içeriyorsa, Azure Monitor ilgili sonek içeren yeni bir özellik oluşturur.

Örneğin, bu gönderme girişi üç özelliği olan bir kayıt oluşturur, **number_d**, **boolean_b**, ve **string_s:**

![Örnek kayıt 1](media/data-collector-api/record-01.png)

Daha sonra bu sonraki girişi, tüm değerler dizeleri olarak biçimlendirilmiş olarak gönderdiyseniz, özellikleri değişmez. Bu değerler varolan veri türlerine dönüştürülebilir:

![Örnek kayıt 2](media/data-collector-api/record-02.png)

Ancak, bu sonraki gönderiyi yaptıysanız, Azure Monitor **boolean_d** ve **string_d**yeni özellikler oluşturur. Bu değerler dönüştürülemez:

![Örnek kayıt 3](media/data-collector-api/record-03.png)

Ardından, kayıt türü oluşturulmadan önce aşağıdaki girişi gönderdiyseniz, Azure Monitor **number_s,** **boolean_s**ve **string_s**olmak üzere üç özellisi olan bir kayıt oluşturur. Bu girişte, ilk değerlerin her biri bir dize olarak biçimlendirilir:

![Örnek kayıt 4](media/data-collector-api/record-04.png)

## <a name="reserved-properties"></a>Ayrılmış özellikler
Aşağıdaki özellikler ayrılmıştır ve özel kayıt türünde kullanılmamalıdır. Yükünüz bu özellik adlarından herhangi birini içeriyorsa bir hata alırsınız.

- Kiracı

## <a name="data-limits"></a>Veri sınırları
Azure Monitor Veri toplama API'sine nakledilen verilerle ilgili bazı kısıtlamalar vardır.

* Azure Monitor Veri Toplayıcı API'sine gönderi başına maksimum 30 MB. Bu, tek bir gönderi için bir boyut sınırıdır. 30 MB'ı aşan tek bir gönderideki veriler, verileri daha küçük boyutlu parçalara bölmeli ve aynı anda göndermelisiniz.
* Alan değerleri için maksimum 32 KB sınırı. Alan değeri 32 KB'den büyükse, veriler kesilir.
* Belirli bir tür için önerilen maksimum alan sayısı 50'dir. Bu, kullanılabilirlik ve arama deneyimi açısından pratik bir sınırdır.  
* Log Analytics çalışma alanındaki bir tablo yalnızca en fazla 500 sütunu destekler (bu makalede alan olarak adlandırılır). 
* Sütun adı için en fazla karakter sayısı 500'dür.

## <a name="return-codes"></a>Dönüş kodları
HTTP durum kodu 200, isteğin işlenmek için alındığı anlamına gelir. Bu, işlemin başarıyla tamamlandığını gösterir.

Bu tablo, hizmetin döndürebileceği durum kodlarının tam kümesini listeler:

| Kod | Durum | Hata kodu | Açıklama |
|:--- |:--- |:--- |:--- |
| 200 |Tamam | |İstek başarıyla kabul edildi. |
| 400 |Kötü istek |Etkin Olmayan Müşteri |Çalışma alanı kapatıldı. |
| 400 |Kötü istek |GeçersizApiVersion |Belirttiğiniz API sürümü hizmet tarafından tanınmadı. |
| 400 |Kötü istek |GeçersizCustomerId |Belirtilen çalışma alanı kimliği geçersizdir. |
| 400 |Kötü istek |GeçersizDataFormat |Geçersiz JSON sunuldu. Yanıt gövdesi, hatanın nasıl çözüleceği hakkında daha fazla bilgi içerebilir. |
| 400 |Kötü istek |GeçersizLogType |Belirtilen günlük türü özel karakterler veya sayısallar içeriyordu. |
| 400 |Kötü istek |EksikApiVersion |API sürümü belirtilmedi. |
| 400 |Kötü istek |Eksik İçerik Türü |İçerik türü belirtilmedi. |
| 400 |Kötü istek |EksikLogType |Gerekli değer günlüğü türü belirtilmedi. |
| 400 |Kötü istek |DesteklenmeyenİçerikTürü |İçerik türü **uygulama/json**olarak ayarlanmadı. |
| 403 |Yasak |Geçersiz Yetkilendirme |Hizmet isteği doğrulamak için başarısız oldu. Çalışma alanı kimliği ve bağlantı anahtarının geçerli olduğunu doğrulayın. |
| 404 |Bulunamadı | | Sağlanan URL yanlış veya istek çok büyük. |
| 429 |Çok Fazla İstek | | Hizmet, hesabınızdan yüksek miktarda veri yaşıyor. Lütfen isteği daha sonra yeniden deneyin. |
| 500 |Dahili Sunucu Hatası |Belirtilmemiş Hata |Hizmet bir iç hatayla karşılaştı. Lütfen isteği yeniden deneyin. |
| 503 |Hizmet Kullanılamıyor |ServiceKullanılamaz |Hizmet şu anda istek almak için kullanılamıyor. Lütfen isteğinizi yeniden deneyin. |

## <a name="query-data"></a>Verileri sorgulama
Azure Monitor HTTP Veri Toplayıcı API'sı tarafından gönderilen verileri sorgulamak için, _CL **eklenen** **LogType** değerine eşit **Type** ile kayıtları arayın. Örneğin, **MyCustomLog'u**kullandıysanız, tüm kayıtları `MyCustomLog_CL`.

## <a name="sample-requests"></a>Örnek istekler
Sonraki bölümlerde, farklı programlama dillerini kullanarak Azure Monitor HTTP Veri Toplayıcı API'sine nasıl veri göndereceğinize ait örnekleri bulabilirsiniz.

Her örnek için, yetkilendirme üstbilgisi için değişkenleri ayarlamak için aşağıdaki adımları yapın:

1. Azure portalında Log Analytics çalışma alanınızı bulun.
2. **Gelişmiş Ayarlar'ı** ve ardından **Bağlı Kaynakları**seçin.
2. Çalışma Alanı **Kimliği'nin**sağında, kopya simgesini seçin ve ardından **Kimliği Müşteri Kimliği** değişkeninin değeri olarak yapıştırın.
3. **Birincil Anahtar'ın**sağında, kopya simgesini seçin ve ardından Kimliği **Paylaşılan Anahtar** değişkeninin değeri olarak yapıştırın.

Alternatif olarak, günlük türü ve JSON verileri için değişkenleri değiştirebilirsiniz.

### <a name="powershell-sample"></a>PowerShell örneği
```powershell
# Replace with your Workspace ID
$CustomerId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"  

# Replace with your Primary Key
$SharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# Specify the name of the record type that you'll be creating
$LogType = "MyRecordType"

# You can use an optional field to specify the timestamp from the data. If the time field is not specified, Azure Monitor assumes the time is the message ingestion time
$TimeStampField = "DateValue"


# Create two records with the same set of properties to create
$json = @"
[{  "StringValue": "MyString1",
    "NumberValue": 42,
    "BooleanValue": true,
    "DateValue": "2019-09-12T20:00:00.625Z",
    "GUIDValue": "9909ED01-A74C-4874-8ABF-D2678E3AE23D"
},
{   "StringValue": "MyString2",
    "NumberValue": 43,
    "BooleanValue": false,
    "DateValue": "2019-09-12T20:00:00.625Z",
    "GUIDValue": "8809ED01-A74C-4874-8ABF-D2678E3AE23D"
}]
"@

# Create the function to create the authorization signature
Function Build-Signature ($customerId, $sharedKey, $date, $contentLength, $method, $contentType, $resource)
{
    $xHeaders = "x-ms-date:" + $date
    $stringToHash = $method + "`n" + $contentLength + "`n" + $contentType + "`n" + $xHeaders + "`n" + $resource

    $bytesToHash = [Text.Encoding]::UTF8.GetBytes($stringToHash)
    $keyBytes = [Convert]::FromBase64String($sharedKey)

    $sha256 = New-Object System.Security.Cryptography.HMACSHA256
    $sha256.Key = $keyBytes
    $calculatedHash = $sha256.ComputeHash($bytesToHash)
    $encodedHash = [Convert]::ToBase64String($calculatedHash)
    $authorization = 'SharedKey {0}:{1}' -f $customerId,$encodedHash
    return $authorization
}


# Create the function to create and post the request
Function Post-LogAnalyticsData($customerId, $sharedKey, $body, $logType)
{
    $method = "POST"
    $contentType = "application/json"
    $resource = "/api/logs"
    $rfc1123date = [DateTime]::UtcNow.ToString("r")
    $contentLength = $body.Length
    $signature = Build-Signature `
        -customerId $customerId `
        -sharedKey $sharedKey `
        -date $rfc1123date `
        -contentLength $contentLength `
        -method $method `
        -contentType $contentType `
        -resource $resource
    $uri = "https://" + $customerId + ".ods.opinsights.azure.com" + $resource + "?api-version=2016-04-01"

    $headers = @{
        "Authorization" = $signature;
        "Log-Type" = $logType;
        "x-ms-date" = $rfc1123date;
        "time-generated-field" = $TimeStampField;
    }

    $response = Invoke-WebRequest -Uri $uri -Method $method -ContentType $contentType -Headers $headers -Body $body -UseBasicParsing
    return $response.StatusCode

}

# Submit the data to the API endpoint
Post-LogAnalyticsData -customerId $customerId -sharedKey $sharedKey -body ([System.Text.Encoding]::UTF8.GetBytes($json)) -logType $logType  
```

### <a name="c-sample"></a>C# örneği
```csharp
using System;
using System.Net;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Security.Cryptography;
using System.Text;
using System.Threading.Tasks;

namespace OIAPIExample
{
    class ApiExample
    {
        // An example JSON object, with key/value pairs
        static string json = @"[{""DemoField1"":""DemoValue1"",""DemoField2"":""DemoValue2""},{""DemoField3"":""DemoValue3"",""DemoField4"":""DemoValue4""}]";

        // Update customerId to your Log Analytics workspace ID
        static string customerId = "xxxxxxxx-xxx-xxx-xxx-xxxxxxxxxxxx";

        // For sharedKey, use either the primary or the secondary Connected Sources client authentication key   
        static string sharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx";

        // LogName is name of the event type that is being submitted to Azure Monitor
        static string LogName = "DemoExample";

        // You can use an optional field to specify the timestamp from the data. If the time field is not specified, Azure Monitor assumes the time is the message ingestion time
        static string TimeStampField = "";

        static void Main()
        {
            // Create a hash for the API signature
            var datestring = DateTime.UtcNow.ToString("r");
            var jsonBytes = Encoding.UTF8.GetBytes(json);
            string stringToHash = "POST\n" + jsonBytes.Length + "\napplication/json\n" + "x-ms-date:" + datestring + "\n/api/logs";
            string hashedString = BuildSignature(stringToHash, sharedKey);
            string signature = "SharedKey " + customerId + ":" + hashedString;

            PostData(signature, datestring, json);
        }

        // Build the API signature
        public static string BuildSignature(string message, string secret)
        {
            var encoding = new System.Text.ASCIIEncoding();
            byte[] keyByte = Convert.FromBase64String(secret);
            byte[] messageBytes = encoding.GetBytes(message);
            using (var hmacsha256 = new HMACSHA256(keyByte))
            {
                byte[] hash = hmacsha256.ComputeHash(messageBytes);
                return Convert.ToBase64String(hash);
            }
        }

        // Send a request to the POST API endpoint
        public static void PostData(string signature, string date, string json)
        {
            try
            {
                string url = "https://" + customerId + ".ods.opinsights.azure.com/api/logs?api-version=2016-04-01";

                System.Net.Http.HttpClient client = new System.Net.Http.HttpClient();
                client.DefaultRequestHeaders.Add("Accept", "application/json");
                client.DefaultRequestHeaders.Add("Log-Type", LogName);
                client.DefaultRequestHeaders.Add("Authorization", signature);
                client.DefaultRequestHeaders.Add("x-ms-date", date);
                client.DefaultRequestHeaders.Add("time-generated-field", TimeStampField);

                System.Net.Http.HttpContent httpContent = new StringContent(json, Encoding.UTF8);
                httpContent.Headers.ContentType = new MediaTypeHeaderValue("application/json");
                Task<System.Net.Http.HttpResponseMessage> response = client.PostAsync(new Uri(url), httpContent);

                System.Net.Http.HttpContent responseContent = response.Result.Content;
                string result = responseContent.ReadAsStringAsync().Result;
                Console.WriteLine("Return Result: " + result);
            }
            catch (Exception excep)
            {
                Console.WriteLine("API Post Exception: " + excep.Message);
            }
        }
    }
}

```

### <a name="python-2-sample"></a>Python 2 örneği
```python
import json
import requests
import datetime
import hashlib
import hmac
import base64

# Update the customer ID to your Log Analytics workspace ID
customer_id = 'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'

# For the shared key, use either the primary or the secondary Connected Sources client authentication key   
shared_key = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# The log type is the name of the event that is being submitted
log_type = 'WebMonitorTest'

# An example JSON web monitor object
json_data = [{
   "slot_ID": 12345,
    "ID": "5cdad72f-c848-4df0-8aaa-ffe033e75d57",
    "availability_Value": 100,
    "performance_Value": 6.954,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "true"
},
{   
    "slot_ID": 67890,
    "ID": "b6bee458-fb65-492e-996d-61c4d7fbb942",
    "availability_Value": 100,
    "performance_Value": 3.379,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "false"
}]
body = json.dumps(json_data)

#####################
######Functions######  
#####################

# Build the API signature
def build_signature(customer_id, shared_key, date, content_length, method, content_type, resource):
    x_headers = 'x-ms-date:' + date
    string_to_hash = method + "\n" + str(content_length) + "\n" + content_type + "\n" + x_headers + "\n" + resource
    bytes_to_hash = bytes(string_to_hash).encode('utf-8')  
    decoded_key = base64.b64decode(shared_key)
    encoded_hash = base64.b64encode(hmac.new(decoded_key, bytes_to_hash, digestmod=hashlib.sha256).digest())
    authorization = "SharedKey {}:{}".format(customer_id,encoded_hash)
    return authorization

# Build and send a request to the POST API
def post_data(customer_id, shared_key, body, log_type):
    method = 'POST'
    content_type = 'application/json'
    resource = '/api/logs'
    rfc1123date = datetime.datetime.utcnow().strftime('%a, %d %b %Y %H:%M:%S GMT')
    content_length = len(body)
    signature = build_signature(customer_id, shared_key, rfc1123date, content_length, method, content_type, resource)
    uri = 'https://' + customer_id + '.ods.opinsights.azure.com' + resource + '?api-version=2016-04-01'

    headers = {
        'content-type': content_type,
        'Authorization': signature,
        'Log-Type': log_type,
        'x-ms-date': rfc1123date
    }

    response = requests.post(uri,data=body, headers=headers)
    if (response.status_code >= 200 and response.status_code <= 299):
        print 'Accepted'
    else:
        print "Response code: {}".format(response.status_code)

post_data(customer_id, shared_key, body, log_type)
```
## <a name="alternatives-and-considerations"></a>Alternatifler ve değerlendirmeler
Veri Toplayıcı API'si Azure Günlükleri'nde serbest biçimli veri toplama gereksinimlerinizin çoğunu karşılaması gerekirken, API'nin bazı sınırlamalarını aşmak için bir alternatifin gerekli olabileceği durumlar vardır. Tüm seçenekleriniz aşağıdaki gibidir, önemli hususlar şunlardır:

| Alternatif | Açıklama | En uygun |
|---|---|---|
| [Özel olaylar](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics?toc=%2Fazure%2Fazure-monitor%2Ftoc.json#properties): Uygulama Öngörülerinde Yerel SDK tabanlı alım | Uygulamanız dahilinde genellikle bir SDK aracılığıyla enstrümante bulunan Uygulama Öngörüleri, Özel Etkinlikler aracılığıyla özel veri gönderme olanağı sunar. | <ul><li> Uygulamanızda oluşturulan ancak Varsayılan veri türlerinden biri (istekler, bağımlılıklar, özel durumlar vb.) aracılığıyla SDK tarafından alınmayan veriler.</li><li> Uygulama Öngörüleri'ndeki diğer uygulama verileriyle en sık ilişkili olan veriler </li></ul> |
| Azure Monitör Günlüklerinde Veri Toplayıcı API'si | Azure Monitör Günlükleri'ndeki Veri Toplayıcı API'si, verileri yutmanın tamamen açık uçlu bir yoludur. JSON nesnesinde biçimlendirilmiş tüm veriler buraya gönderilebilir. Gönderildikten sonra işlenir ve Günlükler'deki diğer verilerle veya diğer Application Insights verileriyle ilişkilendirilecek şekilde Günlükler'de kullanılabilir. <br/><br/> Verileri, bu dosyaların işlenilip Log Analytics'e yüklendiği bir Azure Blob blob'una dosya olarak yüklemek oldukça kolaydır. Böyle bir boru hattının örnek bir uygulama için [bu](https://docs.microsoft.com/azure/log-analytics/log-analytics-create-pipeline-datacollector-api) makaleye bakın. | <ul><li> Uygulama Öngörüleri içinde bulunan bir uygulama içinde mutlaka oluşturulmayan veriler.</li><li> Örnekler arasında arama ve olgu tabloları, başvuru verileri, önceden toplanmış istatistikler ve benzeri sayılabilir. </li><li> Diğer Azure Monitör verileri (Uygulama Öngörüleri, diğer Günlükler veri türleri, Güvenlik Merkezi, Kapsayıcılar/Sanal Kullanımlar için Azure Monitörü vb.) karşı çapraz başvurulacak veriler için tasarlanmıştır. </li></ul> |
| [Azure Veri Gezgini](https://docs.microsoft.com/azure/data-explorer/ingest-data-overview) | Azure Veri Gezgini (ADX), Application Insights Analytics ve Azure Monitor Günlükleri'ne güç veren veri platformudur. Şimdi Genel Olarak Kullanılabilir ("GA"), ham formunda veri platformu kullanarak küme (RBAC, bekletme oranı, şema, vb) üzerinde tam esneklik (ancak yönetim yükü gerektiren) sağlar. [ADX, CSV, TSV ve JSON](https://docs.microsoft.com/azure/kusto/management/mappings?branch=master) dosyaları da dahil olmak üzere birçok [yutma seçeneği](https://docs.microsoft.com/azure/data-explorer/ingest-data-overview#ingestion-methods) sunar. | <ul><li> Uygulama Öngörüleri veya Günlükleri altında başka verilerle ilişkilendirilmeyecek veriler. </li><li> Azure Monitör Günlükleri'nde günümüzde bulunmayan gelişmiş alım veya işleme özellikleri gerektiren veriler. </li></ul> |


## <a name="next-steps"></a>Sonraki adımlar
- Log Analytics çalışma alanından veri almak için [Günlük Arama API'sını](../log-query/log-query-overview.md) kullanın.

- Logic Apps iş akışını kullanarak Azure Monitor'a [veri toplayıcı API'si ile](create-pipeline-datacollector-api.md) nasıl bir veri ardışık alanı oluşturma hakkında daha fazla bilgi edinin.
