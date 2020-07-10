---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/25/2019
ms.author: alkohli
ms.openlocfilehash: 71460af42b4da97a578ae5a3e23a714577e71867
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86218223"
---
| Bağlantı noktası No.| Dışarı veya dışarı | Bağlantı noktası kapsamı| Gerekli|   Notlar |
|--------|-----|-----|-----------|----------|
| TCP 80 (HTTP)|Out|WAN |Hayır|Giden bağlantı noktası, güncelleştirmeleri almak için internet erişimi için kullanılır. <br>Giden Web proxy 'si Kullanıcı tarafından yapılandırılabilir. |
| TCP 443 (HTTPS)|Out|WAN|Evet|Giden bağlantı noktası, buluttaki verilere erişmek için kullanılır.<br>Giden Web proxy 'si Kullanıcı tarafından yapılandırılabilir.|
| UDP 123 (NTP)|Out|WAN|Bazı durumlarda<br>Notlara bakın|Bu bağlantı noktası yalnızca Internet tabanlı bir NTP sunucusu kullanıyorsanız gereklidir.  |   
| UDP 53 (DNS)|Out|WAN|Bazı durumlarda<br>Notlara bakın|Bu bağlantı noktası yalnızca Internet tabanlı bir DNS sunucusu kullanıyorsanız gereklidir.<br>Yerel bir DNS sunucusu kullanmanızı öneririz. |
| TCP 5985 (WinRM)|Out/ın|LAN|Bazı durumlarda<br>Notlara bakın|Bu bağlantı noktası, HTTP üzerinden uzak PowerShell aracılığıyla cihaza bağlanmak için gereklidir.  |
| UDP 67 (DHCP)|Out|LAN|Bazı durumlarda<br>Notlara bakın|Bu bağlantı noktası yalnızca yerel bir DHCP sunucusu kullanıyorsanız gereklidir.  |
| TCP 80 (HTTP)|Out/ın|LAN|Evet|Bu bağlantı noktası, yerel yönetim için cihazdaki yerel kullanıcı arabirimine yönelik gelen bağlantı noktasıdır. <br>HTTP üzerinden yerel kullanıcı arabirimine erişilmesi, otomatik olarak HTTPS 'ye yönlendirilir.  |
| TCP 443 (HTTPS)|Out/ın|LAN|Evet|Bu bağlantı noktası, yerel yönetim için cihazdaki yerel kullanıcı arabirimine yönelik gelen bağlantı noktasıdır. |
| TCP 445 (SMB)|İçinde|LAN|Bazı durumlarda<br>Notlara bakın|Bu bağlantı noktası yalnızca SMB aracılığıyla bağlanıyorsanız gereklidir. |
| TCP 2049 (NFS)|İçinde|LAN|Bazı durumlarda<br>Notlara bakın|Bu bağlantı noktası yalnızca NFS aracılığıyla bağlanıyorsanız gereklidir. |
