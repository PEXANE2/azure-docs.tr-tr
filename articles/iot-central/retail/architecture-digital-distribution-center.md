---
title: Mimarlık IoT Merkezi Dijital Dağıtım Merkezi | Microsoft Dokümanlar
description: IoT Central için Dijital Dağıtım Merkezi uygulama şablonu mimarisi
author: KishorIoT
ms.author: nandab
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: overview
ms.date: 10/20/2019
ms.openlocfilehash: b73d065b43aff5f9793e642a102ff8bb2a7be036
ms.sourcegitcommit: 25490467e43cbc3139a0df60125687e2b1c73c09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "81000451"
---
# <a name="architecture-of-iot-central-digital-distribution-center-application-template"></a>IoT Merkezi dijital dağıtım merkezi uygulama şablonu mimarisi



İş ortakları ve müşteriler, sondan uca **dijital dağıtım merkezi** çözümleri geliştirmek için aşağıdaki kılavuzu & uygulama şablonundan yararlanabilir.

> [!div class="mx-imgBorder"]
> ![dijital dağıtım merkezi](./media/concept-ddc-architecture/digital-distribution-center-architecture.png)

1. Bir ağ geçidi aygıtına telemetri verileri gönderen IoT sensörleri kümesi
2. IoT Central'a telemetri ve toplu kavrayış gönderen ağ geçidi cihazları
3. Veriler, manipülasyon için istenilen Azure hizmetine yönlendirilir
4. ASA veya Azure İşlevleri gibi Azure hizmetleri, veri akışlarını yeniden biçimlemek ve istenen depolama hesaplarına göndermek için kullanılabilir 
5. İşlenen veriler, ML veya toplu iş analizine dayanan ek içgörü geliştirmeleri için yakın gerçek zamanlı eylemler veya soğuk depolama için sıcak depolamada depolanır. 
6. Logic Apps, son kullanıcı iş uygulamalarında çeşitli iş akışlarına güç sağlamak için kullanılabilir

## <a name="details"></a>Ayrıntılar
Aşağıdaki bölümde kavramsal mimarinin her bölümü özetler

## <a name="video-cameras"></a>Video kameralar 
Video kameralar, bu dijital bağlantılı kurumsal ölçekli ekosistemin birincil sensörleridir. Makine öğrenimi ve yapay zekadaki gelişmeler, videonun yapılandırılmış verilere dönüştürülmesini ve buluta göndermeden önce kenarda işlenmesine olanak sağlar. Görüntüleri yakalamak, kameraya sıkıştırmak ve sıkıştırılmış verileri video analitiği için kenar bilgiişleminin üzerine göndermek veya sensördeki görüntüleri yakalamak için GigE görüntü kameralarını kullanmak ve daha sonra bu görüntüleri doğrudan Azure IoT Edge'e göndermek için IP kameralarını kullanabiliriz, bu görüntüler video analitiği boru hattında işlemeden önce sıkıştırır. 

## <a name="azure-iot-edge-gateway"></a>Azure IoT Edge Ağ Geçidi
"Sensörler olarak kameralar" ve kenar iş yükleri Azure IoT Edge tarafından yerel olarak yönetilir ve kamera akışı analitik ardışık işlem boru hattı tarafından işlenir. Azure IoT Edge'deki video analizi işleme ardışık adı, yanıt süresinin azalması, düşük bant genişliği tüketimi gibi birçok avantaj sağlar ve bu da hızlı veri işleme için düşük gecikme süresi sağlar. Buluta yalnızca en önemli meta veriler, öngörüler veya eylemler gönderilir. 

## <a name="device-management-with-iot-central"></a>IoT Central ile Cihaz Yönetimi 
Azure IoT Central, IoT aygıtını Azure IoT Edge ağ geçidi bağlantısı, yapılandırma ve yönetimi & kolaylaştıran bir çözüm geliştirme platformudur. Platform, IoT cihaz yönetimi, operasyonları ve ilgili gelişmelerin yükünü ve maliyetlerini önemli ölçüde azaltır. İş ortakları & müşteriler, dağıtım merkezlerinde dijital geri bildirim döngüsü elde etmek için sondan bir son kurumsal çözümler oluşturabilir.

## <a name="business-insights-and-actions-using-data-egress"></a>Veri çıkışlarını kullanarak İş Öngörüleri ve eylemleri 
IoT Merkezi platformu, Sürekli Veri Aktarım (CDE) ve API'ler aracılığıyla zengin genişletilebilirlik seçenekleri sağlar. Telemetri veri işleme veya ham telemetriye dayalı iş öngörüleri genellikle tercih edilen bir iş satırı uygulamasına aktarılır. Makine öğrenimi modellerini oluşturmak, eğitmek ve dağıtmak ve öngörüleri daha da zenginleştirmek için webhook, Service Bus, event hub veya blob depolama yoluyla elde edilebilir.

## <a name="next-steps"></a>Sonraki adımlar
* Dijital dağıtım [merkezi şablonu](./tutorial-iot-central-digital-distribution-center.md) nasıl dağıtılayacaklarını öğrenin
* [IoT Central perakende şablonları](./overview-iot-central-retail.md) hakkında daha fazla bilgi edinin
* IoT Central hakkında daha fazla bilgi için [IoT Central genel bakış](../core/overview-iot-central.md)
