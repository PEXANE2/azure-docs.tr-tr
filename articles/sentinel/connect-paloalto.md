---
title: Palo Alto Networks verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Azure Sentinel ile Palo Alto ağları günlüklerine kolayca bağlanmak için Palo Alto Networks veri bağlayıcısını nasıl kullanacağınızı öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: a4b21d67-1a72-40ec-bfce-d79a8707b4e1
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: 245db436fc3216fe5c8c8f51c50c0ac03190f9eb
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85564553"
---
# <a name="connect-palo-alto-networks-to-azure-sentinel"></a>Palo Alto ağlarını Azure Sentinel 'e bağlama



Bu makalede, Palo Alto Networks gerecinizi Azure Sentinel 'e nasıl bağlayabileceğiniz açıklanır. Palo Alto Networks Data Connector, Azure Sentinel ile Palo Alto Networks günlüklerine kolayca bağlanmanızı, panoları görüntülemenizi, özel uyarılar oluşturmayı ve araştırmayı geliştirmeyi sağlar. Azure Sentinel 'de Palo Alto ağlarını kullanmak, kuruluşunuzun Internet kullanımına yönelik daha fazla öngörü sağlar ve güvenlik işlemi yeteneklerini geliştirir. 


## <a name="forward-palo-alto-networks-logs-to-the-syslog-agent"></a>Palo Alto Networks günlüklerini Syslog aracısına ilet

Azure çalışma alanınıza Syslog Aracısı aracılığıyla syslog iletilerini CEF biçiminde iletmek için Palo Alto ağlarını yapılandırın:
1.  [Ortak olay biçimi (CEF) yapılandırma kılavuzlarından](https://docs.paloaltonetworks.com/resources/cef) gidin ve gereç türü için PDF 'yi indirin. CEF olaylarını toplamak üzere Palo Alto Networks gerecinizi ayarlamak için kılavuzdaki tüm yönergeleri izleyin. 

1.  [Syslog Izlemesini yapılandırma](https://docs.paloaltonetworks.com/pan-os/8-1/pan-os-admin/monitoring/use-syslog-for-monitoring/configure-syslog-monitoring) bölümüne gidin ve Azure Sentinel 'e Palo Alto Networks GERECINDEN CEF olay iletmeyi yapılandırmak için adım 2 ve 3 ' ü izleyin.

    1. **Syslog Sunucu biçimini** **BSD**olarak ayarladığınızdan emin olun.

       > [!NOTE]
       > PDF 'den Kopyala/yapıştır işlemleri metni değiştirebilir ve rastgele karakterler ekleyebilir. Bunu önlemek için, metni bir düzenleyiciye kopyalayın ve bu örnekte görebileceğiniz gibi, yapıştırmadan önce günlük biçimini bozabilecek tüm karakterleri kaldırın.
 
        ![CEF metin kopyalama sorunu](./media/connect-cef/paloalto-text-prob1.png)

1. Palo Alto Networks olayları için Log Analytics ilgili şemayı kullanmak için, **Commonsecuritylog**' u arayın.

1. 3. [Adım: bağlantıyı doğrulama adımına](connect-cef-verify.md)geçin.




## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Palo Alto Networks gereçlerini Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .


