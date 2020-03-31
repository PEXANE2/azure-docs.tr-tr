---
title: Özel Bağlantı
description: Özel bitiş noktası özelliğine genel bakış
author: rohitnayakmsft
ms.author: rohitna
titleSuffix: Azure SQL Database and SQL Data Warehouse
ms.service: sql-database
ms.topic: overview
ms.reviewer: vanto
ms.date: 03/09/2020
ms.openlocfilehash: ab9c5c5c1134d2e09a790a788a3b7e55f807dd9b
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78945363"
---
# <a name="private-link-for-azure-sql-database-and-data-warehouse"></a>Azure SQL Veritabanı ve Veri Ambarı için Özel Bağlantı

Private Link, Azure'daki çeşitli PaaS hizmetlerine özel bir **bitiş noktası**üzerinden bağlanmanızı sağlar. Özel Bağlantı işlevini destekleyen PaaS hizmetlerine yönelik bir liste için [Özel Bağlantı Belgeleri](../private-link/index.yml) sayfasına gidin. Özel bitiş noktası, belirli bir [VNet](../virtual-network/virtual-networks-overview.md) ve Subnet içindeki özel bir IP adresidir. 

> [!IMPORTANT]
> Bu makale, Azure SQL sunucusu ve Azure SQL sunucusunda oluşturulan SQL Veritabanı ve SQL Veri Ambarı veritabanları için geçerlidir. Kolaylık açısından, hem SQL Veritabanı hem de SQL Veri Ambarı için SQL Veritabanı terimi kullanılmaktadır. Bu makale, Azure SQL Veritabanı'nda **yönetilen** örnek dağıtımı için geçerli *değildir.*

## <a name="data-exfiltration-prevention"></a>Veri sızma önleme

Azure SQL Veritabanı'ndaki veri sızma, veritabanı yöneticisi gibi yetkili bir kullanıcının bir sistemden veri ayıklayıp başka bir konum veya sistemi kuruluş dışına taşıyabilmesidir. Örneğin, kullanıcı verileri üçüncü bir tarafa ait bir depolama hesabına taşır.

Bir SQL Veritabanına bağlanan bir Azure VM içinde SQL Server Management Studio (SSMS) çalıştıran bir kullanıcı ile bir senaryo düşünün. Bu SQL Veritabanı Batı ABD veri merkezindedir. Aşağıdaki örnekte, ağ erişim denetimlerini kullanarak SQL Veritabanı'ndaki ortak uç noktalarla erişimin nasıl sınırlandırılabildiğini gösterilmektedir.

1. Azure Hizmetlerini **KAPAT'a**izin ver'i ayarlayarak tüm Azure hizmet trafiğini ortak bitiş noktası üzerinden SQL Veritabanı'na devre dışı bırak. Sunucu ve veritabanı düzeyinde güvenlik duvarı kurallarında IP adresine izin verilmediğından emin olun. Daha fazla bilgi için Azure [SQL Veritabanı ve Veri Ambarı ağ erişim denetimleri'ne](sql-database-networkaccess-overview.md)bakın.
1. VM'nin Özel IP adresini kullanarak SQL Veritabanı'na trafiğe izin verin. Daha fazla bilgi için [Service Endpoint](sql-database-vnet-service-endpoint-rule-overview.md) ve [VNet güvenlik duvarı kurallarıyla](sql-database-firewall-configure.md)ilgili makalelere bakın.
1. Azure VM'de, [Ağ Güvenlik Grupları (NSG'ler)](../virtual-network/manage-network-security-group.md) ve Hizmet Etiketleri'ni kullanarak giden bağlantının kapsamını aşağıdaki gibi daraltın
    - Service Tag = SQL trafiğine izin vermek için bir NSG kuralı belirtin. WestUs - sadece Batı ABD'de SQL Veritabanı'na bağlantı sağlar
    - Service Tag = SQL trafiğini reddetmek için bir NSG kuralı **(daha yüksek önceliğe**sahip) belirtin - tüm bölgelerde SQL Veritabanına bağlantıları reddetme

