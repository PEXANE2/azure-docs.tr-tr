---
title: 'Bastion: Azure Bastion kullanarak uzaktan çalışma'
description: Bu sayfa, COVID-19 salgını nedeniyle uzaktan çalışmayı etkinleştirmek için Azure Kalesi'nden nasıl yararlanabileceğinizi açıklar.
services: bastion
author: mialdrid
ms.service: bastion
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: mialdrid
ms.openlocfilehash: 182195190fed70b46185f98f595de6b6c32bbffe
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80619392"
---
# <a name="working-remotely-using-azure-bastion"></a>Azure Bastion'u kullanarak uzaktan çalışma

Azure Bastion, internet bağlantısı olan kullanıcıların Azure sanal makinelerine erişmesine izin vererek uzak çalışma senaryolarını desteklemede önemli bir rol oynar. Özellikle, BT yöneticilerinin uygulamalarını azure'da çalışan uygulamalarını her zaman ve dünyanın her yerinden yönetmelerine olanak tanır.

>[!NOTE]
>Bu makalede, COVID-19 krizi nedeniyle karşılaştığınız ağ sorunlarını uzaktan çalışmak ve ağ sorunlarını azaltmak için Azure Bastion, Azure, Microsoft ağı ve Azure iş ortağı ekosisteminden nasıl yararlanabileceğiniz açıklanmaktadır.
>

## <a name="securely-access-virtual-machines"></a>Sanal makinelere güvenli bir şekilde erişin

Azure Bastion, genel bir IP adresi kullanmadan Azure sanal ağındaki sanal makinelere doğrudan Azure portalında güvenli ve sorunsuz RDP/SSH bağlantısı sağlar. Azure Bastion mimarisi ve temel özellikleri hakkında daha fazla bilgi için [Azure Bastion nedir'e](bastion-overview.md)göz atın.

Azure Kalesi sanal ağ başına dağıtılır, bu da şirketlerin bir Azure sanal ağındaki sanal makinelere uzaktan kullanıcı erişimini hızla desteklemek için bir Azure Kalesi'ni yapılandırabileceği ve yönetebileceği anlamına gelir. Azure Kalesi'ni oluşturma ve yönetme hakkında kılavuz için [bir burç ana bilgisayar oluştur'a](bastion-create-host-portal.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Portalı](bastion-create-host-portal.md), [PowerShell](bastion-create-host-powershell.md)veya Azure CLI'yi kullanarak Azure Kalesi'ni yapılandırın.

* Ek bilgi için [Bastion SSS'yi](bastion-faq.md) okuyun.
