---
title: Azure SQL veritabanı Içeri/dışarı aktarma hizmeti 'nin bir veritabanını içeri veya dışarı aktarma işlemi uzun zaman alır | Microsoft Docs
description: Azure SQL veritabanı Içeri/dışarı aktarma hizmeti 'nin bir veritabanını içeri veya dışarı aktarma işlemi uzun zaman alır
services: sql-database
ms.service: sql-database
ms.topic: troubleshooting
author: v-miegge
ms.author: ramakoni
ms.reviewer: ''
ms.date: 09/27/2019
ms.openlocfilehash: ed5f3d1cd505270eb91c9cfbd6fb5c38b908f33d
ms.sourcegitcommit: c2e7595a2966e84dc10afb9a22b74400c4b500ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2019
ms.locfileid: "71974467"
---
# <a name="azure-sql-database-importexport-service-takes-a-long-time-to-import-or-export-a-database"></a>Azure SQL veritabanı Içeri/dışarı aktarma hizmeti 'nin bir veritabanını içeri veya dışarı aktarma işlemi uzun zaman alır

Azure SQL veritabanı Içeri/dışarı aktarma hizmeti kullanılırken, bazen işlemin tamamlanması uzun zaman alabilir. Bu makalede, bu sorunları geçici olarak ortadan kaldırmak için kullanabileceğiniz e gecikmeleri ve alternatif yöntemlerin olası nedenleri hakkında ek bilgiler sağlanmaktadır.

## <a name="azure-sql-database-importexport-service"></a>Azure SQL veritabanı Içeri/dışarı aktarma hizmeti

Azure SQL veritabanı Içeri/dışarı aktarma hizmeti, her Microsoft Azure veri merkezinde çalışan REST tabanlı bir Web hizmetidir. Bu, SQL veritabanınızı Microsoft Azure portal taşımak için [Içeri aktarma veritabanını](https://docs.microsoft.com/azure/sql-database/sql-database-import#import-from-a-bacpac-file-in-the-azure-portal) veya [dışarı aktarma](https://docs.microsoft.com/azure/sql-database/sql-database-export#export-to-a-bacpac-file-using-the-azure-portal) seçeneklerini kullandığınızda çağrılan hizmettir. Hizmet, Microsoft Azure bir SQL veritabanından içeri ve dışarı aktarma işlemleri gerçekleştirmek için ücretsiz bir istek sıraya alma hizmeti ve Microsoft Azure ikili büyük nesne (BLOB) depolama alanı sağlar.

İçeri ve dışarı aktarma işlemleri geleneksel bir fiziksel veritabanı yedeklemesi değildir ancak özel bir BACPAC biçimi kullanan veritabanının mantıksal bir yedeklemesi değildir. Bu mantıksal BACPAC biçimi, SQL Server ve SQL veritabanı sürümleri arasında değişebilen bir fiziksel biçim kullanmanızı önlemenize olanak sağlar. Bu nedenle, veritabanını bir SQL veritabanına ve SQL Server veritabanına güvenli bir şekilde geri yüklemek için kullanabilirsiniz.

## <a name="what-causes-the-process-to-take-a-long-time"></a>İşlemin uzun sürmesine neden olan süre

Azure SQL veritabanı Içeri/dışarı aktarma hizmeti, içeri ve dışarı aktarma işlemlerini işlemek için bölge başına sınırlı sayıda Işlem sanal makinesi (VM) sağlar. Işlem sanal makinesi bölge başına barındırılır ve içeri aktarma veya dışarı aktarma, bölgeler arası bant genişliği gecikmelerinin ve ücretlerinden emin olmanızı sağlar. Bu nedenle, aynı bölgede aynı anda çok fazla istek yapılırsa, işlemleri işlemenin önemli gecikmeleri oluşur. İstekleri tamamlaması için gereken süre birkaç saniyeden fazla saate kadar farklılık gösterebilir.

> [!NOTE]
> Bir istek dört gün içinde işlenmezse, hizmet isteği otomatik olarak iptal eder.

## <a name="recommended-solutions"></a>Önerilen çözümler

Veritabanınızın dışarı aktarmaları yalnızca yanlışlıkla veri silme işleminden kurtarmak için kullanılırsa, tüm Azure SQL Server veritabanı sürümleri sistem tarafından oluşturulan yedeklerden Self Servis geri yükleme özelliği sağlar. Ancak diğer nedenlerle bu dışarı aktarmalar varsa ve sürekli olarak daha hızlı veya daha öngörülebilir içeri aktarma veya dışa aktarma performansı gerekiyorsa, aşağıdaki seçenekleri göz önünde bulundurmanız gerekir:

* [SQLPackage yardımcı programını kullanarak BACPAC dosyasına aktarma](https://docs.microsoft.com/azure/sql-database/sql-database-export#export-to-a-bacpac-file-using-the-sqlpackage-utility)
* [SQL Server Management Studio (SSMS) kullanarak BACPAC dosyasına aktarma](https://docs.microsoft.com/azure/sql-database/sql-database-export#export-to-a-bacpac-file-using-sql-server-management-studio-ssms)
* Microsoft® SQL Server® veri katmanı uygulaması çerçevesi (DacFx) API 'sini kullanarak doğrudan kodunuzda BACPAC içeri aktarma veya dışarı aktarma işlemini çalıştırın. Ek bilgi incelemesi için
  * [Veri katmanı uygulamasını dışarı aktarma](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application)
  * [Microsoft. SqlServer. Dac ad alanı](https://docs.microsoft.com/dotnet/api/microsoft.sqlserver.dac)
  * [DACFx 'i indir](https://www.microsoft.com/download/details.aspx?id=55713)

## <a name="considerations-when-exporting-or-importing-an-azure-sql-database"></a>Azure SQL veritabanı verme veya içeri aktarma konuları

* Bu makalede ele alınan tüm yöntemler DTU kotasını kullanır ve Azure SQLDB hizmeti tarafından azaltma ile sonuçlanır. [Veritabanı IÇIN DTU istatistiklerini Azure Portal görüntüleyebilirsiniz](https://docs.microsoft.com/azure/sql-database/sql-database-monitor-tune-overview#monitor-database-performance). Veritabanı kaynak limitlerine ulaşarak daha fazla kaynak eklemek için [hizmet katmanını yükseltin](https://docs.microsoft.com/azure/sql-database/sql-database-scale-resources) .
* İstemci uygulamaları (SqlPackage yardımcı programı veya özel DAC uygulamanız gibi), SQL veritabanınız ile aynı bölgedeki bir sanal makineden (VM) en iyi şekilde çalıştırılmalıdır veya ağ gecikmesi nedeniyle performans sorunlarıyla karşılaşabilirsiniz.
* Kümelenmiş dizinler olmadan büyük tabloları dışarı aktarmak çok yavaş olabilir veya hatta hataya yol açabilir. Bunun nedeni, tablonun paralel olarak bölünemeyeceği ve dışarı aktarılamadığı ve tek bir işlemde dışa aktarılması ve özellikle büyük tablolar için, dışa aktarma sırasında yavaşlığın ve olası hatalara neden olması gerekir. 


## <a name="related-documents"></a>İlgili belgeler

[Azure SQL veritabanı dışarı aktarılırken dikkat edilecek noktalar](https://docs.microsoft.com/azure/sql-database/sql-database-export#considerations-when-exporting-an-azure-sql-database)
