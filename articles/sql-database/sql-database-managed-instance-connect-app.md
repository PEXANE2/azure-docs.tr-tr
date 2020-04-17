---
title: Yönetilen örnek bağlantı uygulaması
description: Bu makalede, uygulamanızın Azure SQL Veritabanı Yönetilen Örneği'ne nasıl bağlanılabildiğini açıklanmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab, vanto
ms.date: 11/09/2018
ms.openlocfilehash: 8d920fb7815e5a9fe30d8f3b4e40f36133d83222
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81538095"
---
# <a name="connect-your-application-to-azure-sql-database-managed-instance"></a>Uygulamanızı Azure SQL Veritabanı yönetilen örneğine bağlayın

Bugün, uygulamanızı nasıl ve nerede barındıracağınıza karar verirken birden çok seçeneğiniz vardır.

Azure Uygulama Hizmeti'ni veya Azure Uygulama Hizmeti Ortamı, Sanal Makine, Sanal Makine Ölçeği Seti gibi Azure'un sanal ağı (VNet) tümleşik seçeneklerinden bazılarını kullanarak uygulamayı bulutta barındırmayı seçebilirsiniz. Ayrıca karma bulut yaklaşımını benimsebilir ve uygulamalarınızı şirket içinde tutabilirsiniz.

Hangi seçimi yaptıysanız, yönetilen bir örneğe bağlayabilirsiniz.  

![yüksek kullanılabilirlik](./media/sql-database-managed-instance/application-deployment-topologies.png)

## <a name="connect-an-application-inside-the-same-vnet"></a>Aynı VNet içinde bir uygulama bağlama

Bu senaryo en basitidir. VNet içindeki sanal makineler, farklı alt ağlarda olsalar bile birbirlerine doğrudan bağlanabilirler. Bu, uygulamayı bir Azure Uygulama Ortamı veya Sanal Makine'ye bağlamak için gereken tek şeyin bağlantı dizesini uygun şekilde ayarlamak olduğu anlamına gelir.  

## <a name="connect-an-application-inside-a-different-vnet"></a>Bir uygulamayı farklı bir VNet'in içine bağlama

Yönetilen Örnek kendi VNet özel IP adresi olduğundan bu senaryo biraz daha karmaşıktır. Bağlanmak için, bir uygulamanın Yönetilen Örnek'in dağıtıldığı VNet'e erişmesi gerekir. Bu nedenle, öncelikle uygulama ile Yönetilen Örnek VNet arasında bir bağlantı kurmanız gerekir. Bu senaryonun işe yaraması için VNet'lerin aynı abonelikte olması gerekmez.

VNet'leri bağlamak için iki seçenek vardır:

- [Azure Sanal Ağ eşleme](../virtual-network/virtual-network-peering-overview.md)
- VNet-to-VNet VPN ağ geçidi ([Azure portalı](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md), [PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md), [Azure CLI](../vpn-gateway/vpn-gateway-howto-vnet-vnet-cli.md))

Bakan microsoft omurga ağını kullandığından, bağlantı açısından bakıldığında, eşlenen VNet'teki sanal makineler ile aynı VNet'teki sanal makineler arasında fark edilebilir bir fark bulunmadığından, eşleme seçeneği tercih edilir bir seçenektir. VNet'in aynı bölgedeki ağlarla eşlenetliği sınırlıdır.  

