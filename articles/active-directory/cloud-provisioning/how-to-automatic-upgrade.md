---
title: 'Azure AD Connect bulut sağlama Aracısı: otomatik yükseltme | Microsoft Docs'
description: Bu konu, Azure AD Connect bulut sağlama aracısında yerleşik otomatik yükseltme özelliğini açıklamaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/02/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 193804064fbf6d1abb2ce06df1e923ec709ef6de
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74794464"
---
# <a name="azure-ad-connect-cloud-provisioning-agent-automatic-upgrade"></a>Azure AD Connect bulut sağlama Aracısı: otomatik yükseltme

Azure AD Connect bulut sağlama Aracısı yüklemenizin her zaman güncel olduğundan emin olma, **otomatik yükseltme** özelliği ile hiçbir zaman daha kolay olmamıştı. Bu özellik varsayılan olarak etkindir ve devre dışı bırakılamaz.

Aracı buraya yüklendi: **"program files\Azure AD Connect sağlama Danışmanı T\aadconnectprovisioningagent.exe"**

Yürütülebilir dosyaya sağ tıklayıp Özellikler ' i ve sonra Ayrıntılar ' ı seçerek sürümünüzü doğrulayabilirsiniz.

![Aracı dosyası sürümü](media/how-to-automatic-upgrade/agent1.png)

Aracı güncelleştiricisi buradan yüklendi: **"program files\Azure AD Connect sağlama Aracısı Updater\AzureADConnectAgentUpdater.exe"**

Yürütülebilir dosyaya sağ tıklayıp Özellikler ' i ve sonra Ayrıntılar ' ı seçerek sürümünüzü doğrulayabilirsiniz.

![Aracı Güncelleştirici sürümü](media/how-to-automatic-upgrade/agent2.png)

## <a name="uninstalling-the-agent"></a>Aracı kaldırılıyor
Aracıyı kaldırmak için, **Kaldır ' a gidin veya bir programı değiştirin** ve şunları kaldırın:

- Microsoft Azure AD Aracısı güncelleştiricisi bağlama
- Microsoft Azure AD sağlama aracısına bağlanma
- Microsoft Azure AD sağlama Aracısı paketini bağlama

![Aracı kaldırma](media/how-to-automatic-upgrade/agent3.png)

## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)

