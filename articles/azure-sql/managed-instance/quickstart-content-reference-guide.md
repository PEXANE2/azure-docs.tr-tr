---
title: Başlangıç İçerik başvurusu
titleSuffix: Azure SQL Managed Instance
description: 'Azure SQL yönetilen örneği ile çalışmaya başlamanıza yardımcı olan içeriklere yönelik bir başvuru. '
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: quickstart
author: davidtrigano
ms.author: datrigan
ms.reviewer: vanto
ms.date: 07/11/2019
ms.openlocfilehash: 5ed88514650b9931a52e5f155abc34fbf734a3b7
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84310776"
---
# <a name="getting-started-with-azure-sql-managed-instance"></a>Azure SQL yönetilen örneği ile çalışmaya başlama
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

[Azure SQL yönetilen örneği](sql-managed-instance-paas-overview.md) , en son SQL Server (Enterprise Edition) veritabanı altyapısı ile %100 uyumluluğuna sahip bir veritabanı oluşturur. Bu, yaygın güvenlik sorunlarını gideren yerel bir [sanal ağ (VNet)](../../virtual-network/virtual-networks-overview.md) uygulamasını ve mevcut SQL Server müşterileri için uygun bir [iş modelini](https://azure.microsoft.com/pricing/details/sql-database/) sağlar.

Bu makalede, bir SQL yönetilen örneğini hızlı bir şekilde yapılandırmayı ve oluşturmayı ve veritabanlarınızı geçirmeyi öğreten içeriklere yönelik başvurular bulacaksınız.

## <a name="quickstart-overview"></a>Hızlı başlangıca genel bakış

Aşağıdaki hızlı başlangıçlarda hızlı bir şekilde bir SQL yönetilen örneği oluşturabilir, bir sanal makine yapılandırabilir veya istemci uygulaması için site VPN bağlantısına işaret edersiniz ve bir veritabanını bir dosya kullanarak yeni SQL yönetilen örneğinize geri yükleyebilirsiniz `.bak` .

### <a name="configure-environment"></a>Ortamı yapılandırma

İlk adım olarak, yerleştirilecek ağ ortamı ile ilk SQL yönetilen örneğinizi oluşturmanız ve SQL yönetilen örneği 'ne sorgular yürüttüğünüz bilgisayardan veya sanal makineden bağlantıyı etkinleştirmeniz gerekir. Aşağıdaki kılavuzlarla yararlanabilirsiniz:

- [Azure Portal kullanarak BIR SQL yönetilen örneği oluşturun](instance-create-quickstart.md). Azure portal, gerekli parametreleri (Kullanıcı adı/parola, çekirdek sayısı ve en fazla depolama miktarı) yapılandırıp, ağ ayrıntıları ve altyapı gereksinimleri hakkında bilgi sahibi olmadan otomatik olarak Azure ağ ortamını oluşturun. Yalnızca bir SQL yönetilen örneği oluşturmalarına izin verilen bir [abonelik türüne](resource-limits.md#supported-subscription-types) sahip olduğunuzdan emin olun. Kullanmak istediğiniz kendi ağınız varsa veya ağı özelleştirmek istiyorsanız, bkz. [Azure SQL yönetilen örneği için var olan bir sanal ağı yapılandırma](vnet-existing-add-subnet.md) veya [Azure SQL yönetilen örneği için bir sanal ağ oluşturma](virtual-network-subnet-create-arm-template.md).
- Bir SQL yönetilen örneği, kendi VNet 'inde ortak uç nokta olmadan oluşturulur. İstemci uygulama erişimi için, **aynı VNET 'te (farklı alt ağ) BIR VM oluşturabilir** veya şu hızlı **başlangıçlardan birini kullanarak istemci bilgisayarınızdan VNET 'e Noktadan siteye VPN bağlantısı oluşturabilirsiniz** :
  - Verilerinize doğrudan ortamınızdan erişebilmek için SQL yönetilen Örneğinizde [ortak uç noktayı](public-endpoint-configure.md) etkinleştirin.
  - SQL Server Management Studio dahil olmak üzere, istemci uygulama bağlantısı için [SQL yönetilen örnek VNET 'Te Azure sanal makinesi](connect-vm-instance-configure.md) oluşturun.
  - SQL Server Management Studio ve diğer istemci bağlantısı uygulamalarına sahip olduğunuz istemci bilgisayarınızdan [SQL yönetilen örneğiniz için noktadan sıteye VPN bağlantısı](point-to-site-p2s-configure.md) kurun. Bu, SQL yönetilen örneğiniz ve sanal ağı için bağlantı için iki seçenekten oluşur.

  > [!NOTE]
  > - Ayrıca, yerel ağınızdan Express Route veya siteden siteye bağlantı kullanabilirsiniz, ancak bu yaklaşımlar bu hızlı başlangıçların kapsamındadır.
  > - Saklama süresini 0 ' dan (sınırsız saklama) başka herhangi bir değere değiştirirseniz, lütfen bekletme 'nin yalnızca bekletme değeri değiştirildikten sonra yazılan günlüklere uygulanacağını unutmayın (bekletme etkin olduktan sonra bile, bekletme olarak sınırsız olarak ayarlandığında zaman içinde yazılır).

SQL yönetilen örneği 'nin el ile oluşturulmasına alternatif olarak, bu işlemi betik ve otomatikleştirebilmek için [PowerShell](scripts/create-configure-managed-instance-powershell.md), [Kaynak Yöneticisi şablonuyla POWERSHELL](scripts/create-powershell-azure-resource-manager-template.md)veya [Azure CLI](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-create) kullanabilirsiniz.

### <a name="migrate-your-databases"></a>Veritabanlarınızı geçirin

Bir SQL yönetilen örneği oluşturup erişimi yapılandırdıktan sonra, SQL Server veritabanlarınızı geçirmeye başlayabilirsiniz. Kaynak veritabanında geçirmek istediğiniz desteklenmeyen özelliklerden bazıları varsa geçiş başarısız olabilir. Hatalardan kaçınmak ve uyumluluğu denetlemek için [Data Migration Yardımcısı (DMA)](https://www.microsoft.com/download/details.aspx?id=53595) kullanarak SQL Server veritabanlarınızı çözümleyebilir ve [FILESTREAM](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) veya birden çok günlük dosyasının varlığı gibi bir SQL yönetilen örneğine geçişi engelleyebilen herhangi bir sorun bulabilirsiniz. Bu sorunları çöziyorsanız, veritabanlarınız SQL yönetilen örneği 'ne geçirmeye hazırız. [Veritabanı yükseltme deneyimi Yardımcısı](/sql/dea/database-experimentation-assistant-overview) , iş yükünüzü SQL Server KAYDEDIP bir SQL yönetilen örneği üzerinde yürütebilen, bir SQL yönetilen örneği 'ne geçiş yaparsanız herhangi bir performans sorunu olduğunu belirleyebilecek başka bir kullanışlı araçtır.

Veritabanınızı bir SQL yönetilen örneğine geçirebilmeniz için, yerel SQL Server geri yükleme yeteneklerini kullanarak bir veritabanını bir dosyadaki SQL yönetilen örneğine geri yükleyebilirsiniz `.bak` . Bu yöntemi, şirket içi veya Azure sanal makinelerinde yüklü SQL Server veritabanı altyapısından veritabanları geçirmek için kullanabilirsiniz. Hızlı başlangıç için bkz. [SQL tarafından yönetilen bir örneğe yedekten geri yükleme](restore-sample-database-quickstart.md). Bu hızlı başlangıçta `.bak` `RESTORE` Transact-SQL komutunu kullanarak Azure Blob depolama alanında depolanan bir dosyadan geri yükleme yapabilirsiniz.

> [!TIP]
> `BACKUP`Azure Blob depolamada veritabanınızın bir yedeğini oluşturmak için Transact-SQL komutunu kullanmak için bkz. [SQL Server yedekleme URL 'si](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url).

Bu hızlı başlangıç, veritabanı yedeklemesini bir SQL yönetilen örneği için hızlıca oluşturmanıza, yapılandırmanıza ve geri yüklemenize olanak tanır. Bazı senaryolarda, SQL yönetilen örneği ve gerekli ağ ortamının dağıtımını özelleştirmeniz ya da otomatik hale getirmeniz gerekir. Bu senaryolar aşağıda açıklanacaktır.

## <a name="customize-network-environment"></a>Ağ ortamını özelleştirme

Örnek, [Azure Portal kullanılarak oluşturulduğunda](instance-create-quickstart.md)VNET/alt ağ otomatik olarak yapılandırılabilse de, VNET ve alt ağ parametrelerini YAPıLANDıRABILMENIZ Için SQL yönetilen örneği 'nde örnek oluşturmaya başlamadan önce bunu oluşturmak iyi olabilir. Ağ ortamını oluşturmanın ve yapılandırmanın en kolay yolu, ağınızı ve örneğin yerleştirileceği alt ağınızı oluşturup yapılandıran [Azure kaynak dağıtım](virtual-network-subnet-create-arm-template.md) şablonunu kullanmaktır. Yalnızca Azure Resource Manager dağıt düğmesine basmanız ve formu parametrelerle doldurmanız gerekir.

Alternatif olarak, ağ oluşturulmasını otomatik hale getirmek için bu [PowerShell betiğini](https://www.powershellmagazine.com/2018/07/23/configuring-azure-environment-to-set-up-azure-sql-database-managed-instance-preview/) de kullanabilirsiniz.

SQL yönetilen örneğinizi dağıtmak istediğiniz VNet ve alt ağınız zaten varsa, VNet ve alt ağınızın [ağ gereksinimlerini](connectivity-architecture-overview.md#network-requirements)karşıladığınızdan emin olmanız gerekir. [Alt ağınızın düzgün yapılandırıldığını doğrulamak için bu PowerShell betiğini](vnet-existing-add-subnet.md)kullanın. Bu betik, ağınızı doğrular ve herhangi bir sorunu bildirir ve bu, ne değişiklik yapılacağını ve ardından VNet/alt ağınızda gerekli değişiklikleri yapmak için teklif sunar. VNet/alt ağını el ile yapılandırmak istemiyorsanız bu betiği çalıştırın. Ayrıca, ağ altyapınızın önemli bir yeniden yapılandırması sonrasında da çalıştırabilirsiniz. Kendi ağınızı oluşturup yapılandırmak istiyorsanız, [BIR SQL yönetilen örnek ortamı oluşturmak ve yapılandırmak için](https://medium.com/azure-sqldb-managed-instance/the-ultimate-guide-for-creating-and-configuring-azure-sql-managed-instance-environment-91ff58c0be01) [bağlantı mimarisini](connectivity-architecture-overview.md) ve bu üstün kılavuzu okuyun.

## <a name="migrate-to-a-sql-managed-instance"></a>SQL yönetilen örneği 'ne geçiş

Daha önce bahsedilen hızlı başlangıçlarda, bir SQL yönetilen örneğini hızla ayarlamanıza ve yerel özelliği kullanarak veritabanlarınızı taşımanızı sağlayabilirsiniz `RESTORE` . Bu, hızlı kavram kanıtı ve çözümünüzün yönetilen örnek üzerinde çalışabileceğinizden emin olmak istiyorsanız, iyi bir başlangıç noktasıdır.

Ancak, üretim veritabanınızı geçirmek veya bazı performans testi için kullanmak istediğiniz geliştirme/test veritabanlarını geçirmek için, şunlar gibi bazı ek teknikler kullanmayı göz önünde bulundurmanız gerekir:

- Performans testi-kaynak SQL Server Örneğiniz üzerinde temel performans ölçümlerini ölçmelisiniz ve bunları veritabanını geçirdiğiniz hedef SQL yönetilen örneği üzerindeki performans ölçümleriyle karşılaştırmalısınız. [Performans karşılaştırması için en iyi uygulamalar](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/The-best-practices-for-performance-comparison-between-Azure-SQL/ba-p/683210)hakkında daha fazla bilgi edinin.
- Çevrimiçi geçiş- `RESTORE` Bu makalede açıklandığı gibi, veritabanlarının geri yüklenmesini beklemeniz gerekir (ve henüz depolanmıyorsa Azure Blob depolama alanına kopyalanabilir). Bu, özellikle daha büyük veritabanları için uygulamanızın bazı kesintilerine neden olur. Üretim veritabanınızı taşımak için [veri geçiş hizmeti 'ni (DMS)](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance?toc=/azure/sql-database/toc.json) kullanarak veritabanınızı en düşük kapalı kalma süresiyle geçirin. DMS, kaynak veritabanınızda yapılan değişiklikleri geri yüklenmekte olan SQL yönetilen örnek veritabanına artırarak bunu gerçekleştirir. Bu şekilde, uygulamanızı kaynaktan hedef veritabanına en az kapalı kalma süresi ile hızlıca geçirebilirsiniz.

[Önerilen geçiş süreci](migrate-to-instance-from-sql-server.md)hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

- Burada [SQL yönetilen örneği ' nde desteklenen özelliklerin üst düzey bir listesini](../database/features-comparison.md) ve [ayrıntılarını ve bilinen sorunları](transact-sql-tsql-differences-sql-server.md)bulun.
- [SQL yönetilen örneğinin teknik özellikleri](resource-limits.md#service-tier-characteristics)hakkında bilgi edinin.
- [SQL yönetilen örneği kullanma konusunda](how-to-content-reference-guide.md)daha gelişmiş nasıl yapılır hakkında daha fazla bilgi edinin.
- Şirket [içi veritabanınız için doğru Azure SQL yönetilen örnek SKU 'Sunu belirler](/sql/dma/dma-sku-recommend-sql-db/).
