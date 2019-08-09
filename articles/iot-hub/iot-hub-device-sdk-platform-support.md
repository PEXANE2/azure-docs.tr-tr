---
title: Azure IoT cihaz SDK 'Ları platform desteği | Microsoft Docs
description: Kavramlar-Azure IoT cihaz SDK 'Ları tarafından desteklenen platformların listesi
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: robinsh
ms.openlocfilehash: 356962c76bf942c56ace0798ac3cca3d815bc507
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68883114"
---
# <a name="azure-iot-sdks-platform-support"></a>Azure IoT SDK 'Ları platformu desteği

[Azure IoT SDK 'ları](iot-hub-devguide-sdks.md) , büyük dil ve platform desteğiyle IoT Hub ve cihaz sağlama hizmeti ile etkileşime geçmek için bir kitaplık kümesidir. SDK 'lar, en yaygın platformlarda çalışır ve geliştiriciler, [taşıma kılavuzunu](https://github.com/Azure/azure-c-shared-utility/blob/master/devdoc/porting_guide.md)izleyerek C SDK 'sının belirli bir platforma bağlantı noktası oluşturabilir. 

Microsoft çeşitli işletim sistemlerini/platformları/çerçeveleri destekler ve Azure IoT C SDK 'Sı kullanılarak genişletilebilir. Bazıları, kullanıcılar tarafından, kullanıcıların beklediği destek düzeyini temsil eden katmanlar halinde gruplanmış olarak desteklenir. *Tam olarak desteklenen platformlar* , Microsoft 'un şu anlamına gelir:

- Ana ve desteklenen sürüm (ler) e karşı uçtan uca testleri sürekli olarak oluşturup çalıştırır.  Farklı sürümler arasında test kapsamı sağlamak için genellikle en son LTS sürümüne ve en popüler sürüme karşı test ediyoruz.  Aynı platformun diğer sürümleri platform sürümü uyumluluğu aracılığıyla desteklenebilir.
- Uygulanabilirse Yükleme Kılavuzu veya paketleri sağlar.
- GitHub 'daki platformları tam olarak destekler.

Ayrıca, iş ortaklarının bir listesi, C SDK 'imizi daha fazla platforma ve platform soyutlama katmanını (PAL) sürdürmesidir. [IoT Için Azure Sertifikalı cihaz kataloğu](https://catalog.azureiotsolutions.com/) , farklı SDK 'ların test edildiğini de bir işletim sistemi platformları listesini de sunar. SDK 'lar Ayrıca, sınırlı test ve destek ile bu platformlarda düzenli olarak derleme yapmak için:

* MBED2
* Arduino
* Windows CE 2013 (2018 Ekim 'de kullanımdan kalkın)
* .NET Core 2,1 ve .NET Framework 4,7 .NET Standard 1,3
* Xamarin iOS, Android, UWP

## <a name="supported-platforms"></a>Desteklenen platformlar

Desteklenen birkaç platform vardır.

### <a name="c-sdk"></a>C SDK

| OS                  | Mimari | Derleyici             | TLS kitaplığı       |
|---------------------|------|----------------------|-------------------|
| Ubuntu 16.04 LTS    | X64  | GCC-5.4.0            | OpenSSL-1.0.2 g |
| Ubuntu 18,04 LTS    | X64  | GCC-7,3              | WolfSSL – 1,13    |
| Ubuntu 18,04 LTS    | X64  | Clang 6.0. X          | OpenSSL – 1.1.0 g  |
| OSX 10.13.4         | x64  | XCode 9.4.1          | Yerel OSX        |
| Windows Server 2016 | x64  | Visual Studio 14.0. X | SChannel          |
| Windows Server 2016 | x86  | Visual Studio 14.0. X | SChannel          |
| 9 uzat    | x64  | GCC-7,3              | OpenSSL – 1.1.0 f  |

### <a name="python-sdk"></a>Python SDK'sı

| OS                  | Mimari | Derleyici   | TLS kitaplığı |
|---------------------|------|------------|-------------|
| Windows Server 2016 | x86  | Python 2.7 | OpenSSL     |
| Windows Server 2016 | x64  | Python 2.7 | OpenSSL     |
| Windows Server 2016 | x86  | Python 3,5 | OpenSSL     |
| Windows Server 2016 | x64  | Python 3,5 | OpenSSL     |
| Ubuntu 18,04 LTS    | x86  | Python 2.7 | OpenSSL     |
| Ubuntu 18,04 LTS    | x86  | Python 3,4 | OpenSSL     |
| MacOS High Sierra   | x64  | Python 2.7 | OpenSSL     |

### <a name="net-sdk"></a>.NET SDK

| OS                  | Mimari | Çerçevenin            | Standart          |
|---------------------|------|----------------------|-------------------|
| Ubuntu 16.04 LTS    | X64  | .NET Core 2.1        | .NET Standard 2,0 |
| Windows Server 2016 | X64  | .NET Core 2.1        | .NET Standard 2,0 |
| Windows Server 2016 | X64  | .NET framework 4.7   | .NET Standard 2,0 |
| Windows Server 2016 | X64  | .NET Framework 4.5.1 | Yok               |

### <a name="nodejs-sdk"></a>Node.js SDK'sı

| OS                                           | Mimari | Düğüm sürümü |
|----------------------------------------------|------|--------------|
| Ubuntu 16,04 LTS (node 6 Docker görüntüsünü kullanarak) | X64  | Düğüm 6       |
| Windows Server 2016                          | X64  | Düğüm 6       |

### <a name="java-sdk"></a>Java SDK

| OS                  | Mimari | Java sürümü |
|---------------------|------|--------------|
| Ubuntu 16.04 LTS    | X64  | Java 8       |
| Windows Server 2016 | X64  | Java 8       |
| Android API 28 | X64  | Java 8       |
| Android öğeleri | X64  | Java 8      |

## <a name="partner-supported-platforms"></a>İş ortağı tarafından desteklenen platformlar

Müşteriler, Azure IoT C SDK 'sını, özellikle de SDK 'nın platform soyutlama katmanını (PAL) oluşturarak, platform destekimizi genişletebilirler. Microsoft, genişletilmiş destek sağlamak için iş ortaklarıyla birlikte çalışır. İş ortaklarının bir listesi, daha fazla platforma ve PAL 'i sürdürmek için C SDK 'Sı üzerinde yer alır.

| Partner             | Cihazlar                            | Bağlantı                     | Destek |
|---------------------|------------------------------------|--------------------------|---------|
| Espressıo           | ESP32 <br/> ESP8266                              | [ESP-Azure](https://github.com/espressif/esp-azure)                | [GitHub](https://github.com/espressif/esp-azure)  
| Qualcomm            | Qualcomm MDM9206 LTE IoT modem     | [IoT SDK için Qualcomm LTE](https://developer.qualcomm.com/software/lte-iot-sdk) | [Forum](https://developer.qualcomm.com/forums/software/lte-iot-sdk)   |
| ST mikro elektronik | STM32L4 serisi <br/> STM32F4 serisi <br/>  STM32F7 serisi <br/>  IoT düğümü için STM32L4 Discovery Kit    | [X-KÜP-BULUT](https://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32cube-expansion-packages/x-cube-cloud.html) <br/> [X-KÜP-AZURE](https://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32cube-expansion-packages/x-cube-azure.html) <br/> [P-NUCLEO-AZURE](https://www.st.com/content/st_com/en/products/evaluation-tools/solution-evaluation-tools/communication-and-connectivity-solution-eval-boards/p-nucleo-azure1.html) <br/> [FP-CLD-AZURE](https://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32-ode-function-pack-sw/fp-cld-azure1.html)            | [Destek](https://www.st.com/content/st_com/en/support/support-home.html)
| Texas Instruments   | CC3220SF Launchpad <br/> CC3220S Launchpad <br/> MSP432E4 Launchpad      | [SimpleLink için Azure IoT eklentisi](https://github.com/TexasInstruments/azure-iot-pal-simplelink) | [TI E2E Forumu](https://e2e.ti.com) <br/> [CC3220 için tı E2E Forumu](https://e2e.ti.com/support/wireless_connectivity/simplelink_wifi_cc31xx_cc32xx/) <br/> [MSP432E4 için tı E2E Forumu](https://e2e.ti.com/support/microcontrollers/msp430/) |

## <a name="next-steps"></a>Sonraki adımlar

* [Cihaz ve hizmet SDK’ları](iot-hub-devguide-sdks.md)
* [Taşıma Kılavuzu](https://github.com/Azure/azure-c-shared-utility/blob/master/devdoc/porting_guide.md)
