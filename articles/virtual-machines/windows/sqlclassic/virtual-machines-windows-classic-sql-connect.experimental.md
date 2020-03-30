---
title: Azure'da BIR SQL Server Virtual Machine'e bağlanın (Klasik) | Microsoft Dokümanlar
description: Azure'da Sanal Makine'de çalışan SQL Server'a nasıl bağlanıp bağlanıp bağlanılamayı öğrenin. Bu konu klasik dağıtım modelini kullanır. Senaryolar ağ yapılandırması ve istemcinin konumuna bağlı olarak değişir.
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
ms.openlocfilehash: 1c2d5ae5d85624ea172eb9a95d4086dd71287c4f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75978223"
---
# <a name="connect-to-a-sql-server-virtual-machine-on-azure-classic-deployment"></a>Azure’daki bir SQL Server Sanal Makinesi’ne Bağlanma (Klasik Dağıtım)
> [!div class="op_single_selector"]
> * [Kaynak Yöneticisi](../sql/virtual-machines-windows-sql-connect.md)
> * [Klasik](../classic/sql-connect.md)
> 
> 

## <a name="overview"></a>Genel Bakış
Bu konu, Azure sanal makinede çalışan SQL Server örneğinize nasıl bağlanılabildiğini açıklar. Bazı [genel bağlantı senaryolarını](#connection-scenarios) kapsar ve ardından [Bir Azure VM'de SQL Server bağlantısını yapılandırmak için ayrıntılı adımlar](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm)sağlar.

> [!IMPORTANT] 
> Azure'un kaynakları oluşturmak ve onlarla çalışmak için iki farklı dağıtım modeli vardır: [Kaynak Yöneticisi ve Klasik.](../../../azure-resource-manager/management/deployment-models.md) Bu makalede, Klasik dağıtım modeli kullanılarak kapsar. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Kaynak Yöneticisi VM'leri kullanıyorsanız, [Kaynak Yöneticisi'ni kullanarak Azure'daki SQL Server Virtual Machine'e bağlan'a](../sql/virtual-machines-windows-sql-connect.md)bakın.

## <a name="connection-scenarios"></a>Bağlantı senaryoları
Bir istemcinin Sanal Makine'de çalışan SQL Server'a bağlanma şekli, istemcinin konumuna ve makine/ağ yapılandırmasına bağlı olarak değişir. Bu senaryolar şunlardır:

* [Aynı bulut hizmetinde SQL Server'a bağlanma](#connect-to-sql-server-in-the-same-cloud-service)
* [Internet üzerinden SQL Server'a bağlanın](#connect-to-sql-server-over-the-internet)
* [Aynı sanal ağda SQL Server'a bağlanın](#connect-to-sql-server-in-the-same-virtual-network)

> [!NOTE]
> Bu yöntemlerden herhangi biriyle bağlantı kurmadan önce, [bağlantıyı yapılandırmak için bu makaledeki adımları](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm)izlemeniz gerekir.
> 
> 

### <a name="connect-to-sql-server-in-the-same-cloud-service"></a>Aynı bulut hizmetinde SQL Server'a bağlanma
Aynı bulut hizmetinde birden çok sanal makine oluşturulabilir. Bu sanal makine senaryosunun anlaşılması için sanal [makinelerin sanal ağ veya bulut hizmetiyle nasıl bağlanılabildiğini](/previous-versions/azure/virtual-machines/windows/classic/connect-vms-classic#connect-vms-in-a-standalone-cloud-service)öğrenin. Bu senaryoda, bir sanal makinedeki bir istemci, aynı bulut hizmetinde başka bir sanal makinede çalışan SQL Server'a bağlanmaya çalışır.

Bu senaryoda, VM **Adını** kullanarak bağlanabilirsiniz (portalda **Bilgisayar Adı** veya ana **bilgisayar adı** olarak da gösterilir). Bu, oluşturma sırasında VM için sağladığınız addır. Örneğin, SQL VM **mysqlvm**adını verdiyseniz, aynı bulut hizmetindeki bir istemci VM bağlanmak için aşağıdaki bağlantı dizesini kullanabilir:

    "Server=mysqlvm;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

### <a name="connect-to-sql-server-over-the-internet"></a>Internet üzerinden SQL Server'a bağlanın
SQL Server veritabanı altyapınıza Internet'ten bağlanmak istiyorsanız, gelen TCP iletişimi için sanal bir makine bitiş noktası oluşturmanız gerekir. Bu Azure yapılandırma adımı, gelen TCP bağlantı noktası trafiğini sanal makinenin erişebildiği bir TCP bağlantı noktasına yönlendirir.

Internet üzerinden bağlanmak için VM'nin DNS adını ve VM uç nokta bağlantı noktası numarasını kullanmanız gerekir (bu makalenin ilerleyen saatlerinde yapılandırılır). DNS Adını bulmak için Azure portalına gidin ve **Sanal makineler (klasik)** seçeneğini belirleyin. Sonra sanal makine seçin. **DNS adı** **Genel Bakış** bölümünde gösterilir.

Örneğin, **mysqlvm7777.cloudapp.net** DNS Adı ve **57500**VM bitiş noktası ile **mysqlvm** adlı klasik bir sanal makine düşünün. Düzgün yapılandırılmış bağlantı varsayarsak, aşağıdaki bağlantı dizesi internet üzerinde herhangi bir yerden sanal makineerişmek için kullanılabilir:

    "Server=mycloudservice.cloudapp.net,57500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Bu bağlantı dizesi internet üzerinden istemciler için bağlantı sağlar, ancak bu herkesin SQL Server'ınıza bağlanabileceği anlamına gelmez. Dış istemciler doğru kullanıcı adı ve şifre ye sahiptir. Ek güvenlik için, genel sanal makine bitiş noktası için iyi bilinen bağlantı noktası 1433'u kullanmayın. Ve mümkünse, yalnızca izin verdiğiniz istemciler için trafiği kısıtlamak için bitiş noktanıza bir ACL eklemeyi düşünün. Uç noktaları olan ACL'leri kullanma yla ilgili talimatlar için, [acl'yi bitiş noktasında yönet'e](/previous-versions/azure/virtual-machines/windows/classic/setup-endpoints#manage-the-acl-on-an-endpoint)bakın.

> [!NOTE]
> Bu tekniği SQL Server ile iletişim kurmak için kullandığınızda, Azure veri merkezinden gelen tüm giden veriler [giden veri aktarımlarında](https://azure.microsoft.com/pricing/details/data-transfers/)normal fiyatlandırmaya tabidir.
> 
> 

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>Aynı sanal ağda SQL Server'a bağlanın
[Sanal Ağ](../../../virtual-network/virtual-networks-overview.md) ek senaryolar sağlar. Bu Sanal M'ler farklı bulut hizmetlerinde bulunsa bile, VM'leri aynı sanal ağda bağlayabilirsiniz. Ve [siteden siteye VPN](../../../vpn-gateway/vpn-gateway-site-to-site-create.md)ile, VM'leri şirket içi ağlara ve makinelere bağlayan karma bir mimari oluşturabilirsiniz.

Sanal ağlar, Azure Sanal M'lerinizi bir etki alanına katılmanızı da sağlar. Bir etki alanına katılmak, WINDOWS Kimlik Doğrulaması'nı SQL Server ile kullanmanın tek yoludur. Diğer bağlantı senaryoları, kullanıcı adları ve parolalarla BIRLIKTE SQL Kimlik Doğrulaması gerektirir.

Bir etki alanı ortamı nı ve Windows Kimlik Doğrulamasını yapılandırmayı planlıyorsanız, ortak bitiş noktasını veya SQL Kimlik Doğrulamave oturum açma işlemlerini yapılandırmanız gerekmez. Bu senaryoda, bağlantı dizesinde SQL Server VM adını belirterek SQL Server örneğinize bağlanabilirsiniz. Aşağıdaki örnek, Windows Kimlik Doğrulama'nın yapılandırıldığını ve kullanıcıya SQL Server örneğine erişim izni verildiğini varsayar.

    "Server=mysqlvm;Integrated Security=true"

## <a name="steps-for-configuring-sql-server-connectivity-in-an-azure-vm"></a>Azure VM'de SQL Server bağlantısını yapılandırma adımları
Aşağıdaki adımlar, SQL Server Management Studio (SSMS) kullanarak internet üzerinden SQL Server örneğine nasıl bağlanılabildiğini gösterir. Ancak, sql server sanal makinenizi uygulamalarınız için erişilebilir hale getirmek için de aynı adımlar geçerlidir ve hem şirket içinde hem de Azure'da çalışır.

BAŞKA bir VM veya internetten SQL Server örneğine bağlanabilmek için önce aşağıdaki görevleri tamamlamanız gerekir:

* [Sanal makine için TCP uç noktası oluşturma](#create-a-tcp-endpoint-for-the-virtual-machine)
* [Windows güvenlik duvarında TCP bağlantı noktalarını açma](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
* [SQL Server’ı TCP protokolünde dinleyecek şekilde yapılandırma](#configure-sql-server-to-listen-on-the-tcp-protocol)
* [SQL Server’ı karma mod kimlik doğrulaması için yapılandırma](#configure-sql-server-for-mixed-mode-authentication)
* [SQL Server kimlik doğrulaması için oturum açma kimliği oluşturma](#create-sql-server-authentication-logins)
* [Sanal makinenin DNS adını belirleme](#determine-the-dns-name-of-the-virtual-machine)
* [Başka bir bilgisayardan Veritabanı Altyapısına bağlanma](#connect-to-the-database-engine-from-another-computer)

Bağlantı yolu aşağıdaki diyagramla özetlenir:

![SQL Server sanal makinesine bağlanma](../../../../includes/media/virtual-machines-sql-server-connection-steps/SQLServerinVMConnectionMap.png)

[!INCLUDE [Connect to SQL Server in a VM Classic TCP Endpoint](../../../../includes/virtual-machines-sql-server-connection-steps-classic-tcp-endpoint.md)]

[!INCLUDE [Connect to SQL Server in a VM](../../../../includes/virtual-machines-sql-server-connection-steps.md)]

[!INCLUDE [Connect to SQL Server in a VM Classic Steps](../../../../includes/virtual-machines-sql-server-connection-steps-classic.md)]

## <a name="next-steps"></a>Sonraki Adımlar
Yüksek kullanılabilirlik ve olağanüstü durum kurtarma için AlwaysOn Kullanılabilirlik Gruplarını da kullanmayı planlıyorsanız, bir dinleyici uygulamayı düşünmelisiniz. Veritabanı istemcileri doğrudan SQL Server örneklerinden birine değil, dinleyiciye bağlanır. Dinleyici, istemcileri kullanılabilirlik grubundaki birincil yinelemeye yönlendirir. Daha fazla bilgi için [Azure'daki Her Zaman Kullanılabilirlik Grupları için Bir ILB dinleyicisini Yapılandır' a](../classic/ps-sql-int-listener.md)bakın.

Azure sanal makinede çalışan SQL Server için en iyi güvenlik uygulamalarını gözden geçirmek önemlidir. Daha fazla bilgi için bkz. [Azure Sanal Makineler'de SQL Server için Güvenlikle İlgili Dikkat Edilmesi Gerekenler](../sql/virtual-machines-windows-sql-security.md).

Azure Virtual Machines’de SQL Server için.[Öğrenme Yolunu keşfedin](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/). 

Azure VM'lerde SQL Server'ı çalıştırmakla ilgili diğer konular için [Azure Sanal Makineler'deki SQL Server'a](../sql/virtual-machines-windows-sql-server-iaas-overview.md)bakın.

