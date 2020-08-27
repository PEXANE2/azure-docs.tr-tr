---
title: Rezervasyon indirimleri Azure Synapse Analytics için nasıl uygulanır? | Microsoft Docs
description: Para tasarrufu etmenize yardımcı olacak rezervasyon indirimlerinin Azure Synapse Analytics için nasıl uygulanacağını öğrenin.
author: yashesvi
ms.reviewer: yashar
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: banders
ms.openlocfilehash: f0bd57befc0ec30473065ac69026a77e21e30e5c
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88682612"
---
# <a name="how-reservation-discounts-apply-to-azure-synapse-analytics"></a>Azure Synapse Analytics için rezervasyon indirimleri nasıl uygulanır?

Azure Synapse Analytics ayrılmış kapasitesini satın almanızın ardından, o bölgede sağlanan mevcut örneklerinize otomatik olarak rezervasyon indirimi uygulanır. Rezervasyon indirimi, Azure Synapse Analytics cDWU ölçümü tarafından gösterilen kullanıma uygulanır. Depolama ve ağ iletişimi, kullandıkça öde fiyatlarıyla ücretlendirilir.

## <a name="reservation-discount-application"></a>Rezervasyon indirimi uygulama

Azure Synapse Analytics ayrılmış kapasite indirimi, çalışmakta olan ambarlara saat bazında uygulanır. Bir saat için dağıtılmış bir ambarınız yoksa, o saat için ayrılmış kapasite boşa gider. Devredilmez.

Satın alımın ardından, satın aldığınız rezervasyon, belirli bir noktada ambarların çalıştırılmasıyla gösterilen bir Azure Synapse Analytics kullanımı ile eşleştirilir. Bazı ambarları kapatırsanız, eşleşen diğer ambarlara rezervasyon indirimleri otomatik olarak uygulanır.

Tam bir saat boyunca çalıştırılmayan ambarlar için rezervasyon, o saatteki diğer eşleşen örneklere otomatik olarak uygulanır.

## <a name="discount-examples"></a>İndirim örnekleri

Aşağıdaki örneklerde, dağıtımlara bağlı olarak Azure Synapse Analytics ayrılmış kapasite indiriminin nasıl uygulandığı gösterilir.

- **Örnek 1**: 5 birim 100 cDWU ayrılmış kapasite satın alırsınız. DW1500c Azure Synapse Analytics örneğini bir saat süreyle çalıştırırsınız. Bu durumda, 15 birim 100 cDWU kullanımı gösterilir. Rezervasyon indirimi, kullandığınız 5 birime uygulanır. Kullandığınız kalan 10 birim 100 cDWU kullanımı için kullandıkça öde fiyatları kullanılarak ücretlendirilirsiniz. Diğer bir deyişle, birden çok rezervasyon için kısmi kapsam mümkündür.

- **Örnek 2**: 5 birim 100 cDWU ayrılmış kapasite satın alırsınız. İki DW100c Azure Synapse Analytics örneğini bir saat süreyle çalıştırırsınız. Bu durumda, 1 birim 100 cDWU kullanımı için iki kullanım olayı gösterilir. Her iki kullanım olayı da ayrılmış kapasite indirimi alır. Kalan 3 birim 100 cDWU ayrılmış kapasitesi boş gider ve gelecekte kullanılmak üzere devredilmez. Diğer bir deyişle, tek bir rezervasyon birden çok Azure Synapse Analytics örneğiyle eşleştirilebilir.

- **Örnek 3**: 1 birim 100 cDWU ayrılmış kapasitesi satın alırsınız. İki DW100c Azure Synapse Analytics örneğini çalıştırırsınız. Her biri 30 dakika boyunca çalışır. Bu durumda her iki kullanım olayı da ayrılmış kapasite indirimi alır. Kullandıkça öde fiyatları kullanılarak bir kullanım ücretlendirilmez.

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun

- Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

Azure Ayrılmış Sanal Makine Örnekleri hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure Rezervasyonlar nedir?](save-compute-costs-reservations.md)
- [Rezervasyon işlemlerini görüntüleme](view-reservations.md)
- [API aracılığıyla rezervasyon işlemlerini ve kullanım bilgilerini alma](reservation-apis.md)
- [Rezervasyonları yönetme](manage-reserved-vm-instance.md)
