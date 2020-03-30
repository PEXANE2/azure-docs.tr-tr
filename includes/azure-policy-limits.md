---
title: include dosyası
description: include dosyası
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 03/25/2020
ms.author: dacoulte
ms.openlocfilehash: 19b43c857058b3af50db3bc1b42ec1ae558ee884
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80272707"
---
Azure İlkesi için her nesne türü için en fazla sayım vardır. _Kapsam'ın_ girişi, abonelik veya [yönetim grubu](../articles/governance/management-groups/overview.md)anlamına gelir.

| Konum | Nesne | Maksimum sayı |
|---|---|---|
| Kapsam | İlke tanımları | 500 |
| Kapsam | Girişim tanımları | 100 |
| Kiracı | Girişim tanımları | 1000 |
| Kapsam | İlke veya girişim atamaları | 100 |
| İlke tanımı | Parametreler | 20 |
| Girişim tanımı | İlkeler | 100 |
| Girişim tanımı | Parametreler | 100 |
| İlke veya girişim atamaları | Özel durumlar (notScopes) | 400 |
| İlke kuralı | İç içe koşullu | 512 |
| Düzeltme görevi | Kaynaklar | 1000 |
