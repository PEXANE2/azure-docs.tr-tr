---
title: Azure 'da desteklenen FHıR özellikleri-FHıR için Azure API
description: Bu makalede, fhır için Azure API 'de uygulanan FHıR belirtiminin hangi özellikleri açıklanmaktadır
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 02/07/2019
ms.author: cavoeg
ms.openlocfilehash: 3aea2322129c383a385168c54001464da5dae276
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95520093"
---
# <a name="features"></a>Özellikler

FHıR için Azure API 'SI, Azure için Microsoft FHıR sunucusunun tam olarak yönetilen bir dağıtımını sağlar. Sunucu, [Fhır](https://hl7.org/fhir) standardının bir uygulamasıdır. Bu belge, FHıR sunucusunun ana özelliklerini listeler.

## <a name="fhir-version"></a>FHıR sürümü

Desteklenen en son sürüm: `4.0.1`

Şu anda desteklenen önceki sürümler şunlardır: `3.0.2`

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
| geçmiş                        | Evet       | Evet       | Evet       |                                                     |
| oluşturmaya                         | Evet       | Evet       | Evet       | Her iki GÖNDERI/PUT desteği                               |
| oluştur (koşullu)           | Evet       | Evet       | Evet       | Sorun [#1382](https://github.com/microsoft/fhir-server/issues/1382) |
| search                         | Kısmi   | Kısmi   | Kısmi   | Aşağıya bakın                                           |
| zincirleme arama                 | Hayır        | Evet       | Hayır        |                                           |
| ters zincirleme arama         | Hayır        | Hayır        | Hayır        |                                            |
| özellikler                   | Evet       | Evet       | Evet       |                                                     |
| toplu iş                          | Evet       | Evet       | Evet       |                                                     |
| işlem                    | Hayır        | Evet       | Hayır        |                                                     |
| Sayfalamayı                         | Kısmi   | Kısmi   | Kısmi   | `self` ve `next` desteklenir                     |
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
|`:in` simgesinde          | Hayır        | Hayır        | Hayır        |         |
|`:below` simgesinde       | Hayır        | Hayır        | Hayır        |         |
|`:above` simgesinde       | Hayır        | Hayır        | Hayır        |         |
|`:not-in` simgesinde      | Hayır        | Hayır        | Hayır        |         |
|`:[type]` başvurunun  | Hayır        | Hayır        | Hayır        |         |
|`:below` kullanılmamışsa         | Evet       | Evet       | Evet       |         |
|`:not`                 | Hayır        | Hayır        | Hayır        |         |
|`:above` kullanılmamışsa         | Hayır        | Hayır        | Hayır        | Sorun [#158](https://github.com/Microsoft/fhir-server/issues/158) |

| Ortak arama parametresi | Desteklenen-PaaS | Desteklenen-OSS (SQL) | Desteklenen-OSS (Cosmos DB) | Yorum |
|-------------------------| ----------| ----------| ----------|---------|
| `_id`                   | Evet       | Evet       | Evet       |         |
| `_lastUpdated`          | Evet       | Evet       | Evet       |         |
| `_tag`                  | Evet       | Evet       | Evet       |         |
| `_profile`              | Evet       | Evet       | Evet       |         |
| `_security`             | Evet       | Evet       | Evet       |         |
| `_text`                 | Hayır        | Hayır        | Hayır        |         |
| `_content`              | Hayır        | Hayır        | Hayır        |         |
| `_list`                 | Evet       | Evet       | Evet       |         |
| `_has`                  | Hayır        | Hayır        | Hayır        |         |
| `_type`                 | Evet       | Evet       | Evet       |         |
| `_query`                | Hayır        | Hayır        | Hayır        |         |
| `_filter`               | Hayır        | Hayır        | Hayır        |         |

| Arama sonucu parametreleri | Desteklenen-PaaS | Desteklenen-OSS (SQL) | Desteklenen-OSS (Cosmos DB) | Yorum |
|-------------------------|-----------|-----------|-----------|---------|
| `_sort`                 | Kısmi        | Kısmi   | Kısmi        |   `_sort=_lastUpdated` desteklenir       |
| `_count`                | Evet       | Evet       | Evet       | `_count` 100 karakterle sınırlıdır. 100 'den yüksek olarak ayarlandıysa, yalnızca 100 döndürülür ve pakete bir uyarı döndürülür. |
| `_include`              | Evet       | Evet       | Evet       |Dahil edilen öğeler 100 ile sınırlıdır. PaaS ve OSS 'e dahil et Cosmos DB şunları içermez: yineleme desteği.|
| `_revinclude`           | Evet       | Evet       | Evet       | Dahil edilen öğeler 100 ile sınırlıdır. PaaS ve OSS 'e dahil et Cosmos DB şunları içermez: yineleme desteği.|
| `_summary`              | Kısmi   | Kısmi   | Kısmi   | `_summary=count` desteklenir |
| `_total`                | Kısmi   | Kısmi   | Kısmi   | _total = ve _total = doğru      |
| `_elements`             | Evet       | Evet       | Evet       |         |
| `_contained`            | Hayır        | Hayır        | Hayır        |         |
| `containedType`         | Hayır        | Hayır        | Hayır        |         |
| `_score`                | Hayır        | Hayır        | Hayır        |         |

## <a name="extended-operations"></a>Genişletilmiş Işlemler

Yeniden gerçekleştirilen API 'YI genişleten desteklenen tüm işlemler.

| Arama parametresi türü | Desteklenen-PaaS | Desteklenen-OSS (SQL) | Desteklenen-OSS (Cosmos DB) | Yorum |
|------------------------|-----------|-----------|-----------|---------|
| $export (tüm sistem) | Evet       | Evet       | Evet       |         |
| Hasta/$export        | Evet       | Evet       | Evet       |         |
| Grup/$export          | Evet       | Evet       | Evet       |         |

## <a name="persistence"></a>Kalıcılık

Microsoft FHıR sunucusunda takılabilir bir kalıcılık modülü vardır (bkz [`Microsoft.Health.Fhir.Core.Features.Persistence`](https://github.com/Microsoft/fhir-server/tree/master/src/Microsoft.Health.Fhir.Core/Features/Persistence) .).

Şu anda FHıR sunucusu açık kaynaklı kodu, [Azure Cosmos DB](../cosmos-db/index-overview.md) ve [SQL veritabanı](https://azure.microsoft.com/services/sql-database/)için bir uygulama içerir.

Cosmos DB, genel olarak dağıtılmış çok modelli (SQL API, MongoDB API 'SI vb.) veritabanıdır. Farklı [tutarlılık düzeylerini](../cosmos-db/consistency-levels.md)destekler. Varsayılan dağıtım şablonu, FHıR sunucusunu tutarlılık ile yapılandırır `Strong` , ancak istek üst bilgisi kullanılarak istek temeline göre bir istek için tutarlılık İlkesi değiştirilebilir (genellikle gevşek) `x-ms-consistency-level` .

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

FHıR sunucusu, Access Control için [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) kullanır. Özellikle, yapılandırma parametresi olarak ayarlanmışsa rol tabanlı erişim denetimi (RBAC) zorlanır `FhirServer:Security:Enabled` `true` ve `/metadata` Fhır sunucusuna yönelik tüm isteklerin (hariç) `Authorization` istek üst bilgisi olarak ayarlanmış olması gerekir `Bearer <TOKEN>` . Belirtecin, talepte tanımlanan bir veya daha fazla rol içermesi gerekir `roles` . Belirteç belirtilen kaynakta belirtilen eyleme izin veren bir rol içeriyorsa, bir isteğe izin verilir.

Şu anda, belirli bir rolün izin verilen eylemleri API üzerinde *Global olarak* uygulanır.

## <a name="service-limits"></a>Hizmet sınırlamaları

* [**Istek birimleri (ru)**](https://docs.microsoft.com/azure/cosmos-db/concepts-limits) -Azure API için Portal 'da fhır için en fazla 10.000 ru yapılandırabilirsiniz. En az 400 RUs veya 10 ru/GB olmak üzere, hangisi daha büyükse gereklidir. 10.000 ' den fazla ru 'ya ihtiyacınız varsa, bu artış için bir destek bileti koyabilirsiniz. Kullanılabilir en yüksek değer 1.000.000 ' dir.

* **Eş zamanlı bağlantılar** ve **örnekler** -gün içinde, kümedeki iki örnek üzerinde beş eş zamanlı bağlantınız vardır (Toplam 10 eşzamanlı istek için). Daha fazla eşzamanlı istek gerektiğini düşünüyorsanız, gereksinimlerinize ilişkin ayrıntılarla bir destek bileti açın.

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
