---
title: IoT Edge günlüklerini alma-Azure IoT Edge
description: Modül günlüğü alımı IoT Edge ve Azure Blob depolamaya yükleyin.
author: v-tcassi
manager: philmea
ms.author: v-tcassi
ms.date: 09/14/2020
ms.topic: conceptual
ms.reviewer: veyalla
ms.service: iot-edge
ms.custom: devx-track-azurecli
services: iot-edge
ms.openlocfilehash: 64264028706c1493f687f032a7ec39e69188bd45
ms.sourcegitcommit: 2989396c328c70832dcadc8f435270522c113229
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2020
ms.locfileid: "92171908"
---
# <a name="retrieve-logs-from-iot-edge-deployments"></a>IoT Edge dağıtımlarından günlükleri alma

IoT Edge aracı modülüne dahil olan doğrudan yöntemleri kullanarak cihaza fiziksel veya SSH erişimi gerekmeden IoT Edge dağıtımlarınızdan günlükleri alın. Doğrudan yöntemler cihaza uygulanır ve sonra buluttan çağrılabilir. IoT Edge Aracısı, IoT Edge cihazlarınızı uzaktan izlemenize ve yönetmenize yardımcı olan doğrudan yöntemler içerir. Bu makalede ele alınan doğrudan Yöntemler 1.0.10 sürümü ile birlikte kullanılabilir.

Doğrudan yöntemler hakkında daha fazla bilgi için, bunları kullanma ve kendi modüllerinize uygulama hakkında daha fazla bilgi için, bkz. [IoT Hub doğrudan yöntemleri anlama ve çağırma](../iot-hub/iot-hub-devguide-direct-methods.md).

Bu doğrudan yöntemlerin adları, büyük/küçük harfe duyarlı olarak işlenir.

## <a name="recommended-logging-format"></a>Önerilen günlük biçimi

Gerekli olmasa da, bu özellikle en iyi şekilde uyumluluk için önerilen günlük biçimi:

```
<{Log Level}> {Timestamp} {Message Text}
```

`{Log Level}`[Syslog önem düzeyi biçimini](https://wikipedia.org/wiki/Syslog#Severity_lnevel) izlemelidir ve `{Timestamp}` olarak biçimlendirilmelidir `yyyy-mm-dd hh:mm:ss.fff zzz` .

[IoT Edge Içindeki günlükçü sınıfı](https://github.com/Azure/iotedge/blob/master/edge-util/src/Microsoft.Azure.Devices.Edge.Util/Logger.cs) , kurallı bir uygulama işlevi görür.

## <a name="retrieve-module-logs"></a>Modül günlüklerini alma

Bir IoT Edge modülünün günlüklerini almak için **GetModuleLogs** Direct metodunu kullanın.

Bu yöntem, aşağıdaki şemaya sahip bir JSON yükünü kabul eder:

```json
    {
       "schemaVersion": "1.0",
       "items": [
          {
             "id": "regex string",
             "filter": {
                "tail": int,
                "since": int,
                "until": int,
                "loglevel": int,
                "regex": "regex string"
             }
          }
       ],
       "encoding": "gzip/none",
       "contentType": "json/text" 
    }
```

| Ad | Tür | Açıklama |
|-|-|-|
| schemaVersion | string | Ayarla `1.0` |
| öğeler | JSON dizisi | `id`Ve tanımlama gruplarını içeren bir dizi `filter` . |
| ID | string | Modül adını sağlayan bir normal ifade. Bir Edge cihazında birden çok modülle eşleşir. [.Net normal ifade](/dotnet/standard/base-types/regular-expressions) biçimi bekleniyor. |
| filtre | JSON bölümü | Kayıt düzeninde normal ifadeyle eşleşen modüller için uygulanacak günlük filtreleri `id` . |
| Connect | tamsayı | En son 'den başlayarak almak için geçmişte bulunan günlük satırı sayısı. Seçim. |
| getirildikten | tamsayı | Yalnızca bu kez bir süre (1 d, 90 m, 2 gün 3 saat 2 dakika), rfc3339 zaman damgası veya UNIX zaman damgası olarak günlükleri geri döndürür.  `tail`Ve `since` belirtilirse, Günlükler `since` önce değeri kullanılarak alınır. Sonra `tail` değer sonuca uygulanır ve nihai sonuç döndürülür. Seçim. |
| Until | tamsayı | Yalnızca bir rfc3339 zaman damgası, UNIX zaman damgası veya süre (1 d, 90 m, 2 gün 3 saat 2 dakika) olarak belirtilen süreden önceki günlükleri döndürür. Seçim. |
| günlük düzeyi | tamsayı | Günlük satırlarını belirtilen günlük düzeyinden küçük veya buna eşit olarak filtreleyin. Günlük satırları önerilen günlük biçimini izlemelidir ve [Syslog önem düzeyi](https://en.wikipedia.org/wiki/Syslog#Severity_level) standardını kullanmalıdır. Seçim. |
| Regex | string | [.Net normal ifadeler](/dotnet/standard/base-types/regular-expressions) biçimini kullanarak belirtilen normal ifadeyle eşleşen içeriğe sahip olan günlük satırlarını filtreleyin. Seçim. |
| encoding | string | `gzip` veya `none`. `none` varsayılan değerdir. |
| contentType | string | `json` veya `text`. `text` varsayılan değerdir. |

> [!NOTE]
> Günlük içeriği, şu anda 128 KB olan doğrudan yöntemlerin yanıt boyutu sınırını aşarsa, yanıt bir hata döndürür.

Günlüklerin başarılı bir şekilde alınması bir **"Status": 200** ve ardından, isteğiniz içinde belirttiğiniz ayarlara göre filtrelenmiş, modülünden alınan günlükleri içeren bir yük izler.

Örnek:

```azurecli
az iot hub invoke-module-method --method-name 'GetModuleLogs' -n <hub name> -d <device id> -m '$edgeAgent' --method-payload \
'
    {
       "schemaVersion": "1.0",
       "items": [
          {
             "id": "edgeAgent",
             "filter": {
                "tail": 10
             }
          }
       ],
       "encoding": "none",
       "contentType": "text"
    }
'
```

Azure portal Yöntem adı `GetModuleLogs` ve AŞAĞıDAKI JSON yüküyle yöntemi çağırın:

```json
    {
       "schemaVersion": "1.0",
       "items": [
          {
             "id": "edgeAgent",
             "filter": {
                "tail": 10
             }
          }
       ],
       "encoding": "none",
       "contentType": "text"
    }
```

![Azure portal ' GetModuleLogs ' doğrudan metodunu çağır](./media/how-to-retrieve-iot-edge-logs/invoke-get-module-logs.png)

Ayrıca, sıkıştırılmış bir yanıtı işlemek için, CLı çıkışını [gzip](https://en.wikipedia.org/wiki/Gzip)gibi Linux yardımcı programlarına de kanal oluşturarak yapabilirsiniz. Örnek:

```azurecli
az iot hub invoke-module-method \
  --method-name 'GetModuleLogs' \
  -n <hub name> \
  -d <device id> \
  -m '$edgeAgent' \
  --method-payload '{"contentType": "text","schemaVersion": "1.0","encoding": "gzip","items": [{"id": "edgeHub","filter": {"since": "2d","tail": 1000}}],}' \
  -o tsv --query 'payload[0].payloadBytes' \
  | base64 --decode \
  | gzip -d
```

## <a name="upload-module-logs"></a>Modül günlüklerini karşıya yükle

İstenen günlükleri belirtilen bir Azure Blob depolama kapsayıcısına göndermek için **Uploadmodulelogs** Direct metodunu kullanın.

Bu yöntem, "sasUrl" anahtarının eklenmesiyle **GetModuleLogs**ile benzer bir JSON yükünü kabul eder:

```json
    {
       "schemaVersion": "1.0",
       "sasUrl": "Full path to SAS URL",
       "items": [
          {
             "id": "regex string",
             "filter": {
                "tail": int,
                "since": int,
                "until": int,
                "loglevel": int,
                "regex": "regex string"
             }
          }
       ],
       "encoding": "gzip/none",
       "contentType": "json/text" 
    }
```

| Ad | Tür | Açıklama |
|-|-|-|
| sasURL | dize (URI) | [Azure Blob depolama kapsayıcısına yazma erişimi olan paylaşılan erişim imzası URL 'si](/archive/blogs/jpsanders/easily-create-a-sas-to-download-a-file-from-azure-storage-using-azure-storage-explorer). |

Günlükleri karşıya yükleme başarılı bir isteği, **"durum": 200** ve ardından aşağıdaki şemaya sahip bir yük getirir:

```json
    {
        "status": "string",
        "message": "string",
        "correlationId": "GUID"
    }
```

| Ad | Tür | Açıklama |
|-|-|-|
| durum | string | Bunlardan biri,,, `NotStarted` `Running` `Completed` `Failed` veya `Unknown` . |
| message | string | Hata olduğunda ileti, aksi halde boş dize. |
| correlationId | string   | Karşıya yükleme isteğinin durumunu sorgulama KIMLIĞI. |

Örnek:

Aşağıdaki çağrı, sıkıştırılmış JSON biçiminde tüm modüllerden son 100 günlük satırını karşıya yükler:

```azurecli
az iot hub invoke-module-method --method-name UploadModuleLogs -n <hub name> -d <device id> -m \$edgeAgent --method-payload \
'
    {
        "schemaVersion": "1.0",
        "sasUrl": "<sasUrl>",
        "items": [
            {
                "id": ".*",
                "filter": {
                    "tail": 100
                }
            }
        ],
        "encoding": "gzip",
        "contentType": "json"
    }
'
```

Aşağıdaki çağrı, edgeAgent ve edgeHub 'daki son 100 günlük satırını, sıkıştırılmamış metin biçimindeki Tempalgılayıcı modülünden son 1000 günlük satırı ile karşıya yükler:

```azurecli
az iot hub invoke-module-method --method-name UploadModuleLogs -n <hub name> -d <device id> -m \$edgeAgent --method-payload \
'
    {
        "schemaVersion": "1.0",
        "sasUrl": "<sasUrl>",
        "items": [
            {
                "id": "edge",
                "filter": {
                    "tail": 100
                }
            },
            {
                "id": "tempSensor",
                "filter": {
                    "tail": 1000
                }
            }
        ],
        "encoding": "none",
        "contentType": "text"
    }
'
```

Azure portal, `UploadModuleLogs` sasURL 'sini bilgilerinizi doldurduktan sonra Yöntem adı ve AŞAĞıDAKI JSON yükünün bulunduğu yöntemi çağırın:

```json
    {
       "schemaVersion": "1.0",
       "sasUrl": "<sasUrl>",
       "items": [
          {
             "id": "edgeAgent",
             "filter": {
                "tail": 10
             }
          }
       ],
       "encoding": "none",
       "contentType": "text"
    }
```

![Azure portal ' UploadModuleLogs ' doğrudan metodunu çağır](./media/how-to-retrieve-iot-edge-logs/invoke-upload-module-logs.png)

## <a name="upload-support-bundle-diagnostics"></a>Destek paketi tanılamayı karşıya yükle

Mevcut bir Azure Blob depolama kapsayıcısına IoT Edge modül günlüklerinin bir ZIP dosyasını paketleyip yüklemek için **Uploadsupportpaketini** doğrudan yöntemini kullanın. Bu doğrudan yöntem, [`iotedge support-bundle`](./troubleshoot.md#gather-debug-information-with-support-bundle-command) günlükleri almak için IoT Edge cihazınızda komutunu çalıştırır.

Bu yöntem, aşağıdaki şemaya sahip bir JSON yükünü kabul eder:

```json
    {
        "schemaVersion": "1.0",
        "sasUrl": "Full path to SAS url",
        "since": "2d",
        "until": "1d",
        "edgeRuntimeOnly": false
    }
```

| Ad | Tür | Açıklama |
|-|-|-|
| schemaVersion | string | Ayarla `1.0` |
| sasURL | dize (URI) | [Azure Blob depolama kapsayıcısına yazma erişimi olan paylaşılan erişim Imzası URL 'SI](/archive/blogs/jpsanders/easily-create-a-sas-to-download-a-file-from-azure-storage-using-azure-storage-explorer) |
| getirildikten | tamsayı | Yalnızca bu kez bir süre (1 d, 90 m, 2 gün 3 saat 2 dakika), rfc3339 zaman damgası veya UNIX zaman damgası olarak günlükleri geri döndürür. Seçim. |
| Until | tamsayı | Yalnızca bir rfc3339 zaman damgası, UNIX zaman damgası veya süre (1 d, 90 m, 2 gün 3 saat 2 dakika) olarak belirtilen süreden önceki günlükleri döndürür. Seçim. |
| edgeRuntimeOnly | boolean | True ise yalnızca Edge aracısından, Edge hub 'ından ve Edge güvenlik arka plan programından günlükleri geri döndürür. Varsayılan: false.  Seçim. |

> [!IMPORTANT]
> IoT Edge destek paketi, kişisel olarak tanımlanabilir bilgiler içerebilir.

Günlükleri karşıya yükleme başarılı olan bir istek, **"durum": 200** ve ardından **uploadmodulelogs** yanıtı ile aynı şemaya sahip bir yük getirir:

```json
    {
        "status": "string",
        "message": "string",
        "correlationId": "GUID"
    }
```

| Ad | Tür | Açıklama |
|-|-|-|
| durum | string | Bunlardan biri,,, `NotStarted` `Running` `Completed` `Failed` veya `Unknown` . |
| message | string | Hata olduğunda ileti, aksi halde boş dize. |
| correlationId | string   | Karşıya yükleme isteğinin durumunu sorgulama KIMLIĞI. |

Örnek:

```azurecli
az iot hub invoke-module-method --method-name 'UploadSupportBundle' -n <hub name> -d <device id> -m '$edgeAgent' --method-payload \
'
    {
        "schemaVersion": "1.0",
        "sasUrl": "Full path to SAS url",
        "since": "2d",
        "until": "1d",
        "edgeRuntimeOnly": false
    }
'
```

Azure portal, `UploadSupportBundle` sasURL 'sini bilgilerinizi doldurduktan sonra Yöntem adı ve AŞAĞıDAKI JSON yükünün bulunduğu yöntemi çağırın:

```json
    {
        "schemaVersion": "1.0",
        "sasUrl": "Full path to SAS url",
        "since": "2d",
        "until": "1d",
        "edgeRuntimeOnly": false
    }
```

![Azure portal içinde doğrudan ' Uploadsupportpaket ' metodunu çağır](./media/how-to-retrieve-iot-edge-logs/invoke-upload-support-bundle.png)

## <a name="get-upload-request-status"></a>Karşıya yükleme isteği durumunu al

Karşıya yükleme günlüğü isteğinin durumunu sorgulamak için **Gettaskstatus** doğrudan yöntemini kullanın. **Gettaskstatus** istek yükü, `correlationId` görevin durumunu almak için karşıya yükleme günlüğü isteklerini kullanır. , `correlationId` **Uploadmodulelogs** doğrudan yöntem çağrısına yanıt olarak döndürülür.

Bu yöntem, aşağıdaki şemaya sahip bir JSON yükünü kabul eder:

```json
    {
      "schemaVersion": "1.0",
      "correlationId": "<GUID>"
    }
```

Günlükleri karşıya yükleme başarılı olan bir istek, **"durum": 200** ve ardından **uploadmodulelogs** yanıtı ile aynı şemaya sahip bir yük getirir:

```json
    {
        "status": "string",
        "message": "string",
        "correlationId": "GUID"
    }
```

| Ad | Tür | Açıklama |
|-|-|-|
| durum | string | Bunlardan biri,,, `NotStarted` `Running` `Completed` `Failed` veya `Unknown` . |
| message | string | Hata olduğunda ileti, aksi halde boş dize. |
| correlationId | string   | Karşıya yükleme isteğinin durumunu sorgulama KIMLIĞI. |

Örnek:

```azurecli
az iot hub invoke-module-method --method-name 'GetTaskStatus' -n <hub name> -d <device id> -m '$edgeAgent' --method-payload \
'
    {
      "schemaVersion": "1.0",
      "correlationId": "<GUID>"
    }
'
```

Azure portal, `GetTaskStatus` GUID 'yi bilgilerinizi doldurduktan sonra Yöntem adı ve AŞAĞıDAKI JSON yükünün bulunduğu yöntemi çağırın:

```json
    {
      "schemaVersion": "1.0",
      "correlationId": "<GUID>"
    }
```

![Azure portal ' GetTaskStatus ' doğrudan metodunu çağır](./media/how-to-retrieve-iot-edge-logs/invoke-get-task-status.png)

## <a name="next-steps"></a>Sonraki adımlar

[IoT Edge Aracısı ve IoT Edge hub modülünün özellikleri TWINS](module-edgeagent-edgehub.md)
