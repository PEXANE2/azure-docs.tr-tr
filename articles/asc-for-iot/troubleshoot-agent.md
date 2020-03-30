---
title: IoT Linux güvenlik aracısı sorun giderme kılavuzu için Azure Güvenlik Merkezi| Microsoft Dokümanlar
description: Linux için IoT güvenlik aracıları için Azure Güvenlik Merkezi ile çalışma sorun giderme.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 55c8d3b6-3126-4246-8d07-ef88fe5ea84f
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2019
ms.author: mlottner
ms.openlocfilehash: 7f3bd4be3ef927f73643146a457bc551ef86a450
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68600572"
---
# <a name="security-agent-troubleshoot-guide-linux"></a>Güvenlik aracısı sorun giderme kılavuzu (Linux)

Bu makalede, güvenlik aracısı başlatma işleminde olası sorunların nasıl çözüleceğini açıklar.

IoT aracısı için Azure Güvenlik Merkezi, yüklemeden hemen sonra kendi kendine başlar. Aracı başlatma işlemi yerel yapılandırmayı okumayı, Azure IoT Hub'ına bağlanmayı ve uzak ikiz yapılandırmasını almaişlemini içerir. Bu adımlardan herhangi birinde başarısız olmak güvenlik aracısının başarısız olmasıyla ilgili olabilir.

Bu sorun giderme kılavuzunda şunları öğreneceksiniz:
> [!div class="checklist"]
> * Güvenlik aracısı çalışıyorsa doğrulama
> * Güvenlik aracısı hatalarını alma
> * Güvenlik aracısı hatalarını anlama ve düzeltme 

## <a name="validate-if-the-security-agent-is-running"></a>Güvenlik aracısı çalışıyorsa doğrulama

1. Doğrulamak için güvenlik aracısı çalışıyor, aracıyı yükledikten sonra birkaç dakika bekleyin ve aşağıdaki komutu çalıştırın. 
     <br>

    **C ajanı**
    ```bash
    grep "ASC for IoT Agent initialized" /var/log/syslog
    ```
    **C# aracısı**
    ```bash
    grep "Agent is initialized!" /var/log/syslog
    ```
2. Komut boş bir satır döndürürse, güvenlik aracısı başarıyla başedemedi.    

## <a name="force-stop-the-security-agent"></a>Güvenlik ajanını durdurmaya zorlama 
Güvenlik aracısının başlatamadığı durumlarda, aşağıdaki komutla aracıyı durdurun ve aşağıdaki hata tablosuna devam edin:

```bash
systemctl stop ASCIoTAgent.service
```
## <a name="get-security-agent-errors"></a>Güvenlik aracısı hatalarını alma
1. Aşağıdaki komutu çalıştırarak güvenlik aracısı hata(ları) alın:
    ```bash
    grep ASCIoTAgent /var/log/syslog
    ```
2. Güvenlik aracısı al komutu, Azure Güvenlik Merkezi tarafından oluşturulan tüm günlükleri IoT aracısı için alır. Hataları anlamak ve düzeltme için doğru adımları atmak için aşağıdaki tabloyu kullanın. 

> [!Note]
> Hata günlükleri kronolojik sırada gösterilir. Düzeltmenize yardımcı olmak için her hatanın zaman damgasını not edin. 

## <a name="restart-the-agent"></a>Aracıyı yeniden başlatın

1. Bir güvenlik aracısı hatasını bulup düzelttikten sonra, aşağıdaki komutu çalıştırarak aracıyı yeniden başlatmayı deneyin. 
    ```bash
    systemctl restart ASCIoTAgent.service
    ```
1. Aracı başlangıç işleminde başarısız olmaya devam ederse, durdurma yı almak ve hataları almak için önceki işlemi yineleyin. 

## <a name="understand-security-agent-errors"></a>Güvenlik aracısı hatalarını anlama

