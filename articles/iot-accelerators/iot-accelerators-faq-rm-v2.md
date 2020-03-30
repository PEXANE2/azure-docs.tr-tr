---
title: Uzaktan İzleme çözüm hızlandırıcıSSS - Azure | Microsoft Dokümanlar
description: Bu makalede, Uzaktan İzleme çözüm hızlandırıcıları için sık sorulan soruları yanıtlar.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 02/15/2018
ms.author: dobett
ms.openlocfilehash: c410ac7f41adb623a7198320a1edced097778569
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73826244"
---
# <a name="frequently-asked-questions-for-remote-monitoring-solution-accelerator"></a>Uzaktan İzleme çözüm hızlandırıcısı için sık sorulan sorular

Ayrıca bakınız, genel [SSS](iot-accelerators-faq.md).

### <a name="how-much-does-it-cost-to-provision-the-new-remote-monitoring-solution"></a>Yeni Uzaktan İzleme çözümünü sağlamak ne kadara mal olur?

Yeni çözüm hızlandırıcıiki dağıtım seçeneği sunar:

* Daha düşük geliştirme maliyeti arayan geliştiriciler veya bir demo veya kavram kanıtı oluşturmak isteyen müşteriler için tasarlanmış *temel* bir seçenek.
* Üretime hazır bir altyapı dağıtmak isteyen işletmeler için tasarlanmış *standart* bir seçenek.

### <a name="how-can-i-ensure-i-keep-my-costs-down-while-i-develop-my-solution"></a>Çözümümü geliştirirken maliyetlerimi düşük tuttuğumdan nasıl emin olabilirim?

İki farklı dağıtım sağlamanın yanı sıra, yeni Uzaktan İzleme çözümü, isteğe bağlı tüm simüle edilmiş aygıtları etkinleştirmek veya devre dışı kacak bir ayara sahiptir. Simülasyonun devre dışı bırakılması, çözümde alınan verileri ve dolayısıyla toplam maliyeti azaltır.

### <a name="what-is-the-difference-between-the-basic-and-standard-deployment-options-how-do-i-decide-between-the-two-deployment-options"></a>Temel ve standart dağıtım seçenekleri arasındaki fark nedir? İki dağıtım seçeneği arasında nasıl karar veririm?

Her dağıtım seçeneği farklı ihtiyaçlara yanıt verir. Temel dağıtım, PoC ve küçük pilotlar geliştirmek ve başlamak için tasarlanmıştır. Bu minimum gerekli kaynaklar ve daha düşük bir maliyet ile aerodinamik bir mimari sağlar. Standart dağıtım, üretime hazır bir çözüm oluşturmak ve özelleştirmek için tasarlanmıştır ve bunu gerçekleştirmek için gerekli öğeleri içeren bir dağıtım sağlar. Güvenilirlik ve ölçek için, uygulama mikrohizmetleri Docker kapsayıcıları olarak oluşturulur ve bir orkestratör (varsayılan olarak Kubernetes) kullanılarak dağıtılır. Orkestratör, uygulamanın dağıtımından, ölçeklendirilmesinden ve yönetiminden sorumludur. Mevcut gereksinimlerinize göre bir seçenek seçmelisiniz. Proje aşamanıza bağlı olarak birini, diğerini veya her ikisinin bir birleşimini kullanabilirsiniz.

### <a name="how-do-i-configure-a-dynamic-map-on-the-dashboard"></a>Panodaki dinamik bir haritayı nasıl yapılandırıyorum?

Daha fazla bilgi için, [dinamik bir haritadaki aygıtları görmek için Yükseltme haritası anahtarına](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#upgrade-map-key-to-see-devices-on-a-dynamic-map)bakın.

### <a name="where-can-i-find-information-about-the-previous-version-of-the-remote-monitoring-solution"></a>Uzaktan İzleme çözümünün önceki sürümü hakkında nerede bilgi bulabilirim?

Uzaktan İzleme çözüm hızlandırıcısının önceki sürümü, Önceden yapılandırılmış çözüm IoT Suite Uzaktan İzleme olarak biliniyordu. Arşivlenmiş belgeleri [https://docs.microsoft.com/previous-versions/azure/iot-suite/](https://docs.microsoft.com/previous-versions/azure/iot-suite/).

### <a name="next-steps"></a>Sonraki adımlar

IoT çözüm hızlandırıcılarının diğer özellik ve yeteneklerinden bazılarını da keşfedebilirsiniz:

* [Uzaktan İzleme çözüm hızlandırıcısının yeteneklerini keşfedin](quickstart-remote-monitoring-deploy.md)
* [Tahmine Dayalı Bakım çözüm hızlandırıcıya genel bakış](iot-accelerators-predictive-overview.md)
* [Bağlı Fabrika çözüm hızlandırıcıyı dağıtın](quickstart-connected-factory-deploy.md)
* [Baştan sona IoT güvenliği](/azure/iot-fundamentals/iot-security-ground-up)
