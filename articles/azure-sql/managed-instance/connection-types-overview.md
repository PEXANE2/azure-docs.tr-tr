---
title: Bağlantı türleri
titleSuffix: Azure SQL Managed Instance
description: Azure SQL yönetilen örnek bağlantı türleri hakkında bilgi edinin
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: vanto
ms.date: 10/07/2019
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: f1c4fe8268d24026609f55d76a102a5c9a4e8295
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "91356325"
---
# <a name="azure-sql-managed-instance-connection-types"></a>Azure SQL Yönetilen Örneği bağlantı türleri
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Bu makalede, istemcilerin bağlantı türüne bağlı olarak Azure SQL yönetilen örneği 'ne nasıl bağlanacağı açıklanmaktadır. Bağlantı türlerini değiştirmeye yönelik betik örnekleri aşağıda verilmiştir ve varsayılan bağlantı ayarlarını değiştirmekle ilgili önemli noktalar verilmektedir.

## <a name="connection-types"></a>Bağlantı türleri

Azure SQL yönetilen örneği aşağıdaki iki bağlantı türünü destekler:

- **Yeniden yönlendir (önerilir):** İstemciler, veritabanını barındıran düğüme doğrudan bağlantı kurar. Yeniden yönlendirmeyi kullanarak bağlantıyı etkinleştirmek için güvenlik duvarlarını ve Ağ Güvenlik Gruplarını (NSG) 1433 ve 11000-11999 numaralı bağlantı noktalarına erişim izni verecek şekilde açmanız gerekir. Paketler doğrudan veritabanına gider ve bu nedenle ara sunucu yerine yeniden yönlendirme kullanıldığında gecikme süresinde ve aktarım hızı performansında gelişmeler görülür.
- **Ara sunucu (varsayılan):** Bu modda, tüm bağlantılar proxy ağ geçidi bileşeni kullanıyor. Bağlantıyı etkinleştirmek için yalnızca özel ağlarda 1433 numaralı bağlantı noktası ve genel bağlantıda 3342 numaralı bağlantı noktası açılmalıdır. Bu modun seçilmesi, iş yükünün doğasına bağlı olarak gecikme süresini uzatır ve aktarım hızını düşürür. En düşük gecikme süresi ve en yüksek aktarım hızı için ara sunucu bağlantı ilkesi yerine yeniden yönlendirme bağlantı ilkesinin kullanılmasını kesinlikle öneririz.

## <a name="redirect-connection-type"></a>Yeniden yönlendirme bağlantı türü

Yönlendirme bağlantısı türünde, TCP oturumu SQL altyapısına kurulduktan sonra, istemci oturumu yük dengeleyiciden sanal küme düğümünün hedef sanal IP 'sini edinir. Sonraki paketler, ağ geçidini atlayarak doğrudan sanal küme düğümüne akar. Aşağıdaki diyagramda bu trafik akışı gösterilmektedir.

![Diyagram, bir Azure sanal ağındaki bir ağ geçidine bağlı olan, yeniden yönlendirme-bulma-DB ile şirket içi bir ağı ve sanal ağdaki bir veritabanı birincil düğümüne bağlı bir yeniden yönlendirme sorgusu gösterir.](./media/connection-types-overview/redirect.png)

> [!IMPORTANT]
> Yeniden yönlendirme bağlantı türü şu anda yalnızca özel bir uç nokta için çalışıyor. Bağlantı türü ayarından bağımsız olarak, genel uç nokta üzerinden gelen bağlantılar bir ara sunucu üzerinden olur.

## <a name="proxy-connection-type"></a>Proxy bağlantı türü

Ara sunucu bağlantı türünde TCP oturumu, ağ geçidi kullanılarak oluşturulur ve sonraki tüm paketler üzerinden akar. Aşağıdaki diyagramda bu trafik akışı gösterilmektedir.

![Diyagramda bir Azure sanal ağındaki ağ geçidine bağlı bir ara sunucu içeren bir şirket içi ağ görüntülenir, sanal ağdaki bir veritabanı birincil düğümünün yanına bağlanın.](./media/connection-types-overview/proxy.png)

## <a name="changing-connection-type"></a>Bağlantı türünü değiştirme

- **Portalı kullanma:** Azure portal kullanarak bağlantı türünü değiştirmek için, sanal ağ sayfasını açın **ve bağlantı türü ayarını kullanarak** bağlantı türünü değiştirin ve değişiklikleri kaydedin.

- **PowerShell kullanarak bağlantı türü ayarlarını değiştirme betiği:**

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Aşağıdaki PowerShell betiği, yönetilen bir örnek için bağlantı türünün olarak nasıl değiştirileceğini gösterir `Redirect` .

```powershell
Install-Module -Name Az
Import-Module Az.Accounts
Import-Module Az.Sql

Connect-AzAccount
# Get your SubscriptionId from the Get-AzSubscription command
Get-AzSubscription
# Use your SubscriptionId in place of {subscription-id} below
Select-AzSubscription -SubscriptionId {subscription-id}
# Replace {rg-name} with the resource group for your managed instance, and replace {mi-name} with the name of your managed instance
$mi = Get-AzSqlInstance -ResourceGroupName {rg-name} -Name {mi-name}
$mi = $mi | Set-AzSqlInstance -ProxyOverride "Redirect" -force
```

## <a name="next-steps"></a>Sonraki adımlar

- [Veritabanını SQL yönetilen örneğine geri yükleme](restore-sample-database-quickstart.md)
- [SQL yönetilen örneği üzerinde genel uç nokta yapılandırmayı](public-endpoint-configure.md) öğrenin
- [SQL yönetilen örnek bağlantı mimarisi](connectivity-architecture-overview.md) hakkında bilgi edinin
