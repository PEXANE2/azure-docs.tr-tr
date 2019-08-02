---
title: Azure ATP verilerini Azure Sentinel önizlemesine bağlama | Microsoft Docs
description: Azure ATP verilerini Azure Sentinel 'e bağlamayı öğrenin.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 5bf3cc44-ecda-4c78-8a63-31ab42f43605
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/28/2019
ms.author: rkarlin
ms.openlocfilehash: 6f41a5704c783ba8aeab7bc2e82ef731c6a257ac
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68599147"
---
# <a name="connect-data-from-azure-advanced-threat-protection-atp"></a>Verileri Azure Gelişmiş tehdit koruması (ATP) ile bağlama

> [!IMPORTANT]
> Azure Sentinel Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


[Azure Gelişmiş tehdit koruması](https://docs.microsoft.com/azure-advanced-threat-protection/what-is-atp) 'ndan günlükleri tek bir tıklama Ile Azure Sentinel 'e aktarabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- Genel yönetici veya Güvenlik Yöneticisi izinlerine sahip Kullanıcı
- Azure ATP 'nin bir önizleme müşterisi olmanız gerekir

## <a name="connect-to-azure-atp"></a>Azure ATP 'ye bağlanma

Ağınızda Azure ATP önizleme sürümünün [etkinleştirildiğinden](https://docs.microsoft.com/azure-advanced-threat-protection/install-atp-step1)emin olun.
Azure ATP dağıtılırsa ve verilerinizi geri alıyorsa şüpheli uyarılar Azure Sentinel 'e kolayca akışını sağlayabilir. Uyarıların Azure Sentinel 'e akışını başlatması 24 saate kadar sürebilir.


1. Azure ATP 'yi Azure Sentinel 'e bağlamak için öncelikle Azure ATP ve Microsoft Cloud App Security arasındaki tümleştirmeyi etkinleştirmeniz gerekir. Bunun nasıl yapılacağı hakkında bilgi için bkz. [Azure Gelişmiş tehdit koruması tümleştirmesi](https://docs.microsoft.com/cloud-app-security/aatp-integration).

1. Azure Sentinel 'de, **veri bağlayıcıları** ' nı seçin ve ardından **Azure ATP** kutucuğuna tıklayın.

2. **Bağlan**'a tıklayın.

6. Azure ATP uyarıları için Log Analytics ilgili şemayı kullanmak için, **Securityalert**aratın.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede Azure Gelişmiş tehdit koruması 'nı Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.

