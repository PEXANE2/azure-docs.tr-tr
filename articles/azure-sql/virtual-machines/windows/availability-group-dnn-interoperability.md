---
title: Kullanılabilirlik grupları ve DNN dinleyicisi ile özellik birlikte çalışabilirliği
description: "Belirli SQL Server özellikleriyle ve Azure VM 'lerinde SQL Server her zaman açık kullanılabilirlik grubuyla dağıtılmış ağ adı (DNN) dinleyicisiyle çalışırken dikkat edilecek ek konular hakkında bilgi edinin. "
services: virtual-machines
documentationCenter: na
author: MashaMSFT
editor: monicar
tags: azure-service-management
ms.service: virtual-machines-sql
ms.subservice: hadr
ms.topic: how-to
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 10/08/2020
ms.author: mathoma
ms.openlocfilehash: 19b4b7407468b19419e2f85193b1f8fb6ace39c3
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "97359413"
---
# <a name="feature-interoperability-with-ag-and-dnn-listener"></a>AG ve DNN dinleyicisi ile özellik birlikte çalışabilirliği 
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Sabit kodlanmış bir sanal ağ adına (VNN) dayanan bazı SQL Server özellikler vardır. Bu nedenle, her zaman açık kullanılabilirlik grubunuz ve Azure VM 'lerinde SQL Server dağıtılmış ağ adı (DNN) dinleyicisi kullanılırken bazı ek hususlar olabilir. 

Bu makalede SQL Server özellikler ve kullanılabilirlik grubu DNN dinleyicisi ile birlikte çalışabilirlik ayrıntıları yer aldığı açıklanır. 


## <a name="client-drivers"></a>İstemci sürücüleri

ODBC, OLEDB, ADO.NET, JDBC, PHP ve Node.js sürücüleri için, kullanıcıların, bağlantı dizesindeki sunucu adı olarak DNN dinleyici adını ve bağlantı noktasını açıkça belirtmesi gerekir. Yük devretme sonrasında hızlı bağlantı sağlamak için, `MultiSubnetFailover=True` SQL istemcisi destekliyorsa bağlantı dizesine ekleyin. 

## <a name="tools"></a>Araçlar

[SQL Server Management Studio](/sql/ssms/sql-server-management-studio-ssms), [sqlcmd](/sql/tools/sqlcmd-utility), [Azure Data Studio](/sql/azure-data-studio/what-is)ve [SQL Server veri araçları](/sql/ssdt/sql-server-data-tools) kullanıcılarının, dinleyiciye bağlanmak için bağlantı dizesindeki sunucu adı olarak DNN dinleyici adını ve bağlantı noktasını açıkça belirtmesi gerekir. 

SQL Server Management Studio (SSMS) GUI 'si aracılığıyla DNN dinleyicisi oluşturma şu anda desteklenmiyor. 


## <a name="availability-groups-and-fci"></a>Kullanılabilirlik grupları ve FCı

Çoğaltmalardan biri olarak bir yük devretme kümesi örneği (FCı) kullanarak, her zaman açık kullanılabilirlik grubu yapılandırabilirsiniz. Bu yapılandırmanın DNN dinleyicisi ile çalışması için [Yük devretme kümesi örneği](failover-cluster-instance-distributed-network-name-dnn-configure.md) , FCI sanal IP adresinin AG DNN IP listesine yerleştirilmesinin bir yolu olmadığından DNN 'yi de kullanmalıdır. 

Bu yapılandırmada, FCı çoğaltmasının yansıtma uç noktası URL 'sinin FCı DNN 'yi kullanması gerekir. Benzer şekilde, FCı salt okunurdur çoğaltma olarak kullanılıyorsa, FCı çoğaltmasına yönelik salt okuma yönlendirmenin FCı DNN 'yi kullanması gerekir. 

Yansıtma uç noktasının biçimi: `ENDPOINT_URL = 'TCP://<FCI DNN DNS name>:<mirroring endpoint port>'` . 

Örneğin, FCı DNN DNS adınız `dnnlsnr` ve `5022` FCI 'nın yansıtma uç noktasının bağlantı noktası olması halinde, uç nokta URL 'sini oluşturmak için Transact-SQL (T-SQL) kod parçacığı şöyle görünür: 

```sql
ENDPOINT_URL = 'TCP://dnnlsnr:5022'
```

Benzer şekilde, salt okuma yönlendirme URL 'SI için biçim: `TCP://<FCI DNN DNS name>:<SQL Server instance port>` . 

Örneğin, DNN DNS adınız `dnnlsnr` ve `1444` salt okunurdur SQL Server FCI tarafından kullanılan bağlantı noktası ise, salt okuma yönlendirme URL 'sini oluşturmak için T-SQL kod parçacığı şöyle görünür: 

```sql
READ_ONLY_ROUTING_URL = 'TCP://dnnlsnr:1444'
```

