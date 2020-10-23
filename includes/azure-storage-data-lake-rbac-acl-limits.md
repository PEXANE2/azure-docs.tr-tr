---
services: storage
author: normesta
ms.service: storage
ms.topic: include
ms.date: 09/29/2020
ms.author: normesta
ms.custom: include file
ms.openlocfilehash: 8b793a28c31df185555b9a548df52e4beeccf305
ms.sourcegitcommit: 33368ca1684106cb0e215e3280b828b54f7e73e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92131641"
---
| Mechanism | Kapsam |Sınırlar | Desteklenen izin düzeyi |
|---|---|---|---|
| RBAC | Depolama hesapları, kapsayıcılar. <br>Abonelikte veya kaynak grubu düzeyinde çapraz kaynak RBAC rol atamaları. | bir abonelikte 2000 RBAC rol ataması | RBAC rolleri (yerleşik veya özel) |
| TEMEDI| Dizin, dosya |dosya başına ve Dizin başına 32 ACL girişi (etkin 28 ACL girişi). Erişim ve varsayılan ACL 'Lerin her biri kendi 32 ACL girdisi sınırına sahiptir. |ACL izni|
