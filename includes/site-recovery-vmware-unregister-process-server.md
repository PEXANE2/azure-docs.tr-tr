---
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: include
ms.date: 04/28/2019
ms.author: ramamill
ms.openlocfilehash: 00b0c1b1a40ad16db177916c57dba6e9d5a187a7
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "67188899"
---
Belirli koşullara ilişkin adımları izleyin.

### <a name="unregister-a-connected-process-server"></a>Bağlı bir işlem sunucusunun kaydını sil

1. İşlem sunucusuyla yönetici olarak uzak bağlantı kurun.
2. **Denetim Masası**'nda **Programlar > program Kaldır**' ı açın.
3. **Site Recovery Mobility hizmeti/ana hedef sunucusu Microsoft Azure**program kaldırın.
4. **Yapılandırma/Işlem sunucusu Site Recovery Microsoft Azure**programı kaldırın.
5. 3 ve 4. adımlarda bulunan programlar kaldırıldıktan sonra, **Microsoft Azure Site Recovery yapılandırma/Işlem sunucusu bağımlılıklarını**kaldırın.

### <a name="unregister-a-disconnected-process-server"></a>Bağlantısı kesilen bir işlem sunucusunun kaydını silme

Bu adımları yalnızca, işlem sunucusunun yüklü olduğu makinenin bir yolu yoksa kullanın.

1. Yapılandırma sunucusunda yönetici olarak oturum açın.
2. Bir yönetim komut istemi açın ve konumuna gidin `%ProgramData%\ASR\home\svsystems\bin`.
3. Bir veya daha fazla işlem sunucusunun listesini almak için bu komutu çalıştırın.

    ```
    perl Unregister-ASRComponent.pl -IPAddress <IP_of_Process_Server> -Component PS
    ```
    - S. Hayır: işlem sunucusu seri numarası.
    - IP/ad: işlem sunucusunu çalıştıran makinenin IP adresi ve adı.
    - Sinyal: işlem sunucusu makinesinden son sinyal.
    ![Kaydı sil-cmd](media/site-recovery-vmware-unregister-process-server/Unregister-cmd.PNG)

4. Kaldırmak istediğiniz işlem sunucusunun seri numarasını belirtin.
5. Bir işlem sunucusunun kaydını silme tüm ayrıntılarını sistemden kaldırın ve şu iletiyi görüntüler: **sunucu adı> başarıyla silindi (sunucu-IP-adresi)**

