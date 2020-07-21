---
title: Symantec ProxySG verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Symantec ProxySG verilerini Azure Sentinel 'e bağlamayı öğrenin.
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
ms.openlocfilehash: 51a5c5de7bce07aa6a54b9ff81e957c38cfffdd2
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86532026"
---
# <a name="connect-your-symantec-proxysg-to-azure-sentinel"></a>Symantec ProxySG 'nizi Azure Sentinel 'e bağlama

Bu makalede [Symantec ProxySG](https://www.broadcom.com/products/cyber-security/network/gateway/proxy-sg-and-advanced-secure-gateway) gerecinizi Azure Sentinel 'e nasıl bağlayabileceğiniz açıklanır. Symantec ProxySG veri Bağlayıcısı, Azure Sentinel ile Symantec ProxySG günlüklerinizi kolayca bağlamanıza olanak tanır, panoları görüntüleyebilir, özel uyarılar oluşturabilir ve araştırmayı geliştirebilirsiniz. Symantec ProxySG ve Azure Sentinel arasındaki tümleştirme Syslog kullanımını sağlar.

> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="forward-symantec-proxysg-logs-to-the-syslog-agent"></a>Symantec ProxySG günlüklerini Syslog aracısına ilet  

Syslog Aracısı aracılığıyla syslog iletilerini Azure çalışma alanınıza iletmek için Symantec ProxySG öğesini yapılandırın.

1. Azure Sentinel portalında, **veri bağlayıcıları** ' na tıklayın ve **Symantec proxysg** bağlayıcısını seçin.

1. **Bağlayıcı sayfasını aç**' ı seçin.

1. **Symantec ProxySG** sayfasındaki yönergeleri izleyin.

## <a name="find-your-data"></a>Verilerinizi bulun

Başarılı bir bağlantı kurulduktan sonra, veriler Syslog altında Log Analytics görüntülenir.

## <a name="validate-connectivity"></a>Bağlantıyı doğrula

Günlüklerinizin Log Analytics görünene kadar 20 dakikadan bu kadar bir zaman çıkabilir. 

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede Symantec ProxySG 'yi Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .