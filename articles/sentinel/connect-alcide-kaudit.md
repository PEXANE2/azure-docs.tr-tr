---
title: Alcide Kaudıt verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Alcide Kaudıt verilerini Azure Sentinel 'e bağlamayı öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/21/2020
ms.author: yelevin
ms.openlocfilehash: 121c9258353505b6147aa059d327bc711e9fede4
ms.sourcegitcommit: bf8c447dada2b4c8af017ba7ca8bfd80f943d508
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85368461"
---
# <a name="connect-your-alcide-kaudit-to-azure-sentinel"></a>Alcide Kaudıt 'nizi Azure Sentinel 'e bağlama

[Alcide Kaudıt](https://www.alcide.io/kaudit-K8s-forensics/) , anormal Kubernetes davranışlarını belirlemenize ve algılama süresini azaltırken Kubernetes ihlallerine ve olaylarına odaklanmanıza yardımcı olur. Bu makalede, Alcide Kaudıt çözümünüzü Azure Sentinel 'e nasıl bağlayabileceğiniz açıklanır. Alcide Kaudıt veri Bağlayıcısı, Kauıdıt günlük verilerinizi Azure Sentinel 'e kolayca getirmenize olanak tanır. böylece, çalışma kitaplarında görüntüleyebilmeniz için özel uyarılar oluşturabilir ve bunu araştırmayı geliştirebilirsiniz. Alcide Kaudıt ve Azure Sentinel arasındaki tümleştirme REST API kullanır.

> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="prerequisites"></a>Ön koşullar

- Azure Sentinel çalışma alanında okuma ve yazma izinlerine sahip olmanız gerekir.

- Çalışma alanı için paylaşılan anahtarlar için okuma izninizin olması gerekir.

## <a name="configure-and-connect-alcide-kaudit"></a>Alcide Kaudıt 'yi yapılandırma ve bağlama

Alcide Kaudıt, günlükleri doğrudan Azure Sentinel 'e aktarabilir.

1. Azure Sentinel portalında, gezinti menüsünde **veri bağlayıcıları** ' na tıklayın.

1. Galeriden **Alcide Kaudıt** ' i seçin ve ardından **bağlayıcı sayfasını aç** düğmesine tıklayın.

1. [Alcide Kaudıt yükleme kılavuzunda](https://get.alcide.io/hubfs/Azure%20Sentinel%20Integration%20with%20kAudit.pdf)sunulan adım adım yönergeleri izleyin.

1. Çalışma alanı KIMLIĞI ve birincil anahtar sorulduğunda, bunları Alcide Kaudıt veri bağlayıcı sayfasından kopyalayabilirsiniz.

    :::image type="content" source="media/connect-alcide-kaudit/alcide-workspace-id-primary-key.png" alt-text="Çalışma alanı KIMLIĞI ve birincil anahtar":::

## <a name="find-your-data"></a>Verilerinizi bulun

Başarılı bir bağlantı kurulduktan sonra, veriler **Customlogs**'daki şu veri türleri altında **günlüklerde** görünür:

- **alcide_kaudit_detections_1_CL** -Alcide Kaudıt algılamaları 
- **alcide_kaudit_activity_1_CL** -Alcide Kaudıt etkinlik günlükleri
- **alcide_kaudit_selections_count_1_CL** -Alcide Kaudıt etkinlik sayıları
- **alcide_kaudit_selections_details_1_CL** -Alcide Kaudıt etkinlik ayrıntıları

Alcide Kaudıt için günlüklerde ilgili şemayı kullanmak için yukarıda belirtilen veri türlerini arayın.

Günlüklerinizin Log Analytics görünene kadar 20 dakikadan bu kadar bir zaman çıkabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Alcıde Kaudıt 'yi Azure Sentinel 'e bağlamayı öğrendiniz. Bu veri bağlayıcısında yerleşik olarak bulunan özelliklerden tam olarak yararlanabilmek için, veri bağlayıcı sayfasındaki **sonraki adımlar** sekmesine tıklayın. Faydalı bilgiler bulmaya başlayabilmeniz için bazı kullanıma yönelik örnek sorgular bulacaksınız.

Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .
