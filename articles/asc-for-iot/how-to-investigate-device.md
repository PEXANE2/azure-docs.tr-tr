---
title: Şüpheli bir aygıtı araştırma
description: Bu şekilde, Log Analytics'i kullanarak şüpheli bir IoT aygıtını araştırmak için Azure Güvenlik Merkezi'nin IoT için nasıl kullanılacağını açıklar.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: b18b48ae-b445-48f8-9ac0-365d6e065b64
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: f333f28dc0e02e8d010f5521f298d0f0b031dbf2
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81311043"
---
# <a name="investigate-a-suspicious-iot-device"></a>Şüpheli bir IoT aygıtını araştırma

Azure Güvenlik Merkezi IoT hizmeti uyarıları, IoT aygıtlarının şüpheli etkinliklere karıştığından şüphelenildiğinde veya bir aygıtın gizliliğinin ihlal edildiğine dair göstergeler olduğunda net göstergeler sağlar.

Bu kılavuzda, kuruluşunuz için olası riskleri belirlemeye, nasıl düzelteceğine karar vermeye ve gelecekte benzer saldırıları önlemenin en iyi yollarını keşfetmeye yardımcı olmak için sağlanan araştırma önerilerini kullanın.

> [!div class="checklist"]
> * Cihazınızın verilerini bulma
> * Kql sorgularını kullanarak araştırma

## <a name="how-can-i-access-my-data"></a>Verilerime nasıl erişebilirim?

Varsayılan olarak, IoT için Azure Güvenlik Merkezi güvenlik uyarılarınızı ve önerilerinizi Log Analytics çalışma alanınızda saklar. Ham güvenlik verilerinizi de saklamayı seçebilirsiniz.

Veri depolama için Log Analytics çalışma alanınızı bulmak için:

1. IoT hub'ınızı açın,
1. **Güvenlik**altında, **Genel Bakış'ı**tıklatın ve ardından **Ayarlar'ı**seçin.
1. Log Analytics çalışma alanı yapılandırma ayrıntılarınızı değiştirin.
1. **Kaydet**'e tıklayın.

Yapılandırmayı takiben, Log Analytics çalışma alanınızda depolanan verilere erişmek için aşağıdakileri yapın:

1. IoT Hub'ınızdaki IoT uyarısı için Azure Güvenlik Merkezi'ni seçin ve tıklayın.
1. **Daha fazla araştırma**yı tıklatın.
1. **Bu uyarıyı hangi aygıtlara sahip görmek için buraya tıklayın ve DeviceId sütununa bakın.**

## <a name="investigation-steps-for-suspicious-iot-devices"></a>Şüpheli IoT aygıtları için araştırma adımları

