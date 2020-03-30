---
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: include
ms.date: 07/11/2019
ms.author: alkohli
ms.openlocfilehash: 4a3925752d1af5e43d5984b06c0a68aa9faa214b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67839756"
---
| Liman no.| Veya dışarıda | Bağlantı noktası kapsamı| Gerekli| Notlar |   |
|--------|-----|-----|-----------|----------|-----------|
| TCP 80 (HTTP)|İçindeki|LAN|Evet|Bu bağlantı noktası HTTP üzerinden Veri Kutusu Blog depolama REST API'lerine bağlanmak için kullanılır. REST API'lerine bağlanmıyorsanız, bu otomatik olarak 8443 üzerinden yerel web UI'ye yönlendirir. |
| TCP 443 (HTTPS)|İçindeki|LAN|Evet|Bu bağlantı noktası, HTTPS üzerinden Data Box Blog depolama REST API'lerine bağlanmak için kullanılır. REST API'lerine bağlanmıyorsanız, bu otomatik olarak 8443 üzerinden yerel web UI'ye yönlendirir. |
| TCP 8443 (HTTPS-Alt)|İçindeki|LAN|Evet|Bu, HTTPS için alternatif bir bağlantı noktasıdır ve aygıt yönetimi için yerel web Web Web'e bağlanırken kullanılır. |
| TCP 445 (Kobİ)|Çıkış/İç|LAN|Bazı durumlarda<br>Notlara bakın|Bu bağlantı noktası yalnızca SMB üzerinden bağlantı kuruyorsanız gereklidir. |
| TCP 2049 (NFS)|Çıkış/İç|LAN|Bazı durumlarda<br>Notlara bakın|Bu bağlantı noktası yalnızca NFS üzerinden bağlantı kuruyorsanız gereklidir. |
| TCP 111 (NFS)|Çıkış/İç|LAN|Bazı durumlarda<br>Notlara bakın|Bu bağlantı noktası rpcbind/port eşleme için kullanılır ve yalnızca NFS üzerinden bağlantı kuruyorsanız gereklidir.  |
