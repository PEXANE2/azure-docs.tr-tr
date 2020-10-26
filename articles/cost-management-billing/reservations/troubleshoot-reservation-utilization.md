---
title: Azure rezervasyon kullanımı sorunlarını giderme
description: Bu makale, Azure portalda hiç kullanım göstermeyen veya sıfır (0) kullanım gösteren Azure rezervasyonlarını anlamanıza ve bunlarla ilgili sorunları gidermenize yardımcı olur. Eşleşmeyen kullanım da açıklanmaktadır.
author: bandersmsft
ms.service: cost-management-billing
ms.subservice: reservations
ms.author: banders
ms.reviewer: yashar
ms.topic: troubleshooting
ms.date: 10/19/2020
ms.openlocfilehash: 9d5706843e8131110566ad8f955415b6db29f7ba
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2020
ms.locfileid: "92207452"
---
# <a name="troubleshoot-reservation-utilization"></a>Rezervasyon kullanımı sorunlarını giderme

Bu makale, Azure portalda hiç kullanım göstermeyen veya sıfır (0) kullanım gösteren Azure rezervasyonlarını anlamanıza ve bunlarla ilgili sorunları gidermenize yardımcı olur. Eşleşmeyen kullanım da açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

1. [Azure portalda](https://portal.azure.com) oturum açın ve **Rezervasyonlar** ’a gidin.
1. Rezervasyonlar listesinde, bir rezervasyonun kullanım miktarını **Kullanım (%)** sütununda görebilirsiniz. Bu değer %0 olabilir.
1. Rezervasyonu seçin.
1. Rezervasyona genel bakış sayfasındaki grafikte gösterilen kullanım yüzdeniz, rezervasyon listesinde gösterilen değerle eşleşmeyebilir.

## <a name="cause"></a>Nedeni

Azure portaldaki **Kullanım (%)** sütunu, geçerli güne ait değeri gösterir. Bu değer, kaynakların çalıştırıldığı konumdan kullanım verileri geldikçe hesaplanır. Azure, kullanım yüzdesini hesaplamak için bu kullanım verilerinden yararlanır.

Bazı kaynaklar kullanımı diğerlerinden daha yavaş bildirir. Ayrıca, SQL Veritabanları gibi bazı ürün türleri de kullanım verilerini yavaş bir biçimde bildirir.

Bu gecikme süresi, kullanım hesaplamasının gerçek kullanıma kıyasla daha düşük değerler göstermesine neden olabilir. Aradaki fark, gün sınırında belirgin olabilir. Böyle durumlarda, Azure dört ile sekiz saat içinde kullanım verilerini almazsa %0 değerini hesaplar. %0 değeri, kullanım verileri ulaşmadığından gösterilir. Bu durumda, rezervasyonun herhangi bir kaynağa avantaj uygulamadığı görülür.

Kullanım verileri ulaştığında, değer doğru yüzdeyle değiştirilir. Tüm kullanım verileri toplandığında, doğru değer belirlenir ve grafikte doğru şekilde gösterilir.

## <a name="solution"></a>Çözüm

Kullanım değerlerinizin beklediğiniz değerlerle eşleşmediğini fark ederseniz gerçek kullanımınıza ilişkin en doğru görünümü almak için grafiği inceleyin. İki günden daha eski olan tüm nokta değerlerinin doğru olması gerekir. 7 ile 30 gün arasındaki daha uzun dönemli ortalamaların doğru olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- Rezervasyonları yönetme hakkında daha fazla bilgi için bkz. [Azure kaynakları için rezervasyonları yönetme](manage-reserved-vm-instance.md).