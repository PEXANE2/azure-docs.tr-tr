---
title: Güvenlik Aracısı kimlik doğrulama yöntemleri
description: IoT hizmeti için Defender kullanırken kullanılabilecek farklı kimlik doğrulama yöntemleri hakkında bilgi edinin.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/09/2020
ms.author: mlottner
ms.openlocfilehash: 896baa70fb6e18165a025459a063e33d705261ba
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90941707"
---
# <a name="security-agent-authentication-methods"></a>Güvenlik Aracısı kimlik doğrulama yöntemleri

Bu makalede, IoT Hub kimlik doğrulaması yapmak için AzureIoTSecurity aracısında kullanabileceğiniz farklı kimlik doğrulama yöntemleri açıklanmaktadır.

IoT Hub IoT için her cihaz eklendi için Defender, bir güvenlik modülü gereklidir. Cihazın kimliğini doğrulamak için, IoT için Defender iki yöntemden birini kullanabilir. Mevcut IoT çözümünüz için en iyi şekilde çalışacak yöntemi seçin.

> [!div class="checklist"]
> * SecurityModule seçeneği
> * Cihaz seçeneği

## <a name="authentication-methods"></a>Kimlik doğrulama yöntemleri

AzureIoTSecurity aracısının kimlik doğrulaması gerçekleştirmesi için iki yöntemi:

- **Securitymodule** kimlik doğrulama modu<br>
Aracının kimliği, cihaz kimliğinden bağımsız olarak güvenlik modülü kimliği kullanılarak doğrulanır.
Güvenlik aracısının güvenlik modülü aracılığıyla adanmış bir kimlik doğrulama yöntemi kullanmasını istiyorsanız bu kimlik doğrulaması türünü kullanın (yalnızca simetrik anahtar).

- **Cihaz** kimlik doğrulama modu<br>
Bu yöntemde, güvenlik aracısının ilk olarak cihaz kimliğiyle kimliğini doğrular. İlk kimlik doğrulamasından sonra, IoT Aracısı için Defender, cihazın kimlik doğrulama verileriyle REST API kullanarak IoT Hub bir **rest** çağrısı gerçekleştirir. IoT Aracısı için Defender, IoT Hub güvenlik modülü kimlik doğrulama yöntemini ve verileri ister. Son adımda, IoT Aracısı için Defender, bir kimlik doğrulamasını için Defender for IoT modülüne uygular.

Güvenlik aracısının mevcut bir cihaz kimlik doğrulama yöntemini (otomatik olarak imzalanan sertifika veya simetrik anahtar) yeniden kullanmasını istiyorsanız bu kimlik doğrulaması türünü kullanın.

' Nin nasıl yapılandırılacağını öğrenmek için bkz. [Güvenlik Aracısı yükleme parametreleri](#security-agent-installation-parameters) .

## <a name="authentication-methods-known-limitations"></a>Kimlik doğrulama yöntemleri bilinen sınırlamalar

- **Securitymodule** kimlik doğrulama modu yalnızca simetrik anahtar kimlik doğrulamasını destekler.
- CA-Signed sertifikası **cihaz** kimlik doğrulama modu tarafından desteklenmiyor.

## <a name="security-agent-installation-parameters"></a>Güvenlik Aracısı yükleme parametreleri

[Bir güvenlik Aracısı dağıtıldığında](how-to-deploy-agent.md), kimlik doğrulaması ayrıntıları bağımsız değişken olarak sağlanmalıdır.
Bu bağımsız değişkenler aşağıdaki tabloda belgelenmiştir.

|Linux parametre adı | Windows parametre adı | Toplu parametre |Açıklama|Seçenekler|
|---------------------|---------------|---------|---------------|---------------|
|kimlik doğrulama-kimlik|Authenticationıdentity|AUI|Kimlik doğrulama kimliği| **Securitymodule** veya **cihaz**|
|Authentication-yöntemi|AuthenticationMethod|AUD|Kimlik doğrulama yöntemi|**SymmetricKey** veya **Selfsignedcertificate**|
|dosya yolu|Null|f|Sertifikayı veya simetrik anahtarı içeren dosyanın mutlak tam yolu| |
|konak adı|HostName|hn|IoT Hub FQDN 'SI|Örnek: ContosoIotHub.azure-devices.net|
|cihaz kimliği|DeviceId|içerik|Cihaz Kimliği|Örnek: MyDevice1|
|Sertifika-konum-tür|CertificateLocationKind|CL|Sertifika depolama konumu|**YerelDosya** veya **Mağaza**|
|

Güvenlik aracısının yüklenmesi betiği kullanılırken, aşağıdaki yapılandırma otomatik olarak gerçekleştirilir. Güvenlik Aracısı kimlik doğrulamasını el ile düzenlemek için yapılandırma dosyasını düzenleyin.

## <a name="change-authentication-method-after-deployment"></a>Dağıtımdan sonra kimlik doğrulama yöntemini değiştir

Bir güvenlik Aracısı yükleme betiği ile dağıtıldığında, bir yapılandırma dosyası otomatik olarak oluşturulur.

Dağıtımdan sonra kimlik doğrulama yöntemlerini değiştirmek için yapılandırma dosyasını el ile düzenlemeniz gerekir.

### <a name="c-based-security-agent"></a>C# tabanlı güvenlik Aracısı

_Authentication.config_ aşağıdaki parametrelerle düzenleyin:

```xml
<Authentication>
  <add key="deviceId" value=""/>
  <add key="gatewayHostname" value=""/>
  <add key="filePath" value=""/>
  <add key="type" value=""/>
  <add key="identity" value=""/>
  <add key="certificateLocationKind" value="" />
</Authentication>
```

### <a name="c-based-security-agent"></a>C tabanlı güvenlik Aracısı

Aşağıdaki parametrelerle _LocalConfiguration.js'_ i düzenleyin:

```json
"Authentication" : {
    "Identity" : "",
    "AuthenticationMethod" : "",
    "FilePath" : "",
    "DeviceId" : "",
    "HostName" : ""
}
```

## <a name="see-also"></a>Ayrıca bkz.

- [Güvenlik aracılarına genel bakış](security-agent-architecture.md)
- [Güvenlik aracısını dağıt](how-to-deploy-agent.md)
- [Ham güvenlik verilerine erişme](how-to-security-data-access.md)
