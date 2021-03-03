---
title: NXLog Windows DNS günlük verilerini Azure Sentinel 'e bağla | Microsoft Docs
description: Windows DNS olaylarını Azure Sentinel 'e çekmek için NXLog DNS günlükleri veri bağlayıcısını nasıl kullanacağınızı öğrenin. Çalışma kitaplarında Windows DNS verilerini görüntüleyin, uyarılar oluşturun ve araştırmayı geliştirebilirsiniz.
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
ms.date: 03/02/2021
ms.author: yelevin
ms.openlocfilehash: 880aad438d98605d11e5a2a7c314d89bd8beb5c5
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101747083"
---
# <a name="connect-your-nxlog-windows-dns-logs-to-azure-sentinel"></a>NXLog Windows DNS günlüklerinizi Azure Sentinel 'e bağlama

> [!IMPORTANT]
> NXLog DNS günlükleri Bağlayıcısı Şu anda **Önizleme** aşamasındadır. Beta, önizleme veya henüz genel kullanıma sunulmayan Azure özelliklerine uygulanan ek koşullar için [Microsoft Azure önizlemeleri için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) 'na bakın.

[Nxlog DNS günlükleri](https://nxlog.co/documentation/nxlog-user-guide/windows-dns-server.html) Bağlayıcısı, tüm Windows DNS sunucusu olaylarınızı gerçek zamanlı olarak Azure Sentinel 'e kolayca vermenize olanak tanıyarak, kuruluşunuzun tamamında etki alanı adı sunucu etkinliğine ilişkin bir fikir verir. Hem denetim hem de analitik DNS sunucusu olaylarını gerçek zamanlı olarak toplamak ve özel alanlarla olay zenginleştirme özelliğini desteklemek için [Windows için yüksek performanslı olay izleme (ETW)](https://nxlog.co/documentation/nxlog-user-guide/windows-dns-server.html#dns_windows_etw) kullanır. NXLog DNS günlükleri ve Azure Sentinel arasında tümleştirme, REST API aracılığıyla kolaylaştıralınmıştır.

> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="configure-and-connect-nxlog-dns-logs"></a>NXLog DNS günlüklerini yapılandırma ve bağlama

NXLog, olayları JSON biçiminde doğrudan Azure Sentinel 'e gönderecek şekilde yapılandırılabilir.

1. Azure Sentinel portalında, **veri bağlayıcıları** ' na tıklayın ve **NXLOG DNS günlükleri** Bağlayıcısı ' nı seçin.

1. **Bağlayıcı sayfasını aç**' ı seçin.

1. REST API aracılığıyla yönlendirmeyi yapılandırmak için *Nxlog Kullanıcı Kılavuzu* tümleştirme konusundaki adım adım yönergeleri Izleyin [Microsoft Azure Sentinel](https://nxlog.co/documentation/nxlog-user-guide/sentinel.html) .

## <a name="find-your-data"></a>Verilerinizi bulun

Başarılı bir bağlantı kurulduktan sonra, veriler *DNS_Logs_CL* tablosundaki **özel Günlükler** bölümünün altındaki **günlüklerde** görüntülenir.

## <a name="validate-connectivity"></a>Bağlantıyı doğrula

Günlüklerinizin Log Analytics görünmeye başlaması 20 dakikaya kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, NXLog kullanarak Windows DNS günlüklerini Azure Sentinel 'e almak için nasıl kullanacağınızı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Verilerinize ve olası tehditlere nasıl görünürlük alabileceğinizi](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .
