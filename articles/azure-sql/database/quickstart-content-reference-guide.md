---
title: Tek veritabanı hızlı başlangıç içeriği başvurusu
description: Azure SQL veritabanı 'nda tek veritabanlarına hızlıca başlamanıza yardımcı olacak tüm hızlı başlangıçlara yönelik içerik başvurusunu bulun
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: quickstart
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlr
ms.date: 07/29/2019
ms.openlocfilehash: c4a699a6b531feee98f9c6f83ba19cfef9d27620
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84054608"
---
# <a name="getting-started-with-single-databases-in-azure-sql-database"></a>Azure SQL veritabanı 'nda tek veritabanlarıyla çalışmaya başlama
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

[Tek bir veritabanı](../index.yml) , modern bulut kaynaklı uygulamalar için ideal bir depolama altyapısı olan hizmet olarak veritabanı (DBaaS) tam olarak yönetilir. Bu bölümde, Azure SQL veritabanı 'nda tek bir veritabanını hızlı bir şekilde yapılandırmayı ve oluşturmayı öğreneceksiniz.

## <a name="quickstart-overview"></a>Hızlı başlangıca genel bakış

Bu bölümde, tek veritabanlarını hızlıca kullanmaya başlamanıza yardımcı olabilecek kullanılabilir makalelere bir genel bakış görürsünüz. Aşağıdaki hızlı başlangıçler hızlı bir şekilde tek bir veritabanı oluşturmanıza, sunucu düzeyinde bir güvenlik duvarı kuralı yapılandırmanıza ve sonra bir dosyayı kullanarak yeni tek veritabanına bir veritabanını aktarmanıza olanak tanır `.bacpac` :

- [Azure Portal kullanarak tek bir veritabanı oluşturun](single-database-create-quickstart.md).
- Veritabanını oluşturduktan sonra, [güvenlik duvarı kurallarını yapılandırarak veritabanınızı güvenli hale](firewall-create-server-level-portal-quickstart.md)getirmeniz gerekir.
- Azure SQL veritabanı 'na geçirmek istediğiniz SQL Server var olan bir veritabanınız varsa, SQL Server veritabanlarınızı çözümleyen ve geçişi engelleyebilen herhangi bir sorunu bulacağınız [Data Migration Yardımcısı (DMA)](https://www.microsoft.com/download/details.aspx?id=53595) yüklemelisiniz. Herhangi bir sorun bulamazsanız, veritabanınızı dosya olarak dışa aktarabilir `.bacpac` ve [Azure Portal veya SqlPackage kullanarak içeri aktarabilirsiniz](database-import.md).


## <a name="automating-management-operations"></a>Yönetim işlemlerini otomatikleştirme

Veritabanınızı oluşturmak, yapılandırmak ve ölçeklendirmek için PowerShell veya Azure CLı kullanabilirsiniz.

- PowerShell veya [Azure CLI](scripts/create-and-configure-database-cli.md) [kullanarak tek bir veritabanı oluşturma ve yapılandırma](scripts/create-and-configure-database-powershell.md)
- PowerShell veya [Azure CLI](scripts/monitor-and-scale-database-cli.md) [kullanarak tek veritabanınızı güncelleştirin ve kaynakları ölçeklendirin](scripts/monitor-and-scale-database-powershell.md)

## <a name="migrating-to-a-single-database-with-minimal-downtime"></a>En az kapalı kalma süresiyle tek bir veritabanına geçiş

Bu hızlı başlangıç, bir dosya kullanarak veritabanınızı hızlı bir şekilde oluşturmanıza veya Azure 'da aktarmanıza olanak tanır `.bacpac` . Ancak, `.bacpac` ve `.dacpac` dosyaları, veritabanlarını SQL Server ve Azure SQL içindeki farklı sürümlerinde hızla taşımak veya DevOps işlem hattınızda sürekli tümleştirme uygulamak üzere tasarlanmıştır. Ancak, bu yöntem en az kapalı kalma süresiyle üretim veritabanlarınızın geçirilmesi için tasarlanmamıştır, çünkü yeni veri ekleme işlemini durdurmanız, kaynak veritabanının bir `.bacpac` dosyanın tamamlanmasını beklemeniz ve sonra Azure SQL veritabanı 'na aktarma işleminin tamamlanmasını beklemeniz gerekir. Bu bekleyen tüm bu durum, özellikle büyük veritabanları için uygulamanızın kapalı kalma süresine neden olur. Üretim veritabanınızı taşımak için geçişin en az kapalı kalma süresini garanti eden geçiş için daha iyi bir yol gerekir. Bunun için, [veri geçiş hizmeti 'ni (DMS)](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-azure-sql?toc=/azure/sql-database/toc.json) kullanarak veritabanınızı en düşük kapalı kalma süresiyle geçirin. DMS, kaynak veritabanınızda yapılan değişiklikleri geri yüklenen tek veritabanına artırarak bunu gerçekleştirir. Bu şekilde, uygulamanızı kaynaktan hedef veritabanına en az kapalı kalma süresi ile hızlıca geçirebilirsiniz.

## <a name="hands-on-learning-modules"></a>Uygulamalı öğrenme modülleri

Aşağıdaki Microsoft Learn modülleri, Azure SQL veritabanı hakkında ücretsiz bilgi edinmenize yardımcı olur.

- [Uygulama verilerini depolamak için SQL veritabanı 'nda veritabanı sağlama](https://docs.microsoft.com/learn/modules/provision-azure-sql-db/)
- [Azure SQL veritabanı 'nı sorgulayan bir ASP.NET uygulaması geliştirin ve yapılandırın](https://docs.microsoft.com/learn/modules/develop-app-that-queries-azure-sql/)
- [Azure SQL veritabanınızın güvenliğini sağlama](https://docs.microsoft.com/learn/modules/secure-your-azure-sql-database/)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SQL veritabanı 'nda desteklenen özelliklerin üst düzey bir listesini](features-comparison.md)bulun.
- [Veritabanınızı daha güvenli](secure-database-tutorial.md)hale getirme hakkında bilgi edinin.
- [Azure SQL veritabanı 'nda tek bir veritabanının nasıl kullanılacağı](how-to-content-reference-guide.md)hakkında daha gelişmiş nasıl yapılır hakkında daha fazla bilgi edinin.
- [PowerShell](powershell-script-content-guide.md) ve [Azure CLI](az-cli-script-samples-content-guide.md)'de yazılmış daha fazla örnek komut dosyası bulabilirsiniz.
- Veritabanlarınızı yapılandırmak için kullanabileceğiniz [Yönetim API 'si](single-database-manage.md) hakkında daha fazla bilgi edinin.
- Şirket [içi veritabanınız için doğru Azure SQL veritabanını veya SQL yönetilen örnek SKU 'Sunu belirler](/sql/dma/dma-sku-recommend-sql-db/).
