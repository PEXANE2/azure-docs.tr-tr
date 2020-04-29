---
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: include
ms.date: 07/11/2019
ms.author: alkohli
ms.openlocfilehash: 4a3925752d1af5e43d5984b06c0a68aa9faa214b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "67839756"
---
| Bağlantı noktası No.| Dışarı veya dışarı | Bağlantı noktası kapsamı| Gerekli| Notlar |   |
|--------|-----|-----|-----------|----------|-----------|
| TCP 80 (HTTP)|İçindeki|LAN|Yes|Bu bağlantı noktası, HTTP üzerinden blog depolama REST API 'Lerine Data Box bağlanmak için kullanılır. REST API 'Lerine bağlanmadığından, bu otomatik olarak 8443 üzerinden yerel Web Kullanıcı arabirimine yeniden yönlendirir. |
| TCP 443 (HTTPS)|İçindeki|LAN|Yes|Bu bağlantı noktası, HTTPS üzerinden blog depolama REST API 'Lerine Data Box bağlanmak için kullanılır. REST API 'Lerine bağlanmadığından, bu otomatik olarak 8443 üzerinden yerel Web Kullanıcı arabirimine yeniden yönlendirir. |
| TCP 8443 (HTTPS-alt)|İçindeki|LAN|Yes|Bu, HTTPS için alternatif bir bağlantı noktasıdır ve cihaz yönetimi için yerel Web Kullanıcı arabirimine bağlanırken kullanılır. |
| TCP 445 (SMB)|Out/ın|LAN|Bazı durumlarda<br>Notlara bakın|Bu bağlantı noktası yalnızca SMB aracılığıyla bağlanıyorsanız gereklidir. |
| TCP 2049 (NFS)|Out/ın|LAN|Bazı durumlarda<br>Notlara bakın|Bu bağlantı noktası yalnızca NFS aracılığıyla bağlanıyorsanız gereklidir. |
| TCP 111 (NFS)|Out/ın|LAN|Bazı durumlarda<br>Notlara bakın|Bu bağlantı noktası, rpcbind/Port eşlemesi için kullanılır ve yalnızca NFS aracılığıyla bağlanıyorsanız gereklidir.  |
