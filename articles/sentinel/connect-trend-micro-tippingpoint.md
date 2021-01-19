---
title: Eğilim mikro TippingPoint 'ı Azure Sentinel 'e bağlama | Microsoft Docs
description: Azure Sentinel 'e TippingPoint SMS günlüklerini çekmek için Trend mikro TippingPoint veri bağlayıcısını nasıl kullanacağınızı öğrenin. Çalışma kitaplarında TippingPoint verilerini görüntüleyin, uyarılar oluşturun ve araştırmayı yapın.
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
ms.openlocfilehash: 989520e079988e1821d8bb9a936f857e1f62c11a
ms.sourcegitcommit: ca215fa220b924f19f56513fc810c8c728dff420
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2021
ms.locfileid: "98567758"
---
# <a name="connect-your-trend-micro-tippingpoint-solution-to-azure-sentinel"></a>Trend mikro TippingPoint çözümünüzü Azure Sentinel 'e bağlama

> [!IMPORTANT]
> Trend mikro TippingPoint Bağlayıcısı Şu anda **Önizleme** aşamasındadır. Beta, önizleme veya henüz genel kullanıma sunulmayan Azure özelliklerine uygulanan ek koşullar için [Microsoft Azure önizlemeleri için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) 'na bakın.

Bu makalede, Trend mikro TippingPoint tehdit koruması sistem çözümünüzün Azure Sentinel 'e nasıl bağlanacağı açıklanır. Trend mikro TippingPoint veri Bağlayıcısı, çalışma kitaplarında verileri görüntüleyebilmeniz, özel uyarılar oluşturmak için onu kullanarak ve araştırmayı iyileştirebilmeniz için, Azure Sentinel ile TippingPoint güvenlik yönetimi sistemi (SMS) günlüklerinizi kolayca bağlamanıza olanak tanır. 

> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="prerequisites"></a>Ön koşullar

- Azure Sentinel çalışma alanınızda okuma ve yazma izinlerine sahip olmanız gerekir.

- Çalışma alanı için paylaşılan anahtarlar için okuma izninizin olması gerekir.

## <a name="send-trend-micro-tippingpoint-logs-to-azure-sentinel"></a>Eğilim mikro TippingPoint günlüklerini Azure Sentinel 'e gönder

Günlüklerini Azure Sentinel 'e almak için, TippingPoint TPS çözümünüzü, CEF biçiminde bir Linux tabanlı günlük iletme sunucusuna (rsyslog veya Syslog-ng çalıştıran) Syslog iletileri gönderecek şekilde yapılandırın. Bu sunucuda Log Analytics Aracısı yüklü olacaktır ve Aracı günlükleri Azure Sentinel çalışma alanınıza iletir.

1. Azure Sentinel gezinti menüsünde **veri bağlayıcıları**' nı seçin.

1. **Veri bağlayıcıları** galerisinden, **Trend mikro TippingPoint (Önizleme)** öğesini seçin ve ardından **bağlayıcı sayfasını açın**.

1. **Yapılandırma** altında, **yönergeler** sekmesindeki yönergeleri izleyin:

    1. **1 altında. Linux Syslog Aracısı yapılandırması** -çalışır durumda bir günlük ileticisi yoksa veya başka bir tane gerekiyorsa bu adımı izleyin. Daha ayrıntılı yönergeler ve açıklamalar için bkz. 1. Adım: Azure Sentinel belgelerindeki [günlük ileticisini dağıtma](connect-cef-agent.md) .

    1. **2 altında. İleriye doğru trend mikro TippingPoint SMS günlükleri Syslog aracısına ilet** -bu yapılandırma aşağıdaki öğeleri içermelidir:
        - Günlük hedefi – günlük iletme sunucunuzun ana bilgisayar adı ve/veya IP adresi
        - Protokol ve bağlantı noktası – **TCP 514** (Aksi takdirde önerilir), günlük iletme sunucunuzdaki Syslog Daemon öğesinde paralel değişikliği yaptığınızdan emin olun.
        - Günlük biçimi – **Arcgörüş CEF Format v 4.2**
        - Günlük türleri: tümü kullanılabilir

    1. 3 ' ün altında **. Bağlantıyı doğrulama** -bağlayıcı sayfasına komutu kopyalayarak ve günlük ileticisinde çalıştırarak veri alımını doğrulayın. Daha ayrıntılı yönergeler ve açıklamalar için bkz. [3. Adım:](connect-cef-verify.md) Azure Sentinel belgelerindeki bağlantıyı doğrulama.

        Günlüklerinizin Log Analytics görünmeye başlaması 20 dakikaya kadar sürebilir.

## <a name="find-your-data"></a>Verilerinizi bulun

Başarılı bir bağlantı kurulduktan sonra, veriler **günlüklerde**, **Azure Sentinel** bölümünde, *commonsecuritylog* tablosunda görünür.

Log Analytics 'de TrendMicro TippingPoint verilerini sorgulamak için, aşağıdakileri yaptığınız gibi diğer filtreleri uygulayarak sorgu penceresine kopyalayın:

```kusto
CommonSecurityLog 
| where DeviceVendor == "TrendMicroTippingPoint"
```

Daha fazla sorgu örneği için bağlayıcı sayfasındaki **sonraki adımlar** sekmesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Trend mikro TippingPoint 'ı Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .