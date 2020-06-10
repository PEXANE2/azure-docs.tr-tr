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
ms.openlocfilehash: 561aba31bfd1cc37173f3ee41625ba3687f5e657
ms.sourcegitcommit: 5a8c8ac84c36859611158892422fc66395f808dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84660905"
---
# <a name="verify-the-azure-sql-managed-instance-built-in-firewall"></a>Azure SQL yönetilen örneği yerleşik güvenlik duvarını doğrulama
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Azure SQL yönetilen örnek [zorunlu gelen güvenlik kuralları](connectivity-architecture-overview.md#mandatory-inbound-security-rules) , yönetim bağlantı noktalarını 9000, 9003, 1438, 1440 ve 1452 ' yi, ağ güvenlik grubundaki (NSG) SQL yönetilen örneğini koruyan **herhangi bir kaynaktan** açık olmasını gerektirir. Bu bağlantı noktaları NSG düzeyinde açık olsa da, yerleşik güvenlik duvarı tarafından ağ düzeyinde korunur.

## <a name="verify-firewall"></a>Güvenlik duvarını doğrulama

Bu bağlantı noktalarını doğrulamak için, bu bağlantı noktalarını sınamak üzere herhangi bir güvenlik tarayıcısı aracını kullanın. Aşağıdaki ekran görüntüsünde, bu araçlardan birinin nasıl kullanılacağı gösterilmektedir.

![Yerleşik güvenlik duvarı doğrulanıyor](./media/management-endpoint-verify-built-in-firewall/03_verify_firewall.png)

## <a name="next-steps"></a>Sonraki adımlar

SQL yönetilen örneği ve bağlantısı hakkında daha fazla bilgi için bkz. [Azure SQL yönetilen örnek bağlantısı mimarisi](connectivity-architecture-overview.md).
