---
title: Cisco Meraki verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Cisco Meraki bağlayıcısını kullanarak Azure Sentinel 'e Cisco Meraki günlüklerini çekme hakkında bilgi edinin. Çalışma kitaplarında Cisco Meraki verilerini görüntüleyin, uyarılar oluşturun ve araştırmayı geliştirebilirsiniz.
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
ms.date: 02/28/2021
ms.author: yelevin
ms.openlocfilehash: dc2d2a0724f18a02a5b667eb1004963a326ec360
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "101747084"
---
# <a name="connect-your-cisco-meraki-to-azure-sentinel"></a>Cisco Meraki 'nizi Azure Sentinel 'e bağlama

> [!IMPORTANT]
> Cisco Meraki Bağlayıcısı Şu anda **Önizleme** aşamasındadır. Beta, önizleme veya henüz genel kullanıma sunulmayan Azure özelliklerine uygulanan ek koşullar için [Microsoft Azure önizlemeleri için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) 'na bakın.

Bu makalede Cisco Meraki (MX/MS/MR) gereclerinizi Azure Sentinel 'e nasıl bağlayabileceğiniz açıklanır. Cisco Meraki verileri Bağlayıcısı, Azure Sentinel ile Cisco Meraki günlüklerinizi kolayca bağlamanıza olanak tanır, panoları görüntüleyebilir, özel uyarılar oluşturabilir ve araştırmayı geliştirebilirsiniz. Cisco Meraki ve Azure Sentinel arasında tümleştirme, Log Analytics aracısının yüklü olduğu Syslog sunucusunu kullanır. Ayrıca, kusto işlevine dayalı özel olarak oluşturulmuş bir günlük ayrıştırıcısı kullanır.

> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="prerequisites"></a>Önkoşullar

- Azure Sentinel çalışma alanında okuma ve yazma izninizin olması gerekir.

- Cisco Meraki çözümünüz, günlükleri Syslog aracılığıyla dışarı aktarılacak şekilde yapılandırılmalıdır.

## <a name="send-cisco-meraki-logs-to-azure-sentinel-via-the-syslog-agent"></a>Syslog Aracısı aracılığıyla Cisco Meraki günlüklerini Azure Sentinel 'e gönder  

Syslog Aracısı aracılığıyla syslog iletilerini Azure Sentinel çalışma alanınıza iletmek için Cisco Meraki yapılandırma.

1. Azure Sentinel gezinti menüsünde **veri bağlayıcıları**' nı seçin.

1. **Veri bağlayıcıları** galerisinden **Cisco Meraki (Önizleme)** bağlayıcısını seçin ve ardından **bağlayıcı sayfasını açın**.

1. **Cisco Meraki** bağlayıcı sayfasındaki yönergeleri izleyin:

    1. Linux için aracıyı yükleyip ekleme

        - Azure Linux VM veya Azure olmayan bir Linux makinesi (fiziksel veya sanal) seçin.

    1. Toplanacak günlükleri yapılandırın

        - **Çalışma alanı aracıları yapılandırmasındaki** tesisleri ve önem derecelerine seçin.

    1. Cisco Meraki cihazlarını yapılandırma ve bağlama

        - Syslog 'ı iletmek için Cisco Meraki cihazlarını yapılandırmak için [Bu yönergeleri](https://documentation.meraki.com/General_Administration/Monitoring_and_Reporting/Meraki_Device_Reporting_-_Syslog%2C_SNMP_and_API) izleyin. Uzak sunucu için, Linux aracısını yüklediğiniz Linux makinenin IP adresini kullanın.

## <a name="validate-connectivity-and-find-your-data"></a>Bağlantıyı doğrulayın ve verilerinizi bulun

Günlüklerinizin Azure Sentinel 'de görünmeye başlaması 20 dakikaya kadar sürebilir. 

Başarılı bir bağlantı kurulduktan sonra, veriler **günlüklerde**, *günlük yönetimi* bölümünde, *Syslog* tablosunda görüntülenir.

Bu veri Bağlayıcısı, beklenen şekilde çalışması için bir kusto Işlevini temel alan bir ayrıştırıcıya bağımlıdır. **Cıcomsilinebilir ki** kusto işlev diğer adını oluşturmak için [Bu adımları izleyin](https://aka.ms/sentinel-ciscomeraki-parser) . Ardından `CiscoMeraki` , verileri sorgulamak için sorgu penceresine yazabilirsiniz.

Bazı yararlı sorgu örnekleri için bağlayıcı sayfasındaki **sonraki adımlar** sekmesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede Cisco Meraki 'yi Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Verilerinize ve olası tehditlere nasıl görünürlük alabileceğinizi](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .
