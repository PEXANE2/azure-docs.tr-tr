---
title: DAHA ıyı Mobile Threat Defense (MTD) 'i Azure Sentinel 'e bağlama | Microsoft Docs
description: DAHA ıyı Mobile Threat Defense (MTD) veri bağlayıcısını kullanarak Azure Sentinel 'e MTD günlüklerini çekme hakkında bilgi edinin. Çalışma kitaplarında MTD verilerini görüntüleyin, uyarı oluşturun ve araştırmayı geliştirebilirsiniz.
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
ms.openlocfilehash: 4828e31b9d15f101740c158ee62c90c95673c9a7
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98541592"
---
# <a name="connect-your-better-mobile-threat-defense-mtd-to-azure-sentinel"></a>DAHA ıyı Mobile Threat Defense (MTD) ' i Azure Sentinel 'e bağlama

> [!IMPORTANT]
> DAHA ıyı Mobile Threat Defense (MTD) Bağlayıcısı Şu anda **Önizleme** aşamasındadır. Beta, önizleme veya henüz genel kullanıma sunulmayan Azure özelliklerine uygulanan ek koşullar için [Microsoft Azure önizlemeleri için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) 'na bakın.

DAHA ıyı Mobile Threat Defense (MTD) Bağlayıcısı, daha ıyı bir Azure Sentinel güvenlik çözüm günlüklerinizi kolayca bağlamanıza olanak tanır, panoları görüntüleyebilir, özel uyarılar oluşturabilir ve araştırmayı geliştirebilirsiniz. DAHA ıyı mobil tehdit savunması ve Azure Sentinel arasında tümleştirme, REST API kullanımını sağlar.

> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="configure-and-connect-better-mobile-threat-defense"></a>DAHA ıyı mobil tehdit savunması yapılandırma ve bağlama

DAHA ıyı MTD günlükleri doğrudan Azure Sentinel 'e tümleştirebilir ve dışarı aktarabilir.

1. Azure Sentinel gezinti menüsünde **veri bağlayıcıları**' nı seçin.

1. **Veri bağlayıcıları** galerisinde, **daha iyi Mobile Threat Defense (MTD) (Önizleme)** öğesini seçin ve ardından **bağlayıcı sayfasını açın**.

1. DAHA ıyı MTD konsolunda tümleştirmeyi sonuçlandırmak için bağlayıcı sayfasında ve [Bu sayfada daha ıyı MTD belgelerindeki](https://mtd-docs.bmobi.net/integrations/azure-sentinel/setup-integration#mtd-integration-configuration) adımları izleyin.

    **Çalışma alanı kimliği** ve **birincil anahtar** değerlerini girmek istendiğinde, Azure Sentinel bağlayıcı sayfasından kopyalayın ve bunları daha iyi MTD yapılandırmasına yapıştırın.

    :::image type="content" source="media/connectors/workspace-id-primary-key.png" alt-text="{Çalışma alanı KIMLIĞI ve birincil anahtar}":::

## <a name="find-your-data"></a>Verilerinizi bulun

Başarılı bir bağlantı kurulduktan sonra, veriler **günlüklerde**, **customlogs** bölümünün altında aşağıdaki tablolardan bir veya daha fazlasına görünür:
- `BetterMTDDeviceLog_CL`
- `BetterMTDIncidentLog_CL`
- `BetterMTDAppLog_CL`
- `BetterMTDNetflowLog_CL`

Analiz kurallarında daha ıyı MTD günlüklerini sorgulamak için, sorguları veya Azure Sentinel 'de başka herhangi bir yerde sorgu penceresinin en üstünde yer alan yukarıdaki tablo adlarından birini girin.

## <a name="validate-connectivity"></a>Bağlantıyı doğrula

Günlüklerinizin Log Analytics görünmeye başlaması 20 dakikaya kadar sürebilir. 

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, daha ıyı Mobile Threat Defense (MTD) ile Azure Sentinel 'e nasıl bağlanaleceğini öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere nasıl görünürlük alabileceğinizi](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .
