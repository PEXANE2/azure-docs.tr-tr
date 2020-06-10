---
title: Yönetim uç noktası IP adresini bul
titleSuffix: Azure SQL Managed Instance
description: Azure SQL yönetilen örnek yönetimi uç noktası genel IP adresini almayı ve yerleşik güvenlik duvarı korumasını doğrulamayı öğrenin
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, carlrab
ms.date: 12/04/2018
ms.openlocfilehash: f63167824a06a877456db4a18622710922e026ff
ms.sourcegitcommit: 5a8c8ac84c36859611158892422fc66395f808dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84659269"
---
# <a name="determine-the-management-endpoint-ip-address---azure-sql-managed-instance"></a>Yönetim uç noktası IP adresini belirleme-Azure SQL yönetilen örneği 
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Azure SQL yönetilen örneği sanal kümesi, Azure 'un yönetim işlemleri için kullandığı bir yönetim uç noktası içerir. Yönetim uç noktası, ağ düzeyinde yerleşik bir güvenlik duvarı ve uygulama düzeyinde karşılıklı sertifika doğrulaması ile korunur. Yönetim uç noktasının IP adresini belirleyebilirsiniz, ancak bu uç noktaya erişemezsiniz.

Yönetim IP adresini öğrenmek için, SQL yönetilen örnek FQDN 'niz üzerinde bir [DNS araması](/windows-server/administration/windows-commands/nslookup) yapın: `mi-name.zone_id.database.windows.net` . Bu, gibi bir DNS girişi döndürür `trx.region-a.worker.vnet.database.windows.net` . Daha sonra bu FQDN üzerinde ". VNET" kaldırılmış bir DNS araması yapabilirsiniz. Bu, yönetim IP adresini döndürür. 

Bu PowerShell kodu, \<MI FQDN\> SQL yönetilen ÖRNEĞININ DNS girdisiyle değiştirin: `mi-name.zone_id.database.windows.net`
  
``` powershell
  $MIFQDN = "<MI FQDN>"
  resolve-dnsname $MIFQDN | select -first 1  | %{ resolve-dnsname $_.NameHost.Replace(".vnet","")}
```

SQL yönetilen örneği ve bağlantısı hakkında daha fazla bilgi için bkz. [Azure SQL yönetilen örnek bağlantısı mimarisi](connectivity-architecture-overview.md).
