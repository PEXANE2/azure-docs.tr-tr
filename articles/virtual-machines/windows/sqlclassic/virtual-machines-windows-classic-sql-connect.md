---
title: Azure 'da SQL Server sanal makinesine bağlanma (klasik) | Microsoft Docs
description: Azure 'daki bir sanal makinede çalışan SQL Server bağlanmayı öğrenin. Bu konu klasik dağıtım modelini kullanır. Senaryolar ağ yapılandırmasına ve istemcinin konumuna göre farklılık gösterir.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-service-management
ms.assetid: 416948af-454f-4cfe-8fd2-7cf971cbd3e9
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/31/2017
ms.author: mathoma
ms.reviewer: jroth
experimental: true
experimental_id: d51f3cc6-753b-4e
ms.openlocfilehash: 0717f6f75b6bd8bb7ba4d53f8240414b5169540d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84014799"
---
# <a name="connect-to-a-sql-server-virtual-machine-on-azure-classic-deployment"></a>Azure’daki bir SQL Server Sanal Makinesi’ne Bağlanma (Klasik Dağıtım)
> [!div class="op_single_selector"]
> * [Resource Manager](../../../azure-sql/virtual-machines/windows/ways-to-connect-to-sql.md)
> * [Klasik](../classic/sql-connect.md)
> 
> 

