---
title: Azure IOT Hub fiyatlandırması anlama | Microsoft Docs
description: Geliştirici Kılavuzu - örnekler ölçüm ve works fiyatlandırma dahil olmak üzere IOT Hub ile nasıl çalıştığı hakkında bilgi.
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 03/11/2019
ms.openlocfilehash: 9b6db1b7171652ea5ace4db370b72dc22b6bdc90
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79271232"
---
# <a name="azure-iot-hub-pricing-information"></a>Azure IOT Hub fiyatlandırma bilgileri

[Azure IoT Hub fiyatlandırması](https://azure.microsoft.com/pricing/details/iot-hub) , IoT Hub Için farklı SKU 'ların ve fiyatlandırmayla ilgili genel bilgileri sağlar. Bu makalede, çeşitli IOT hub'ı işlevlerini iletileri olarak IOT Hub tarafından nasıl ölçülür ek ayrıntılı bilgi içerir.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="charges-per-operation"></a>İşlem başına ücret

| İşlem | Faturalama bilgileri | 
| --------- | ------------------- |
| Kimlik kayıt defteri işlemleri <br/> (oluşturma, Al, Listele, güncelleştirme ve silme) | Ücretsizdir. |
| Cihazdan buluta iletiler | Başarıyla gönderilen iletileri IOT Hub'ına giriş üzerinde 4 KB'lık parçalar halinde ücretlendirilir. Örneğin, 6-KB'lık mesaj, 2 mesaj ücretlendirilir. |
| Bulut-cihaz iletilerini | Başarıyla gönderilen iletiler 4 KB'lık parçalar halinde ücretlendirilir, örneğin 6 KB'lık bir ileti 2 ileti ücretlendirilir. |
| Dosya yüklemeleri | IOT Hub tarafından Azure Depolama'ya dosya aktarımı ölçülen değil. Dosya aktarımı başlangıcını ve tamamlanmasını iletiler 4 KB'lık artışlarla tarifeli messaged olarak ücretlendirilir. Örneğin, 10 MB 'lık bir dosyanın aktarılması, Azure depolama maliyetine ek olarak iki ileti olarak ücretlendirilir. |
| Doğrudan yöntemler | Başarılı Yöntem istekleri 4 KB 'lık öbeklere göre ücretlendirilir ve yanıtlar ek iletiler olarak 4 KB 'lık parçalar halinde ücretlendirilir. Bağlantısı kesilmiş cihazlar isteklerine uygunsa iletiler 4 KB'lık parçalar halinde ücretlendirilir. Örneğin, cihazdan gövdesi olmayan bir Yanıt ile sonuçlanan 4 KB 'lık gövdeye sahip bir yöntem iki ileti olarak ücretlendirilir. Bir CİHAZDAN bir 1 KB'lık yanıt sonuçlanır bir 6-KB gövdesi bir yöntemle yanı sıra başka bir ileti yanıtı için istek için iki ileti olarak ücretlendirilir. |
| Cihaz ve modül ikizi okumaları | İkiz cihaz veya modül ve çözüm arka uç, iletileri 512 baytlık parçalar halinde ücretlendirilir okur. Örneğin, 6-KB ikizi okuma 12 iletileri olarak ücretlendirilir. |
| Cihaz ve modül ikizi güncelleştirmeleri (etiketler ve Özellikler) | Ve çözüm arka ucu cihaz veya modül ikizi güncelleştirmeleri iletileri 512 baytlık parçalar halinde ücretlendirilir. Örneğin, 6-KB ikizi okuma 12 iletileri olarak ücretlendirilir. |
| Cihaz ve modül ikizi sorguları | Sorguları iletileri 512 baytlık öbekler halinde sonucu boyutuna bağlı olarak ücretlendirilir. |
| İş işlemleri <br/> (oluşturma, güncelleştirme, listeleme, silme) | Ücretsizdir. |
| İşleri cihaz başına işlem | İş işlemleri (örneğin, ikizi güncelleştirmeleri ve yöntemleri) normal olarak ücretlendirilir. Örneğin, 1 KB'lık isteklerini ve yanıtlarını gövdesi boş ile 1000 yöntem çağrılarını sonuçta bir işi 1000 iletileri üzerinden ücretlendirilir. |
| Etkin tutma iletileri | AMQP veya MQTT protokolleri kullanırken bağlantı kurmak için alınıp verilen iletileri ve anlaşmasında alınıp verilen iletileri ücretlendirilmez. |

> [!NOTE]
> Tüm boyutları (Protokol çerçeveleme göz ardı edilir) bayt yükü boyutu dikkate hesaplanır. Özellikler ve gövdesi olan iletiler için bir protokol belirsiz şekilde boyutu hesaplanır. Daha fazla bilgi için [IoT Hub ileti biçimi](iot-hub-devguide-messages-construct.md)bölümüne bakın.

## <a name="example-1"></a>Örnek #1

Bir cihaz IOT Hub, ardından Azure Stream Analytics tarafından okunur dakika başına tek bir 1 KB'lık CİHAZDAN buluta ileti gönderir. Çözüm arka ucu, belirli bir eylemi tetiklemek için her 10 dakikada bir bir yöntemi (512 baytlık yük ile) çağırır. Cihaz yönteminin bir sonuç 200 bayt ile yanıt verir.

Aygıt kullanır:

* Bir ileti * = 60 dakika * 24 saat CİHAZDAN buluta iletiler için günde 1440 ileti.
* İki istek yanıt * 6 kez / saat * 24 saat = 288 iletileri için yöntemleri.

Bu hesaplama, günlük 1728 ileti toplam sağlar.

## <a name="example-2"></a>Örnek #2

Bir cihazı, saatte bir 100 KB'lık CİHAZDAN buluta ileti gönderir. Ayrıca, cihaz ikizi dört saatte bir 1 KB'lık yükleriyle güncelleştirir. Çözüm arka son kez, günlük 14-KB cihaz ikizi okumaları ve yapılandırmaları değiştirmek için 512 baytlık yükleriyle güncelleştirir.

Aygıt kullanır:

* 25 (100 KB/4 KB) iletileri * CİHAZDAN buluta iletiler için 24 saat.
* İki ileti (1 KB/0,5 KB) * altı kat her gün için cihaz ikizi güncelleştirmeleri.

Bu hesaplama, günlük 612 ileti toplam sağlar.

Çözüm arka ucu, cihaz ikizi okumak için 28 iletileri (14 KB/0,5 KB) ek olarak, güncelleştirilecek bir ileti 29 iletilerin toplam tüketir.

Toplam günlük 641 ileti cihaz ve çözüm arka ucu kullanır.