URL 'deki bağlantı noktasını, varsayılan 1433 bağlantı noktası ise atlayabilirsiniz. Adlandırılmış bir örnek için, adlandırılmış örnek için bir statik bağlantı noktası yapılandırın ve bunu salt okuma yönlendirme URL 'SI içinde belirtin.  

## <a name="distributed-availability-group"></a>Dağıtılmış kullanılabilirlik grubu

Dağıtılmış kullanılabilirlik grupları Şu anda DNN dinleyicisi ile desteklenmiyor. 

## <a name="replication"></a>Çoğaltma

İşlem, birleştirme ve anlık görüntü çoğaltma tüm destek, VNN dinleyicisini, dinleyiciye bağlanan yineleme nesnelerinde DNN dinleyicisi ve bağlantı noktasıyla değiştirme desteği. 

Çoğaltma 'yı kullanılabilirlik gruplarıyla kullanma hakkında daha fazla bilgi için bkz. [Yayımcı ve AG](/sql/database-engine/availability-groups/windows/configure-replication-for-always-on-availability-groups-sql-server), [abone ve AG](/sql/database-engine/availability-groups/windows/replication-subscribers-and-always-on-availability-groups-sql-server)ve [dağıtıcı ve AG](/sql/relational-databases/replication/configure-distribution-availability-group).

## <a name="msdtc"></a>MSDTC

Hem yerel hem de kümelenmiş MSDTC desteklenir, ancak MSDTC, HA bağlantı noktasını yapılandırmak için standart bir Azure Load Balancer gerektiren dinamik bir bağlantı noktası kullanır. Bu nedenle, VM standart bir IP ayırması kullanmalıdır ya da internet 'e sunulamaz. 

Biri RPC uç nokta Eşleyici bağlantı noktası 135 ve diğeri de gerçek MSDTC bağlantı noktası için olmak üzere iki kural tanımlayın. Yük devretmeden sonra, yeni düğümde değiştirildikten sonra LB kuralını yeni MSDTC bağlantı noktasıyla değiştirin. 

MSDTC yerel ise giden iletişime izin vermeyi unutmayın. 

## <a name="distributed-query"></a>Dağıtılmış sorgu 

Dağıtılmış sorgu, AG DNN dinleyicisi ve bağlantı noktası kullanılarak yapılandırılabilen bağlı bir sunucu kullanır. Bağlantı noktası 1433 değilse, bağlı sunucunuzu yapılandırırken SQL Server Management Studio (SSMS) içindeki **diğer veri kaynağını kullan** seçeneğini belirleyin. 

## <a name="filestream"></a>Akışı

FILESTREAM, kullanıcıların Windows dosya API 'sini kullanarak kapsamlı dosya paylaşımında erişebileceği senaryolar için desteklenir ancak desteklenmez. 

## <a name="filetable"></a>Filetable

FileTable, kullanıcıların Windows dosya API 'sini kullanarak kapsamlı dosya paylaşımında erişebileceği senaryolar için desteklenir ancak desteklenmez. 

## <a name="linked-servers"></a>Bağlı sunucular

Ağ DNN dinleyici adını ve bağlantı noktasını kullanarak bağlı sunucuyu yapılandırın. Bağlantı noktası 1433 değilse, bağlı sunucunuzu yapılandırırken SQL Server Management Studio (SSMS) içindeki **diğer veri kaynağını kullan** seçeneğini belirleyin. 


## <a name="frequently-asked-questions"></a>Sık sorulan sorular


- Hangi SQL Server sürümü AG DNN dinleyicisi destekler? 

   SQL Server 2019 CU8 ve üzeri.

- DNN dinleyicisi kullanıldığında beklenen yük devretme süresi nedir?

   DNN dinleyicisi için, yük devretme süresi ek bir süre (Azure Load Balancer kullanırken araştırma süresi gibi) olmadan yalnızca AG yük devretme süresi olacaktır.

- SQL istemcilerinin OLEDB ve ODBC ile DNN 'yi desteklemesi için herhangi bir sürüm gereksinimi var mı?

   `MultiSubnetFailover=True`DNN dinleyicisi için bağlantı dizesi desteği öneririz. SQL Server 2012 (11. x) tarihinden itibaren kullanılabilir.

- DNN dinleyicisini kullanmanı için gereken SQL Server yapılandırma değişiklikleri var mı? 

   SQL Server DNN 'yi kullanmak için herhangi bir yapılandırma değişikliği gerektirmez, ancak bazı SQL Server özellikleri daha fazla değerlendirme gerektirebilir. 

- DNN birden çok alt ağ kümesini destekliyor mu?

   Evet. Küme, DNS 'deki DNN 'yi, alt ağdan bağımsız olarak kullanılabilirlik içindeki tüm çoğaltmaların fiziksel IP adresleriyle bağlar. SQL istemcisi, alt ağdan bağımsız olarak DNS adının tüm IP adreslerini dener. 



## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. 

- [Windows küme teknolojileri](/windows-server/failover-clustering/failover-clustering-overview)   
- [Always on kullanılabilirlik grubu](/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server)

