---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: 880b630ae48eda086f6454f0d7108d27d3403b77
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67189039"
---
DHCP olmayan bir ortamda önyükleme yapacaksanız, Veri Kutusu Ağ Geçidiniz için sanal makineyi dağıtmak için aşağıdaki adımları izleyin.

1. [Aygıtın Windows PowerShell arabirimine bağlanın.](#connect-to-the-powershell-interface)
2. Sanal `Get-HcsIpAddress` cihazınızda etkinleştirilen ağ arabirimlerini listelemek için cmdlet'i kullanın. Cihazınızda tek bir ağ arabirimi varsa `Ethernet` varsayılan adı atanır.

    Aşağıdaki örnek, bu cmdlet kullanımını gösterir:

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

