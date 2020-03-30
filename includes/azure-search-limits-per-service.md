---
title: include dosyası
description: include dosyası
services: search
author: HeidiSteen
ms.service: cognitive-search
ms.topic: include
ms.date: 05/06/2019
ms.author: heidist
ms.custom: include file
ms.openlocfilehash: 179f525b7b6a7e51889b14b66df6c537ca56bd75
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80272853"
---
Depolama, hangisi önce gerçekleşirse, disk alanı veya *en fazla* dizin, belge veya diğer üst düzey kaynak sayısında sabit bir sınırla sınırlandırılmıştır. Aşağıdaki tablo depolama sınırlarını belgeler. Dizinler, belgeler ve diğer nesnelerle ilgili maksimum sınırlar için kaynak [tarafından sınırlar'a](../articles/search/search-limits-quotas-capacity.md#index-limits)bakın.

| Kaynak | Ücretsiz | Temel<sup>1</sup> | S1 | S2 | S3 | S3&nbsp;HD<sup>2</sup> | L1 | L2 |
| -------- | --- | --- | --- | --- | --- | --- | --- | --- |
| Hizmet düzeyi sözleşmesi (SLA)<sup>3</sup>  |Hayır |Evet |Evet |Evet |Evet |Evet |Evet |Evet |
| Bölüm başına depolama |50 MB |2 GB |25 GB |100 GB |200 GB |200 GB |1 TB |2 TB |
| Hizmet başına bölüm |Yok |1 |12 |12 |12 |3 |12 |12 |
| Bölüm boyutu |Yok |2 GB |25 GB |100 GB |200 GB |200 GB |1 TB |2 TB |
| Çoğaltmalar |Yok |3 |12 |12 |12 |12 |12 |12 |

<sup>1</sup> Basic'in bir sabit bölümü vardır. Bu katmanda, artan sorgu iş yükleri için daha fazla yineleme ayırmak için ek arama birimleri kullanılır.

<sup>2</sup> S3 HD, S3 için bölüm sınırından daha düşük olan üç bölümden oluşan sabit bir sınıra sahiptir. S3 HD dizin sınırı çok daha yüksek olduğu için daha düşük bir bölüm sınırı uygulanmaktadır. Hem işlem kaynakları (depolama ve işleme) hem de içerik (dizinler ve belgeler) için hizmet sınırları mevcut olduğu için içerik sınırına ilk önce ulaşılır.

<sup>3</sup> Özel kaynaklarda faturalandırılabilir hizmetler için hizmet düzeyi anlaşmaları sunulur. Ücretsiz hizmetler ve önizleme özellikleri sla yok. Faturalandırılabilir hizmetler için, hizmetiniz için yeterli fazlalık sağlandığında SLA'lar etkili olur. Sorgu (okuma) SLA'ları için iki veya daha fazla yineleme gereklidir. Sorgu ve dizin oluşturma (okuma-yazma) SLA'ları için üç veya daha fazla yineleme gereklidir. Bölüm sayısı bir SLA dikkate değildir. 