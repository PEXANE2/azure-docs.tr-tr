---
title: Azure 'da desteklenen FHıR özellikleri-FHıR için Azure API
description: Bu makalede, fhır için Azure API 'de uygulanan FHıR belirtiminin hangi özellikleri açıklanmaktadır
services: healthcare-apis
author: caitlinv39
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 1/21/2021
ms.author: cavoeg
ms.openlocfilehash: 43372eb0a9f7c08f6957627950769c1941580bd8
ms.sourcegitcommit: 4d48a54d0a3f772c01171719a9b80ee9c41c0c5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2021
ms.locfileid: "98745876"
---
# <a name="features"></a>Özellikler

FHıR için Azure API 'SI, Azure için Microsoft FHıR sunucusunun tam olarak yönetilen bir dağıtımını sağlar. Sunucu, [Fhır](https://hl7.org/fhir) standardının bir uygulamasıdır. Bu belge, FHıR sunucusunun ana özelliklerini listeler.

## <a name="fhir-version"></a>FHıR sürümü

Desteklenen en son sürüm: `4.0.1`

Şu anda desteklenen önceki sürümler şunlardır: `3.0.2`

## <a name="rest-api"></a>REST API

| API                            | Desteklenen-PaaS | Desteklenen-OSS (SQL) | Desteklenen-OSS (Cosmos DB) | Yorum                                             |
|--------------------------------|-----------|-----------|-----------|-----------------------------------------------------|
| okuma                           | Yes       | Yes       | Yes       |                                                     |
| vread                          | Yes       | Yes       | Yes       |                                                     |
| update                         | Yes       | Yes       | Yes       |                                                     |
| iyimser kilitleme ile güncelleştirme | Yes       | Yes       | Yes       |                                                     |
| Güncelleştirme (koşullu)           | Yes       | Yes       | Yes       |                                                     |
| düzeltmesi                          | Hayır        | Hayır        | Hayır        |                                                     |
| silme                         | Yes       | Yes       | Yes       |                                                     |
| Sil (koşullu)           | Hayır        | Hayır        | Hayır        |                                                     |
| geçmiş                        | Yes       | Yes       | Yes       |                                                     |
| oluşturmaya                         | Yes       | Yes       | Yes       | Her iki GÖNDERI/PUT desteği                               |
| oluştur (koşullu)           | Yes       | Yes       | Yes       | Sorun [#1382](https://github.com/microsoft/fhir-server/issues/1382) |
| search                         | Kısmi   | Kısmi   | Kısmi   | Aşağıya bakın                                           |
| zincirleme arama                 | Hayır        | Yes       | Hayır        |                                           |
| ters zincirleme arama         | Hayır        | Hayır        | Hayır        |                                            |
| özellikler                   | Yes       | Yes       | Yes       |                                                     |
| toplu iş                          | Yes       | Yes       | Yes       |                                                     |
| işlem                    | Hayır        | Yes       | Hayır        |                                                     |
| Sayfalamayı                         | Kısmi   | Kısmi   | Kısmi   | `self` ve `next` desteklenir                     |
| aracıların                 | Hayır        | Hayır        | Hayır        |                                                     |

## <a name="search"></a>Arama

Tüm arama parametresi türleri desteklenir. 

| Arama parametresi türü | Desteklenen-PaaS | Desteklenen-OSS (SQL) | Desteklenen-OSS (Cosmos DB) | Yorum |
|-----------------------|-----------|-----------|-----------|---------|
| Sayı                | Yes       | Yes       | Yes       |         |
| Tarih/Tarih saat         | Yes       | Yes       | Evet       |         |
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
| `_profile`              | Kısmi   | Kısmi   | Kısmi   | Yalnızca STU3 sürümünde desteklenir, R4 'de destek yoktur |
| `_security`             | Yes       | Yes       | Yes       |         |
| `_text`                 | Hayır        | Hayır        | Hayır        |         |
| `_content`              | Hayır        | Hayır        | Hayır        |         |
| `_list`                 | Yes       | Yes       | Yes       |         |
| `_has`                  | Hayır        | Hayır        | Hayır        |         |
| `_type`                 | Yes       | Yes       | Yes       |         |
| `_query`                | Hayır        | Hayır        | Hayır        |         |
| `_filter`               | Hayır        | Hayır        | Hayır        |         |

| Arama sonucu parametreleri | Desteklenen-PaaS | Desteklenen-OSS (SQL) | Desteklenen-OSS (Cosmos DB) | Yorum |
|-------------------------|-----------|-----------|-----------|---------|
| `_sort`                 | Kısmi        | Kısmi   | Kısmi        |   `_sort=_lastUpdated` desteklenir       |
| `_count`                | Yes       | Yes       | Yes       | `_count` 100 karakterle sınırlıdır. 100 'den yüksek olarak ayarlandıysa, yalnızca 100 döndürülür ve pakete bir uyarı döndürülür. |
| `_include`              | Yes       | Yes       | Yes       |Dahil edilen öğeler 100 ile sınırlıdır. PaaS ve OSS 'e dahil et Cosmos DB şunları içermez: yineleme desteği.|
| `_revinclude`           | Yes       | Yes       | Yes       | Dahil edilen öğeler 100 ile sınırlıdır. PaaS ve OSS 'e dahil et Cosmos DB şunları içermez: yineleme desteği.|
| `_summary`              | Kısmi   | Kısmi   | Kısmi   | `_summary=count` desteklenir |
| `_total`                | Kısmi   | Kısmi   | Kısmi   | _total = ve _total = doğru      |
| `_elements`             | Yes       | Yes       | Yes       |         |
| `_contained`            | Hayır        | Hayır        | Hayır        |         |
| `containedType`         | Hayır        | Hayır        | Hayır        |         |
| `_score`                | Hayır        | Hayır        | Hayır        |         |

## <a name="extended-operations"></a>Genişletilmiş Işlemler

Yeniden gerçekleştirilen API 'YI genişleten desteklenen tüm işlemler.

| Arama parametresi türü | Desteklenen-PaaS | Desteklenen-OSS (SQL) | Desteklenen-OSS (Cosmos DB) | Yorum |
|------------------------|-----------|-----------|-----------|---------|
| $export (tüm sistem) | Yes       | Yes       | Yes       |         |
| Hasta/$export        | Yes       | Yes       | Yes       |         |
| Grup/$export          | Yes       | Yes       | Yes       |         |

## <a name="persistence"></a>Kalıcılığı

Microsoft FHıR sunucusunda takılabilir bir kalıcılık modülü vardır (bkz [`Microsoft.Health.Fhir.Core.Features.Persistence`](https://github.com/Microsoft/fhir-server/tree/master/src/Microsoft.Health.Fhir.Core/Features/Persistence) .).

Şu anda FHıR sunucusu açık kaynaklı kodu, [Azure Cosmos DB](../cosmos-db/index-overview.md) ve [SQL veritabanı](https://azure.microsoft.com/services/sql-database/)için bir uygulama içerir.

Cosmos DB, genel olarak dağıtılmış çok modelli (SQL API, MongoDB API 'SI vb.) veritabanıdır. Farklı [tutarlılık düzeylerini](../cosmos-db/consistency-levels.md)destekler. Varsayılan dağıtım şablonu, FHıR sunucusunu tutarlılık ile yapılandırır `Strong` , ancak istek üst bilgisi kullanılarak istek temeline göre bir istek için tutarlılık İlkesi değiştirilebilir (genellikle gevşek) `x-ms-consistency-level` .

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

FHıR sunucusu, Access Control için [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) kullanır. Özellikle, yapılandırma parametresi olarak ayarlanmışsa rol tabanlı erişim denetimi (RBAC) zorlanır `FhirServer:Security:Enabled` `true` ve `/metadata` Fhır sunucusuna yönelik tüm isteklerin (hariç) `Authorization` istek üst bilgisi olarak ayarlanmış olması gerekir `Bearer <TOKEN>` . Belirtecin, talepte tanımlanan bir veya daha fazla rol içermesi gerekir `roles` . Belirteç belirtilen kaynakta belirtilen eyleme izin veren bir rol içeriyorsa, bir isteğe izin verilir.

Şu anda, belirli bir rolün izin verilen eylemleri API üzerinde *Global olarak* uygulanır.

## <a name="service-limits"></a>Hizmet sınırlamaları

* [**Istek birimleri (ru)**](../cosmos-db/concepts-limits.md) -Azure API için Portal 'da fhır için en fazla 10.000 ru yapılandırabilirsiniz. En az 400 RUs veya 10 ru/GB olmak üzere, hangisi daha büyükse gereklidir. 10.000 ' den fazla ru 'ya ihtiyacınız varsa, bu artış için bir destek bileti koyabilirsiniz. Kullanılabilir en yüksek değer 1.000.000 ' dir.

* **Eşzamanlı bağlantılar** ve **örnekler** -varsayılan olarak, kümedeki iki örnek üzerinde beş eş zamanlı bağlantınız vardır (Toplam 10 eşzamanlı istek için). Daha fazla eşzamanlı istek gerektiğini düşünüyorsanız, gereksinimlerinize ilişkin ayrıntılarla bir destek bileti açın.

* **Paket boyutu** -her paket 500 öğe ile sınırlıdır.

* **Veri boyutu** -veri/belge her bırı 2 MB 'tan biraz daha az olmalıdır.

## <a name="performance-expectations"></a>Performans beklentileri

Sistemin performansı, RUs sayısına, eşzamanlı bağlantılara ve gerçekleştirdiğiniz işlem türüne (put, Post, vb.) bağlıdır. Aşağıda, yapılandırılmış RUs 'e göre beklediklerinize ilişkin bazı genel aralıklar verilmiştir. Genel olarak, performans, ru 'daki bir artış ile doğrusal şekilde ölçeklendirilir:

| RUs | Kaynak/sn |
|----------|---------------|
| 400      | 5-10          |
| 1.000    | 100-150       |
| 10,000   | 225-400       |
| 100.000  | 2500-4000   |

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, FHıR için Azure API 'sindeki desteklenen FHıR özellikleri hakkında bilgi aldınız. Ardından, FHıR için Azure API 'YI dağıtın.
 
>[!div class="nextstepaction"]
>[FHIR için Azure API'sini dağıtma](fhir-paas-portal-quickstart.md)