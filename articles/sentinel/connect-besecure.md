---
title: Güvenlik beSECURE verilerinin ötesinde Azure Sentinel 'e bağlanın | Microsoft Docs
description: Azure Sentinel 'e güvenli Günlükler çekmek için daha fazla güvenlik beSECURE veri bağlayıcısını nasıl kullanacağınızı öğrenin. Çalışma kitaplarında beSECURE verileri görüntüleyin, uyarı oluşturun ve araştırmayı yapın.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 0001cad6-699c-4ca9-b66c-80c194e439a5
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/12/2021
ms.author: yelevin
ms.openlocfilehash: 313f201aeabd470850b27d979dc5253f80e82a55
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "98541180"
---
# <a name="connect-your-beyond-security-besecure-to-azure-sentinel"></a>Azure Sentinel 'e daha fazla güveninizi bağlama

> [!IMPORTANT]
> Daha fazla güvenlik beSECURE Bağlayıcısı Şu anda **önizlemededir**. Beta, önizleme veya henüz genel kullanıma sunulmayan Azure özelliklerine uygulanan ek koşullar için [Microsoft Azure önizlemeleri için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) 'na bakın.

Daha fazla güvenlik beSECURE Bağlayıcısı sayesinde tüm beSECURE güvenlik çözüm günlüklerinizi Azure Sentinel ile kolayca bağlamanıza, panoları görüntüleyebilir, özel uyarılar oluşturabilir ve araştırmayı geliştirebilirsiniz. BeSECURE ve Azure Sentinel arasındaki tümleştirme REST API kullanır.

> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="configure-and-connect-besecure"></a>BeSECURE yapılandırma ve bağlanma

beSECURE, günlükleri doğrudan Azure Sentinel ile tümleştirebilir ve dışarı aktarabilir.

1. Azure Sentinel gezinti menüsünde **veri bağlayıcıları**' nı seçin.

1. **Veri bağlayıcıları** galerisinden **güvenlik Besecure (Önizleme)** ve ardından **bağlayıcı sayfasını aç**' ı seçin.

1. Tarama sonuçlarını, tarama durumunu ve denetim izi günlüklerini Azure Sentinel 'e göndermek üzere beSECURE çözümünüzü yapılandırmak için aşağıdaki adımları izleyin.

    **Tümleştirme menüsüne erişin:**
    1. ' Daha fazla ' menü seçeneğine tıklayın

    1. Sunucu Seç

    1. Tümleştirme seçin

    1. Azure Sentinel'i etkinleştirme

    **Azure Sentinel ayarları ile beSECURE sağlama:**

      *Çalışma alanı kimliği* ve *birincil anahtar* değerlerini Azure Sentinel bağlayıcı sayfasından kopyalayın, Besecure yapılandırmasına yapıştırın ve **Değiştir**' e tıklayın.
      
      :::image type="content" source="media/connectors/workspace-id-primary-key.png" alt-text="{Çalışma alanı KIMLIĞI ve birincil anahtar}":::

## <a name="find-your-data"></a>Verilerinizi bulun

Başarılı bir bağlantı kurulduktan sonra, veriler **günlüklerde**, **customlogs** bölümünün altında aşağıdaki tablolardan bir veya daha fazlasına görünür:
  - `beSECURE_ScanResults_CL`
  - `beSECURE_ScanEvents_CL`
  - `beSECURE_Audit_CL`

Analiz kurallarında beSECURE günlüklerini sorgulamak, sorguları, araştırmaları veya Azure Sentinel 'de başka herhangi bir yeri ele almak için, sorgu penceresinin en üstündeki yukarıdaki tablo adlarından birini girin.

## <a name="validate-connectivity"></a>Bağlantıyı doğrula
Günlüklerinizin Log Analytics görünmeye başlaması 20 dakikaya kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Sentinel 'e nasıl bağlanaleceğini öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere nasıl görünürlük alabileceğinizi](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .
