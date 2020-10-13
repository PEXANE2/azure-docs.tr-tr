---
title: dosya dahil etme
description: dosya dahil etme
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/30/2020
ms.author: dacoulte
ms.openlocfilehash: f3f706789e14cb20214bf17fd91f6ec1e503848f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91605942"
---
Azure Ilkesi için her nesne türü için en fazla sayı vardır. Tanımlar için bir _kapsam_ girişi, [yönetim grubunun](../articles/governance/management-groups/overview.md) veya aboneliğin anlamına gelir.
Atamalar ve muafiyetler için bir _kapsam_ girişi, [Yönetim grubu](../articles/governance/management-groups/overview.md), abonelik, kaynak grubu veya tek bir kaynak anlamına gelir.

| Konum | Ne? | Maksimum sayı |
|---|---|---|
| Kapsam | İlke tanımları | 500 |
| Kapsam | Girişim tanımları | 200 |
| Kiracı | Girişim tanımları | 2,500 |
| Kapsam | İlke veya girişim atamaları | 200 |
| Kapsam | Dışarıda | 1000 |
| İlke tanımı | Parametreler | 20 |
| Girişim tanımı | İlkeler | 1000 |
| Girişim tanımı | Parametreler | 100 |
| İlke veya girişim atamaları | Özel durumlar (notScopes) | 400 |
| İlke kuralı | İç içe Koşullular | 512 |
| Düzeltme görevi | Kaynaklar | 500 |
