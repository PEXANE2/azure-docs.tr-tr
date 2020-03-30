---
title: Azure IoT Cihazı Platform Desteğini SDK'lar | Microsoft Dokümanlar
description: Açık kaynak kodlu aygıt SDK'ları, aygıtları Azure IoT Hub ve Aygıt Sağlama Hizmeti'ne (DPS) bağlamak için C, .NET (C#), Java, Node.js ve Python'daki GitHub'da kullanılabilir.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/08/2019
ms.author: robinsh
ms.openlocfilehash: 496b890cc49b6b6b9f15213a48472447f801b1c9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76045128"
---
# <a name="azure-iot-device-sdks-platform-support"></a>Azure IoT Cihazı Platform Desteğini SDK'lar

Microsoft, Azure IoT Hub özellikli cihazların evrenini sürekli olarak genişletmeye çalışmaktadır. Microsoft, aygıtları Azure IoT Hub'ına ve Aygıt Sağlama Hizmetine bağlamaya yardımcı olmak için GitHub'da açık kaynak kodlu aygıt SDK'ları yayımlar. SDK'lar C, .NET (C#), Java, Node.js ve Python için kullanılabilir. Microsoft, [Microsoft SDK'lar ve aygıt platformu destek](#microsoft-sdks-and-device-platform-support) bölümünde bunun için ayrıntılı olarak açıklanan desteklenen yapılandırmalar üzerinde çalıştığından emin olmak için her SDK'yı sınar.

Microsoft, aygıt SDK'larına ek olarak, müşterilerin ve geliştiricilerin cihazlarını Azure IoT'ye bağlamalarını sağlamak için başka yollar da sağlar:

* Microsoft, donanım platformları için Azure IoT C SDK'yı temel alan geliştirme kitlerini yayımlamalarına yardımcı olmak için çeşitli ortak şirketlerle işbirliği yapmaktadır.

* Microsoft, Azure IoT için test edilmiş ve sertifikalanmış sürekli genişleyen bir aygıt kümesi sağlamak için Microsoft'un güvendiği iş ortaklarıyla birlikte çalışır. Bu aygıtların geçerli bir listesi [için, IoT aygıt kataloğu için Azure sertifikalı](https://catalog.azureiotsolutions.com/)bakın.

* Microsoft, Azure IoT Hub Aygıt C SDK'da geliştiricilerin SDK'yı platformlarına kolayca taşımalarına yardımcı olan bir platform soyutlama katmanı (PAL) sağlar. Daha fazla bilgi için [C SDK taşıma kılavuzuna](https://github.com/Azure/azure-c-shared-utility/blob/master/devdoc/porting_guide.md)bakın.

Bu konu, Microsoft SDK'ları ve destekledikleri platform yapılandırmaları ve yukarıda listelenen diğer seçenekler hakkında bilgi sağlar.

## <a name="microsoft-sdks-and-device-platform-support"></a>Microsoft SDK'lar ve cihaz platformu desteği

Microsoft, GitHub'da açık kaynak kodlu SDK'ları aşağıdaki diller için yayımlar: C, .NET (C#), Node.js, Java ve Python. SDK'lar ve bunların bağımlılıkları bu bölümde listelenmiştir. SDK'lar, bu bağımlılıkları karşılayan herhangi bir aygıt platformunda desteklenir.

Listelenen SDK'ların her biri için Microsoft:

* Çeşitli popüler platformlarda GitHub'daki ilgili SDK'nın ana dalı na karşı sürekli olarak uçtan uca testler oluşturur ve çalıştırın.  Farklı derleyici sürümlerinde test kapsamı sağlamak için, genellikle en son LTS sürümüne ve en popüler sürüme karşı test ediyoruz.

* Varsa kurulum kılavuzu veya kurulum paketleri sağlar.

* GitHub'daki SDK'ları açık kaynak kodu, müşteri katkıları için bir yol ve GitHub sorunlarıyla ürün ekibi etkileşimi ile tam olarak destekler.

### <a name="c-sdk"></a>C SDK

[Azure IoT Hub C cihazı SDK](https://github.com/Azure/azure-iot-sdk-c) aşağıdaki yapılandırmalarla test edilir ve destekler.

| İşletim Sistemi                  | TLS kitaplığı                  | Ek gereksinimler                                                                     |
|---------------------|------------------------------|---------------------------------------------------------------------------------------------|
| Linux               | OpenSSL, WolfSSL veya BearSSL | Berkeley soketleri</br></br>Taşınabilir İşletim Sistemi Arabirimi (POSIX)                       |
| iOS 12.2            | OpenSSL                      | OSX 10.13.4'te taklit edilmiş XCode                                                               |
| Windows 10 ailesi   | Schannel                     |                                                                                             |
| Mbed OS 5.4         | Mbed TLS 2                   | [MXChip IoT dev kiti](https://microsoft.github.io/azure-iot-developer-kit/)                  |
| Azure Küre İşletim Sistemi     | WolfssL                      | [Azure Küre MT3620](https://azure.microsoft.com/services/azure-sphere/get-started/) |
| Arduino             | BearsSL                      | [ESP32 veya ESP8266](https://github.com/Azure/azure-iot-arduino#simple-sample-instructions) 

### <a name="python-sdk"></a>Python SDK'sı

[Azure IoT Hub Python cihazı SDK](https://github.com/Azure/azure-iot-sdk-python) aşağıdaki yapılandırmalarla test edilir ve destekler.

| İşletim Sistemi                  | Derleyici                          |
|---------------------|-----------------------------------|
| Linux               | Python 2.7.*, 3.5 veya sonrası |
| MacOS Yüksek Sierra   | Python 2.7.*, 3.5 veya sonrası |
| Windows 10 ailesi   | Python 2.7.*, 3.5 veya sonrası |

Yalnızca Python sürüm 3.5.3 veya daha sonra eşzamanlı API'leri destekler, sürüm 3.7 veya sonraki sürüm kullanmanızı öneririz.

### <a name="net-sdk"></a>.NET SDK

[Azure IoT Hub .NET (C#) aygıtı SDK](https://github.com/Azure/azure-iot-sdk-csharp) ile sınanır ve aşağıdaki yapılandırmaları destekler.

| İşletim Sistemi                                   | Standart                                                   |
|--------------------------------------|------------------------------------------------------------|
| Linux                                | .NET Core 2.1                                              |
| Windows 10 Masaüstü ve Sunucu SBİz'LERI   | .NET Çekirdek 2.1, .NET Çerçeve 4.5.1 veya .NET Çerçeve 4.7 |

.NET SDK, Windows IoT Core ile [Azure Aygıt Aracısı](https://github.com/ms-iot/azure-client-tools/blob/master/docs/device-agent/device-agent.md) veya [UWP uygulamalarıyla iletişim kurmak için RPC'yi kullanabilen özel bir NTService](https://docs.microsoft.com/samples/microsoft/windows-iotcore-samples/ntservice-rpc/)ile de kullanılabilir.

### <a name="nodejs-sdk"></a>Node.js SDK'sı

[Azure IoT Hub Düğümü.js cihazı SDK](https://github.com/Azure/azure-iot-sdk-node) aşağıdaki yapılandırmalarla test edilir ve aşağıdaki yapılandırmaları destekler.

| İşletim Sistemi                  | Düğüm sürümü    |
|---------------------|-----------------|
| Linux               | LTS ve Akım |
| Windows 10 ailesi   | LTS ve Akım |

### <a name="java-sdk"></a>Java SDK

[Azure IoT Hub Java cihazı SDK](https://github.com/Azure/azure-iot-sdk-java) aşağıdaki yapılandırmalarla test edilir ve destekler.

| İşletim Sistemi                     | Java sürümü |
|------------------------|--------------|
| Android API 28         | Java 8       |
| Linux x64             | Java 8       |
| Windows 10 ailesi x64  | Java 8       |

## <a name="partner-supported-development-kits"></a>İş ortağı destekli geliştirme kitleri

Microsoft, çeşitli mikroişlemci mimarileri için geliştirme kitleri sağlamak için çeşitli iş ortakları ile çalışır. Bu iş ortakları Azure IoT C SDK'yı platformlarına taşıymış. Ortaklar SDK'nın platform soyutlama katmanını (PAL) oluşturur ve korur. Microsoft, genişletilmiş destek sağlamak için bu iş ortaklarıyla birlikte çalışır.

| İş Ortağı             | Cihazlar                            | Bağlantı                     | Destek |
|---------------------|------------------------------------|--------------------------|---------|
| Espressif           | ESP32 <br/> ESP8266                              | [Esp-azure](https://github.com/espressif/esp-azure)                | [GitHub](https://github.com/espressif/esp-azure)  
| Qualcomm            | Qualcomm MDM9206 LTE IoT Modem     | [IoT SDK için Qualcomm LTE](https://developer.qualcomm.com/software/lte-iot-sdk) | [Forum](https://developer.qualcomm.com/forums/software/lte-iot-sdk)   |
| ST Mikroelektronik | STM32L4 Serisi <br/> STM32F4 Serisi <br/>  STM32F7 Serisi <br/>  IoT düğümü için STM32L4 Keşif Kiti    | [X-KÜP-AZURE](https://www.st.com/en/embedded-software/x-cube-azure.html) <br/>  <br/> [P-NUCLEO-AZURE](https://www.st.com/content/st_com/en/products/evaluation-tools/solution-evaluation-tools/communication-and-connectivity-solution-eval-boards/p-nucleo-azure1.html) <br/> [FP-CLD-AZURE](https://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32-ode-function-pack-sw/fp-cld-azure1.html)            | [Destek](https://www.st.com/content/st_com/en/support/support-home.html)
| Teksas Enstrümanlar   | CC3220SF Fırlatma Rampası </br> CC3220S Fırlatma Rampası </br> CC3235SF Fırlatma Rampası </br> CC3235S Fırlatma Rampası </br> MSP432E4 Fırlatma Rampası | [SimpleLink için Azure IoT Eklentisi](https://github.com/TexasInstruments/azure-iot-pal-simplelink) | [TI E2E Forumu](https://e2e.ti.com) <br/> [CC3220 için TI E2E Forumu](https://e2e.ti.com/support/wireless_connectivity/simplelink_wifi_cc31xx_cc32xx/) <br/> [MSP432E4 için TI E2E Forumu](https://e2e.ti.com/support/microcontrollers/msp430/) |

## <a name="porting-the-microsoft-azure-iot-c-sdk"></a>Microsoft Azure IoT C SDK'yı taşıma

Cihaz platformunuz önceki bölümlerden biri tarafından karşılamıyorsa, Azure IoT C SDK'yı taşımayı düşünebilirsiniz. C SDK'nın taşıması öncelikle SDK'nın platform soyutlama katmanının (PAL) uygulanmasını içerir. PAL, aygıtınız ile SDK'daki üst düzey işlevler arasındaki tutkal sağlayan ilkel leri tanımlar. Daha fazla bilgi için Bkz. [Taşıma Kılavuzu.](https://github.com/Azure/azure-c-shared-utility/blob/master/devdoc/porting_guide.md)

## <a name="microsoft-partners-and-certified-azure-iot-devices"></a>Microsoft iş ortakları ve sertifikalı Azure IoT aygıtları

Microsoft, Azure IoT test edilmiş ve sertifikalı cihazlarla Azure IoT evrenini sürekli olarak genişletmek için bir dizi iş ortağıyla birlikte çalışır.

* Azure IoT sertifikalı cihazlara göz atmak [için IoT Aygıt Kataloğu için Microsoft Azure Sertifikalı'ya](https://catalog.azureiotsolutions.com/)bakın.

* IoT için Azure Sertifikalı ekosistemi hakkında daha fazla bilgi edinmek [için](https://catalog.azureiotsolutions.com/register)bkz.

## <a name="connecting-to-iot-hub-without-an-sdk"></a>SDK olmadan IoT Hub'a bağlanma

IoT Hub aygıtı SDK'larından birini kullanamıyorsanız, HTTPS istekleri ni ve yanıtlarını gönderip alabilen herhangi bir uygulamadan [IoT Hub REST API'lerini](https://docs.microsoft.com/rest/api/iothub/) kullanarak doğrudan IoT Hub'a bağlanabilirsiniz.

## <a name="support-and-other-resources"></a>Destek ve diğer kaynaklar

Azure IoT aygıt SDK'larını kullanırken sorun yaşıyorsanız, destek aramanın birkaç yolu vardır. Aşağıdaki kanallardan birini deneyebilirsiniz:

**Hataların bildirilmesi** – Aygıt SDK'larında bulunan hatalar ilgili GitHub projesinin sorunlar sayfasında bildirilebilir. Düzeltmeler hızla projeden ürün güncelleştirmelerine doğru yol alar.

* [Azure IoT Hub C SDK sorunları](https://github.com/Azure/azure-iot-sdk-c/issues)

* [Azure IoT Hub .NET (C#) SDK sorunları](https://github.com/Azure/azure-iot-sdk-csharp/issues)

* [Azure IoT Hub Java SDK sorunları](https://github.com/Azure/azure-iot-sdk-java/issues)

* [Azure IoT Hub Düğümü.js SDK sorunları](https://github.com/Azure/azure-iot-sdk-node/issues)

* [Azure IoT Hub Python SDK sorunları](https://github.com/Azure/azure-iot-sdk-python/issues)

**Microsoft Müşteri Destek ekibi** – [Destek planı](https://azure.microsoft.com/support/plans/) olan kullanıcılar, doğrudan [Azure portalından](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)yeni bir destek isteği oluşturarak Microsoft Müşteri Destek ekibini devreye alabilir.

**Özellik istekleri** – Azure IoT özellik istekleri ürünün [Kullanıcı Sesi sayfası](https://feedback.azure.com/forums/321918-azure-iot)üzerinden izlenir.

## <a name="next-steps"></a>Sonraki adımlar

* [Cihaz ve hizmet SDK’ları](iot-hub-devguide-sdks.md)
* [Taşıma Kılavuzu](https://github.com/Azure/azure-c-shared-utility/blob/master/devdoc/porting_guide.md)
