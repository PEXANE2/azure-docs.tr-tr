---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 02/11/2021
ms.author: alkohli
ms.openlocfilehash: b2c1ebe390b1a2dec7be678b5d6f3a991056a23b
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102517589"
---
1. Başlamadan önce aşağıdakilerden emin olun:

    1. [Azure Stack Edge Pro cihazınızı](../articles/databox-online/azure-stack-edge-gpu-deploy-activate.md) Azure 'Da Azure Stack Edge kaynağıyla yapılandırdık ve etkinleştirdiniz.
    1. [Bu cihazda işlem Azure Portal yapılandırdınız](../articles/databox-online/azure-stack-edge-deploy-configure-compute.md#configure-compute).
    
1. [PowerShell arabirimine bağlanın](../articles/databox-online/azure-stack-edge-gpu-connect-powershell-interface.md#connect-to-the-powershell-interface).
1. Cihazınızda MPS 'yi etkinleştirmek için aşağıdaki komutu kullanın.

    ```powershell
    Start-HcsGpuMPS
    ```