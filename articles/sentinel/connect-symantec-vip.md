---
title: Symantec VIP verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Symantec VIP verilerini Azure Sentinel 'e bağlamayı öğrenin.
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
ms.openlocfilehash: c7429108f70d735cb6e314a0d4daa27ba0d31637
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "100090420"
---
# <a name="connect-your-symantec-vip-to-azure-sentinel"></a>Symantec VIP 'nizi Azure Sentinel 'e bağlama

> [!IMPORTANT]
> Azure Sentinel 'deki Symantec VIP veri Bağlayıcısı Şu anda genel önizlemededir.
> Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu makalede [Symantec VIP](https://vip.symantec.com/) gerecinizi Azure Sentinel 'e nasıl bağlayabileceğiniz açıklanır. Symantec VIP veri Bağlayıcısı, Azure Sentinel ile Symantec VIP günlüklerinizi kolayca bağlamanıza olanak tanır, panoları görüntüleyebilir, özel uyarılar oluşturabilir ve araştırmayı geliştirebilirsiniz. Symantec VIP ve Azure Sentinel arasındaki tümleştirme Syslog kullanımını kullanır.

> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="forward-symantec-vip-logs-to-the-syslog-agent"></a>Symantec VIP günlüklerini Syslog aracısına ilet  

Syslog Aracısı aracılığıyla syslog iletilerini Azure çalışma alanınıza iletmek için Symantec VIP 'i yapılandırın.

1. Azure Sentinel portalında, **veri bağlayıcıları** ' na tıklayın ve **Symantec VIP** Bağlayıcısı ' nı seçin.

1. **Bağlayıcı sayfasını aç**' ı seçin.

1. **Symantec VIP** sayfasındaki yönergeleri izleyin.

## <a name="find-your-data"></a>Verilerinizi bulun

Başarılı bir bağlantı kurulduktan sonra, veriler Syslog altında Log Analytics görüntülenir.

## <a name="validate-connectivity"></a>Bağlantıyı doğrula

Günlüklerinizin Log Analytics görünmeye başlaması 20 dakikaya kadar sürebilir. 

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede Symantec VIP günlüklerini Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .