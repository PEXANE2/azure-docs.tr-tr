---
title: Azure SQL veritabanı yönetilen örneği yerleşik güvenlik duvarını bul | Microsoft Docs
description: Azure SQL veritabanı yönetilen örneği 'nde yerleşik güvenlik duvarı korumasını doğrulamayı öğrenin.
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
ms.openlocfilehash: c98b0fd5669140559b4840e157394c2e8c6086ae
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68567443"
---
# <a name="verifying-the-managed-instance-built-in-firewall"></a>Yönetilen örnek yerleşik güvenlik duvarı doğrulanıyor

Yönetilen örnek [zorunlu gelen güvenlik kuralları](sql-database-managed-instance-connectivity-architecture.md#mandatory-inbound-security-rules) , yönetim bağlantı noktalarını 9000, 9003, 1438, 1440, 1452 ' i ağ güvenlik grubundaki (NSG) yönetilen örneği koruyan **herhangi bir kaynaktan** açık olmasını gerektirir. Bu bağlantı noktaları NSG düzeyinde açık olsa da, yerleşik güvenlik duvarı tarafından ağ düzeyinde korunur.

## <a name="verify-firewall"></a>Güvenlik duvarını doğrulama

Bu bağlantı noktalarını doğrulamak için, bu bağlantı noktalarını sınamak üzere herhangi bir güvenlik tarayıcısı aracını kullanın. Aşağıdaki ekran görüntüsünde, bu araçlardan birinin nasıl kullanılacağı gösterilmektedir.

![Yerleşik güvenlik duvarı doğrulanıyor](./media/sql-database-managed-instance-management-endpoint/03_verify_firewall.png)

## <a name="next-steps"></a>Sonraki adımlar

Yönetilen örnekler ve bağlantı hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı yönetilen örnek bağlantı mimarisi](sql-database-managed-instance-connectivity-architecture.md).