Bu kurulumun sonunda, Azure VM yalnızca Batı ABD bölgesindeki SQL Veritabanlarına bağlanabilir. Ancak, bağlantı tek bir SQL Veritabanı ile sınırlı değildir. VM, aboneliğin parçası olmayan veritabanları da dahil olmak üzere Batı ABD bölgesindeki tüm SQL Veritabanlarına bağlanmaya devam edebilir. Yukarıdaki senaryodaki veri sızma kapsamını belirli bir bölgeye indirgemiş olsak da, bunu tamamen ortadan kaldırmadık.

Private Link ile müşteriler artık özel bitiş noktasına erişimi kısıtlamak için NSG'ler gibi ağ erişim denetimleri ayarlayabilir. Tek tek Azure PaaS kaynakları daha sonra belirli özel uç noktalara eşlenir. Kötü niyetli bir içeriden yalnızca eşlenen PaaS kaynağına (örneğin BIR SQL Veritabanı) erişebilir ve başka bir kaynağa erişemez. 

## <a name="on-premises-connectivity-over-private-peering"></a>Özel bakış üzerinde şirket içi bağlantı

Müşteriler şirket içi makinelerden ortak bitiş noktasına bağlandıklarında, IP adreslerinin [Sunucu düzeyinde güvenlik duvarı kuralı](sql-database-server-level-firewall-rule.md)kullanılarak IP tabanlı güvenlik duvarına eklenmesi gerekir. Bu model, dev veya test iş yükleri için tek tek makinelere erişim sağlamak için iyi çalışıyor olsa da, bir üretim ortamında yönetmek zordur.

Private Link ile müşteriler [ExpressRoute,](../expressroute/expressroute-introduction.md)özel eşleme veya VPN tünelleme kullanarak özel bitiş noktasına binalar arası erişimi etkinleştirebilir. Müşteriler daha sonra tüm erişimi ortak bitiş noktası üzerinden devre dışı bırakıp IP tabanlı güvenlik duvarını ip adreslerine izin vermek için kullanmayabilir.

## <a name="how-to-set-up-private-link-for-azure-sql-database"></a>Azure SQL Veritabanı için Özel Bağlantı nasıl ayarlanır? 

### <a name="creation-process"></a>Oluşturma Süreci
Özel Uç Noktalar portalı, PowerShell veya Azure CLI kullanılarak oluşturulabilir:
- [Portal](../private-link/create-private-endpoint-portal.md)
- [Powershell](../private-link/create-private-endpoint-powershell.md)
- [CLI](../private-link/create-private-endpoint-cli.md)

### <a name="approval-process"></a>Onay Süreci
Ağ yöneticisi Özel Bitiş Noktası 'nı (PE) oluşturduktan sonra, SQL yöneticisi Özel Bitiş Noktası Bağlantısını (PEC) SQL Veritabanı'na yönetebilir.

1. Aşağıdaki ekran görüntüsünde gösterilen adımlara göre Azure portalındaki SQL sunucu kaynağına gidin

    - (1) Sol bölmedeki Özel bitiş noktası bağlantılarını seçin
    - (2) Tüm Özel Bitiş Noktası Bağlantılarının (PECs) listesini gösterir
    - (3) Karşılık Gelen Özel Bitiş ![Noktası (PE) tüm PECs ekran görüntüsü oluşturdu][3]

1. Listeden tek bir PEC'i seçerek seçin.
![Ekran görüntüsü seçilen PEC][6]

1. SQL yöneticisi bir PEC'i onaylamayı veya reddetmeyi seçebilir ve isteğe bağlı olarak kısa bir metin yanıtı ekleyebilir.
![PEC onay ekran görüntüsü][4]

1. Onay veya ret sonrasında, liste yanıt metniyle birlikte uygun durumu yansıtır.
![Onaylandıktan sonra tüm PECs ekran görüntüsü][5]

## <a name="use-cases-of-private-link-for-azure-sql-database"></a>Azure SQL Veritabanı için Özel Bağlantı servis taleplerini kullanma 

İstemciler, aynı VNet'ten, aynı bölgede bakan VNet'ten veya bölgeler arasında VNet-vNet bağlantısı üzerinden Özel bitiş noktasına bağlanabilir. Ayrıca, istemciler ExpressRoute, özel bakış veya VPN tünelleme kullanarak şirket içinde bağlanabilir. Aşağıda, ortak kullanım durumlarını gösteren basitleştirilmiş bir diyagram verilmiştir.

 ![Bağlantı seçenekleridiyagramı][1]

