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
ms.openlocfilehash: 46568bf3969d050fd964c85278debd9d599db266
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88796570"
---
# <a name="features"></a>Özellikler

FHıR için Azure API 'SI, Azure için Microsoft FHıR sunucusunun tam olarak yönetilen bir dağıtımını sağlar. Sunucu, [Fhır](https://hl7.org/fhir) standardının bir uygulamasıdır. Bu belge, FHıR sunucusunun ana özelliklerini listeler.

## <a name="fhir-version"></a>FHıR sürümü

Desteklenen en son sürüm: `4.0.1`

Şu anda desteklenen önceki sürümler şunlardır: `3.0.2`

## <a name="rest-api"></a>REST API

| API                            | Desteklenen-PaaS | Desteklenen-OSS (SQL) | Desteklenen-OSS (Cosmos DB) | Yorum                                             |
|--------------------------------|-----------|-----------|-----------|-----------------------------------------------------|
| read                           | Yes       | Yes       | Yes       |                                                     |
| vread                          | Yes       | Yes       | Yes       |                                                     |
| update                         | Yes       | Yes       | Yes       |                                                     |
| iyimser kilitleme ile güncelleştirme | Yes       | Yes       | Yes       |                                                     |
| Güncelleştirme (koşullu)           | Yes       | Yes       | Yes       |                                                     |
| düzeltmesi                          | Hayır        | Hayır        | Hayır        |                                                     |
| delete                         | Yes       | Yes       | Yes       |                                                     |
| Sil (koşullu)           | Hayır        | Hayır        | Hayır        |                                                     |
| oluşturmaya                         | Yes       | Yes       | Yes       | Her iki GÖNDERI/PUT desteği                               |
| oluştur (koşullu)           | Yes       | Yes       | Yes       |                                                     |
| search                         | Kısmi   | Kısmi   | Kısmi   | Aşağıya bakın                                           |
| zincirleme arama                 | Hayır        | Yes       | Hayır        |                                           |
| ters zincirleme arama         | Hayır        | Hayır        | Hayır        |                                            |
| özellikler                   | Yes       | Yes       | Yes       |                                                     |
| toplu iş                          | Yes       | Yes       | Yes       |                                                     |
| işlem                    | Hayır        | Yes       | Hayır        |                                                     |
| geçmiş                        | Yes       | Yes       | Yes       |                                                     |
| Sayfalamayı                         | Kısmi   | Kısmi   | Kısmi   | `self` ve `next` desteklenir                     |
| aracıların                 | Hayır        | Hayır        | Hayır        |                                                     |

## <a name="search"></a>Ara

Tüm arama parametresi türleri desteklenir. 

| Arama parametresi türü | Desteklenen-PaaS | Desteklenen-OSS (SQL) | Desteklenen-OSS (Cosmos DB) | Yorum |
|-----------------------|-----------|-----------|-----------|---------|
| Sayı                | Yes       | Yes       | Yes       |         |
| Tarih/Tarih saat         | Yes       | Yes       | Yes       |         |
| Dize                | Yes       | Yes       | Yes       |         |
| Belirteç                 | Yes       | Yes       | Yes       |         |
| Başvuru             | Yes       | Yes       | Yes       |         |
| Bileşik             | Yes       | Yes       | Yes       |         |
| Miktar              | Yes       | Yes       | Yes       |         |
| URI                   | Yes       | Yes       | Yes       |         |
| Spec               | Hayır        | Hayır        | Hayır        |         |


| Değiştiriciler             | Desteklenen-PaaS | Desteklenen-OSS (SQL) | Desteklenen-OSS (Cosmos DB) | Yorum |
|-----------------------|-----------|-----------|-----------|---------|
|`:missing`             | Yes       | Yes       | Yes       |         |
|`:exact`               | Yes       | Yes       | Yes       |         |
|`:contains`            | Yes       | Yes       | Yes       |         |
|`:text`                | Yes       | Yes       | Yes       |         |
|`:in` simgesinde          | Hayır        | Hayır        | Hayır        |         |
|`:below` simgesinde       | Hayır        | Hayır        | Hayır        |         |
|`:above` simgesinde       | Hayır        | Hayır        | Hayır        |         |
|`:not-in` simgesinde      | Hayır        | Hayır        | Hayır        |         |
|`:[type]` başvurunun  | Hayır        | Hayır        | Hayır        |         |
|`:below` kullanılmamışsa         | Yes       | Yes       | Yes       |         |
|`:not`                 | Hayır        | Hayır        | Hayır        |         |
|`:above` kullanılmamışsa         | Hayır        | Hayır        | Hayır        | Sorun [#158](https://github.com/Microsoft/fhir-server/issues/158) |

| Ortak arama parametresi | Desteklenen-PaaS | Desteklenen-OSS (SQL) | Desteklenen-OSS (Cosmos DB) | Yorum |
|-------------------------| ----------| ----------| ----------|---------|
| `_id`                   | Yes       | Yes       | Yes       |         |
| `_lastUpdated`          | Yes       | Yes       | Yes       |         |
| `_tag`                  | Yes       | Yes       | Yes       |         |
| `_profile`              | Yes       | Yes       | Yes       |         |
| `_security`             | Yes       | Yes       | Yes       |         |
| `_text`                 | Hayır        | Hayır        | Hayır        |         |
| `_content`              | Hayır        | Hayır        | Hayır        |         |
| `_list`                 | Yes       | Yes       | Yes       |         |
| `_has`                  | Hayır        | Hayır        | Hayır        |         |
| `_type`                 | Yes       | Yes       | Yes       |         |
| `_query`                | Hayır        | Hayır        | Hayır        |         |

| İşlemleri ara       | Desteklenen-PaaS | Desteklenen-OSS (SQL) | Desteklenen-OSS (Cosmos DB) | Yorum |
|-------------------------|-----------|-----------|-----------|---------|
| `_filter`               | Hayır        | Hayır        | Hayır        |         |
| `_sort`                 | Hayır        | Hayır        | Hayır        |         |
| `_score`                | Hayır        | Hayır        | Hayır        |         |
| `_count`                | Yes       | Yes       | Yes       |         |
| `_summary`              | Kısmi   | Kısmi   | Kısmi   | `_summary=count` desteklenir |
| `_include`              | Hayır        | Yes       | Hayır        |         |
| `_revinclude`           | Hayır        | Yes       | Hayır        | Dahil edilen öğeler 100 ile sınırlıdır. |
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
