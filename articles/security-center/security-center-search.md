---
title: Azure Güvenlik Merkezi araması | Microsoft Docs
description: Azure Güvenlik Merkezi 'nin güvenlik verilerinizi almak ve analiz etmek için Azure Izleyici günlükleri aramasını nasıl kullandığını öğrenin.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/11/2017
ms.author: rkarlin
ms.openlocfilehash: c88e2025444aa2fd83e05fdff97ea640ceefaa04
ms.sourcegitcommit: e3b0fb00b27e6d2696acf0b73c6ba05b74efcd85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68662922"
---
# <a name="azure-security-center-search-retired"></a>Azure Güvenlik Merkezi araması (kullanımdan kaldırıldı)

> [!NOTE]
> Güvenlik Merkezi 'nin arama panosu 31 Temmuz 2019 tarihinde kullanımdan kaldırılmıştır. Daha fazla bilgi ve diğer hizmetler için bkz. [Güvenlik Merkezi özelliklerini devre dışı bırakma (2019 Temmuz)](security-center-features-retirement-july2019.md#menu_search).

Azure Güvenlik Merkezi, güvenlik verilerinizi almak ve analiz etmek için [Azure izleyici günlükleri aramasını](../log-analytics/log-analytics-log-searches.md) kullanır. Azure Izleyici günlükleri verileri hızlı bir şekilde almak ve birleştirmek için bir sorgu dili içerir. Güvenlik Merkezi 'nden, sorguları oluşturmak ve toplanan verileri analiz etmek için Azure Izleyici günlükleri araması özelliğinden yararlanabilirsiniz.

Arama, hem ücretsiz katmanda hem de Güvenlik Merkezi 'nin standart katmanında kullanılabilir.  Günlük aramalarınızda bulunan veriler, çalışma alanınıza uygulanan katman düzeyine bağlıdır.  Daha fazla bilgi için Güvenlik Merkezi [fiyatlandırma sayfasına](../security-center/security-center-pricing.md) bakın.


> [!NOTE]
> Güvenlik Merkezi, ücretsiz katman altındaki bir çalışma alanı için güvenlik verilerini kaydetmez. Ücretsiz katman altında bir çalışma alanına çeşitli Günlükler gönderebilir ve bu verileri arayabilir, ancak arama sonuçları Güvenlik Merkezi 'nden veri içermez. Güvenlik Merkezi, verileri yalnızca Standart katman altındaki bir çalışma alanına kaydeder.
>
>

## <a name="access-search"></a>Erişim arama
1. Güvenlik Merkezi ana menüsünde **Ara**' yı seçin.

   ![Günlük aramasını seçin][1]

2. Güvenlik Merkezi, Azure aboneliklerinizin altındaki tüm çalışma alanlarını listeler. Bir çalışma alanı seçin. (Yalnızca bir çalışma alanınız varsa, bu çalışma alanı Seçicisi görünmez.)

   ![Çalışma alanı seçin][2]

3. **Günlük araması** açılır. Seçili çalışma alanı altında daha fazla veri sorgulamak için şu örnek sorguyu girin:

   SecurityEvent | WHERE EventID = = 4625 | Count () ile TargetAccount özetleme

   Sonuç, oturum açma başarısız olan tüm hesapları gösterir (olay 4625).

   ![Arama sonuçları][3]

Seçilen çalışma alanı altında verileri sorgulama hakkında daha fazla bilgi için bkz. [kusto Query Language](../log-analytics/log-analytics-search-reference.md) .

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, güvenlik merkezi 'nde aramaya erişme hakkında daha fazla öğrendiniz. Güvenlik Merkezi, Azure Izleyici günlükleri aramasını kullanır. Azure Izleyici günlükleri araması hakkında daha fazla bilgi edinmek için bkz.:

- [Azure Izleyici günlükleri nedir?](../log-analytics/log-analytics-overview.md) – Azure Izleyici günlüklerine genel bakış
- [Azure izleyici günlüklerinde günlük aramalarını anlama](../log-analytics/log-analytics-log-search-new.md) -günlük aramalarının Azure izleyici günlüklerinde nasıl kullanıldığını açıklar ve günlük araması oluşturmadan önce anlaşılması gereken kavramlar sağlar
- [Azure izleyici günlüklerinde günlük aramalarını kullanarak veri bulma](../log-analytics/log-analytics-log-searches.md) – günlük aramasını kullanma öğreticisi
- [Kusto arama başvurusu](../log-analytics/log-analytics-search-reference.md) – Azure izleyici günlüklerinde sorgu dilini açıklar

Güvenlik Merkezi hakkında daha fazla bilgi için bkz.

- [Güvenlik Merkezi 'Ne genel bakış](security-center-intro.md) – Güvenlik Merkezi 'nin temel yeteneklerini açıklar

<!--Image references-->
[1]: ./media/security-center-search/search.png
[2]: ./media/security-center-search/workspace-selector.png
[3]: ./media/security-center-search/log-search.png
