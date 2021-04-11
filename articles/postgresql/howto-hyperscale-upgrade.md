---
title: Sunucu grubunu Yükselt-hiper ölçek (Citus)-PostgreSQL için Azure veritabanı
description: Bu makalede PostgreSQL için Azure veritabanı 'nda PostgreSQL ve Citus 'ı nasıl yükseltebileceğinizi (Citus) açıklanmaktadır.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: how-to
ms.date: 4/5/2021
ms.openlocfilehash: 3758e2d458e1a6bd052ac746ac361de033d508e9
ms.sourcegitcommit: 6ed3928efe4734513bad388737dd6d27c4c602fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2021
ms.locfileid: "107024029"
---
# <a name="upgrade-hyperscale-citus-server-group"></a>Hyperscale (Citus) sunucu grubunu yükselt

Bu yönergeler, tüm sunucu grubu düğümlerinde PostgreSQL 'in yeni ana sürümüne nasıl yükselteceğiniz açıklanır.

## <a name="test-the-upgrade-first"></a>Önce yükseltmeyi test et

PostgreSQL 'in yükseltilmesi, Imagine 'den daha fazla değişikliğe neden olur, çünkü hiper ölçek (Citus), Citus uzantısı da dahil olmak üzere [veritabanı uzantılarını](concepts-hyperscale-extensions.md)da yükseltecektir.
Üretim ortamınızı yükseltmeden önce, uygulamanızı yeni PostgreSQL ve Citus sürümüyle test etmenizi kesinlikle öneririz.

Test etmenin kolay bir yolu, sunucu grubunuzun bir kopyasını, [zaman içinde geri yükleme](concepts-hyperscale-backup.md#point-in-time-restore-pitr)kullanarak yapmakla sonuçlanır. Kopyayı yükseltin ve uygulamanızı buna karşı test edin. Her şeyin düzgün şekilde çalışmasını doğruladıktan sonra, özgün sunucu grubunu yükseltin.

## <a name="upgrade-a-server-group-in-the-azure-portal"></a>Azure portal bir sunucu grubunu yükseltme

1. Hiper ölçek (Citus) sunucu grubunun **genel bakış** bölümünde, **Yükselt** düğmesini seçin.
1. PostgreSQL ve Citus 'in geçerli sürümünü gösteren bir iletişim kutusu görünür.
   **Yükseltme için** listeden yeni bir PostgreSQL sürümü seçin.
1. **Yükseltmeden sonra Citus sürümündeki** değeri doğrulayın.
   Bu değer, seçtiğiniz PostgreSQL sürümüne göre değişir.
1. Devam etmek için **Yükselt** düğmesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Desteklenen PostgreSQL sürümleri](concepts-hyperscale-versions.md)hakkında bilgi edinin.
* Hiper ölçek (Citus) sunucu grubundaki her bir PostgreSQL sürümü ile [hangi uzantıların](concepts-hyperscale-extensions.md) paketlendiğini görün.
