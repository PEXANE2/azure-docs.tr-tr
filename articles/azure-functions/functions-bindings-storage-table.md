---
title: Azure Işlevleri için Azure Tablo depolama bağlamaları
description: Azure Işlevleri 'nde Azure Tablo depolama bağlamalarını nasıl kullanacağınızı anlayın.
author: craigshoemaker
ms.topic: reference
ms.date: 09/03/2018
ms.author: cshoe
ms.custom: devx-track-csharp, devx-track-python
ms.openlocfilehash: 4f2b890dc60cd50b5fcaefabe8d418268b738c20
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "92096734"
---
# <a name="azure-table-storage-bindings-for-azure-functions"></a>Azure Işlevleri için Azure Tablo depolama bağlamaları

Azure Işlevleri [, Tetikleyiciler ve bağlamalar](./functions-triggers-bindings.md)aracılığıyla [Azure depolama](../storage/index.yml) ile tümleşir. Tablo depolama ile tümleştirme, tablo depolama verilerini okuyan ve yazan işlevler oluşturmanızı sağlar.

| Eylem | Tür |
|---------|---------|
| Bir işlevde tablo depolama verilerini okuma | [Giriş bağlama](./functions-bindings-storage-table-input.md) |
| Bir işlevin tablo depolama verilerini yazmasına izin ver |[Çıkış bağlama](./functions-bindings-storage-table-output.md) |

## <a name="packages---functions-2x-and-higher"></a>Paketler-Işlevler 2. x ve üzeri

Tablo depolama bağlamaları [Microsoft. Azure. WebJobs. Extensions. Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage) NuGet paketi, sürüm 3. x içinde verilmiştir. Paketin kaynak kodu, [Azure-WebJobs-SDK](https://github.com/Azure/azure-webjobs-sdk/tree/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables) GitHub deposundadır.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="packages---functions-1x"></a>Paketler-Işlevler 1. x

Tablo depolama bağlamaları [Microsoft. Azure. WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs) NuGet paketi, sürüm 2. x ' de verilmiştir. Paketin kaynak kodu, [Azure-WebJobs-SDK](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.Storage/Table) GitHub deposundadır.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

[!INCLUDE [functions-storage-sdk-version](../../includes/functions-storage-sdk-version.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Bir işlev çalıştığında tablo depolama verilerini okuma](./functions-bindings-storage-table-input.md)
- [Bir işlevden tablo depolama verileri yazma](./functions-bindings-storage-table-output.md)
