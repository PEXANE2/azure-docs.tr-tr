---
title: Azure IoT Merkezi aygıt köprüsünü oluşturun | Microsoft Dokümanlar
description: Diğer IoT bulutlarını (Sigfox, Parçacık, The Things Network vb.) IoT Central uygulamanıza bağlamak için IoT Merkezi cihaz köprüsünü oluşturun.
services: iot-central
ms.service: iot-central
author: viv-liu
ms.author: viviali
ms.date: 07/09/2019
ms.topic: how-to
manager: peterpr
ms.openlocfilehash: 6499c9c29d10a2056b0af5499b68b5edd67d82cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80158427"
---
# <a name="build-the-iot-central-device-bridge-to-connect-other-iot-clouds-to-iot-central"></a>Diğer IoT bulutlarını IoT Central'a bağlamak için IoT Merkezi aygıt köprüsünü oluşturun

*Bu konu yöneticiler için geçerlidir.*

IoT Central aygıt köprüsü, Sigfox, Parçacık, Things Network ve diğer bulutlarınızı IoT Central uygulamanıza bağlayan açık kaynaklı bir çözümdür. İster Sigfox'un Düşük Güç-Geniş Alan Ağı'na bağlı varlık izleme cihazları kullanıyor olun, ister Parçacık Aygıt Bulutu'ndaki hava kalitesi izleme cihazlarını kullanıyor olun, ister TTN'deki toprak nem izleme cihazlarını kullanıyor olun, Doğrudan IoT'nin gücünden yararlanabilirsiniz IoT Merkezi cihaz köprüsünü kullanarak merkezi. Aygıt köprüsü, aygıtlarınızın diğer bulutlara gönderdiği verileri IoT Central uygulamanıza ileterek diğer IoT bulutlarını IoT Central'a bağlar. IoT Central uygulamanızda, bu veriler üzerinde kurallar oluşturabilir ve analitik çalıştırabilir, Microsoft Flow ve Azure Logic uygulamalarında iş akışları oluşturabilir, bu verileri dışa aktarabilir ve çok daha fazlasını yapabilirsiniz. GitHub'dan [IoT Merkezi cihaz köprüsünü](https://aka.ms/iotcentralgithubdevicebridge) alın

## <a name="what-is-it-and-how-does-it-work"></a>Nedir ve nasıl çalışır?
IoT Central aygıt köprüsü GitHub'da açık kaynaklı bir çözümdür. Azure aboneliğinize birkaç Azure kaynağı içeren özel bir Azure Kaynak Yöneticisi şablonu dağıtan "Azure'a Dağıt" düğmesiyle devam etmeye hazırdır. Kaynaklar şunlardır:
-    Azure İşlevi uygulaması
-    Azure Depolama Hesabı
-    Tüketim Planı
-    Azure Key Vault

İşlev uygulaması aygıt köprüsünün kritik bir parçasıdır. Basit bir webhook entegrasyonu aracılığıyla diğer IoT platformlarından veya herhangi bir özel platformdan HTTP POST isteklerini alır. Sigfox, Parçacık ve TTN bulutlarına nasıl bağlanılabildiğini gösteren örnekler sunduk. Platformunuz işlev uygulamanıza HTTP POST istekleri gönderebiliyorsa, bu çözümü özel IoT bulutunuza bağlanmak için kolayca genişletebilirsiniz.
İşlev uygulaması verileri IoT Central tarafından kabul edilen bir biçime dönüştürür ve DPS API'leri aracılığıyla iletir.

![Azure işlevleri ekran görüntüsü](media/howto-build-iotc-device-bridge/azfunctions.png)

IoT Central uygulamanız iletilen iletideki aygıt kimliğiyle aygıtı tanırsa, bu aygıt için yeni bir ölçüm görüntülenir. Aygıt kimliği IoT Central uygulamanız tarafından hiç görülmediyse, işlev uygulamanız bu aygıt kimliğine yeni bir aygıt kaydetmeye çalışır ve IoT Central uygulamanızda "İlişkisiz aygıt" olarak görünür. 

## <a name="how-do-i-set-it-up"></a>Nasıl ayarlıyorum?
Yönergeler GitHub repo'sundaki README dosyasında ayrıntılı olarak listelenir. 

## <a name="pricing"></a>Fiyatlandırma
Azure kaynakları Azure aboneliğinizde barındırılacaktır. Fiyatlandırma hakkında daha fazla bilgi için [README dosyasında](https://aka.ms/iotcentralgithubdevicebridge)bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Şimdi Nasıl IoT Merkezi cihaz köprü inşa etmeyi öğrendim, burada önerilen bir sonraki adımdır:

> [!div class="nextstepaction"]
> [Cihazlarınızı yönetme](howto-manage-devices.md)
