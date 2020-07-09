---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 8c60e0275853f3c879db22f5414f0fbbbdb47b85
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86050459"
---
## <a name="preparing-for-updates"></a>Güncelleştirmeler için hazırlanma
Güncelleştirmeyi taramadan ve uygulamadan önce aşağıdaki adımları gerçekleştirmeniz gerekir:

1. Cihaz verilerinin bulut anlık görüntüsünü alın.
2. Denetleyicinizin sabit IP 'Lerinin yönlendirilebilir olduğundan ve Internet 'e bağlanabildiğinden emin olun. Bu sabit IP 'Ler, cihazınızdaki güncelleştirmelere hizmet vermek için kullanılacaktır. Bu, cihazın Windows PowerShell arabiriminden her bir denetleyicide aşağıdaki cmdlet 'i çalıştırarak test edebilirsiniz:
   
     `Test-Connection -Source <Fixed IP of your device controller> -Destination <Any IP or computer name outside of datacenter network>`
   
    **Sabit IP 'Lerin Internet 'e bağlanabilmesi durumunda Test-Connection için örnek çıkış**

    ```output
    Controller0>Test-Connection -Source 10.126.173.91 -Destination bing.com

    Source      Destination     IPV4Address      IPV6Address
    ----------------- -----------  -----------
    HCSNODE0  bing.com        204.79.197.200
    HCSNODE0  bing.com        204.79.197.200
    HCSNODE0  bing.com        204.79.197.200
    HCSNODE0  bing.com        204.79.197.200

    Controller0>Test-Connection -Source 10.126.173.91 -Destination  204.79.197.200

    Source      Destination       IPV4Address    IPV6Address
    ----------------- -----------  -----------
    HCSNODE0  204.79.197.200  204.79.197.200
    HCSNODE0  204.79.197.200  204.79.197.200
    HCSNODE0  204.79.197.200  204.79.197.200
    HCSNODE0  204.79.197.200  204.79.197.200
    ```

Bu el ile yapılan ön denetimleri başarıyla tamamladıktan sonra güncelleştirmeleri taramaya ve yüklemeye devam edebilirsiniz.

