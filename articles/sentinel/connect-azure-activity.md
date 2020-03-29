---
title: Azure Etkinliği verilerini Azure Sentinel'e bağlayın | Microsoft Dokümanlar
description: Azure Etkinliği verilerini Azure Sentinel'e nasıl bağlayabilirsiniz öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 8c25baa8-b93b-41da-9e6c-15bb7b5c5511
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/22/2020
ms.author: yelevin
ms.openlocfilehash: 82dfcaf3394703aae531c828a1b96ad290bab798
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80124980"
---
# <a name="connect-data-from-azure-activity-log"></a>Azure Etkinliği günlüğünden veri bağlama

Günlükleri [Azure Etkinliği'nden](../azure-monitor/platform/platform-logs-overview.md) tek bir tıklamayla Azure Sentinel'e aktarabilirsiniz. Etkinlik günlüğü, Azure Kaynak Yöneticisi operasyonel verilerinden Hizmet Durumu etkinliklerindeki güncelleştirmelere kadar Azure genelinde abonelik düzeyinde ki olayları kaydeden ve görüntüleyen bir abonelik günlüğüdür. Etkinlik günlüğünü kullanarak, aboneliğinizdeki kaynaklarüzerinde gerçekleştirilen herhangi bir yazma işlemi (PUT, POST, DELETE) için 'ne, kim ve ne zaman' gerçekleştirilmesini belirleyebilirsiniz. Ayrıca işlemin durumunu ve diğer ilgili özellikleri de öğrenebilirsiniz. Etkinlik günlüğü, Klasik/"RDFE" modelini kullanan kaynaklar için okuma (GET) işlemlerini veya işlemleri içermez. 

## <a name="prerequisites"></a>Ön koşullar

- Kullanıcınızın Log Analytics çalışma alanına Katkıda Bulunan izinleri olmalıdır.
- Kullanıcınızın, günlüklerini Azure Sentinel'e aktarmak istediğiniz herhangi bir abonelik için Reader izinlerine sahip olması gerekir.

## <a name="set-up-the-azure-activity-connector"></a>Azure Etkinliği bağlayıcısını ayarlama

1. Azure Sentinel gezinti menüsünden **Veri bağlayıcılarını**seçin. Bağlayıcılar listesinden Azure **Etkinliği'ni**ve ardından sağ alttaki **Açık bağlayıcı sayfası** düğmesini tıklatın.

2. **Yönergeler** sekmesinin altında, **Azure Etkinliğini Yapılandır>** bağlantısını tıklatın.

3. Azure **Etkinliği günlüğü** bölmesinde, günlüklerini Azure Sentinel'e aktarmak istediğiniz abonelikleri seçin. 

4. Sağa açılan abonelik bölmesinde **Bağlan'ı**tıklatın.

5. Azure Etkinliği uyarıları için Günlük Analizi'nde ilgili şemayı `AzureActivity` kullanmak için sorgu penceresine yazın.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Etkinliği günlüğünü Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Yerleşik veya [özel](tutorial-detect-threats-custom.md) kurallar kullanarak Azure Sentinel ile tehditleri [algılamaya](tutorial-detect-threats-built-in.md) başlayın.
