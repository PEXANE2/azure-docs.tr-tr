---
title: Planlar ve faturalandırma
description: Azure Scheduler için planlar ve faturalandırma hakkında bilgi edinin
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam, estfan
ms.topic: article
ms.date: 08/18/2016
ms.openlocfilehash: e821036ff4ddb5a9786bc4f4537bb81539ab2c87
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78898469"
---
# <a name="plans-and-billing-for-azure-scheduler"></a>Azure Zamanlayıcısı için planlar ve faturalandırma

> [!IMPORTANT]
> [Azure Logic Apps,](../logic-apps/logic-apps-overview.md) [kullanımdan kaldırılan](../scheduler/migrate-from-scheduler-to-logic-apps.md#retire-date)Azure Zamanlayıcısı'nın yerini alıyor. Zamanlayıcı'da ayarladığınız işlerle çalışmaya devam etmek için lütfen mümkün olan en kısa sürede [Azure Logic Apps'a geçirin.](../scheduler/migrate-from-scheduler-to-logic-apps.md) 
>
> Zamanlayıcı artık Azure portalında kullanılamıyor, ancak işlerinizi ve iş koleksiyonlarınızı yönetebilmeniz için [ŞU anda REST API](/rest/api/scheduler) ve [Azure Scheduler PowerShell cmdlets'i](scheduler-powershell-reference.md) kullanılabilir durumda kalır.

## <a name="job-collection-plans"></a>İş toplama planları

Azure Zamanlayıcısı'nda, bir iş koleksiyonu belirli sayıda iş içerir. İş toplama faturalandırılabilir varlıktır ve burada açıklanan Standart, P10 Premium ve P20 Premium planlarında gelir: 

| İş toplama planı | Koleksiyon başına maksimum işler | Maksimum yineleme | Abonelik başına maksimum iş koleksiyonları | Sınırlar | 
|:--- |:--- |:--- |:--- |:--- |
| **Standart** | Koleksiyon başına 50 iş | Dakikada bir tane. İşleri dakikada birden fazla çalıştıramaz. | Her Azure aboneliğinde en fazla 100 Standart iş koleksiyonu olabilir. | Zamanlayıcı tam özellik kümesine erişim | 
| **P10 Premium** | Koleksiyon başına 50 iş | Dakikada bir tane. İşleri dakikada birden fazla çalıştıramaz. | Her Azure aboneliğinde 10.000'e kadar P10 Premium iş koleksiyonu bulunabilir. Daha fazla koleksiyon için <a href="mailto:wapteams@microsoft.com">bize ulaşın.</a> | Zamanlayıcı tam özellik kümesine erişim |
| **P20 Premium** | Koleksiyon başına 1000 iş | Dakikada bir tane. İşleri dakikada birden fazla çalıştıramaz. | Her Azure aboneliğinde 5.000'e kadar P20 Premium iş koleksiyonu bulunabilir. Daha fazla koleksiyon için <a href="mailto:wapteams@microsoft.com">bize ulaşın.</a> | Zamanlayıcı tam özellik kümesine erişim |
|||||| 

## <a name="pricing"></a>Fiyatlandırma

Fiyatlandırma ayrıntıları [için, Zamanlayıcı](https://azure.microsoft.com/pricing/details/scheduler/)Fiyatlandırması'na bakın.

## <a name="upgrade-or-downgrade-plans"></a>Yükseltme veya düşürme planları

İstediğiniz zaman, Standart, P10 Premium ve P20 Premium planlarında bir iş toplama planını yükseltebilir veya düşürebilirsiniz.

## <a name="active-status-and-billing"></a>Etkin durum ve faturalandırma

Tüm Azure aboneliğiniz faturalandırma sorunları nedeniyle geçici devre dışı bırakılmış duruma gitmediği sürece iş koleksiyonları her zaman etkindir. Tek bir işlem aracılığıyla bir iş koleksiyonundaki tüm işleri devre dışı bilebilir, ancak bu eylem iş koleksiyonunun faturalandırma durumunu değiştirmez, bu nedenle iş koleksiyonu *hala* faturalandırılır. Boş iş koleksiyonları etkin olarak kabul edilir ve faturalandırılır.

Bir iş koleksiyonunun faturalandırılmadığından emin olmak için iş koleksiyonunu silmeniz gerekir.

## <a name="standard-billable-units"></a>Standart faturalandırılabilir birimler

Standart faturalandırılabilir birim en fazla 10 Standart iş koleksiyonuna sahip olabilir. Standart iş koleksiyonunun koleksiyon başına en fazla 50 işi olabileceğinden, standart bir faturalandırma birimi Azure aboneliğinizin ayda 500'e kadar iş veya yaklaşık 22 *milyon* iş yürütmesine sahip olmasını sağlar. Bu liste, çeşitli standart iş koleksiyonlarına göre nasıl faturalandırıldığınızda açıklanmaktadır:

* 1 ile 10 Arasında Standart iş koleksiyonunuz varsa, tek bir standart faturabirimi için faturalandırılırsınız. 

* 11 ile 20 Arasında Standart iş koleksiyonunuz varsa, iki standart fatura birimi için faturalandırılırsınız. 

* 21 ile 30 arasında Standart iş koleksiyonunuz varsa, üç standart faturalandırma birimi için faturalandırılırsınız ve böyle devam eder.

## <a name="p10-premium-billable-units"></a>P10 prim faturalandırılabilir birimler

Bir P10 premium faturalandırılabilir birim 10.000 P10 Premium iş koleksiyonlarına sahip olabilir. Bir P10 Premium iş koleksiyonunun koleksiyon başına en fazla 50 iş sahibi olması nedeniyle, bir P10 premium faturalandırma birimi Azure aboneliğinizin ayda 500.000'e kadar iş veya neredeyse 22 *milyar* iş yürütmesine sahip olmasını sağlar. 

P10 Premium iş koleksiyonları Standart iş koleksiyonlarıyla aynı özellikleri sağlar, ancak birçok iş koleksiyonu gerektiren ve daha fazla ölçeklenebilirlik sağlayan uygulamalar için fiyat molası sunar. Bu liste, çeşitli P10 Premium iş koleksiyonlarına göre nasıl faturalandırıldığınızda açıklanmaktadır:

* 1 ile 10.000 P10 P10 Premium iş koleksiyonunuz varsa, bir P10 prim faturalandırma birimi için faturalandırılırsınız. 

* 10.001 ile 20.000 P10 Premium iş koleksiyonunuz varsa, 2 P10 premium faturalandırma birimi için faturalandırılırsınız.

## <a name="p20-premium-billable-units"></a>P20 prim faturalandırılabilir birimler

Bir P20 premium faturalandırılabilir birim 5.000 P20 Premium iş koleksiyonlarına sahip olabilir. Bir P20 Premium iş koleksiyonunun iş koleksiyonu başına 1.000'e kadar iş bulabileceğinden, bir P20 premium faturalandırma birimi Azure aboneliğinizin ayda 5.000.000'e kadar iş veya yaklaşık 220 *milyar* iş yürütmesine sahip olmasını sağlar.

P20 Premium iş koleksiyonları, P10 Premium iş koleksiyonlarıyla aynı yetenekleri sağlarken, aynı zamanda koleksiyon başına daha fazla sayıda işi ve genel olarak P10 Premium'dan daha fazla toplam işi destekleyerek daha fazla ölçeklenebilirlik sağlar.

## <a name="plan-comparison"></a>Plan karşılaştırması

* 100'den fazla Standart iş koleksiyonunuz (10 standart faturalandırma birimi) varsa, tüm iş koleksiyonlarını Premium planda bulundurarak daha iyi bir anlaşma elde edebilirsiniz.

* Bir Standart iş koleksiyonunuz ve bir Premium iş koleksiyonunuz varsa, bir standart faturalandırma birimi *ve* bir premium faturalandırma birimi için faturalandırılırsınız.

  Zamanlayıcı hizmet faturaları, standart veya premium olan etkin iş koleksiyonlarının sayısına göre faturalanır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Scheduler kavramları, terminolojisi ve varlık hiyerarşisi](scheduler-concepts-terms.md)
* [Azure Scheduler sınırları, varsayılanları ve hata kodları](scheduler-limits-defaults-errors.md)