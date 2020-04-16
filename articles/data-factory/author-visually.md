---
title: Görsel yazma
description: Azure Veri Fabrikası'nda görsel yazma yı nasıl kullanacağınızı öğrenin
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: djpmsft
ms.author: daperlov
ms.reviewer: ''
manager: anandsub
ms.date: 12/19/2019
ms.openlocfilehash: e7de92878dac72470c0b65d1cf18c1a2d526a0bb
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81418499"
---
# <a name="visual-authoring-in-azure-data-factory"></a>Azure Veri Fabrikası'nda görsel yazma

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Azure Veri Fabrikası kullanıcı arabirimi deneyimi (UX), herhangi bir kod yazmak zorunda kalmadan veri fabrikanız için kaynakları görsel olarak yazmanızı ve dağıtmanızı sağlar. Etkinlikleri bir boru hattı tuvaline sürükleyebilir, test çalıştırmaları gerçekleştirebilir, yinelemeli hata ayıklayabilir ve ardışık hatlar çalışanlarınızı dağıtabilir ve izleyebilirsiniz.

Şu anda Azure Veri Fabrikası UX yalnızca Microsoft Edge ve Google Chrome'da desteklenir.

## <a name="authoring-canvas"></a>Tuval yazma

**Yazma tuvalini**açmak için kalem simgesine tıklayın. 

![Tuval Yazma](media/author-visually/authoring-canvas.png)

Burada, fabrikanızı oluşturan ardışık hatları, etkinlikleri, veri kümelerini, bağlantılı hizmetleri, veri akışlarını, tetikleyicileri ve tümleştirme çalışma sürelerini yazarsınız. Yazma tuvalini kullanarak bir ardışık yapı oluşturmaya başlamak için, [kopyalama Etkinliği'ni kullanarak verileri kopyala'ya](tutorial-copy-data-portal.md)bakın. 

Varsayılan görsel yazma deneyimi doğrudan Veri Fabrikası hizmetiyle çalışıyor. Azure Repos Git veya GitHub tümleştirmesi, veri fabrikası ardışık hatlarınızda çalışmak için kaynak denetimi ve işbirliğine olanak sağlamak için de desteklenir. Bu yazma deneyimleri arasındaki farklar hakkında daha fazla bilgi edinmek için [Azure Veri Fabrikası'nda Kaynak denetimine](source-control.md)bakın.

## <a name="expressions-and-functions"></a>İfadeler ve işlevler

İfadeler ve işlevler, Azure Veri Fabrikası'nda birçok özelliği belirtmek için statik değerler yerine kullanılabilir.

Özellik değeri için bir ifade belirtmek için **Dinamik İçerik Ekle'yi** seçin veya alana odaklanırken **Alt + P'yi** tıklatın.

![Dinamik İçerik Ekle](media/author-visually/dynamic-content-1.png)

Bu, desteklenen sistem değişkenlerinden, etkinlik çıktısından, işlevlerden ve kullanıcı tarafından belirtilen değişkenlerden veya parametrelerden ifadeler oluşturabileceğiniz **Veri Fabrikası İfade Oluşturucusu'nu** açar. 

![İfade oluşturucusu](media/author-visually/dynamic-content-2.png)

İfade dili hakkında bilgi için [Azure Veri Fabrikası'ndaki ifadeler ve işlevler'e](control-flow-expression-language-functions.md)bakın.

## <a name="provide-feedback"></a>Geri bildirimde bulunma

Özellikler hakkında yorum yapmak veya araçla ilgili sorunlar hakkında Microsoft'a bilgi vermek için **Geri Bildirim'i** seçin:

![Geri Bildirim](media/author-visually/provide-feedback.png)

## <a name="next-steps"></a>Sonraki adımlar

Boru hatlarını izleme ve yönetme hakkında daha fazla bilgi edinmek [için, denetim kanallarını programlı olarak izleyin ve yönetin.](monitor-programmatically.md)
