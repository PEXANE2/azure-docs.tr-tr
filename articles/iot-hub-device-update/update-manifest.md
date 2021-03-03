---
title: IoT Hub güncelleştirme bildirimi için cihaz güncelleştirmesi | Microsoft Docs
description: Güncelleştirme sırasında özelliklerin cihaz güncelleştirme hizmetinden cihaza nasıl gönderileceğini öğrenin
author: andbrown
ms.author: andbrown
ms.date: 2/17/2021
ms.topic: conceptual
ms.service: iot-hub-device-update
ms.openlocfilehash: ea438a4a8db8dce9cf2acb0b5b3ff4e250c3cf39
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101663957"
---
# <a name="device-update-for-iot-hub-update-manifest"></a>IoT Hub güncelleştirme bildirimi için cihaz güncelleştirmesi

## <a name="overview"></a>Genel Bakış

IoT Hub cihaz güncelleştirmesi, eylemleri ve ayrıca [AzureDeviceUpdateCore. OrchestratorMetadata: 4](./device-update-plug-and-play.md)arabirim özellikleri aracılığıyla bu eylemleri destekleyen meta verileri iletmek için bir _güncelleştirme bildirimi_ kullanır.
Bu belgede, `updateManifest` özelliğinin, arabirim içinde, `AzureDeviceUpdateCore.OrchestratorMetadata:4` Cihaz Güncelleştirme Aracısı tarafından nasıl kullanıldığı hakkında temel bilgiler açıklanmaktadır. Arabirim Özellikleri, cihaz güncelleştirme `AzureDeviceUpdateCore.OrchestratorMetadata:4` aracısına IoT Hub hizmeti Için cihaz güncelleştirmesinden gönderilir. , `updateManifest` Cihaz Güncelleştirme Aracısı tarafından ayrıştırılmış bir seri hale GETIRILMIŞ JSON nesnesidir.

### <a name="an-example-update-manifest"></a>Örnek bir güncelleştirme bildirimi

```JSON
{
    "manifestVersion": "1",
    "updateId": {
        "provider": "DuTest",
        "name": "DuTestUser",
        "version": "2020.611.534.16"
    },
    "updateType": "microsoft/swupdate:1",
    "installedCriteria": "1.0",
    "files": {
        "00000": {
            "fileName": "image.swu",
            "sizeInBytes": 256000,
            "hashes": {
                "sha256": "IhIIxBJpLfazQOk/PVi6SzR7BM0jf4HDqw+6gdZ3vp8="
            }
        }
    },
    "createdDateTime": "2020-06-12T00:38:13.9350278"
}
```

Güncelleştirme bildiriminin amacı, kimlik, tür, yüklü ölçüt ve güncelleştirme dosyası meta verilerini de güncelleştiren bir güncelleştirmenin içeriğini tanımlıyor. Ayrıca, güncelleştirme bildirimi, cihaz güncelleştirme aracısının orijinalliğini doğrulamasına izin vermek üzere şifreli olarak imzalanır. Güncelleştirme bildiriminin içeriği güvenli bir şekilde içeri aktarmak için nasıl kullanıldığı hakkında daha fazla bilgi için [bkz. cihaz güncelleştirme güvenliği](./device-update-security.md) belgeleri.

## <a name="import-manifest-vs-update-manifest"></a>İçeri aktarma bildirimi vs güncelleştirme bildirimi

IoT Hub için cihaz güncelleştirmesinde içeri aktarma bildirimi ve güncelleştirme bildirimi kavramları arasındaki farkları anlamak önemlidir. 
* [İçeri aktarma bildirimi](./import-concepts.md) , ilgili güncelleştirmeyi oluşturan tarafından oluşturulur. IoT Hub için cihaz güncelleştirmesine aktarılacak güncelleştirmenin içeriğini açıklar. 
* Güncelleştirme bildirimi, içeri aktarma bildiriminde tanımlanmış özelliklerden bazıları kullanılarak IoT Hub hizmeti için cihaz güncelleştirmesi tarafından otomatik olarak oluşturulur. Güncelleştirme işlemi sırasında ilgili bilgileri cihaz güncelleştirme aracısıyla iletişim kurmak için kullanılır. 

Her bildirim türünün kendi şeması ve şema sürümü vardır.

## <a name="update-manifest-properties"></a>Bildirim özelliklerini güncelleştirme

Güncelleştirme bildirimi özelliklerinin üst düzey tanımları [burada](./device-update-plug-and-play.md)bulunan arabirim tanımlarında bulunabilir. Daha derin bağlam sağlamak için özelliklere ve bunların sistemde nasıl kullanıldıklarından daha yakından bakalım.

### <a name="updateid"></a>UpdateID

`provider` `name` `version` Güncelleştirme için uyumlu cihazları belirlemede kullanılan IoT Hub güncelleştirme kimliği Için tam cihaz güncelleştirmesini temsil eden, ve içerir.

### <a name="updatetype"></a>Güncelleştirme türü

Belirli bir güncelleştirme işleyici türü tarafından işlenen güncelleştirmenin türünü temsil eder. `microsoft/swupdate:1`Görüntü tabanlı güncelleştirme ve `microsoft/apt:1` paket tabanlı güncelleştirme için ( `Update Handler Types` aşağıdaki bölümüne bakın) biçimini izler.

### <a name="installedcriteria"></a>ınstalınstalbu ölçüt

Güncelleştirmenin cihaza yüklenip yüklenmediğini belirlemede cihaz güncelleştirme aracısının güncelleştirme Işleyicisi için gereken bilgileri içeren bir dize. `Update Handler Types`Bölüm, `installedCriteria` IoT Hub Için cihaz güncelleştirmesi tarafından desteklenen her güncelleştirme türü için biçimini belgeler.

### <a name="files"></a>files

Cihaz güncelleştirme aracısına hangi dosyaların indirileceğini ve dosyaların doğru şekilde indirildiğini doğrulamak için kullanılacak karmayı söyler.
Özellik içeriğine daha yakından bakacağız `files` :

```json
"files":{
        <FILE_ID_STRING>:{
            "fileName":<STRING>,
            "sizeInBytes":<INTEGER>,
            "hashes":{
                <HASH-TYPE>:<HASH-STRING>
            }
        }
    }
```

Öğesinin dışında, `updateManifest` `fileUrls` JSON nesnesinin dizisidir.

```json
"fileUrls":{
      <FILE_ID_STRING>: <URL-in-String-Format>
 }
```

, Ve içindeki her ikisi de `FILE_ID_STRING` `fileUrls` `files` aynıdır (örneğin, içinde "0000" `files` içinde "0000" konumunda URL bulunur `fileUrls` ).

### <a name="manifestversion"></a>manifestVersion

Şema sürümünü temsil eden bir dize.

## <a name="update-handler-types"></a>Işleyici türlerini güncelleştirme

|Yöntemi Güncelleştir|Güncelleştirme Işleyicisi türü|Güncelleştirme türü|Yükleme ölçütleri|Yayımlama için beklenen dosyalar|
|-------------|-------------------|----------|-----------------|--------------|
|Görüntü tabanlı|SWUpdate|"Microsoft/swupdate: sürüm"|Başvuru görüntüsü, onun sürümünün ipucunu/etc/ADU-Version dosyasına kaydeder.  |SWUpdate resmi içeren. SWU dosyası|
|Paket tabanlı|APT|"Microsoft/Apt: sürüm"|`<name>` + "-" + `<version>` (APT bildirim dosyasında tanımlı özellikler|`<APT Update Manifest>`APT yapılandırmasını ve paket listesini içeren. JSON|

