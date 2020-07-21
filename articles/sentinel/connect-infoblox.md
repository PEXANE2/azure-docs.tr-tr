---
title: Infoblox ağ kimliği Işletim sistemi (NIOS) verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Infoblox ağ kimliği Işletim sistemi (NIOS) verilerini Azure Sentinel 'e bağlamayı öğrenin.
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
ms.openlocfilehash: ed4f2d769dbda3dec7b353fddfd1e5e0f3d00f9b
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86532175"
---
# <a name="connect-your-infoblox-nios-to-azure-sentinel"></a>Infoblox NIOS 'larınızı Azure Sentinel 'e bağlama

Bu makalede, [Infoblox ağ kimliği Işletim sistemi (NIOS)](https://www.infoblox.com/glossary/network-identity-operating-system-nios/) gerecinizi Azure Sentinel 'e nasıl bağlayabileceğiniz açıklanır. Infoblox NIOS veri Bağlayıcısı, Azure Sentinel ile Naoblox günlüklerinizi kolayca bağlamanıza olanak tanır, panoları görüntüleyebilir, özel uyarılar oluşturabilir ve araştırmayı geliştirebilirsiniz. Infoblox NIOS ve Azure Sentinel arasındaki tümleştirme Syslog kullanımını sağlar.

> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="forward-infoblox-logs-to-the-syslog-agent"></a>Infoblox günlüklerini Syslog aracısına ilet  

Syslog Aracısı aracılığıyla syslog iletilerini Azure çalışma alanınıza iletmek için Infoblox ' i yapılandırın.

1. Azure Sentinel portalında, **veri bağlayıcıları** ' na tıklayın ve **Infoblox Nios** Bağlayıcısı ' nı seçin.

1. **Bağlayıcı sayfasını aç**' ı seçin.

1. **Infoblox NIOS** sayfasındaki yönergeleri izleyin.

## <a name="find-your-data"></a>Verilerinizi bulun

Başarılı bir bağlantı kurulduktan sonra, veriler Syslog altında Log Analytics görüntülenir.

## <a name="validate-connectivity"></a>Bağlantıyı doğrula

Günlüklerinizin Log Analytics görünene kadar 20 dakikadan bu kadar bir zaman çıkabilir. 

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Infoblox NIOS 'ı Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .