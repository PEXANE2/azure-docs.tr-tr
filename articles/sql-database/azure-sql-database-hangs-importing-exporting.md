---
title: İthalat/İhracat hizmeti uzun zaman alır
description: Azure SQL Veritabanı İçe/Dışa Aktar hizmetinin veritabanını alma veya dışa aktarma işlemi uzun zaman alır
ms.custom: seo-lt-2019
services: sql-database
ms.service: sql-database
ms.topic: troubleshooting
author: v-miegge
ms.author: ramakoni
ms.reviewer: ''
ms.date: 09/27/2019
manager: dcscontentpm
ms.openlocfilehash: cf2d9b218fe63414af2446b8562d3ba187b2d395
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79535774"
---
# <a name="azure-sql-database-importexport-service-takes-a-long-time-to-import-or-export-a-database"></a>Azure SQL Veritabanı İçe/Dışa Aktar hizmetinin veritabanını alma veya dışa aktarma işlemi uzun zaman alır

Azure SQL Veritabanı İçe Alma/Dışa Aktarma hizmetini kullandığınızda, işlem beklenenden uzun sürebilir. Bu makalede, bu gecikme ve alternatif geçici çözüm yöntemlerinin olası nedenleri açıklanmaktadır.

## <a name="azure-sql-database-importexport-service"></a>Azure SQL Veritabanı İçe/Dışa Aktarma hizmeti

Azure SQL Veritabanı Alma/Verme hizmeti, her Azure veri merkezinde çalışan REST tabanlı bir web hizmetidir. Bu hizmet, SQL veritabanınızı Azure portalına taşımak için [İçe Aktarma veritabanını](sql-database-import.md#using-azure-portal) veya [Dışa Aktarma](https://docs.microsoft.com/azure/sql-database/sql-database-export#export-to-a-bacpac-file-using-the-azure-portal) seçeneğini kullandığınızda çağrılır. Hizmet, Azure SQL veritabanı ile Azure Blob depolama alanı arasında aktarım ve dışa aktarma gerçekleştirmek için ücretsiz istek kuyruk ve bilgi işlem hizmetleri sağlar.

Alma ve dışa aktarma işlemleri geleneksel fiziksel veritabanı yedeklemesini değil, bunun yerine özel bir BACPAC biçimini kullanan veritabanının mantıksal bir yedeklemesini temsil etmez. BACPAC biçimi, Microsoft SQL Server ve Azure SQL Veritabanı sürümleri arasında farklılık gösterebilecek fiziksel bir biçim kullanmak zorunda kalmanızı sağlar. Bu nedenle, veritabanını güvenli bir şekilde bir SQL Server veritabanına ve bir SQL veritabanına geri yüklemek için kullanabilirsiniz.

## <a name="what-causes-delays-in-the-process"></a>Bu süreçte gecikmelere ne sebep olur?

Azure SQL Veritabanı Alma/Dışa Aktarma hizmeti, alma ve dışa aktarma işlemlerini işlemek için bölge başına sınırlı sayıda bilgi işlem sanal makinesi (VM) sağlar. İşlem VM'leri, içe aktarma veya dışa aktarmanın bölgeler arası bant genişliği gecikmelerini ve ücretlendirmelerini önlediğinden emin olmak için bölge başına barındırılır. Aynı bölgede aynı anda çok fazla istek yapılırsa, işlemlerin işlenmesinde önemli gecikmeler oluşabilir. İstekleri tamamlamak için gereken süre birkaç saniye ile birkaç saat arasında değişebilir.

> [!NOTE]
> Bir istek dört gün içinde işleme alınmazsa, hizmet isteği otomatik olarak iptal eder.

## <a name="recommended-solutions"></a>Önerilen çözümler

Veritabanı dışa aktarımlarınız yalnızca yanlışlıkla veri silme işleminden kurtarma için kullanılıyorsa, tüm Azure SQL Veritabanı sürümleri sistem tarafından oluşturulan yedeklemelerden self servis geri yükleme özelliği sağlar. Ancak, başka nedenlerle bu dışa aktarmaya ihtiyacınız varsa ve sürekli olarak daha hızlı veya daha öngörülebilir ithalat/dışa aktarma performansı gerekiyorsa, aşağıdaki seçenekleri göz önünde bulundurun:

* [SQLPackage yardımcı programını kullanarak bir BACPAC dosyasına dışa aktarın.](https://docs.microsoft.com/azure/sql-database/sql-database-export#export-to-a-bacpac-file-using-the-sqlpackage-utility)
* [SQL Server Management Studio (SSMS) kullanarak bir BACPAC dosyasına dışa](https://docs.microsoft.com/azure/sql-database/sql-database-export#export-to-a-bacpac-file-using-sql-server-management-studio-ssms)aktarın.
* Microsoft SQL Server Veri Katmanı Uygulama Çerçevesi (DacFx) API'sini kullanarak BACPAC içe aktarma veya dışa aktar'ı doğrudan kodunuzda çalıştırın. Daha fazla bilgi için bkz:
  * [Veri katmanı uygulaması dışa aktarma](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application)
  * [Microsoft.SqlServer.Dac İsim Alanı](https://docs.microsoft.com/dotnet/api/microsoft.sqlserver.dac)
  * [DACFx'i İndirin](https://www.microsoft.com/download/details.aspx?id=55713)

## <a name="things-to-consider-when-you-export-or-import-an-azure-sql-database"></a>Azure SQL veritabanını dışa aktarırken veya içe aktarırken göz önünde bulundurulması gerekenler

* Bu makalede tartışılan tüm yöntemler, Azure SQL Veritabanı hizmeti tarafından daraltılmasına neden olan Veritabanı İşlem Birimi (DTU) kotasını kullanır. Azure [portalındaki veritabanının DTU istatistiklerini görüntüleyebilirsiniz.](https://docs.microsoft.com/azure/sql-database/sql-database-monitor-tune-overview#sql-database-resource-monitoring) Veritabanı kaynak sınırlarına ulaşmışsa, daha fazla kaynak eklemek için [hizmet katmanını yükseltin.](https://docs.microsoft.com/azure/sql-database/sql-database-scale-resources)
* İdeal olarak, istemci uygulamalarını (sqlpackage yardımcı programı veya özel DAC uygulamanız gibi) SQL veritabanınızla aynı bölgede bulunan bir VM'den çalıştırmalısınız. Aksi takdirde, ağ gecikmesi ile ilgili performans sorunları karşılaşabilirsiniz.
* Kümelenmiş dizinler olmadan büyük tablolar dışa aktarma çok yavaş olabilir ve hatta hataya neden olabilir. Tablo bölünemediği ve paralel olarak dışa aktarılamadığı için bu davranış oluşur. Bunun yerine, tek bir işlemde dışa aktarılmalıdır ve bu da özellikle büyük tablolar için dışa aktarma sırasında yavaş performans alave nerelerde hataya neden olur.


## <a name="related-documents"></a>İlgili belgeler

[Azure SQL veritabanı dışa aktarırken dikkat edilmesi gerekenler](https://docs.microsoft.com/azure/sql-database/sql-database-export#considerations-when-exporting-an-azure-sql-database)
