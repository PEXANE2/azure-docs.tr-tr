---
title: Yönetim API'leri ve hizmet API'leri arasındaki farklar - Azure Toplu İş | Microsoft Dokümanlar
description: API'ler Azure Toplu İşlem hizmetinin farklı katmanlarında çalışır.
services: batch
author: LauraBrenner
manager: evansma
ms.service: batch
ms.topic: conceptual
ms.date: 02/26/2020
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: 181515c0f497af8ffadcb909c13e51a40bfbf3b0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78672754"
---
# <a name="service-level-and-management-level-apis"></a>Hizmet düzeyi ve yönetim düzeyi API'leri

Azure Toplu İşlem,biri hizmet düzeyi ve diğeri yönetim düzeyi için olmak üzere iki API kümesine sahiptir. Adlandırma genellikle benzerdir, ancak farklı sonuçlar döndürerler. Etkinlik günlükleri istiyorsanız, yönetim API'lerini kullanmanız gerekir. Hizmet düzeyi API'leri Azure Kaynak Yönetimi katmanını atlar ve günlüğe kaydedilmez.


Toplu iş yönetimi ve Toplu İşlem hizmetinin her ikiside havuz için API'ler bulunmaktadır. 
- Havuzu silmek için bu API doğrudan toplu iş hesabına hedeflenir:https://docs.microsoft.com/rest/api/batchservice/pool/delete 

- Havuzu silmek https://docs.microsoft.com/rest/api/batchmanagement/pool/delete için bu API management.azure.com katmanı hedeflenir.

