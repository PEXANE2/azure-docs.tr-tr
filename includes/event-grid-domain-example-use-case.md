---
title: include dosyası
description: include dosyası
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: include
ms.date: 03/04/2021
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 2459cf6c5055dcde83dccf37addc160aeeaa64ad
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102198669"
---
Olay etki alanları bir örnek kullanılarak kolayca açıklanmıştır. Tractors, dijitalde ekipman ve diğer ağır makineleri ürettiğiniz contoso oluşturma makinemizin olduğunu varsayalım. İşletmeyi çalıştırmanın bir parçası olarak, donanım bakımı, sistem durumu ve sözleşme güncelleştirmeleri hakkındaki müşterilere gerçek zamanlı bilgiler gönderirsiniz. Bu bilgilerin tümü, uygulamanız, müşteri uç noktaları ve müşterilerin ayarladığı diğer altyapıda dahil olmak üzere çeşitli uç noktalara gider.

Olay etki alanları, contoso oluşturma makineleri tek bir olay varlığı olarak modelleyebilir. Müşterilerinizin her biri etki alanı içinde bir konu olarak temsil edilir. Kimlik doğrulama ve yetkilendirme Azure Active Directory kullanılarak işlenir. Müşterilerinizin her biri konuya abone olabilir ve olaylarını kendilerine teslim edebilir. Olay etki alanı üzerinden yönetilen erişim, yalnızca kendi konularına erişebilmelerini sağlar.

Ayrıca, size tüm müşteri olaylarınızı yayımlayabilmeniz için tek bir uç nokta sağlar. Event Grid her konunun yalnızca kiracının kapsamına alınmış olayların farkında olduğundan emin olunacaktır.

:::image type="content" source="./media/event-grid-domain-example-use-case/contoso-construction-example.png" alt-text="Contoso yapımı örneği" lightbox="./media/event-grid-domain-example-use-case/contoso-construction-example.png":::