---
title: Mağaza Analitiği Mimarisi
description: IoT Central'da Checkout uygulama şablonu kullanarak mağaza içi analiz uygulaması oluşturmayı öğrenin
author: avneets
ms.author: avneets
ms.date: 10/13/2019
ms.topic: overview
ms.service: iot-central
ms.subservice: iot-central-retail
services: iot-central
manager: eliotgra
ms.openlocfilehash: 6c2514bd078cc3feee4bd2802cf314079b824311
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77022129"
---
# <a name="in-store-analytics-architecture"></a>Mağaza içi analitik mimarisi



Mağaza içi analiz çözümleri, perakende mağaza ortamındaki çeşitli koşulları izlemenize olanak sağlar. Bu çözümler, IoT Central'daki uygulama şablonlarından biri ve aşağıdaki mimari kullanılarak kılavuz olarak oluşturulabilir.


![Azure IoT Merkezi Mağaza Analitiği](./media/architecture/store-analytics-architecture-frame.png)

- Bir ağ geçidi aygıtına telemetri verileri gönderen IoT sensörleri kümesi
- IoT Central'a telemetri ve toplu kavrayış gönderen ağ geçidi cihazları
- Manipülasyon için istenilen Azure hizmetine sürekli veri aktarımı
- Veriler istenilen biçimde yapılandırılabilir ve bir depolama hizmetine gönderilebilir
- İş uygulamaları verileri sorgulayabilir ve perakende işlemlerine güç veren öngörüler oluşturabilir
 
Genellikle mağaza içi analiz çözümünde rol oynayan önemli bileşenlere bir göz atalım.

## <a name="condition-monitoring-sensors"></a>Durum izleme sensörleri

IoT çözümü, bir perakende mağaza ortamından anlamlı sinyaller yakalayan bir dizi sensörle başlar. Yukarıdaki mimari diyagramın en solundaki farklı sensörler tarafından yansıtılır.

## <a name="gateway-devices"></a>Ağ geçidi aygıtları

Birçok IoT sensörü ham sinyalleri doğrudan buluta veya yakınlarında bulunan bir ağ geçidi cihazına besleyebilir. Ağ geçidi aygıtı, bir IoT Merkezi uygulamasına özet öngörüler göndermeden önce kenarda veri toplama gerçekleştirir. Ağ geçidi cihazları, gerektiğinde komuta ve kontrol işlemlerini sensör aygıtlarına aktarmakla da yükümlüdür. 

## <a name="iot-central-application"></a>IoT Merkezi uygulaması

Azure IoT Merkezi uygulaması, perakende mağaza ortamındaki farklı IoT sensörleri ve ağ geçidi aygıtlarından gelen verileri alır ve bir dizi anlamlı öngörü oluşturur.

Azure IoT Central, mağaza operatörüne altyapı aygıtlarını uzaktan izlemelerini ve yönetmelerini sağlayan özel bir deneyim de sunar.

## <a name="data-transform"></a>Veri dönüşümü
Bir çözümdeki Azure IoT Merkezi uygulaması, veri işleme gerçekleştirebilen ve bu öngörüleri bir işletmeye çıkarmadan önce zenginleştirebilen bir dizi Azure PaaS (Hizmet Olarak Platform) hizmetine ham veya toplu öngörüler dışa aktaracak şekilde yapılandırılabilir Uygulama. 

## <a name="business-application"></a>İş başvurusu
IoT verileri, perakende ortamında dağıtılan farklı türdeki iş uygulamalarına güç sağlamak için kullanılabilir. Bir perakende mağaza yöneticisi veya personel, iş öngörülerini görselleştirmek ve gerçek zamanlı olarak anlamlı eylemlerde bulunmak için bu uygulamaları kullanabilir. Perakende ekibiniz için gerçek zamanlı bir Power BI panosu oluşturmayı öğrenmek için [öğreticiyi](./tutorial-in-store-analytics-create-app-pnp.md)izleyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Mağaza İçi Analitik Ödeme](https://aka.ms/checkouttemplate) ve Mağaza [İçi Analitik Durum İzleme](https://aka.ms/conditiontemplate) uygulama şablonları ile başlayın. 
* Mağaza İçi Analitik uygulama şablonlarından birini kullanarak bir çözüm oluşturmakonusunda size yol gösteren [sondan uca öğreticiye](https://aka.ms/storeanalytics-tutorial) bir göz atın.
