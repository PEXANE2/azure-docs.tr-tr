---
title: Kablolu x Network Forensics platformunu (NFP) Azure Sentinel 'e bağlama | Microsoft Docs
description: Kablox sistem NFP verileri bağlayıcısını kullanarak, kablosuz NFP günlüklerini Azure Sentinel 'e çekmek için nasıl kullanacağınızı öğrenin. Çalışma kitaplarında kablolu x NFP verilerini görüntüleyin, uyarılar oluşturun ve araştırmayı geliştirebilirsiniz.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 0001cad6-699c-4ca9-b66c-80c194e439a5
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/12/2021
ms.author: yelevin
ms.openlocfilehash: 9029b945eabd05b34306393b513e26ee9c1563f2
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98541597"
---
# <a name="connect-your-wirex-network-forensics-platform-nfp-appliance-to-azure-sentinel"></a>Kablolu x Network Forensics platformunu (NFP) Azure Sentinel 'e bağlama

> [!IMPORTANT]
> Kablolu x sistemleri NFP Bağlayıcısı Şu anda **Önizleme** aşamasındadır. Beta, önizleme veya henüz genel kullanıma sunulmayan Azure özelliklerine uygulanan ek koşullar için [Microsoft Azure önizlemeleri için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) 'na bakın.

Bu makalede, kablolu x Systems Network Forensics platformu (NFP) gerecinizi Azure Sentinel 'e nasıl bağlayabileceğiniz açıklanır. Kablolu x NFP veri Bağlayıcısı, çalışma kitaplarında verileri görüntüleyebilmeniz, özel uyarılar oluşturmak için onu kullanarak ve araştırmayı iyileştirebilmeniz için Azure Sentinel ile NFP günlüklerini kolayca bağlamanıza olanak tanır. 

> [!NOTE] 
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="prerequisites"></a>Önkoşullar

- Azure Sentinel çalışma alanınızda okuma ve yazma izinlerine sahip olmanız gerekir.

- Çalışma alanı için paylaşılan anahtarlar için okuma izninizin olması gerekir.

## <a name="send-wirex-nfp-logs-to-azure-sentinel"></a>Kablolu x NFP günlüklerini Azure Sentinel 'e gönder

Günlüklerini Azure Sentinel 'e almak için, kablolu x Systems NFP gerecinizi, CEF biçiminde bir Linux tabanlı günlük iletme sunucusuna (rsyslog veya Syslog-ng çalıştıran) Syslog iletileri gönderecek şekilde yapılandırın. Bu sunucuda Log Analytics Aracısı yüklü olacaktır ve Aracı günlükleri Azure Sentinel çalışma alanınıza iletir.

1. Azure Sentinel gezinti menüsünde **veri bağlayıcıları**' nı seçin.

1. **Veri bağlayıcıları** galerisinden **kablolu x Network Forensics platformu (Önizleme)** öğesini seçin ve ardından **bağlayıcı sayfasını açın**.

1. **Yapılandırma** altında, **yönergeler** sekmesindeki yönergeleri izleyin:

    1. **1. Linux Syslog Aracısı yapılandırması** -çalışır durumda bir günlük ileticisi yoksa veya başka bir tane gerekiyorsa bu adımı izleyin. Daha ayrıntılı yönergeler ve açıklamalar için bkz. 1. Adım: Azure Sentinel belgelerindeki [günlük ileticisini dağıtma](connect-cef-agent.md) .

    1. **2. Syslog aracısına ortak olay biçimi (CEF) günlüklerini ilet** -kablolu x NFP çözümünüzün doğru yapılandırması Için [kablolu x desteğiyle](https://wirexsystems.com/contact-us/) iletişim kurun. Bu yapılandırma aşağıdaki öğeleri içermelidir:
        - Günlük hedefi – günlük iletme sunucunuzun ana bilgisayar adı ve/veya IP adresi
        - Protokol ve bağlantı noktası – TCP 514 (Aksi takdirde önerilir), günlük iletme sunucunuzdaki Syslog Daemon öğesinde paralel değişikliği yaptığınızdan emin olun.
        - Günlük biçimi – CEF
        - Günlük türleri – tüm kablolu x tarafından önerilen

    1. **3. bağlantıyı doğrulama** -bağlayıcı sayfasına komutu kopyalayarak ve günlük ileticide çalıştırarak veri alımını doğrulayın. Daha ayrıntılı yönergeler ve açıklamalar için bkz. [3. Adım:](connect-cef-verify.md) Azure Sentinel belgelerindeki bağlantıyı doğrulama.

        Günlüklerinizin Log Analytics görünmeye başlaması 20 dakikaya kadar sürebilir.

## <a name="find-your-data"></a>Verilerinizi bulun

Başarılı bir bağlantı kurulduktan sonra, veriler **günlüklerde**, **Azure Sentinel** bölümünde, *commonsecuritylog* tablosunda görünür.

Log Analytics ile kablolu x NFP verilerini sorgulamak için aşağıdakileri yaptığınız gibi diğer filtreleri uygulayarak sorgu penceresine kopyalayın:

```kusto
CommonSecurityLog 
| where DeviceVendor == "WireX"
```

Daha fazla sorgu örneği için bağlayıcı sayfasındaki **sonraki adımlar** sekmesine bakın.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, kablolu x sistemlerini NFP ile Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .