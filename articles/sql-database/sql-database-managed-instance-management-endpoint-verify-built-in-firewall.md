---
title: Yönetilen örneği yerleşik güvenlik duvarını keşfedin
description: Azure SQL Veritabanı Yönetilen Örneği'nde yerleşik güvenlik duvarı korumasını nasıl doğrulayaylıktunuzu öğrenin.
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
ms.openlocfilehash: 555ef56aafa37a1e1d384f945b04f9237adc5f7d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73821811"
---
# <a name="verifying-the-managed-instance-built-in-firewall"></a>Yönetilen Örnek yerleşik güvenlik duvarını doğrulama

Yönetilen Örnek [zorunlu gelen güvenlik kuralları,](sql-database-managed-instance-connectivity-architecture.md#mandatory-inbound-security-rules) yönetim bağlantı noktalarının 9000, 9003, 1438, 1440, 1452'nin Yönetilen Örneği koruyan Ağ Güvenlik Grubu'ndaki (NSG) herhangi bir **kaynaktan** açık olmasını gerektirir. Bu bağlantı noktaları NSG düzeyinde açık olsa da, yerleşik güvenlik duvarı tarafından ağ düzeyinde korunur.

## <a name="verify-firewall"></a>Güvenlik duvarını doğrulay

Bu bağlantı noktalarını doğrulamak için, bu bağlantı noktalarını sınamak için herhangi bir güvenlik tarayıcı aracı kullanın. Aşağıdaki ekran görüntüsü, bu araçlardan birinin nasıl kullanılacağını gösterir.

![Yerleşik güvenlik duvarLarını doğrulama](./media/sql-database-managed-instance-management-endpoint/03_verify_firewall.png)

## <a name="next-steps"></a>Sonraki adımlar

Yönetilen Örnekler ve bağlantı hakkında daha fazla bilgi için Azure [SQL Veritabanı Yönetilen Örnek Bağlantı Mimarisi'ne](sql-database-managed-instance-connectivity-architecture.md)bakın.