---
title: Sorgu dili
titleSuffix: Azure Digital Twins
description: Azure Digital TWINS sorgu dilinin temellerini anlayın.
author: baanders
ms.author: baanders
ms.date: 3/26/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: 29e1fa603600e246031f2a86aae3b0876b4910ba
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87562489"
---
# <a name="about-the-query-language-for-azure-digital-twins"></a>Azure dijital TWINS için sorgu dili hakkında

Azure dijital TWINS merkezinin, **dijital TWINS** ve **ilişkilerden**oluşturulan [**ikizi Graf**](concepts-twins-graph.md)olduğunu unutmayın. Bu grafik, içerdiği dijital TWINS ve ilişkiler hakkında bilgi almak için sorgulanabilir. Bu sorgular, **Azure dijital TWINS sorgu dili**olarak adlandırılan özel bir SQL benzeri sorgu dilinde yazılır.

Bir istemci uygulamasından hizmete bir sorgu göndermek için Azure Digital TWINS [**sorgu API**](https://docs.microsoft.com/dotnet/api/azure.digitaltwins.core.digitaltwinsclient.query?view=azure-dotnet-preview)'sini kullanırsınız. Bu, geliştiricilerin ikizi grafiğindeki dijital TWINS kümelerini ve Azure Digital TWINS senaryosuyla ilgili diğer bilgileri bulmasını ve filtre uygulamasını sağlar.

[!INCLUDE [digital-twins-query-operations.md](../../includes/digital-twins-query-operations.md)]

## <a name="next-steps"></a>Sonraki adımlar

Sorguları yazmayı ve bkz. [*nasıl yapılır: ikizi grafiğini sorgulama*](how-to-query-graph.md)hakkında istemci kodu örnekleri.
