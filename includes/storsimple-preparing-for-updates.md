---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 4e262c9e5bb88e77bc9c09853c06f4cdb41eedaa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188564"
---
## <a name="preparing-for-updates"></a>Güncellemeler için hazırlanma
Güncelleştirmeyi tarayıp uygulamadan önce aşağıdaki adımları gerçekleştirmeniz gerekir:

1. Aygıt verilerinin bulut anlık görüntüsünü alın.
2. Denetleyicinizin sabit IP'lerinin çözülebilen olduğundan ve Internet'e bağlanadığından emin olun. Bu sabit IP'ler cihazınızdaki güncelleştirmeleri servis etmek için kullanılır. Aygıtın Windows PowerShell arabiriminden her kumandada aşağıdaki cmdlet'i çalıştırarak bunu test edebilirsiniz:
   
     `Test-Connection -Source <Fixed IP of your device controller> -Destination <Any IP or computer name outside of datacenter network>`
   
    **Sabit IP'ler Internet'e bağlanabildiği zaman Test-Bağlantı için örnek çıktı**

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

Bu el ile ön kontrolleri başarıyla tamamladıktan sonra, güncelleştirmeleri tarayıp yükleyebilirsiniz.

