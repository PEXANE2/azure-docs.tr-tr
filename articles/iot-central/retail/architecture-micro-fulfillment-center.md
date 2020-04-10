---
title: Azure IoT Merkezi mikro-karşılama merkezi | Microsoft Dokümanlar
description: IoT Central'daki Mikro-karşılama merkezi uygulama şablonumuzu kullanarak bir mikro-karşılama merkezi uygulaması oluşturmayı öğrenin
author: avneet723
ms.author: avneets
ms.date: 10/13/2019
ms.topic: overview
ms.service: iot-central
ms.subservice: iot-central-retail
services: iot-central
manager: eliotgra
ms.openlocfilehash: b0d030240ebe22886826b7a25bd5ca7b8f54e358
ms.sourcegitcommit: 25490467e43cbc3139a0df60125687e2b1c73c09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "81000430"
---
# <a name="micro-fulfillment-center-architecture"></a>Mikro-karşılama merkezi mimarisi

Mikro-karşılama merkezi çözümleri, güvenliği ve genel verimliliği artırırken kapalı kalma süresini ortadan kaldırarak maliyetleri azaltmak için tam otomatik bir karşılama merkezinin tüm yönlerini dijital olarak bağlamanızı, izlemenizi ve yönetmenize olanak sağlar. Bu çözümler, IoT Central'daki uygulama şablonlarından biri ve aşağıdaki mimari kullanılarak kılavuz olarak oluşturulabilir.

![Azure IoT Merkezi Mağaza Analitiği](./media/architecture/micro-fulfillment-center-architecture-frame.png)

- Bir ağ geçidi aygıtına telemetri verileri gönderen IoT sensörleri kümesi
- IoT Central'a telemetri ve toplu kavrayış gönderen ağ geçidi cihazları
- Manipülasyon için istenilen Azure hizmetine sürekli veri aktarımı
- Veriler istenilen biçimde yapılandırılabilir ve bir depolama hizmetine gönderilebilir
- İş uygulamaları verileri sorgulayabilir ve perakende işlemlerine güç veren öngörüler oluşturabilir
 
Genellikle bir mikro-yerine getirme merkezi çözümünde rol oynayan önemli bileşenlere bir göz atalım.

## <a name="robotic-carriers"></a>Robotik taşıyıcılar

Bir mikro-yerine getirme merkezi çözümü büyük olasılıkla telemetri sinyalleri farklı üreten robotik taşıyıcılar büyük bir dizi olacaktır. Bu sinyaller bir ağ geçidi aygıtı tarafından yutulabilir, toplanabilir ve mimari diyagramın sol tarafından yansıtılan IoT Central'a gönderilebilir.  

## <a name="condition-monitoring-sensors"></a>Durum izleme sensörleri

IoT çözümü, yerine getirme merkezinizin içinden anlamlı sinyaller yakalayan bir dizi sensörle başlar. Yukarıdaki mimari diyagramın en solundaki farklı sensörler tarafından yansıtılır.

## <a name="gateway-devices"></a>Ağ geçidi aygıtları

Birçok IoT sensörü ham sinyalleri doğrudan buluta veya yakınlarında bulunan bir ağ geçidi cihazına besleyebilir. Ağ geçidi aygıtı, bir IoT Merkezi uygulamasına özet öngörüler göndermeden önce kenarda veri toplama gerçekleştirir. Ağ geçidi cihazları, gerektiğinde komuta ve kontrol işlemlerini sensör aygıtlarına aktarmakla da yükümlüdür. 

## <a name="iot-central-application"></a>IoT Merkezi uygulaması

Azure IoT Central uygulaması, karşılama merkezi ortamındaki farklı IoT sensörleri, robotlar ve ağ geçidi aygıtlarından gelen verileri alır ve bir dizi anlamlı öngörü oluşturur.

Azure IoT Central, mağaza operatörüne altyapı aygıtlarını uzaktan izlemelerini ve yönetmelerini sağlayan özel bir deneyim de sunar.

## <a name="data-transform"></a>Veri dönüşümü
Bir çözümdeki Azure IoT Merkezi uygulaması, veri işleme gerçekleştirebilen ve bu öngörüleri bir iş uygulamasına çıkarmadan önce zenginleştirebilen bir dizi Azure PaaS (Hizmet Olarak Platform) hizmetine ham veya toplu öngörüler dışa aktaracak şekilde yapılandırılabilir. 

## <a name="business-application"></a>İş başvurusu
IoT verileri, perakende ortamında dağıtılan farklı türdeki iş uygulamalarına güç sağlamak için kullanılabilir. Bir yerine getirme merkezi yöneticisi veya çalışanı iş öngörülerini görselleştirmek ve gerçek zamanlı olarak anlamlı eylemlerde bulunmak için bu uygulamaları kullanabilir. Perakende ekibiniz için gerçek zamanlı bir Power BI panosu oluşturmayı öğrenmek için [öğreticiyi](./tutorial-in-store-analytics-create-app.md)izleyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Mikro-yerine getirme Merkezi](https://aka.ms/checkouttemplate) uygulama şablonu ile başlayın. 
* Micro-fulfillment Center uygulama şablonu kullanarak bir çözüm oluşturmak için size yol gösteren [öğretici](https://aka.ms/mfc-tutorial) bir göz atın.
