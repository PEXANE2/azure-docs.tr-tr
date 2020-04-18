---
title: IP güvenlik duvarı kuralları
description: BIR SQL veritabanı veya SQL Veri Ambarı güvenlik duvarı için sunucu düzeyinde IP güvenlik duvarı kurallarını yapılandırın. Erişimi yönetin ve veritabanı düzeyindeki IP güvenlik duvarı kurallarını tek veya havuza edilmiş bir veritabanı için yapılandırın.
services: sql-database
ms.service: sql-database
ms.subservice: security
titleSuffix: Azure SQL Database and SQL Data Warehouse
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: VanMSFT
ms.author: vanto
ms.reviewer: carlrab
ms.date: 03/18/2019
ms.openlocfilehash: 12280e8a5b90c6712703fefc60ec1bfb12ba8573
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81606085"
---
# <a name="azure-sql-database-and-azure-sql-data-warehouse-ip-firewall-rules"></a>Azure SQL Veritabanı ve Azure SQL Veri Ambarı IP güvenlik duvarı kuralları

> [!NOTE]
> Bu makale, Azure SQL sunucuları ve Azure SQL veritabanı ve Azure SQL Veritabanı veritabanları için geçerlidir. Basitlik için *SQL Veritabanı* hem SQL Veritabanı hem de SQL Veri Ambarı'nı ifade etmek için kullanılır.

> [!IMPORTANT]
> Bu makale, *Azure SQL Veritabanı Yönetilen Örnek*için geçerli *değildir.* Ağ yapılandırması hakkında bilgi için [bkz.](sql-database-managed-instance-connect-app.md)

Örneğin *mysqlserver*adında yeni bir Azure SQL sunucusu oluşturduğunuzda, SQL Veritabanı güvenlik duvarı sunucunun ortak bitiş noktasına tüm erişimi engeller *(mysqlserver.database.windows.net*erişilebilir).

> [!IMPORTANT]
> SQL Veri Ambarı yalnızca sunucu düzeyindeIP güvenlik duvarı kurallarını destekler. Veritabanı düzeyindeIP güvenlik duvarı kurallarını desteklemez.

## <a name="how-the-firewall-works"></a>Güvenlik duvarı nasıl çalışır?
Aşağıdaki diyagramda görüldüğü gibi, Internet ve Azure'dan gelen bağlantı girişimleri, SQL sunucunuza veya SQL veritabanınıza ulaşmadan önce güvenlik duvarından geçmelidir.

   ![Güvenlik duvarı yapılandırma diyagramı][1]

### <a name="server-level-ip-firewall-rules"></a>Sunucu düzeyinde IP güvenlik duvarı kuralları

  Bu kurallar, istemcilerin tüm Azure SQL sunucunuza, yani aynı SQL Veritabanı sunucusundaki tüm veritabanlarına erişmesini sağlar. Kurallar *ana* veritabanında depolanır. Bir Azure SQL Server için en fazla 128 sunucu düzeyinde IP güvenlik duvarı kuralına sahip olabilirsiniz. Azure Hizmetlerine **İzin Ver'in ve bu sunucu ayarına erişebilmek için kaynaklarınız** varsa, bu Azure SQL Server için tek bir güvenlik duvarı kuralı olarak sayılır.
  
  Azure portalı, PowerShell veya Transact-SQL deyimlerini kullanarak sunucu düzeyindeIP güvenlik duvarı kurallarını yapılandırabilirsiniz.
  - Portalı veya PowerShell'i kullanmak için abonelik sahibi veya abonelik katılımcısı olmalısınız.
  - Transact-SQL'i kullanmak için, SUNUCU düzeyindeasıl giriş olarak veya Azure Active Directory yöneticisi olarak SQL Veritabanı örneğine bağlanmanız gerekir. (Sunucu düzeyinde bir IP güvenlik duvarı kuralı ilk olarak Azure düzeyinde izinleri olan bir kullanıcı tarafından oluşturulmalıdır.)

