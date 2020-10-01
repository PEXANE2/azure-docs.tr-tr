---
title: Yerleşik güvenlik duvarında bağlantı noktası güvenliğini doğrulama
description: Azure SQL yönetilen örneği 'nde yerleşik güvenlik duvarı korumasını doğrulamayı öğrenin.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: how-to
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein
ms.date: 12/04/2018
ms.openlocfilehash: 35d0053d8c45547eaa5e89e6da15fcaf0343d6c5
ms.sourcegitcommit: 4bebbf664e69361f13cfe83020b2e87ed4dc8fa2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2020
ms.locfileid: "91617307"
---
# <a name="verify-the-azure-sql-managed-instance-built-in-firewall"></a>Azure SQL Yönetilen Örneği yerleşik güvenlik duvarını doğrulama
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Azure SQL yönetilen örnek [zorunlu gelen güvenlik kuralları](connectivity-architecture-overview.md#mandatory-inbound-security-rules) , yönetim bağlantı noktalarını 9000, 9003, 1438, 1440 ve 1452 ' yi, ağ güvenlik grubundaki (NSG) SQL yönetilen örneğini koruyan **herhangi bir kaynaktan** açık olmasını gerektirir. Bu bağlantı noktaları NSG düzeyinde açık olsa da, yerleşik güvenlik duvarı tarafından ağ düzeyinde korunur.

## <a name="verify-firewall"></a>Güvenlik duvarını doğrulama

Bu bağlantı noktalarını doğrulamak için, bu bağlantı noktalarını sınamak üzere herhangi bir güvenlik tarayıcısı aracını kullanın. Aşağıdaki ekran görüntüsünde, bu araçlardan birinin nasıl kullanılacağı gösterilmektedir.

![Yerleşik güvenlik duvarı doğrulanıyor](./media/management-endpoint-verify-built-in-firewall/03_verify_firewall.png)

## <a name="next-steps"></a>Sonraki adımlar

SQL yönetilen örneği ve bağlantısı hakkında daha fazla bilgi için bkz. [Azure SQL yönetilen örnek bağlantısı mimarisi](connectivity-architecture-overview.md).
