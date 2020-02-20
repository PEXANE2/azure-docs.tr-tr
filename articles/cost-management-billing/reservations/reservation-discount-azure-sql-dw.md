---
title: Azure SQL Veri Ambarı için rezervasyon indirimleri nasıl uygulanır? | Microsoft Docs
description: Para tasarrufu etmenize yardımcı olması için Azure SQL Veri Ambarı için rezervasyon indirimlerinin nasıl uygulanacağını öğrenin.
author: yashesvi
ms.reviewer: yashar
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: banders
ms.openlocfilehash: b08f11799c1471af22138fefcd57ed1eb951a1a6
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77199390"
---
# <a name="how-reservation-discounts-apply-to-azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı için rezervasyon indirimleri nasıl uygulanır?

Azure SQL Veri Ambarı ayrılmış kapasitesini satın almanızın ardından, o bölgede mevcut olan veri ambarlarına otomatik olarak rezervasyon indirimi uygulanır. Rezervasyon indirimi, SQL Veri Ambarı cDWU ölçümü tarafından gösterilen kullanıma uygulanır. Depolama ve ağ iletişimi, kullandıkça öde fiyatlarıyla ücretlendirilir.

## <a name="reservation-discount-application"></a>Rezervasyon indirimi uygulama

SQL Veri Ambarı ayrılmış kapasite indirimi, çalışmakta olan ambarlara saat bazında uygulanır. Bir saat için dağıtılmış bir ambarınız yoksa, o saat için ayrılmış kapasite boşa gider. Devredilmez.

Satın alımın ardından, satın aldığınız rezervasyon, belirli bir noktada ambarlar çalıştırılarak gösterilen bir SQL Veri Ambarı kullanımı ile eşleştirilir. Bazı ambarları kapatırsanız, eşleşen diğer ambarlara rezervasyon indirimleri otomatik olarak uygulanır.

Tam bir saat boyunca çalıştırılmayan ambarlar için rezervasyon, o saatteki diğer eşleşen örneklere otomatik olarak uygulanır.

## <a name="discount-examples"></a>İndirim örnekleri

Aşağıdaki örneklerde, dağıtımlara bağlı olarak SQL Veri Ambarı ayrılmış kapasite indiriminin nasıl uygulandığı gösterilmektedir.

- **Örnek 1**: 5 birim 100 cDWU ayrılmış kapasite satın alırsınız. Bir saat boyunca bir DW1500c SQL Veri Ambarı örneği çalıştırırsınız. Bu durumda, 15 birim 100 cDWU kullanımı gösterilir. Rezervasyon indirimi, kullandığınız 5 birime uygulanır. Kullandığınız kalan 10 birim 100 cDWU kullanımı için kullandıkça öde fiyatları kullanılarak ücretlendirilirsiniz. Diğer bir deyişle, birden çok rezervasyon için kısmi kapsam mümkündür.

- **Örnek 2**: 5 birim 100 cDWU ayrılmış kapasite satın alırsınız. Bir saat boyunca iki DW100c SQL Veri Ambarı örneğini çalıştırırsınız. Bu durumda, 1 birim 100 cDWU kullanımı için iki kullanım olayı gösterilir. Her iki kullanım olayı da ayrılmış kapasite indirimi alır. Kalan 3 birim 100 cDWU ayrılmış kapasitesi boş gider ve gelecekte kullanılmak üzere devredilmez. Diğer bir deyişle, tek bir rezervasyon birden çok SQL Veri Ambarı örneğiyle eşleştirilebilir.

- **Örnek 3**: 1 birim 100 cDWU ayrılmış kapasitesi satın alırsınız. İki DW100c SQL Veri Ambarı örneğini çalıştırırsınız. Her biri 30 dakika boyunca çalışır. Bu durumda her iki kullanım olayı da ayrılmış kapasite indirimi alır. Kullandıkça öde fiyatları kullanılarak bir kullanım ücretlendirilmez.

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun

- Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

Azure Ayrılmış Sanal Makine Örnekleri hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure Rezervasyonlar nedir?](save-compute-costs-reservations.md)
- [Rezervasyon işlemlerini görüntüleme](view-reservations.md)
- [API aracılığıyla rezervasyon işlemlerini ve kullanım bilgilerini alma](reservation-apis.md)
- [Rezervasyonları yönetme](manage-reserved-vm-instance.md)
