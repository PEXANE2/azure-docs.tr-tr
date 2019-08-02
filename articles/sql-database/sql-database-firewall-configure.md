---
title: Azure SQL veritabanı ve veri ambarı IP güvenlik duvarı kuralları | Microsoft Docs
description: Erişimi yönetmek için SQL veritabanı veya SQL veri ambarı güvenlik duvarı kurallarını sunucu düzeyi IP güvenlik duvarı kurallarıyla yapılandırmayı ve veritabanı düzeyinde IP güvenlik duvarı kurallarıyla tek veya havuza alınmış bir veritabanını yapılandırmayı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: VanMSFT
ms.author: vanto
ms.reviewer: carlrab
ms.date: 03/12/2019
ms.openlocfilehash: 39d2dae28bde8ff35408733a1af886c302ec79bf
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68568190"
---
# <a name="azure-sql-database-and-sql-data-warehouse-ip-firewall-rules"></a>Azure SQL veritabanı ve SQL veri ambarı IP güvenlik duvarı kuralları

[SQL veritabanı](sql-database-technical-overview.md) ve [sql veri ambarı](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) Microsoft Azure, Azure ve diğer Internet tabanlı uygulamalar için bir ilişkisel veritabanı hizmeti sağlar. Güvenlik duvarları, verilerinizin korunmasına yardımcı olmak üzere, hangi bilgisayarların izinli olduğunu belirtmenize kadar veritabanı sunucunuza tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.

> [!NOTE]
> Bu makale Azure SQL Server ve Azure SQL Server 'da oluşturulan SQL veritabanı ve SQL veri ambarı veritabanları için geçerlidir. Kolaylık açısından, hem SQL Veritabanı hem de SQL Veri Ambarı için SQL Veritabanı terimi kullanılmaktadır.
> [!IMPORTANT]
> Bu *Makale,* **Azure SQL veritabanı yönetilen örneği**için geçerlidir. Gereken ağ yapılandırması hakkında daha fazla bilgi için lütfen [yönetilen bir örneğe bağlanma](sql-database-managed-instance-connect-app.md) hakkında aşağıdaki makaleye bakın.

## <a name="virtual-network-rules-as-alternatives-to-ip-rules"></a>IP kuralları alternatifleri olarak sanal ağ kuralları

IP kurallarının yanı sıra, güvenlik duvarı da *sanal ağ kurallarını*da yönetir. Sanal ağ kuralları, sanal ağ hizmeti uç noktalarına dayalıdır. Sanal ağ kuralları, bazı durumlarda IP kuralları için tercih edilebilir. Daha fazla bilgi için bkz. [Azure SQL veritabanı Için sanal ağ hizmet uç noktaları ve kuralları](sql-database-vnet-service-endpoint-rule-overview.md).

## <a name="overview"></a>Genel Bakış

Başlangıçta, Azure SQL sunucunuza tüm erişim SQL veritabanı güvenlik duvarı tarafından engellenir. Bir veritabanı sunucusuna erişmek için, Azure SQL sunucunuza erişimi etkinleştiren bir veya daha fazla sunucu düzeyi IP güvenlik duvarı kuralı belirtmeniz gerekir. Internet 'ten hangi IP adresi aralıklarının izin verileceğini ve Azure uygulamalarının Azure SQL sunucunuza bağlanmayı deneyebilir olup olmayacağını belirtmek için IP güvenlik duvarı kurallarını kullanın.

Azure SQL sunucunuzdaki veritabanlarından yalnızca birine seçmeli olarak erişim vermek için, gerekli veritabanına yönelik bir veritabanı düzeyinde kural oluşturmanız gerekir. Veritabanı IP güvenlik duvarı kuralı için, sunucu düzeyinde IP güvenlik duvarı kuralında belirtilen IP adresi aralığının ötesinde bir IP adresi aralığı belirtin ve istemcinin IP adresinin veritabanı düzeyinde kuralda belirtilen aralığa denk olduğundan emin olun.

> [!IMPORTANT]
> SQL veri ambarı yalnızca sunucu düzeyinde IP güvenlik duvarı kurallarını destekler ve veritabanı düzeyinde IP güvenlik duvarı kurallarını desteklemez.

İnternet’ten ve Azure’dan gelen bağlantı denemeleri, Azure SQL sunucunuza veya SQL Veritabanına ulaşmadan önce aşağıdaki diyagramda gösterildiği gibi ilk olarak güvenlik duvarından geçmelidir:

   ![Güvenlik duvarı yapılandırmasını açıklayan diyagram.][1]

- **Sunucu düzeyinde IP güvenlik duvarı kuralları:**

  Bu kurallar, istemcilerin tüm Azure SQL Server 'a, yani aynı SQL veritabanı sunucusundaki tüm veritabanlarına erişmesini sağlar. Bu kurallar **ana** veritabanına depolanır. Sunucu düzeyi IP güvenlik duvarı kuralları, Portal kullanılarak veya Transact-SQL deyimleri kullanılarak yapılandırılabilir. Azure portal veya PowerShell 'i kullanarak sunucu düzeyinde IP güvenlik duvarı kuralları oluşturmak için, abonelik sahibi veya abonelik katılımcısı olmanız gerekir. Transact-SQL kullanarak sunucu düzeyinde bir IP güvenlik duvarı kuralı oluşturmak için SQL veritabanı örneğine sunucu düzeyi asıl oturum açma veya Azure Active Directory yönetici olarak bağlanmanız gerekir (Bu, bir sunucu düzeyi IP güvenlik duvarı kuralının öncelikle bir tarafından oluşturulması gerektiği anlamına gelir. Azure düzeyinde izinlere sahip kullanıcı).

- **Veritabanı düzeyinde IP güvenlik duvarı kuralları:**

  Bu kurallar istemcilerin aynı SQL veritabanı sunucusu içindeki belirli (güvenli) veritabanlarına erişmesini sağlar. Her veritabanı ( **ana** veritabanı dahil) için bu kuralları oluşturabilirsiniz ve ayrı veritabanlarında depolanır. Ana ve kullanıcı veritabanları için veritabanı düzeyinde IP güvenlik duvarı kuralları yalnızca Transact-SQL deyimleri kullanılarak oluşturulabilir ve yönetilebilir ve yalnızca ilk sunucu düzeyinde güvenlik duvarını yapılandırdıktan sonra yönetebilirsiniz. Veritabanı düzeyi IP güvenlik duvarı kuralında sunucu düzeyi IP güvenlik duvarı kuralında belirtilen aralığın dışında bir IP adresi aralığı belirtirseniz, yalnızca veritabanı düzeyinde IP adresleri olan istemciler veritabanına erişebilir. Bir veritabanı için en fazla 128 veritabanı düzeyinde IP güvenlik duvarı kuralına sahip olabilirsiniz. Veritabanı düzeyinde IP güvenlik duvarı kurallarını yapılandırma hakkında daha fazla bilgi için bu makalenin sonraki kısımlarında yer alan örneğe bakın ve bkz. [sp_set_database_firewall_rule (Azure SQL veritabanı)](https://msdn.microsoft.com/library/dn270010.aspx).

### <a name="recommendation"></a>Öneri

Microsoft, güvenliği artırmak ve veritabanınızı daha taşınabilir hale getirmek için mümkün olduğunda veritabanı düzeyinde IP Güvenlik Duvarı kurallarının kullanılmasını önerir. Yöneticiler için sunucu düzeyi IP güvenlik duvarı kurallarını kullanın ve aynı erişim gereksinimlerine sahip birçok veritabanınız varsa ve her veritabanını ayrı ayrı yapılandırmak istemiyorsanız.

> [!IMPORTANT]
> Windows Azure SQL veritabanı en fazla 128 IP güvenlik duvarı kuralını destekler.
> [!NOTE]
> İş sürekliliği bağlamında taşınabilir veritabanları hakkında bilgi edinmek için bkz. [Olağanüstü durum kurtarma için kimlik doğrulama gereksinimleri](sql-database-geo-replication-security-config.md).

### <a name="connecting-from-the-internet"></a>İnternet'ten bağlanma

Bir bilgisayar Internet 'ten veritabanı sunucunuza bağlanmaya çalıştığında, güvenlik duvarı ilk olarak isteğin kaynak IP adresini, bağlantının istediği veritabanı için veritabanı düzeyinde IP güvenlik duvarı kurallarına karşı denetler:

- İsteğin IP adresi veritabanı düzeyinde IP güvenlik duvarı kurallarında belirtilen aralıklardan biri içindeyse, kuralı içeren SQL veritabanına bağlantı verilir.
- İsteğin IP adresi veritabanı düzeyinde IP güvenlik duvarı kuralında belirtilen aralıklardan biri içinde değilse, sunucu düzeyinde IP güvenlik duvarı kuralları denetlenir. İsteğin IP adresi sunucu düzeyinde IP güvenlik duvarı kurallarında belirtilen aralıklardan biri içindeyse, bağlantı verilir. Sunucu düzeyinde IP güvenlik duvarı kuralları, Azure SQL Server 'daki tüm SQL veritabanları için geçerlidir.  
- İsteğin IP adresi, veritabanı düzeyinde veya sunucu düzeyinde IP güvenlik duvarı kurallarında belirtilen aralıklar dahilinde değilse, bağlantı isteği başarısız olur.

> [!NOTE]
> Azure SQL Veritabanına yerel bilgisayarınızdan erişmek için, ağ ve yerel bilgisayarınız üzerindeki güvenlik duvarının TCP bağlantı noktası 1433 üzerinde giden iletişime izin verdiğinden emin olun.

### <a name="connecting-from-azure"></a>Azure'dan bağlanma

Azure’daki uygulamaların Azure SQL sunucunuza bağlanmasına izin verebilmeniz için Azure bağlantılarının etkinleştirilmesi gerekir. Azure’dan bir uygulama, veritabanı sunucunuza bağlanmayı denediğinizde güvenlik duvarı Azure bağlantılarına izin verildiğini doğrular. Başlangıç ve bitiş adreslerini 0.0.0.0 değerine eşit bir güvenlik duvarı ayarı, Azure bağlantılarına izin verildiğini gösterir. Bağlantı denemesine izin verilmezse, istek Azure SQL Veritabanı sunucusuna ulaşmaz.

> [!IMPORTANT]
> Bu seçenek, diğer müşterilerin aboneliklerinden gelen bağlantılar dahil Azure’dan tüm bağlantılara izin verecek şekilde güvenlik duvarınızı yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.

## <a name="creating-and-managing-ip-firewall-rules"></a>IP güvenlik duvarı kuralları oluşturma ve yönetme

Sunucu düzeyinde ilk güvenlik duvarı ayarı, [Azure Portal](https://portal.azure.com/) veya program aracılığıyla [Azure POWERSHELL](https://docs.microsoft.com/powershell/module/az.sql), [Azure CLI](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-create)veya [REST API](https://docs.microsoft.com/rest/api/sql/firewallrules/createorupdate)kullanılarak oluşturulabilir. Sonraki sunucu düzeyi IP güvenlik duvarı kuralları, bu yöntemler ve Transact-SQL aracılığıyla oluşturulabilir ve yönetilebilir.

> [!IMPORTANT]
> Veritabanı düzeyinde IP güvenlik duvarı kuralları yalnızca Transact-SQL kullanılarak oluşturulabilir ve yönetilebilir.

Performansı artırmak için sunucu düzeyi IP güvenlik duvarı kuralları veritabanı düzeyinde geçici olarak önbelleğe alınır. Önbelleği yenilemek için bkz. [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx).

> [!TIP]
> Sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı değişikliklerini denetlemek için [SQL veritabanı denetimini](sql-database-auditing.md) kullanabilirsiniz.

## <a name="manage-server-level-ip-firewall-rules-using-the-azure-portal"></a>Azure portal kullanarak sunucu düzeyi IP güvenlik duvarı kurallarını yönetme

Azure portal sunucu düzeyinde bir IP güvenlik duvarı kuralı ayarlamak için Azure SQL veritabanınızın genel bakış sayfasına ya da SQL veritabanı sunucunuzun genel bakış sayfasına gidebilirsiniz.

> [!TIP]
> Öğretici için, bkz. [Azure Portal kullanarak veritabanı oluşturma](sql-database-single-database-get-started.md).

### <a name="from-database-overview-page"></a>Veritabanına genel bakış sayfasından

1. Veritabanı genel bakış sayfasından sunucu düzeyinde bir IP güvenlik duvarı kuralı ayarlamak için aşağıdaki görüntüde gösterildiği gibi araç çubuğunda **sunucu güvenlik duvarını ayarla** ' ya tıklayın: SQL Veritabanı sunucusu için **Güvenlik duvarı ayarları** sayfası açılır.

      ![Sunucu IP güvenlik duvarı kuralı](./media/sql-database-get-started-portal/server-firewall-rule.png)

2. Kullanmakta olduğunuz bilgisayarın IP adresini eklemek için araç çubuğunda **istemci IP 'Si Ekle** ' ye tıklayın ve ardından **Kaydet**' e tıklayın. Geçerli IP adresiniz için sunucu düzeyinde bir IP güvenlik duvarı kuralı oluşturulur.

      ![Sunucu düzeyi IP güvenlik duvarı kuralını ayarla](./media/sql-database-get-started-portal/server-firewall-rule-set.png)

### <a name="from-server-overview-page"></a>Sunucuya genel bakış sayfasından

Sunucunuzun genel bakış sayfası açılır ve tam sunucu adı (örneğin, **mynewserver20170403.Database.Windows.net**) görüntülenerek daha fazla yapılandırma seçeneği sunulur.

1. Sunucu düzeyinde bir kuralı sunucuya genel bakış sayfasından ayarlamak için, Ayarlar ' ın altındaki sol taraftaki menüden **güvenlik duvarı** ' na tıklayın:

2. Kullanmakta olduğunuz bilgisayarın IP adresini eklemek için araç çubuğunda **istemci IP 'Si Ekle** ' ye tıklayın ve ardından **Kaydet**' e tıklayın. Geçerli IP adresiniz için sunucu düzeyinde bir IP güvenlik duvarı kuralı oluşturulur.

## <a name="manage-ip-firewall-rules-using-transact-sql"></a>Transact-SQL kullanarak IP güvenlik duvarı kurallarını yönetme

| Katalog Görünümü veya Saklı Yordam | Düzey | Açıklama |
| --- | --- | --- |
| [sys.firewall_rules](https://msdn.microsoft.com/library/dn269980.aspx) |Sunucusu |Geçerli sunucu düzeyi IP güvenlik duvarı kurallarını görüntüler |
| [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) |Sunucusu |Sunucu düzeyi IP güvenlik duvarı kuralları oluşturur veya güncelleştirir |
| [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx) |Sunucusu |Sunucu düzeyi IP güvenlik duvarı kurallarını kaldırır |
| [sys.database_firewall_rules](https://msdn.microsoft.com/library/dn269982.aspx) |Database |Geçerli veritabanı düzeyinde IP güvenlik duvarı kurallarını görüntüler |
| [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) |Database |Veritabanı düzeyinde IP güvenlik duvarı kuralları oluşturur veya güncelleştirir |
| [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx) |Veritabanları |Veritabanı düzeyinde IP güvenlik duvarı kurallarını kaldırır |

Aşağıdaki örnekler mevcut kuralları gözden geçirir, contoso sunucusunda bir IP adresi aralığını etkinleştirir ve bir IP güvenlik duvarı kuralını siler:

```sql
SELECT * FROM sys.firewall_rules ORDER BY name;
```

Ardından, sunucu düzeyinde bir IP güvenlik duvarı kuralı ekleyin.

```sql
EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
   @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'
```

Sunucu düzeyinde bir IP güvenlik duvarı kuralını silmek için sp_delete_firewall_rule saklı yordamını yürütün. Aşağıdaki örnek, ContosoFirewallRule adlı kuralı siler:

```sql
EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'
```

## <a name="manage-server-level-ip-firewall-rules-using-azure-powershell"></a>Azure PowerShell kullanarak sunucu düzeyi IP güvenlik duvarı kurallarını yönetme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azure Resource Manager modülü Azure SQL veritabanı tarafından hala desteklenmektedir, ancak gelecekteki tüm geliştirmeler az. SQL modülüne yöneliktir. Bu cmdlet 'ler için bkz. [Azurerd. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az Module ve Azurerd modüllerinde komutların bağımsız değişkenleri önemli ölçüde aynıdır.

| Cmdlet | Düzey | Açıklama |
| --- | --- | --- |
| [Get-AzSqlServerFirewallRule](/powershell/module/az.sql/get-azsqlserverfirewallrule) |Sunucusu |Sunucu düzeyinde geçerli güvenlik duvarı kurallarını döndürür |
| [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) |Sunucusu |Sunucu düzeyinde yeni bir güvenlik duvarı kuralı oluşturur |
| [Set-AzSqlServerFirewallRule](/powershell/module/az.sql/set-azsqlserverfirewallrule) |Sunucusu |Sunucu düzeyinde mevcut güvenlik duvarı kuralının özelliklerini güncelleştirir |
| [Remove-AzSqlServerFirewallRule](/powershell/module/az.sql/remove-azsqlserverfirewallrule) |Sunucusu |Sunucu düzeyinde güvenlik duvarı kurallarını kaldırır |

Aşağıdaki örnek, PowerShell kullanarak sunucu düzeyinde bir IP güvenlik duvarı kuralı ayarlıyor:

```powershell
New-AzSqlServerFirewallRule -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
```

> [!TIP]
> PowerShell örnekleri için hızlı başlangıç bağlamında, bkz. [DB-PowerShell oluşturma](sql-database-powershell-samples.md) ve [tek bir veritabanı oluşturma ve PowerShell kullanarak SQL VERITABANı sunucu düzeyi IP güvenlik duvarı kuralı yapılandırma](scripts/sql-database-create-and-configure-database-powershell.md)

## <a name="manage-server-level-ip-firewall-rules-using-azure-cli"></a>Azure CLı kullanarak sunucu düzeyi IP güvenlik duvarı kurallarını yönetme

| Cmdlet | Düzey | Açıklama |
| --- | --- | --- |
|[az SQL Server Firewall-Rule Create](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-create)|Sunucusu|Sunucu IP güvenlik duvarı kuralı oluşturur|
|[az SQL Server Firewall-Rule List](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-list)|Sunucusu|Bir sunucudaki IP güvenlik duvarı kurallarını listeler|
|[az SQL Server Firewall-Rule Show](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-show)|Sunucusu|Bir IP güvenlik duvarı kuralının ayrıntılarını gösterir|
|[az SQL Server Firewall-Rule Update](/cli/azure/sql/server/firewall-rule##az-sql-server-firewall-rule-update)|Sunucusu|Bir IP güvenlik duvarı kuralını güncelleştirir|
|[az SQL Server Firewall-Rule Delete](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-delete)|Sunucusu|Bir IP güvenlik duvarı kuralını siler|

Aşağıdaki örnek, Azure CLı kullanarak sunucu düzeyinde bir IP güvenlik duvarı kuralı ayarlıyor:

```azurecli-interactive
az sql server firewall-rule create --resource-group myResourceGroup --server $servername \
-n AllowYourIp --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!TIP]
> Hızlı başlangıç bağlamındaki bir Azure CLı örneği için bkz. [Create DB-Azure CLI](sql-database-cli-samples.md) , [Azure CLI kullanarak tek bir VERITABANı oluşturma ve SQL veritabanı IP güvenlik duvarı kuralını yapılandırma](scripts/sql-database-create-and-configure-database-cli.md)

## <a name="manage-server-level-ip-firewall-rules-using-rest-api"></a>REST API kullanarak sunucu düzeyi IP güvenlik duvarı kurallarını yönetme

| API | Düzey | Açıklama |
| --- | --- | --- |
| [Güvenlik Duvarı Kurallarını Listele](https://docs.microsoft.com/rest/api/sql/firewallrules/listbyserver) |Sunucusu |Geçerli sunucu düzeyi IP güvenlik duvarı kurallarını görüntüler |
| [Güvenlik Duvarı Kuralı Oluşturma veya Güncelleştirme](https://docs.microsoft.com/rest/api/sql/firewallrules/createorupdate) |Sunucusu |Sunucu düzeyi IP güvenlik duvarı kuralları oluşturur veya güncelleştirir |
| [Güvenlik Duvarı Kuralını Sil](https://docs.microsoft.com/rest/api/sql/firewallrules/delete) |Sunucusu |Sunucu düzeyi IP güvenlik duvarı kurallarını kaldırır |
| [Güvenlik duvarı kuralları al](https://docs.microsoft.com/rest/api/sql/firewallrules/get) | Sunucusu | Sunucu düzeyi IP güvenlik duvarı kurallarını alır |

## <a name="server-level-versus-database-level-ip-firewall-rules"></a>Sunucu düzeyinde ve veritabanı düzeyinde IP güvenlik duvarı kurallarına karşılık

S. Tek bir veritabanının kullanıcıları başka bir veritabanından tamamen yalıtılmalıdır mi?
Yanıt Evet ise, veritabanı düzeyinde IP güvenlik duvarı kurallarını kullanarak erişim izni verin. Bu, güvenlik duvarı üzerinden tüm veritabanlarına erişime izin veren sunucu düzeyi IP güvenlik duvarı kurallarını kullanmaktan kaçınır ve savunmaları derinliğini azaltır.

S. IP adresinin tüm veritabanlarına erişmesi gereken kullanıcılar mı?
IP güvenlik duvarı kurallarını yapılandırmanın kaç kez yapılandırılacağını azaltmak için sunucu düzeyi IP güvenlik duvarı kurallarını kullanın.

S. IP güvenlik duvarı kurallarını yapılandıran kişi veya takım yalnızca Azure portal, PowerShell veya REST API erişimi olabilir mi?
Sunucu düzeyinde IP güvenlik duvarı kuralları kullanmanız gerekir. Veritabanı düzeyinde IP güvenlik duvarı kuralları yalnızca Transact-SQL kullanılarak yapılandırılabilir.  

S. IP güvenlik duvarı kurallarını yapılandıran kişi veya ekip, veritabanı düzeyinde yüksek düzeyde izinlere sahip olmasını yasakladı mı?
Sunucu düzeyi IP güvenlik duvarı kurallarını kullanın. Transact-SQL kullanarak veritabanı düzeyinde IP güvenlik duvarı kuralları yapılandırmak için veritabanı düzeyinde en `CONTROL DATABASE` az izin gerekir.  

S. Kişi veya takım IP güvenlik duvarı kurallarını yapılandırıyor veya denetletsin, IP güvenlik duvarı kurallarını birçok (Belki de 100s) veritabanı için merkezi olarak yönetebilir mi?
Bu seçim gereksinimlerinize ve ortamınıza bağlıdır. Sunucu düzeyi IP Güvenlik Duvarı kurallarının yapılandırılması daha kolay olabilir, ancak komut dosyası, kuralları veritabanı düzeyinde yapılandırabilir. Sunucu düzeyinde IP güvenlik duvarı kuralları kullanıyor olsanız bile, veritabanında izne sahip olan `CONTROL` kullanıcıların veritabanı düzeyinde IP güvenlik duvarı kuralları oluşturduğunu görmek için veritabanı düzeyinde IP güvenlik duvarı kurallarını denetlemeniz gerekebilir.

S. Sunucu düzeyinde ve veritabanı düzeyinde IP Güvenlik Duvarı kurallarının bir karışımını kullanabilir miyim?
Evet. Yöneticiler gibi bazı kullanıcıların sunucu düzeyinde IP güvenlik duvarı kuralları olması gerekebilir. Bir veritabanı uygulamasının kullanıcıları gibi diğer kullanıcıların veritabanı düzeyinde IP güvenlik duvarı kuralları olması gerekebilir.

## <a name="troubleshooting-the-database-firewall"></a>Veritabanı güvenlik duvarı sorunlarını giderme

Microsoft Azure SQL Veritabanı hizmetine erişim beklediğiniz gibi davranmadığında aşağıdaki noktaları göz önünde bulundurun:

- **Yerel güvenlik duvarı yapılandırması:**

  Bilgisayarınızın Azure SQL veritabanına erişebilmesi için bilgisayarınızda TCP bağlantı noktası 1433 için bir güvenlik duvarı özel durumu oluşturmanız gerekebilir. Azure bulut limitleri içerisinde bağlantı oluşturuyorsanız başka bağlantı noktalarını da açmanız gerekebilir. Daha fazla bilgi için bkz **. SQL veritabanı:**  [ADO.NET 4,5 ve SQL veritabanı için 1433 ' den sonraki bağlantı noktalarının](sql-database-develop-direct-route-ports-adonet-v12.md)iç içe geçmiş bölümünün dışında.

- **Ağ adresi çevirisi (NAT):**

  NAT nedeniyle, bilgisayarınız tarafından Azure SQL veritabanına bağlanmak için kullanılan IP adresi, bilgisayarınızın IP yapılandırması ayarlarınızda gösterilen IP adresinden farklı olabilir. Bilgisayarınızın Azure’a bağlanmak için kullandığı IP adresini görüntülemek üzere portalda oturum açın ve veritabanınızı barındıran sunucudaki **Yapılandır** sekmesine gidin. **İzin Verilen IP Adresleri** bölümü altında **Geçerli İstemci IP Adresi** gösterilir. Bu bilgisayarın sunucuya erişmesine izin vermek için **İzin Verilen IP Adresleri** bölümünde **Ekle**’ye tıklayın.

- **İzin verilenler listesindeki değişiklikler henüz uygulanmadı:**

  Azure SQL veritabanı güvenlik duvarı yapılandırmasındaki değişikliklerin etkili olabilmesi için beş dakikalık bir gecikme olabilir.

- **Oturum açma yetkisi yok veya yanlış parola kullanıldı:**

  Bir oturumun Azure SQL veritabanı sunucusunda izinleri yoksa veya kullanılan parola yanlışsa, Azure SQL veritabanı sunucusu bağlantısı reddedilir. Bir güvenlik duvarı ayarının oluşturulması yalnızca istemcilere sunucunuzla bağlantı kurmayı deneme fırsatı sunar; her istemci gerekli güvenlik kimlik bilgilerini belirtmek zorundadır. Oturumları hazırlama hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı 'Nda veritabanlarını, oturum açmaları ve kullanıcıları yönetme](sql-database-manage-logins.md).

- **Dinamik IP adresi:**

  Dinamik IP adresleme ile Internet bağlantınız varsa ve güvenlik duvarından geçiş konusunda sorun yaşıyorsanız, aşağıdaki çözümlerden birini deneyebilirsiniz:
  
  - Azure SQL veritabanı sunucusuna erişen istemci bilgisayarlarınıza atanan IP adresi aralığı için Internet servis sağlayıcınıza (ISS) sorun ve IP adresi aralığını bir IP güvenlik duvarı kuralı olarak ekleyin.
  - İstemci bilgisayarlarınız yerine statik IP adresleme alın ve IP adreslerini IP güvenlik duvarı kuralları olarak ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

- Şirket ağ ortamınızın, Microsoft Azure veri merkezleri tarafından kullanılan Işlem IP adresi aralıklarından (SQL aralıkları dahil) gelen iletişime izin verdiğini doğrulayın. Bu IP adreslerini beyaz listeye eklemek gerekebilir, bkz. [Microsoft Azure veri MERKEZI IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653)  
- Sunucu düzeyinde bir IP güvenlik duvarı kuralı oluşturma konusunda hızlı bir başlangıç için bkz. [Azure SQL veritabanı oluşturma](sql-database-single-database-get-started.md).
- Açık kaynak veya üçüncü taraf uygulamalardan bir Azure SQL veritabanına bağlanma konusunda yardım için bkz. [SQL Veritabanına yönelik istemci hızlı başlatma kod örnekleri](https://msdn.microsoft.com/library/azure/ee336282.aspx).
- Açmanız gerekebilecek ek bağlantı noktaları hakkında daha fazla bilgi için bkz **. SQL veritabanı:**  [ADO.NET 4,5 ve SQL veritabanı için 1433 ' den sonraki bağlantı noktalarının](sql-database-develop-direct-route-ports-adonet-v12.md) iç içe geçmiş bölümü
- Azure SQL veritabanı güvenliğine genel bakış için bkz. [veritabanınızın güvenliğini sağlama](sql-database-security-overview.md)

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png
