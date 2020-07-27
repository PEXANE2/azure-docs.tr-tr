---
title: Doğrudan yöntemleri çağırmak için Azure portal kullanma
description: Bu makale, doğrudan yöntemleri çağırmak için Azure portal kullanan bir genel bakıştır.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.subservice: ''
ms.workload: ''
ms.topic: tutorial
ms.custom: ''
ms.date: 07/21/2020
ms.author: inhenkel
ms.openlocfilehash: 1fa25210bc8e4dc9f20e49e90b8e71082edb12c2
ms.sourcegitcommit: d7bd8f23ff51244636e31240dc7e689f138c31f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87172743"
---
# <a name="tutorial-use-azure-portal-to-invoke-direct-methods"></a>Öğretici: doğrudan yöntemleri çağırmak için Azure portal kullanma

IoT Hub, uç cihazlarda buluttan [doğrudan Yöntemler](/azure/iot-hub/iot-hub-devguide-direct-methods#method-invocation-for-iot-edge-modules) çağırma olanağı sağlar. IoT Edge (LVA) modülündeki canlı video analizi, canlı videoyu çözümlemek üzere farklı iş akışlarını tanımlamak, dağıtmak ve örneklemek için kullanılabilecek çeşitli [doğrudan Yöntemler](/azure/media-services/live-video-analytics-edge/direct-methods) sunar.

Bu öğreticide, Azure portal aracılığıyla bir IoT Edge modülü için canlı video analiziyle doğrudan Yöntem çağrılarını çağırmayı öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

* Uç cihazınızda çalışan IoT Edge modülde canlı video analizinden yararlanarak [hızlı başlangıç: canlı video analizi IoT Edge](/azure/media-services/live-video-analytics-edge/get-started-detect-motion-emit-events-quickstart) veya portalı kullanma ' da açıklanan yöntemleri kullanabilirsiniz [.](/azure/media-services/live-video-analytics-edge/deploy-iot-edge-device)

* [Canlı video analizlerini](/azure/media-services/live-video-analytics-edge/overview) ve [medya grafiği kavramını](/azure/media-services/live-video-analytics-edge/media-graph-concept)anlamış olursunuz.

## <a name="invoking-direct-methods-via-azure-portal"></a>Azure portal aracılığıyla doğrudan Yöntemler çağırma

LVA modülü tarafından kullanıma sunulan [doğrudan yöntemlerin](/azure/media-services/live-video-analytics-edge/direct-methods) her biri Azure Portal aracılığıyla çağrılabilir. Aşağıdaki adımlarda, bir doğrudan yöntem için ayrıntılar sağlanmaktadır. Benzer adımları kullanarak diğer doğrudan yöntemleri çağırabilirsiniz. Ancak, her bir doğrudan yöntem için belirli bir JSON gövdesi gerekir.

`GraphTopologyList`IoT Edge modülündeki canlı video analizi üzerinde şu anda dağıtılmış olan tüm grafik topolojilerinin bir listesini almak için yöntem çağrısını kullanın. Bu doğrudan yöntemi çağırmak için aşağıdaki adımları kullanın:

1. Azure portal oturum aç
1. IoT Hub bulmak için Portal giriş sayfasından ilgili kaynak grubunu bulun veya IoT Hub biliyorsanız, seçin.
    ![Portal giriş sayfasında kaynak grubu](media/use-azure-portal-to-invoke-directs-methods/portal-rg-home.png)
1. IoT Hub sayfasından bir kez, çeşitli cihaz kimliklerini listelemek için otomatik cihaz yönetimi altında IoT Edge ' i seçin. Cihazda çalışan modülleri listelemek için ilgili cihaz KIMLIĞINI seçin.
    ![IoT Hub sayfası](media/use-azure-portal-to-invoke-directs-methods/iot-hub-page.png)
1. Yapılandırma sayfasını açmak için IoT Edge modüldeki canlı video analizi ' ni seçin.<br><br>
    ![Yapılandırma sayfasını açmak için IoT Edge modüldeki canlı video analizlerini seçin](media/use-azure-portal-to-invoke-directs-methods/modules.png)
1. Doğrudan yöntem menü seçeneğini belirleyin. <br><br>
    ![Doğrudan yöntem menü seçeneğine tıklayın](media/use-azure-portal-to-invoke-directs-methods/module-details.png)
    > [!NOTE]
    > Geçerli sayfada görebileceğiniz gibi, bağlantı dizesi bölümlerine bir değer eklemeniz gerekebilir. Ayar adı bölümünde herhangi bir şeyi gizlemenize veya değiştirmenize gerek yoktur. Bu, herkese açık olmasına izin vermek için Tamam.

1. **Yöntem adı** alanına *Graphtopologyılist* yazın.
1. Aşağıdaki JSON 'ı kopyalayıp **Yük** alanına yapıştırın.
    ```json
    {
    "@apiVersion":
    }
    ```
1. Sayfanın üst kısmındaki **yöntemi çağır** düğmesini seçin.<br><br>
    ![Invoke yöntemi düğmesi](media/use-azure-portal-to-invoke-directs-methods/direct-method.png)
1. **Sonuç** alanında bir durum 200 iletisi görmeniz gerekir.<br><br>
    ![bağlantı zaman aşımı](media/use-azure-portal-to-invoke-directs-methods/connection-timeout.png)

## <a name="responses"></a>Yanıtlar

| Koşul             | Durum Kodu | Ayrıntılı hata kodu |
|-----------------------|-------------|---------------------|
| Başarılı               | 200         | Yok                 |
| Genel Kullanıcı hataları   | 400 aralığı   |                     |
| Genel sunucu hataları | 500 aralığı   |                     |

## <a name="next-steps"></a>Sonraki adımlar

[Doğrudan Yöntemler](/azure/media-services/live-video-analytics-edge/direct-methods) sayfasında daha doğrudan Yöntemler bulunabilir.

> [!NOTE]
> Grafik örneği belirli bir topolojiyi örnekledi, bu nedenle lütfen bir grafik örneği oluşturmadan önce doğru topoloji ayarlamış olduğunuzdan emin olun.

[Hızlı başlangıç: hareket yayma olaylarını algılama](/azure/media-services/live-video-analytics-edge/get-started-detect-motion-emit-events-quickstart) , doğrudan yöntem çağrılarının tam sırasını anlamak için iyi bir başvurudur.
