---
title: Dağıtılmış ağ adı (DNN) yapılandırma
description: Azure VM yük devretme kümesi örneği (FCı) üzerinde SQL Server trafiği yönlendirmek için bir dağıtılmış ağ adı (DNN) yapılandırma hakkında bilgi edinin.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: jroth
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/02/2020
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 7c40f4d9f86f27af34c1bc649483810f6756c41d
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86169825"
---
# <a name="configure-a-distributed-network-name-for-an-fci"></a>FCı için dağıtılmış ağ adı yapılandırma 
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Azure sanal makineler 'de, trafiği uygun kümelenmiş kaynağa yönlendirmek için dağıtılmış ağ adı (DNN) kullanılır. Bu, Azure Load Balancer gereksinimi olmadan sanal ağ adından (VNN) SQL Server yük devretme kümesi örneğine (FCı) bağlanmak için daha kolay bir yol sağlar. Bu özellik şu anda önizleme aşamasındadır ve yalnızca SQL Server 2019 CU2 UYGULAMAZSANıZ ve üzeri ve Windows Server 2016 ve üzeri için kullanılabilir. 

Bu makalede, yüksek kullanılabilirlik ve olağanüstü durum kurtarma (HADR) için Azure VM 'lerinde SQL Server hesabınızı Fcsize yönlendirmek üzere DNN 'yi yapılandırma öğretilir. 

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları tamamlamadan önce, zaten şunları yapmalısınız:

- Dağıtılmış ağ adının [HADR çözümünüz için uygun bağlantı seçeneği](hadr-cluster-best-practices.md#connectivity)olduğuna karar verilir.
- [Yük devretme kümesi örneklerinizi](failover-cluster-instance-overview.md)yapılandırdınız. 
- En son [PowerShell](/powershell/azure/install-az-ps)sürümü yüklendi. 

## <a name="create-dnn-resource"></a>DNN kaynağı oluşturma 

DNN kaynağı, SQL Server FCı ile aynı küme grubunda oluşturulur. FCı küme grubu içinde DNN kaynağını oluşturmak için PowerShell 'i kullanın. 

Aşağıdaki PowerShell komutu, kaynak adı SQL Server FCı kümesi grubuna DNN kaynağı ekler `<dnnResourceName>` . Kaynak adı bir kaynağı benzersiz bir şekilde tanımlamak için kullanılır. Sizin için anlamlı olan ve küme genelinde benzersiz olan bir tane kullanın. Kaynak türü olmalıdır `Distributed Network Name` . 

`-Group`Değerin, dağıtılmış ağ adını eklemek istediğiniz SQL Server FCI 'ye karşılık gelen küme grubunun adı olması gerekir. Varsayılan örnek için tipik biçim olur `SQL Server (MSSQLSERVER)` . 


```powershell
Add-ClusterResource -Name <dnnResourceName> `
-ResourceType "Distributed Network Name" -Group "<WSFC role of SQL server instance>"
```

Örneğin, `dnn-demo` bir varsayılan SQL Server FCI için DNN kaynağınızı oluşturmak için aşağıdaki PowerShell komutunu kullanın:

```powershell
Add-ClusterResource -Name dnn-demo `
-ResourceType "Distributed Network Name" -Group "SQL Server (MSSQLSERVER)"

```

## <a name="set-cluster-dnn-dns-name"></a>Küme DNN DNS adını ayarla

Kümedeki DNN kaynağı için DNS adını ayarlayın. Daha sonra küme, trafiği Şu anda SQL Server FCı barındıran düğüme yönlendirmek için bu değeri kullanır. 
 
İstemciler SQL Server FCı 'ya bağlanmak için DNS adını kullanır. Benzersiz bir değer seçebilirsiniz. Ya da zaten var olan bir FCı varsa ve istemci bağlantı dizelerini güncelleştirmek istemiyorsanız DNN 'yi istemcilerin zaten kullandığı geçerli VNN 'yi kullanacak şekilde yapılandırabilirsiniz. Bunu yapmak için, DNS 'de DNN 'yi ayarlamadan önce [VNN öğesini yeniden adlandırmanız](#rename-the-vnn) gerekir.


DNN 'niz için DNS adını ayarlamak üzere bu komutu kullanın: 

```powershell
Get-ClusterResource -Name <dnnResourceName> | `
Set-ClusterParameter -Name DnsName -Value <DNSName>
```

Bu `DNSName` değer, istemcilerin SQL Server FCI 'ya bağlanmak için kullanacağı değerdir. Örneğin, istemcilerin bağlanması için `FCIDNN` aşağıdaki PowerShell komutunu kullanın:

```powershell
Get-ClusterResource -Name dnn-demo | `
Set-ClusterParameter -Name DnsName -Value FCIDNN
```

İstemciler şimdi `FCIDNN` SQL Server FCI 'ya bağlanılırken bağlantı dizesine girer. 

   > [!WARNING]
   > FCı altyapısının gerekli bir bileşeni olduğundan geçerli sanal ağ adını (VNN) silmeyin. 


### <a name="rename-the-vnn"></a>VNN 'i yeniden adlandırma 

Var olan bir sanal ağ adınız varsa ve istemcilerin SQL Server FCı 'ya bağlanmak için bu değeri kullanmaya devam etmesini istiyorsanız, geçerli VNN değerini bir yer tutucu değeri olarak yeniden adlandırmanız gerekir. Geçerli VNN yeniden adlandırıldıktan sonra DNN için DNS adı değerini VNN olarak ayarlayabilirsiniz. 

VNN 'i yeniden adlandırmak için bazı kısıtlamalar uygulanır. Daha fazla bilgi için bkz. [FCI 'Yi yeniden adlandırma](/sql/sql-server/failover-clusters/install/rename-a-sql-server-failover-cluster-instance).

Geçerli VNN kullanılması işiniz için gerekli değilse, bu bölümü atlayın. VNN 'yi yeniden adlandırdıktan sonra, [küme DNN DNS adını ayarlayın](#set-cluster-dnn-dns-name). 

   
## <a name="set-dnn-resource-online"></a>DNN kaynağını çevrimiçi ayarla

DNN kaynağınız uygun şekilde adlandırıldıktan sonra ve kümede DNS adı değerini ayarladıktan sonra, PowerShell kullanarak DNN kaynağını kümede çevrimiçi olarak ayarlayın: 

```powershell
Start-ClusterResource -Name <dnnResourceName>
```

Örneğin, DNN kaynağını başlatmak için `dnn-demo` aşağıdaki PowerShell komutunu kullanın: 

```powershell
Start-ClusterResource -Name dnn-demo
```

## <a name="configure-possible-owners"></a>Olası sahipleri yapılandırma

Varsayılan olarak, küme DNN DNS adını kümedeki tüm düğümlere bağlar. Ancak, kümedeki SQL Server FCı 'nin parçası olmayan düğümler DNN olası sahipler listesinden dışlanmalıdır. 

Olası sahipleri güncelleştirmek için şu adımları izleyin:

1. Yük Devretme Kümesi Yöneticisi DNN kaynağına gidin. 
1. DNN kaynağına sağ tıklayın ve **Özellikler**' i seçin. 
   :::image type="content" source="media/hadr-distributed-network-name-dnn-configure/fci-dnn-properties.png" alt-text="DNN kaynağı için kısayol menüsü, Özellikler komutuyla vurgulanır.":::
1. Yük devretme kümesi örneğine katılmayan düğümlerin onay kutusunu temizleyin. DNN kaynağı için olası sahipler listesi, SQL Server örneği kaynağı için olası sahipler listesiyle eşleşmelidir. Örneğin, DATA3 'in FCı 'ye katılmadığından, aşağıdaki görüntüde DNN kaynağı için olası sahipler listesinden DATA3 kaldırma örneği verilmiştir: 

   :::image type="content" source="media/hadr-distributed-network-name-dnn-configure/clear-check-for-nodes-not-in-fci.png" alt-text="DNN kaynağının olası sahipleri için FCı 'ye katılmayan düğümlerin yanındaki onay kutusunu temizleyin":::

1. Ayarlarınızı kaydetmek için **Tamam ' ı** seçin. 


## <a name="restart-sql-server-instance"></a>SQL Server örneğini yeniden Başlat 

