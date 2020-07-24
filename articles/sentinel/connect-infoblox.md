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
ms.openlocfilehash: c8af1c32607e5fc7bf548b40069e18923c666303
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87089660"
---
# <a name="connect-your-infoblox-nios-to-azure-sentinel"></a>Infoblox NIOS 'larınızı Azure Sentinel 'e bağlama

> [!IMPORTANT]
> Azure Sentinel 'deki Infoblox NIOS veri Bağlayıcısı Şu anda genel önizlemededir.
> Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

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