---
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: include
ms.date: 09/06/2020
ms.author: alkohli
ms.openlocfilehash: b9ff5968b4bb406f1a96780985b5c6fe64ca976c
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "89505906"
---
| Bağlantı noktası No.| Dışarı veya dışarı | Bağlantı noktası kapsamı| Gerekli| Notlar |
|--------|-----|-----|-----------|----------|
| TCP 80 (HTTP)|İçinde|LAN|Yes|Bu bağlantı noktası, HTTP üzerinden Data Box BLOB depolama REST API 'Lerine bağlanmak için kullanılır. REST API 'Lerine bağlanmadığından, bu otomatik olarak 8443 üzerinden yerel Web Kullanıcı arabirimine yeniden yönlendirir. |
| TCP 443 (HTTPS)|İçinde|LAN|Yes|Bu bağlantı noktası, HTTPS üzerinden Data Box BLOB depolama REST API 'Lerine bağlanmak için kullanılır. REST API 'Lerine bağlanmadığından, bu otomatik olarak 8443 üzerinden yerel Web Kullanıcı arabirimine yeniden yönlendirir. |
| TCP 8443 (HTTPS-alt)|İçinde|LAN|Yes|Bu, HTTPS için alternatif bir bağlantı noktasıdır ve cihaz yönetimi için yerel Web Kullanıcı arabirimine bağlanırken kullanılır. |
| TCP 445 (SMB)|Out/ın|LAN|Bazı durumlarda<br>Notlara bakın|Bu bağlantı noktası yalnızca SMB aracılığıyla bağlanıyorsanız gereklidir. |
| TCP 2049 (NFS)|Out/ın|LAN|Bazı durumlarda<br>Notlara bakın|Bu bağlantı noktası yalnızca NFS aracılığıyla bağlanıyorsanız gereklidir. |
| TCP 111 (NFS)|Out/ın|LAN|Bazı durumlarda<br>Notlara bakın|Bu bağlantı noktası, rpcbind/Port eşlemesi için kullanılır ve yalnızca NFS aracılığıyla bağlanıyorsanız gereklidir.  |
