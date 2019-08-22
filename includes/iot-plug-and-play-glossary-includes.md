---
author: ChrisGMsft
ms.service: iot-pnp
ms.topic: include
ms.date: 06/28/2019
ms.author: chrisgre
ms.openlocfilehash: 7e8174855800bc6beea8750c32a6dd32588ccd9e
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69880468"
---
## <a name="iot-plug-and-play-device"></a>IoT Tak ve Kullan cihazı

IoT Tak ve Kullan cihazı genellikle veri toplayacak veya diğer cihazları denetleyebilen ve [cihaz yetenek modelinde](#device-capability-model)belirtilen özellikleri uygulayan yazılım veya bellenim çalıştıran bir küçük ölçekli, tek başına bilgi işlem aygıtıdır.  Örneğin, bir IoT Tak ve Kullan cihazı bir çevre izleme cihazı veya bir akıllı agrigulture sulama sistemi denetleyicisi olabilir. Bulutta barındırılan IoT çözümleri, IoT Tak ve Kullan cihazlarından komut, denetim ve veri almak için yazılabilir. IoT Tak ve Kullan cihazları [IoT Için Azure Sertifikalı cihaz kataloğu](https://catalog.azureiotsolutions.com/)aracılığıyla bulabilirsiniz. Katalogdaki her bir IoT Tak ve Kullan aygıtı doğrulandıktan sonra bir [cihaz yetenek modeli](#device-capability-model)vardır.

## <a name="digital-twin"></a>Dijital ikizi

Dijital TWINS, IoT Tak ve Kullan cihazlarının modelleridir.  Dijital TWINS, [dijital Ikizi tanım dili](https://aka.ms/DTDL)kullanılarak modellenir.  Azure IoT API 'sini kullanarak dijital TWINS ile etkileşim kurabilirsiniz. 

## <a name="digital-twin-definition-language"></a>Digital Ikizi tanım dili

[Iot Tak ve kullan cihazları](#iot-plug-and-play-device)için modelleri ve arabirimleri tanımlamaya yönelik bir dil.  Dijital [Ikizi tanım dilini](https://aka.ms/DTDL) kullanarak bir [Digital ikizi 'ın](#digital-twin) yeteneklerini ve IoT platformunu ve IoT çözümlerini, varlığın semantiklerinden yararlanmak için etkinleştirin.

## <a name="device-capability-model"></a>Cihaz yetenek modeli

Cihaz yetenek modeli bir cihazı tanımlar ve cihaz tarafından uygulanan arabirim kümesini tanımlar. Fiziksel bir cihaza, ürüne veya SKU 'ya karşılık gelen bir cihaz yetenek modeli oluşturun.

## <a name="interface"></a>Arabirim

Bir arabirim, bir cihaz veya dijital ikizi tarafından uygulanan ilgili özellikleri açıklar. Arabirimler, farklı yetenek modelleri arasında yeniden kullanılabilir.

## <a name="property"></a>Özellik

Özellikler, bir Digital ikizi 'ın bazı durumlarını temsil eden bir [arabirimde](#interface) tanımlanan veri alanlarıdır. Özellikleri salt okunurdur veya yazılabilir olarak bildirebilirsiniz. Salt okuma özellikleri, IoT Tak ve Kullan cihazının kendi bağlamında çalışan koda göre ayarlanır, örneğin seri numarası.  Yazılabilir özellikler, alarm eşikleri gibi dış varlıklara göre ayarlanır.

## <a name="telemetry"></a>Telemetri

Bir [arabirimde](#interface) tanımlanan telemetri alanları ölçümleri temsil eder. Bu ölçümler genellikle algılayıcı ayarları gibi değerlerdir.

## <a name="command"></a>Komut

Bir [arabirimde](#interface) tanımlanan komutlar, dijital ikizi yürütülebilecek yöntemleri temsil eder. Örneğin, bir cihazı sıfırlamaya yönelik bir Reset komutu.
