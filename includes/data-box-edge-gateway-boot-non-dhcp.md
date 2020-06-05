---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: 880b630ae48eda086f6454f0d7108d27d3403b77
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "67189039"
---
DHCP olmayan bir ortamda önyükleme yaparsanız, Data Box Gateway sanal makineyi dağıtmak için aşağıdaki adımları izleyin.

1. [Cihazın Windows PowerShell arabirimine bağlanın](#connect-to-the-powershell-interface).
2. `Get-HcsIpAddress`Sanal cihazınızda etkin olan ağ arabirimlerini listelemek için cmdlet 'ini kullanın. Cihazınızda tek bir ağ arabirimi varsa `Ethernet` varsayılan adı atanır.

    Aşağıdaki örnek, bu cmdlet 'in kullanımını gösterir:

    ```
    [10.100.10.10]: PS>Get-HcsIpAddress

    OperationalStatus : Up
    Name              : Ethernet
    UseDhcp           : True
    IpAddress         : 10.100.10.10
    Gateway           : 10.100.10.1
    ```

3. Ağı yapılandırmak için `Set-HcsIpAddress` cmdlet'ini kullanın. Aşağıdaki örneğe bakın:

    ```
    Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1
    ```

