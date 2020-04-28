---
title: EDIOLGU iletilerindeki UNH 2,5 segmentleri
description: Enterprise Integration Pack ile Azure Logic Apps içindeki UNH 2.5 kesimlerle EDIOLGU iletilerini çözümleyin
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 04/27/2017
ms.openlocfilehash: ad50cbb423f8c60f1caad159bc1a20cf96ed98aa
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74792545"
---
# <a name="handle-edifact-documents-with-unh25-segments-in-azure-logic-apps"></a>Azure Logic Apps 'de UNH 2.5 segmentiyle EDIOLGU belgelerini işleme

Bir EDIOLGU belgesinde bir UNH 2.5 segmenti varsa, segment şema araması için kullanılır. Örneğin, bu örnek EDIOLGU iletisinde UNH alanı şu şekilde olur `EAN008`:

`UNH+SSDD1+ORDERS:D:03B:UN:EAN008`

Bu iletiyi işlemek için aşağıda açıklanan adımları izleyin:

1. Şemayı güncelleştirin.

1. Anlaşma ayarlarını kontrol edin.

## <a name="update-the-schema"></a>Şemayı güncelleştirme

İletiyi işlemek için, UNH 2.5 kök düğüm adına sahip bir şemayı dağıtmanız gerekir. Örneğin, örnek UNH alanı için şema kök adı `EFACT_D03B_ORDERS_EAN008`. Her birinin `D03B_ORDERS` farklı bir Unh 2.5 kesimine sahip olması için, tek bir şemayı dağıtmanız gerekir.

## <a name="add-schema-to-edifact-agreement"></a>EDıOLGU anlaşmasına şema ekle

### <a name="edifact-decode"></a>EDIOLGU kod çözme

Gelen iletinin kodunu çözmek için, EDıOLGU sözleşmesinin alma ayarlarında şemayı ayarlayın:

1. [Azure Portal](https://portal.azure.com)tümleştirme hesabınızı açın.

1. Şemayı tümleştirme hesabınıza ekleyin.

1. Yapılandırma, EDıOLGU sözleşmesinin alma ayarlarında yapılandırılır.

1. EDIOLGU Sözleşmesi ' ni seçin ve **JSON olarak Düzenle**' yi seçin. Alma sözleşmesinin `schemaReferences` bölümüne Unh 2.5 değerini ekleyin:

   ![Sözleşmeyi almak için UNH 2.5 ekleyin](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image1.png)

### <a name="edifact-encode"></a>EDIOLGU kodlama

Gelen iletiyi kodlamak için, bu şemayı EDıOLGU sözleşmesi gönderme ayarları ' nda yapılandırın

1. [Azure Portal](https://portal.azure.com)tümleştirme hesabınızı açın.

1. Şemayı tümleştirme hesabınıza ekleyin.

1. EDıOLGU sözleşmesinin gönderme ayarlarındaki şemayı yapılandırın.

1. EDIOLGU Sözleşmesi ' ni seçin ve **JSON olarak Düzenle**' ye tıklayın.  Gönderme sözleşmesi 'nde UNH 2.5 değeri ekleyin **schemaReferences**

1. EDIOLGU Sözleşmesi ' ni seçin ve **JSON olarak Düzenle**' yi seçin. Gönderme sözleşmesinin `schemaReferences` bölümüne Unh 2.5 değerini ekleyin:

   ![Anlaşmayı göndermek için UNH 2.5 ekleyin](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image2.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Tümleştirme hesabı sözleşmeleri](../logic-apps/logic-apps-enterprise-integration-agreements.md) hakkında daha fazla bilgi edinin