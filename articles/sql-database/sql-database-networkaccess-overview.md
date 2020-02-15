---
title: Ağ erişim denetimleri
description: Erişimi yönetmek ve tek veya havuza alınmış bir veritabanını yapılandırmak için Azure SQL veritabanı ve veri ambarı için ağ erişim denetimlerine genel bakış.
services: sql-database
ms.service: sql-database
ms.subservice: security
titleSuffix: Azure SQL Database and SQL Data Warehouse
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: vanto
ms.date: 08/05/2019
ms.openlocfilehash: 16ba90aab52c00f77af590f854217cd989df53b3
ms.sourcegitcommit: 79cbd20a86cd6f516acc3912d973aef7bf8c66e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77251915"
---
# <a name="azure-sql-database-and-data-warehouse-network-access-controls"></a>Azure SQL veritabanı ve veri ambarı ağ erişim denetimleri

> [!NOTE]
> Bu makale Azure SQL Server ve Azure SQL Server 'da oluşturulan SQL veritabanı ve SQL veri ambarı veritabanları için geçerlidir. Kolaylık açısından, hem SQL Veritabanı hem de SQL Veri Ambarı için SQL Veritabanı terimi kullanılmaktadır.

> [!IMPORTANT]
> Bu *Makale,* **Azure SQL veritabanı yönetilen örneği**için geçerlidir. ağ yapılandırması hakkında daha fazla bilgi için bkz. [yönetilen örneğe bağlanma](sql-database-managed-instance-connect-app.md) .

[Azure Portal](sql-database-single-database-get-started.md)yeni bir Azure SQL Server oluşturduğunuzda sonuç, *yourservername.Database.Windows.net*biçimindeki genel bir uç noktadır. Tasarıma göre, genel uç noktaya tüm erişim reddedilir. Daha sonra, genel uç nokta aracılığıyla SQl veritabanı erişimine seçmeli olarak izin vermek için aşağıdaki ağ erişim denetimlerini kullanabilirsiniz
- Azure hizmetlerine izin ver:-açık olarak ayarlandığında, Azure sınırının içindeki diğer kaynaklar, örneğin bir Azure sanal makinesi SQL veritabanına erişebilir

- IP güvenlik duvarı kuralları:-Bu özelliği, örneğin şirket içi makinelerden gelen belirli bir IP adresinden gelen bağlantılara açıkça izin vermek için kullanın.

- Sanal ağ güvenlik duvarı kuralları:-Azure sınırları içindeki belirli bir sanal ağdan gelen trafiğe izin vermek için bu özelliği kullanın

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Data-Exposed--SQL-Database-Connectivity-Explained/player?WT.mc_id=dataexposed-c9-niner]

## <a name="allow-azure-services"></a>Azure hizmetlerine izin ver 
[Azure Portal](sql-database-single-database-get-started.md)yeni bir Azure SQL Server oluşturma sırasında, bu ayar işaretsiz bırakılır.

 ![Yeni sunucu oluşturma ekranının ekran görüntüsü][1]

Bu ayarı, Azure SQL Server aşağıdaki gibi oluşturulduktan sonra güvenlik duvarı bölmesi aracılığıyla da değiştirebilirsiniz.
  
 ![Sunucu güvenlik duvarını yönetme ekran görüntüsü][2]

Azure SQL Server olarak ayarlandığında **, Azure sınırının** içindeki tüm kaynaklardan gelen iletişimlere izin verir, bu da aboneliğinizin bir parçası olmayabilir veya olmayabilir.

Çoğu durumda, **Açık** ayarı, çoğu müşterinin izin verilenden daha fazla izne sahiptir. Bu ayarı **kapalı** olarak ayarlamak ve daha kısıtlayıcı IP güvenlik duvarı kuralları ya da sanal ağ güvenlik duvarı kuralları ile değiştirmek isteyebilir. Bunun yapılması, Azure 'da sanal ağınızın parçası olmayan ve bu nedenle bir Azure IP adresi aracılığıyla SQL veritabanı 'na bağlanan VM 'lerde çalışan aşağıdaki özellikleri etkiler.

