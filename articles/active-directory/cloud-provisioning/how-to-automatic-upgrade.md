---
title: 'Azure AD Connect bulut sağlama aracısı: Otomatik yükseltme | Microsoft Dokümanlar'
description: Bu makalede, Azure AD Connect bulut sağlama aracısında yerleşik otomatik yükseltme özelliği açıklanmaktadır.
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
ms.openlocfilehash: f09b2fc685881aa8a7bd87b6a855c657af9ef43d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78190322"
---
# <a name="azure-ad-connect-cloud-provisioning-agent-automatic-upgrade"></a>Azure AD Connect bulut sağlama aracısı: Otomatik yükseltme

Azure Active Directory (Azure AD) Connect bulut sağlama aracıyüklemenizin otomatik yükseltme özelliğiyle her zaman güncel olduğundan emin olmak kolaydır.

Aracı burada yüklenir: "Program dosyaları\Azure AD Connect Provisioning Agent\AADConnectProvisioningAgent.exe"

Sürümünüzü doğrulamak için, çalıştırılabilir ve seçili özellikleri ve ardından ayrıntıları sağ tıklatın.

![Aracı dosyası sürümü](media/how-to-automatic-upgrade/agent1.png)

Aracı verici güncelleyici burada yüklenir: "Program dosyaları\Azure AD Connect Provisioning Agent Updater\AzureADConnectAgentUpdater.exe"

Sürümünüzü doğrulamak için, çalıştırılabilir ve seçili özellikleri ve ardından ayrıntıları sağ tıklatın.

![Aracı verici güncelleyici sürümü](media/how-to-automatic-upgrade/agent2.png)

## <a name="uninstall-the-agent"></a>Aracıyı kaldırın
Aracıyı kaldırmak **için, bir programı kaldır veya değiştir** ve aşağıdakileri kaldırın:

- **Microsoft Azure AD Bağlantı Aracısı Güncelleyici**
- **Microsoft Azure AD Connect Sağlama Aracısı**
- **Microsoft Azure AD Connect Sağlama Aracısı Paketi**

![Aracı kaldırma](media/how-to-automatic-upgrade/agent3.png)

## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)

