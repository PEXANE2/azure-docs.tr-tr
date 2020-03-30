---
title: Streç Veritabanı için Saydam Veri Şifrelemesini Etkinleştir
description: Azure'daki SQL Server Stretch Veritabanı için Saydam Veri Şifrelemesini (TDE) etkinleştirme
services: sql-server-stretch-database
documentationcenter: ''
ms.assetid: a44ed8f5-b416-4c41-9b1e-b7271f10bdc3
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/14/2016
author: blazem-msft
ms.author: blazem
ms.reviewer: jroth
manager: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: 465338bb793ad7b4fc406e9d1c073b9dc0dc06c8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74034006"
---
# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure"></a>Azure'da Streç Veritabanı için Saydam Veri Şifrelemesini (TDE) etkinleştirme
> [!div class="op_single_selector"]
> * [Azure portalında](sql-server-stretch-database-encryption-tde.md)
> * [Tsql](sql-server-stretch-database-tde-tsql.md)
>
>

Saydam Veri Şifreleme (TDE), uygulamada değişiklik gerektirmeden veritabanının, ilişkili yedeklemelerin ve işlem günlüğü dosyalarının gerçek zamanlı şifreleme ve şifre çözme işlemlerini gerçekleştirerek kötü amaçlı etkinlik tehdidine karşı korumaya yardımcı olur.

TDE, veritabanı şifreleme anahtarı olarak adlandırılan bir simetrik anahtarı kullanarak veritabanı depolama alanının tamamını şifreler. Veritabanı şifreleme anahtarı yerleşik bir sunucu sertifikası tarafından korunur. Yerleşik sunucu sertifikası her Azure sunucusu için benzersizdir. Microsoft bu sertifikaları en az 90 günde bir otomatik olarak döndürür. TDE'nin genel açıklaması için [bkz.]

## <a name="enabling-encryption"></a>Şifrelemeyi Etkinleştirme
Esneme etkin leştirilmiş bir SQL Server veritabanından geçirilen verileri depolayan bir Azure veritabanı için TDE'yi etkinleştirmek için aşağıdaki leri yapın:

1. [Azure portalında](https://portal.azure.com) veritabanını açma
2. Veritabanı bıçak, **Ayarlar** düğmesini tıklatın
3. Saydam **veri şifreleme** seçeneğini seçin![][1]
4. **Ayarı'nı** seçin ve sonra **Kaydet'i** seçin
   ![][2]

## <a name="disabling-encryption"></a>Şifrelemeyi Devre Dışı Bırakma
Esneme etkin leştirilmiş bir SQL Server veritabanından geçirilen verileri depolayan bir Azure veritabanı için TDE'yi devre dışı kılabilir, aşağıdaki leri yapın:

1. [Azure portalında](https://portal.azure.com) veritabanını açma
2. Veritabanı bıçak, **Ayarlar** düğmesini tıklatın
3. Saydam **veri şifreleme** seçeneğini seçin
4. **Kapalı** ayarını seçin ve sonra **Kaydet'i** seçin

<!--Anchors-->
[Saydam Veri Şifrelemesi (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->
[1]: ./media/sql-server-stretch-database-encryption-tde/stretchtde1.png
[2]: ./media/sql-server-stretch-database-encryption-tde/stretchtde2.png


<!--Link references-->
