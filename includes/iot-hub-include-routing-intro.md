---
title: include dosyası
description: include dosyası
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 03/05/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 9a20dca71727e83db98c4c97567949bd127fc7fb
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77111201"
---
[İleti yönlendirme,](../articles/iot-hub/iot-hub-devguide-messages-d2c.md) IoT aygıtlarınızdan telemetri verilerinin yerleşik Event Hub uyumlu uç noktalarına veya blob depolama, Servis Veri Hizmetleri Kuyrukları, Servis Veri Hizmetleri Konuları ve Olay Hub'ları gibi özel uç noktalarına gönderilmesini sağlar. Özel ileti yönlendirmesini yapılandırmak için, belirli bir koşulla eşleşen yolu özelleştirmek için [yönlendirme sorguları](../articles/iot-hub/iot-hub-devguide-routing-query-syntax.md) oluşturursunuz. Ayarlandıktan sonra, gelen veriler IoT Hub'ı tarafından otomatik olarak uç noktalara yönlendirilir. İleti, tanımlanan yönlendirme sorgularından herhangi biri ile eşleşmiyorsa, varsayılan bitiş noktasına yönlendirilir.

Bu 2 bölümlü eğitimde, IoT Hub ile bu özel yönlendirme sorgularını nasıl ayarlayıp kullanacağınızı öğrenirsiniz. İletileri bir IoT aygıtından blob depolama ve Servis Veri Yolu kuyruğu da dahil olmak üzere birden çok uç noktadan birine yönlendirirsiniz. Servis Veri Servisi kuyruğuna gönderilen iletiler Bir Mantık Uygulaması tarafından alınır ve e-posta ile gönderilir. Tanımlanan özel ileti yönlendirmesi olmayan iletiler varsayılan bitiş noktasına gönderilir, ardından Azure Akışı Analitiği tarafından alınır ve Power BI görselleştirmesinde görüntülenir.

Bu öğreticinin Bölüm 1 ve 2'sini tamamlamak için aşağıdaki görevleri yerine getirirsiniz:

**Bölüm I: Kaynak oluşturma, ileti yönlendirmesi ayarlama**
> [!div class="checklist"]
> * Kaynakları oluşturun -- bir IoT hub'ı, depolama hesabı, Servis Veri Hizmeti veri sırası ve benzetimli aygıt. Bu işlem, Azure portalı, Azure Kaynak Yöneticisi şablonu, Azure CLI veya Azure PowerShell kullanılarak yapılabilir.
> * Depolama hesabı ve Servis Veri Yolu kuyruğu için IoT Hub'daki uç noktaları ve ileti yollarını yapılandırın.

**Bölüm II: Hub'a ileti gönderme, yönlendirilen sonuçları görüntüleme**
> [!div class="checklist"]
> * Service Bus kuyruğuna ileti eklendiğinde tetiklenen ve e-posta gönderen bir Mantıksal Uygulama oluşturma.
> * Farklı yönlendirme seçenekleri için hub'a iletiler gönderen bir IoT Cihazının simülasyonu olan bir uygulama indirme ve çalıştırma.
> * Varsayılan uç noktaya gönderilen veriler için bir Power BI görselleştirmesi oluşturun.
> * Service Bus kuyruğunda ve e-postalarda ...
> * Depolama hesabında ...
> * PowerBI görselleştirmesinde ...
> * ...sonuçları görüntüleyin.

## <a name="prerequisites"></a>Ön koşullar

* Bu öğretici Bölüm 1 için:
  - Bir Azure aboneliğiniz olmalıdır. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

* Bu öğretici Bölüm 2 için:
  - Bu öğreticinin Bölüm 1'ini tamamlamış ve kaynaklar hala kullanılabilir olmalıdır.
  - [Visual Studio'u](https://www.visualstudio.com/)yükleyin.
  - Varsayılan uç noktanın akış analizini analiz etmek için bir Power BI hesabına erişin. ([Power BI'ı ücretsiz deneyin](https://app.powerbi.com/signupredirect?pbi_source=web).)
  - Bildirim e-postaları göndermek için bir Office 365 hesabınız var.
  - 8883 bağlantı noktasının güvenlik duvarınızda açık olduğundan emin olun. Bu öğreticideki örnek, bağlantı noktası 8883 üzerinden iletişim sağlayan MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağı ortamlarında engellenebilir. Daha fazla bilgi ve bu sorunu çözmenin yolları için [IoT Hub'ına Bağlanma (MQTT)](../articles/iot-hub/iot-hub-mqtt-support.md#connecting-to-iot-hub)konusuna bakın.

[!INCLUDE [cloud-shell-try-it.md](cloud-shell-try-it.md)]
