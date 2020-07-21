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
ms.openlocfilehash: 7d2d3871dd9836e2c68155aa82ce01dced128bf3
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86532166"
---
# <a name="connect-your-illusive-attack-management-system-to-azure-sentinel"></a>Illusive saldırı yönetim sisteminizi Azure Sentinel 'e bağlama

Bu makalede, [Illusive saldırı yönetim sisteminizin](https://www.illusivenetworks.com/technology/platform/attack-detection-system) Azure Sentinel 'e nasıl bağlanacağı açıklanır. Illusive saldırı yönetimi sistem verileri Bağlayıcısı, Azure Sentinel ile Illusive 'ın saldırı yüzeyi analizi verilerini ve olay günlüklerini paylaşmanıza ve bu bilgileri, kuruluşunuzun saldırı yüzeyi riski (ASM Panosu) ile ilgili bilgiler sunan ve kuruluşunuzun ağında (ADS Panosu) yetkisiz bir yan yana izlemeyi sağlayan özel panolar halinde görüntülemenize olanak sağlar.

> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="forward-illusive-attack-management-system-logs-to-the-syslog-agent"></a>Illusive saldırı yönetimi sistem günlüklerini Syslog aracısına ilet  

CEF biçimindeki syslog iletilerini, syslog Aracısı aracılığıyla Azure çalışma alanınıza iletmek için saldırı yönetim sistemini yapılandırın.

1. Illusive konsolunda oturum açın ve ayarlar->Raporlama ' ya gidin.

1. Syslog Serversץ bulma

1. Aşağıdaki bilgileri verin:
   - Ana bilgisayar adı: Linux Syslog Aracısı IP adresi veya FQDN ana bilgisayar adı
   - Bağlantı noktası: 514
   - Protokol: TCP
   - Denetim iletileri: sunucuya denetim iletileri gönderme

1. Syslog sunucusunu eklemek için Ekle ' ye tıklayın.

1. Illusive saldırı yönetimi sisteminde ilgili Log Analytics şemayı kullanmak için, CommonSecurityLog ' u arayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Illusive saldırı yönetim sistemini Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .
