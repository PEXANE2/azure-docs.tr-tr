---
title: Azure Stream Analytics çıkışı Azure Cosmos DB
description: Bu makalede, Azure Stream Analytics verilerin Azure Cosmos DB ' den nasıl çıktısının yapılacağı açıklanır.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 08/25/2020
ms.openlocfilehash: e322135cfdb7aaff331367e84c603e8344436528
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90906249"
---
# <a name="azure-cosmos-db-output-from-azure-stream-analytics"></a>Azure Stream Analytics çıkışı Azure Cosmos DB

[Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) , dünya genelinde sınırsız esnek ölçeklendirme, zengin sorgu ve şema belirsiz veri modelleri üzerinden otomatik dizin oluşturma olanağı sunan, küresel olarak dağıtılmış bir veritabanı hizmetidir. Stream Analytics için Azure Cosmos DB kapsayıcı seçenekleri hakkında bilgi edinmek için, [çıkış olarak Azure Cosmos DB Stream Analytics](stream-analytics-documentdb-output.md) bakın.

Stream Analytics çıkış Azure Cosmos DB Şu anda Azure Çin 21Vianet ve Azure Almanya (T-Systems Uluslararası) bölgelerinde kullanılamıyor.

> [!Note]
> Azure Stream Analytics yalnızca SQL API 'sini kullanarak Azure Cosmos DB bağlantısını destekler.
> Diğer Azure Cosmos DB API 'Leri henüz desteklenmiyor. Diğer API 'lerle oluşturulan Azure Cosmos DB hesaplara Azure Stream Analytics işaret ediyorsanız, veriler düzgün şekilde depolanmayabilir.

Aşağıdaki tabloda Azure Cosmos DB çıktısı oluşturma özellikleri açıklanmaktadır.

| Özellik adı | Açıklama |
| --- | --- |
| Çıktı diğer adı | Stream Analytics sorgunuzda bu çıktıyı ifade eden bir diğer ad. |
| Havuz | Azure Cosmos DB. |
| İçeri aktarma seçeneği | **Aboneliğinizden Cosmos DB seçin** ya da **Cosmos DB ayarları el ile sağlayın**.
| Hesap Kimliği | Azure Cosmos DB hesabının adı veya uç nokta URI 'SI. |
| Hesap anahtarı | Azure Cosmos DB hesabının paylaşılan erişim anahtarı. |
| Veritabanı | Azure Cosmos DB veritabanı adı. |
| Kapsayıcı adı | Cosmos DB olması gereken, kullanılacak kapsayıcı adı. Örnek:  <br /><ul><li> _MyContainer_: "myContainer" adlı bir kapsayıcı var olmalıdır.</li>|
| Belge KIMLIĞI |İsteğe bağlı. Ekleme veya güncelleştirme işlemlerinin dayandığı birincil anahtarı belirtmek için kullanılan çıkış olaylarında alanın adı.

## <a name="partitioning"></a>Bölümleme

Bölüm anahtarı, sorgudaki bölüm BY yan tümcesine dayalıdır. Çıktı yazıcılarının sayısı, [tam paralelleştirilmiş sorgular](stream-analytics-scale-jobs.md)için giriş bölümlemesini izler. Stream Analytics Cosmos DB çıkış bölümü anahtarını bir dizeye dönüştürür. Örneğin, bigint türünde 1 değeri olan bir bölüm anahtarınız varsa, bu dize tür dizesinin "1" değerine dönüştürülür.

## <a name="output-batch-size"></a>Çıkış toplu iş boyutu

En büyük ileti boyutu için bkz. [Azure Cosmos DB sınırları](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-cosmos-db-limits). Toplu iş boyutu ve yazma sıklığı Azure Cosmos DB yanıtlara göre dinamik olarak ayarlanır. Stream Analytics için önceden tanımlanmış bir sınırlama yoktur.

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md)
* [Hızlı başlangıç: Azure CLı kullanarak Azure Stream Analytics işi oluşturma](quick-create-azure-cli.md)
* [Hızlı başlangıç: ARM şablonu kullanarak Azure Stream Analytics işi oluşturma](quick-create-azure-resource-manager.md)
* [Hızlı başlangıç: Azure PowerShell kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-powershell.md)
* [Hızlı başlangıç: Visual Studio 'Yu kullanarak Azure Stream Analytics işi oluşturma](stream-analytics-quick-create-vs.md)
* [Hızlı başlangıç: Visual Studio Code Azure Stream Analytics iş oluşturma](quick-create-visual-studio-code.md)