SQL Server örneğini yeniden başlatmak için Yük Devretme Kümesi Yöneticisi kullanın. Şu adımları izleyin:

1. Yük Devretme Kümesi Yöneticisi SQL Server kaynağına gidin.
1. SQL Server kaynağına sağ tıklayın ve çevrimdışına alın. 
1. Tüm ilişkili kaynaklar çevrimdışı olduktan sonra, SQL Server kaynağına sağ tıklayıp tekrar çevrimiçi duruma getirin. 

## <a name="update-connection-string"></a>Bağlantı dizesini Güncelleştir

Yük devretme sonrasında hızlı bağlantı sağlamak için, `MultiSubnetFailover=True` SQL istemci sürümü 4.6.1 ' den daha eski olursa bağlantı dizesine ekleyin. 

Ayrıca, DNN özgün VNN kullanıyorsa, FCı SQL Server bağlanan SQL istemcilerinin bağlantı dizelerini DNN DNS adına güncelleştirmesi gerekir. Bu gereksinimden kaçınmak için DNS ad değerini VNN adı olacak şekilde güncelleştirebilirsiniz. Ancak [var olan VNN öğesini önce bir yer tutucu ile değiştirmeniz](#rename-the-vnn) gerekir. 

## <a name="test-failover"></a>Yük devretme testi

Küme işlevselliğini doğrulamak için kümelenmiş kaynağın yük devretmesini test edin. 

Yük devretmeyi sınamak için aşağıdaki adımları izleyin: 

1. RDP kullanarak SQL Server küme düğümlerinden birine bağlanın.
1. **Yük devretme kümesi Yöneticisi**açın. **Rolleri**seçin. Hangi düğümün SQL Server FCı rolüne sahip olduğuna dikkat edin.
1. SQL Server FCı rolüne sağ tıklayın. 
1. **Taşı**' yı seçin ve ardından **mümkün olan en iyi düğümü**seçin.

**Yük devretme kümesi Yöneticisi** rolü gösterir ve kaynakları çevrimdışı duruma geçer. Kaynaklar daha sonra taşınır ve diğer düğümde yeniden çevrimiçi duruma gelir.

## <a name="test-connectivity"></a>Bağlantıyı test etme

Bağlantıyı sınamak için aynı sanal ağdaki başka bir sanal makinede oturum açın. **SQL Server Management Studio** açın ve DNN DNS adını kullanarak SQL Server FCI 'ye bağlanın.

Gerekirse, [SQL Server Management Studio indirebilirsiniz](/sql/ssms/download-sql-server-management-studio-ssms).

## <a name="limitations"></a>Sınırlamalar

- Şu anda DNN, Windows Server 2016 üzerinde yalnızca SQL Server 2019 CU2 UYGULAMAZSANıZ ve üzeri için desteklenir. 
- Şu anda DNN yalnızca Azure VM 'lerinde SQL Server olan yük devretme kümesi örnekleri için desteklenir. Kullanılabilirlik grubu dinleyicileri için Azure Load Balancer sanal ağ adını kullanın.
- Diğer SQL Server özellikleriyle ve DNN içeren bir FCı ile çalışırken daha fazla dikkat edilmesi gerekebilir. Daha fazla bilgi için bkz. [DNN birlikte çalışabilirliği Ile FCI](failover-cluster-instance-dnn-interoperability.md). 

## <a name="next-steps"></a>Sonraki adımlar

Azure 'da SQL Server HADR özellikleri hakkında daha fazla bilgi için bkz. [kullanılabilirlik grupları](availability-group-overview.md) ve [Yük devretme kümesi örneği](failover-cluster-instance-overview.md). Ayrıca, yüksek kullanılabilirlik ve olağanüstü durum kurtarma için ortamınızı yapılandırmaya yönelik [en iyi yöntemleri](hadr-cluster-best-practices.md) öğrenebilirsiniz. 

DNN ve FCı ile birlikte kullanıldığında bazı belirli SQL Server özellikleri için ek yapılandırma gereksinimleri olabilir. Daha fazla bilgi için bkz. [DNN birlikte çalışabilirliği Ile FCI](failover-cluster-instance-dnn-interoperability.md) . 