Güvenlik aracısı hatalarının çoğu aşağıdaki biçimde görüntülenir: 
```
Azure Security Center for IoT agent encountered an error! Error in: {Error Code}, reason: {Error sub code}, extra details: {error specific details}
```
| Hata Kodu | Hata alt kodu | Hata ayrıntıları | İyileştirici C | İyileştirici C # |
|:-----------|:---------------|:--------|:------------|:------------|
| Yerel Yapılandırma | Eksik yapılandırma | Yerel yapılandırma dosyasında bir yapılandırma eksik. Hata iletisi hangi anahtarın eksik olduğunu belirtmelidir. | /var/LocalConfiguration.json dosyasına eksik anahtarı ekleyin, ayrıntılar için [cs-localconfig-reference'a](azure-iot-security-local-configuration-c.md) bakın.| General.config dosyasına eksik anahtarı ekleyin, ayrıntılar için [c#-localconfig-reference'a](azure-iot-security-local-configuration-csharp.md) bakın. |
| Yerel Yapılandırma | Cant Parse Yapılandırma | Yapılandırma değeri ayrıştısı olamaz. Hata iletisi hangi anahtarın ayrışdırılamaması gerektiğini belirtmelidir. Değer beklenen türde olmadığından veya değer aralık dışında olduğundan yapılandırma değeri ayrıştırılamaz. | /var/LocalConfiguration.json dosyasındaki anahtarın değerini yerel Yapılandırma şemasıyla eşleşecek şekilde düzeltin, ayrıntılar için [c#-localconfig-reference'a](azure-iot-security-local-configuration-csharp.md) bakın. |  General.config dosyasındaki anahtarın değerini şemayla eşleşecek şekilde düzeltin, ayrıntılar için [cs-localconfig-reference'a](azure-iot-security-local-configuration-c.md) bakın.|
| Yerel Yapılandırma | Dosya Biçimi | Yapılandırma dosyasını ayrışdırmak için başarısız oldu. | Yapılandırma dosyası bozuk, aracıyı karşıdan yükleyin ve yeniden yükleyin. | |
| Uzaktan Yapılandırma | Zaman aşımı | Aracı, zaman zaman dilimi içinde azureiotsecurity modülü ikizini getiremedi. | Kimlik doğrulama yapılandırmanın doğru olduğundan emin olun ve yeniden deneyin. | Aracı, azureiotsecurity modülü ikizini zaman araları içinde getiremedi. | Kimlik doğrulama yapılandırmanın doğru olduğundan emin olun ve yeniden deneyin. |
| Kimlik doğrulaması | Dosya Yok | Verilen yoldaki dosya yok. | Dosyanın verilen yolda bulunduğundan emin olun veya **LocalConfiguration.json** dosyasına gidin ve **FilePath** yapılandırmasını değiştirin. | Dosyanın verilen yolda bulunduğundan emin olun veya **Authentication.config** dosyasına gidin ve **filePath** yapılandırmasını değiştirin.|
| Kimlik doğrulaması | Dosya İzni | Aracının dosyayı açmak için yeterli izinleri yok. | **Asciotagent** kullanıcısına verilen yolda dosyadaki okuma izinlerini verin. | Dosyaya erişilebilir olduğundan emin olun. |
| Kimlik doğrulaması | Dosya Biçimi | Verilen dosya doğru biçimde değil. | Dosyanın doğru biçimde olduğundan emin olun. Desteklenen dosya türleri .pfx ve .pem'dir. | Dosyanın geçerli bir sertifika dosyası olduğundan emin olun. |
| Kimlik doğrulaması | Yetkisiz | Aracı, verilen kimlik bilgileriyle IoT Hub'a karşı kimlik doğrulaması yapamazdı. | Yerel Yapılandırma dosyasında kimlik doğrulama yapılandırmasını doğrulayın, kimlik doğrulama yapılandırmasını gözden geçirin ve tüm ayrıntıların doğru olduğundan emin olun, dosyadaki sırrın kimlik doğrulamasıyla eşleştiğini doğrulayın. | Kimlik doğrulama yapılandırmasını Authentication.config'de doğrulayın, kimlik doğrulama yapılandırmasını gözden geçirin ve tüm ayrıntıların doğru olduğundan emin olun, ardından dosyadaki sırrın kimlik doğrulamasıyla eşleştiğini doğrulayın.
| Kimlik doğrulaması | Bulunamadı | Cihaz / modül bulundu. | Kimlik doğrulama yapılandırmasını doğrulayın - ana bilgisayar adının doğru olduğundan, aygıtın IoT Hub'Da olduğundan ve azureiotsecurity ikiz modülüne sahip olduğundan emin olun. |  Kimlik doğrulama yapılandırmasını doğrulayın - ana bilgisayar adının doğru olduğundan, aygıtın IoT Hub'Da olduğundan ve azureiotsecurity ikiz modülüne sahip olduğundan emin olun. |
| Kimlik doğrulaması | Eksik Yapılandırma | *Authentication.config* dosyasında bir yapılandırma eksik. Hata iletisi hangi anahtarın eksik olduğunu belirtmelidir. | *LocalConfiguration.json* dosyasına eksik anahtarı ekleyin.| *Authentication.config* dosyasına eksik anahtarı ekleyin, ayrıntılar için [c#-localconfig-reference'a](azure-iot-security-local-configuration-csharp.md) bakın. |
| Kimlik doğrulaması | Cant Parse Yapılandırma | Yapılandırma değeri ayrıştısı olamaz. Hata iletisi hangi anahtarın ayrışdırılamaması gerektiğini belirtmelidir. Değer beklenen türde olmadığından veya değer aralık dışında olduğundan yapılandırma değeri ayrıştırılamaz. |**LocalConfiguration.json** dosyasındaki anahtarın değerini düzeltin. |Şemaya uyacak şekilde **Authentication.config** dosyasındaki anahtarın değerini düzeltin, ayrıntılar için [cs-localconfig-reference'a](azure-iot-security-local-configuration-c.md) bakın.|
|

## <a name="restart-the-agent"></a>Aracıyı yeniden başlatın
1. Bir güvenlik aracısı hatasını bulup düzelttikten sonra, aşağıdaki komutu çalıştırarak aracıyı yeniden başlatın:

    ```bash
    systemctl restart ASCIoTAgent.service
    ```
2. Gerekirse, aracıyı durdurmaya zorlamak için önceki işlemleri tekrarlayın ve aracı başlangıç işleminde başarısız olmaya devam ederse hataları alın. 

## <a name="next-steps"></a>Sonraki adımlar
- IoT hizmetine [Genel Bakış](overview.md) için Azure Güvenlik Merkezi'ni okuyun
- IoT [Mimarisi](architecture.md) için Azure Güvenlik Merkezi hakkında daha fazla bilgi edinin
- IoT [hizmeti](quickstart-onboard-iot-hub.md) için Azure Güvenlik Merkezi'ni etkinleştirme
- IoT hizmeti [SSS](resources-frequently-asked-questions.md) için Azure Güvenlik Merkezi'ni okuyun
- Ham güvenlik [verilerine](how-to-security-data-access.md) nasıl erişin gerektiğini öğrenin
- [Önerileri](concept-recommendations.md) anlama
- Güvenlik [uyarılarını](concept-security-alerts.md) anlama
