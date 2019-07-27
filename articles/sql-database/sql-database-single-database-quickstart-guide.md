---
title: Hızlı başlangıç-Azure SQL veritabanı 'nda tek veritabanları | Microsoft Docs
description: Azure SQL veritabanı 'nda tek veritabanlarına hızlı bir şekilde nasıl başlaleyeceğinizi öğrenin
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlr
ms.date: 02/04/2019
ms.openlocfilehash: b4341cb7d1734cb25aae890cc282c67ef38d67bf
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68566486"
---
# <a name="getting-started-with-single-databases-in-azure-sql-database"></a>Azure SQL veritabanı 'nda tek veritabanlarıyla çalışmaya başlama

[Tek bir veritabanı](sql-database-single-index.yml) , modern bulut kaynaklı uygulamalar için ideal bir depolama altyapısı olan hizmet olarak veritabanı (DBaaS) tam olarak yönetilir. Bu bölümde, SQL veritabanında tek bir veritabanını hızlı bir şekilde yapılandırmayı ve oluşturmayı öğreneceksiniz.

## <a name="quickstart-overview"></a>Hızlı başlangıç genel bakış

Bu bölümde, tek veritabanlarını hızlıca kullanmaya başlamanıza yardımcı olabilecek kullanılabilir makalelere bir genel bakış görürsünüz. Aşağıdaki hızlı başlangıçler hızlı bir şekilde tek bir veritabanı oluşturmanızı, bir veritabanı sunucusu güvenlik duvarı kuralını yapılandırmanızı ve sonra bir `.bacpac` dosyayı kullanarak yeni tek veritabanına bir veritabanını almanızı sağlar:

- [Azure Portal kullanarak tek bir veritabanı oluşturun](sql-database-single-database-get-started.md).
- Veritabanını oluşturduktan sonra, [güvenlik duvarı kurallarını yapılandırarak veritabanınızı güvenli hale](sql-database-server-level-firewall-rule.md)getirmeniz gerekir.
- Azure 'a geçirmek istediğiniz SQL Server var olan bir veritabanınız varsa, SQL Server veritabanlarınızı çözümleyen ve tek veritabanı dağıtımına geçişi engelleyebilen herhangi bir sorun bulacağınız [Data Migration Yardımcısı (DMA)](https://www.microsoft.com/download/details.aspx?id=53595) yüklemelisiniz. seçeneği. Herhangi bir sorun bulamazsanız, veritabanınızı dosya olarak `.bacpac` dışa aktarabilir ve [Azure Portal veya SqlPackage kullanarak içeri aktarabilirsiniz](sql-database-import.md).

## <a name="automating-management-operations"></a>Yönetim işlemlerini otomatikleştirme

Veritabanınızı oluşturmak, yapılandırmak ve ölçeklendirmek için PowerShell veya Azure CLı kullanabilirsiniz.

- [PowerShell kullanarak tek bir veritabanı oluşturma ve yapılandırma](scripts/sql-database-create-and-configure-database-powershell.md)
- [Azure CLı kullanarak tek bir veritabanı oluşturma ve yapılandırma](scripts/sql-database-create-and-configure-database-cli.md)
- [PowerShell kullanarak tek veritabanınızı güncelleştirme ve kaynakları ölçeklendirme](scripts/sql-database-monitor-and-scale-database-powershell.md)
- [Azure CLı kullanarak tek veritabanınızı güncelleştirme ve kaynakları ölçeklendirme](scripts/sql-database-monitor-and-scale-database-cli.md)

## <a name="migrating-to-a-single-database-with-minimal-downtime"></a>En az kapalı kalma süresiyle tek bir veritabanına geçiş

Bu hızlı başlangıç, bir dosya kullanarak veritabanınızı hızlı bir `.bacpac` şekilde oluşturmanıza veya Azure 'da aktarmanıza olanak tanır. Ancak, `.bacpac` ve `.dacpac` dosyaları Azure SQL veritabanı içindeki farklı SQL Server ve dağıtım seçenekleri arasında hızlı bir şekilde taşımak veya DevOps işlem hattınızda sürekli tümleştirme uygulamak için tasarlanmıştır. Ancak, bu yöntem, üretim veritabanlarınızı en az kapalı kalma süresiyle geçiş için tasarlanmamıştır, çünkü yeni veri ekleme işlemini durdurmanız, kaynak veritabanının bir `.bacpac` dosyanın tamamlanmasını beklemeniz ve sonra içeri aktarılmasını beklemeniz gerekir Tamamlanacak Azure SQL veritabanı. Bu bekleyen tüm bu durum, özellikle büyük veritabanları için uygulamanızın kapalı kalma süresine neden olur. Üretim veritabanınızı taşımak için geçişin en az kapalı kalma süresini garanti eden geçiş için daha iyi bir yol gerekir. Bunun için, [veri geçiş hizmeti 'ni (DMS)](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-azure-sql?toc=/azure/sql-database/toc.json) kullanarak veritabanınızı en düşük kapalı kalma süresiyle geçirin. DMS, kaynak veritabanınızda yapılan değişiklikleri geri yüklenen tek veritabanına artırarak bunu gerçekleştirir. Bu şekilde, uygulamanızı kaynaktan hedef veritabanına en az kapalı kalma süresi ile hızlıca geçirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SQL veritabanı 'nda desteklenen özelliklerin üst düzey bir listesini](sql-database-features.md)bulun.
- [Veritabanınızı daha güvenli](sql-database-security-tutorial.md)hale getirme hakkında bilgi edinin.
- [Azure SQL veritabanı 'nda tek bir veritabanının nasıl kullanılacağı](sql-database-howto-single-database.md)hakkında daha gelişmiş nasıl yapılır hakkında daha fazla bilgi edinin.
- [PowerShell](sql-database-powershell-samples.md) ve [Azure CLI](sql-database-cli-samples.md)'de yazılmış daha fazla örnek komut dosyası bulabilirsiniz.
- Veritabanlarınızı yapılandırmak için kullanabileceğiniz [Yönetim API 'si](sql-database-single-databases-manage.md) hakkında daha fazla bilgi edinin.
- Şirket [içi veritabanınız için doğru Azure SQL veritabanı/yönetilen örnek SKU 'Sunu belirler](/sql/dma/dma-sku-recommend-sql-db/).