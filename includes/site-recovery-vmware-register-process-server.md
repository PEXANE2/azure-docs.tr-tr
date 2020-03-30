---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 04/28/2019
ms.author: raynew
ms.openlocfilehash: 088cd5447b1f96dbf172b5918c29e4f3293289a6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67534737"
---
1. İşlem sunucusunu çalıştıran makineye uzak masaüstü bağlantısı kurun. 
2. Azure Site Kurtarma İşlemi Sunucusu yapılandırma aracını başlatmak için cspsconfigtool.exe'yi çalıştırın.
    - İşlem sunucusunda ilk oturum açĞınızda araç otomatik olarak başlatılır.
    - Otomatik olarak açılmıyorsa, masaüstündeki kısayolu tıklatın.

3. **Configuration server FQDN veya**IP'de, işlem sunucusunu kaydetmek için yapılandırma sunucusunun adını veya IP adresini belirtin.
4. **Configuration Server Port'ta**443'ün belirtildiğinden emin olun. Bu, yapılandırma sunucusunun istekleri dinlediği bağlantı noktasıdır.
5. **Connection Passphrase'de,** yapılandırma sunucusunu ayarlarken belirttiğiniz parolayı belirtin. Parolayı bulmak için:
    -  Yapılandırma sunucusunda, Site Kurtarma yükleme klasörüne **\home\svssystems\bin\**:
    ```
    cd %ProgramData%\ASR\home\svsystems\bin
    ```
    - Geçerli parolayı yazdırmak için aşağıdaki komutu çalıştırın:
    ```
    genpassphrase.exe -n
    ```

6. **Veri Aktarım Bağlantı**Noktası'nda, özel bir bağlantı noktası belirtmediğiniz sürece varsayılan değeri bırakın.

7. Ayarları **kaydet'i kaydet'i** tıklatın ve işlem sunucusunu kaydedin.

    
    ![İşlem sunucusunu kaydetme](./media/site-recovery-vmware-register-process-server/register-ps.png)
