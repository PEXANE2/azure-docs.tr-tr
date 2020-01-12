---
title: Azure IoT Central Micro-Karşılama Merkezi | Microsoft Docs
description: IoT Central içinde mikro Karşılama Merkezi uygulama şablonumuzu kullanarak mikro Karşılama Merkezi uygulaması derlemeyi öğrenin
author: avneet723
ms.author: avneets
ms.date: 10/13/2019
ms.topic: overview
ms.service: iot-central
ms.subservice: iot-central-retail
services: iot-central
manager: eliotgra
ms.openlocfilehash: 35f99abaf5e0142c29d6dd43c968b66b21a28a50
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75899219"
---
# <a name="micro-fulfillment-center-architecture"></a>Mikro-karşılama merkezi mimarisi

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Mikro Karşılama Merkezi çözümleri, güvenlik ve genel verimliliği artırarak kapalı kalma süresini ortadan kaldırarak maliyetleri azaltmak için tamamen otomatik bir karşılama merkezinin tüm yönlerini dijital olarak bağlamanıza, izlemenize ve yönetmenize olanak tanır. Bu çözümler IoT Central içindeki uygulama şablonlarından biri ve kılavuz olarak aşağıdaki mimariden yararlanarak oluşturulabilir.

![Azure IoT Central Mağazası Analizi](./media/architecture/micro-fulfillment-center-architecture-frame.png)

- Bir ağ geçidi cihazına telemetri verileri gönderen IoT sensörleri kümesi
- IoT Central telemetri ve toplu Öngörüler gönderen ağ geçidi cihazları
- İşleme için istenen Azure hizmetine sürekli veri aktarma
- Veriler istenen biçimde yapılandırılabilir ve bir depolama hizmetine gönderilebilir
- İş uygulamaları, verileri sorgulayabilir ve Power Retail işlemleri ile ilgili öngörüler oluşturabilir
 
Genellikle, mikro Karşılama Merkezi çözümünde bir bölümü oynatacak anahtar bileşenlere göz atalım.

## <a name="robotic-carriers"></a>Robot taşıyıcılar

Mikro Karşılama Merkezi çözümü, büyük olasılıkla çeşitli telemetri sinyalleri oluşturan büyük bir robot taşıyıcılar kümesine sahip olacaktır. Bu sinyaller, bir ağ geçidi cihazı tarafından alınabilir, toplanır ve ardından mimari diyagramının sol tarafında yansıtıldığı gibi IoT Central gönderilir.  

## <a name="condition-monitoring-sensors"></a>Koşul izleme algılayıcılar

IoT çözümü, yerine getirme merkezinizden anlamlı sinyalleri yakalayan bir algılayıcı kümesiyle başlar. Yukarıdaki mimari diyagramının en solundaki çeşitli sensörlerle yansıtılır.

## <a name="gateway-devices"></a>Ağ geçidi cihazları

Birçok IoT algılayıcı, ham sinyalleri doğrudan buluta veya neredeyse bulunan bir ağ geçidi cihazına akışa alabilir. Ağ geçidi cihazı, bir IoT Central uygulamasına Özet Öngörüler göndermeden önce, uç sırada veri toplama işlemini gerçekleştirir. Ağ Geçidi cihazları, uygun olduğunda, algılayıcı cihazlarına geçiş komutu ve denetim işlemlerini de sorumludur. 

## <a name="iot-central-application"></a>IoT Central uygulaması

Azure IoT Central uygulaması, farklı IoT algılayıcılarının, robotların yanı sıra karşılama merkezi ortamındaki ağ geçidi cihazlarından verileri alır ve anlamlı bir Öngörüler kümesi oluşturur.

Azure IoT Central Ayrıca, altyapı cihazlarını uzaktan izleyip yönetebilmesini sağlayan mağaza işlecine özel bir deneyim sağlar.

## <a name="data-transform"></a>Veri dönüştürme
Bir çözüm içindeki Azure IoT Central uygulaması, ham veya toplanmış öngörüleri veri işleme gerçekleştirebilen ve bu öngörüleri bir işletmeye göre daha zengin bir şekilde zenginleştiren Azure PaaS (hizmet olarak platform) Hizmetleri kümesine dışarı aktarmak üzere yapılandırılabilir Uygulamanızı. 

## <a name="business-application"></a>İş uygulaması
IoT verileri, perakende ortamında dağıtılan çeşitli iş uygulamalarını desteklemek için kullanılabilir. Bir karşılama Merkezi Yöneticisi veya çalışanı, iş öngörülerini görselleştirmek ve gerçek zamanlı olarak anlamlı eylemler gerçekleştirmek için bu uygulamaları kullanabilir. Perakende ekibiniz için gerçek zamanlı Power BI panosu oluşturmayı öğrenmek için [öğreticiyi](./tutorial-in-store-analytics-create-app-pnp.md)izleyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Mikro Karşılama Merkezi](https://aka.ms/checkouttemplate) uygulama şablonunu kullanmaya başlayın. 
* Mikro Karşılama Merkezi uygulama şablonuyla bir çözüm oluşturma konusunda size yol gösteren [öğreticiye](https://aka.ms/mfc-tutorial) göz atın.
