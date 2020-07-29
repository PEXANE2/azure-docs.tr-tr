---
title: Zyium mobil tehdit savunması 'nı Azure Sentinel 'e bağlama | Microsoft Docs
description: Zkusurda mobil tehdit savunması 'nı Azure Sentinel 'e bağlamayı öğrenin.
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77587949"
---
# <a name="connect-your-zimperium-mobile-threat-defense-to-azure-sentinel"></a>Zkusurum mobil tehdit savunmanızı Azure Sentinel 'e bağlama


> [!IMPORTANT]
> Azure Sentinel 'deki Zyium mobil tehdit savunma verileri Bağlayıcısı Şu anda genel önizleme aşamasındadır.
> Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).



Zsayıen ium Mobile Threat Defense Bağlayıcısı, panoları görüntülemek, özel uyarılar oluşturmak ve araştırmayı geliştirmek için Azure Sentinel ile Zsayımlar olan tehdit günlüğünü bağlamanıza olanak tanır. Bu sayede kuruluşunuzun mobil tehdit yataya ilişkin daha fazla öngörü elde edersiniz ve güvenlik işlemi yeteneklerini geliştirir.

> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="configure-and-connect-zimperium-mobile-threat-defense"></a>Zyium Mobile Threat Defense 'ı yapılandırma ve bağlama

Zyium mobil tehdit savunması, günlükleri doğrudan **Azure Sentinel**'e tümleştirebilir ve dışarı aktarabilir.

1. Azure Sentinel portalında, veri bağlayıcıları ' na tıklayın ve **Zkusurda mobil tehdit savunması**' nı seçin.
2. **Bağlayıcı sayfasını aç**' ı seçin.
3. Aşağıdaki gibi özetlenen **Zleneen IUM Mobile Threat** Defense bağlayıcı sayfasındaki yönergeleri izleyin.
 1. ZConsole ' da, gezinti çubuğunda **Yönet** ' e tıklayın.
 2. **Tümleştirmeler** sekmesine tıklayın.
 3. **Tehdit raporlama** düğmesine ve ardından **tümleştirme Ekle** düğmesine tıklayın.
 4. Kullanılabilir tümleştirmelerle **Microsoft Azure Sentinel** ' i seçerek tümleştirmeyi oluşturun ve Azure Sentinel 'e bağlanmak için çalışma alanı kimliği ve birincil anahtar girin.
 5. Tehdit verilerinin Azure Sentinel 'e gönderimi için bir filtre düzeyi seçme seçeneği de mevcuttur. 

4. Daha fazla bilgi için lütfen [Zkusuren ium müşteri destek portalına](https://support.zimperium.com)bakın.


## <a name="find-your-data"></a>Verilerinizi bulun

Başarılı bir bağlantı kurulduktan sonra, veriler CustomLogs ZimperiumThreatLog_CL ve ZimperiumMitigationLog_CL altında Log Analytics görüntülenir.

Zkusurda mobil tehdit savunması için Log Analytics ilgili şemayı kullanmak için, ZimperiumThreatLog_CL ve ZimperiumMitigationLog_CL arayın.


## <a name="validate-connectivity"></a>Bağlantıyı doğrula

Günlüklerinizin Log Analytics görünene kadar 20 dakikadan bu kadar bir zaman çıkabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Zumium mobil tehdit savunması 'nı Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.

- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.

- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .

Zkusurium hakkında daha fazla bilgi için aşağıdakilere bakın:

- [Zimperium](https://zimperium.com)

- [Zyium mobil güvenlik blogu](https://blog.zimperium.com)

- [Zyium müşteri destek portalı](https://support.zimperium.com)

