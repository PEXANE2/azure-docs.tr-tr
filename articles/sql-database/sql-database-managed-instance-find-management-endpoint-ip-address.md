---
title: Yönetilen örnek yönetimi bitiş noktasını keşfedin
description: Azure SQL Veritabanı Yönetilen Örnek yönetimi bitiş noktası genel IP adresini nasıl alacağınızı ve yerleşik güvenlik duvarı korumasını nasıl doğrulayaylıktunuzu öğrenin
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, carlrab
ms.date: 12/04/2018
ms.openlocfilehash: 03cd89084c2bae3339311f2f684a0d5e7bac1f68
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73825711"
---
# <a name="determine-the-management-endpoint-ip-address"></a>Yönetim bitiş noktası IP adresini belirleme

Azure SQL Veritabanı Yönetilen Örnek sanal küme, Microsoft'un yönetim işlemleri için kullandığı bir yönetim bitiş noktası içerir. Yönetim bitiş noktası, ağ düzeyinde yerleşik bir güvenlik duvarı ve uygulama düzeyinde karşılıklı sertifika doğrulama ile korunur. Yönetim bitiş noktasının IP adresini belirleyebilirsiniz, ancak bu bitiş noktasına erişemezsiniz.

Yönetim IP adresini belirlemek için, yönetilen örneğiniz FQDN üzerinde `mi-name.zone_id.database.windows.net`bir DNS araması yapın: . Bu, dns girişini döndürecek. `trx.region-a.worker.vnet.database.windows.net` Daha sonra ".vnet" kaldırıldı ile bu FQDN bir DNS arama yapabilirsiniz. Bu, yönetimin IP adresini döndürecektir. 

Mi FQDN'yi \<\> yönetilen örneğinizin DNS girişiyle değiştirirseniz bu PowerShell `mi-name.zone_id.database.windows.net`her şeyi sizin için yapar: :
  
``` powershell
  $MIFQDN = "<MI FQDN>"
  resolve-dnsname $MIFQDN | select -first 1  | %{ resolve-dnsname $_.NameHost.Replace(".vnet","")}
```

Yönetilen Örnekler ve bağlantı hakkında daha fazla bilgi için Azure [SQL Veritabanı Yönetilen Örnek Bağlantı Mimarisi'ne](sql-database-managed-instance-connectivity-architecture.md)bakın.
