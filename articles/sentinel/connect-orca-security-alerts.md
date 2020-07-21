---
title: Orca uyarılarını Azure Sentinel 'e bağlama | Microsoft Docs
description: Orca güvenlik uyarısı verilerini Azure Sentinel 'e bağlamayı, panoları görüntülemeyi, özel uyarılar oluşturmayı ve araştırmayı geliştirmeyi öğrenin.
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
ms.date: 07/17/2020
ms.author: yelevin
ms.openlocfilehash: 1f2f67281079142f0c4fe4985738dc48332a107d
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86532265"
---
# <a name="connect-your-orca-alerts-to-azure-sentinel"></a>Orca uyarılarınızı Azure Sentinel 'e bağlama 

Orca güvenlik uyarıları Bağlayıcısı, [Orca uyarılarınızı](https://orca.security/) güvenlik çözüm uyarılarınızı Azure Sentinel 'e kolayca getirmenize olanak tanır, böylece bunları çalışma kitaplarında görüntüleyebilir, özel uyarılar oluşturmak için kullanabilir ve araştırmayı geliştirebilirsiniz. Orca güvenlik uyarıları ve Azure Sentinel arasında tümleştirme, REST API kullanımını sağlar.

> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="configure-and-connect-orca-security-alerts"></a>Orca güvenlik uyarılarını yapılandırma ve bağlama

Orca güvenlik uyarıları, günlükleri doğrudan Azure Sentinel 'e tümleştirebilir ve dışarı aktarabilir.

1. Azure Sentinel portalında, **veri bağlayıcıları** ' na tıklayın ve **Orca güvenlik uyarıları** ' nı seçin ve ardından **bağlayıcı sayfası**' nı açın.

2. https://orcasecurity.zendesk.com/hc/en-us/articles/360043941992-Azure-Sentinel-integrationOrca platformundan tümleştirmeyi tamamen gerçekleştirmek için bölümüne bakın.

## <a name="find-your-data"></a>Verilerinizi bulun

Başarılı bir bağlantı kurulduktan sonra, **OrcaAlerts_CL** tablosundaki **customlogs** altında Log Analytics veriler görüntülenir.
Orca uyarıları için Log Analytics ilgili şemayı kullanmak için arama yapın `OrcaAlerts_CL` .

## <a name="validate-connectivity"></a>Bağlantıyı doğrula
Günlüklerinizin Log Analytics görünene kadar 20 dakikadan bu kadar bir zaman çıkabilir. 


## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Orca güvenlik uyarılarını Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .

