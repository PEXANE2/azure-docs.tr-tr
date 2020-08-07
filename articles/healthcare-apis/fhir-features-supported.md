---
title: Azure 'da desteklenen FHıR özellikleri-FHıR için Azure API
description: Bu makalede, fhır için Azure API 'de uygulanan FHıR belirtiminin hangi özellikleri açıklanmaktadır
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 02/07/2019
ms.author: matjazl
ms.openlocfilehash: 1cb3af32f1ad16218c82f91c3f28d4f4ab47e677
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87843513"
---
# <a name="features"></a>Özellikler

FHıR için Azure API 'SI, Azure için Microsoft FHıR sunucusunun tam olarak yönetilen bir dağıtımını sağlar. Sunucu, [Fhır](https://hl7.org/fhir) standardının bir uygulamasıdır. Bu belge, FHıR sunucusunun ana özelliklerini listeler.

## <a name="fhir-version"></a>FHıR sürümü

Desteklenen en son sürüm:`4.0.1`

Şu anda desteklenen önceki sürümler şunlardır:`3.0.2`

## <a name="rest-api"></a>REST API

| API                            | Desteklenen-PaaS | Desteklenen-OSS (SQL) | Desteklenen-OSS (Cosmos DB) | Yorum                                             |
|--------------------------------|-----------|-----------|-----------|-----------------------------------------------------|
| read                           | Evet       | Evet       | Evet       |                                                     |
| vread                          | Evet       | Evet       | Evet       |                                                     |
| update                         | Evet       | Evet       | Evet       |                                                     |
| iyimser kilitleme ile güncelleştirme | Evet       | Evet       | Evet       |                                                     |
| Güncelleştirme (koşullu)           | Evet       | Evet       | Evet       |                                                     |
| düzeltmesi                          | Hayır        | Hayır        | Hayır        |                                                     |
| delete                         | Evet       | Evet       | Evet       |                                                     |
| Sil (koşullu)           | Hayır        | Hayır        | Hayır        |                                                     |
| oluşturmaya                         | Evet       | Evet       | Evet       | Her iki GÖNDERI/PUT desteği                               |
| oluştur (koşullu)           | Evet       | Evet       | Evet       |                                                     |
| search                         | Kısmi   | Kısmi   | Kısmi   | Aşağıya bakın                                           |
| zincirleme arama                 | Hayır        | Evet       | Hayır        |                                           |
| ters zincirleme arama         | Hayır        | Hayır        | Hayır        |                                            |
| özellikler                   | Evet       | Evet       | Evet       |                                                     |
| toplu iş                          | Evet       | Evet       | Evet       |                                                     |
| işlem                    | Hayır        | Evet       | Hayır        |                                                     |
| geçmiş                        | Evet       | Evet       | Evet       |                                                     |
| Sayfalamayı                         | Kısmi   | Kısmi   | Kısmi   | `self`ve `next` desteklenir                     |
| aracıların                 | Hayır        | Hayır        | Hayır        |                                                     |

## <a name="search"></a>Arayın

Tüm arama parametresi türleri desteklenir. 

| Arama parametresi türü | Desteklenen-PaaS | Desteklenen-OSS (SQL) | Desteklenen-OSS (Cosmos DB) | Yorum |
|-----------------------|-----------|-----------|-----------|---------|
| Sayı                | Evet       | Evet       | Evet       |         |
| Tarih/Tarih saat         | Evet       | Evet       | Evet       |         |
| Dize                | Evet       | Evet       | Evet       |         |
| Belirteç                 | Evet       | Evet       | Evet       |         |
| Başvuru             | Evet       | Evet       | Evet       |         |
| Bileşik             | Evet       | Evet       | Evet       |         |
| Miktar              | Evet       | Evet       | Evet       |         |
| URI                   | Evet       | Evet       | Evet       |         |
| Spec               | Hayır        | Hayır        | Hayır        |         |


| Değiştiriciler             | Desteklenen-PaaS | Desteklenen-OSS (SQL) | Desteklenen-OSS (Cosmos DB) | Yorum |
|-----------------------|-----------|-----------|-----------|---------|
|`:missing`             | Evet       | Evet       | Evet       |         |
|`:exact`               | Evet       | Evet       | Evet       |         |
|`:contains`            | Evet       | Evet       | Evet       |         |
|`:text`                | Evet       | Evet       | Evet       |         |
|`:in`simgesinde          | Hayır        | Hayır        | Hayır        |         |
|`:below`simgesinde       | Hayır        | Hayır        | Hayır        |         |
|`:above`simgesinde       | Hayır        | Hayır        | Hayır        |         |
|`:not-in`simgesinde      | Hayır        | Hayır        | Hayır        |         |
|`:[type]`başvurunun  | Hayır        | Hayır        | Hayır        |         |
|`:below`kullanılmamışsa         | Evet       | Evet       | Evet       |         |
|`:not`                 | Hayır        | Hayır        | Hayır        |         |
|`:above`kullanılmamışsa         | Hayır        | Hayır        | Hayır        | Sorun [#158](https://github.com/Microsoft/fhir-server/issues/158) |

| Ortak arama parametresi | Desteklenen-PaaS | Desteklenen-OSS (SQL) | Desteklenen-OSS (Cosmos DB) | Yorum |
|-------------------------| ----------| ----------| ----------|---------|
| `_id`                   | Evet       | Evet       | Evet       |         |
| `_lastUpdated`          | Evet       | Evet       | Evet       |         |
| `_tag`                  | Evet       | Evet       | Evet       |         |
| `_profile`              | Evet       | Evet       | Evet       |         |
| `_security`             | Evet       | Evet       | Evet       |         |
| `_text`                 | Hayır        | Hayır        | Hayır        |         |
| `_content`              | Hayır        | Hayır        | Hayır        |         |
| `_list`                 | Hayır        | Evet       | Evet       |         |
| `_has`                  | Hayır        | Hayır        | Hayır        |         |
| `_type`                 | Evet       | Evet       | Evet       |         |
| `_query`                | Hayır        | Hayır        | Hayır        |         |

| İşlemleri ara       | Desteklenen-PaaS | Desteklenen-OSS (SQL) | Desteklenen-OSS (Cosmos DB) | Yorum |
|-------------------------|-----------|-----------|-----------|---------|
| `_filter`               | Hayır        | Hayır        | Hayır        |         |
| `_sort`                 | Hayır        | Hayır        | Hayır        |         |
| `_score`                | Hayır        | Hayır        | Hayır        |         |
| `_count`                | Evet       | Evet       | Evet       |         |
| `_summary`              | Kısmi   | Kısmi   | Kısmi   | `_summary=count`desteklenir |
| `_include`              | Hayır        | Evet       | Hayır        |         |
| `_revinclude`           | Hayır        | Hayır        | Hayır        |         |
| `_contained`            | Hayır        | Hayır        | Hayır        |         |
| `_elements`             | Hayır        | Hayır        | Hayır        |         |

## <a name="persistence"></a>Kalıcılık

Microsoft FHıR sunucusunda takılabilir bir kalıcılık modülü vardır (bkz [`Microsoft.Health.Fhir.Core.Features.Persistence`](https://github.com/Microsoft/fhir-server/tree/master/src/Microsoft.Health.Fhir.Core/Features/Persistence) .).

Şu anda FHıR sunucusu açık kaynaklı kodu, [Azure Cosmos DB](../cosmos-db/index-overview.md) ve [SQL veritabanı](https://azure.microsoft.com/services/sql-database/)için bir uygulama içerir.

Cosmos DB, genel olarak dağıtılmış çok modelli (SQL API, MongoDB API 'SI vb.) veritabanıdır. Farklı [tutarlılık düzeylerini](../cosmos-db/consistency-levels.md)destekler. Varsayılan dağıtım şablonu, FHıR sunucusunu tutarlılık ile yapılandırır `Strong` , ancak istek üst bilgisi kullanılarak istek temeline göre bir istek için tutarlılık İlkesi değiştirilebilir (genellikle gevşek) `x-ms-consistency-level` .

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

FHıR sunucusu, Access Control için [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) kullanır. Özellikle, rol tabanlı Access Control (RBAC), `FhirServer:Security:Enabled` yapılandırma parametresi olarak ayarlanmışsa `true` ve `/metadata` Fhır sunucusuna yönelik tüm isteklerin (hariç) `Authorization` istek üst bilgisi olarak ayarlanmış `Bearer <TOKEN>` olması halinde zorlanır. Belirtecin, talepte tanımlanan bir veya daha fazla rol içermesi gerekir `roles` . Belirteç belirtilen kaynakta belirtilen eyleme izin veren bir rol içeriyorsa, bir isteğe izin verilir.

Şu anda, belirli bir rolün izin verilen eylemleri API üzerinde *Global olarak* uygulanır.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, FHıR için Azure API 'sindeki desteklenen FHıR özellikleri hakkında bilgi aldınız. Ardından, FHıR için Azure API 'YI dağıtın.
 
>[!div class="nextstepaction"]
>[FHIR için Azure API'sini dağıtma](fhir-paas-portal-quickstart.md)
