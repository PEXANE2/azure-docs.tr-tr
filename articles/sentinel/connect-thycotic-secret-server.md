---
title: TH, Cotik gizli sunucuyu Azure Sentinel 'e bağlama | Microsoft Docs
description: Thincotik gizli sunucu günlüklerini Azure Sentinel 'e çekmek için Thimcotik gizli sunucu veri bağlayıcısını nasıl kullanacağınızı öğrenin. Çalışma kitaplarında gizli olmayan gizli sunucu verilerini görüntüleyin, uyarı oluşturun ve araştırmayı geliştirebilirsiniz.
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
ms.date: 12/13/2020
ms.author: yelevin
ms.openlocfilehash: a0056391dddec25511deb7033d37f59db3d835c8
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "98567987"
---
# <a name="connect-your-thycotic-secret-server-to-azure-sentinel"></a>TH, Cotik gizli sunucunuzu Azure Sentinel 'e bağlama

> [!IMPORTANT]
> Thincotik gizli sunucu Bağlayıcısı Şu anda **Önizleme** aşamasındadır. Beta, önizleme veya henüz genel kullanıma sunulmayan Azure özelliklerine uygulanan ek koşullar için [Microsoft Azure önizlemeleri için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) 'na bakın.

Bu makalede, TH, Cotik gizli sunucu gerecinizi Azure Sentinel 'e nasıl bağlayabileceğiniz açıklanır. Thincotik gizli sunucu verileri Bağlayıcısı, çalışma kitaplarında verileri görüntüleyebilmeniz, özel uyarılar oluşturmak için onu kullanarak ve araştırmayı iyileştirebilmeniz için Azure Sentinel ile bağımsız gizli sunucu günlüklerinizi kolayca bağlamanıza olanak tanır. Thintik ve Azure Sentinel arasındaki tümleştirme, gizli sunucu syslog iletilerini doğru bir şekilde ayrıştırmak ve göstermek için CEF veri bağlayıcısını kullanır.

> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="prerequisites"></a>Önkoşullar

- Azure Sentinel çalışma alanınızda okuma ve yazma izinlerine sahip olmanız gerekir.

- Çalışma alanı için paylaşılan anahtarlar için okuma izninizin olması gerekir.

- Thincotik gizli sunucunuzun günlükleri Syslog aracılığıyla dışarı aktarılacak şekilde yapılandırılması gerekir.

## <a name="send-thycotic-secret-server-logs-to-azure-sentinel"></a>Azure Sentinel 'e bağımsız gizli sunucu günlükleri gönderme

Günlük dosyalarını Azure Sentinel 'e almak için, Thtacotik gizli sunucunuzu, CEF formatında Linux tabanlı günlük iletme sunucunuza (rsyslog veya Syslog-ng çalıştıran) Syslog iletileri gönderecek şekilde yapılandırın. Bu sunucuda Log Analytics Aracısı yüklü olacaktır ve Aracı günlükleri Azure Sentinel çalışma alanınıza iletir.

1. Azure Sentinel gezinti menüsünde **veri bağlayıcıları**' nı seçin.

1. **Veri bağlayıcıları** Galerisi ' nden, **Thartik gizli dizi sunucusu (Önizleme)** öğesini seçin ve ardından **bağlayıcı sayfasını açın**.

1. **Yapılandırma** altında, **yönergeler** sekmesindeki yönergeleri izleyin:

    1. **1 altında. Linux Syslog Aracısı yapılandırması** -çalışır durumda bir günlük ileticisi yoksa veya başka bir tane gerekiyorsa bu adımı izleyin. Daha ayrıntılı yönergeler ve açıklamalar için bkz. 1. Adım: Azure Sentinel belgelerindeki [günlük ileticisini dağıtma](connect-cef-agent.md) .

    1. **2 altında. Ortak olay biçimi (CEF) günlüklerini Syslog aracısına ilet** - [gizli sunucu yapılandırma](https://thy.center/ss/link/syslog)yönergelerini izleyin. Bu yapılandırma aşağıdaki öğeleri içermelidir:
        - Günlük hedefi – günlük iletme sunucunuzun ana bilgisayar adı ve/veya IP adresi
        - Protokol ve bağlantı noktası – **TCP 514** (Aksi takdirde önerilir), günlük iletme sunucunuzdaki Syslog Daemon öğesinde paralel değişikliği yaptığınızdan emin olun.
        - Günlük biçimi – CEF
        - Günlük türleri: tümü kullanılabilir

    1. 3 ' ün altında **. Bağlantıyı doğrulama** -bağlayıcı sayfasına komutu kopyalayarak ve günlük ileticisinde çalıştırarak veri alımını doğrulayın. Daha ayrıntılı yönergeler ve açıklamalar için bkz. [3. Adım:](connect-cef-verify.md) Azure Sentinel belgelerindeki bağlantıyı doğrulama.

        Günlüklerinizin Log Analytics görünmeye başlaması 20 dakikaya kadar sürebilir.

## <a name="find-your-data"></a>Verilerinizi bulun

Başarılı bir bağlantı kurulduktan sonra, veriler **günlüklerde**, **Azure Sentinel** bölümünde, *commonsecuritylog* tablosunda görünür.

Log Analytics bağımsız gizli sunucu verilerini sorgulamak için, aşağıdakileri seçtiğiniz şekilde sorgu penceresine kopyalayın:

```kusto
CommonSecurityLog 
| where DeviceVendor == "Thycotic Software"
```

Bazı yararlı çalışma kitapları ve sorgu örnekleri için bağlayıcı sayfasındaki **sonraki adımlar** sekmesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, TH, Cotik gizli sunucuyu Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .