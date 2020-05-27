---
title: Yönetim API 'Leri ve hizmet API 'Leri arasındaki farklılıklar
description: API 'Ler Azure Batch hizmetin farklı katmanlarında çalışır.
ms.topic: conceptual
ms.date: 02/26/2020
ms.custom: seodec18
ms.openlocfilehash: 33b8c5980aba1090155d6b136c6707e928666abf
ms.sourcegitcommit: 95269d1eae0f95d42d9de410f86e8e7b4fbbb049
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83867610"
---
# <a name="service-level-and-management-level-apis"></a>Hizmet düzeyi ve Yönetim düzeyi API 'Leri

Azure Batch, biri hizmet düzeyi ve diğeri Yönetim düzeyi için olmak üzere iki API kümesine sahiptir. Adlandırma genellikle benzerdir ancak farklı sonuçlar döndürür. Etkinlik günlükleri istiyorsanız, yönetim API 'Lerini kullanmanız gerekir. Hizmet düzeyi API 'Leri, Azure Kaynak Yönetimi katmanını atlar ve günlüğe kaydedilmez.


Batch yönetimi ve Batch hizmetinin her ikisi de havuz için API 'Lere sahiptir. 
- Havuzu silmeye yönelik bu API, doğrudan Batch hesabına yöneliktir:https://docs.microsoft.com/rest/api/batchservice/pool/delete 

- Havuzu silmeye yönelik bu API https://docs.microsoft.com/rest/api/batchmanagement/pool/delete , Management.Azure.com katmanına yöneliktir.

