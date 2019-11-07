---
title: Azure SQL veritabanı yönetilen örnek yönetimi uç noktasını bul
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
ms.date: 12/04/2018
ms.openlocfilehash: 0cc3ca3a2b8130e52d8c7fc670c0ee7997e4aa47
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73688034"
---
# <a name="determine-the-management-endpoint-ip-address"></a>Yönetim uç noktası IP adresini belirleme

Azure SQL veritabanı yönetilen örneği sanal kümesi, Microsoft 'un yönetim işlemleri için kullandığı bir yönetim uç noktası içerir. Yönetim uç noktası, ağ düzeyinde yerleşik bir güvenlik duvarı ve uygulama düzeyinde karşılıklı sertifika doğrulaması ile korunur. Yönetim uç noktasının IP adresini belirleyebilirsiniz, ancak bu uç noktaya erişemezsiniz.

Yönetim IP adresini öğrenmek için, yönetilen örnek FQDN 'niz üzerinde bir DNS araması yapın: `mi-name.zone_id.database.windows.net`. Bu, `trx.region-a.worker.vnet.database.windows.net`gibi bir DNS girişi döndürür. Daha sonra bu FQDN üzerinde ". VNET" kaldırılmış bir DNS araması yapabilirsiniz. Bu, yönetim IP adresini döndürür. 

\<mı FQDN\> yönetilen örneğinizin DNS girdisiyle değiştirirseniz, bu PowerShell bunu sizin için bu şekilde yapacaktır: `mi-name.zone_id.database.windows.net`:
  
``` powershell
  $MIFQDN = "<MI FQDN>"
  resolve-dnsname $MIFQDN | select -first 1  | %{ resolve-dnsname $_.NameHost.Replace(".vnet","")}
```

Yönetilen örnekler ve bağlantı hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı yönetilen örnek bağlantı mimarisi](sql-database-managed-instance-connectivity-architecture.md).
