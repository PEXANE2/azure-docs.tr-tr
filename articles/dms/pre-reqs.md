---
title: Azure Veritabanı Geçiş Hizmeti için Ön Koşullar
description: Veritabanı geçişlerini gerçekleştirmek için Azure Veritabanı Geçiş Hizmeti'ni kullanmanın ön koşulları hakkında genel bir bakış hakkında bilgi edinin.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: article
ms.date: 02/25/2020
ms.openlocfilehash: 89cb63630e3dbe953ed3f4fd8796d01ba0d36067
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77651500"
---
# <a name="overview-of-prerequisites-for-using-the-azure-database-migration-service"></a>Azure Veritabanı Geçiş Hizmeti'ni kullanmak için önkoşullara genel bakış

Veritabanı geçişlerini gerçekleştirirken Azure Veritabanı Geçiş Hizmeti'nin sorunsuz çalıştığından emin olmak için gereken birkaç ön koşul vardır. Bazı ön koşullar, hizmet tarafından desteklenen tüm senaryolar (kaynak hedef çiftleri) arasında geçerliyken, diğer ön koşullar belirli bir senaryoya özgüdür.

Azure Veritabanı Geçiş Hizmeti'ni kullanmayla ilgili ön koşullar aşağıdaki bölümlerde listelenmiştir.

## <a name="prerequisites-common-across-migration-scenarios"></a>Geçiş senaryoları arasında yaygın olan önkoşullar

Desteklenen tüm geçiş senaryolarında yaygın olan Azure Veritabanı Geçiş Hizmeti ön koşulları aşağıdakileri içerir:

* [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)kullanarak şirket içi kaynak sunucularınıza site bağlantısı sağlayan Azure Kaynak Yöneticisi dağıtım modelini kullanarak Azure Veritabanı Geçiş Hizmeti için bir Microsoft Azure Sanal Ağı oluşturun.
* Sanal ağ Ağ Güvenlik Grubu (NSG) kurallarınızın aşağıdaki iletişim bağlantı noktalarını engellemediğinden emin olun 443, 53, 9354, 445, 12000. Sanal ağ NSG trafik filtreleme hakkında daha fazla ayrıntı için, [ağ güvenlik grupları ile](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)makale Filtre ağ trafiği bakın.
* Kaynak veritabanınızın (ler) önünde bir güvenlik duvarı cihazı kullanırken, Azure Veritabanı Geçiş Hizmeti'nin geçiş için kaynak veritabanına(ler) erişmesine izin vermek için güvenlik duvarı kuralları eklemeniz gerekebilir.
* [Windows Güvenlik Duvarınızı veritabanı altyapısı erişimi](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access) için yapılandırın.
* [Sunucu Ağ Protokolünü Etkinleştirme veya Devre Dışı Bırakma](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure) makalesindeki yönergeleri izleyerek SQL Server Express yüklemesi sırasında varsayılan olarak devre dışı bırakılan TCP/IP protokolünü etkinleştirin.

    > [!IMPORTANT]
    > Azure Veritabanı Geçiş Hizmeti'nin bir örneğini oluşturmak, normalde aynı kaynak grubunda olmayan sanal ağ ayarlarına erişim gerektirir. Sonuç olarak, DMS örneğini oluşturan kullanıcı abonelik düzeyinde izin gerektirir. Gerektiğinde atayabileceğiniz gerekli rolleri oluşturmak için aşağıdaki komut dosyasını çalıştırın:
    >
    > ```
    >
    > $readerActions = `
    > "Microsoft.Network/networkInterfaces/ipConfigurations/read", `
    > "Microsoft.DataMigration/*/read", `
    > "Microsoft.Resources/subscriptions/resourceGroups/read"
    >
    > $writerActions = `
    > "Microsoft.DataMigration/services/*/write", `
    > "Microsoft.DataMigration/services/*/delete", `
    > "Microsoft.DataMigration/services/*/action", `
    > "Microsoft.Network/virtualNetworks/subnets/join/action", `
    > "Microsoft.Network/virtualNetworks/write", `
    > "Microsoft.Network/virtualNetworks/read", `
    > "Microsoft.Resources/deployments/validate/action", `
    > "Microsoft.Resources/deployments/*/read", `
    > "Microsoft.Resources/deployments/*/write"
    >
    > $writerActions += $readerActions
    >
    > # TODO: replace with actual subscription IDs
    > $subScopes = ,"/subscriptions/00000000-0000-0000-0000-000000000000/","/subscriptions/11111111-1111-1111-1111-111111111111/"
    >
    > function New-DmsReaderRole() {
    > $aRole = [Microsoft.Azure.Commands.Resources.Models.Authorization.PSRoleDefinition]::new()
    > $aRole.Name = "Azure Database Migration Reader"
    > $aRole.Description = "Lets you perform read only actions on DMS service/project/tasks."
    > $aRole.IsCustom = $true
    > $aRole.Actions = $readerActions
    > $aRole.NotActions = @()
    >
    > $aRole.AssignableScopes = $subScopes
    > #Create the role
    > New-AzRoleDefinition -Role $aRole
    > }
    >
    > function New-DmsContributorRole() {
    > $aRole = [Microsoft.Azure.Commands.Resources.Models.Authorization.PSRoleDefinition]::new()
    > $aRole.Name = "Azure Database Migration Contributor"
    > $aRole.Description = "Lets you perform CRUD actions on DMS service/project/tasks."
    > $aRole.IsCustom = $true
    > $aRole.Actions = $writerActions
    > $aRole.NotActions = @()
    >
    >   $aRole.AssignableScopes = $subScopes
    > #Create the role
    > New-AzRoleDefinition -Role $aRole
    > }
    > 
    > function Update-DmsReaderRole() {
    > $aRole = Get-AzRoleDefinition "Azure Database Migration Reader"
    > $aRole.Actions = $readerActions
    > $aRole.NotActions = @()
    > Set-AzRoleDefinition -Role $aRole
    > }
    >
    > function Update-DmsConributorRole() {
    > $aRole = Get-AzRoleDefinition "Azure Database Migration Contributor"
    > $aRole.Actions = $writerActions
    > $aRole.NotActions = @()
    > Set-AzRoleDefinition -Role $aRole
    > }
    >
    > # Invoke above functions
    > New-DmsReaderRole
    > New-DmsContributorRole
    > Update-DmsReaderRole
    > Update-DmsConributorRole
    > ```

## <a name="prerequisites-for-migrating-sql-server-to-azure-sql-database"></a>SQL Server'ı Azure SQL Veritabanına geçirmek için ön koşullar

Tüm geçiş senaryolarında ortak olan Azure Veritabanı Geçiş Hizmeti ön koşullarının yanı sıra, özellikle bir senaryo veya başka bir senaryo için geçerli olan ön koşullar da vardır.

SQL Server to Azure SQL Veritabanı geçişlerini gerçekleştirmek için Azure Veritabanı Geçiş Hizmeti'ni kullanırken, tüm geçiş senaryolarında ortak olan ön koşullara ek olarak aşağıdaki ek ön koşulları ele aldığından emin olun:

* [Azure portalda Azure SQL veritabanı oluşturma](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal) makalesindeki adımları izleyerek bir Azure SQL Veritabanı örneği oluşturun.
* [Data Migration Yardımcısı](https://www.microsoft.com/download/details.aspx?id=53595) 3.3 veya üzeri sürümünü indirip yükleyin.
* Azure Veritabanı Geçiş Hizmeti'ne kaynak SQL Server erişimi sağlamak için Windows güvenlik duvarınızı açın. Varsayılan ayarlarda 1433 numaralı TCP bağlantı noktası kullanılır.
* Dinamik bağlantı noktası kullanarak birden fazla adlandırılmış SQL Server örneği çalıştırıyorsanız, Azure Veritabanı Geçiş Hizmeti'nin kaynak sunucunuzdaki adlandırılmış örneğe bağlanabilmesi için SQL Browser Hizmeti'ni etkinleştirebilir ve güvenlik duvarınızda 1434 numaralı UDP bağlantı noktasına erişim izni verebilirsiniz.
* Azure Veritabanı Geçiş Hizmeti'nin hedef veritabanlarına erişmesini sağlama amacıyla Azure SQL Veritabanı için sunucu düzeyinde [güvenlik duvarı kuralı](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) oluşturun. Azure Veritabanı Geçiş Hizmeti için kullanılan sanal ağın alt ağ aralığını sağlayın.
* SQL Server örneğine bağlanmak için kullanılan kimlik bilgilerinin [CONTROL SERVER](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql) izinlerine sahip olduğundan emin olun.
* Hedef Azure SQL Veritabanı örneğine bağlanmak için kullanılan kimlik bilgilerinin hedef Azure SQL veritabanlarında CONTROL DATABASE iznine sahip olduğundan emin olun.

   > [!NOTE]
   > SQL Server'dan Azure SQL Veritabanı'na geçiş yapmak için Azure Veritabanı Geçiş Hizmeti'ni kullanmak için gereken ön koşulların tam bir listesi için, [SQL Server'ı Azure SQL Veritabanı'na geçir](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-azure-sql)öğreticibölümüne bakın.
   >

## <a name="prerequisites-for-migrating-sql-server-to-an-azure-sql-database-managed-instance"></a>SQL Server'ı Azure SQL Veritabanı'na geçirmek için ön koşullar yönetilen bir örnek

* Makaledeki ayrıntıları izleyerek yönetilen bir Azure SQL Veritabanı örneği oluşturun Azure [portalında bir Azure SQL Veritabanı Yönetilen Örnek Oluşturun.](https://aka.ms/sqldbmi)
* Azure Veritabanı Geçiş Hizmeti IP adresi veya alt net aralığı için 445 bağlantı noktasında Kobİ trafiğine izin vermek için güvenlik duvarlarınızı açın.
* Azure Veritabanı Geçiş Hizmeti'ne kaynak SQL Server erişimi sağlamak için Windows güvenlik duvarınızı açın. Varsayılan ayarlarda 1433 numaralı TCP bağlantı noktası kullanılır.
* Dinamik bağlantı noktası kullanarak birden fazla adlandırılmış SQL Server örneği çalıştırıyorsanız, Azure Veritabanı Geçiş Hizmeti'nin kaynak sunucunuzdaki adlandırılmış örneğe bağlanabilmesi için SQL Browser Hizmeti'ni etkinleştirebilir ve güvenlik duvarınızda 1434 numaralı UDP bağlantı noktasına erişim izni verebilirsiniz.
* Kaynak SQL Server ve hedef Yönetilen Örnek bağlantısı kurmak için kullanılan oturum açma bilgilerinin sysadmin sunucu rolüne üye olduğundan emin olun.
* Azure Veritabanı Geçiş Hizmeti'nin kaynak veritabanını yedeklemek için kullanabileceği bir ağ paylaşımı oluşturun.
* Kaynak SQL Server örneğini çalıştıran hizmet hesabının oluşturduğunuz ağ paylaşımında yazma ayrıcalıklarına sahip olduğundan ve kaynak sunucunun bilgisayar hesabının aynı paylaşımda okuma/yazma erişimine sahip olduğundan emin olun.
* Önceden oluşturduğunuz ağ paylaşımında tam denetim ayrıcalığına sahip olan Windows kullanıcısını (ve parolasını) not edin. Azure Veritabanı Geçiş Hizmeti, yedekleme dosyalarını geri yükleme işlemi için Azure Depolama kapsayıcısına yüklemek için kullanıcı kimlik bilgilerini taklit eder.
* Bir blob kapsayıcısı oluşturun ve [Depolama Gezgini ile Azure Blob Depolama kaynaklarını yönet](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container)makaledeki adımları kullanarak SAS URI'sini alın. SAS URI oluştururken ilke penceresindeki tüm izinleri (Oku, Yaz, Sil, Listele) seçtiğinizden emin olun.

   > [!NOTE]
   > SQL Server'dan Azure SQL Veritabanı Yönetilen Örneği'ne geçiş yapmak için Azure Veritabanı Geçiş Hizmeti'ni kullanmak için gereken ön koşulların tam bir listesi için, [SQL Server'ı Azure SQL Veritabanı Yönetilen Örneği'ne geçir](https://aka.ms/migratetomiusingdms)öğreticisine bakın.

## <a name="next-steps"></a>Sonraki adımlar

Azure Veritabanı Geçiş Hizmeti'ne ve bölgesel kullanılabilirlik egenel bakış için, [Azure Veritabanı Geçiş Hizmeti nedir makalesine](dms-overview.md)bakın.
