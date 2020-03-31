---
title: Azure portalından bağlantı dizesini edinme
description: Azure portalından bağlantı dizesini edinme
keywords: sql bağlantısı,bağlantı dizesi
services: sql-database
author: dalechen
manager: craigg
ms.service: sql-database
ms.custom: develop apps
ms.topic: include
ms.date: 07/13/2018
ms.author: ninarn
ms.openlocfilehash: 6ead2e0ea326b5c3f2e76e7aa9cc4ab3c50d4154
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188451"
---
### <a name="obtain-the-connection-string-from-the-azure-portal"></a>Azure portalından bağlantı dizesini edinme
İstemci programınızın Azure SQL Veritabanı ile etkileşimkurabilmesi için gerekli olan bağlantı dizesini elde etmek için [Azure portalını](https://portal.azure.com/) kullanın.

1. **Tüm hizmetler** > **SQL veritabanlarını**seçin.

2. **Veritabanınızın** adını SQL veritabanları bıçağının sol üst yanındaki filtre metin kutusuna girin.

3. Veritabanınız için satırı seçin.

4. Bıçak veritabanınız için göründükten sonra, görsel kolaylık sağlamak için göz atma ve veritabanı filtreleme için kullandığınız bıçakları daraltmak için **Simge Durumuna Küçült** düğmelerini seçin.

5. Veritabanınızın bıçak üzerinde veritabanı **bağlantı dizelerini göster'i**seçin.

6. Uygun bağlantı dizesini kopyalayın. yani ADO.NET bağlantı kitaplığını kullanmayı planlıyorsanız, **ADO.NET** sekmesinden uygun dizekopyalayın.

    ![Veritabanınız için ADO bağlantı dizesini kopyalama][20-CopyAdoConnectionString]

7. Bağlantı dizesini gerektiği gibi edin. kullanıcı adı veya sunucu adı çok uzunsa parolanızı bağlantı dizesine ekleyin veya kullanıcı adından "@&lt;servername"&gt;kaldırın.

8. Şu veya bu biçimde, bağlantı dize bilgilerini istemci program kodunuza yapıştırın.

Daha fazla bilgi için [Bağlantı dizeleri ve yapılandırma dosyalarına](https://msdn.microsoft.com/library/ms254494.aspx)bakın.

<!-- Image references. -->



[20-CopyAdoConnectionString]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-b.png


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
