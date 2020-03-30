---
title: Quickstart - SQL yönetilen örnek
description: Azure SQL Veritabanı ile hızlı bir şekilde nasıl başlayın - yönetilen örnek
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlr
ms.date: 07/11/2019
ms.openlocfilehash: 602de3e23eb5419958f84b071e2220550d1d04d0
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "73821728"
---
# <a name="getting-started-with-azure-sql-database-managed-instance"></a>Azure SQL Veritabanı yönetilen örneğiyle çalışmaya başlama

[Yönetilen örnek](sql-database-managed-instance-index.yml) dağıtım seçeneği, en son SQL Server şirket içi (Enterprise Edition) veritabanı altyapısıyla %100'e yakın uyumluluğa sahip bir veritabanı oluşturarak, ortak güvenlik endişelerini gideren yerel bir sanal [ağ (VNet)](../virtual-network/virtual-networks-overview.md) uygulaması ve şirket içi SQL Server müşterileri için uygun bir iş [modeli](https://azure.microsoft.com/pricing/details/sql-database/) sağlar. Bu makalede, yönetilen bir örneği hızlı bir şekilde yapılandırmayı ve oluşturmayı ve veritabanlarınızı nasıl geçireceğinizi öğreneceksiniz.

## <a name="quickstart-overview"></a>Hızlı başlangıca genel bakış

Aşağıdaki hızlı başlatmalar, hızlı bir şekilde yönetilen bir örnek oluşturmanıza, sanal bir makineyi yapılandırmanıza veya istemci uygulaması `.bak` için site VPN bağlantısına işaret etmenizi ve bir dosyayı kullanarak yeni yönetilen örneğinize bir veritabanı geri yüklemenize olanak tanır.

### <a name="configure-environment"></a>Ortamı yapılandırma

İlk adım olarak, ilk Yönetilen Örneğinizi yerleştirileceği ağ ortamıyla oluşturmanız ve sorguları Yönetilen Örnek'e yürüttüğünüz bilgisayardan veya sanal makineden bağlantıyı etkinleştirmeniz gerekir. Aşağıdaki kılavuzları kullanabilirsiniz:

- [Azure portalını kullanarak yönetilen bir örnek oluşturun.](sql-database-managed-instance-get-started.md) Azure portalında, gerekli parametreleri (kullanıcı adı/parola, çekirdek sayısı ve maksimum depolama miktarı) yapılandırın ve ağ ayrıntıları ve altyapı gereksinimleri hakkında bilgi sahibi olmanıza gerek kalmadan Azure ağ ortamını otomatik olarak oluşturursunuz. Şu anda yönetilen bir örnek oluşturmak için izin verilen bir [abonelik türüne](sql-database-managed-instance-resource-limits.md#supported-subscription-types) sahip olduğundan emin olun. Kullanmak istediğiniz kendi ağınız varsa veya ağı özelleştirmek istiyorsanız, [Azure SQL Veritabanı yönetilen örneği için varolan bir sanal ağı yapılandırmaya](sql-database-managed-instance-configure-vnet-subnet.md) bakın veya [Azure SQL Veritabanı yönetilen örneği için sanal ağ oluşturun.](sql-database-managed-instance-create-vnet-subnet.md)
- Yönetilen bir örnek, ortak bitiş noktası olmayan kendi VNet'te oluşturulur. İstemci uygulaması na erişmek için, **aynı VNet'te (farklı alt ağ) bir VM oluşturabilir** veya aşağıdaki hızlı başlangıçlardan birini kullanarak **istemci bilgisayarınızdan VNet'e noktadan siteye VPN bağlantısı oluşturabilirsiniz:**
  - Verilerinize doğrudan ortamınızdan erişmek için Yönetilen Örneğinizde [genel bitiş noktasını](sql-database-managed-instance-public-endpoint-configure.md) etkinleştirin.
  - SQL Server Management Studio da dahil olmak üzere istemci uygulama bağlantısı için [yönetilen vnet örneğinde Azure Sanal Makine](sql-database-managed-instance-configure-vm.md) oluşturun.
  - SQL Server Management Studio ve diğer istemci bağlantı uygulamalarına sahip olduğunuz istemci bilgisayarınızdan [yönetilen örneğinize noktadan siteye VPN bağlantısı](sql-database-managed-instance-configure-p2s.md) ayarlayın. Bu, yönetilen örneğinize ve VNet'ine bağlantı için iki seçenekseçeneğinden başka bir seçenektir.

  > [!NOTE]
  > Yerel ağınızdan ekspres rota veya siteden siteye bağlantı da kullanabilirsiniz, ancak bu yaklaşımlar bu hızlı başlangıçların kapsamı dışındadır.

Yönetilen Örnek'in el ile oluşturulmasına alternatif olarak, [PowerShell](scripts/sql-database-create-configure-managed-instance-powershell.md), [PowerShell with Resource Manager şablonu](scripts/sql-managed-instance-create-powershell-azure-resource-manager-template.md)veya [Azure CLI'yi](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-create) bu işlemi komut dosyası ve otomatikleştirmek için kullanabilirsiniz.

