---
title: Mimarlık IoT Akıllı Envanter Yönetimi | Microsoft Dokümanlar
description: IoT Central için IoT Akıllı Envanter Yönetimi şablonunun mimarisi
author: KishorIoT
ms.author: nandab
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: overview
ms.date: 10/20/2019
ms.openlocfilehash: 756f15f6cb0d797d2a7db5fbe5c6157b2913b40b
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77020871"
---
# <a name="architecture-of-iot-central-smart-inventory-management-application-template"></a>IoT Merkezi akıllı envanter yönetimi uygulama şablonu mimarisi

İş ortakları ve müşteriler, **akıllı envanter yönetimi** çözümlerini sona erdirmek için uygulama şablonu ve aşağıdaki kılavuzu kullanabilir.

> [!div class="mx-imgBorder"]
> ![akıllı envanter yönetimi](./media/concept-smart-inventory-mgmt-architecture/smart-inventory-management-architecture.png)

1. Bir ağ geçidi aygıtına telemetri verileri gönderen IoT sensörleri kümesi
2. IoT Central'a telemetri ve toplu kavrayış gönderen ağ geçidi cihazları
3. Veriler, manipülasyon için istenilen Azure hizmetine yönlendirilir
4. ASA veya Azure İşlevleri gibi Azure hizmetleri, veri akışlarını yeniden biçimlemek ve istenen depolama hesaplarına göndermek için kullanılabilir 
5. İşlenen veriler, ML veya toplu iş analizine dayanan ek içgörü geliştirmeleri için yakın gerçek zamanlı eylemler veya soğuk depolama için sıcak depolamada depolanır. 
6. Logic Apps, son kullanıcı iş uygulamalarında çeşitli iş akışlarına güç sağlamak için kullanılabilir

## <a name="details"></a>Ayrıntılar
Aşağıdaki bölümde Radyo frekansı tanımlama (RFID), Bluetooth düşük enerji (BLE) etiketleri kavramsal mimari Telemetri yutma her bölümü özetliyor

## <a name="rfid-tags"></a>RFID etiketleri
RFID etiketleri radyo dalgaları aracılığıyla bir öğe hakkında veri aktarır. RFID etiketleri belirtilmediği sürece genellikle pili yoktur. Etiketler okuyucu tarafından oluşturulan radyo dalgalarından enerji alır ve RFID okuyucuya doğru bir sinyal iletir.

## <a name="ble-tags"></a>BLE etiketleri
Enerji sinyali, veri paketlerini düzenli aralıklarla yayınlar. İşaret verileri BLE okuyucular veya akıllı telefonlarda yüklü hizmetler tarafından algılanır ve bunu buluta iletir.

## <a name="rfid--ble-readers"></a>RFID & BLE okuyucular
RFID okuyucu, radyo dalgalarını daha kullanılabilir bir veri biçimine dönüştürür. Etiketlerden toplanan bilgiler daha sonra yerel kenar sunucusunda depolanır veya MQTT üzerinden JSON-RPC 2.0 kullanılarak buluta gönderilir.
Erişim Noktaları (AP) olarak da bilinen BLE okuyucu, RFID okuyucuya benzer. Yakındaki Bluetooth sinyallerini algılamak ve mqtt üzerinden JSON-RPC 2.0 kullanarak iletisini yerel Azure IoT Edge veya buluta iletmek için kullanılır.
Birçok okuyucu RFID ve işaret sinyallerini okuma ve sıcaklık, nem, ivmeölçer ve jiroskop ile ilgili ek sensör yeteneği sağlama yeteneğine sahiptir.

## <a name="azure-iot-edge-gateway"></a>Azure IoT Edge ağ geçidi
Azure IoT Edge sunucusu, bu verileri buluta göndermeden önce yerel olarak önceden işlemek için bir yer sağlar. Ayrıca bulut iş yüklerini yapay zeka, Azure ve üçüncü taraf hizmetleri, iş mantığı nı standart kapsayıcılar kullanarak dağıtabiliriz.

## <a name="device-management-with-iot-central"></a>IoT Central ile cihaz yönetimi 
Azure IoT Central, IoT aygıt bağlantısını, yapılandırmayı ve yönetimini basitleştiren bir çözüm geliştirme platformudur. Platform, IoT cihaz yönetimi, operasyonları ve ilgili gelişmelerin yükünü ve maliyetlerini önemli ölçüde azaltır. İş ortakları & müşteriler, stok yönetiminde dijital geri bildirim döngüsü elde etmek için son dan sona kurumsal çözümler oluşturabilir.

## <a name="business-insights--actions-using-data-egress"></a>Veri çıkışlarını kullanarak iş öngörüleri & eylemler 
IoT Merkezi platformu, Sürekli Veri Aktarım (CDE) ve API'ler aracılığıyla zengin genişletilebilirlik seçenekleri sağlar. Telemetri veri işleme veya ham telemetriye dayalı iş öngörüleri genellikle tercih edilen bir iş yeri uygulamasına aktarılır. Webhook, servis veri merkezi, etkinlik merkezi veya blob depolama sı kullanılarak, makine öğrenimi modellerini oluşturmak, eğitmek ve dağıtmak & öngörüleri daha da zenginleştirmek sağlanabilir.

## <a name="next-steps"></a>Sonraki adımlar
* Akıllı envanter [yönetimi şablonu](./tutorial-iot-central-smart-inventory-management-pnp.md) nasıl dağıtılayacaklarını öğrenin
* [IoT Central perakende şablonları](./overview-iot-central-retail-pnp.md) hakkında daha fazla bilgi edinin
* IoT Central hakkında daha fazla bilgi için [IoT Central genel bakış](../core/overview-iot-central.md)
