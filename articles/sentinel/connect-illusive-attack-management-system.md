---
title: Illusive saldırı yönetimi sistem verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Illusive saldırı yönetimi sistem verilerini Azure Sentinel 'e bağlamayı öğrenin.
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
ms.date: 07/17/2020
ms.author: yelevin
ms.openlocfilehash: ea7953be25473357f7ed572fa8b76076edc6f75a
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87038126"
---
# <a name="connect-your-illusive-attack-management-system-to-azure-sentinel"></a>Illusive saldırı yönetim sisteminizi Azure Sentinel 'e bağlama

> [!IMPORTANT]
> Azure Sentinel 'deki Illusive saldırı yönetimi sistem verileri Bağlayıcısı Şu anda genel önizlemededir.
> Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu makalede, [Illusive saldırı yönetim sisteminizin](https://www.illusivenetworks.com/technology/platform/attack-detection-system) Azure Sentinel 'e nasıl bağlanacağı açıklanır. Illusive saldırı yönetimi sistem verileri Bağlayıcısı, Azure Sentinel ile Illusive 'ın saldırı yüzeyi analizi verilerini ve olay günlüklerini paylaşmanıza ve bu bilgileri, kuruluşunuzun saldırı yüzeyi riski (ASM Panosu) ile ilgili bilgiler sunan ve kuruluşunuzun ağında (ADS Panosu) yetkisiz bir yan yana izlemeyi sağlayan özel panolar halinde görüntülemenize olanak sağlar.

> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="forward-illusive-attack-management-system-logs-to-the-syslog-agent"></a>Illusive saldırı yönetimi sistem günlüklerini Syslog aracısına ilet  

CEF biçimindeki syslog iletilerini, syslog Aracısı aracılığıyla Azure çalışma alanınıza iletmek için saldırı yönetim sistemini yapılandırın.

1. Illusive konsolunda oturum açın ve ayarlar->Raporlama ' ya gidin.

1. Syslog sunucularını bulun.

1. Aşağıdaki bilgileri verin:
   - Ana bilgisayar adı: Linux Syslog Aracısı IP adresi veya FQDN ana bilgisayar adı
   - Bağlantı noktası: 514
   - Protokol: TCP
   - Denetim iletileri: sunucuya denetim iletileri gönderme

1. Syslog sunucusunu eklemek için Ekle ' ye tıklayın.

1. Illusive saldırı yönetimi sistemine yönelik **günlüklerde** ilgili şemayı kullanmak Için, **commonsecuritylog**' u arayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Illusive saldırı yönetim sistemini Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .
