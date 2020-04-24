---
title: include dosyası
description: include dosyası
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/23/2020
ms.author: dacoulte
ms.openlocfilehash: 04e607296478520298a2febab61a7edac911a59c
ms.sourcegitcommit: f7d057377d2b1b8ee698579af151bcc0884b32b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82131556"
---
Azure Ilkesi için her nesne türü için en fazla sayı vardır. _Kapsam_ girişi, abonelik ya da [Yönetim grubu](../articles/governance/management-groups/overview.md)anlamına gelir.

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
| İlke kuralı | İç içe Koşullular | 512 |
| Düzeltme görevi | Kaynaklar | 500 |
