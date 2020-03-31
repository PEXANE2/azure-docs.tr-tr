---
title: Quickstart - tek veritabanları
description: Azure SQL Veritabanı'ndaki tek veritabanlarıyla nasıl hızla başlayın gerektiğini öğrenin
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlr
ms.date: 07/29/2019
ms.openlocfilehash: 6070b53e5f906bc378402d98275b8f798f57b505
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78674423"
---
# <a name="getting-started-with-single-databases-in-azure-sql-database"></a>Azure SQL Veritabanı'nda tek veritabanları yla başlarken

Tek bir [veritabanı,](sql-database-single-index.yml) modern bulut kaynaklı uygulamalar için ideal depolama motoru olan bir hizmet (DbaaS) olarak PaaS veritabanını tam olarak yönetilir. Bu bölümde, SQL Veritabanı'nda hızlı bir şekilde nasıl yapılandırAcağınızı ve tek bir veritabanı oluşturacağınızı öğreneceksiniz.

## <a name="quickstart-overview"></a>Hızlı başlangıca genel bakış

Bu bölümde, tek veritabanlarıyla hızla başlamanıza yardımcı olabilecek kullanılabilir makalelere genel bir bakış göreceksiniz. Aşağıdaki hızlı başlatmalar, hızlı bir şekilde tek bir veritabanı oluşturmanızı, bir veritabanı sunucusu güvenlik duvarı kuralını yapılandırmanızı ve ardından bir `.bacpac` dosyayı kullanarak yeni tek veritabanına bir veritabanı almanızı sağlar:

- [Azure portalını kullanarak tek bir veritabanı oluşturun.](sql-database-single-database-get-started.md)
- Veritabanını oluşturduktan sonra, [güvenlik duvarı kurallarını yapılandırarak veritabanınızı güvenli hale](sql-database-server-level-firewall-rule.md)almanız gerekir.
- SQL Server'da Azure'a geçirmek istediğiniz varolan bir veritabanınız varsa, veritabanlarınızı SQL Server'da çözümleyecek ve tek veritabanı dağıtım seçeneğine geçişi engelleyebilecek herhangi bir sorun bulacak [Veri Geçiş Yardımcısı 'nı (DMA)](https://www.microsoft.com/download/details.aspx?id=53595) yüklemeniz gerekir. Herhangi bir sorun bulamazsanız, veritabanınızı dosya `.bacpac` olarak dışa aktarabilir ve [Azure portalını veya SqlPackage'ı kullanarak içe aktarabilirsiniz.](sql-database-import.md)

## <a name="automating-management-operations"></a>Yönetim işlemlerini otomatikleştirmek

Veritabanınızı oluşturmak, yapılandırmak ve ölçeklendirmek için PowerShell'i veya Azure CLI'yi kullanabilirsiniz.

- [PowerShell veya](scripts/sql-database-create-and-configure-database-powershell.md) [Azure CLI](scripts/sql-database-create-and-configure-database-cli.md) kullanarak tek bir veritabanı oluşturma ve yapılandırma
- PowerShell veya [Azure CLI](scripts/sql-database-monitor-and-scale-database-cli.md) [kullanarak tek veritabanınızı ve ölçek kaynaklarınızı güncelleştirin](scripts/sql-database-monitor-and-scale-database-powershell.md)

## <a name="migrating-to-a-single-database-with-minimal-downtime"></a>En az kapalı kalma süresiyle tek bir veritabanına geçiş

Bu hızlı başlangıçlar, bir `.bacpac` dosya kullanarak veritabanınızı hızla oluşturmanıza veya Azure'a aktarmanıza olanak tanır. `.dacpac` Ancak, `.bacpac` dosyalar veritabanlarını SQL Server'ın farklı sürümlerinde hızlı bir şekilde taşımak ve Azure SQL Veritabanı'ndaki dağıtım seçenekleri arasında hızlı bir şekilde taşımak veya DevOps ardışık ardınızda sürekli tümleştirme uygulamak için tasarlanmıştır. Ancak, bu yöntem üretim veritabanlarınızın en az kapalı kalma süresiyle geçiş ilerlediği için tasarlanmaz, çünkü yeni `.bacpac` veri eklemeyi durdurmanız, kaynak veritabanının bir dosyaya dışa aktarılmasını beklemeniz ve ardından Azure SQL Veritabanı'na içe aktarmanın tamamlanmasını beklemeniz gerekir. Tüm bu bekleme, uygulamanızın özellikle büyük veritabanları için kapalı kalma süresine neden olabilir. Üretim veritabanınızı taşımak için, en az kesinti süresini garanti eden daha iyi bir geçiş yolu gerekir. Bunun için, veritabanınızı en az kapalı kalma süresiyle geçirmek için [Veri Geçiş Hizmeti'ni (DMS)](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-azure-sql?toc=/azure/sql-database/toc.json) kullanın... DMS bunu, kaynak veritabanınızda yapılan değişiklikleri geri yüklenen tek veritabanına kademeli olarak iterek gerçekleştirir. Bu şekilde, uygulamanızı en az kapalı kalma süresiyle kaynaktan hedef veritabanına hızlı bir şekilde değiştirebilirsiniz.

## <a name="hands-on-learning-modules"></a>Uygulamalı öğrenme modülleri

Aşağıdaki Microsoft Learn modülleri Azure SQL Veritabanı hakkında ücretsiz bilgi edinmenize yardımcı olur.

- [Uygulama verilerini depolamak için bir Azure SQL veritabanı sağlama](https://docs.microsoft.com/learn/modules/provision-azure-sql-db/)
- [Azure SQL Veritabanı'nı sorgulayan bir ASP.NET uygulaması geliştirme ve yapılandırma](https://docs.microsoft.com/learn/modules/develop-app-that-queries-azure-sql/)
- [Azure SQL Veritabanınızı Güvenli Hale](https://docs.microsoft.com/learn/modules/secure-your-azure-sql-database/)

## <a name="next-steps"></a>Sonraki adımlar

- Azure [SQL Veritabanı'nda desteklenen özelliklerin üst düzey bir listesini](sql-database-features.md)bulun.
- Veritabanınızı nasıl [daha güvenli](sql-database-security-tutorial.md)hale getirebildiğini öğrenin.
- [Azure SQL Veritabanı'nda tek bir veritabanının nasıl kullanılacağı](sql-database-howto-single-database.md)konusunda daha gelişmiş nasıl yapılır'ı öğrenin.
- [PowerShell](sql-database-powershell-samples.md) ve [Azure CLI'de](sql-database-cli-samples.md)yazılmış daha fazla örnek komut dosyası bulun.
- Veritabanlarınızı yapılandırmak için kullanabileceğiniz [yönetim API'si](sql-database-single-databases-manage.md) hakkında daha fazla bilgi edinin.
- [Şirket içi veritabanınız için doğru Azure SQL Veritabanı/Yönetilen Örnek SKU'yu tanımlayın.](/sql/dma/dma-sku-recommend-sql-db/)