### <a name="import-export-service"></a>İçeri aktarma hizmeti al
İçeri aktarma verme hizmeti, **Azure hizmetlerinin sunucuya erişmesine Izin ver** ayarı çalışmıyor. Ancak, [SqlPackage. exe ' yi bir Azure VM 'sinden el ile çalıştırarak veya](https://docs.microsoft.com/azure/sql-database/import-export-from-vm) DACFX API kullanarak doğrudan kodunuzda dışarı aktarmayı gerçekleştirerek soruna geçici bir çözüm bulabilirsiniz.

### <a name="data-sync"></a>Data Sync
Veri eşitleme özelliğini, **Azure hizmetlerinin sunucuya erişmesine Izin ver** özelliği kapalı olarak kullanmak Için, **hub** veritabanını barındıran BÖLGENIN **SQL hizmeti etiketinden** [IP adresleri eklemek](sql-database-server-level-firewall-rule.md) üzere ayrı ayrı güvenlik duvarı kuralı girdileri oluşturmanız gerekir.
Bu sunucu düzeyi güvenlik duvarı kurallarını hem **hub** 'ı hem de **üye** veritabanlarını (farklı bölgelerde olabilir) barındıran mantıksal sunuculara ekleyin

Batı ABD bölgenin SQL Service etiketine karşılık gelen IP adreslerini oluşturmak için aşağıdaki PowerShell betiğini kullanın
```powershell
PS C:\>  $serviceTags = Get-AzNetworkServiceTag -Location eastus2
PS C:\>  $sql = $serviceTags.Values | Where-Object { $_.Name -eq "Sql.WestUS" }
PS C:\> $sql.Properties.AddressPrefixes.Count
70
PS C:\> $sql.Properties.AddressPrefixes
13.86.216.0/25
13.86.216.128/26
13.86.216.192/27
13.86.217.0/25
13.86.217.128/26
13.86.217.192/27
```

> [!TIP]
> Get-AzNetworkServiceTag, location parametresini belirtirken sql Service etiketinin genel aralığını döndürür. Eşitleme grubunuz tarafından kullanılan Merkez veritabanını barındıran bölgeye filtre uyguladığınızdan emin olun

PowerShell betiğinin çıktısının sınıfsız etki alanları arası yönlendirme (CıDR) gösterimi içinde ve [Get-IPrangeStartEnd. ps1](https://gallery.technet.microsoft.com/scriptcenter/Start-and-End-IP-addresses-bcccc3a9) kullanarak bir başlangıç ve bitiş IP adresi biçimine dönüştürülmesi gerektiğini unutmayın
```powershell
PS C:\> Get-IPrangeStartEnd -ip 52.229.17.93 -cidr 26                                                                   
start        end
-----        ---
52.229.17.64 52.229.17.127
```

CıDR 'ten gelen tüm IP adreslerini başlangıç ve bitiş IP adresi biçimine dönüştürmek için aşağıdaki ek adımları uygulayın.

```powershell
PS C:\>foreach( $i in $sql.Properties.AddressPrefixes) {$ip,$cidr= $i.split('/') ; Get-IPrangeStartEnd -ip $ip -cidr $cidr;}                                                                                                                
start          end
-----          ---
13.86.216.0    13.86.216.127
13.86.216.128  13.86.216.191
13.86.216.192  13.86.216.223
```
Artık bunları farklı güvenlik duvarı kuralları olarak ekleyebilir ve sonra **Azure hizmetlerinin sunucuya erişmesine Izin ver** seçeneğini ayarlayabilirsiniz.


## <a name="ip-firewall-rules"></a>IP güvenlik duvarı kuralları
IP tabanlı güvenlik duvarı, istemci makinelerin [IP adreslerini açıkça eklemeene](sql-database-server-level-firewall-rule.md) kadar veritabanı sunucunuza tüm erişimi önleyen bir Azure SQL Server özelliğidir.


## <a name="virtual-network-firewall-rules"></a>Sanal ağ güvenlik duvarı kuralları

Azure SQL Server güvenlik duvarı, IP kurallarına ek olarak *sanal ağ kurallarını*tanımlamanızı sağlar.  
Daha fazla bilgi edinmek için bkz. [Azure SQL veritabanı Için sanal ağ hizmet uç noktaları ve kuralları](sql-database-vnet-service-endpoint-rule-overview.md) . bu videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Data-Exposed--Demo--Vnet-Firewall-Rules-for-SQL-Database/player?WT.mc_id=dataexposed-c9-niner]

 ### <a name="azure-networking-terminology"></a>Azure ağ terminolojisi  
Sanal ağ güvenlik duvarı kurallarını araştırırken aşağıdaki Azure ağ koşullarına dikkat edin

**Sanal ağ:** Azure aboneliğinizle ilişkili sanal ağlarınız olabilir 

**Alt ağ:** Bir sanal ağ, **alt ağlar**içerir. Sahip olduğunuz tüm Azure sanal makineleri (VM 'Ler) alt ağlara atanır. Bir alt ağ birden çok VM veya başka işlem düğümü içerebilir. Ağınızı erişime izin verecek şekilde yapılandırmadığınız müddetçe, sanal ağınızın dışındaki işlem düğümleri sanal ağınıza erişemez.

**Sanal ağ hizmeti uç noktası:** [Sanal ağ hizmeti uç noktası](../virtual-network/virtual-network-service-endpoints-overview.md) , özellik değerleri bir veya daha fazla resmi Azure hizmet türü adı içeren bir alt ağıdır. Bu makalede, SQL veritabanı adlı Azure hizmetine başvuran **Microsoft. SQL**tür adı ile ilgileniyoruz.

**Sanal ağ kuralı:** SQL veritabanı sunucunuz için bir sanal ağ kuralı, SQL veritabanı sunucunuzun erişim denetim listesinde (ACL) listelenen bir alt ağıdır. SQL veritabanınızın ACL 'sinde olması için alt ağın **Microsoft. SQL** tür adını içermesi gerekir. Bir sanal ağ kuralı, SQL veritabanı sunucunuza alt ağdaki her düğümden gelen iletişimleri kabul etmesini söyler.


## <a name="ip-vs-virtual-network-firewall-rules"></a>IP-sanal ağ güvenlik duvarı kuralları

Azure SQL Server güvenlik duvarı, iletişimlerin SQL veritabanı 'na kabul edileceği IP adresi aralıklarını belirtmenize olanak tanır. Bu yaklaşım, Azure özel ağının dışında olan kararlı IP adresleri için çok uygundur. Ancak, Azure özel ağı içindeki sanal makineler (VM) *dinamik* IP adresleriyle yapılandırılır. Dinamik IP adresleri, VM 'niz yeniden başlatıldığında değişebilir ve IP tabanlı güvenlik duvarı kuralını geçersiz kılar. Bir güvenlik duvarı kuralında, bir üretim ortamında dinamik bir IP adresi belirtmek de bu şekilde yapılır.

SANAL ağınız için bir *statik* IP adresi elde ederek bu sınırlamaya geçici bir çözüm bulabilirsiniz. Ayrıntılar için bkz. [Azure Portal kullanarak bir sanal makine için özel IP adreslerini yapılandırma](../virtual-network/virtual-networks-static-private-ip-arm-pportal.md). Ancak, statik IP yaklaşımının yönetilmesi zor olabilir ve ölçekteki tamamlandığında maliyetli hale gelir. 

Sanal ağ kuralları, VM 'lerinizi içeren belirli bir alt ağdan erişimi kurmak ve yönetmek için daha kolay bir alternatiftir.

> [!NOTE]
> Henüz bir alt ağda SQL veritabanınız olamaz. Azure SQL veritabanı sunucunuz, sanal ağınızdaki bir alt ağda bulunan bir düğümse, sanal ağ içindeki tüm düğümler SQL veritabanınıza iletişim kurabilir. Bu durumda, VM 'niz herhangi bir sanal ağ kuralına veya IP kuralına gerek duymadan SQL veritabanı ile iletişim kurabilir.

## <a name="next-steps"></a>Sonraki adımlar

- Sunucu düzeyinde bir IP güvenlik duvarı kuralı oluşturmaya yönelik hızlı başlangıç için bkz. [Azure SQL veritabanı oluşturma](sql-database-single-database-get-started.md).

- Sunucu düzeyinde VNET güvenlik duvarı kuralı oluşturmaya yönelik hızlı başlangıç için bkz. [Azure SQL veritabanı Için sanal ağ hizmet uç noktaları ve kuralları](sql-database-vnet-service-endpoint-rule-overview.md).

- Açık kaynaklı veya üçüncü taraf uygulamalardan bir Azure SQL veritabanına bağlanma konusunda yardım için bkz. [SQL veritabanı Için istemci hızlı başlangıç kodu örnekleri](https://msdn.microsoft.com/library/azure/ee336282.aspx).

- Açmanız gerekebilecek ek bağlantı noktaları hakkında daha fazla bilgi için bkz **. SQL veritabanı:** [ADO.NET 4,5 ve SQL veritabanı için 1433 ' den sonraki bağlantı noktalarının](sql-database-develop-direct-route-ports-adonet-v12.md) iç içe geçmiş bölümü

- Azure SQL veritabanı bağlantısına genel bakış için bkz. [Azure SQL bağlantı mimarisi](sql-database-connectivity-architecture.md)

- Azure SQL veritabanı güvenliğine genel bakış için bkz. [veritabanınızın güvenliğini sağlama](sql-database-security-overview.md)

<!--Image references-->
[1]: ./media/sql-database-get-started-portal/new-server2.png
[2]: ./media/sql-database-get-started-portal/manage-server-firewall.png