### <a name="migrate-your-databases"></a>Veritabanlarınızı geçirin

Yönetilen bir örnek oluşturduktan ve erişimi yapılandırmadan sonra, veritabanlarınızı SQL Server'dan veya Azure VM'lerinden geçirebilirsiniz. Geçiş yapmak istediğiniz kaynak veritabanında desteklenmeyen bazı özellikleriniz varsa geçiş başarısız olur. Hataları önlemek ve uyumluluğu denetlemek için, veritabanlarınızı SQL Server'da çözümleyen ve [Dosya Akışı](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) veya birden çok günlük dosyasının varlığı gibi yönetilen bir örneye geçişi engelleyebilecek herhangi bir sorunu bulan Veri Geçiş Yardımcısı'nı [(DMA)](https://www.microsoft.com/download/details.aspx?id=53595) yükleyebilirsiniz. Bu sorunları giderirseniz, veritabanlarınız yönetilen örneğe geçmeye hazırdır. [Veritabanı Deneme Yardımcısı,](https://blogs.msdn.microsoft.com/datamigration/2018/08/06/release-database-experimentation-assistant-dea-v2-6/) iş yükünüzü SQL Server'a kaydedip yönetilen bir örneğe yeniden oynatabilen yararlı bir araçtır, böylece yönetilen bir örneye geçiş yaptığınızda performans sorunları olup olmayacağını belirleyebilirsiniz.

Veritabanınızı yönetilen bir örne geçirebileceğinizden emin olduktan sonra, veritabanını `.bak` bir dosyadan yönetilen bir örneğe geri yüklemek için yerel SQL Server geri yükleme özelliklerini kullanabilirsiniz. Veritabanlarını şirket içinde yüklü olan SQL Server veritabanı altyapısından veya Azure VM'den geçirmek için bu yöntemi kullanabilirsiniz. Hızlı bir başlangıç için, [yedekten yönetilen bir örne geri yükle'ye](sql-database-managed-instance-get-started-restore.md)bakın. Bu hızlı başlangıçta, Transact-SQL komutunu kullanarak Azure Blob depolama alanında depolanan bir `.bak` dosyadan `RESTORE` geri yüklenirsiniz.

> [!TIP]
> Azure Blob depolama alanında veritabanınızın yedeğini oluşturmak için `BACKUP` Transact-SQL komutunu kullanmak [için, SQL Server yedeklemesi ile URL'ye](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url)bakın.

Bu hızlı başlatmalar, veritabanı yedeklemesini yönetilen bir örneğe hızla oluşturmanıza, yapılandırmanıza ve geri yüklemenize olanak tanır. Bazı senaryolarda, yönetilen örneklerin ve gerekli ağ ortamının dağıtımını özelleştirmeniz veya otomatikleştirmeniz gerekir. Bu senaryolar aşağıda açıklanacaktır.

## <a name="customize-network-environment"></a>Ağ ortamını özelleştirme

Örnek [Azure portalı](sql-database-managed-instance-get-started.md)kullanılarak oluşturulduğunda VNet/alt net otomatik olarak yapılandırılabilir, ancak VNet ve alt ağ parametrelerini yapılandırabileceğinizden Yönetilen Örnekler oluşturmadan önce bu ağı oluşturmak iyi olabilir. Ağ ortamını oluşturmanın ve yapılandırmanın en kolay yolu, örneğin yerleştirileceği ağ ve alt ağ oluşturup yapılandıracak [Azure Kaynak dağıtım](sql-database-managed-instance-create-vnet-subnet.md) şablonu kullanmaktır. Azure Kaynak Yöneticisi dağıtma düğmesine basmanız ve formu parametrelerle doldurmanız yeterlidir.

Alternatif olarak, bu [PowerShell komut dosyasını](https://www.powershellmagazine.com/2018/07/23/configuring-azure-environment-to-set-up-azure-sql-database-managed-instance-preview/) ağın oluşturulmasını otomatikleştirmek için de kullanabilirsiniz.

Yönetilen örneğini dağıtmak istediğiniz bir VNet ve alt ağınız varsa, VNet'inizin ve alt ağınızın [ağ gereksinimlerini](sql-database-managed-instance-connectivity-architecture.md#network-requirements)karşıladığından emin olmanız gerekir. Alt [ağınızın düzgün şekilde yapılandırıldığından doğrulamak için bu PowerShell komut dosyasını](sql-database-managed-instance-configure-vnet-subnet.md)kullanın. Bu komut dosyası ağınızı doğrular ve herhangi bir sorunu bildirir ve size nelerin değiştirilmesi gerektiğini söyler ve vnet/alt netinizde gerekli değişiklikleri yapmayı teklif eder. VNet/subnet'inizi el ile yapılandırmak istemiyorsanız bu komut dosyasını çalıştırın. Ağ altyapınızın önemli bir yeniden yapılandırması sonrasında da çalıştırabilirsiniz. Kendi ağınızı oluşturmak ve yapılandırmak istiyorsanız, [yönetilen bir örnek ortamı oluşturmak ve yapılandırmak için](https://medium.com/azure-sqldb-managed-instance/the-ultimate-guide-for-creating-and-configuring-azure-sql-managed-instance-environment-91ff58c0be01)bağlantı [mimarisini](sql-database-managed-instance-connectivity-architecture.md) ve bu nihai kılavuzu okuyun.

## <a name="migrate-to-a-managed-instance"></a>Yönetilen bir örne geçirin

Bu hızlı başlangıçlarla ilgili makaleler, yönetilen bir örneği hızlı bir şekilde `RESTORE` ayarlamanızı ve yerel yeteneği kullanarak veritabanlarınızı taşımanızı sağlar. Kavramların hızlı bir şekilde kanıtlanması ve çözümünüzün Yönetilen Örnek üzerinde çalışabileceğini doğrulamak istiyorsanız, bu iyi bir başlangıç noktasıdır. 

Ancak, üretim veritabanınızı geçirmek için ve hatta bazı performans testi için kullanmak istediğiniz geliştirme/test veritabanlarını gerçekleştirmek için, örneğin bazı ek teknikler kullanmayı düşünmeniz gerekir:
- Performans testi - Kaynak SQL Server örneğinizdeki temel performansı ölçmeli ve veritabanını geçirdiğiniz hedef Yönetilen Örnek'teki performansla karşılaştırmalısınız. [Performans karşılaştırması için en iyi uygulamalar](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/The-best-practices-for-performance-comparison-between-Azure-SQL/ba-p/683210)hakkında daha fazla bilgi edinin.
- Çevrimiçi geçiş - `RESTORE` Bu makalede açıklanan yerel veri tabanının geri yüklanmasını (ve orada zaten depolanmamışsa Azure Blob depolama alanına kopyalanmasını) beklemeniz gerekir. Bu, özellikle daha büyük veritabanları için uygulamanızın bazı kapalı kalma sürelerine neden olur. Üretim veritabanınızı taşımak için, veritabanınızı en az kapalı kalma süresiyle geçirmek için [Veri Aktarımı hizmetini (DMS)](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance?toc=/azure/sql-database/toc.json) kullanın. DMS bunu, kaynak veritabanınızda yapılan değişiklikleri geri yüklenen yönetilen örnek veritabanına kademeli olarak iterek gerçekleştirir. Bu şekilde, uygulamanızı en az kapalı kalma süresiyle kaynaktan hedef veritabanına hızlı bir şekilde değiştirebilirsiniz.

Önerilen geçiş [işlemi](sql-database-managed-instance-migrate.md)hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen [örneğinde desteklenen özelliklerin üst düzey](sql-database-features.md) bir listesini burada ve [ayrıntıları ve bilinen sorunları burada](sql-database-managed-instance-transact-sql-information.md)bulabilirsiniz.
- Yönetilen [örneğin teknik özellikleri](sql-database-managed-instance-resource-limits.md#service-tier-characteristics)hakkında bilgi edinin.
- [Azure SQL Veritabanı'nda yönetilen bir örneğin nasıl kullanılacağı](sql-database-howto-managed-instance.md)konusunda daha gelişmiş nasıl yapılır'ı öğrenin.
- [Şirket içi veritabanınız için doğru Azure SQL Veritabanı/Yönetilen Örnek SKU'yu tanımlayın.](/sql/dma/dma-sku-recommend-sql-db/)
