---
title: Arama Darbeli bağlantı güvenli verileri Azure Sentinel 'e bağla | Microsoft Docs
description: Pulse Connect güvenli verileri Azure Sentinel 'e bağlamayı öğrenin.
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
ms.openlocfilehash: 01d41b20c543262280e215cf4e31fd0cccce877f
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86531972"
---
# <a name="connect-your-pulse-connect-secure-to-azure-sentinel"></a>Azure Sentinel 'de Pulse Connect 'i güvenli hale getirin

Bu makalede, [Pulse Connect güvenli](https://www.pulsesecure.net/products/pulse-connect-secure/) Gerecinizin Azure Sentinel 'e nasıl bağlanacağı açıklanır. Pulse Connect Secure Data Connector, Azure Sentinel ile Pulse Connect güvenli günlüklerinizi kolayca bağlamanıza olanak tanır, panoları görüntüleyebilir, özel uyarılar oluşturabilir ve araştırmayı geliştirebilirsiniz. Pulse Connect Secure ve Azure Sentinel arasında tümleştirme, syslog kullanımını sağlar.

> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="forward-pulse-connect-secure-logs-to-the-syslog-agent"></a>İlet Pulse güvenli günlükleri Syslog aracısına bağla  

Syslog iletilerini Syslog Aracısı aracılığıyla Azure çalışma alanınıza iletmek için Pulse Connect 'i yapılandırın.

1. Azure Sentinel portalında, **veri bağlayıcıları** ' na tıklayın ve **Pulse Connect güvenli** Bağlayıcısı ' nı seçin.

1. **Bağlayıcı sayfasını aç**' ı seçin.

1. **Pulse Connect Secure** sayfasındaki yönergeleri izleyin.

## <a name="find-your-data"></a>Verilerinizi bulun

Başarılı bir bağlantı kurulduktan sonra, veriler Syslog altında Log Analytics görüntülenir.

## <a name="validate-connectivity"></a>Bağlantıyı doğrula

Günlüklerinizin Log Analytics görünene kadar 20 dakikadan bu kadar bir zaman çıkabilir. 

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Pulse Connect 'in güvenliğini Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .