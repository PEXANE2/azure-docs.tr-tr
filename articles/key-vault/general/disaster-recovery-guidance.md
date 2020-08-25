---
title: Azure Key Vault Azure Key Vault etkileyen bir Azure hizmet kesintisi durumunda yapılacaklar | Microsoft Docs
description: Azure Key Vault etkileyen bir Azure hizmet kesintisi durumunda yapılacaklar hakkında bilgi edinin.
services: key-vault
author: ShaneBala-keyvault
manager: ravijan
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 05/04/2020
ms.author: sudbalas
ms.openlocfilehash: 4796e6c555ca67794409fb1476f3c4fd0d760719
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "82780462"
---
# <a name="azure-key-vault-availability-and-redundancy"></a>Azure Key Vault kullanılabilirliği ve yedekliliği

, Tek tek hizmet bileşenleri başarısız olsa bile, anahtarlarınızın ve sırlarınızın uygulamanız için kullanılabilir kalmasını sağlamak için birden çok artıklık katmanı Azure Key Vault.

Anahtar kasanızın içeriği bölge içinde ve bir ikincil bölgeye en az 150 mil uzakta, ancak aynı coğrafya dahilinde çoğaltılır. Bu, anahtarlarınızın ve sırlarınızın yüksek dayanıklılığına sahip olmasını sağlar. Belirli bölge çiftleri hakkındaki ayrıntılar için bkz. [Azure eşleştirilmiş bölgeler](../../best-practices-availability-paired-regions.md) belgesi.

Anahtar Kasası hizmetindeki tek tek bileşenler başarısız olursa, işlevselliğe bir azalma olmadığından emin olmak için, içindeki bölge adımındaki alternatif bileşenler. Bunu tetiklemek için herhangi bir işlem gerçekleştirmeniz gerekmez. Otomatik olarak gerçekleşir ve sizin için saydam hale gelir.

Tüm Azure bölgesinin kullanılamadığı nadir bir olayda, bu bölgedeki Azure Key Vault yaptığınız istekler otomatik olarak ikincil bir bölgeye yönlendirilir (*Yük devredildi*). Birincil bölge yeniden kullanılabilir olduğunda, istekler birincil bölgeye geri yönlendirilir (*başarısız*olur). Yine de, bu otomatik olarak gerçekleştiğinden herhangi bir işlem yapmanız gerekmez.

Bu yüksek kullanılabilirlik tasarımı aracılığıyla Azure Key Vault bakım etkinlikleri için kapalı kalma süresi gerektirmez.

Bilmeniz için bazı uyarılar vardır:

* Bölge yük devretmesi durumunda, hizmetin yük devretmesinin tamamlanması birkaç dakika sürebilir. Bu süre boyunca yapılan istekler yük devretme tamamlanana kadar başarısız olabilir.
* Yük devretme tamamlandıktan sonra, anahtar kasanızın salt okuma modunda olması gerekir. Bu modda desteklenen istekler şunlardır:
  * Anahtar kasalarını Listele
  * Anahtar kasalarının özelliklerini al
   * Sertifikaları listeleme
  * Sertifika Al
  * Gizli dizileri listeleme
  * Gizli dizileri al
  * Anahtarları Listele
  * Get (özellikleri) anahtarları
  * Şifreleme
  * Şifre Çözme
  * Ilacağını
  * Unwrap
  * Doğrulama
  * İşaret
  * Backup
* Yük devretme işlemi geri alındıktan sonra, tüm istek türleri (okuma *ve* yazma istekleri dahil) kullanılabilir.

