---
title: Azure Faturalandırma kurumsal API 'Leri | Microsoft Docs
description: Kurumsal Azure müşterilerinin tüketim verilerini programlı bir şekilde çekmesini sağlayan raporlama API 'Leri hakkında bilgi edinin.
services: ''
documentationcenter: ''
author: mumami
manager: mumami
editor: ''
tags: billing
ms.assetid: 3e817b43-0696-400c-a02e-47b7817f9b77
ms.service: billing
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 04/25/2017
ms.author: banders
ms.openlocfilehash: f706ad86493981d5b38248ec209a7c8b936f6817
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68443221"
---
# <a name="overview-of-reporting-apis-for-enterprise-customers"></a>Kurumsal müşterilere yönelik Raporlama API 'Lerine genel bakış
Raporlama API 'Leri, kurumsal Azure müşterilerinin tüketim ve faturalandırma verilerini tercih edilen veri analizi araçlarına programlı bir şekilde çekmesini sağlar. Kurumsal müşteriler Azure ile bir [Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) imzalayan parasal taahhütler elde edebilir ve Azure kaynakları için özel fiyatlandırmaya erişim sağlar.

## <a name="enabling-data-access-to-the-api"></a>API 'ye veri erişimini etkinleştirme
* API anahtarını **oluşturun veya alın** -Enterprise Portal 'da oturum açın ve API anahtarı oluşturmak veya almak Için API erişim anahtarı > > Raporlar ' a gidin.
* **API 'de anahtarları geçirme** -API anahtarı, kimlik doğrulama ve yetkilendirme için her çağrı için geçirilmelidir. Aşağıdaki özelliğin HTTP üstbilgileri olması gerekir

|İstek üst bilgisi anahtarı | Değer|
|-|-|
|Authorization| Değeri şu biçimde belirtin: **taşıyıcı {API_KEY}** <br/> Örnek: taşıyıcı eyr....09| 

## <a name="consumption-apis"></a>Tüketim API 'Leri
Aşağıda açıklanan API 'lerin, API 'nin Easy iç denetim 'i etkinleştirmesi ve [oto Rest](https://github.com/Azure/AutoRest) veya [Swagger CodeGen](https://swagger.io/swagger-codegen/)kullanarak istemci SDK 'ları oluşturma olanağı sağlaması gereken API ['ler için bir](https://consumption.azure.com/swagger/ui/index) Swagger uç noktası kullanılabilir. 1 Mayıs 2014 ' den başlayan veriler bu API aracılığıyla kullanılabilir. 

* **Bakiye ve Özet** - [Bakiye ve Özet API 'si](/rest/api/billing/enterprise/billing-enterprise-api-balance-summary) , bakiyeler, yeni satın alma işlemleri, Azure Marketi hizmeti ücretleri, ayarlamalar ve fazla kullanım ücretleri hakkında aylık bir özet sunar.

* **Kullanım ayrıntıları** - [kullanım ayrıntısı API 'Si](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail) , bir kayıt tarafından tüketilen miktarların ve tahmini giderlerin günlük bir dökümünü sağlar. Sonuç Ayrıca örnekler, ölçümler ve departmanlar hakkındaki bilgileri de içerir. API, fatura dönemi veya belirtilen başlangıç ve bitiş tarihi ile sorgulanabilir. 

* **Market Mağazası ücreti** - [Market mağaza ücreti API 'Si](/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge) , belirtilen fatura dönemi veya başlangıç ve bitiş tarihleri için güne göre kullanım tabanlı Market ücretleri dökümünü döndürür (bir zaman ücreti dahil değildir).

* **Fiyat listesi** - [Fiyat listesi API 'Si](/rest/api/billing/enterprise/billing-enterprise-api-pricesheet) , belirtilen kayıt ve fatura dönemi için her bir ölçüm için uygun ücreti sağlar.

* **Ayrılmış örnek ayrıntıları** -ayrılmış örnek [kullanım API 'Si](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage) , ayrılmış örnek satın alma işlemlerinin kullanımını döndürür. [Ayrılmış örnek ücretleri API 'si](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage) , yapılan faturalandırma işlemlerini gösterir. 

## <a name="data-freshness"></a>Veri Güncelliği
ETags, yukarıdaki tüm API 'nin yanıtında döndürülür. ETag içindeki bir değişiklik, verilerin yenilendiğini gösterir.  Aynı parametreleri kullanarak aynı API 'ye yapılan sonraki çağrılar için, yakalanan ETag öğesini http isteği üstbilgisinde "If-None-Match" anahtarıyla geçirin. "NotModified" yanıt durum kodu, verilerin daha fazla yenilenmediği ve verilerin döndürülmediği durumlarda "NotModified" olacaktır. Bir ETag değişikliği olduğunda, API gerekli dönem için tam veri kümesini döndürür.

## <a name="helper-apis"></a>Yardımcı API 'Ler
 **Faturalama dönemlerini listeleme** - [Fatura dönemleri API 'Si](/rest/api/billing/enterprise/billing-enterprise-api-billing-periods) , belirtilen kayıt için tüketim verilerine ters kronolojik düzende sahip olan faturalandırma dönemlerinin bir listesini döndürür. Her dönem, dört veri kümesi için API yolunu işaret eden bir özellik içerir-BalanceSummary, UsageDetails, Market ücretleri ve fiyat listesi.


## <a name="api-response-codes"></a>API yanıt kodları   
|Yanıt durum kodu|`Message`|Açıklama|
|-|-|-|
|200| Tamam|Hata yok|
|401| Yetkilendirilmemiş| API anahtarı bulunamadı, geçersiz, zaman aşımına uğradı.|
|404| Kullanılamaz| Rapor uç noktası bulunamadı|
|400| Bozuk İstek| Geçersiz params – tarih aralıkları EA numaralarını vb.|
|500| Sunucu Hatası| İstek işlenirken beklenmeyen hata oluştu| 









