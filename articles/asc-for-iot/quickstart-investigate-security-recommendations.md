---
title: 'Hızlı başlangıç: güvenlik önerilerini araştırın'
description: IoT güvenlik hizmeti için Azure Güvenlik Merkezi ile güvenlik önerilerini araştırın.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/12/2020
ms.author: mlottner
ms.openlocfilehash: aa241a9108be32f88357065732c6f283d312be6c
ms.sourcegitcommit: a2a7746c858eec0f7e93b50a1758a6278504977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2020
ms.locfileid: "88144236"
---
# <a name="quickstart-investigate-security-recommendations"></a>Hızlı başlangıç: güvenlik önerilerini araştırın


IoT için Azure Güvenlik Merkezi 'Ndeki öneriler için zamanında analiz ve risk azaltma, güvenlik duruşunu artırmanın ve IoT çözümünüz genelinde saldırı yüzeyini azaltmanın en iyi yoludur.

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
