---
title: Ağ Erişim Denetimleri
description: Erişimi yönetmek ve tek veya havuza alınan bir veritabanını yapılandırmak için Azure SQL Veritabanı ve Veri Ambarı için ağ erişim denetimlerine genel bakış.
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
ms.date: 03/09/2020
ms.openlocfilehash: 822fab5c00501d415c3c184587141e869523e417
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78945388"
---
# <a name="azure-sql-database-and-data-warehouse-network-access-controls"></a>Azure SQL Veritabanı ve Veri Ambarı ağ erişim denetimleri

> [!NOTE]
> Bu makale, Azure SQL sunucusu ve Azure SQL sunucusunda oluşturulan SQL Veritabanı ve SQL Veri Ambarı veritabanları için geçerlidir. Kolaylık açısından, hem SQL Veritabanı hem de SQL Veri Ambarı için SQL Veritabanı terimi kullanılmaktadır.

> [!IMPORTANT]
> Bu makale, **Azure SQL Veritabanı Yönetilen Örnek**için geçerli *değildir.* ağ yapılandırması hakkında daha fazla bilgi için yönetilen [bir örneğe bağlanma](sql-database-managed-instance-connect-app.md) yı görün.

[Azure portalından](sql-database-single-database-get-started.md)yeni bir Azure SQL Server oluşturduğunuzda, sonuç *biçimi, yourservername.database.windows.net*genel bir bitiş noktasıdır.

Ortak bitiş noktası üzerinden SQl Veritabanına erişime seçici olarak izin vermek için aşağıdaki ağ erişim denetimlerini kullanabilirsiniz:
- Azure Hizmetlerine İzin Ver: A)'ya ayarlandığında, Azure sınırındaki diğer kaynaklar (örneğin bir Azure Sanal Makinesi) SQL Veritabanı'na erişebilir

- IP güvenlik duvarı kuralları: Belirli bir IP adresinden (örneğin şirket içi makinelerden) bağlantılara açıkça izin vermek için bu özelliği kullanın

Sanal [Ağlar'dan](../virtual-network/virtual-networks-overview.md) SQL Veritabanı'na özel erişime de izin verebilirsiniz:
- Sanal Ağ güvenlik duvarı kuralları: Azure sınırı içinde belirli bir Sanal Ağ'dan gelen trafiğe izin vermek için bu özelliği kullanın

- Özel Bağlantı: Belirli bir Sanal Ağ içinde Azure SQL Server için özel bir bitiş noktası oluşturmak için bu özelliği kullanın



Bu erişim denetimlerinin üst düzey bir açıklaması ve ne yaptıkları için aşağıdaki videoyu izleyin:
> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Data-Exposed--SQL-Database-Connectivity-Explained/player?WT.mc_id=dataexposed-c9-niner]


## <a name="allow-azure-services"></a>Azure hizmetlerine izin ver 
[Azure portalından](sql-database-single-database-get-started.md)yeni bir Azure SQL Server oluşturulması sırasında bu ayar işaretsiz bırakılır.



Azure SQL Server oluşturulduktan sonra bu ayarı güvenlik duvarı bölmesi aracılığıyla da değiştirebilirsiniz.
  
 ![Sunucu güvenlik duvarını yönet ekran görüntüsü][2]

**ON'da** Azure SQL Server olarak ayarlandığında, Azure sınırı içindeki tüm kaynaklardan gelen ve aboneliğinizin bir parçası olabilecek veya olmayabilir.

Çoğu durumda, **ON** ayarı çoğu müşterinin istediğinden daha müsamahakârdır. Bu ayarı **OFF** olarak ayarlamak ve daha kısıtlayıcı IP güvenlik duvarı kuralları veya Sanal Ağ güvenlik duvarı kurallarıyla değiştirmek isteyebilirler. Bunu yapmak, Azure'da VM'lerde çalışan ve VNet'inizin bir parçası olmayan ve dolayısıyla bir Azure IP adresi üzerinden Sql Veritabanı'na bağlanan aşağıdaki özellikleri etkiler.

### <a name="import-export-service"></a>İthalat İhracat Hizmeti
Dışa Aktarma Hizmeti çalışmıyor Azure hizmetlerine IZIN ver, KAPALI olarak ayarlanmış **sunucuya erişin.** Ancak, bir Azure [VM'den sqlpackage.exe'yi el ile çalıştırarak veya](https://docs.microsoft.com/azure/sql-database/import-export-from-vm) DACFx API'sini kullanarak dışa aktarmayı doğrudan kodunuzda gerçekleştirerek sorunu çözebilirsiniz.

### <a name="data-sync"></a>Data Sync
**Azure hizmetlerine izin vererek sunucu kümesine erişim için** İzin verme özelliğini kullanmak için, **Hub** veritabanını barındıran bölge için Sql **hizmet etiketinden** [IP adresleri eklemek](sql-database-server-level-firewall-rule.md) için tek tek güvenlik duvarı kuralı girişleri oluşturmanız gerekir.
Hub **ve** **Üye** veritabanlarını barındıran mantıksal sunuculara bu sunucu düzeyinde güvenlik duvarı kuralları nı ekleyin (farklı bölgelerde olabilir)

Batı ABD bölgesi için Sql hizmet etiketine karşılık gelen IP adreslerini oluşturmak için aşağıdaki PowerShell komut dosyasını kullanın
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
> Get-AzNetworkServiceTag, Konum parametresini belirtmeye rağmen Sql Service Tag için genel aralığı döndürür. Eşitleme grubunuz tarafından kullanılan Hub veritabanını barındıran bölgeye filtrelediğinden emin olun

PowerShell komut dosyasının çıktısının Classless Inter-Domain Yönlendirme (CIDR) gösteriminde olduğunu ve bunun [get-IPrangeStartEnd.ps1](https://gallery.technet.microsoft.com/scriptcenter/Start-and-End-IP-addresses-bcccc3a9) kullanarak Başlangıç ve Bitiş IP adresi biçimine dönüştürülmesi gerektiğini unutmayın
```powershell
PS C:\> Get-IPrangeStartEnd -ip 52.229.17.93 -cidr 26                                                                   
start        end
-----        ---
52.229.17.64 52.229.17.127
```

CIDR'den Başlangıç ve Son IP adresi biçimine tüm IP adreslerini dönüştürmek için aşağıdaki ek adımları yapın.

```powershell
PS C:\>foreach( $i in $sql.Properties.AddressPrefixes) {$ip,$cidr= $i.split('/') ; Get-IPrangeStartEnd -ip $ip -cidr $cidr;}                                                                                                                
start          end
-----          ---
13.86.216.0    13.86.216.127
13.86.216.128  13.86.216.191
13.86.216.192  13.86.216.223
```
Artık bunları farklı güvenlik duvarı kuralları olarak ekleyebilir ve ardından **Sunucuya erişmek için Azure hizmetlerine İzin Ver'i** AYARLAYABILIRSINIZ.


## <a name="ip-firewall-rules"></a>IP güvenlik duvarı kuralları
Ip tabanlı güvenlik duvarı, istemci makinelerin IP adreslerini açıkça [ekleyene](sql-database-server-level-firewall-rule.md) kadar veritabanı sunucunuza tüm erişimi engelleyen Azure SQL Server özelliğidir.


## <a name="virtual-network-firewall-rules"></a>Sanal Ağ güvenlik duvarı kuralları

Azure SQL Server güvenlik duvarı, IP kurallarına ek olarak *sanal ağ kurallarını*da tanımlamanıza olanak tanır.  
Daha fazla bilgi edinmek [için Azure SQL Veritabanı için Sanal Ağ hizmet bitiş noktalarına ve kurallarına](sql-database-vnet-service-endpoint-rule-overview.md) bakın veya bu videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Data-Exposed--Demo--Vnet-Firewall-Rules-for-SQL-Database/player?WT.mc_id=dataexposed-c9-niner]

 ### <a name="azure-networking-terminology"></a>Azure Ağ terminolojisi  
Sanal Ağ güvenlik duvarı kurallarını keşfederken aşağıdaki Azure Ağ terimlerini unutmayın

**Sanal ağ:** Azure aboneliğinizle ilişkili sanal ağlarınız olabilir 

**Alt ağ:** Sanal ağ **alt ağlar**içerir. Sahip olduğunuz tüm Azure sanal makineleri (VM'ler) alt ağlara atanır. Bir alt ağ birden çok VM veya diğer işlem düğümleri içerebilir. Sanal ağınızın dışındaki bilgi işlem düğümleri, güvenliğinizi erişime izin verecek şekilde yapılandırmadığınız sürece sanal ağınıza erişemez.

**Sanal Ağ hizmeti bitiş noktası:** [Sanal Ağ hizmeti bitiş noktası,](../virtual-network/virtual-network-service-endpoints-overview.md) özellik değerleri bir veya daha fazla resmi Azure hizmet türü adlarını içeren bir alt ağdır. Bu makalede, SQL Veritabanı adlı Azure hizmetini ifade eden **Microsoft.Sql'in**tür adı ile ilgileniyoruz.

**Sanal ağ kuralı:** SQL Database sunucunuz için sanal ağ kuralı, SQL Veritabanı sunucunuzun erişim denetim listesinde (ACL) listelenen bir alt ağdır. SQL Veritabanınızın ACL'sinde olmak için alt ağ **Microsoft.Sql** türü adını içermelidir. Sanal ağ kuralı, SQL Veritabanı sunucunuza alt ağdaki her düğümden iletişimkabul etmesini söyler.


## <a name="ip-vs-virtual-network-firewall-rules"></a>IP vs. Virtual Network güvenlik duvarı kuralları

Azure SQL Server güvenlik duvarı, iletişimin SQL Veritabanı'na kabul edildiği IP adres aralıklarını belirtmenize olanak tanır. Bu yaklaşım, Azure özel ağının dışındaki kararlı IP adresleri için idealdir. Ancak, Azure özel ağındaki sanal makineler (VM'ler) *dinamik* IP adresleriyle yapılandırılır. VM'niz yeniden başlatıldığında dinamik IP adresleri değişebilir ve sırayla IP tabanlı güvenlik duvarı kuralını geçersiz kılabilir. Bir üretim ortamında, bir güvenlik duvarı kuralı dinamik bir IP adresi belirtmek aptallık olacaktır.

VM'niz için *statik* bir IP adresi alarak bu sınırlamayı çözebilirsiniz. Ayrıntılar için, [Azure portalını kullanarak sanal bir makineiçin özel IP adreslerini yapılandırın' a](../virtual-network/virtual-networks-static-private-ip-arm-pportal.md)bakın. Ancak, statik IP yaklaşımının yönetilmesi zor olabilir ve ölçekte yapıldığında maliyetlidir. 

