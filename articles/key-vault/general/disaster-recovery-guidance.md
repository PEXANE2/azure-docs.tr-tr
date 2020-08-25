---
title: Bir Azure hizmet kesintisi Azure Key Vault etkiliyorsa yapmanız gerekenler Azure Key Vault | Microsoft Docs
description: Bir Azure hizmet kesintisi Azure Key Vault etkileneceğinizi öğrenin.
services: key-vault
author: ShaneBala-keyvault
manager: ravijan
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 08/24/2020
ms.author: sudbalas
ms.openlocfilehash: e4364c3c5bcba1a04837f3f9e7a0576579211c72
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88796604"
---
# <a name="azure-key-vault-availability-and-redundancy"></a>Azure Key Vault kullanılabilirliği ve yedekliliği

, Tek tek hizmet bileşenleri başarısız olsa bile, anahtarlarınızın ve sırlarınızın uygulamanız için kullanılabilir kalmasını sağlamak için birden çok artıklık katmanı Azure Key Vault.

Anahtar kasanızın içeriği bölge içinde ve bir ikincil bölgeye en az 150 mil uzakta, ancak anahtarlarınızın ve sırlarınızın yüksek dayanıklılığını sürdürmek için aynı coğrafya dahilinde çoğaltılır. Belirli bölge çiftleri hakkındaki ayrıntılar için bkz. [Azure eşleştirilmiş bölgeler](../../best-practices-availability-paired-regions.md) belgesi.

Anahtar Kasası hizmetindeki tek tek bileşenler başarısız olursa, işlevselliğe bir azalma olmadığından emin olmak için, içindeki bölge adımındaki alternatif bileşenler. Bu işlemi başlatmak için herhangi bir işlem gerçekleştirmeniz gerekmez, otomatik olarak gerçekleşir ve sizin için saydam hale gelir.

Tüm Azure bölgesinin kullanılamadığı nadir bir olayda, bu bölgedeki Azure Key Vault yaptığınız istekler otomatik olarak ikincil bir bölgeye yönlendirilir (*Yük devredildi*). Birincil bölge yeniden kullanılabilir olduğunda, istekler birincil bölgeye geri yönlendirilir (*başarısız*olur). Yine de, bu otomatik olarak gerçekleştiğinden herhangi bir işlem yapmanız gerekmez.

Bu yüksek kullanılabilirlik tasarımı aracılığıyla Azure Key Vault bakım etkinlikleri için kapalı kalma süresi gerektirmez.

Bilmeniz için bazı uyarılar vardır:

* Bölge yük devretmesi durumunda, hizmetin yük devretmesinin tamamlanması birkaç dakika sürebilir. Yük devretmeden önce bu süre içinde yapılan istekler başarısız olabilir.
* Yük devretme sırasında, anahtar kasanızın salt okuma modunda olması gerekir. Bu modda desteklenen istekler şunlardır:
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
* Yük devretme sırasında, Anahtar Kasası özelliklerinde değişiklik yapamazsınız. Erişim ilkesi veya güvenlik duvarı yapılandırma ve ayarlarını değiştiremeyeceksiniz.
* Yük devretme işlemi geri alındıktan sonra, tüm istek türleri (okuma *ve* yazma istekleri dahil) kullanılabilir.

