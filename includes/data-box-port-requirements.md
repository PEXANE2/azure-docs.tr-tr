---
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: include
ms.date: 07/11/2019
ms.author: alkohli
ms.openlocfilehash: 948f5399ceb931d701341c2cf378482ede94979d
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86200314"
---
| Bağlantı noktası No.| Dışarı veya dışarı | Bağlantı noktası kapsamı| Gerekli| Notlar |
|--------|-----|-----|-----------|----------|
| TCP 80 (HTTP)|İçinde|LAN|Evet|Bu bağlantı noktası, HTTP üzerinden blog depolama REST API 'Lerine Data Box bağlanmak için kullanılır. REST API 'Lerine bağlanmadığından, bu otomatik olarak 8443 üzerinden yerel Web Kullanıcı arabirimine yeniden yönlendirir. |
| TCP 443 (HTTPS)|İçinde|LAN|Evet|Bu bağlantı noktası, HTTPS üzerinden blog depolama REST API 'Lerine Data Box bağlanmak için kullanılır. REST API 'Lerine bağlanmadığından, bu otomatik olarak 8443 üzerinden yerel Web Kullanıcı arabirimine yeniden yönlendirir. |
| TCP 8443 (HTTPS-alt)|İçinde|LAN|Evet|Bu, HTTPS için alternatif bir bağlantı noktasıdır ve cihaz yönetimi için yerel Web Kullanıcı arabirimine bağlanırken kullanılır. |
| TCP 445 (SMB)|Out/ın|LAN|Bazı durumlarda<br>Notlara bakın|Bu bağlantı noktası yalnızca SMB aracılığıyla bağlanıyorsanız gereklidir. |
| TCP 2049 (NFS)|Out/ın|LAN|Bazı durumlarda<br>Notlara bakın|Bu bağlantı noktası yalnızca NFS aracılığıyla bağlanıyorsanız gereklidir. |
| TCP 111 (NFS)|Out/ın|LAN|Bazı durumlarda<br>Notlara bakın|Bu bağlantı noktası, rpcbind/Port eşlemesi için kullanılır ve yalnızca NFS aracılığıyla bağlanıyorsanız gereklidir.  |
