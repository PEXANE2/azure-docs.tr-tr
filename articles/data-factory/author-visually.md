---
title: Görsel yazma
description: Azure Data Factory 'de görsel yazma kullanmayı öğrenin
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: djpmsft
ms.author: daperlov
ms.reviewer: ''
manager: anandsub
ms.date: 12/19/2019
ms.openlocfilehash: 09d4055ba98da2dd87efc9421402f2827a87ba16
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75440927"
---
# <a name="visual-authoring-in-azure-data-factory"></a>Azure Data Factory 'de görsel yazma

Azure Data Factory Kullanıcı arabirimi deneyimi (UX), herhangi bir kod yazmak zorunda kalmadan veri fabrikanızın kaynaklarını görsel olarak yazmanızı ve dağıtmanızı sağlar. Etkinlikleri bir işlem hattı tuvaline sürükleyebilir, test çalıştırmaları gerçekleştirebilir, tekrarlayarak hata ayıklayın ve işlem hattı çalıştırmalarınızın dağıtımını yapabilir ve izleyebilirsiniz.

Şu anda Azure Data Factory UX yalnızca Microsoft Edge ve Google Chrome 'da desteklenir.

## <a name="authoring-canvas"></a>Yazı tuvali

**Yazma tuvalini**açmak için kalem simgesine tıklayın. 

![Yazı tuvali](media/author-visually/authoring-canvas.png)

Burada, fabrikanızı oluşturan işlem hatları, Etkinlikler, veri kümeleri, bağlı hizmetler, veri akışları, Tetikleyiciler ve tümleştirme çalışma zamanları yazacaktır. Yazma tuvali 'ni kullanarak bir işlem hattı oluşturmaya başlamak için bkz. [kopyalama etkinliğini kullanarak verileri kopyalama](tutorial-copy-data-portal.md). 

Varsayılan görsel yazma deneyimi, Data Factory hizmetiyle doğrudan çalışır. Veri Fabrikası işlem hatlarınız üzerinde çalışma için kaynak denetimi ve işbirliği yapmak üzere Azure Repos git veya GitHub tümleştirmesi de desteklenir. Bu yazma deneyimleri arasındaki farklar hakkında daha fazla bilgi edinmek için [Azure Data Factory Içindeki kaynak denetimi](source-control.md)konusuna bakın.

## <a name="expressions-and-functions"></a>İfadeler ve işlevler

Azure Data Factory ' de birçok özellik belirtmek için, statik değerler yerine ifadeler ve işlevler kullanılabilir.

Bir özellik değeri için bir ifade belirtmek üzere, **dinamik Içerik Ekle** ' yi seçin veya alana odaklanırken **alt + P** ' ye tıklayın.

![Dinamik Içerik Ekle](media/author-visually/dynamic-content-1.png)

Bu, desteklenen sistem değişkenlerinden, etkinlik çıktısından, işlevlerden ve Kullanıcı tarafından belirtilen değişkenlerle ya da parametrelerden ifade oluşturabileceğiniz **Data Factory Ifade oluşturucusunu** açar. 

![İfade oluşturucusu](media/author-visually/dynamic-content-2.png)

İfade dili hakkında daha fazla bilgi için, [Azure Data Factory Içindeki ifadeler ve işlevler](control-flow-expression-language-functions.md)bölümüne bakın.

## <a name="provide-feedback"></a>Geri bildirim sağlayın

Özellikler hakkında yorum yapmak veya araçla ilgili sorunları bildirmek için **geri bildirim** ' ı seçin:

![Geri Bildirim](media/author-visually/provide-feedback.png)

## <a name="next-steps"></a>Sonraki adımlar

İşlem hatlarını izleme ve yönetme hakkında daha fazla bilgi edinmek için bkz. işlem [hatlarını programlama yoluyla izleme ve yönetme](monitor-programmatically.md).