### <a name="database-level-ip-firewall-rules"></a>Veritabanı düzeyinde IP güvenlik duvarı kuralları

  Bu kurallar, istemcilerin aynı SQL Veritabanı sunucusuiçinde belirli (güvenli) veritabanlarına erişmesini sağlar. Her veritabanı *(ana* veritabanı dahil) için kurallar oluşturursunuz ve bunlar tek tek veritabanında depolanır.
  
  Yalnızca Transact-SQL deyimlerini kullanarak ve yalnızca ilk sunucu düzeyindegüvenlik duvarını yapılandırıldıktan sonra ana ve kullanıcı veritabanları için veritabanı düzeyinde IP güvenlik duvarı kuralları oluşturabilir ve yönetebilirsiniz.
  
  Sunucu düzeyindeki IP güvenlik duvarı kuralında aralığın dışında olan veritabanı düzeyindeki IP güvenlik duvarı kuralında bir IP adresi aralığı belirtirseniz, veritabanı düzeyinde ki aralıkta yalnızca IP adresleri olan istemciler veritabanına erişebilir.
  
  Bir veritabanı için en fazla 128 veritabanı düzeyinde IP güvenlik duvarı kuralına sahip olabilirsiniz. Veritabanı düzeyindeki IP güvenlik duvarı kurallarının yapılandırılması hakkında daha fazla bilgi için, bu makalenin ilerleyen bilgilerine bakın ve [sp_set_database_firewall_rule (Azure SQL Veritabanı)](https://msdn.microsoft.com/library/dn270010.aspx)bakın.

### <a name="recommendations-for-how-to-set-firewall-rules"></a>Güvenlik duvarı kurallarının nasıl ayarlanabildiğini anlatan öneriler

Mümkün olduğunda veritabanı düzeyinde IP güvenlik duvarı kurallarını kullanmanızı öneririz. Bu uygulama güvenliği artırır ve veritabanınızı daha taşınabilir hale getirir. Yöneticiler için sunucu düzeyinde IP güvenlik duvarı kurallarını kullanın. Ayrıca, aynı erişim gereksinimlerine sahip birçok veritabanınız varsa ve her veritabanını ayrı ayrı yapılandırmak istemiyorsanız da bunları kullanın.

> [!NOTE]
> İş sürekliliği bağlamında taşınabilir veritabanları hakkında bilgi edinmek için bkz. [Olağanüstü durum kurtarma için kimlik doğrulama gereksinimleri](sql-database-geo-replication-security-config.md).

## <a name="server-level-versus-database-level-ip-firewall-rules"></a>Sunucu düzeyi ne kadar veritabanı düzeyinde IP güvenlik duvarı kuralları

*Bir veritabanının kullanıcıları başka bir veritabanından tamamen izole edilmeli mi?*

*Evet ise,* erişim sağlamak için veritabanı düzeyindeIP güvenlik duvarı kurallarını kullanın. Bu yöntem, güvenlik duvarı üzerinden tüm veritabanlarına erişime izin veren sunucu düzeyindeIP güvenlik duvarı kurallarının kullanılmasını önler. Bu savunmanın derinliğini azaltır.

*IP adreslerindeki kullanıcıların tüm veritabanlarına erişmeye ihtiyacı var mı?*

*Evet ise,* IP güvenlik duvarı kurallarını yapılandırmanız gereken sayısını azaltmak için sunucu düzeyindeKI IP güvenlik duvarı kurallarını kullanın.

*IP güvenlik duvarı kurallarını yapılandıran kişi veya ekip yalnızca Azure portalı, PowerShell veya REST API üzerinden erişebilir mi?*

Bu nedenle, sunucu düzeyinde IP güvenlik duvarı kurallarını kullanmanız gerekir. Veritabanı düzeyindeKI IP güvenlik duvarı kuralları yalnızca Transact-SQL ile yapılandırılabilir.  

*IP güvenlik duvarı kurallarını yapılandıran kişi veya ekibin veritabanı düzeyinde üst düzey izin almaları yasak mıdır?*

Bu nedenle, sunucu düzeyindeIP güvenlik duvarı kurallarını kullanın. Transact-SQL üzerinden veritabanı düzeyindeIP güvenlik duvarı kurallarını yapılandırmak için veritabanı düzeyinde en az *CONTROL DATABASE* iznine ihtiyacınız vardır.  

*IP güvenlik duvarı kurallarını yapılandıran veya denetleyen kişi veya ekip, birçok veritabanı için (belki de yüzlerce) IP güvenlik duvarı kurallarını merkezi olarak yönetiyor mu?*

Bu senaryoda, en iyi uygulamalar ihtiyaçlarınız ve ortamınız tarafından belirlenir. Sunucu düzeyindeki IP güvenlik duvarı kurallarını yapılandırmak daha kolay olabilir, ancak komut dosyası oluşturma veritabanı düzeyinde kuralları yapılandırabilir. Sunucu düzeyindeIP güvenlik duvarı kuralları kullansanız bile, veritabanında *CONTROL* izni olan kullanıcıların veritabanı düzeyinde IP güvenlik duvarı kuralları oluşturup oluşturmadığını görmek için veritabanı düzeyindeKI IP güvenlik duvarı kurallarını denetlemeniz gerekebilir.

*Sunucu düzeyi ve veritabanı düzeyindeIP güvenlik duvarı kurallarının bir karışımını kullanabilir miyim?*

Evet. Yöneticiler gibi bazı kullanıcıların sunucu düzeyinde IP güvenlik duvarı kurallarına ihtiyacı olabilir. Veritabanı uygulaması kullanıcıları gibi diğer kullanıcıların veritabanı düzeyinde IP güvenlik duvarı kuralları na ihtiyacı olabilir.

### <a name="connections-from-the-internet"></a>Internet'ten bağlantılar

Bir bilgisayar veritabanı sunucunuza Internet'ten bağlanmaya çalıştığında, güvenlik duvarı önce isteğin menşeli IP adresini bağlantının istediği veritabanı için veritabanı düzeyindeki IP güvenlik duvarı kurallarıyla karşıdenetler.

- Adres veritabanı düzeyindeIP güvenlik duvarı kurallarında belirtilen bir aralıktaysa, bağlantı kuralı içeren SQL veritabanına verilir.
- Adres veritabanı düzeyindeki IP güvenlik duvarı kurallarında bir aralık içinde değilse, güvenlik duvarı sunucu düzeyindeKI IP güvenlik duvarı kurallarını denetler. Adres, sunucu düzeyindeki IP güvenlik duvarı kurallarına sahip bir aralıktaysa, bağlantı verilir. Sunucu düzeyindeki IP güvenlik duvarı kuralları, Azure SQL sunucusundaki tüm SQL veritabanları için geçerlidir.  
- Adres, veritabanı düzeyindeveya sunucu düzeyindeki IP güvenlik duvarı kurallarının herhangi birinde olan bir aralıkta değilse, bağlantı isteği başarısız olur.

> [!NOTE]
> Yerel bilgisayarınızdan SQL Veritabanı'na erişmek için, ağınızdaki ve yerel bilgisayarınızdaki güvenlik duvarının TCP bağlantı noktası 1433'te giden iletişime izin verdiğinden emin olun.

### <a name="connections-from-inside-azure"></a>Azure'un içinden bağlantılar

Azure'da barındırılan uygulamaların SQL sunucunuza bağlanmasına izin vermek için Azure bağlantılarının etkinleştirilmesi gerekir. Azure'daki bir uygulama veritabanı sunucunuza bağlanmaya çalıştığında, güvenlik duvarı Azure bağlantılarına izin verildiğini doğrular. Bu işlem, Güvenlik Duvarı kuralları ayarlayarak ve Bu sunucuya **Güvenlik Duvarları ve sanal ağlar** ayarlarında **Açık** olarak erişmek için Azure Hizmetlerine **ve kaynaklarına geçiş** yaparak doğrudan Azure Portal bladeinden açılabilir. Bağlantıya izin verilmiyorsa, istek SQL Veritabanı sunucusuna ulaşmaz.

> [!IMPORTANT]
> Bu seçenek, güvenlik duvarını, diğer müşterilerin abonelikleri de dahil olmak üzere Azure'daki tüm bağlantılara izin verecek şekilde yapılandırır. Bu seçeneği belirlerseniz, oturum açma ve kullanıcı izinlerinizin yalnızca yetkili kullanıcılara erişimi sınırladığından emin olun.

## <a name="create-and-manage-ip-firewall-rules"></a>IP güvenlik duvarı kuralları oluşturma ve yönetme

[Azure portalını](https://portal.azure.com/) kullanarak veya [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.sql), [Azure CLI](https://docs.microsoft.com/cli/azure/sql/server/firewall-rule)veya Azure [REST API'sini](https://docs.microsoft.com/rest/api/sql/firewallrules/createorupdate)kullanarak ilk sunucu düzeyinde güvenlik duvarı ayarını oluşturursunuz. Bu yöntemleri veya Transact-SQL'i kullanarak ek sunucu düzeyinde IP güvenlik duvarı kuralları oluşturur ve yönetirsiniz.

> [!IMPORTANT]
> Veritabanı düzeyindeKI IP güvenlik duvarı kuralları yalnızca Transact-SQL kullanılarak oluşturulabilir ve yönetilebilir.

Performansı artırmak için sunucu düzeyindeki IP güvenlik duvarı kuralları veritabanı düzeyinde geçici olarak önbelleğe alınır. Önbelleği yenilemek için bkz. [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx).

> [!TIP]
> Sunucu düzeyi ve veritabanı düzeyindegüvenlik değişiklikleri denetlemek için [SQL Veritabanı Denetimi'ni](sql-database-auditing.md) kullanabilirsiniz.

### <a name="use-the-azure-portal-to-manage-server-level-ip-firewall-rules"></a>Sunucu düzeyindeKI IP güvenlik duvarı kurallarını yönetmek için Azure portalını kullanın

Azure portalında sunucu düzeyinde bir IP güvenlik duvarı kuralı ayarlamak için Azure SQL veritabanınız veya SQL Veritabanı sunucunuz için genel bakış sayfasına gidin.

> [!TIP]
> Bir öğretici için Azure [portalını kullanarak DB oluşturma'ya](sql-database-single-database-get-started.md)bakın.

#### <a name="from-the-database-overview-page"></a>Veritabanına genel bakış sayfasından

1. Veritabanına genel bakış sayfasından sunucu düzeyinde bir IP güvenlik duvarı kuralı ayarlamak için, aşağıdaki resimde görüldüğü gibi araç çubuğunda **sunucu güvenlik duvarını** ayarla'yı seçin. 

    ![Sunucu IP güvenlik duvarı kuralı](./media/sql-database-get-started-portal/sql-database-server-set-firewall-rule.png)

    SQL Veritabanı sunucusu için **Güvenlik duvarı ayarları** sayfası açılır.

2. Kullandığınız bilgisayarın IP adresini eklemek için araç çubuğuna **istemci IP ekle'yi** seçin ve ardından **Kaydet'i**seçin. Geçerli IP adresiniz için sunucu düzeyinde bir IP güvenlik duvarı kuralı oluşturulur.

    ![Sunucu düzeyinde IP güvenlik duvarı kuralını ayarlama](./media/sql-database-get-started-portal/sql-database-server-firewall-settings.png)

#### <a name="from-the-server-overview-page"></a>Sunucuya genel bakış sayfasından

Sunucunuz için genel bakış sayfası açılır. Tam nitelikli sunucu adını *(mynewserver20170403.database.windows.net*gibi) gösterir ve daha fazla yapılandırma için seçenekler sağlar.

1. Bu sayfadan sunucu düzeyinde bir kural ayarlamak için, sol taraftaki **Ayarlar** menüsünden **Güvenlik Duvarı'nı** seçin.

2. Kullandığınız bilgisayarın IP adresini eklemek için araç çubuğuna **istemci IP ekle'yi** seçin ve ardından **Kaydet'i**seçin. Geçerli IP adresiniz için sunucu düzeyinde bir IP güvenlik duvarı kuralı oluşturulur.

### <a name="use-transact-sql-to-manage-ip-firewall-rules"></a>IP güvenlik duvarı kurallarını yönetmek için Transact-SQL'i kullanın

| Katalog görünümü veya depolanan yordam | Düzey | Açıklama |
| --- | --- | --- |
| [sys.firewall_rules](https://msdn.microsoft.com/library/dn269980.aspx) |Sunucu |Geçerli sunucu düzeyindeki IP güvenlik duvarı kurallarını görüntüler |
| [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) |Sunucu |Sunucu düzeyinde IP güvenlik duvarı kuralları oluşturur veya güncelleştirir |
| [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx) |Sunucu |Sunucu düzeyindeKI IP güvenlik duvarı kurallarını kaldırır |
| [sys.database_firewall_rules](https://msdn.microsoft.com/library/dn269982.aspx) |Veritabanı |Geçerli veritabanı düzeyindeki IP güvenlik duvarı kurallarını görüntüler |
| [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) |Veritabanı |Veritabanı düzeyindeIP güvenlik duvarı kurallarını oluşturur veya güncelleştirir |
| [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx) |Veritabanları |Veritabanı düzeyindeIP güvenlik duvarı kurallarını kaldırır |

Aşağıdaki örnek, varolan kuralları gözden geçirir, sunucu *Contoso'da*bir dizi IP adresine olanak tanır ve bir IP güvenlik duvarı kuralını siler:

```sql
SELECT * FROM sys.firewall_rules ORDER BY name;
```

Ardından, sunucu düzeyinde bir IP güvenlik duvarı kuralı ekleyin.

```sql
EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
   @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'
```

Sunucu düzeyindebir IP güvenlik duvarı kuralını silmek için, depolanan *sp_delete_firewall_rule* yordamı çalıştırın. Aşağıdaki örnek *kural ContosoFirewallRule*siler:

```sql
EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'
```

### <a name="use-powershell-to-manage-server-level-ip-firewall-rules"></a>Sunucu düzeyindeKI IP güvenlik duvarı kurallarını yönetmek için PowerShell'i kullanın 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azure Kaynak Yöneticisi modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak tüm geliştirme artık Az.Sql modülü içindir. Bu cmdlets için [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)bakın. Az ve AzureRm modüllerinde komutlar için bağımsız değişkenler önemli ölçüde aynıdır.

| Cmdlet | Düzey | Açıklama |
| --- | --- | --- |
| [Get-AzSqlServerFirewallRule](/powershell/module/az.sql/get-azsqlserverfirewallrule) |Sunucu |Sunucu düzeyinde geçerli güvenlik duvarı kurallarını döndürür |
| [Yeni-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) |Sunucu |Sunucu düzeyinde yeni bir güvenlik duvarı kuralı oluşturur |
| [Set-AzSqlServerFirewallRule](/powershell/module/az.sql/set-azsqlserverfirewallrule) |Sunucu |Sunucu düzeyinde mevcut güvenlik duvarı kuralının özelliklerini güncelleştirir |
| [Kaldır-AzSqlServerFirewallRule](/powershell/module/az.sql/remove-azsqlserverfirewallrule) |Sunucu |Sunucu düzeyinde güvenlik duvarı kurallarını kaldırır |

Aşağıdaki örnekte, sunucu düzeyinde bir IP güvenlik duvarı kuralı ayarlamak için PowerShell kullanır:

```powershell
New-AzSqlServerFirewallRule -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -FirewallRuleName "ContosoIPRange" -StartIpAddress "192.168.1.0" -EndIpAddress "192.168.1.255"
```
> [!TIP]
> $servername sunucu adını değil, tam nitelikli DNS adını belirtin **mysqldbserver.database.windows.net** yerine **mysqldbserver** belirtin

> [!TIP]
> Hızlı başlangıç bağlamında PowerShell örnekleri için [DB Oluştur - PowerShell](sql-database-powershell-samples.md) ve [Tek bir veritabanı oluştur ve PowerShell kullanarak BIR SQL Veritabanı sunucu düzeyinde IP güvenlik duvarı kuralını yapılandırın.](scripts/sql-database-create-and-configure-database-powershell.md)

### <a name="use-cli-to-manage-server-level-ip-firewall-rules"></a>Sunucu düzeyindeKI IP güvenlik duvarı kurallarını yönetmek için CLI'yi kullanın

| Cmdlet | Düzey | Açıklama |
| --- | --- | --- |
|[az sql server güvenlik duvarı kuralı oluşturma](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-create)|Sunucu|Sunucu IP güvenlik duvarı kuralı oluşturur|
|[az sql server güvenlik duvarı kural listesi](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-list)|Sunucu|Sunucudaki IP güvenlik duvarı kurallarını listeler|
|[az sql server güvenlik duvarı kuralı göster](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-show)|Sunucu|IP güvenlik duvarı kuralının ayrıntılarını gösterir|
|[az sql server güvenlik duvarı kuralı güncellemesi](/cli/azure/sql/server/firewall-rule##az-sql-server-firewall-rule-update)|Sunucu|IP güvenlik duvarı kuralını güncelleştirir|
|[az sql server güvenlik duvarı kuralı silme](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-delete)|Sunucu|IP güvenlik duvarı kuralını siler|

Aşağıdaki örnek, sunucu düzeyinde bir IP güvenlik duvarı kuralı ayarlamak için CLI kullanır:

```azurecli-interactive
az sql server firewall-rule create --resource-group myResourceGroup --server $servername \
-n ContosoIPRange --start-ip-address 192.168.1.0 --end-ip-address 192.168.1.255
```
> [!TIP]
> $servername sunucu adını değil, tam nitelikli DNS adını belirtin **mysqldbserver.database.windows.net** yerine **mysqldbserver** belirtin

> [!TIP]
> Hızlı başlatma bağlamında bir CLI örneği için [DB Oluştur - Azure CLI](sql-database-cli-samples.md) ve [Tek bir veritabanı oluştur ve Azure CLI'yi kullanarak BIR SQL Veritabanı IP güvenlik duvarı kuralını yapılandırın.](scripts/sql-database-create-and-configure-database-cli.md)

### <a name="use-a-rest-api-to-manage-server-level-ip-firewall-rules"></a>Sunucu düzeyindeKI IP güvenlik duvarı kurallarını yönetmek için REST API'si kullanın

| API | Düzey | Açıklama |
| --- | --- | --- |
| [Güvenlik duvarı kurallarını listele](https://docs.microsoft.com/rest/api/sql/firewallrules/listbyserver) |Sunucu |Geçerli sunucu düzeyindeki IP güvenlik duvarı kurallarını görüntüler |
| [Güvenlik duvarı kuralları oluşturma veya güncelleştirme](https://docs.microsoft.com/rest/api/sql/firewallrules/createorupdate) |Sunucu |Sunucu düzeyinde IP güvenlik duvarı kuralları oluşturur veya güncelleştirir |
| [Güvenlik duvarı kurallarını silme](https://docs.microsoft.com/rest/api/sql/firewallrules/delete) |Sunucu |Sunucu düzeyindeKI IP güvenlik duvarı kurallarını kaldırır |
| [Güvenlik duvarı kurallarını alın](https://docs.microsoft.com/rest/api/sql/firewallrules/get) | Sunucu | Sunucu düzeyinde IP güvenlik duvarı kurallarını alır |

## <a name="troubleshoot-the-database-firewall"></a>Veritabanı güvenlik duvarını sorun giderme

SQL Veritabanı hizmetine erişim beklediğiniz gibi görünmüyorsa aşağıdaki noktaları göz önünde bulundurun.

- **Yerel güvenlik duvarı yapılandırması:**

  Bilgisayarınız SQL Veritabanı'na erişemeden önce, TCP bağlantı noktası 1433 için bilgisayarınızda bir güvenlik duvarı özel durumu oluşturmanız gerekebilir. Azure bulut sınırı içinde bağlantı yapmak için ek bağlantı noktaları açmanız gerekebilir. Daha fazla bilgi için, [ADO.NET 4.5 ve SQL Veritabanı için 1433 ötesinde Ports](sql-database-develop-direct-route-ports-adonet-v12.md)"SQL Veritabanı: Dış vs içinde" bölümüne bakın.

- **Ağ adresi çevirisi:**

  Ağ adresi çevirisi (NAT) nedeniyle, bilgisayarınız tarafından SQL Veritabanı'na bağlanmak için kullanılan IP adresi, bilgisayarınızın IP yapılandırma ayarlarındaki IP adresinden farklı olabilir. Bilgisayarınızın Azure'a bağlanmak için kullandığı IP adresini görüntülemek için:
    1. Portalda oturum açın.
    1. Veritabanınızı barındıran sunucudaki **Yapılandırma** sekmesine gidin.
    1. **Geçerli İstemci IP Adresi,** **İzin Verilen IP Adresleri** bölümünde görüntülenir. Bu bilgisayarın sunucuya erişmesine izin vermek için **İzin Verilen IP Adresleri** için **Ekle'yi** seçin.

- **İzin veren ler listesindeyapılan değişiklikler henüz yürürlüğe girmedi:**

  SQL Veritabanı güvenlik duvarı yapılandırmasıdeğişikliklerinin etkili olması için beş dakikaya kadar gecikme olabilir.

- **Oturum açma yetkisi yok veya yanlış bir parola kullanıldı:**

  Oturum açmanın SQL Veritabanı sunucusunda izinleri yoksa veya parola yanlışsa, sunucuya bağlantı reddedilir. Güvenlik duvarı ayarı oluşturmak, istemcilere yalnızca sunucunuza bağlanmayı deneme *fırsatı* verir. İstemci yine de gerekli güvenlik kimlik bilgilerini sağlamalıdır. Oturum açma hakkında daha fazla bilgi için [bkz.](sql-database-manage-logins.md)

- **Dinamik IP adresi:**

  Dinamik IP adresi kullanan bir internet bağlantınız varsa ve güvenlik duvarını geçmekte sorun yaşıyorsanız, aşağıdaki çözümlerden birini deneyin:
  
  - SQL Veritabanı sunucusuna erişen istemci bilgisayarlarınıza atanan IP adresi aralığını internet servis sağlayıcınızdan isteyin. Ip adresi aralığını IP güvenlik duvarı kuralı olarak ekleyin.
  - İstemci bilgisayarlarınız için statik IP adresi alın. IP adreslerini IP güvenlik duvarı kuralları olarak ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

- Kurumsal ağ ortamınızın Azure veri merkezleri tarafından kullanılan bilgi işlem IP adres aralıklarından (SQL aralıkları dahil) gelen iletişime izin verdiğini doğrulayın. Bu IP adreslerini izin listesine eklemeniz gerekebilir. Bkz. [Microsoft Azure veri merkezi IP aralıkları.](https://www.microsoft.com/download/details.aspx?id=41653)  
- Sunucu düzeyinde BIR IP güvenlik duvarı kuralı oluşturma hakkında hızlı bir başlangıç için [bkz.](sql-database-single-database-get-started.md)
- Açık kaynak veya üçüncü taraf uygulamalardan bir Azure SQL veritabanına bağlanma konusunda yardım için, [SQL Veritabanı'na Istemci hızlı başlatma kodu örneklerine](https://msdn.microsoft.com/library/azure/ee336282.aspx)bakın.
- Açmanız gereken ek bağlantı noktaları hakkında bilgi için, [ADO.NET 4.5 ve SQL Veritabanı için 1433'ten sonraki Ports'un](sql-database-develop-direct-route-ports-adonet-v12.md) "SQL Veritabanı: Dış vs iç" bölümüne bakın
- Azure SQL Veritabanı güvenliğine genel bakış için [veritabanınızı güvence altına alma](sql-database-security-overview.md)bölümüne bakın.

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png
