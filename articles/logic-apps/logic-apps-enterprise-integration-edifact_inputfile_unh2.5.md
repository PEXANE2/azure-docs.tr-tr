---
title: EDIFACT mesajlarında UNH 2.5 segmentleri
description: Kurumsal Tümleştirme Paketi ile Azure Logic Apps'ta UNH2.5 segmentleriyle EDIFACT mesajlarını çözme
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 04/27/2017
ms.openlocfilehash: ad50cbb423f8c60f1caad159bc1a20cf96ed98aa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74792545"
---
# <a name="handle-edifact-documents-with-unh25-segments-in-azure-logic-apps"></a>Azure Logic Apps'ta UNH2.5 segmentleriyle EDIFACT belgelerini işleme

Bir EDIFACT belgesinde bir UNH2.5 kesimi varsa, segment şema araması için kullanılır. Örneğin, bu örnek EDIFACT iletisinde, UNH `EAN008`alanı:

`UNH+SSDD1+ORDERS:D:03B:UN:EAN008`

Bu iletiyi işlemek için aşağıda açıklanan aşağıdaki adımları izleyin:

1. Şeayı güncelle.

1. Anlaşma ayarlarını kontrol edin.

## <a name="update-the-schema"></a>Şeayı güncelleştir

İletiyi işlemek için UNH2.5 kök düğümü adı olan bir şema dağıtmanız gerekir. Örneğin, örnek UNH alanının şema kök `EFACT_D03B_ORDERS_EAN008`adı. Farklı `D03B_ORDERS` bir UNH2.5 segmentine sahip her biri için, ayrı bir şema dağıtmanız gerekir.

## <a name="add-schema-to-edifact-agreement"></a>EDIFACT anlaşmasına şema ekle

### <a name="edifact-decode"></a>EDIFACT Decode

Gelen iletiyi çözmek için EDIFACT anlaşmasının alma ayarlarındaki şemayı ayarlayın:

1. Azure [portalında](https://portal.azure.com)entegrasyon hesabınızı açın.

1. Şemayı entegrasyon hesabınıza ekleyin.

1. EDIFACT anlaşmasının alma ayarlarındaki şema yapılandırın.

1. EDIFACT anlaşmasını seçin ve **JSON olarak Edit'i**seçin. UNH2.5 değerini Alma Anlaşması'nın `schemaReferences` bölümüne ekleyin:

   ![Anlaşmayı almak için UNH2.5 ekleyin](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image1.png)

### <a name="edifact-encode"></a>EDIFACT Kodlama

Gelen iletiyi kodlamak için EDIFACT anlaşmasındaki şemayı yapılandırın ayarları

1. Azure [portalında](https://portal.azure.com)entegrasyon hesabınızı açın.

1. Şemayı entegrasyon hesabınıza ekleyin.

1. EDIFACT anlaşmasının gönderme ayarlarındaki şema yapılandırır.

1. EDIFACT anlaşmasını seçin ve **JSON olarak Edit'i**tıklatın.  Gönder Anlaşması **şemaReferanslarına** UNH2.5 değeri ekleyin

1. EDIFACT anlaşmasını seçin ve **JSON olarak Edit'i**seçin. UNH2.5 değerini Gönder Anlaşması'nın `schemaReferences` bölümüne ekleyin:

   ![Anlaşma göndermek için UNH2.5 ekleyin](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image2.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Entegrasyon hesabı anlaşmaları](../logic-apps/logic-apps-enterprise-integration-agreements.md) hakkında daha fazla bilgi edinin