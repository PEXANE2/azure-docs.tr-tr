---
author: rothja
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 10/26/2018
ms.author: jroth
ms.openlocfilehash: 8b919608dfc562d8db77619d5215a6828a53a4aa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188313"
---
1. Uzak Masaüstü ile sanal makineye bağlıyken **Yapılandırma Yöneticisi**‘ni arayın:

    ![SSCM’yi Açma](./media/virtual-machines-sql-server-connection-tcp-protocol/sql-server-configuration-manager.png)

1. SQL Server Yapılandırma Yöneticisi’ndeki konsol bölmesinde **SQL Server Ağ Yapılandırması**’nı genişletin.

1. Konsol bölmesinde, **MSSQLSERVER için Protokoller'i** (varsayılan örnek adı) tıklatın. Ayrıntılar bölmesinde, **TCP'yi** sağ tıklatın ve zaten etkinleştirilemediyse **Etkinleştir'i** tıklatın.

    ![TCP’yi Etkinleştirme](./media/virtual-machines-sql-server-connection-tcp-protocol/enable-tcp.png)

1. Konsol bölmesinde **SQL Server Hizmetleri**’ne tıklayın. Ayrıntılar bölmesinde, **SQL Server *(örnek adı)*(varsayılan** örnek **SQL Server (MSSQLSERVER)**) seçeneğini tıklatın ve ardından SQL Server örneğini durdurmak ve yeniden başlatmak için **Yeniden Başlat'ı**tıklatın.

    ![Veritabanı Altyapısını Yeniden Başlatma](./media/virtual-machines-sql-server-connection-tcp-protocol/restart-sql-server.png)

1. SQL Server Yapılandırma Yöneticisi’ni kapatın.

SQL Server Veritabanı Altyapısı’nda protokolleri etkinleştirme hakkında daha fazla bilgi için bkz. [Sunucu Ağ Protokolünü Etkinleştirme veya Devre Dışı Bırakma](https://msdn.microsoft.com/library/ms191294.aspx).
