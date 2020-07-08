---
title: AI Vektöri algılama verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: AI Vektörte algılama verilerini Azure Sentinel 'e bağlamayı öğrenin.
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
ms.date: 06/21/2020
ms.author: yelevin
ms.openlocfilehash: 964bab2b81f6ce69e77fd69cab55f6d1894e4edb
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85368467"
---
# <a name="connect-ai-vectra-detect-to-azure-sentinel"></a>AI Vektöri algılamasına Azure Sentinel 'e bağlama

Bu makalede, [AI Vektörte algılama](https://www.vectra.ai/product/cognito-detect) gerecinizi Azure Sentinel 'e bağlama açıklanmaktadır. AI vektörler Data Connector, AI vektörler verilerini Azure Sentinel 'e kolayca getirmenize olanak tanır. böylece, çalışma kitaplarında görüntüleyebilmeniz için, özel uyarılar oluşturmak için onu kullanabilir ve araştırmayı geliştirebilirsiniz.

> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="configure-your-ai-vectra-detect-appliance-to-send-cef-messages"></a>CEF iletileri göndermek için AI Vectra algılaması gerecinizi yapılandırma  

CEF biçimli syslog iletilerini, [1. Adım: günlük Ileticisini dağıtma](connect-cef-agent.md)bölümünde ayarladığınız Syslog ileticisi aracılığıyla Log Analytics çalışma alanınıza ILETMEK Için AI vektörsel algılama ' yı yapılandırın.

1. Vectra arabiriminden ayarlar > bildirimler ' e gidin ve Syslog yapılandırmasını düzenle ' yi seçin. Bağlantıyı kurmak için aşağıdaki yönergeleri izleyin:

    - Yeni bir hedef ekleyin ( [günlük ileticinin](connect-cef-agent.md)ana bilgisayar adı)
    - Bağlantı noktasını **514** olarak ayarlayın
    - Protokolü **UDP** olarak ayarlama
    - Biçimi **CEF** olarak ayarlayın
    - Günlük türlerini ayarla (kullanılabilir tüm günlük türlerini seç)
    - **Kaydet** 'e tıklayın

2. **Test** düğmesine tıklayarak bazı test olaylarının günlük ileticiye gönderilmesini zorlayabilirsiniz.

3. AI Vektörli algılama olayları için Log Analytics ilgili şemayı kullanmak için, **Commonsecuritylog**' u arayın.

4. 3. [Adım: bağlantıyı doğrulama adımına](connect-cef-verify.md)geçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, AI Vectra gereçlerini Azure Sentinel 'e bağlamayı öğrendiniz. Bu veri bağlayıcısında yerleşik olarak bulunan özelliklerden tam olarak yararlanabilmek için, veri bağlayıcı sayfasındaki **sonraki adımlar** sekmesine tıklayın. Faydalı bilgiler bulmaya başlayabilmeniz için bazı kullanıma yönelik örnek sorgular bulacaksınız.

Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .
