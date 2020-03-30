---
title: Azure IoT SDK'ları kullanan mobil cihazlar için geliştirme | Microsoft Dokümanlar
description: Geliştirici kılavuzu - Azure IoT Hub SDK'larını kullanarak mobil cihazlarda nasıl geliştirileceğimiz hakkında bilgi edinin.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/16/2018
ms.author: robinsh
ms.openlocfilehash: 945b02003a443c04e692fdc06ca5714de362d074
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68883089"
---
# <a name="develop-for-mobile-devices-using-azure-iot-sdks"></a>Azure IoT SDK'ları kullanan mobil cihazlar için geliştirme

Nesnelerin İnterneti'ndeki şeyler, sensörler, mikrodenetleyiciler, akıllı cihazlar, endüstriyel ağ geçitleri ve hatta mobil cihazlar gibi farklı yeteneklere sahip çok çeşitli aygıtlara atıfta bulunabilir.  Mobil aygıt, aygıtdan buluta telemetri gönderdiği ve bulut tarafından yönetildiği bir IoT aygıtı olabilir.  Ayrıca, diğer IoT aygıtlarını yöneten bir arka uç servis uygulaması çalıştıran aygıt da olabilir.  Her iki durumda [da, Azure IoT Hub SDK'ları](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks) mobil cihazlariçin çalışan uygulamalar geliştirmek için kullanılabilir.  

## <a name="develop-for-native-ios-platform"></a>Yerel iOS platformu için geliştirin

Azure IoT Hub SDK'ları, Azure IoT Hub C SDK aracılığıyla yerel iOS platform desteği sağlar.  Swift veya Objective C XCode projenize dahil olabileceğiniz bir iOS SDK olarak düşünebilirsiniz.  iOS'ta C SDK'yı kullanmanın iki yolu vardır:

* XCode projesindeki CocoaPod kitaplıklarını doğrudan kullanın.  
* C SDK'nın kaynak kodunu indirin ve MacOS için [yapı talimatını](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) takiben iOS platformu için oluşturun.  

Azure IoT Hub C SDK, çeşitli platformlarda maksimum taşınabilirlik için C99'da yazılmıştır.  Taşıma işlemi, [iOS](https://github.com/Azure/azure-c-shared-utility/tree/master/pal/ios-osx)için burada bulunabilen platforma özgü bileşenler için ince bir benimseme katmanı yazmayı içerir.  C SDK'daki özellikler, desteklenen Azure IoT Hub ilkelleri ve ağ güvenilirliği için yeniden deneme ilkesi gibi SDK'ya özgü özellikler de dahil olmak üzere iOS platformunda kullanılabilir.  iOS SDK arabirimi, Azure IoT Hub C SDK arabirimine de benzer.  

Bu belgeler, bir iOS aygıtında bir aygıt uygulaması veya hizmet uygulamasının nasıl geliştirilebildiğini gözden geçirir:

* [Hızlı Başlangıç: Bir cihazdan IoT hub’a telemetri gönderme](quickstart-send-telemetry-ios.md)  
* [IoT hub'ı ile buluttan cihazınıza ileti gönderme](iot-hub-ios-swift-c2d.md) 

### <a name="develop-with-azure-iot-hub-cocoapod-libraries"></a>Azure IoT Hub CocoaPod kitaplıkları ile geliştirme

Azure IoT Hub SDK'ları, iOS geliştirme için bir dizi Objective-C CocoaPod kitaplığı yayımlar.  CocoaPod kitaplıklarının en son listesini görmek [için Microsoft Azure IoT için CocoaPods'a](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/samples/ios/CocoaPods.md)bakın.  İlgili kitaplıklar XCode projenize dahil edildikten sonra, IoT Hub ile ilgili kodu yazmanın iki yolu vardır:

* Amaç C işlevi: Projeniz Objective-C ile yazılmışsa, Doğrudan Azure IoT Hub C SDK'dan API'leri arayabilirsiniz.  Projeniz Swift ile yazılmışsa, `@objc func` işlevinizi oluşturmadan önce arayabilir ve C veya Objective-C kodunu kullanarak Azure IoT Hub ile ilgili tüm mantıkları yazmaya devam edebilirsiniz.  Her ikisini de gösteren bir dizi [örnek örnek deposunda](https://github.com/Azure-Samples/azure-iot-samples-ios)bulunabilir.  

* C örneklerini birleştirin: Bir C cihazı uygulaması yazdıysanız, doğrudan XCode projenizde başvuruda bulunabilirsiniz:
    * XCode'dan xcode projenize örnek.c dosyasını ekleyin.  
    * Üstbilgi dosyasını bağımlılığınıza ekleyin.  Bir üstbilgi dosyası [örnek](https://github.com/Azure-Samples/azure-iot-samples-ios) deposuna örnek olarak dahil edilir. Daha fazla bilgi için lütfen [Objective-C](https://developer.apple.com/documentation/objectivec)için Apple'ın dokümantasyon sayfasını ziyaret edin.

## <a name="develop-for-android-platform"></a>Android platformu için geliştirin
Azure IoT Hub Java SDK Android platformlarını destekler.  Test edilen özel API sürümü için lütfen en son güncelleme için [platform destek sayfamızı](iot-hub-device-sdk-platform-support.md) ziyaret edin.

Bu dokümantasyon, Gradle ve Android Studio kullanarak bir Android cihazda bir cihaz uygulaması veya hizmet uygulaması geliştirmek için nasıl yürür:

* [Hızlı Başlangıç: Bir cihazdan IoT hub’a telemetri gönderme](quickstart-send-telemetry-android.md)  
* [Quickstart: Aygıtı denetleme](quickstart-control-device-android.md) 

## <a name="next-steps"></a>Sonraki adımlar

* [IoT Hub REST API başvurusu](https://docs.microsoft.com/rest/api/iothub/)
* [Azure IoT C SDK kaynak kodu](https://github.com/Azure/azure-iot-sdk-c)
