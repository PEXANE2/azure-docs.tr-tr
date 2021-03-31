---
title: Siark EPV verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Azure Sentinel 'e günlüklerini çekmek için Siark kurumsal parola Kasası (EPV) veri bağlayıcısını nasıl kullanacağınızı öğrenin. Çalışma kitaplarında Siark EPV verilerini görüntüleyin, uyarılar oluşturun ve araştırmayı geliştirebilirsiniz.
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
ms.date: 10/25/2020
ms.author: yelevin
ms.openlocfilehash: aa57963ce369e4c8f84f4aae5f99fe343181ff6b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "100530510"
---
# <a name="connect-cyberark-enterprise-password-vault-epv-to-azure-sentinel"></a>Siark kurumsal parola kasasını (EPV) Azure Sentinel 'e bağlama

> [!IMPORTANT]
> Azure Sentinel 'deki Siark kurumsal parola Kasası (EPV) veri Bağlayıcısı Şu anda genel önizlemededir. Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sunulmaktadır. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Siark Syslog Bağlayıcısı, tüm Siark güvenlik çözüm günlüklerinizi Azure Sentinel ile kolayca bağlamanıza olanak tanır, panoları görüntüleyebilir, özel uyarılar oluşturabilir ve araştırmayı geliştirebilirsiniz. Siark ve Azure Sentinel arasında tümleştirme, SIF veri bağlayıcısını kullanarak Siark syslog iletilerini doğru bir şekilde ayrıştırır ve görüntüler.

> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="configure-and-connect-cyberark-epv"></a>Siark EPV yapılandırma ve bağlama

Siark EPV günlükleri, kasadan Linux tabanlı bir günlük iletme sunucusuna (rsyslog veya Syslog-ng çalıştıran), günlükleri Azure Sentinel 'e aktaran Log Analytics aracı ile birlikte gönderilir. Bu tür bir günlük iletme sunucunuz yoksa, bir tane çalışır duruma getirmek için [Bu yönergelere](connect-cef-agent.md) bakın.

1. Azure Sentinel portalında, **veri bağlayıcıları**' na tıklayın, **Siark kurumsal parola Kasası (EPV) olayları ' nı (Önizleme)** seçin ve ardından **bağlayıcı sayfasını açın**.

1. Günlük iletme sunucusuna Syslog verileri göndermeyi yapılandırmak için [Siark EPV yönergelerini](https://docs.cyberark.com/Product-Doc/OnlineHelp/PAS/Latest/en/Content/PASIMP/DV-Integrating-with-SIEM-Applications.htm) izleyin.

1. Bağlantınızı doğrulayın ve [Bu yönergeleri](connect-cef-verify.md)kullanarak veri alımını doğrulayın. Günlüklerinizin Log Analytics görünmeye başlaması 20 dakikaya kadar sürebilir.

## <a name="find-your-data"></a>Verilerinizi bulun

Başarılı bir bağlantı kurulduktan sonra, veriler **günlüklerde**, **Azure Sentinel** bölümünde, *commonsecuritylog* tablosunda görünür.

Log Analytics Siark EPV günlüklerini sorgulamak için, `CommonSecurityLog` sorgu penceresinin üst kısmına girin.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Siark kurumsal parola Kasası günlüklerini Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .
