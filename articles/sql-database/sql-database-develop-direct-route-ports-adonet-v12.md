---
title: 1433 dışındaki bağlantı noktaları
description: ADO.NET 'den Azure SQL veritabanı 'na istemci bağlantıları, proxy 'yi atlayabilir ve 1433 dışındaki bağlantı noktalarını kullanarak doğrudan veritabanıyla etkileşime geçebilir.
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
ms.openlocfilehash: 34e9b40b0b6989edacd72eb23d357e2a84b6f8d8
ms.sourcegitcommit: b396c674aa8f66597fa2dd6d6ed200dd7f409915
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2020
ms.locfileid: "82891073"
---
# <a name="ports-beyond-1433-for-adonet-45"></a>ADO.NET 4.5 için 1433’ten sonraki bağlantı noktaları

Bu konuda, ADO.NET 4,5 veya sonraki bir sürümünü kullanan istemciler için Azure SQL veritabanı bağlantı davranışı açıklanmaktadır.

> [!IMPORTANT]
> Bağlantı mimarisi hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı bağlantı mimarisi](sql-database-connectivity-architecture.md).
>

## <a name="outside-vs-inside"></a>Dış ve iç

Azure SQL veritabanı bağlantıları için, önce istemci programınızın Azure bulut sınırının *dışında* mı yoksa *içinde* mi çalıştığını sormalıdır. Alt bölümlerde iki yaygın senaryo ele alınmaktadır.

### <a name="outside-client-runs-on-your-desktop-computer"></a>*Dış:* İstemci masaüstü bilgisayarınızda çalışır

Bağlantı noktası 1433, SQL veritabanı istemci uygulamanızı barındıran masaüstü bilgisayarınızda açık olması gereken tek bağlantı noktasıdır.

### <a name="inside-client-runs-on-azure"></a>*İçinde:* İstemci Azure 'da çalışır

İstemciniz Azure bulut sınırının içinde çalıştığında, SQL veritabanı sunucusu ile etkileşim kurmak için *doğrudan bir yol* arayabilmeniz gerekenleri kullanır. Bir bağlantı kurulduktan sonra, istemci ve veritabanı arasındaki diğer etkileşimler Azure SQL veritabanı ağ geçidi gerektirmez.

Sıra aşağıdaki gibidir:

1. ADO.NET 4,5 (veya üzeri), Azure bulutuyla kısa bir etkileşim başlatır ve dinamik olarak tanımlanan bir bağlantı noktası numarası alır.

   * Dinamik olarak tanımlanan bağlantı noktası numarası 11000-11999 aralığındadır.
2. ADO.NET ardından, arasında bir ara yazılım olmadan doğrudan SQL veritabanı sunucusuna bağlanır.
3. Sorgular doğrudan veritabanına gönderilir ve sonuçlar doğrudan istemciye döndürülür.

Azure istemci makinenizde 11000-11999 numaralı bağlantı noktası aralıklarının SQL veritabanı ile ADO.NET 4,5 istemci etkileşimleri için kullanılabilir olduğundan emin olun.

* Özellikle, aralıktaki bağlantı noktaları diğer giden engelleyicilerin dışında olmalıdır.
* Azure VM 'niz üzerinde, **Gelişmiş Güvenlik Özellikli Windows Güvenlik Duvarı** bağlantı noktası ayarlarını denetler.
  
  * [Güvenlik duvarının Kullanıcı arabirimini](https://msdn.microsoft.com/library/cc646023.aspx) , **11000-11999**gibi bir bağlantı noktası aralığıyla birlikte **TCP** protokolünü belirlediğiniz bir kural eklemek için kullanabilirsiniz.

## <a name="version-clarifications"></a>Sürüm hakkında açıklamalar

Bu bölümde ürün sürümlerine başvuran bilinen adlar açıklığa kavuşturulur. Ayrıca, ürünler arasında bazı sürüm eşleştirmeleri de listelenir.

### <a name="adonet"></a>ADO.NET

* ADO.NET 4,0, TDS 7,3 protokolünü destekler, ancak 7,4.
* ADO.NET 4,5 ve üzeri, TDS 7,4 protokolünü destekler.

### <a name="odbc"></a>ODBC

* Microsoft SQL Server ODBC 11 veya üzeri

### <a name="jdbc"></a>JDBC

* Microsoft SQL Server JDBC 4,2 veya üzeri (JDBC 4,0 aslında TDS 7,4 destekliyor ancak "yeniden yönlendirme" uygulamaz)

## <a name="related-links"></a>İlgili bağlantılar

* ADO.NET 4,6, 20 Temmuz 2015 tarihinde yayınlandı. [Burada](https://devblogs.microsoft.com/dotnet/announcing-net-framework-4-6/).net ekibinin bir blog duyurusu bulunur.
* ADO.NET 4,5, 15 Ağustos 2012 tarihinde yayınlandı. [Burada](https://devblogs.microsoft.com/dotnet/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code/).net ekibinin bir blog duyurusu bulunur.
  * ADO.NET 4.5.1 hakkında bir blog gönderisi [burada](https://devblogs.microsoft.com/dotnet/announcing-the-net-framework-4-5-1-preview/)bulunabilir.

* SQL Server® için Microsoft® ODBC sürücüsü 17-Windows, Linux, & macOShttps://www.microsoft.com/download/details.aspx?id=56567

* Yeniden yönlendirme yoluyla Azure SQL Veritabanı V12 'e bağlanmahttps://techcommunity.microsoft.com/t5/DataCAT/Connect-to-Azure-SQL-Database-V12-via-Redirection/ba-p/305362

* [TDS protokol sürümü listesi](https://www.freetds.org/userguide/tdshistory.htm)
* [SQL veritabanı geliştirmeye genel bakış](sql-database-develop-overview.md)
* [Azure SQL veritabanı güvenlik duvarı](sql-database-firewall-configure.md)
* [Nasıl yapılır: SQL Veritabanı’nda güvenlik duvarı ayarlarını yapılandırma](sql-database-configure-firewall-settings.md)