IoT aygıtlarınız hakkındaki istatistikleri ve ham verileri görüntülemek [için, verilerinize erişmek için](#how-can-i-access-my-data)Log Analytics çalışma alanınıza gidin.

Cihazınızdaki uyarıları ve etkinlikleri araştırmaya başlamak için aşağıdaki örnek kql sorgularına bakın.

### <a name="related-alerts"></a>İlgili uyarılar

Aynı anda başka uyarıların tetiklenip tetiklenmedi ğinde aşağıdaki kql sorgusunu kullanın:

  ```
  let device = "YOUR_DEVICE_ID";
  let hub = "YOUR_HUB_NAME";
  SecurityAlert
  | where ExtendedProperties contains device and ResourceId contains tolower(hub)
  | project TimeGenerated, AlertName, AlertSeverity, Description, ExtendedProperties
  ```

### <a name="users-with-access"></a>Erişimi olan kullanıcılar

Bu cihaza hangi kullanıcıların erişebildiği hakkında aşağıdaki kql sorgusunu kullanın:

 ```
  let device = "YOUR_DEVICE_ID";
  let hub = "YOUR_HUB_NAME";
  SecurityIoTRawEvent
  | where
     DeviceId == device and AssociatedResourceId contains tolower(hub)
     and RawEventName == "LocalUsers"
  | project
     TimestampLocal=extractjson("$.TimestampLocal", EventDetails, typeof(datetime)),
     GroupNames=extractjson("$.GroupNames", EventDetails, typeof(string)),
     UserName=extractjson("$.UserName", EventDetails, typeof(string))
  | summarize FirstObserved=min(TimestampLocal) by GroupNames, UserName
 ```
Keşfetmek için bu verileri kullanın:

- Aygıta hangi kullanıcılar erişebilir?
- Erişimi olan kullanıcılar beklenen izin düzeylerine sahip mi?

### <a name="open-ports"></a>Bağlantı noktalarını açma

Aygıttaki hangi bağlantı noktalarının şu anda kullanıldığını veya kullanıldığını öğrenmek için aşağıdaki kql sorgusunu kullanın:

 ```
  let device = "YOUR_DEVICE_ID";
  let hub = "YOUR_HUB_NAME";
  SecurityIoTRawEvent
  | where
     DeviceId == device and AssociatedResourceId contains tolower(hub)
     and RawEventName == "ListeningPorts"
     and extractjson("$.LocalPort", EventDetails, typeof(int)) <= 1024 // avoid short-lived TCP ports (Ephemeral)
  | project
     TimestampLocal=extractjson("$.TimestampLocal", EventDetails, typeof(datetime)),
     Protocol=extractjson("$.Protocol", EventDetails, typeof(string)),
     LocalAddress=extractjson("$.LocalAddress", EventDetails, typeof(string)),
     LocalPort=extractjson("$.LocalPort", EventDetails, typeof(int)),
     RemoteAddress=extractjson("$.RemoteAddress", EventDetails, typeof(string)),
     RemotePort=extractjson("$.RemotePort", EventDetails, typeof(string))
  | summarize MinObservedTime=min(TimestampLocal), MaxObservedTime=max(TimestampLocal), AllowedRemoteIPAddress=makeset(RemoteAddress), AllowedRemotePort=makeset(RemotePort) by Protocol, LocalPort
 ```

Keşfetmek için bu verileri kullanın:

- Cihazda şu anda hangi dinleme soketleri etkindir?
- Şu anda etkin olan dinleme yuvalarına izin verilmeli mi?
- Cihaza bağlı şüpheli uzak adresler var mı?

### <a name="user-logins"></a>Kullanıcı girişleri

Aygıta giriş yapan kullanıcıları bulmak için aşağıdaki kql sorgusunu kullanın:

 ```
  let device = "YOUR_DEVICE_ID";
  let hub = "YOUR_HUB_NAME";
  SecurityIoTRawEvent
  | where
     DeviceId == device and AssociatedResourceId contains tolower(hub)
     and RawEventName == "Login"
     // filter out local, invalid and failed logins
     and EventDetails contains "RemoteAddress"
     and EventDetails !contains '"RemoteAddress":"127.0.0.1"'
     and EventDetails !contains '"UserName":"(invalid user)"'
     and EventDetails !contains '"UserName":"(unknown user)"'
     //and EventDetails !contains '"Result":"Fail"'
  | project
     TimestampLocal=extractjson("$.TimestampLocal", EventDetails, typeof(datetime)),
     UserName=extractjson("$.UserName", EventDetails, typeof(string)),
     LoginHandler=extractjson("$.Executable", EventDetails, typeof(string)),
     RemoteAddress=extractjson("$.RemoteAddress", EventDetails, typeof(string)),
     Result=extractjson("$.Result", EventDetails, typeof(string))
  | summarize CntLoginAttempts=count(), MinObservedTime=min(TimestampLocal), MaxObservedTime=max(TimestampLocal), CntIPAddress=dcount(RemoteAddress), IPAddress=makeset(RemoteAddress) by UserName, Result, LoginHandler
 ```

Keşfetmek için sorgu sonuçlarını kullanın:

- Aygıtta hangi kullanıcılar oturum açtı?
- Oturum açan, oturum açması gereken kullanıcılar mı?
- Oturum açan kullanıcılar beklenen veya beklenmeyen IP adreslerinden bağlandı mı?

### <a name="process-list"></a>İşlem listesi

İşlem listesinin beklendiği gibi olup olmadığını öğrenmek için aşağıdaki kql sorgusunu kullanın:

 ```
  let device = "YOUR_DEVICE_ID";
  let hub = "YOUR_HUB_NAME";
  SecurityIoTRawEvent
  | where
     DeviceId == device and AssociatedResourceId contains tolower(hub)
     and RawEventName == "ProcessCreate"
  | project
     TimestampLocal=extractjson("$.TimestampLocal", EventDetails, typeof(datetime)),
     Executable=extractjson("$.Executable", EventDetails, typeof(string)),
     UserId=extractjson("$.UserId", EventDetails, typeof(string)),
     CommandLine=extractjson("$.CommandLine", EventDetails, typeof(string))
  | join kind=leftouter (
     // user UserId details
     SecurityIoTRawEvent
     | where
        DeviceId == device and AssociatedResourceId contains tolower(hub)
        and RawEventName == "LocalUsers"
     | project
        UserId=extractjson("$.UserId", EventDetails, typeof(string)),
        UserName=extractjson("$.UserName", EventDetails, typeof(string))
     | distinct UserId, UserName
  ) on UserId
  | extend UserIdName = strcat("Id:", UserId, ", Name:", UserName)
  | summarize CntExecutions=count(), MinObservedTime=min(TimestampLocal), MaxObservedTime=max(TimestampLocal), ExecutingUsers=makeset(UserIdName), ExecutionCommandLines=makeset(CommandLine) by Executable
```

Keşfetmek için sorgu sonuçlarını kullanın:

- Cihazda şüpheli bir işlem var mıydı?
- İşlemler uygun kullanıcılar tarafından yürütüldü mü?
- Herhangi bir komut satırı yürütmedoğru ve beklenen bağımsız değişkenleri içeriyor muydu?

## <a name="next-steps"></a>Sonraki adımlar

Bir aygıtı araştırdıktan ve risklerinizi daha iyi anladıktan sonra, IoT çözüm güvenlik duruşunuzu iyileştirmek için [özel uyarıları yapılandırmayı](quickstart-create-custom-alerts.md) düşünebilirsiniz. Zaten bir aygıt aracınız yoksa, sonuçlarınızı iyileştirmek için [bir güvenlik aracısı dağıtmayı](how-to-deploy-agent.md) veya [varolan bir aygıt aracısının yapılandırmasını değiştirmeyi](how-to-agent-configuration.md) düşünün.
