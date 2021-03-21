---
services: storage
author: normesta
ms.service: storage
ms.topic: include
ms.date: 09/29/2020
ms.author: normesta
ms.custom: include file
ms.openlocfilehash: 97b47b48a183951002d04cd58b08d5e2b9051eda
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96017703"
---
| Mechanism | Kapsam |Sınırlar | Desteklenen izin düzeyi |
|---|---|---|---|
| Azure RBAC | Depolama hesapları, kapsayıcılar. <br>Abonelikte veya kaynak grubu düzeyinde Azure rolü atamaları çapraz kaynağı. | abonelik içindeki 2000 Azure rol atamaları | Azure rolleri (yerleşik veya özel) |
| TEMEDI| Dizin, dosya |dosya başına ve Dizin başına 32 ACL girişi (etkin 28 ACL girişi). Erişim ve varsayılan ACL 'Lerin her biri kendi 32 ACL girdisi sınırına sahiptir. |ACL izni|
