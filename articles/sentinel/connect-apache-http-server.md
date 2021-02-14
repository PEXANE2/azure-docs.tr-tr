---
title: Apache HTTP sunucunuzu Azure Sentinel 'e bağlama | Microsoft Docs
description: Apache HTTP sunucu bağlayıcısını kullanarak Apache günlüklerini Azure Sentinel 'e çekme hakkında bilgi edinin. Çalışma kitaplarında Apache verilerini görüntüleyin, uyarılar oluşturun ve araştırmayı geliştirebilirsiniz.
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
ms.date: 02/03/2021
ms.author: yelevin
ms.openlocfilehash: 6a1a2a2a7dac961e49e6ced38803649ebf5ad523
ms.sourcegitcommit: 24f30b1e8bb797e1609b1c8300871d2391a59ac2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100096863"
---
# <a name="connect-your-apache-http-server-to-azure-sentinel"></a>Apache HTTP sunucunuzu Azure Sentinel 'e bağlama

> [!IMPORTANT]
> Apache HTTP sunucu Bağlayıcısı Şu anda **Önizleme** aşamasındadır. Beta, önizleme veya henüz genel kullanıma sunulmayan Azure özelliklerine uygulanan ek koşullar için [Microsoft Azure önizlemeleri için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) 'na bakın.

Bu makalede Apache HTTP sunucunuzu Azure Sentinel 'e nasıl bağlayabileceğiniz açıklanır. Apache HTTP sunucu Bağlayıcısı, çalışma kitaplarında verileri görüntüleyebilmeniz, özel uyarılar oluşturmak için sorgulayıp ve araştırmayı iyileştirecek şekilde birleştirebilmeniz için Apache HTTP sunucu günlüklerinizi Azure Sentinel 'e kolayca almanıza olanak sağlar. Apache HTTP sunucusu ile Azure Sentinel arasında tümleştirme, Log Analytics Aracısı tarafından yerel dosya işlemeyi kullanır.

> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="prerequisites"></a>Önkoşullar

- Azure Sentinel çalışma alanında yazma izninizin olması gerekir.

## <a name="configure-and-integrate-apache-http-server-logs-via-log-analytics-agent"></a>Log Analytics Aracısı aracılığıyla Apache HTTP sunucusu günlüklerini yapılandırma ve Tümleştirme

Apache HTTP sunucusunu, Log Analytics Aracısı aracılığıyla Azure çalışma alanınıza günlük dosyaları gönderecek şekilde yapılandırın.
Log Analytics aracısını Apache HTTP sunucusu günlük dosyalarını okuyacak şekilde yapılandırın.

1. https://httpd.apache.org/docs/2.4/logs.htmlApache HTTP Server 'da günlük dosyaları konumunu ayarlama konusundaki yönergeleri izleyin.

1. Azure Sentinel gezinti menüsünde, **veri bağlayıcıları** ' nı ve ardından **Apache HTTP sunucusu (Önizleme)** öğesini seçin.

1. **Bağlayıcı sayfasını aç**' ı seçin.

1. Apache HTTP sunucusu sayfasındaki yönergeleri izleyin.

## <a name="find-your-data"></a>Verilerinizi bulun

Başarılı bir bağlantı kurulduktan sonra, veriler ApacheHTTPServer_CL altında Log Analytics görüntülenir.

## <a name="validate-connectivity"></a>Bağlantıyı doğrula

Günlüklerinizin Log Analytics görünmeye başlaması 20 dakikaya kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede Apache HTTP Server 'ı Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Verilerinize ve olası tehditlere nasıl görünürlük alabileceğinizi](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .
