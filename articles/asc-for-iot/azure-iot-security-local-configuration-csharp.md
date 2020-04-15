---
title: Güvenlik aracısı yerel yapılandırması (C#)
description: C# için güvenlik aracısı yerel yapılandırma dosyası olan IoT güvenlik hizmeti için Azure Güvenlik Merkezi hakkında daha fazla bilgi edinin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 2cf6a49b-5d35-491f-abc3-63ec24eb4bc2
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2019
ms.author: mlottner
ms.openlocfilehash: adf0d72763e0cb1892d64c68a6dce05abbf6f582
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81311672"
---
# <a name="understanding-the-local-configuration-file-c-agent"></a>Yerel yapılandırma dosyasını anlama (C# aracısı)

Azure Güvenlik Merkezi ioT güvenlik aracısı için yerel bir yapılandırma dosyasından yapılandırmalar kullanır.

Güvenlik aracısı, aracı başlatıldığında yapılandırma dosyasını bir kez okur. Yerel yapılandırma dosyasında bulunan yapılandırmalar hem kimlik doğrulama yapılandırması hem de diğer aracıyla ilgili yapılandırmaları içerir.

C# güvenlik aracısı birden çok yapılandırma dosyası kullanır:

- **General.config** - Aracı ile ilgili yapılandırmalar.
- **Authentication.config** - Kimlik doğrulamayla ilgili yapılandırma (kimlik doğrulama ayrıntıları dahil).
- **SecurityIotInterface.config** - IoT ile ilgili yapılandırmalar.

Yapılandırma dosyaları varsayılan yapılandırmayı içerir. Temsilcilik yapılandırması aracı yükleme sırasında doldurulur ve aracı yeniden başlatıldığında yapılandırma dosyasında değişiklikler yapılır.

## <a name="configuration-file-location"></a>Yapılandırma dosyası konumu

Linux için:

- İşletim sistemi yapılandırma dosyaları. `/var/ASCIoTAgent`

Windows için:

- İşletim sistemi yapılandırma dosyaları güvenlik aracısının dizininde yer alır.

### <a name="generalconfig-configurations"></a>General.config yapılandırmaları

| Yapılandırma Adı | Olası değerler | Ayrıntılar |
|:-----------|:---------------|:--------|
| agentId | GUID | Aracı benzersiz tanımlayıcı |
| readRemoteConfigurationTimeout | TimeSpan | IoT Hub'dan uzak yapılandırma alma süresi. Aracı, yapılandırmayı belirtilen süre içinde getiremezse, işlem zaman dolacaktır.|
| schedulerInterval | TimeSpan | Dahili zamanlayıcı aralığı. |
| yapımcıInterval | TimeSpan | Olay üretici işçi aralığı. |
| consumerInterval | TimeSpan | Olay tüketici işçi aralığı. |
| highPriorityQueueSizePercentage | 0 < sayısı < 1 | Toplam önbelleğin yüksek öncelikli iletiler için ayrılmış bölümü. |
| Loglevel | "Kapalı", "Ölümcül", "Hata", "Uyarı", "Bilgi", "Hata Ayıklama"  | Bu önem in verime konsoluna (Linux'ta Syslog) eşit ve üstünde günlük iletileri günlüğe kaydedilir. |
| fileLogLevel |  "Kapalı", "Ölümcül", "Hata", "Uyarı", "Bilgi", "Hata Ayıklama"| Bu önem derecesinin üzerinde ve eşit olan günlük iletileri dosyaya kaydedilir (Linux'ta Syslog). |
| diagnostikVerbosityDüzey | "Yok", "Bazıları", "Hepsi", | Tanısal olayların ayrıntılılık düzeyi. Hiçbiri - tanılama olayları gönderilmez, Bazı - Sadece yüksek öneme sahip tanıolayları gönderilir, Tüm - tüm günlükleri de tanılama olayları olarak gönderilir. |
| Logfilepath | Dosyaya giden yol | FileLogLevel > Kapalı ise, günlükleri bu dosyaya yazılır. |
| varsayılanEventEventPriority | "Yüksek", "Düşük", "Kapalı" | Varsayılan olay önceliği. |

### <a name="generalconfig-example"></a>General.config örneği

```XML
<?xml version="1.0" encoding="utf-8"?>
<General>
  <add key="agentId" value="da00006c-dae9-4273-9abc-bcb7b7b4a987" />
  <add key="readRemoteConfigurationTimeout" value="00:00:30" />
  <add key="schedulerInterval" value="00:00:01" />
  <add key="producerInterval" value="00:02:00" />
  <add key="consumerInterval" value="00:02:00" />
  <add key="highPriorityQueueSizePercentage" value="0.5" />
  <add key="logLevel" value="Information" />
  <add key="fileLogLevel" value="Off"/>
  <add key="diagnosticVerbosityLevel" value="Some" />
  <add key="logFilePath" value="IotAgentLog.log" />
  <add key="defaultEventPriority" value="Low"/>
</General>
```

### <a name="authenticationconfig"></a>Kimlik Doğrulama.config

| Yapılandırma adı | Olası değerler | Ayrıntılar |
|:-----------|:---------------|:--------|
| Modulename | string | Güvenlik modülü kimliğinin adı. Bu ad, aygıttaki modül kimlik adına karşılık getirmelidir. |
| deviceId | string | Aygıtın kimliği (Azure IoT Hub'ında kayıtlı olarak). || schedulerInterval | TimeSpan dizesi | Dahili zamanlayıcı aralığı. |
| ağ geçidiHostname | string | Azure Iot Hub'ının ana bilgisayarı. Genellikle <benim-hub>.azure-devices.net |
| Filepath | string - dosyaya giden yol | Kimlik doğrulama sırrını içeren dosyaya giden yol.|
| type | "SymmetricKey", "SelfSignedCertificate" | Kimlik doğrulama için kullanıcı sırrı. Kullanıcı sırrı Simetrik bir anahtarsa *SymmetricKey'i* seçin, gizli kendi imzalı bir sertifikaysa *kendi imzalı sertifikayı* seçin. |
| identity | "DPS", "Modül", "Cihaz" | Kimlik doğrulama kimliği - DPS üzerinden kimlik doğrulama yapılıyorsa DPS, kimlik doğrulama modül kimlik bilgileri kullanılarak yapılıyorsa modül veya aygıt kimlik bilgileri kullanılarak yapılan aygıt.
| sertifikaLocationKind |  "LocalFile", "Mağaza" | LocalFile sertifika bir dosyada depolanmışsa, sertifika bir sertifika deposunda bulunuyorsa depolayın. |
| idScope | string | DPS'nin kimlik kapsamı |
| registrationId | string  | DPS cihaz kayıt kimliği. |
|

### <a name="authenticationconfig-example"></a>Authentication.config örneği

```XML
<?xml version="1.0" encoding="utf-8"?>
<Authentication>
  <add key="moduleName" value="azureiotsecurity"/>
  <add key="deviceId" value="d1"/>
  <add key="gatewayHostname" value=""/>
  <add key="filePath" value="c:\p-dps-d1.pfx"/>
  <add key="type" value="SelfSignedCertificate" />                     <!-- SymmetricKey, SelfSignedCertificate-->
  <add key="identity" value="DPS" />                 <!-- Device, Module, DPS -->
  <add key="certificateLocationKind" value="LocalFile" />  <!-- LocalFile, Store -->
  <add key="idScope" value="0ne0005335B"/>
  <add key="registrationId" value="d1"/>
</Authentication>
```

### <a name="securityiotinterfaceconfig"></a>GüvenlikIotInterface.config

| Yapılandırma Adı | Olası değerler | Ayrıntılar |
|:-----------|:---------------|:--------|
| Transporttype | "Ampq" "Mqtt" | IoT Hub taşıma türü. |
|

### <a name="securityiotinterfaceconfig-example"></a>SecurityIotInterface.config örnek

```XML
<ExternalInterface>
  <add key="facadeType"  value="Microsoft.Azure.Security.IoT.Agent.Common.SecurityIoTHubInterface, Security.Common" />
  <add key="transportType" value="Amqp"/>
</ExternalInterface>
```

## <a name="next-steps"></a>Sonraki adımlar

- IoT hizmetine [Genel Bakış](overview.md) için Azure Güvenlik Merkezi'ni okuyun
- IoT [Mimarisi](architecture.md) için Azure Güvenlik Merkezi hakkında daha fazla bilgi edinin
- IoT [hizmeti](quickstart-onboard-iot-hub.md) için Azure Güvenlik Merkezi'ni etkinleştirme
- IoT hizmeti [SSS](resources-frequently-asked-questions.md) için Azure Güvenlik Merkezi'ni okuyun
- Ham güvenlik [verilerine](how-to-security-data-access.md) nasıl erişin gerektiğini öğrenin
- [Önerileri](concept-recommendations.md) anlama
- Güvenlik [uyarılarını](concept-security-alerts.md) anlama
