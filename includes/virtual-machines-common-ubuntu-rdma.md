---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: d41b86b902d9a58b144e251e6922fbd95d459031
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67671187"
---
1. Dapl, rdmacm, ibverbs ve mlx4 yükleyin

   ```bash
   sudo apt-get update

   sudo apt-get install libdapl2 libmlx4-1

   ```

2. /etc/waagent.conf'ta, aşağıdaki yapılandırma satırlarını açıklamadan ederek RDMA'yı etkinleştirin. Bu dosyayı erişmek için kök erişiminiz gerekir.
  
   ```
   OS.EnableRDMA=y

   OS.UpdateRdmaDriver=y
   ```

3. /etc/security/limits.conf dosyasında KB'deki aşağıdaki bellek ayarlarını ekleyin veya değiştirin. Bu dosyayı erişmek için kök erişiminiz gerekir. Test amacıyla sınırsız memlock ayarlayabilirsiniz. Örneğin: `<User or group name>   hard    memlock   unlimited`.

   ```
   <User or group name> hard    memlock <memory required for your application in KB>

   <User or group name> soft    memlock <memory required for your application in KB>
   ```
  
4. Intel MPI Kitaplığını yükleyin. Ya satın almak ve Intel kütüphane [indirin](https://software.intel.com/intel-mpi-library/) veya [ücretsiz değerlendirme sürümünü](https://registrationcenter.intel.com/en/forms/?productid=1740)indirin.

   ```bash
   wget http://registrationcenter-download.intel.com/akdlm/irc_nas/tec/9278/l_mpi_p_5.1.3.223.tgz
   ```
 
   Yalnızca Intel MPI 5.x çalışma süreleri desteklenir.
 
   Yükleme adımları için [Intel MPI Kitaplık Yükleme Kılavuzu'na](https://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html)bakın.

5. Kök olmayan hata ayıklama işlemleri (Intel MPI'nin en son sürümleri için gerekli) için ptrace'i etkinleştirin.
 
   ```bash
   echo 0 | sudo tee /proc/sys/kernel/yama/ptrace_scope
   ```
