---
title: 'Azure AD Connect bulut sağlama Aracısı: otomatik yükseltme | Microsoft Docs'
description: Bu makalede, Azure AD Connect bulut sağlama aracısında yerleşik otomatik yükseltme özelliği açıklanır.
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
ms.openlocfilehash: d3786386f75f4b85fe89562254eab63471de6cb2
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2019
ms.locfileid: "75549406"
---
# <a name="azure-ad-connect-cloud-provisioning-agent-automatic-upgrade"></a>Azure AD Connect bulut sağlama Aracısı: otomatik yükseltme

Azure Active Directory (Azure AD) bulut sağlama Aracısı yüklemesinin her zaman güncel olduğundan emin olmak otomatik yükseltme özelliği ile oldukça kolaydır. Bu özellik varsayılan olarak etkindir ve devre dışı bırakılamaz.

Aracı buraya yüklendi: "program files\Azure AD Connect sağlama Danışmanı T\aadconnectprovisioningagent.exe"

Sürümünüzü doğrulamak için, yürütülebilir dosyaya sağ tıklayın ve Özellikler ' i ve ardından Ayrıntılar ' ı seçin.

![Aracı dosyası sürümü](media/how-to-automatic-upgrade/agent1.png)

Aracı güncelleştiricisi buradan yüklendi: "program files\Azure AD Connect sağlama Aracısı Updater\AzureADConnectAgentUpdater.exe"

Sürümünüzü doğrulamak için, yürütülebilir dosyaya sağ tıklayın ve Özellikler ' i ve ardından Ayrıntılar ' ı seçin.

![Aracı Güncelleştirici sürümü](media/how-to-automatic-upgrade/agent2.png)

## <a name="uninstall-the-agent"></a>Aracıyı kaldırın
Aracıyı kaldırmak için, **Kaldır ' a gidin veya bir programı değiştirin** ve şunları kaldırın:

- **Microsoft Azure AD Aracısı güncelleştiricisi bağlama**
- **Microsoft Azure AD sağlama aracısına bağlanma**
- **Microsoft Azure AD sağlama Aracısı paketini bağlama**

![Aracı kaldırma](media/how-to-automatic-upgrade/agent3.png)

## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)