## <a name="overview"></a>Genel Bakış
Bu konu, bir Azure sanal makinesinde çalışan SQL Server örneğine nasıl bağlanabileceğinizi açıklamaktadır. Bazı [genel bağlantı senaryolarını](#connection-scenarios) ele alır ve [Azure VM 'de SQL Server bağlantısını yapılandırmaya yönelik ayrıntılı adımlar](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm)sağlar.

> [!IMPORTANT] 
> Azure 'da kaynak oluşturmak ve bunlarla çalışmak için iki farklı dağıtım modeli vardır: [Kaynak Yöneticisi ve klasik](../../../azure-resource-manager/management/deployment-models.md). Bu makalede, klasik dağıtım modelinin kullanımı ele alınmaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Kaynak Yöneticisi VM 'Leri kullanıyorsanız, bkz. [Kaynak Yöneticisi kullanarak Azure 'da SQL Server sanal makinesine bağlanma](../../../azure-sql/virtual-machines/windows/ways-to-connect-to-sql.md).

## <a name="connection-scenarios"></a>Bağlantı senaryoları
Bir istemcinin sanal makinede çalışan SQL Server bağlandığı şekilde, istemcinin konumuna ve makine/ağ yapılandırmasına bağlı olarak farklılık gösterir. Bu senaryolar şunlardır:

* [Aynı bulut hizmetindeki SQL Server bağlanma](#connect-to-sql-server-in-the-same-cloud-service)
* [Internet üzerinden SQL Server bağlanma](#connect-to-sql-server-over-the-internet)
* [Aynı sanal ağda SQL Server Bağlan](#connect-to-sql-server-in-the-same-virtual-network)

> [!NOTE]
> Bu yöntemlerin herhangi birine bağlanmadan önce, [bağlantıyı yapılandırmak için bu makaledeki adımları](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm)izlemeniz gerekir.
> 
> 

### <a name="connect-to-sql-server-in-the-same-cloud-service"></a>Aynı bulut hizmetindeki SQL Server bağlanma
Aynı bulut hizmetinde birden çok sanal makine oluşturulabilir. Bu sanal makineler senaryosunu anlamak için bkz. sanal [makineleri bir sanal ağ veya bulut hizmeti ile bağlama](/previous-versions/azure/virtual-machines/windows/classic/connect-vms-classic#connect-vms-in-a-standalone-cloud-service). Bu senaryo, bir sanal makinedeki bir istemci aynı bulut hizmetindeki başka bir sanal makinede çalışan SQL Server bağlanmaya çalıştığında olur.

Bu senaryoda, VM **adını** (portalda **bilgisayar adı** veya **ana bilgisayar** adı olarak da gösterilir) kullanarak bağlanabilirsiniz. Bu, oluşturma sırasında VM için verdiğiniz addır. Örneğin, SQL VM 'nizi, **mysqlvm**'niz olarak adlandırdıysanız, aynı bulut hizmetindeki bir istemci sanal makinesi bağlanmak için aşağıdaki bağlantı dizesini kullanabilir:

    "Server=mysqlvm;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

### <a name="connect-to-sql-server-over-the-internet"></a>Internet üzerinden SQL Server bağlanma
SQL Server veritabanı altyapısına Internet 'ten bağlanmak istiyorsanız, gelen TCP iletişimi için bir sanal makine uç noktası oluşturmanız gerekir. Bu Azure yapılandırma adımı, gelen TCP bağlantı noktası trafiğini sanal makinenin erişebildiği bir TCP bağlantı noktasına yönlendirir.

Internet üzerinden bağlanmak için, sanal makinenin DNS adını ve VM uç noktası bağlantı noktası numarasını (Bu makalede daha sonra yapılandırılan) kullanmanız gerekir. DNS adını bulmak için Azure portal gidin ve **sanal makineler (klasik)** öğesini seçin. Ardından sanal makinenizi seçin. **DNS adı** **genel bakış** bölümünde gösterilir.

Örneğin, **MYSQLVM7777.CLOUDAPP.net** DNS adına sahip **mysqlvm** adlı klasik bir sanal makineyi ve **57500**VM uç noktasını düşünün. Doğru şekilde yapılandırılmış bağlantı olduğunu varsayarsak, sanal makineye İnternet üzerinde herhangi bir yerden erişmek için aşağıdaki bağlantı dizesi kullanılabilir:

    "Server=mycloudservice.cloudapp.net,57500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Bu, internet üzerinden istemciler için bağlantı sağlamasına karşın, bu, herkesin SQL Server bağlanabileceği anlamına gelmez. Dış istemciler, doğru Kullanıcı adı ve parolaya sahiptir. Ek güvenlik için, ortak sanal makine uç noktası için iyi bilinen 1433 numaralı bağlantı noktasını kullanmayın. Mümkünse, trafiği yalnızca izin veren istemcilerle kısıtlamak için uç noktanıza bir ACL eklemeyi düşünün. ACL 'Leri uç noktalarla kullanma hakkında yönergeler için bkz. [bir uç noktada ACL 'Yi yönetme](/previous-versions/azure/virtual-machines/windows/classic/setup-endpoints#manage-the-acl-on-an-endpoint).

> [!NOTE]
> SQL Server ile iletişim kurmak için bu tekniği kullandığınızda, Azure veri merkezindeki tüm giden verilerin [giden veri aktarımları için normal fiyatlandırmaya](https://azure.microsoft.com/pricing/details/data-transfers/)tabi olduğunu unutmayın.
> 
> 

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>Aynı sanal ağda SQL Server Bağlan
[Sanal ağ](../../../virtual-network/virtual-networks-overview.md) , ek senaryolar sunar. Aynı sanal ağdaki VM 'lere, bu VM 'Ler farklı bulut hizmetlerinde bulunsa bile bağlayabilirsiniz. [Siteden sıteye VPN](../../../vpn-gateway/vpn-gateway-site-to-site-create.md)ile, şirket içi ağlar ve makinelerle VM 'leri bağlayan bir karma mimari de oluşturabilirsiniz.

Sanal ağlar Ayrıca, Azure VM 'lerinizi bir etki alanına katmanızı sağlar. Bu, SQL Server için Windows kimlik doğrulaması kullanmanın tek yoludur. Diğer bağlantı senaryoları, Kullanıcı adları ve parolalarla SQL kimlik doğrulaması gerektirir.

Bir etki alanı ortamı ve Windows kimlik doğrulaması yapılandıracaksanız, genel uç noktasını veya SQL kimlik doğrulamasını ve oturum açmaları yapılandırmak için bu makaledeki adımları kullanmanız gerekmez. Bu senaryoda, bağlantı dizesinde SQL Server VM adını belirterek SQL Server örneğine bağlanabilirsiniz. Aşağıdaki örnek, Windows kimlik doğrulamasının de yapılandırıldığını ve kullanıcıya SQL Server örneğine erişim verildiğini varsayar.

    "Server=mysqlvm;Integrated Security=true"

## <a name="steps-for-configuring-sql-server-connectivity-in-an-azure-vm"></a>Azure VM 'de SQL Server bağlantısını yapılandırma adımları
Aşağıdaki adımlarda, SQL Server Management Studio (SSMS) kullanarak Internet üzerinden SQL Server örneğine nasıl bağlanabileceğiniz gösterilmektedir. Ancak, SQL Server sanal makinenizi, hem şirket içinde hem de Azure 'da çalışan uygulamalarınız için erişilebilir hale getirmek için de aynı adımlar geçerlidir.

Başka bir VM 'den veya Internet 'ten SQL Server örneğine bağlanabilmeniz için, aşağıdaki bölümlerde açıklanan şekilde aşağıdaki görevleri gerçekleştirmeniz gerekir:

* [Sanal makine için TCP uç noktası oluşturma](#create-a-tcp-endpoint-for-the-virtual-machine)
* [Windows güvenlik duvarında TCP bağlantı noktalarını açma](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
* [SQL Server’ı TCP protokolünde dinleyecek şekilde yapılandırma](#configure-sql-server-to-listen-on-the-tcp-protocol)
* [SQL Server’ı karma mod kimlik doğrulaması için yapılandırma](#configure-sql-server-for-mixed-mode-authentication)
* [SQL Server kimlik doğrulaması için oturum açma kimliği oluşturma](#create-sql-server-authentication-logins)
* [Sanal makinenin DNS adını belirleme](#determine-the-dns-name-of-the-virtual-machine)
* [Başka bir bilgisayardan Veritabanı Altyapısına bağlanma](#connect-to-the-database-engine-from-another-computer)

Bağlantı yolu aşağıdaki diyagram tarafından özetlenir:

![SQL Server sanal makinesine bağlanma](../../../../includes/media/virtual-machines-sql-server-connection-steps/SQLServerinVMConnectionMap.png)

[!INCLUDE [Connect to SQL Server in a VM Classic TCP Endpoint](../../../../includes/virtual-machines-sql-server-connection-steps-classic-tcp-endpoint.md)]

[!INCLUDE [Connect to SQL Server in a VM](../../../../includes/virtual-machines-sql-server-connection-steps.md)]

[!INCLUDE [Connect to SQL Server in a VM Classic Steps](../../../../includes/virtual-machines-sql-server-connection-steps-classic.md)]

## <a name="next-steps"></a>Sonraki Adımlar
Ayrıca yüksek kullanılabilirlik ve olağanüstü durum kurtarma için AlwaysOn Kullanılabilirlik Grupları kullanmayı planlıyorsanız, bir dinleyici uygulamayı düşünmelisiniz. Veritabanı istemcileri, SQL Server örneklerinden birine doğrudan değil, dinleyiciye bağlanır. Dinleyici, istemcileri kullanılabilirlik grubundaki birincil çoğaltmaya yönlendirir. Daha fazla bilgi için bkz. [Azure 'da AlwaysOn kullanılabilirlik grupları için BIR ıLB dinleyicisi yapılandırma](../classic/ps-sql-int-listener.md).

Azure sanal makinesinde çalışan SQL Server için en iyi güvenlik uygulamalarını gözden geçirmeniz önemlidir. Daha fazla bilgi için bkz. [Azure Sanal Makineler'de SQL Server için Güvenlikle İlgili Dikkat Edilmesi Gerekenler](../../../azure-sql/virtual-machines/windows/security-considerations-best-practices.md).

Azure Virtual Machines’de SQL Server için.[Öğrenme Yolunu keşfedin](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/). 

Azure VM 'lerinde SQL Server çalıştırmaya ilişkin diğer konular için bkz. [Azure sanal makinelerinde SQL Server](../../../azure-sql/virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview.md).

