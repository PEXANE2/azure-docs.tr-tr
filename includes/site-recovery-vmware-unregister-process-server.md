---
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: include
ms.date: 04/28/2019
ms.author: ramamill
ms.openlocfilehash: 00b0c1b1a40ad16db177916c57dba6e9d5a187a7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188899"
---
Özel durumunuz için adımları izleyin.

### <a name="unregister-a-connected-process-server"></a>Bağlı bir işlem sunucusunun kaydını kaldırın

1. Yönetici olarak işlem sunucusuna uzak bir bağlantı kurun.
2. Denetim **Paneli'nde,** Programları açın **> Programı Kaldır.**
3. Programı microsoft **Azure Site Kurtarma Mobilite Hizmeti/Master Target Server'ı**kaldırın.
4. Programı Microsoft **Azure Site Kurtarma Yapılandırma/İşlem Sunucusu'nu**kaldırın.
5. 3 ve 4 adımdaki programlar yüklendikten sonra **Microsoft Azure Site Kurtarma Yapılandırması/İşlem Sunucusu Bağımlılıklarını**kaldırın.

### <a name="unregister-a-disconnected-process-server"></a>Bağlantısı kesilen bir işlem sunucusunun kaydını kaldırın

İşlem sunucusunun yüklü olduğu makineyi yeniden canlandırmanın bir yolu yoksa bu adımları kullanın.

1. Yapılandırma sunucusunda Yönetici olarak oturum açın.
2. Yönetim komut istemini açın ve `%ProgramData%\ASR\home\svsystems\bin`''ye göz atın.
3. Bir veya daha fazla işlem sunucusunun listesini almak için bu komutu çalıştırın.

    ```
    perl Unregister-ASRComponent.pl -IPAddress <IP_of_Process_Server> -Component PS
    ```
    - S. No: işlem sunucusu seri numarası.
    - IP/Ad: İşlem sunucusunu çalıştıran makinenin IP adresi ve adı.
    - Sinyal: İşlem sunucusu makinesinden son sinyal.
    ![Kayıt dışı-cmd](media/site-recovery-vmware-unregister-process-server/Unregister-cmd.PNG)

4. Kaydını açmak istediğiniz işlem sunucusunun seri numarasını belirtin.
5. İşlem sunucusunun kaydını kaldırma sistemi tüm ayrıntılarını kaldırır ve iletiyi görüntüler: **Başarıyla kayıt dışı sunucu adı> (sunucu-IP adresi)**

