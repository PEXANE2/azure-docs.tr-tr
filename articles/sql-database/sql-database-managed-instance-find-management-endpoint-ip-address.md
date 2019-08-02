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
ms.date: 12/04/2018
ms.openlocfilehash: a0d47c61bf84cfb22c767fd09b3feed74f55b7fc
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68567494"
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
