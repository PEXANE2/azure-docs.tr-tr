---
title: Azure IoT Hub fiyatlandırması | Microsoft Dokümanlar
description: Geliştirici kılavuzu - ioT Hub ile ölçüm ve fiyatlandırmanın nasıl çalıştığı hakkında bilgi çalıştı örnekler.
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 03/11/2019
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: 4c7382f84522333b6aae0d79941aae8f2147a12f
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81729154"
---
# <a name="azure-iot-hub-pricing-information"></a>Azure IoT Hub fiyatlandırma bilgileri

[Azure IoT Hub fiyatlandırması,](https://azure.microsoft.com/pricing/details/iot-hub) farklı SK'ler hakkında genel bilgiler ve IoT Hub için fiyatlandırma sağlar. Bu makalede, çeşitli IoT Hub işlevlerinin IoT Hub tarafından ileti olarak nasıl ölçüldİğİ hakkında ek ayrıntılar yer alıyor.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="charges-per-operation"></a>Operasyon başına ücretler

| İşlem | Fatura bilgileri | 
| --------- | ------------------- |
| Kimlik kayıt defteri işlemleri <br/> (oluşturmak, almak, listele, güncellemek, silmek) | Şarj edilmedi. |
| Cihazdan buluta iletiler | Başarıyla gönderilen iletiler IoT Hub'a girişte 4-KB parçalar halinde ücretlendirilir. Örneğin, 6-KB iletisi 2 ileti yle ücretlendirilir. |
| Buluttan cihaza iletiler | Başarıyla gönderilen iletiler 4-KB parçalar halinde ücretlendirilir, örneğin 6-KB ileti2 ileti ücretlendirilir. |
| Dosya karşıya yüklemeleri | Azure Depolama'ya dosya aktarımı IoT Hub tarafından ölçülmez. Dosya aktarım başlatma ve tamamlama iletileri 4-KB artışlarla tarifeli olarak ücretlendirilir. Örneğin, 10 MB'lık bir dosyanın aktarılması, Azure Depolama maliyetine ek olarak iki ileti olarak ücretlendirilir. |
| Doğrudan yöntemler | Başarılı yöntem istekleri 4-KB parçalar halinde ücretlendirilir ve yanıtlar ek ileti ler olarak 4-KB parçalar halinde ücretlendirilir. Bağlantısı kesilen aygıtlara gelen istekler 4-KB'lık parçalar halinde ileti olarak yüklenir. Örneğin, cihazdan gövde olmadan yanıt veren 4-KB gövdeli bir yöntem iki ileti olarak ücretlendirilir. Cihazdan 1-KB yanıtı ile sonuçlanan 6-KB gövdeli bir yöntem, istek için iki ileti artı yanıt için başka bir ileti olarak ücretlendirilir. |
| Cihaz ve modül ikiz okuma | Cihazdan veya modülden ve çözüm arka ucundan gelen çift okumalar 512 bayt lık parçalar halinde mesaj olarak yüklenir. Örneğin, 6-KB ikiz okuma 12 ileti olarak ücretlendirilir. |
| Cihaz ve modül ikiz güncellemeleri (etiketleri ve özellikleri) | Cihazdan veya modülden ve çözüm arka ucundan gelen çift güncelleştirmeler 512 bayt lık parçalar halinde ileti olarak yüklenir. Örneğin, 6-KB ikiz okuma 12 ileti olarak ücretlendirilir. |
| Cihaz ve modül ikiz sorguları | Sorgular, 512 baytlık parçalar halinde sonuç boyutuna bağlı olarak ileti olarak ücretlendirilir. |
| İş işlemleri <br/> (oluşturma, güncelleştirme, listeleme, silme) | Şarj edilmedi. |
| Aygıt başına işler işlemleri | İş işleri işlemleri (ikiz güncelleştirmeler ve yöntemler gibi) normal olarak ücretlendirilir. Örneğin, 1-KB istekleri ve boş gövde yanıtları ile 1000 yöntem aramaları sonuçlanan bir iş 1000 iletileri tahsil edilir. |
| İletileri canlı tutma | AMQP veya MQTT protokolleri kullanırken, bağlantıyı kurmak için değiştirilen iletiler ve anlaşmada değiştirilen iletiler ücretlendirilmez. |

> [!NOTE]
> Tüm boyutlar baytlarda yük boyutu göz önünde bulundurularak hesaplanır (protokol çerçeveleme göz ardı edilir). Özellikleri ve gövdesi olan iletiler için boyut protokol-agnostik bir şekilde hesaplanır. Daha fazla bilgi için [IoT Hub ileti biçimine](iot-hub-devguide-messages-construct.md)bakın.

## <a name="example-1"></a>Örnek #1

Bir aygıt, IoT Hub'ına dakikada bir adet 1 KB aygıttan buluta ileti gönderir ve bu mesaj Azure Akış Analizi tarafından okunur. Çözüm arka uç, belirli bir eylemi tetiklemek için cihazda her 10 dakikada bir bir yöntem (512 bayt yüküyle) çağırır. Cihaz yönteme 200 bayt lık bir sonuçla yanıt verir.

Cihaz şunları tüketir:

* Bir mesaj * 60 dakika * 24 saat = cihazdan buluta iletiler için günde 1440 ileti.
* İki istek artı yanıt * saatte 6 kez * 24 saat = 288 yöntemleri için mesaj.

Bu hesaplama günde toplam 1728 ileti verir.

## <a name="example-2"></a>Örnek #2

Bir aygıt her saat 100 KB aygıttan buluta bir ileti gönderir. Ayrıca cihaz ikizini her dört saatte bir 1-KB yükleri ile günceller. Çözüm arka uç, günde bir kez, 14-KB cihaz ikiz okur ve yapılandırmaları değiştirmek için 512-bayt yükleri ile günceller.

Cihaz şunları tüketir:

* 25 (100 KB / 4 KB) ileti * cihazdan buluta iletiler için 24 saat.
* İki mesaj (1 KB / 0.5 KB) * cihaz ikiz güncellemeleri için günde altı kez.

Bu hesaplama günde toplam 612 ileti verir.

Çözüm arka uç 28 mesaj (14 KB / 0,5 KB) cihaz ikiz okumak için, artı bir mesaj güncellemek için, toplam 29 mesaj tüketir.

Toplamda, cihaz ve çözüm arka uç günde 641 mesaj tüketir.
