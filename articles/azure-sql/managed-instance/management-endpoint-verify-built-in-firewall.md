---
title: Yerleşik güvenlik duvarında bağlantı noktası güvenliğini doğrulama
description: Azure SQL yönetilen örneği 'nde yerleşik güvenlik duvarı korumasını doğrulamayı öğrenin.
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
ms.openlocfilehash: 04a630e51a68fab8f6c60262fbd14fad2e4aef1c
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84045077"
---
# <a name="verify-the-azure-sql-managed-instance-built-in-firewall"></a>Azure SQL yönetilen örneği yerleşik güvenlik duvarını doğrulama
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Azure SQL yönetilen örnek [zorunlu gelen güvenlik kuralları](connectivity-architecture-overview.md#mandatory-inbound-security-rules) , yönetim bağlantı noktalarını 9000, 9003, 1438, 1440, 1452 ' i ağ güvenlik grubundaki (NSG) SQL yönetilen örneği koruyan **herhangi bir kaynaktan** açık olmasını gerektirir. Bu bağlantı noktaları NSG düzeyinde açık olsa da, yerleşik güvenlik duvarı tarafından ağ düzeyinde korunur.

## <a name="verify-firewall"></a>Güvenlik duvarını doğrulama

Bu bağlantı noktalarını doğrulamak için, bu bağlantı noktalarını sınamak üzere herhangi bir güvenlik tarayıcısı aracını kullanın. Aşağıdaki ekran görüntüsünde, bu araçlardan birinin nasıl kullanılacağı gösterilmektedir.

![Yerleşik güvenlik duvarı doğrulanıyor](./media/management-endpoint-verify-built-in-firewall/03_verify_firewall.png)

## <a name="next-steps"></a>Sonraki adımlar

SQL yönetilen örnekler ve bağlantı hakkında daha fazla bilgi için bkz. [Azure SQL yönetilen örnek bağlantısı mimarisi](connectivity-architecture-overview.md).
