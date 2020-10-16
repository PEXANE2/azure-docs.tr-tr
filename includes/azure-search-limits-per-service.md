---
title: dosya dahil etme
description: dosya dahil etme
services: search
author: HeidiSteen
ms.service: cognitive-search
ms.topic: include
ms.date: 05/11/2020
ms.author: heidist
ms.custom: include file
ms.openlocfilehash: fe5c8129434ddb4eec2dd25a3f123f28b4db221b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "83682624"
---
Bir arama hizmeti, disk alanı veya en fazla dizin ya da Dizin Oluşturucu sayısı için, hangisi önce gelirse bir sabit sınır ile kısıtlanır. Aşağıdaki tablo, depolama sınırlarını belgelemektedir. En fazla nesne sınırları için bkz. [kaynaklara göre sınırlar](../articles/search/search-limits-quotas-capacity.md#index-limits).

| Kaynak | Ücretsiz | Temel<sup>1</sup> | S1 | S2 | S3 | S3 &nbsp; HD | L1 | L2 |
| -------- | --- | --- | --- | --- | --- | --- | --- | --- |
| Hizmet düzeyi sözleşmesi (SLA)<sup>2</sup>  |Hayır |Evet |Evet |Evet |Evet |Evet |Evet |Evet |
| Bölüm başına depolama |50 MB |2 GB |25 GB |100 GB |200 GB |200 GB |1 TB |2 TB |
| Hizmet başına bölüm |Yok |1 |12 |12 |12 |3 |12 |12 |
| Bölüm boyutu |Yok |2 GB |25 GB |100 GB |200 GB |200 GB |1 TB |2 TB |
| Çoğaltmalar |Yok |3 |12 |12 |12 |12 |12 |12 |

<sup>1</sup> temel, sabit bir bölüme sahiptir. Daha büyük sorgu birimlerine yönelik çoğaltmalar eklemek için ek arama birimleri kullanılabilir.

<sup>2</sup> hizmet düzeyi sözleşmeleri, ayrılmış kaynaklarda faturalandırılabilir hizmetler için geçerli olur. Ücretsiz hizmetler ve Önizleme özelliklerinin SLA 'Sı yoktur. Faturalanabilir hizmetler için, hizmet için yeterli artıklık sağladığınızda SLA 'Lar devreye girer. Sorgu (Read) SLA 'Lar için iki veya daha fazla çoğaltma gerekiyor. Sorgu ve dizin oluşturma (okuma-yazma) SLA 'Lar için üç veya daha fazla çoğaltma gereklidir. Bölüm sayısı bir SLA değerlendirmesi değildir. 