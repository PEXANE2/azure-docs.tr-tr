---
title: Bağlantı türleri
titleSuffix: Azure SQL Managed Instance
description: Azure SQL yönetilen örnek bağlantı türleri hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: vanto
ms.date: 10/07/2019
ms.openlocfilehash: cee913e846ebfef174a3cd6383401eace89187f0
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84044342"
---
# <a name="azure-sql-managed-instance-connection-types"></a>Azure SQL yönetilen örnek bağlantı türleri
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Bu makalede, istemcilerin bağlantı türüne bağlı olarak Azure SQL yönetilen örneği 'ne nasıl bağlanacağı açıklanmaktadır. Bağlantı türlerini değiştirmeye yönelik betik örnekleri aşağıda verilmiştir ve varsayılan bağlantı ayarlarını değiştirmekle ilgili önemli noktalar verilmektedir.

## <a name="connection-types"></a>Bağlantı türleri

Azure SQL yönetilen örneği aşağıdaki iki bağlantı türünü destekler:

- **Yeniden yönlendir (önerilir):** İstemciler, veritabanını barındıran düğüme doğrudan bağlantı kurar. Yeniden yönlendirme kullanarak bağlantı sağlamak için, 1433 ve 11000-11999 bağlantı noktalarında erişime izin vermek üzere güvenlik duvarları ve ağ güvenlik grupları (NSG) açmanız gerekir. Paketler doğrudan veritabanına gider ve bu nedenle proxy üzerinden yeniden yönlendirme kullanan gecikme süresi ve işleme performans iyileştirmeleri vardır.
- **Ara sunucu (varsayılan):** Bu modda, tüm bağlantılar proxy ağ geçidi bileşeni kullanıyor. Bağlantıyı etkinleştirmek için, yalnızca özel ağlar için bağlantı noktası 1433 ve genel bağlantı bağlantı noktası 3342 ' nin açılması gerekir. Bu modun seçilmesi, iş yükünün doğasına bağlı olarak daha yüksek gecikme süresine ve düşük aktarım hızına yol açabilir. En düşük gecikme süresi ve en yüksek aktarım hızı için proxy bağlantı İlkesi üzerinden yeniden yönlendirme bağlantı ilkesini öneririz.

## <a name="redirect-connection-type"></a>Yeniden yönlendirme bağlantı türü

Yeniden yönlendirme bağlantı türü, TCP oturumu SQL altyapısına kurulduktan sonra, istemci oturumu yük dengeleyiciden sanal küme düğümünün hedef sanal IP 'sini edinir. Sonraki paketler, ağ geçidini atlayarak doğrudan sanal küme düğümüne akar. Aşağıdaki diyagramda bu trafik akışı gösterilmektedir.

![yeniden yönlendirme. png](./media/connection-types-overview/redirect.png)

> [!IMPORTANT]
> Yeniden yönlendirme bağlantı türü şu anda yalnızca özel uç nokta için çalışıyor. Bağlantı türü ayarından bağımsız olarak, genel uç nokta üzerinden gelen bağlantılar bir ara sunucu üzerinden olur.

## <a name="proxy-connection-type"></a>Proxy bağlantı türü

Ara sunucu bağlantı türü, TCP oturumunun ağ geçidi kullanılarak kurulduğu ve sonraki tüm paketlerin üzerinden akış yaptığı anlamına gelir. Aşağıdaki diyagramda bu trafik akışı gösterilmektedir.

![Proxy. png](./media/connection-types-overview/proxy.png)

## <a name="script-to-change-connection-type-settings-using-powershell"></a>PowerShell kullanarak bağlantı türü ayarlarını değiştirme betiği

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Aşağıdaki PowerShell betiği, bir SQL yönetilen örneği için bağlantı türünün olarak nasıl değiştirileceğini gösterir `Redirect` .

```powershell
Install-Module -Name Az
Import-Module Az.Accounts
Import-Module Az.Sql

Connect-AzAccount
# Get your SubscriptionId from the Get-AzSubscription command
Get-AzSubscription
# Use your SubscriptionId in place of {subscription-id} below
Select-AzSubscription -SubscriptionId {subscription-id}
# Replace {rg-name} with the resource group for your SQL Managed Instance, and replace {mi-name} with the name of your SQL Managed Instance
$mi = Get-AzSqlInstance -ResourceGroupName {rg-name} -Name {mi-name}
$mi = $mi | Set-AzSqlInstance -ProxyOverride "Redirect" -force
```

## <a name="next-steps"></a>Sonraki adımlar

- [Veritabanını bir SQL yönetilen örneğine geri yükleme](restore-sample-database-quickstart.md)
- [SQL yönetilen örneği üzerinde genel uç nokta yapılandırmayı](public-endpoint-configure.md) öğrenin
- [SQL yönetilen örnek bağlantı mimarisi](connectivity-architecture-overview.md) hakkında bilgi edinin