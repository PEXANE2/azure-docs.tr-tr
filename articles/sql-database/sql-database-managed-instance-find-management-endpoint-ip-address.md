---
title: Azure SQL veritabanı yönetilen örnek yönetimi uç noktasını bul | Microsoft Docs
description: Azure SQL veritabanı yönetilen örnek yönetimi uç noktası genel IP adresini almayı ve yerleşik güvenlik duvarı korumasını doğrulamayı öğrenin
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, carlrab
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: c5304c62b29d842f9beeadb34eba1cb53048d179
ms.sourcegitcommit: af58483a9c574a10edc546f2737939a93af87b73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68302286"
---
# <a name="determine-the-management-endpoint-ip-address"></a>Yönetim uç noktası IP adresini belirleme

Azure SQL veritabanı yönetilen örneği sanal kümesi, Microsoft 'un yönetim işlemleri için kullandığı bir yönetim uç noktası içerir. Yönetim uç noktası, ağ düzeyinde yerleşik bir güvenlik duvarı ve uygulama düzeyinde karşılıklı sertifika doğrulaması ile korunur. Yönetim uç noktasının IP adresini belirleyebilirsiniz, ancak bu uç noktaya erişemezsiniz.

Yönetim IP adresini öğrenmek için, yönetilen örnek FQDN 'niz üzerinde bir DNS araması yapın: `mi-name.zone_id.database.windows.net`. Bu, gibi `trx.region-a.worker.vnet.database.windows.net`bir DNS girişi döndürür. Daha sonra bu FQDN üzerinde ". VNET" kaldırılmış bir DNS araması yapabilirsiniz. Bu, yönetim IP adresini döndürür. 

\<Mı `mi-name.zone_id.database.windows.net`FQDN\> 'yi yönetilen örneğinizin DNS girdisiyle değiştirirseniz, bu PowerShell bunu sizin için tek yapmanız gerekir:
  
``` powershell
  $MIFQDN = "<MI FQDN>"
  resolve-dnsname $MIFQDN | select -first 1  | %{ resolve-dnsname $_.NameHost.Replace(".vnet","")}
```

Yönetilen örnekler ve bağlantı hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı yönetilen örnek bağlantı mimarisi](sql-database-managed-instance-connectivity-architecture.md).
