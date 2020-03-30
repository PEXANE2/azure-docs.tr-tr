---
title: Zimperium Mobil Tehdit Savunmasını Azure Sentinel'e bağlayın| Microsoft Dokümanlar
description: Zimperium Mobil Tehdit Savunması'nı Azure Sentinel'e nasıl bağlayabilirsiniz öğrenin.
services: sentinel
author: yelevin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/20/2020
ms.author: yelevin
ms.openlocfilehash: 86854fa22a49f09e5d3d2fc5fdb53c245850fbac
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77587949"
---
# <a name="connect-your-zimperium-mobile-threat-defense-to-azure-sentinel"></a>Zimperium Mobil Tehdit Savunmanızı Azure Sentinel'e bağlayın


> [!IMPORTANT]
> Azure Sentinel'deki Zimperium Mobile Threat Defense veri bağlayıcısı şu anda genel önizlemededir.
> Bu özellik bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.



Zimperium Mobil Tehdit Savunması konektörü, panoları görüntülemek, özel uyarılar oluşturmak ve araştırmayı iyileştirmek için Zimperium tehdit günlüğünü Azure Sentinel'e bağlama nızı sağlar. Bu, kuruluşunuzun mobil tehdit ortamı hakkında daha fazla bilgi verir ve güvenlik işlem yeteneklerinizi geliştirir.

> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="configure-and-connect-zimperium-mobile-threat-defense"></a>Zimperium Mobil Tehdit Savunması'nı yapılandırın ve bağlayın

Zimperium Mobil Tehdit Savunması, günlükleri doğrudan **Azure Sentinel'e**entegre edebilir ve dışa aktarabilir.

1. Azure Sentinel portalında Veri bağlayıcılarını tıklatın ve **Zimperium Mobil Tehdit Savunması'nı**seçin.
2. **Bağlayıcıyı Aç'ı**seçin.
3. Aşağıdaki gibi özetlenen **Zimperium Mobil Tehdit Savunma** konektör sayfasındaki talimatları izleyin.
 1. zConsole'da, gezinme çubuğunda **Yönet'i** tıklatın.
 2. **Tümleştirmeler** sekmesini tıklatın.
 3. Tehdit **Raporlama** düğmesini ve ardından **Tümleştirmeler Ekle** düğmesini tıklatın.
 4. Kullanılabilir tümleştirmelerden **Microsoft Azure Sentinel'i** seçerek Tümleştirme'yi oluşturun ve Azure Sentinel'e bağlanmak için çalışma alanı kimliği ve birincil anahtarı girin.
 5. Tehdit verilerinin Azure Sentinel'e taşımak için filtre düzeyi seçme seçeneği de mevcuttur. 

4. Daha fazla bilgi için lütfen [Zimperium müşteri destek portalına](https://support.zimperium.com)bakın.


## <a name="find-your-data"></a>Verilerinizi bulun

Başarılı bir bağlantı kurulduktan sonra veriler, CustomLogs ZimperiumThreatLog_CL ve ZimperiumMitigationLog_CL altında Log Analytics'te görünür.

Zimperium Mobil Tehdit Savunması için Log Analytics'teki ilgili şemayı kullanmak için ZimperiumThreatLog_CL ve ZimperiumMitigationLog_CL arayın.


## <a name="validate-connectivity"></a>Bağlantıyı doğrulama

Günlüklerinizin Log Analytics'te görünmeye başlaması 20 dakikadan fazla sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Zimperium Mobil Tehdit Savunması'nı Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.

- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.

- Verilerinizi izlemek için [çalışma kitaplarını kullanın.](tutorial-monitor-your-data.md)

Zimperium hakkında daha fazla bilgi edinmek için aşağıdakilere bakın:

- [Zimperium](https://zimperium.com)

- [Zimperium Mobil Güvenlik Blog](https://blog.zimperium.com)

- [Zimperium Müşteri Destek Portalı](https://support.zimperium.com)

