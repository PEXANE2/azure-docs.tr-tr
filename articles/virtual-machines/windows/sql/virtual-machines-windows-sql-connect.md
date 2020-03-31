---
title: SQL Server Virtual Machine (Resource Manager) bağlanın | Microsoft Dokümanlar
description: Azure'da Sanal Makine'de çalışan SQL Server'a nasıl bağlanıp bağlanıp bağlanılamayı öğrenin. Bu konu klasik dağıtım modelini kullanır. Senaryolar ağ yapılandırması ve istemcinin konumuna bağlı olarak değişir.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 12/12/2017
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: deb337d989a3658e909cefa7a9ab028e37792562
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79243178"
---
# <a name="connect-to-a-sql-server-virtual-machine-on-azure"></a>Azure’da SQL Server Sanal Makinesine Bağlanma

## <a name="overview"></a>Genel Bakış

Bu konu, Azure sanal makinede çalışan SQL Server örneğinize nasıl bağlanılabildiğini açıklar. Bazı [genel bağlantı senaryolarını](#connection-scenarios) kapsar ve ardından [bağlantı ayarlarını değiştirmek için portalda adımlar](#change)sağlar. Bağlantıyla ilgili bağlantıyı portalın dışında gidermeniz veya yapılandırmanız gerekiyorsa, bu konunun sonundaki [el ile yapılandırmaya](#manual) bakın. 

Hem sağlama hem de bağlantı konusunda tam bir gözden geçirme istiyorsanız, [Azure'da SQL Server Virtual Machine Sağlama](virtual-machines-windows-portal-sql-server-provision.md)bölümüne bakın.

## <a name="connection-scenarios"></a>Bağlantı senaryoları

Bir istemcinin Sanal Makine'de çalışan SQL Server'a bağlanma şekli, istemcinin konumuna ve ağ yapılandırmasına bağlı olarak değişir.

Azure portalında bir SQL Server VM sağlarsanız, **SQL bağlantısının**türünü belirtme seçeneğiniz vardır.

![Sağlama sırasında ortak SQL bağlantısı seçeneği](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity.png)

Bağlantı seçenekleriniz şunlardır:

| Seçenek | Açıklama |
|---|---|
| **Kamu** | Internet üzerinden SQL Server'a bağlanın |
| **Özel** | Aynı sanal ağda SQL Server'a bağlanın |
| **Yerel** | Aynı sanal makinede SQL Server'a yerel olarak bağlanma | 

Aşağıdaki bölümlerde **Genel** ve **Özel** seçenekleri daha ayrıntılı olarak açıklayınız.

## <a name="connect-to-sql-server-over-the-internet"></a>Internet üzerinden SQL Server'a bağlanın

Internet'ten SQL Server veritabanı altyapınıza bağlanmak istiyorsanız, sağlama sırasında portaldaki **SQL bağlantı** türü için **Genel'i** seçin. Portal otomatik olarak aşağıdaki adımları yapar:

* SQL Server için TCP/IP protokolünü etkinleştiri.
* SQL Server TCP bağlantı noktasını açmak için bir güvenlik duvarı kuralını yapılandırır (varsayılan 1433).
* Ortak erişim için gerekli olan SQL Server Kimlik Doğrulamasını sağlar.
* VM'deki ağ güvenlik grubunu SQL Server bağlantı noktasındaki tüm TCP trafiğine göre yapılandırır.

> [!IMPORTANT]
> SQL Server Developer ve Express sürümleriiçin sanal makine görüntüleri TCP/IP protokolünü otomatik olarak etkinleştirmez. Geliştirici ve Express sürümleri için, VM'yi oluşturduktan sonra [TCP/IP protokolünü el ile etkinleştirmek](#manualtcp) için SQL Server Configuration Manager'ı kullanmanız gerekir.

Internet erişimi olan herhangi bir istemci, sanal makinenin genel IP adresini veya bu IP adresine atanan herhangi bir DNS etiketini belirterek SQL Server örneğine bağlanabilir. SQL Server bağlantı noktası 1433 ise, bağlantı dizesinde belirtmeniz gerekmez. Aşağıdaki bağlantı dizesi, SQL Kimlik `sqlvmlabel.eastus.cloudapp.azure.com` Doğrulaması'nı kullanan bir DNS etiketiyle bir SQL VM'ye bağlanır (ortak IP adresini de kullanabilirsiniz).

```
Server=sqlvmlabel.eastus.cloudapp.azure.com;Integrated Security=false;User ID=<login_name>;Password=<your_password>
```

Bu, internet üzerinden istemciler için bağlantı sağlar, ancak bu herkesin SQL Server'ınıza bağlanabileceği anlamına gelmez. Dış istemciler doğru kullanıcı adı ve şifre ye sahiptir. Ancak, ek güvenlik için, iyi bilinen bağlantı noktası 1433 önleyebilirsiniz. Örneğin, SQL Server'ı bağlantı noktası 1500'de dinleyecek şekilde yapılandırdıysanız ve uygun güvenlik duvarı ve ağ güvenlik grubu kuralları oluşturduysanız, bağlantı noktası numarasını Sunucu adına ekleyerek bağlanabilirsiniz. Aşağıdaki örnek, sunucu adına özel bir bağlantı noktası numarası olan **1500**ekleyerek öncekini değiştirir:

```
Server=sqlvmlabel.eastus.cloudapp.azure.com,1500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"
```

> [!NOTE]
> SQL Server'ı internet üzerinden bir VM'de sorgularken, Azure veri merkezinden gelen tüm giden veriler [giden veri aktarımlarında](https://azure.microsoft.com/pricing/details/data-transfers/)normal fiyatlandırmaya tabidir.

## <a name="connect-to-sql-server-within-a-virtual-network"></a>Sanal ağ içinde SQL Server'a bağlanma

Portaldaki **SQL bağlantı** türü için **Özel'i** seçtiğinizde, Azure ayarların çoğunu **Genel**olarak yapılandırır. Bir fark, SQL Server bağlantı noktasında (varsayılan 1433) dış trafiğe izin verecek ağ güvenliği grubu kuralı nın olmamasıdır.

> [!IMPORTANT]
> SQL Server Developer ve Express sürümleriiçin sanal makine görüntüleri TCP/IP protokolünü otomatik olarak etkinleştirmez. Geliştirici ve Express sürümleri için, VM'yi oluşturduktan sonra [TCP/IP protokolünü el ile etkinleştirmek](#manualtcp) için SQL Server Configuration Manager'ı kullanmanız gerekir.

Özel bağlantı genellikle sanal [ağ](../../../virtual-network/virtual-networks-overview.md)ile birlikte kullanılır , hangi çeşitli senaryolar sağlar. Bu Sanal M'ler farklı kaynak gruplarında bulunsa bile, VM'leri aynı sanal ağda bağlayabilirsiniz. Ve [siteden siteye VPN](../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)ile, VM'leri şirket içi ağlara ve makinelere bağlayan karma bir mimari oluşturabilirsiniz.

Sanal ağlar, Azure Sanal M'lerinizi bir etki alanına katılmanızı da sağlar. Windows Authentication'ı SQL Server'a kullanmanın tek yolu budur. Diğer bağlantı senaryoları, kullanıcı adları ve parolalarla BIRLIKTE SQL Kimlik Doğrulaması gerektirir.

Sanal ağınızda DNS yapılandırdığınızı varsayarsak, bağlantı dizesinde SQL Server VM bilgisayar adını belirterek SQL Server örneğinize bağlanabilirsiniz. Aşağıdaki örnek, Windows Kimlik Doğrulama'nın da yapılandırıldığını ve kullanıcıya SQL Server örneğine erişim izni verildiğini varsayar.

```
Server=mysqlvm;Integrated Security=true
```

## <a name="change-sql-connectivity-settings"></a><a id="change"></a>SQL bağlantı ayarlarını değiştirme

[!INCLUDE [windows-virtual-machines-sql-use-new-management-blade](../../../../includes/windows-virtual-machines-sql-new-resource.md)]

Azure portalında SQL Server sanal makinenizin bağlantı ayarlarını değiştirebilirsiniz.

1. Azure portalında **SQL sanal makinelerini**seçin.

2. SQL Server VM'nizi seçin.

3. **Ayarlar**altında **Güvenlik'i**seçin.

4. SQL **bağlantı düzeyini** gerekli ayarına göre değiştirin. Bu alanı isteğe bağlı olarak SQL Server bağlantı noktasını veya SQL Kimlik Doğrulama ayarlarını değiştirmek için kullanabilirsiniz.

   ![SQL bağlantısını değiştirme](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity-change.png)

5. Güncelleştirmenin tamamlanması için birkaç dakika bekleyin.

   ![SQL VM güncelleştirme bildirimi](./media/virtual-machines-windows-sql-connect/sql-vm-updating-notification.png)

## <a name="enable-tcpip-for-developer-and-express-editions"></a><a id="manualtcp"></a>Geliştirici ve Express sürümleri için TCP/IP'yi etkinleştirme

SQL Server bağlantı ayarlarını değiştirirken Azure, SQL Server Developer ve Express sürümleri için TCP/IP protokolünü otomatik olarak etkinleştirmez. Aşağıdaki adımlarda, uzaktan IP adresiyle bağlanabilmeniz için TCP/IP’yi el ile nasıl etkinleştirebileceğiniz açıklanmıştır.

İlk olarak, uzak masaüstü ile SQL Server makinesine bağlanın.

[!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-remote-desktop-connect.md)]

Ardından, **SQL Server Configuration Manager**ile TCP/IP protokolünü etkinleştirin.

[!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-connection-tcp-protocol.md)]

## <a name="connect-with-ssms"></a>SSMS ile bağlanma

Aşağıdaki adımlar, Azure VM'niz için isteğe bağlı bir DNS Etiketi oluşturup SQL Server Management Studio 'ya (SSMS) nasıl bağlanışsüreceğini gösterir.

[!INCLUDE [Connect to SQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="manual-configuration-and-troubleshooting"></a><a id="manual"></a>El ile yapılandırma ve sorun giderme

Portal, bağlantıyı otomatik olarak yapılandırmak için seçenekler sağlasa da, bağlantıyı el ile nasıl yapılandıracağını bilmek yararlıdır. Gereksinimleri anlamak da sorun giderme yardımcı olabilir.

Aşağıdaki tabloda, Azure VM'de çalışan SQL Server'a bağlanma gereksinimleri listeleneb.r.a.)

| Gereksinim | Açıklama |
|---|---|
| [SQL Server Kimlik Doğrulama modunu etkinleştirme](/sql/database-engine/configure-windows/change-server-authentication-mode#use-ssms) | Sanal Ağ'da Active Directory'yi yapılandırmadığınız sürece VM'ye uzaktan bağlanmak için SQL Server Authentication gereklidir. |
| [SQL girişi oluşturma](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/create-a-login) | SQL Authentication kullanıyorsanız, hedef veritabanınıza izinleri olan bir kullanıcı adı ve parolaiçeren bir SQL Girişi'ne ihtiyacınız vardır. |
| [TCP/IP protokolünü etkinleştirme](#manualtcp) | SQL Server, TCP üzerinden bağlantılara izin vermelidir. |
| [SQL Server bağlantı noktası için güvenlik duvarı kuralını etkinleştirme](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access) | VM'deki güvenlik duvarı, SQL Server bağlantı noktasında gelen trafiğe izin vermelidir (varsayılan 1433). |
| [TCP 1433 için ağ güvenliği grubu kuralı oluşturma](../../../virtual-network/manage-network-security-group.md#create-a-security-rule) | Internet üzerinden bağlanmak istiyorsanız VM'nin SQL Server bağlantı noktasında (varsayılan 1433) trafik almasına izin vermelisiniz. Yerel ve sanal ağ bağlantıları bunu gerektirmez. Bu, Azure portalında gereken tek adımdır. |

> [!TIP]
> Yukarıdaki tablodaki adımlar, portaldaki bağlantıyı yapılandırdığınızda sizin için yapılır. Bu adımları yalnızca yapılandırmanızı onaylamak veya SQL Server için el ile bağlantı kurmak için kullanın.

## <a name="next-steps"></a>Sonraki Adımlar

Bu bağlantı adımlarıyla birlikte sağlama yönergelerini görmek için [Azure'da SQL Server Virtual Machine Sağlama](virtual-machines-windows-portal-sql-server-provision.md)konusuna bakın.

Azure VM'lerde SQL Server'ı çalıştırmakla ilgili diğer konular için [Azure Sanal Makineler'deki SQL Server'a](virtual-machines-windows-sql-server-iaas-overview.md)bakın.