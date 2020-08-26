---
title: Azure Stream Analytics çıkışı Power BI
description: Bu makalede, Azure Stream Analytics verilerin Power BI ' den nasıl çıktısının yapılacağı açıklanır.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 08/25/2020
ms.openlocfilehash: 0fc4e9a794429e7f1c1609fac287e67dabb3c878
ms.sourcegitcommit: 927dd0e3d44d48b413b446384214f4661f33db04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88875945"
---
# <a name="power-bi-output-from-azure-stream-analytics"></a>Azure Stream Analytics çıkışı Power BI

Analiz sonuçlarına yönelik zengin bir görselleştirme deneyimi sağlamak üzere bir Stream Analytics iş için çıkış olarak [Power BI](https://powerbi.microsoft.com/) kullanabilirsiniz. Bu özelliği işlemsel panolar, rapor oluşturma ve ölçüm temelli raporlama için kullanabilirsiniz.

Stream Analytics çıkış Power BI Şu anda Azure Çin 21Vianet ve Azure Almanya (T-Systems Uluslararası) bölgelerinde kullanılamıyor.

## <a name="ouput-configuration"></a>Ouput yapılandırması

Aşağıdaki tablo Power BI çıktlarınızı yapılandırmak için özellik adlarını ve açıklamalarını listelemektedir.

| Özellik adı | Açıklama |
| --- | --- |
| Çıktı diğer adı |Sorgu çıkışını bu Power BI çıktısına yönlendirmek için sorgularda kullanılan kolay bir ad sağlayın. |
| Grup çalışma alanı |Diğer Power BI kullanıcılarla veri paylaşmayı etkinleştirmek için Power BI hesabınızda grupları seçebilir veya bir gruba yazmak istemiyorsanız **çalışma alanım** ' ı seçebilirsiniz. Mevcut bir grubun güncelleştirilmesi için Power BI kimlik doğrulamasının yenilenmesi gerekir. |
| Veri kümesi adı |Power BI çıkışının kullanmasını istediğiniz bir veri kümesi adı belirtin. |
| Tablo adı |Power BI çıkışının veri kümesi altında bir tablo adı sağlayın. Şu anda, Stream Analytics işlerden alınan Power BI çıktısı bir veri kümesinde yalnızca bir tablo içerebilir. |
| Bağlantı yetkilendirme | Çıkış ayarlarınızı yapılandırmak için Power BI yetkilendirme yapmanız gerekir. Bu çıkış Power BI panonuza erişim izni verdiğinizde, Kullanıcı hesabı parolasını değiştirerek, iş çıktısını silerek veya Stream Analytics işini silerek erişimi iptal edebilirsiniz. | 

Power BI çıktısını ve panoyu yapılandırmaya yönelik bir anlatım için, [Azure Stream Analytics ve Power BI](stream-analytics-power-bi-dashboard.md) öğreticisine bakın.

> [!NOTE]
> Veri kümesini ve tabloyu Power BI panosunda açıkça oluşturmayın. Veri kümesi ve tablo, iş başlatıldığında otomatik olarak doldurulur ve iş, Power BI ' a çıktı olarak başlatılır. İş sorgusu herhangi bir sonuç oluşturmazsa, veri kümesi ve tablo oluşturulmaz. Power BI zaten bu Stream Analytics işinde belirtilen adla aynı ada sahip bir veri kümesi ve tablo varsa, varolan verilerin üzerine yazılır.
>

### <a name="create-a-schema"></a>Şema oluşturma

Azure Stream Analytics, zaten mevcut değilse Kullanıcı için bir Power BI veri kümesi ve tablo şeması oluşturur. Diğer tüm durumlarda, tablo yeni değerlerle güncellenir. Şu anda bir veri kümesi içinde yalnızca bir tablo bulunabilir. 

Power BI ilk kez ilk çıkar (FıFO) bekletme ilkesini kullanır. Veriler, 200.000 satır isabetlerine kadar bir tabloda toplanır.

### <a name="convert-a-data-type-from-stream-analytics-to-power-bi"></a>Stream Analytics bir veri türünü Power BI Dönüştür

Azure Stream Analytics, çıkış şeması değişirse veri modelini çalışma zamanında dinamik olarak güncelleştirir. Sütun adı değişiklikleri, sütun türü değişiklikleri ve sütun ekleme veya kaldırma işlemlerinin tümü izlenir.

Bu tablo, bir Power BI veri kümesi ve tablo yoksa [Stream Analytics veri türlerinden](https://docs.microsoft.com/stream-analytics-query/data-types-azure-stream-analytics) Power BI [varlık veri modeli (EDM) türlerine](https://docs.microsoft.com/dotnet/framework/data/adonet/entity-data-model)veri türü dönüştürmelerini içerir.

Stream Analytics 'den | Power BI için
-----|-----
bigint | Int64
nvarchar (max) | Dize
datetime | Tarih saat
float | Çift
Kayıt dizisi | Dize türü, sabit değer "IRecord" veya "IArray"

### <a name="update-the-schema"></a>Şemayı güncelleştirme

Stream Analytics, veri modeli şemasını çıkışdaki ilk olay kümesine göre algılar. Daha sonra, gerekirse, veri modeli şeması özgün şemaya sığmayan gelen olayları kapsayacak şekilde güncelleştirilir.

`SELECT *`Satırlarda dinamik şema güncelleştirmesini önlemek için sorgu kullanmaktan kaçının. Olası performans etkilerine ek olarak, sonuçlar için geçen sürenin açıklanmasına neden olabilir. Power BI panosunda gösterilmesi gereken tam alanları seçin. Ayrıca, veri değerleri seçili veri türüyle uyumlu olmalıdır.

Önceki/geçerli | Int64 | Dize | Tarih saat | Çift
-----------------|-------|--------|----------|-------
Int64 | Int64 | Dize | Dize | Çift
Çift | Çift | Dize | Dize | Çift
Dize | Dize | Dize | Dize | Dize 
Tarih saat | Dize | Dize |  Tarih saat | Dize

## <a name="output-batch-size"></a>Çıkış toplu iş boyutu

Çıkış toplu iş boyutu için [Power BI REST API sınırları](https://msdn.microsoft.com/library/dn950053.aspx)' na bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md)
* [Hızlı başlangıç: Azure CLı kullanarak Azure Stream Analytics işi oluşturma](quick-create-azure-cli.md)
* [Hızlı başlangıç: ARM şablonu kullanarak Azure Stream Analytics işi oluşturma](quick-create-azure-resource-manager.md)
* [Hızlı başlangıç: Azure PowerShell kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-powershell.md)
* [Hızlı başlangıç: Visual Studio 'Yu kullanarak Azure Stream Analytics işi oluşturma](stream-analytics-quick-create-vs.md)
* [Hızlı başlangıç: Visual Studio Code Azure Stream Analytics iş oluşturma](quick-create-vs-code.md)