Sanal ağ kuralları, Sanal M'lerinizi içeren belirli bir alt ağdan erişimi oluşturmak ve yönetmek için daha kolay bir alternatiftir.

> [!NOTE]
> Henüz bir alt ağ üzerinde SQL Veritabanı olamaz. Azure SQL Veritabanı sunucunuz sanal ağınızdaki bir alt ağda düğümse, sanal ağdaki tüm düğümler SQL Veritabanınızla iletişim kurabilir. Bu durumda, Sanal M'leriniz sanal ağ kurallarına veya IP kurallarına gerek kalmadan SQL Veritabanı ile iletişim kurabilir.

## <a name="private-link"></a>Özel Bağlantı 
Private Link, Azure SQL Server'a özel bir **bitiş noktası**üzerinden bağlanmanızı sağlar. Özel bitiş noktası, belirli bir [Sanal Ağ](../virtual-network/virtual-networks-overview.md) ve Subnet içindeki özel bir IP adresidir.

## <a name="next-steps"></a>Sonraki adımlar

- Sunucu düzeyinde BIR IP güvenlik duvarı kuralı oluşturmaya hızlı bir başlangıç için [bkz.](sql-database-single-database-get-started.md)

- Sunucu düzeyinde bir Vnet güvenlik duvarı kuralı oluşturmaya hızlı bir başlangıç için, [Azure SQL Veritabanı için Sanal Ağ hizmet bitiş noktalarına ve kurallarına](sql-database-vnet-service-endpoint-rule-overview.md)bakın.

- Açık kaynak veya üçüncü taraf uygulamalardan bir Azure SQL veritabanına bağlanma konusunda yardım için, [SQL Veritabanı'na Istemci hızlı başlatma kodu örneklerine](https://msdn.microsoft.com/library/azure/ee336282.aspx)bakın.

- Açmanız gereken ek bağlantı noktaları hakkında daha fazla bilgi için **SQL Veritabanı:** [4,5 ADO.NET ve SQL Veritabanı için 1433'ün dışındaki Bağlantı Noktalarının](sql-database-develop-direct-route-ports-adonet-v12.md) dış vs iç bölümüne bakın

- Azure SQL Veritabanı Bağlantısına genel bakış için [bkz.](sql-database-connectivity-architecture.md)

- Azure SQL Veritabanı güvenliğine genel bakış için [bkz.](sql-database-security-overview.md)

<!--Image references-->
[1]: ./media/sql-database-get-started-portal/new-server2.png
[2]: ./media/sql-database-get-started-portal/manage-server-firewall.png

