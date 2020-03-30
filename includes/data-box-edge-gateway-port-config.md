---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/25/2019
ms.author: alkohli
ms.openlocfilehash: 98f9e0377e560fa0bba2fd470ff01431b2ed21d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188811"
---
| Liman no.| Veya dışarıda | Bağlantı noktası kapsamı| Gerekli|   Notlar |   |
|--------|-----|-----|-----------|----------|-----------|
| TCP 80 (HTTP)|Out|WAN |Hayır|Giden bağlantı noktası güncellemeleri almak için internet erişimi için kullanılır. <br>Giden web proxy kullanıcı yapılandırılabilir. |
| TCP 443 (HTTPS)|Out|WAN|Evet|Giden bağlantı noktası buluttaki verilere erişmek için kullanılır.<br>Giden web proxy kullanıcı yapılandırılabilir.|
| UDP 123 (NTP)|Out|WAN|Bazı durumlarda<br>Notlara bakın|Bu bağlantı noktası yalnızca internet tabanlı bir NTP sunucusu kullanıyorsanız gereklidir.  |   
| UDP 53 (DNS)|Out|WAN|Bazı durumlarda<br>Notlara bakın|Bu bağlantı noktası yalnızca internet tabanlı bir DNS sunucusu kullanıyorsanız gereklidir.<br>Yerel bir DNS sunucusu kullanmanızı öneririz. |
| TCP 5985 (WinRM)|Çıkış/İç|LAN|Bazı durumlarda<br>Notlara bakın|Bu bağlantı noktası, http üzerinden uzaktan PowerShell üzerinden cihaza bağlanmak için gereklidir.  |
| UDP 67 (DHCP)|Out|LAN|Bazı durumlarda<br>Notlara bakın|Bu bağlantı noktası yalnızca yerel bir DHCP sunucusu kullanıyorsanız gereklidir.  |
| TCP 80 (HTTP)|Çıkış/İç|LAN|Evet|Bu bağlantı noktası, yerel yönetim için aygıtta yerel UI için gelen bağlantı noktasıdır. <br>HTTP üzerinden yerel UI'ye erişmek otomatik olarak HTTPS'ye yönlendirilecektir.  |
| TCP 443 (HTTPS)|Çıkış/İç|LAN|Evet|Bu bağlantı noktası, yerel yönetim için aygıtta yerel UI için gelen bağlantı noktasıdır. |
| TCP 445 (Kobİ)|İçindeki|LAN|Bazı durumlarda<br>Notlara bakın|Bu bağlantı noktası yalnızca SMB üzerinden bağlanıyorsanız gereklidir. |
| TCP 2049 (NFS)|İçindeki|LAN|Bazı durumlarda<br>Notlara bakın|Bu bağlantı noktası yalnızca NFS üzerinden bağlanıyorsanız gereklidir. |
