---
title: Uygulama Geliştirme genel bakış
description: SQL Veritabanına bağlanan uygulamalar için kullanılabilen bağlantı kitaplıkları ve en iyi uygulamalar hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: genemi
ms.date: 11/14/2019
ms.openlocfilehash: 741906bbe9de68459b2e4a704a243fde4771b3a2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80067321"
---
# <a name="sql-database-application-development-overview"></a>SQL Veritabanı uygulama geliştirme genel bakış

Bu makalede, geliştiricilerin Azure SQL Veritabanı ile bağlantı kurmak üzere kod yazarken dikkat etmesi gereken noktalara yer verilmiştir. Bu makale, Azure SQL Veritabanı'nın tüm dağıtım modelleri için geçerlidir (Tek veritabanı, Elastik havuzlar, Yönetilen örnek).

> [!TIP]
> Azure SQL Veritabanınızı kurmanız [gerekiyorsa, tek veritabanları](sql-database-single-database-quickstart-guide.md) ve [yönetilen örnekler](sql-database-managed-instance-quickstart-guide.md) için başlangıç kılavuzlarına bakın.
>

## <a name="language-and-platform"></a>Dil ve platform

Azure SQL [Veritabanı'nı](sql-database-connect-query.md) bağlamak ve sorgulamak için çeşitli programlama dillerini ve platformlarını kullanabilirsiniz. Azure SQL Veritabanı'na bağlanmak için kullanabileceğiniz [örnek uygulamaları](https://azure.microsoft.com/resources/samples/?service=sql-database&sort=0) bulabilirsiniz.

[Çita](https://github.com/wunderlist/cheetah), [sql-cli](https://www.npmjs.com/package/sql-cli), [VS Kodu](https://code.visualstudio.com/)gibi açık kaynak araçlarından yararlanabilirsiniz. Ayrıca, Azure SQL Veritabanı [Visual Studio](https://www.visualstudio.com/downloads/) ve [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) gibi Microsoft araçlarıyla birlikte çalışır. Azure portalını, PowerShell'i ve REST API'lerini ek üretkenlik elde etmek için de kullanabilirsiniz.

## <a name="authentication"></a>Kimlik doğrulaması

Azure SQL Veritabanına erişim, oturum açma lar ve güvenlik duvarlarıyla korunur. Azure SQL Veritabanı hem SQL Server'ı hem de [Azure Active Directory (AAD) kimlik doğrulama](sql-database-aad-authentication.md) kullanıcılarını ve oturum açmaları destekler. AAD girişleri yalnızca Yönetilen Örnek'te kullanılabilir. 

[Veritabanı erişimini ve oturum açmayı yönetme](sql-database-manage-logins.md)hakkında daha fazla bilgi edinin.

## <a name="connections"></a>Bağlantılar

İstemci bağlantısı mantığınızda varsayılan zaman aşımını 30 saniye olacak şekilde geçersiz kılın. 15 saniyelik varsayılan değer, internet kullanan bağlantılar için çok kısadır.

[Bağlantı havuzu](https://msdn.microsoft.com/library/8xx3tyca.aspx) kullanıyorsanız programınız etkin olarak kullanmadığında ve yeniden kullanma hazırlığı yapmadığında bağlantıyı kapatın.

Herhangi bir altyapı veya bağlantı hatası hareketi geri alabilir, çünkü uzun süren işlemlerkaçının. Mümkünse, hareketi birden çok küçük harekette bölün ve [performansı artırmak için toplu işlemi](sql-database-use-batching-to-improve-performance.md)kullanın.

## <a name="resiliency"></a>Dayanıklılık

Azure SQL Veritabanı, temel altyapıda veya bulut varlıkları arasındaki iletişimde geçici hatalar bekleyebilirsiniz bir bulut hizmetidir. Azure SQL Veritabanı geçişli altyapı hatalarında esnek olsa da, bu hatalar bağlantınızı etkileyebilir. SQL Veritabanı'na bağlanırken geçici bir hata oluştuğunda, kodunuz [aramayı yeniden denemelidir.](sql-database-connectivity-issues.md) Yeniden deneme mantığının SQL Veritabanını aynı anda yeniden deneme yapan birden fazla istemciyle boğmaması için geri alma mantığını kullanmasını öneririz. Yeniden deneme [mantığı, SQL Veritabanı istemci programlarının hata iletilerine](troubleshoot-connectivity-issues-microsoft-azure-sql-database.md)bağlıdır.

Azure SQL veritabanınızda planlanan bakım etkinliklerine nasıl hazırlanacağınız hakkında daha fazla bilgi için [Azure SQL Veritabanı'nda Azure bakım etkinlikleri için planlama](sql-database-planned-maintenance.md)ya da planlama ya da

## <a name="network-considerations"></a>Ağ konuları

- İstemci programınızı barındıran bilgisayarda güvenlik duvarının 1433 numaralı bağlantı noktasından giden TCP iletişimine izin verdiğinden emin olun.  Daha fazla bilgi: [Azure SQL Veritabanı güvenlik duvarLarını yapılandırın.](sql-database-configure-firewall-settings.md)
- İstemciniz bir Azure sanal makinede (VM) çalışırken istemci programınız SQL Veritabanı'na bağlanırsa, VM'de belirli bağlantı noktası aralıklarını açmanız gerekir. Daha fazla bilgi: [4.5 ADO.NET ve SQL Veritabanı için 1433'ün ötesindeki bağlantı noktaları.](sql-database-develop-direct-route-ports-adonet-v12.md)
- Azure SQL Veritabanı'na istemci bağlantıları bazen proxy'yi atlar ve veritabanıyla doğrudan etkileşimde bulunabilir. 1433 dışındaki bağlantı noktaları önemli hale gelmiştir. Daha fazla bilgi için, [4,5 ADO.NET ve SQL Veritabanı için 1433'ün ötesindeki](sql-database-develop-direct-route-ports-adonet-v12.md)Azure SQL Veritabanı bağlantı [mimarisi](sql-database-connectivity-architecture.md) ve Bağlantı Noktaları.
- Yönetilen bir örnek için ağ yapılandırması [için yönetilen örnekler için ağ yapılandırması'na](sql-database-howto-managed-instance.md#network-configuration)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Tüm [SQL Veritabanı özelliklerini](sql-database-technical-overview.md) keşfedin.
