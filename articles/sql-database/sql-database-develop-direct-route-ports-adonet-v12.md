---
title: 1433 dışındaki bağlantı noktaları
description: ADO.NET'dan Azure SQL Veritabanı'na istemci bağlantıları proxy'yi atlayabilir ve 1433 dışındaki bağlantı noktalarını kullanarak veritabanıyla doğrudan etkileşimkurabilir.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: sstein
ms.date: 04/03/2019
ms.openlocfilehash: c0012b61cf43d01afd5e7f5f52948310b5eb8420
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73828069"
---
# <a name="ports-beyond-1433-for-adonet-45"></a>ADO.NET 4.5 için 1433’ten sonraki bağlantı noktaları

Bu konu, 4,5 veya daha sonraki sürümADO.NET kullanan istemciler için Azure SQL Veritabanı bağlantı davranışını açıklar.

> [!IMPORTANT]
> Bağlantı mimarisi hakkında daha fazla bilgi için [Azure SQL Veritabanı bağlantı mimarisine](sql-database-connectivity-architecture.md)bakın.
>

## <a name="outside-vs-inside"></a>Dış vs içinde

Azure SQL Veritabanı'na bağlantılar için öncelikle istemci programınızın Azure bulut *sınırının dışında* mı yoksa *içinde* mi çalıştığını sormamız gerekir. Alt bölümlerde iki yaygın senaryo görüşür.

### <a name="outside-client-runs-on-your-desktop-computer"></a>*Dış:* İstemci masaüstü bilgisayarınızda çalışır

Port 1433, MASAÜSTÜ bilgisayarınızda SQL Veritabanı istemci uygulamanızı barındıran açık olması gereken tek bağlantı noktasıdır.

### <a name="inside-client-runs-on-azure"></a>*İçinde:* İstemci Azure'da çalışır

İstemciniz Azure bulut sınırı içinde çalıştığında, SQL Veritabanı sunucusuyla etkileşim kurmak için *doğrudan rota* dediğimiz şeyi kullanır. Bir bağlantı kurulduktan sonra, istemci ve veritabanı arasındaki diğer etkileşimler azure SQL Veritabanı Ağ Geçidi içermez.

Dizi aşağıdaki gibidir:

1. ADO.NET 4,5 (veya sonraki) Azure bulutuyla kısa bir etkileşim başlatır ve dinamik olarak tanımlanmış bir bağlantı noktası numarası alır.

   * Dinamik olarak tanımlanan bağlantı noktası numarası 11000-11999 aralığındadır.
2. ADO.NET sonra doğrudan SQL Veritabanı sunucusuna bağlanır, arasında hiçbir ara yazılım ile.
3. Sorgular doğrudan veritabanına gönderilir ve sonuçlar doğrudan istemciye döndürülür.

Azure istemci makinenizdeki 11000-11999 bağlantı noktası aralıklarının SQL Veritabanı ile ADO.NET 4,5 istemci etkileşimi için kullanılabilir bırakıldığından emin olun.

* Özellikle, aralıktaki bağlantı noktaları diğer giden engelleyicilerden arındırılmış olmalıdır.
* Azure VM'nizde Gelişmiş **Güvenlikli Windows Güvenlik Duvarı** bağlantı noktası ayarlarını denetler.
  
  * **11000-11999**gibi sözdizimi ile birlikte bir bağlantı noktası aralığı ile birlikte **TCP** protokolü belirten bir kural eklemek için [güvenlik duvarının kullanıcı arabirimini](https://msdn.microsoft.com/library/cc646023.aspx) kullanabilirsiniz.

## <a name="version-clarifications"></a>Sürüm açıklamaları

Bu bölümde, ürün sürümlerine atıfta bulunan monikerler açıklığa kavuşturulmuştur. Ayrıca, ürünler arasındaki bazı sürüm eşleşmelerini de listeler.

### <a name="adonet"></a>ADO.NET

* ADO.NET 4.0 TDS 7.3 protokolünü destekler, ancak 7.4'ünü desteklemez.
* ADO.NET 4.5 ve daha sonra TDS 7.4 protokolünü destekler.

### <a name="odbc"></a>ODBC

* Microsoft SQL Server ODBC 11 veya üzeri

### <a name="jdbc"></a>JDBC

* Microsoft SQL Server JDBC 4.2 veya üzeri (JDBC 4.0 aslında TDS 7.4'ü destekler, ancak "yeniden yönlendirme" uygulamaz)

## <a name="related-links"></a>İlgili bağlantılar

* ADO.NET 4.6 20 Temmuz 2015 tarihinde yayınlandı. .NET ekibinden bir blog [duyurusuna buradan](https://blogs.msdn.com/b/dotnet/archive/20../../announcing-net-framework-4-6.aspx)ulaşabilirsiniz.
* ADO.NET 4.5 15 Ağustos 2012 tarihinde yayınlandı. .NET ekibinden bir blog [duyurusuna buradan](https://blogs.msdn.com/b/dotnet/archive/20../../announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx)ulaşabilirsiniz.
  * 4.5.1 ADO.NET hakkında bir blog yazısı [burada](https://blogs.msdn.com/b/dotnet/archive/20../../announcing-the-net-framework-4-5-1-preview.aspx)mevcuttur.

* SQL Server için Microsoft® ODBC Driver 17® - Windows, Linux, & macOShttps://www.microsoft.com/download/details.aspx?id=56567

* Yeniden Yönlendirme ile Azure SQL Veritabanı V12'ye bağlanınhttps://techcommunity.microsoft.com/t5/DataCAT/Connect-to-Azure-SQL-Database-V12-via-Redirection/ba-p/305362

* [TDS protokolü sürüm listesi](https://www.freetds.org/userguide/tdshistory.htm)
* [SQL Veritabanı Geliştirme genel bakış](sql-database-develop-overview.md)
* [Azure SQL Veritabanı güvenlik duvarı](sql-database-firewall-configure.md)
* [Nasıl yapılır: SQL Veritabanı’nda güvenlik duvarı ayarlarını yapılandırma](sql-database-configure-firewall-settings.md)


