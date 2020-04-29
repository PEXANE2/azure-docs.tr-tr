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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "77111201"
---
[İleti yönlendirme](../articles/iot-hub/iot-hub-devguide-messages-d2c.md) , IoT cihazlarınızdaki telemetri verilerini, yerleşik Olay Hub 'ı ile uyumlu uç noktalara veya blob depolama, Service Bus kuyrukları, Service Bus konuları ve Event Hubs gibi özel uç noktalara göndermeye olanak sağlar. Özel ileti yönlendirmeyi yapılandırmak için, belirli bir koşulla eşleşen yolu özelleştirmek üzere [yönlendirme sorguları](../articles/iot-hub/iot-hub-devguide-routing-query-syntax.md) oluşturursunuz. Ayarlandıktan sonra, gelen veriler IoT Hub'ı tarafından otomatik olarak uç noktalara yönlendirilir. Bir ileti tanımlanmış yönlendirme sorgularının hiçbiriyle eşleşmezse, varsayılan uç noktaya yönlendirilir.

Bu 2 parçalı öğreticide, IoT Hub ile bu özel yönlendirme sorgularını ayarlamayı ve kullanmayı öğreneceksiniz. Bir IoT cihazından gelen iletileri BLOB depolama ve Service Bus kuyruğu dahil olmak üzere birden çok uç noktasından birine yönlendirdiğinizde. Service Bus kuyruğuna iletiler bir mantıksal uygulama tarafından çekilir ve e-posta ile gönderilir. Özel ileti yönlendirmesi tanımlı olmayan iletiler varsayılan uç noktaya gönderilir, ardından Azure Stream Analytics tarafından çekilir ve bir Power BI görselleştirmesinde görüntülenir.

Bu öğreticinin 1. ve 2. kısmını tamamlamak için aşağıdaki görevleri gerçekleştirirsiniz:

**Bölüm ı: kaynak oluşturma, ileti yönlendirmeyi ayarlama**
> [!div class="checklist"]
> * Kaynakları oluşturma--bir IoT Hub, bir depolama hesabı, Service Bus kuyruğu ve sanal cihaz. Bu, Azure portal, bir Azure Resource Manager şablonu, Azure CLı veya Azure PowerShell kullanılarak yapılabilir.
> * Depolama hesabı ve Service Bus kuyruğu için IoT Hub uç noktaları ve ileti yollarını yapılandırın.

**Bölüm II: hub 'a ileti gönderme, yönlendirilmiş sonuçları görüntüleme**
> [!div class="checklist"]
> * Service Bus kuyruğuna ileti eklendiğinde tetiklenen ve e-posta gönderen bir Mantıksal Uygulama oluşturma.
> * Farklı yönlendirme seçenekleri için hub'a iletiler gönderen bir IoT Cihazının simülasyonu olan bir uygulama indirme ve çalıştırma.
> * Varsayılan uç noktaya gönderilen veriler için bir Power BI görselleştirmesi oluşturun.
> * Service Bus kuyruğunda ve e-postalarda ...
> * Depolama hesabında ...
> * PowerBI görselleştirmesinde ...
> * ...sonuçları görüntüleyin.

## <a name="prerequisites"></a>Ön koşullar

* Bu öğreticinin 1. bölümü için:
  - Bir Azure aboneliğiniz olmalıdır. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

* Bu öğreticinin 2. bölümü için:
  - Bu öğreticinin 1. kısmını tamamlamış ve kaynakları hala kullanılabilir durumda olmalıdır.
  - [Visual Studio 'yu](https://www.visualstudio.com/)yükler.
  - Varsayılan uç noktanın Stream Analytics 'i çözümlemek için bir Power BI hesabına erişiminiz vardır. ([Power BI'ı ücretsiz deneyin](https://app.powerbi.com/signupredirect?pbi_source=web).)
  - Bildirim e-postaları göndermek için Office 365 hesabına sahip olmanız gerekir.
  - Güvenlik duvarınızdaki 8883 numaralı bağlantı noktasını açık olduğundan emin olun. Bu öğreticideki örnek, 8883 bağlantı noktası üzerinden iletişim kuran MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağ ortamlarında engellenebilir. Bu sorunu geçici olarak çözmek için daha fazla bilgi ve IoT Hub bkz. [bağlanma (MQTT)](../articles/iot-hub/iot-hub-mqtt-support.md#connecting-to-iot-hub).

[!INCLUDE [cloud-shell-try-it.md](cloud-shell-try-it.md)]
