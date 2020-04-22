---
title: Architecture IoT Connected lojistik | Microsoft Dokümanlar
description: IoT Central için IoT Connected Logistics uygulama şablonunun mimarisi
author: KishorIoT
ms.author: nandab
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: overview
ms.date: 10/20/2019
ms.custom: mqtt
ms.openlocfilehash: 8925b98269b67bfb8a96cb057982ee4e396f17ed
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81686221"
---
# <a name="architecture-of-iot-central-connected-logistics-application-template"></a>IoT Central bağlantılı lojistik uygulama şablonu mimarisi



İş ortakları & müşteri, bitişten uca **bağlı lojistik çözümleri**geliştirmek için aşağıdaki kılavuz& uygulama şablonu kullanabilir.

> [!div class="mx-imgBorder"]
> ![bağlı lojistik panosu](./media/concept-connected-logistics-architecture/connected-logistics-architecture.png)

1. Bir ağ geçidi aygıtına telemetri verileri gönderen IoT etiketleri kümesi
2. IoT Central'a telemetri ve toplu kavrayış gönderen ağ geçidi cihazları
3. Veriler, manipülasyon için istenilen Azure hizmetine yönlendirilir
4. ASA veya Azure İşlevleri gibi Azure hizmetleri, veri akışlarını yeniden biçimlemek ve istenen depolama hesaplarına göndermek için kullanılabilir 
5. Çeşitli iş akışları son kullanıcı iş uygulamaları tarafından desteklenebilir

## <a name="details"></a>Ayrıntılar
Aşağıdaki bölümde IoT Etiketleri & Ağ Geçitleri kavramsal mimari Telemetri yutma her bölümü özetliyor

## <a name="iot-tags"></a>IoT etiketleri
IoT etiketleri Sıcaklık, Nem, Şok, Tilt &Işığı gibi fiziksel, ortam ve çevresel sensör özellikleri sağlar. IoT etiketleri genellikle Zigbee (802.15.4) üzerinden ağ geçidi aygıtına bağlanır. Etiketler daha az pahalı sensörler; bu nedenle, ters lojistik ile zorlukları önlemek için tipik bir lojistik yolculuğun sonunda atılabilir.

## <a name="gateway"></a>Ağ geçidi
Ağ geçitleri, ortam algılama yetenekleriyle IoT etiketleri olarak da hareket edebilir. Ağ geçidi, hücresel, Wi-Fi kanallarını kullanarak azure ioT bulut bağlantısının (MQTT) yukarı yada akışını sağlar.  Bluetooth, NFC ve 802.15.4 Kablosuz Sensör Ağı (WSN) modları, IoT etiketleri ile aşağı akış iletişimi için kullanılır. Ağ geçitleri uçlardan uca güvenli bulut bağlantısı, IoT etiket eşleştirme, sensör veri toplama, veri tutma ve alarm eşiklerini yapılandırma olanağı sağlar.

## <a name="device-management-with-iot-central"></a>IoT Central ile cihaz yönetimi 
Azure IoT Central, IoT aygıt bağlantısını, yapılandırmayı ve yönetimini basitleştiren bir çözüm geliştirme platformudur. Platform, IoT cihaz yönetimi, operasyonları ve ilgili gelişmelerin yükünü ve maliyetlerini önemli ölçüde azaltır. İş ortakları & müşteriler, lojistikte dijital geri bildirim döngüsü elde etmek için son dan sona kurumsal çözümler oluşturabilir.

## <a name="business-insights-and-actions-using-data-egress"></a>Veri çıkışlarını kullanarak iş öngörüleri ve eylemler 
IoT Merkezi platformu, Sürekli Veri Aktarım (CDE) ve API'ler aracılığıyla zengin genişletilebilirlik seçenekleri sağlar. Telemetri veri işleme veya ham telemetriye dayalı iş öngörüleri genellikle tercih edilen bir iş yeri uygulamasına aktarılır. Webhook, servis veri merkezi, etkinlik merkezi veya blob depolama sı kullanılarak, makine öğrenimi modellerini oluşturmak, eğitmek ve dağıtmak & öngörüleri daha da zenginleştirmek sağlanabilir.

## <a name="next-steps"></a>Sonraki adımlar
* Bağlı lojistik [çözüm şablonu](./tutorial-iot-central-connected-logistics.md) nasıl dağıtılayacaklarını öğrenin
* [IoT Central perakende şablonları](./overview-iot-central-retail.md) hakkında daha fazla bilgi edinin
* IoT Central hakkında daha fazla bilgi için [IoT Central genel bakış](../core/overview-iot-central.md)
