---
title: IoT için Azure Güvenlik Merkezi için kimlik doğrulama yöntemleri| Microsoft Dokümanlar
description: IoT için Azure Güvenlik Merkezi hizmetini kullanırken kullanılabilen farklı kimlik doğrulama yöntemleri hakkında bilgi edinin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 10b38f20-b755-48cc-8a88-69828c17a108
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: 16f7f91e02d118d9f9a295ebb79a6cd0187dd9fd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68596461"
---
# <a name="security-agent-authentication-methods"></a>Güvenlik aracısı kimlik doğrulama yöntemleri 

Bu makalede, IoT Hub ile kimlik doğrulaması yapmak için AzureIoTSecurity aracısıyla kullanabileceğiniz farklı kimlik doğrulama yöntemleri açıklanmaktadır.

IoT Hub'daki Azure Güvenlik Merkezi IoT için binen her cihaz için bir güvenlik modülü gereklidir. Aygıtın kimliğini doğrulamak için Azure Güvenlik Merkezi ioT için iki yöntemden birini kullanabilir. Mevcut IoT çözümünüz için en uygun yöntemi seçin. 

> [!div class="checklist"]
> * SecurityModule seçeneği
> * Cihaz seçeneği

## <a name="authentication-methods"></a>Kimlik doğrulama yöntemleri

AzureIoTSecurity aracısının kimlik doğrulaması gerçekleştirmesi için iki yöntem:

 - **SecurityModule** kimlik doğrulama modu<br>
   Aracı, aygıt kimliğinden bağımsız olarak güvenlik modülü kimliği kullanılarak kimlik doğrulanır.
   Güvenlik aracısının güvenlik modülü (yalnızca simetrik anahtar) aracılığıyla özel bir kimlik doğrulama yöntemi kullanmasını istiyorsanız, bu kimlik doğrulama türünü kullanın.
        
 - **Aygıt** kimlik doğrulama modu<br>
    Bu yöntemde, güvenlik aracısı önce aygıt kimliğiyle kimlik doğrular. İlk kimlik doğrulamadan sonra, IoT aracısı için Azure Güvenlik Merkezi, aygıtın kimlik doğrulama verilerini içeren REST API'sini kullanarak IoT Hub'ına **REST** çağrısı gerçekleştirir. Azure Güvenlik Merkezi IoT aracısı için daha sonra güvenlik modülü kimlik doğrulama yöntemini ve verilerini IoT Hub'ından ister. Son adımda, Azure Güvenlik Merkezi IoT aracısı için Azure Güvenlik Merkezi, IoT modülü için Azure Güvenlik Merkezi'ne karşı bir kimlik doğrulaması gerçekleştirir.
    
    Güvenlik aracısının varolan bir aygıt kimlik doğrulama yöntemini (kendi imzalı sertifika veya simetrik anahtar) yeniden kullanmasını istiyorsanız, bu kimlik doğrulama türünü kullanın. 

Yapılandırmayı öğrenmek için [Güvenlik aracısı yükleme parametrelerine](#security-agent-installation-parameters) bakın.
                                
## <a name="authentication-methods-known-limitations"></a>Bilinen sınırlamaları doğrulama yöntemleri

- **SecurityModule** kimlik doğrulama modu yalnızca simetrik anahtar kimlik doğrulamasını destekler.
- CA Imzalı sertifika **Aygıt** kimlik doğrulama modu tarafından desteklenmez.  

## <a name="security-agent-installation-parameters"></a>Güvenlik aracısı yükleme parametreleri

[Bir güvenlik aracısı dağıtılırken,](how-to-deploy-agent.md)kimlik doğrulama ayrıntıları bağımsız değişken olarak sağlanmalıdır.
Bu bağımsız değişkenler aşağıdaki tabloda belgelenmiştir.


|Linux Parametre Adı | Windows Parametre Adı | Steno Parametresi |Açıklama|Seçenekler|
|---------------------|---------------|---------|---------------|---------------|
|kimlik doğrulama-kimlik|Kimlik DoğrulamaKimliği|Auı|Kimlik doğrulama kimliği| **Güvenlik Modülü** veya **Cihaz**|
|kimlik doğrulama yöntemi|Authenticationmethod|Aum|Kimlik doğrulama Yöntemi|**SymmetricKey** veya **SelfSignedCertificate**|
|dosya yolu|Filepath|f|Sertifika veya simetrik anahtarı içeren dosya için mutlak tam yol| |
|ana bilgisayar adı|Ana bilgisayar adı|Hn|FQDN of the IoT Hub|Örnek: ContosoIotHub.azure-devices.net|
|cihaz kimliği|DeviceId|Di|Cihaz Kimliği|Örnek: MyDevice1|
|sertifika-konum türü|SertifikaKonum Türü|Cl|Sertifika depolama konumu|**LocalFile** veya **Store**|
|


Yükle güvenlik aracısı komut dosyası kullanılırken, aşağıdaki yapılandırma otomatik olarak gerçekleştirilir. Güvenlik aracısının kimlik doğrulamasını el ile demi etmek için config dosyasını edin. 

## <a name="change-authentication-method-after-deployment"></a>Dağıtımdan sonra kimlik doğrulama yöntemini değiştirme

Yükleme komut dosyası olan bir güvenlik aracısı dağıtılırken, yapılandırma dosyası otomatik olarak oluşturulur.

Dağıtımdan sonra kimlik doğrulama yöntemlerini değiştirmek için yapılandırma dosyasının el ile düzenlenmesi gerekir.


### <a name="c-based-security-agent"></a>C# tabanlı güvenlik aracısı

Aşağıdaki parametrelerle _Authentication.config'i_ edin:

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

### <a name="c-based-security-agent"></a>C tabanlı güvenlik aracısı

_LocalConfiguration.json'u_ aşağıdaki parametrelerle düzenleme:

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
- [Güvenlik ajanlarına genel bakış](security-agent-architecture.md)
- [Güvenlik aracısı dağıtma](how-to-deploy-agent.md)
- [Ham güvenlik verilerine erişme](how-to-security-data-access.md)
