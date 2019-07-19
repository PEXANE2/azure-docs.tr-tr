---
title: Cloud App Security verilerini Azure Sentinel önizlemesine bağlama | Microsoft Docs
description: Cloud App Security verilerini Azure Sentinel 'e bağlamayı öğrenin.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: cd9e5e27-fdd4-4717-8924-be4c1c430f23
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2019
ms.author: rkarlin
ms.openlocfilehash: 92de4120d73af70c5de013488344a15ba46513c7
ms.sourcegitcommit: 1b7b0e1c915f586a906c33d7315a5dc7050a2f34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2019
ms.locfileid: "67881081"
---
# <a name="connect-data-from-microsoft-cloud-app-security"></a>Microsoft Cloud App Security verileri bağlama 

> [!IMPORTANT]
> Azure Sentinel Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) günlüklerini Azure Sentinel 'e tek bir tıklama ile akışla aktarabilirsiniz. Bu bağlantı, uyarıları Azure Sentinel 'e Cloud App Security akışını sağlar. 

## <a name="prerequisites"></a>Önkoşullar

- Genel yönetici veya Güvenlik Yöneticisi izinlerine sahip Kullanıcı

## <a name="connect-to-cloud-app-security"></a>Cloud App Security Bağlan

Zaten Cloud App Security varsa, [Ağınızda etkinleştirildiğinden](https://docs.microsoft.com/cloud-app-security/getting-started-with-cloud-app-security)emin olun.
Cloud App Security dağıtılırsa ve verileriniz geri alıyorsa, uyarı verileri kolayca Azure Sentinel 'e akışla eklenebilir.


1. Azure Sentinel 'de **veri bağlayıcıları** ' nı seçin ve **Cloud App Security** kutucuğuna tıklayın.

2. **Bağlan**'a tıklayın.

3. Cloud App Security uyarıları için Log Analytics ilgili şemayı kullanmak için, **Securityalert**aratın.


## <a name="next-steps"></a>Sonraki adımlar
Bu belgede Microsoft Cloud App Security Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.