> [!IMPORTANT]
> Yönetilen Örnek için VNet eşleme [senaryosu, Global Virtual Network'ün eşleme kısıtlamaları](../virtual-network/virtual-network-manage-peering.md#requirements-and-constraints)nedeniyle aynı bölgedeki ağlarla sınırlıdır. Daha fazla ayrıntı için [Azure Sanal Ağlar Sık Sorulan Sorular](https://docs.microsoft.com/azure/virtual-network/virtual-networks-faq#what-are-the-constraints-related-to-global-vnet-peering-and-load-balancers) makalesinin ilgili bölümüne de bakın. 

## <a name="connect-an-on-premises-application"></a>Şirket içi uygulamaya bağlanma

Yönetilen Örnek'e yalnızca özel bir IP adresi üzerinden erişilebilir. Şirket içinde erişmek için, uygulama ile Yönetilen Örnek VNet arasında Siteden Siteye bağlantı kurmanız gerekir.

Şirket içi Azure VNet'e bağlanmanın iki seçeneği vardır:

- Siteden Siteye VPN bağlantısı ([Azure portalı](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md), [PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md), [Azure CLI](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md))
- [ExpressRoute](../expressroute/expressroute-introduction.md) bağlantısı  

Yerleşik olarak Azure bağlantısını başarıyla kurduysanız ve Yönetilen Örnek'e bağlantı kuramıyorsanız, güvenlik duvarınızın SQL 1433'te ve yeniden yönlendirme için 11000-11999 bağlantı aralığında açık giden bağlantıolup olmadığını kontrol edin.

## <a name="connect-an-application-on-the-developers-box"></a>Geliştirici kutusuna uygulama bağlama

Yönetilen Örnek'e yalnızca özel bir IP adresi üzerinden erişilebilen bu nedenle geliştirici kutunuzdan erişmek için öncelikle geliştirici kutunuzla Yönetilen Örnek VNet arasında bir bağlantı kurmanız gerekir. Bunu yapmak için, yerel Azure sertifika kimlik doğrulamasını kullanarak bir VNet'e Nokta-Çözüm bağlantısı yapılandırın. Daha fazla bilgi için bkz. Şirket [içi bilgisayardan Azure SQL Veritabanı Yönetilen Örneği'ne bağlanmak için yerinde](sql-database-managed-instance-configure-p2s.md)bağlantı yapılandırın.

## <a name="connect-from-on-premises-with-vnet-peering"></a>VNet peering ile şirket içinde bağlantı kurun

Müşteriler tarafından uygulanan bir diğer senaryo da VPN ağ geçidinin ayrı bir sanal ağa yüklendiği ve Yönetilen Örnek'i barındıran bir abonenin bulunduğu yerdir. İki sanal ağ daha sonra bakılır. Aşağıdaki örnek mimari diyagramı bunun nasıl uygulanabileceğini gösterir.

![VNet eşlemesi](./media/sql-database-managed-instance-connect-app/vnet-peering.png)

Temel altyapıyı ayarladıktan sonra, VPN Ağ Geçidi'nin Yönetilen Örnek'i barındıran sanal ağdaki IP adreslerini görebilmeleri için bazı ayarları değiştirmeniz gerekir. Bunu yapmak için, **Peering ayarları**altında aşağıdaki çok özel değişiklikleri yapın.

1. VPN ağ geçidini barındıran VNet'te, **Peerings'e**gidin, ardından Yönetilen Örnek eşlenen VNet bağlantısına gidin ve ardından **Ağ Geçidi Geçişine İzin Ver'i**tıklatın.
2. Yönetilen Örneği barındıran VNet'te, **Peerings'e**, ardından VPN Ağ Geçidi'ne giden VNet bağlantısına gidin ve ardından **uzak ağ geçitlerini kullan'ı**tıklatın.

## <a name="connect-an-azure-app-service-hosted-application"></a>Azure Uygulama Hizmeti barındırılan uygulamayı bağlayın

Yönetilen Örnek'e yalnızca özel bir IP adresi üzerinden erişilebilir, böylece Azure Uygulama Hizmeti'nden erişebilmek için öncelikle uygulama ile Yönetilen Örnek VNet arasında bağlantı kurmanız gerekir. Bkz. [Uygulamanızı Azure Sanal Ağıyla Tümleştirin.](../app-service/web-sites-integrate-with-vnet.md)  

Sorun giderme için Bkz. [Sorun Giderme VNet'leri ve Uygulamaları.](../app-service/web-sites-integrate-with-vnet.md#troubleshooting) Bağlantı kurulamıyorsa, ağ [yapılandırmasını senkronize etmeye](sql-database-managed-instance-sync-network-configuration.md)çalışın.

Azure Uygulama Hizmetini Yönetilen Örnek'e bağlamanın özel bir durumu, Azure App Service'i Yönetilen Örnek VNet'e bakan bir ağa entegre ettiğinizde yapılır. Bu durumda, aşağıdaki yapılandırmanın ayarlanmasını gerektirir:

- Yönetilen Örnek VNet ağ geçidi olmamalıdır  
- Yönetilen Örnek VNet uzak ağ geçitleri kullan seçeneği ne ayarlanmış olmalıdır
- Eşli VNet ağ geçidi geçiş seçeneğine izin verme

Bu senaryo aşağıdaki diyagramda gösterilmiştir:

![entegre uygulama izleme](./media/sql-database-managed-instance/integrated-app-peering.png)

>[!NOTE]
>VNet Tümleştirme özelliği, expressroute ağ geçidi olan bir VNet ile bir uygulamayı tümleştirmez. ExpressRoute Ağ Geçidi birlikte yaşam modunda yapılandırılsa bile VNet Tümleştirmesi çalışmaz. Kaynaklara ExpressRoute bağlantısı üzerinden erişmeniz gerekiyorsa, VNet'inizde çalışan bir Uygulama Hizmet Ortamı kullanabilirsiniz.

## <a name="troubleshooting-connectivity-issues"></a>Bağlantı sorunlarını giderme

Sorun giderme bağlantısı sorunları için aşağıdakileri gözden geçirin:

- Yönetilen Örnek'e aynı VNet içindeki ancak farklı bir alt ağ içindeki bir Azure sanal makinesinden bağlanamıyorsanız, VM alt ağında erişimi engelleyebilecek bir Ağ Güvenlik Grubu olup olmadığını kontrol edin. Ayrıca, Sql portu 1433'te ve 11000-11999 aralığındaki bağlantı noktalarında giden bağlantıyı açmanız gerektiğini unutmayın, çünkü azure sınırı içinde yeniden yönlendirme yoluyla bağlanmak için bunlar gereklidir.
- VNet ile ilişkili rota tablosu için BGP Yayılma'nın **Etkin** olarak ayarlı olduğundan emin olun.
- P2S VPN kullanıyorsanız, **Giriş/Çıkış** numaralarını görüp görmediğiniz görmek için Azure portalındaki yapılandırmayı kontrol edin. Sıfır olmayan sayılar, Azure'un trafiği şirket içinde/şirket için yönlendirmesi olduğunu gösterir.

   ![giriş/çıkış numaraları](./media/sql-database-managed-instance-connect-app/ingress-egress-numbers.png)

- İstemci makinesinin (VPN istemcisini çalıştıran) erişmeniz gereken tüm VNet'ler için rota girişleri ne olduğunu kontrol edin. Rotalar `%AppData%\ Roaming\Microsoft\Network\Connections\Cm\<GUID>\routes.txt`.

   ![route.txt](./media/sql-database-managed-instance-connect-app/route-txt.png)

   Bu resimde gösterildiği gibi, ilgili her VNet için iki giriş ve Portal'da yapılandırılan VPN bitiş noktası için üçüncü bir giriş vardır.

   Yolları kontrol etmenin başka bir yolu da aşağıdaki komutla mümkündür. Çıktı, çeşitli alt ağlara giden yolları gösterir:

   ```cmd
   C:\ >route print -4
   ===========================================================================
   Interface List
   14...54 ee 75 67 6b 39 ......Intel(R) Ethernet Connection (3) I218-LM
   57...........................rndatavnet
   18...94 65 9c 7d e5 ce ......Intel(R) Dual Band Wireless-AC 7265
   1...........................Software Loopback Interface 1
   Adapter===========================================================================

   IPv4 Route Table
   ===========================================================================
   Active Routes:
   Network Destination        Netmask          Gateway       Interface  Metric
          0.0.0.0          0.0.0.0       10.83.72.1     10.83.74.112     35
         10.0.0.0    255.255.255.0         On-link       172.26.34.2     43
         10.4.0.0    255.255.255.0         On-link       172.26.34.2     43
   ===========================================================================
   Persistent Routes:
   None
   ```

- VNet eşlemesini kullanıyorsanız, [Ağ Geçidi Geçişine İzin Ver ve Uzak Ağ Geçitlerini Kullan'ı](#connect-from-on-premises-with-vnet-peering)ayarlama yönergelerini izlediğinizi emin olun.

- Azure App Service barındırılan bir uygulamayı bağlamak için VNet eşlemesini kullanıyorsanız ve Yönetilen Örnek VNet'te genel bir IP adresi aralığı varsa, barındırılan uygulama ayarlarınızın giden trafiğinizin genel IP ağlarına yönlendirilmesine izin verdiğinden emin olun. [Bölgesel VNet Entegrasyonu](../app-service/web-sites-integrate-with-vnet.md#regional-vnet-integration)talimatları izleyin.

## <a name="required-versions-of-drivers-and-tools"></a>Sürücülerin ve araçların gerekli sürümleri

Yönetilen Örnek'e bağlanmak istiyorsanız, araçların ve sürücülerin aşağıdaki en az sürümleri önerilir:

| Sürücü/araç | Sürüm |
| --- | --- |
|.NET Framework | 4.6.1 (veya .NET Çekirdek) |
|ODBC sürücüsü| v17 v17 |
|PHP sürücüsü| 5.2.0 |
|JDBC sürücüsü| 6.4.0 |
|Node.js sürücüsü| 2.1.1 |
|OLEDB sürücüsü| 18.0.2.0 |
|SSMS| 18.0 veya [üzeri](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) |
|[SMO](https://docs.microsoft.com/sql/relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide) | [150](https://www.nuget.org/packages/Microsoft.SqlServer.SqlManagementObjects) veya daha yüksek |

## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen Örnek hakkında bilgi için yönetilen [örnek nedir'e](sql-database-managed-instance.md)bakın.
- Yeni yönetilen örnek oluşturmayı gösteren [bir](sql-database-managed-instance-get-started.md)öğretici için bkz.
