---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 04/28/2019
ms.author: raynew
ms.openlocfilehash: 088cd5447b1f96dbf172b5918c29e4f3293289a6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "67534737"
---
1. İşlem sunucusunu çalıştıran makineye Uzak Masaüstü Bağlantısı oluşturun. 
2. Azure Site Recovery Işlemi sunucu yapılandırma aracını başlatmak için Cspsconfigtool. exe ' yi çalıştırın.
    - Araç, işlem sunucusunda ilk kez oturum açtığınızda otomatik olarak başlatılır.
    - Otomatik olarak açılmazsa, masaüstündeki kısayoluna tıklayın.

3. **Yapılandırma sunucusu FQDN 'si veya IP**'de, işlem sunucusunun kaydedileceği yapılandırma sunucusunun adını veya IP adresini belirtin.
4. **Yapılandırma sunucusu bağlantı noktasında**443 belirtildiğinden emin olun. Bu, yapılandırma sunucusunun istekleri dinlediği bağlantı noktasıdır.
5. **Bağlantı parolası**' nda, yapılandırma sunucusunu ayarlarken belirttiğiniz parolayı belirtin. Parolayı bulmak için:
    -  Yapılandırma sunucusunda, Site Recovery yükleme klasörüne gidin **\ home\svssystems\bin\**:
    ```
    cd %ProgramData%\ASR\home\svsystems\bin
    ```
    - Geçerli parolayı yazdırmak için aşağıdaki komutu çalıştırın:
    ```
    genpassphrase.exe -n
    ```

6. **Veri aktarımı bağlantı noktası**' nda, özel bir bağlantı noktası belirtmediğiniz müddetçe varsayılan değeri bırakın.

7. **Kaydet** ' e tıklayarak ayarları kaydedin ve işlem sunucusunu kaydedin.

    
    ![İşlem sunucusunu kaydetme](./media/site-recovery-vmware-register-process-server/register-ps.png)
