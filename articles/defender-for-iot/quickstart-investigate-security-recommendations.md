---
title: 'Hızlı başlangıç: güvenlik önerilerini araştırın'
description: IoT güvenlik hizmeti için Defender ile ilgili güvenlik önerilerini araştırın.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/09/2020
ms.author: mlottner
ms.openlocfilehash: 859f1c4a1ed1b3d9139307c52f44a14e3089e31f
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90948023"
---
# <a name="quickstart-investigate-security-recommendations"></a>Hızlı başlangıç: güvenlik önerilerini araştırın


IoT için Defender 'ın zamanında analiz ve önerilmesine yönelik öneriler, güvenlik duruşunu artırmanın ve IoT çözümünüz genelinde saldırı yüzeyini azaltmanın en iyi yoludur.

Bu hızlı başlangıçta, her bir IoT güvenlik önerisi içinde sunulan bilgileri keşfedebilir ve risk düzeyini azaltmak için her bir öneri ve ilgili cihazın ayrıntılarının nasıl detayına ve nasıl kullanılacağını açıklayacağız.

Haydi başlayalım.

## <a name="investigate-new-recommendations"></a>Yeni önerileri araştır

IoT Hub öneriler listesi, IoT Hub için tüm toplu güvenlik önerilerini görüntüler.

1.  Azure portal, **IoT Hub**   yeni öneriler için araştırmak istediğiniz IoT Hub açın.

1.   **Güvenlik**   menüsünden **öneriler**' i seçin. IoT Hub yönelik tüm güvenlik önerileri ve **Yeni**bir bayrağıyla öneriler görüntülenir   , son 24 saat içindeki önerilerinizi işaretleyin. 

    [![IoT IÇIN ASC ile güvenlik önerilerini araştırın](media/quickstart/investigate-security-recommendations-inline.png)](media/quickstart/investigate-security-recommendations-expanded.png#lightbox)


1.  Öneri ayrıntılarını açmak ve ayrıntılara gitmek için listeden herhangi bir öneriyi seçip açın.

## <a name="security-recommendation-details"></a>Güvenlik önerisi ayrıntıları

Her toplanmış öneriyi açarak, bir önerisi tetikleyen her bir cihaz için ayrıntılı öneri açıklaması, düzeltme adımları, cihaz KIMLIĞI ' ni görüntüleyin. Ayrıca, Log Analytics kullanarak öneri önem derecesi ve doğrudan araştırma erişimi de görüntülenir.

1.   **IoT Hub**   \>  **güvenlik**   \>  **önerileri**   listesinden herhangi bir güvenlik önerisi seçin ve açın.

1.  Bu öneriyi toplama döneminde veren tüm cihazların öneri **açıklaması**, **önem derecesi**, **cihaz ayrıntılarını**gözden geçirin   . 

1.  Öneri özelliklerini inceledikten sonra, **manual remediation step**   öneriye neden olan sorunu düzeltmeye ve çözmeye yardımcı olması için el ile düzeltme adım yönergelerini kullanın. 

    [![IoT IÇIN ASC ile güvenlik önerilerini düzeltin](media/quickstart/remediate-security-recommendations-inline.png)](media/quickstart/remediate-security-recommendations-expanded.png#lightbox)


1.  Ayrıntıya gitme sayfasında istenen cihazı seçerek belirli bir cihazın öneri ayrıntılarını araştırın.

    [![IoT IÇIN ASC ile bir cihaz için belirli güvenlik önerilerini araştırın](media/quickstart/explore-security-recommendation-detail-inline.png)](media/quickstart/explore-security-recommendation-detail-expanded.png#lightbox)


1.  Daha fazla araştırma gerekiyorsa, bağlantıyı kullanarak **Log Analytics öneriyi araştırın**   . 


## <a name="next-steps"></a>Sonraki adımlar

Özel uyarılar oluşturmayı öğrenmek için bir sonraki makaleye ilerleyin...

> [!div class="nextstepaction"]
> [Özel uyarılar oluşturma](quickstart-create-custom-alerts.md)
