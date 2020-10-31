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
ms.date: 10/25/2020
ms.author: yelevin
ms.openlocfilehash: f6fd6920fafe4c1080cb5539e4e0222d9d6e18cd
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93103029"
---
# <a name="connect-your-beyond-security-besecure-to-azure-sentinel"></a>Azure Sentinel 'e daha fazla güveninizi bağlama

> [!IMPORTANT]
> Azure Sentinel 'de daha fazla güvenlik beSECURE veri Bağlayıcısı Şu anda genel önizlemededir. Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sunulmaktadır. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Güvenlik beSECURE bağlayıcısının ötesinde, tüm beSECURE güvenlik çözüm günlüklerinizi Azure Sentinel ile kolayca bağlamanıza, panoları görüntülemenize, özel uyarılar oluşturmanıza ve araştırmaya olanak tanır. BeSECURE ve Azure Sentinel arasındaki tümleştirme REST API kullanır.

> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="configure-and-connect-besecure"></a>BeSECURE yapılandırma ve bağlanma

beSECURE, günlükleri doğrudan Azure Sentinel ile tümleştirebilir ve dışarı aktarabilir.

1. Azure Sentinel portalında, **veri bağlayıcıları** ' na tıklayın ve **güvenlik Besecure (Önizleme) dışında** bir seçim yapın ve **bağlayıcı sayfasını açın** .

1. Tarama sonuçlarını, tarama durumunu ve denetim izi günlüklerini Azure Sentinel 'e göndermek üzere beSECURE çözümünüzü yapılandırmak için aşağıdaki adımları izleyin.

    **Tümleştirme menüsüne erişin:**
    1. ' Daha fazla ' menü seçeneğine tıklayın

    1. Sunucu Seç

    1. Tümleştirme seçin

    1. Azure Sentinel'i etkinleştirme

    **Azure Sentinel ayarlarını kullanarak beSECURE sağlayın.**
      - *Çalışma alanı kimliği* ve *birincil anahtar* değerlerini Azure Sentinel bağlayıcı sayfasından kopyalayın, Besecure yapılandırmasına yapıştırın ve **Değiştir** ' e tıklayın.

## <a name="find-your-data"></a>Verilerinizi bulun

Başarılı bir bağlantı kurulduktan sonra, veriler **günlüklerde** , **customlogs** bölümünün altında aşağıdaki tablolardan bir veya daha fazlasına görünür:
  - `beSECURE_ScanResults_CL`
  - `beSECURE_ScanEvents_CL`
  - `beSECURE_Audit_CL`

Log Analytics beSECURE günlüklerini sorgulamak için, sorgu penceresinin en üstündeki yukarıdaki tablo adlarından birini girin.

## <a name="validate-connectivity"></a>Bağlantıyı doğrula
Günlüklerinizin Log Analytics görünmeye başlaması 20 dakikaya kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Sentinel 'e nasıl bağlanaleceğini öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .
