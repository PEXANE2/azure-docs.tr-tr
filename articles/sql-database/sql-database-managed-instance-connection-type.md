---
title: Yönetilen örnek bağlantı türleri
description: Yönetilen örnek bağlantı türleri hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: vanto
ms.date: 10/07/2019
ms.openlocfilehash: 46223d1701b930d93de7c49c1e216a41045dda16
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73819469"
---
# <a name="azure-sql-database-managed-instance-connection-types"></a>Azure SQL Veritabanı yönetilen örnek bağlantı türleri

Bu makalede, istemcilerin bağlantı türüne bağlı olarak Azure SQL Veritabanı'na nasıl bağlanılmayı başardığı açıklanmaktadır. Bağlantı türlerini değiştirmek için komut dosyası örnekleri aşağıda, varsayılan bağlantı ayarlarını değiştirmeye ilişkin hususların yanı sıra verilmiştir.

## <a name="connection-types"></a>Bağlantı türleri

Azure SQL Veritabanı yönetilen örnek aşağıdaki iki bağlantı türünü destekler:

- **Yönlendirme (önerilir):** İstemciler doğrudan veritabanını barındıran düğüme bağlantılar kurar. Yeniden yönlendirmeyi kullanarak bağlantıyı etkinleştirmek için, 1433 ve 11000-11999 bağlantı noktalarına erişime izin vermek için güvenlik duvarları ve Ağ Güvenlik Grupları (NSG) açmanız gerekir. Paketler doğrudan veritabanına gider ve bu nedenle Proxy üzerinden Yönlendirme'yi kullanarak gecikme ve iş verme performansı iyileştirmeleri vardır.
- **Proxy (varsayılan):** Bu modda, tüm bağlantılar bir proxy ağ geçidi bileşeni kullanıyor. Bağlantıyı etkinleştirmek için, özel ağlar için yalnızca 1433 bağlantı noktası ve ortak bağlantı için 3342 bağlantı noktasının açılması gerekir. Bu modun seçilmesi, iş yükünün doğasına bağlı olarak daha yüksek gecikme ve daha düşük iş yüküne neden olabilir. En düşük gecikme ve en yüksek iş için Proxy bağlantı ilkesi üzerinden Yeniden Yönlendirme bağlantı ilkesini şiddetle öneririz.

## <a name="redirect-connection-type"></a>Bağlantı türünü yeniden yönlendirme

Yeniden yönlendirme bağlantı türü, TCP oturumu SQL motoruna kurulduktan sonra istemci oturumunun sanal küme düğümünün hedef sanal IP'sini yük bakiyesinden elde ettiği anlamına gelir. Sonraki paketler ağ geçidini atlayarak doğrudan sanal küme düğümüne akar. Aşağıdaki diyagram bu trafik akışını göstermektedir.

![redirect.png](media/sql-database-managed-instance-connection-types/redirect.png)

> [!IMPORTANT]
> Yeniden yönlendirme bağlantı türü şu anda yalnızca özel bitiş noktası için çalışır. Bağlantı türü ayarı ne olursa olsun, ortak bitiş noktasından gelen bağlantılar bir proxy aracılığıyla olacaktır.

## <a name="proxy-connection-type"></a>Proxy bağlantı türü

Proxy bağlantı türü, TCP oturumunun ağ geçidi kullanılarak kurulduğu ve sonraki tüm paketlerin içinden aktığı anlamına gelir. Aşağıdaki diyagram bu trafik akışını göstermektedir.

![proxy.png](media/sql-database-managed-instance-connection-types/proxy.png)

## <a name="script-to-change-connection-type-settings-using-powershell"></a>PowerShell kullanarak bağlantı türü ayarlarını değiştirmek için komut dosyası

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Aşağıdaki PowerShell komut dosyası, yönetilen bir örneğin yeniden yönlendirmeiçin bağlantı türünü nasıl değiştireceğinigösterir.

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

- [Veritabanını yönetilen örneğe geri yükleme](sql-database-managed-instance-get-started-restore.md)
- Yönetilen örnekte genel bitiş noktasını nasıl [yapılandırılamayı](sql-database-managed-instance-public-endpoint-configure.md) öğrenin
- Yönetilen [örnek bağlantı mimarisi](sql-database-managed-instance-connectivity-architecture.md) hakkında bilgi edinin