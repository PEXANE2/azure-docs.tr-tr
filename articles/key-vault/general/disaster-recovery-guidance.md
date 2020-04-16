---
title: Azure Key Vault - Azure Key Vault | Microsoft Dokümanlar
description: Azure Anahtar Kasası'nı etkileyen bir Azure hizmetikesintisi durumunda ne yapmanız gerektiğini öğrenin.
services: key-vault
author: msmbaldwin
manager: rkarlin
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: mbaldwin
ms.openlocfilehash: 96929dcbe3d51589b0c3c0df89671dadb20e37cc
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81422952"
---
# <a name="azure-key-vault-availability-and-redundancy"></a>Azure Anahtar Kasası kullanılabilirliği ve artıklığı

Azure Key Vault, hizmetin tek tek bileşenleri başarısız olsa bile anahtarlarınızın ve sırlarınızın uygulamanızda kullanılabilir kalmasını sağlamak için birden çok yedek katmana sahiptir.

Anahtar kasanızın içeriği bölge içinde ve en az 150 mil uzaktaki ikincil bir bölgeye ama aynı coğrafyada çoğaltılır. Bu anahtarlar ve sırları yüksek dayanıklılık korur. Belirli [bölge](../../best-practices-availability-paired-regions.md) çiftleri hakkındaki ayrıntılar için Azure eşleştirilmiş bölgeler belgesine bakın.

Anahtar kasa hizmetiiçindeki tek tek bileşenler başarısız olursa, işlevsellikte herhangi bir bozulma olmadığından emin olmak için isteğinizi sunmak için bölge içindeki alternatif bileşenler devreye giriyor. Bunu tetiklemek için herhangi bir işlem yapmanız gerekmez. Bu otomatik olarak olur ve size şeffaf olacaktır.

Tüm Azure bölgesinin kullanılamaması durumunda, o bölgedeki Azure Anahtar Kasası'ndan yaptığınız istekler otomatik olarak ikincil bir bölgeye yönlendirilir *(başarısız olun)* olur. Birincil bölge yeniden kullanılabilir olduğunda, istekler birincil bölgeye geri yönlendirilir *(başarısız oldu).* Yine, bu otomatik olarak gerçekleşir, çünkü herhangi bir eylem de gerekmez.

Azure Key Vault, bu yüksek kullanılabilirlik tasarımı sayesinde bakım faaliyetleri için kapalı kalma süresi gerektirmez.

Dikkat edilmesi gereken birkaç uyarı vardır:

* Bir bölgenin başarısız olması durumunda, hizmetin başarısız olması birkaç dakika sürebilir. Bu süre içinde yapılan istekler, başarısız olana kadar başarısız olabilir.
* Bir failover tamamlandıktan sonra, anahtar kasası salt okunur modundadır. Bu modda desteklenen istekler şunlardır:
  * Anahtar kasalarını listele
  * Anahtar kasalarının özelliklerini alın
  * Sırları listele
  * Sırları alın
  * Liste tuşları
  * Anahtarları al (özellikleri)
  * Şifreleme
  * Şifre Çözme
  * Sarma
  * Unwrap
  * Doğrulama
  * İşaret
  * Backup
* Bir başarısız geri başarısız olduktan sonra, tüm istek türleri (okuma *ve* yazma istekleri dahil) kullanılabilir.

