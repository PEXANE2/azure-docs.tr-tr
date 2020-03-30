---
title: Azure Nesnelerin İnternetine Giriş (IoT)
description: Azure IoT ve IoT hizmetlerinin temellerini açıklayan giriş, IoT'nin kullanımını göstermeye yardımcı olan örnekler de dahil olmak üzere.
author: dominicbetts
ms.service: iot-fundamentals
services: iot-fundamentals
ms.topic: overview
ms.date: 01/15/2020
ms.author: dobett
ms.openlocfilehash: c79f18669e1b13f79491e98658107221b43f3ff5
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77046186"
---
# <a name="what-is-azure-internet-of-things-iot"></a>Azure Nesnelerin İnterneti (IoT) nedir?

Azure Nesnelerin İnterneti (IoT), Microsoft tarafından yönetilen ve milyarlarca IoT varlığını bağlayan, izleyen ve denetleyen bulut hizmetlerinden oluşan bir koleksiyondur. Daha basit bir ifadeyle, Bir IoT çözümü, bulutta barındırılan bir veya daha fazla arka uç hizmetiyle iletişim kuran bir veya daha fazla IoT aygıtından oluşur. 

## <a name="iot-devices"></a>IoT cihazları

Bir IoT cihazı genellikle internete bağlanmak için WiFi kullanan sensörleri ile bir devre kartı oluşur. Örnek:

* Uzaktan kumandalı yağ pompasında basınç sensörü.
* Klima ünitesinde sıcaklık ve nem sensörleri.
* Asansörde bir ivmeölçer.
* Bir odadaki durum sensörleri.

Çözümünüzü oluşturmak için farklı üreticilerden çok çeşitli cihazlar mevcuttur. Azure IoT Hub ile çalışmak üzere sertifikalı aygıtların listesi [için, IoT aygıt için Azure Sertifikalı aygıt kataloğuna](https://catalog.azureiotsolutions.com/alldevices)bakın. Prototip yazmak için [MXChip IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) veya [Raspberry Pi](https://www.raspberrypi.org/)gibi aygıtları kullanabilirsiniz. Devkit'te sıcaklık, basınç, nem ve jiroskop, ivmeölçer ve manyetometre için dahili sensörler bulunmaktadır. Raspberry Pi, birçok farklı sensör türünü takmanızı sağlar. 

Microsoft, aygıtlarınızda çalışan uygulamaları oluşturmak için kullanabileceğiniz açık kaynaklı [Aygıt SDK'ları](../iot-hub/iot-hub-devguide-sdks.md) sağlar. Bu SDK'lar IoT çözümlerinizi [basitleştirir ve hızlandırır.](https://azure.microsoft.com/blog/benefits-of-using-the-azure-iot-sdks-in-your-azure-iot-solution/)

## <a name="communication"></a>İletişim

Genellikle, IoT aygıtları sensörlerden buluttaki arka uç hizmetlerine telemetri gönderir. Ancak, aygıtlarınıza komut gönderen bir arka uç hizmeti gibi diğer iletişim türleri de mümkündür. Aşağıda aygıttan buluta ve buluttan cihaza iletişime örnek verilmiştir:

* Mobil soğutma kamyonu her 5 dakikada bir sıcaklığı Bir IoT Hub'ına gönderir. 

* Arka uç hizmeti, bir sorunu tanılamaya yardımcı olmak için telemetri gönderme sıklığını değiştirmek için bir aygıta komut gönderir. 

* Aygıt, sensörlerinden okunan değerlere göre uyarılar gönderir. Örneğin, bir kimya tesisindeki toplu reaktörü izleyen bir cihaz, sıcaklık belirli bir değeri aştığında bir uyarı gönderir.

* Aygıtlarınız, insan operatörler tarafından görüntülenmek üzere panoda görüntülenmek üzere bilgi gönderir. Örneğin, rafinerideki bir kontrol odası her borudaki sıcaklık, basınç ve akış hacimlerini göstererek operatörlerin tesisi izlemesini sağlayabilir. 

[IoT Aygıt SDK'ları](../iot-hub/iot-hub-devguide-sdks.md) ve IoT Hub, HTTP, MQTT ve AMQP gibi yaygın [iletişim protokollerini](../iot-hub/iot-hub-devguide-protocols.md) destekler.

IoT aygıtları, tarayıcılar ve mobil uygulamalar gibi diğer istemcilerle karşılaştırıldığında farklı özelliklere sahiptir. Cihaz SDK'ları, cihazları arka uç hizmetinize güvenli ve güvenilir bir şekilde bağlamanın zorluklarını gidermenize yardımcı olur.  IoT cihazları şu özelliklere sahiptir:

* Genellikle insan kullanıcısı bulunmayan katıştırılmış sistemlerdir (bir telefonun aksine).
* Fiziksel erişimin pahalı olduğu uzak konumlarda dağıtılabilir.
* Yalnızca çözüm arka ucu aracılığıyla erişilebilir.
* Sınırlı güç ve işleme kaynaklarına sahip olabilir.
* Aralıklı, yavaş veya pahalı bir ağ bağlantısına sahip olabilir.
* Mülkiyete ait, özel veya sektöre özgü uygulama protokolleri kullanması gerekebilir.

## <a name="back-end-services"></a>Arka uç hizmetleri 

Bir IoT çözümünde, arka uç hizmeti aşağıdakiler gibi işlevler sağlar:

* Aygıtlarınızdan ölçekolarak telemetri alma ve bu verilerin nasıl işleyip depolayıcı yapılacağını belirleme.
* Telemetriyi analiz ederek, gerçek zamanlı ya da gerçeklerden sonra içgörüsağlamak için.
* Buluttan belirli bir aygıta komut gönderme. 
* Aygıtları sağlama ve hangi cihazların altyapınıza bağlanabileceğini denetleme.
* Aygıtlarınızın durumunu denetleme ve etkinliklerini izleme.
* Aygıtlarınıza yüklenen firmware'i yönetme.

Örneğin, bir petrol pompalama istasyonu için uzaktan izleme çözümünde, bulut arka uç anormal davranışı tanımlamak için pompalardan telemetri kullanır. Arka uç hizmeti bir anomaliyi tanımladığında, düzeltici bir eylem de almak için otomatik olarak aygıta bir komut gönderebilir. Bu işlem, cihaz ile bulut arasında çözüm verimliliğini önemli ölçüde artıran otomatik bir geri bildirim döngüsü oluşturur.

## <a name="azure-iot-examples"></a>Azure IoT örnekleri

Kuruluşların Azure IoT'yi nasıl kullandığına ilgili gerçek hayattan örnekler [için, IoT için Microsoft Teknik Örnek İncelemeleri'ne](https://microsoft.github.io/techcasestudies/#technology=IoT&sortBy=featured)bakın. 

## <a name="next-steps"></a>Sonraki adımlar

Bazı gerçek iş durumları ve kullanılan mimari için [Microsoft Azure IoT Teknik Örnek Olay İncelemeleri'ne](https://microsoft.github.io/techcasestudies/#technology=IoT&sortBy=featured)bakın.

IoT DevKit ile deneyebileceğiniz bazı örnek projeler için [IoT DevKit Proje Kataloğu'na](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/)bakın. 

Farklı hizmetlerin ve bunların nasıl kullanıldığı hakkında daha kapsamlı bir açıklama için [Azure IoT hizmetleri ve teknolojilerine](iot-services-and-technologies.md)bakın.

IoT mimarisinin ayrıntılı incelemesi için bkz. [Microsoft Azure IoT Başvuru Mimarisi](https://aka.ms/iotrefarchitecture).
