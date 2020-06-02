---
title: IoT Edge Azure 'da canlı video analizi nedir?
description: Bu konu, IoT Edge üzerindeki canlı video analizlerine genel bakış sağlar. Platform, IoT çözümlerinizi geliştirmek için kullanabileceğiniz özelliği sunar. Örneğin, canlı videoyu yakalayın, kaydedin, çözümleyin, Azure hizmetlerine sonuçları (video ve/veya video analizi) yayımlayın.
ms.topic: overview
ms.date: 05/27/2020
ms.openlocfilehash: 81a67322d0a5e524d75b9bf3e481be7157e09e63
ms.sourcegitcommit: 309cf6876d906425a0d6f72deceb9ecd231d387c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84266806"
---
# <a name="what-is-live-video-analytics-on-iot-edge-preview"></a>IoT Edge canlı video analizi nedir? (önizleme)

IoT Edge üzerindeki canlı video analizi, kenarı ve bulutu kapsayan akıllı video uygulamaları oluşturmak için bir platform sağlar. Platform, Azure hizmetlerine (bulutta ve/veya kenarda) sonuçları (video ve/veya video analizi) yayımlamayla birlikte canlı videoyu yakalama, kaydetme ve Çözümleme özelliği sunar. Platform, video analiziyle IoT çözümlerini geliştirmek için kullanılabilir. IoT Edge işlevselliğindeki canlı video analizi, IoT Edge üzerinde Stream Analytics ve IoT Edge, Olay Hub 'ı, bilişsel hizmetler gibi bulutta bulunan Azure hizmetlerinin yanı sıra güçlü karma (örneğin, Edge + bulut) uygulamaları oluşturmak için kullanabileceğiniz diğer Azure IoT Edge modülleriyle birleştirilebilir.

IoT Edge üzerindeki canlı video analizi, genişletilebilir bir platform olacak şekilde tasarlanmıştır. bu sayede, farklı video analizi kenar modüllerini (örneğin, açık kaynaklı makine öğrenimi modelleri veya kendi verilerinize göre tasarlanmış özel modeller) bağlayarak canlı videoyu analiz etmek ve canlı video işlem hattı oluşturma ve çalıştırma karmaşıklığı hakkında endişelenmeden bunları kullanabilirsiniz.

## <a name="accelerate-iot-solutions-development"></a>IoT çözümlerini geliştirmeyi hızlandırma 

Video analizlerini diğer IoT algılayıcılarının ve/veya iş verilerinin sinyalleriyle birleştiren IoT çözümleri, iş kararlarını otomatik hale getirmenize veya yarı otomatikleştirmenize yardımcı olabilir ve bu da üretkenlik geliştirmeleri elde edebilir. IoT Edge üzerinde canlı video analizi, bu tür çözümleri daha hızlı derlemenize olanak sağlar. İşletmenize özgü video analizi modüllerini ve mantığını oluşturmaya odaklanabilirsiniz ve platformun bir video işlem hattını yönetme ve çalıştırma karmaşıklıklarını gizlemesini sağlayabilirsiniz.

IoT Edge üzerinde canlı video analizi sayesinde, mevcut [video yönetim sistemlerinizle (VM 'ler)](https://en.wikipedia.org/wiki/Video_management_system) [CCTV kameralarınızı](https://en.wikipedia.org/wiki/Closed-circuit_television_camera) kullanmaya devam edebilir ve video analizi uygulamalarını bağımsız olarak oluşturabilirsiniz. IoT Edge üzerindeki canlı video analizi, son teknoloji IoT çözümleri oluşturmak için bilgisayar Vision SDK 'Ları ve araç setleriyle birlikte kullanılabilir. Aşağıdaki diyagramda bu gösterilmektedir.

![IoT Edge 'da canlı video analiziyle IoT çözümleri geliştirin](./media/overview/product-diagram.svg)

## <a name="supported-environments"></a>Desteklenen ortamlar

Linux AMD64 ve x64 ortamları desteklenir.

## <a name="get-started"></a>başlarken

Aşağıdaki kavram makalelerini okuyun ve ardından canlı bir video akışında hareket algılamayı çalıştırmak için hızlı başlangıç yapmayı deneyin.

### <a name="concepts"></a>Kavramlar

* [Medya grafiği](media-graph-concept.md)
* [Video kaydı](video-recording-concept.md)
* [Video kayıttan yürütme](video-playback-concept.md)
* [Sürekli video kaydı](continuous-video-recording-concept.md)
* [Olay tabanlı video kaydı](event-based-video-recording-concept.md)
* [Video kaydı olmadan canlı video analizi](analyze-live-video-concept.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç: canlı video analizinizi kendi modelinizle çalıştırın](use-your-model-quickstart.md) ve canlı video akışında hareket algılamayı nasıl çalıştıracağınızı öğrenin.
* [Terminolojiyi](terminology.md) gözden geçir
* [Canlı video analizi açık kaynak malzemesini](https://github.com/Azure/live-video-analytics) keşfet