## <a name="test-connectivity-to-sql-database-from-an-azure-vm-in-same-virtual-network-vnet"></a>Aynı Sanal Ağdaki (VNet) bir Azure VM'den SQL Veritabanına bağlantı testi yapın

Bu senaryo için, Windows Server 2016'yı çalıştıran bir Azure Sanal Makine (VM) oluşturduğunuzu varsayalım. 

1. [Uzak Masaüstü (RDP) oturumunu başlatın ve sanal makineye bağlanın.](../virtual-machines/windows/connect-logon.md#connect-to-the-virtual-machine) 
1. Daha sonra, VM'nin aşağıdaki araçları kullanarak özel bitiş noktası üzerinden SQL Veritabanı'na bağlandığından emin olmak için bazı temel bağlantı denetimleri yapabilirsiniz:
    1. Telnet
    1. Psping
    1. Nmap
    1. SQL Server Management Studio (SSMS)

### <a name="check-connectivity-using-telnet"></a>Telnet kullanarak Bağlantı yı kontrol edin

[Telnet İstemci,](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754293%28v%3dws.10%29) bağlantı özelliğini sınamak için kullanılabilecek bir Windows özelliğidir. Windows Os sürümüne bağlı olarak, bu özelliği açıkça etkinleştirmeniz gerekebilir. 

Telnet'i yükledikten sonra Komut İstemi pencerelerini açın. Telnet komutunu çalıştırın ve SQL Veritabanı'nın IP adresini ve özel bitiş noktasını belirtin.

```
>telnet 10.1.1.5 1433
```

Telnet başarılı bir şekilde bağlandığında, komut penceresinde aşağıdaki resimdeki gibi boş bir ekran görürsünüz:

 ![Telnet diyagramı][2]

### <a name="check-connectivity-using-psping"></a>Psping kullanarak Bağlantı yı denetleme

[Özel](/sysinternals/downloads/psping) uç nokta bağlantısının (PEC) bağlantı noktası 1433'teki bağlantıları dinlediğini kontrol etmek için aşağıdaki gibi kullanılabilir.

SQL Veritabanı sunucunuz ve bağlantı noktası 1433 için FQDN'yi sağlayarak aşağıdaki gibi çalıştırın:

```
>psping.exe mysqldbsrvr.database.windows.net:1433

PsPing v2.10 - PsPing - ping, latency, bandwidth measurement utility
Copyright (C) 2012-2016 Mark Russinovich
Sysinternals - www.sysinternals.com

TCP connect to 10.6.1.4:1433:
5 iterations (warmup 1) ping test:
Connecting to 10.6.1.4:1433 (warmup): from 10.6.0.4:49953: 2.83ms
Connecting to 10.6.1.4:1433: from 10.6.0.4:49954: 1.26ms
Connecting to 10.6.1.4:1433: from 10.6.0.4:49955: 1.98ms
Connecting to 10.6.1.4:1433: from 10.6.0.4:49956: 1.43ms
Connecting to 10.6.1.4:1433: from 10.6.0.4:49958: 2.28ms
```

Çıktı, Psping'in PEC ile ilişkili özel IP adresini pingleyebileceğini gösteriyor.

### <a name="check-connectivity-using-nmap"></a>Nmap'i kullanarak bağlantıyı denetleme

Nmap (Network Mapper), ağ bulma ve güvenlik denetimi için kullanılan ücretsiz ve açık kaynak kodlu bir araçtır. Daha fazla bilgi ve indirme https://nmap.orgbağlantısı için. Özel bitiş noktasının bağlantı noktası 1433'teki bağlantıları dinlediğinden emin olmak için bu aracı kullanabilirsiniz.

Özel bitiş noktasını barındıran alt netin adres aralığını sağlayarak Nmap'i aşağıdaki gibi çalıştırın.

```
>nmap -n -sP 10.1.1.0/24
...
...
Nmap scan report for 10.1.1.5
Host is up (0.00s latency).
Nmap done: 256 IP addresses (1 host up) scanned in 207.00 seconds
```

Sonuç, bir IP adresinin dolduğunu gösterir; hangi özel bitiş noktası için IP adresine karşılık gelir.


### <a name="check-connectivity-using-sql-server-management-studio-ssms"></a>SQL Server Management Studio (SSMS) kullanarak Bağlantı yı kontrol edin
> [!NOTE]
> Müşterileriniz için bağlantı dizelerinde sunucunun **Tam Nitelikli Alan Adı (FQDN)** kullanın. Doğrudan IP adresine yapılan tüm giriş girişimleri başarısız olacaktır. Özel uç nokta trafiği bölgedeki SQL Ağ Geçidi'ne yönlendirdiğinden ve girişlerin başarılı olması için FQDN'nin belirtilmesi gerektiğinden, bu davranış tasarım gereğidir.

[SQL Veritabanına bağlanmak için SSMS'i](sql-database-connect-query-ssms.md)kullanmak için aşağıdaki adımları izleyin. SSMS kullanarak SQL Veritabanı'na bağlandıktan sonra, aşağıdaki sorguyu çalıştırarak Azure VM'nin özel IP adresinden bağlandığınızı doğrulayın:

````
select client_net_address from sys.dm_exec_connections 
where session_id=@@SPID
````

## <a name="limitations"></a>Sınırlamalar 
Özel bitiş noktasına bağlantılar yalnızca [bağlantı ilkesi](sql-database-connectivity-architecture.md#connection-policy) olarak **Proxy'yi** destekler


## <a name="connecting-from-an-azure-vm-in-peered-virtual-network-vnet"></a>Eşli Sanal Ağda (VNet) Azure VM'den bağlanma 

Eşli bir VNet'teki Bir Azure VM'den SQL Veritabanına bağlantı kurmak için [VNet eşlemesini](../virtual-network/tutorial-connect-virtual-networks-powershell.md) yapılandırın.

## <a name="connecting-from-an-azure-vm-in-vnet-to-vnet-environment"></a>VNet-to-VNet ortamında bir Azure VM'den bağlanma

Farklı bir bölgedeki veya abonelikteki bir Azure VM'den SQL Veritabanına bağlantı kurmak için [VNet'ten VNet VPN ağ geçidi bağlantısını](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) yapılandırın.

## <a name="connecting-from-an-on-premises-environment-over-vpn"></a>VPN üzerinden şirket içi ortamdan bağlanma

Şirket içi ortamdan SQL Veritabanı'na bağlantı oluşturmak için seçeneklerden birini seçin ve uygulayın:
- [Noktadan Siteye bağlantı](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)
- [Konumlar arası VPN bağlantısı](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
- [ExpressRoute devresi](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md)


## <a name="connecting-from-an-azure-sql-data-warehouse-to-azure-storage-using-polybase"></a>Polybase kullanarak Azure SQL Veri Ambarından Azure Depolamasına bağlanma

PolyBase genellikle Azure Depolama hesaplarından Azure SQL Veri Ambarı'na veri yüklemek için kullanılır. Limitlerden veri yüklediğiniz Azure Depolama hesabı yalnızca Özel Uç Noktaları, Hizmet Bitiş Noktaları veya IP tabanlı güvenlik duvarları aracılığıyla bir dizi VNet alt ağına erişirse, PolyBase'den hesaba bağlantı bozulur. Bir VNet'e güvenli olan Azure Depolama'ya bağlanan Azure SQL Veri Ambarı ile hem PolyBase alma hem de dışa aktarma senaryolarını etkinleştirmek [için, burada](sql-database-vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage)sağlanan adımları izleyin. 



## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL Veritabanı güvenliğine genel bakış için [bkz.](sql-database-security-overview.md)
- Azure SQL Veritabanı bağlantısına genel bakış için [bkz.](sql-database-connectivity-architecture.md)

<!--Image references-->
[1]: ./media/sql-database-get-started-portal/pe-connect-overview.png
[2]: ./media/sql-database-get-started-portal/telnet-result.png
[3]: ./media/sql-database-get-started-portal/pec-list-before.png
[4]: ./media/sql-database-get-started-portal/pec-approve.png
[5]: ./media/sql-database-get-started-portal/pec-list-after.png
[6]: ./media/sql-database-get-started-portal/pec-select.png
