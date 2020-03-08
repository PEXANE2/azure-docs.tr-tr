---
title: Yönetim API 'Leri ve hizmet API 'Leri arasındaki farklar-Azure Batch | Microsoft Docs
description: API 'Ler Azure Batch hizmetin farklı katmanlarında çalışır.
services: batch
author: LauraBrenner
manager: evansma
ms.service: batch
ms.topic: conceptual
ms.date: 02/26/2020
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: 181515c0f497af8ffadcb909c13e51a40bfbf3b0
ms.sourcegitcommit: bc792d0525d83f00d2329bea054ac45b2495315d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78672754"
---
# <a name="service-level-and-management-level-apis"></a>Hizmet düzeyi ve Yönetim düzeyi API 'Leri

Azure Batch, biri hizmet düzeyi ve diğeri Yönetim düzeyi için olmak üzere iki API kümesine sahiptir. Adlandırma genellikle benzerdir ancak farklı sonuçlar döndürür. Etkinlik günlükleri istiyorsanız, yönetim API 'Lerini kullanmanız gerekir. Hizmet düzeyi API 'Leri, Azure Kaynak Yönetimi katmanını atlar ve günlüğe kaydedilmez.


Batch yönetimi ve Batch hizmetinin her ikisi de havuz için API 'Lere sahiptir. 
- Havuzu silmeye yönelik bu API, doğrudan Batch hesabını hedefledi: https://docs.microsoft.com/rest/api/batchservice/pool/delete 

- https://docs.microsoft.com/rest/api/batchmanagement/pool/delete havuzunu silmeye yönelik bu API, management.azure.com katmanına yöneliktir.

