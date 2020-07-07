---
title: 'Savunma kullanarak uzaktan çalışma: Azure savunma'
description: Bu sayfada COVıN-19 pandemıc nedeniyle uzaktan çalışmayı etkinleştirmek üzere Azure savunma özelliğinden nasıl yararlanabileceğinizi açıklanmaktadır.
services: bastion
author: mialdrid
ms.service: bastion
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: mialdrid
ms.openlocfilehash: 182195190fed70b46185f98f595de6b6c32bbffe
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80619392"
---
# <a name="working-remotely-using-azure-bastion"></a>Azure savunma kullanarak uzaktan çalışma

Azure savunma, internet bağlantısı olan kullanıcıların Azure sanal makinelerine erişmesine izin vererek uzak çalışma senaryolarını desteklemeye yönelik bir özetleme rolü oynar. Özellikle, BT yöneticilerinin Azure üzerinde çalışan uygulamalarını dilediğiniz zaman ve dünyanın her yerinden yönetmesine olanak sağlar.

>[!NOTE]
>Bu makalede, Azure savunma, Azure, Microsoft ağı ve Azure iş ortağı ekosisteminden yararlanarak uzaktan nasıl çalışabileceğinizi ve COVı-19 çapraz olduğu için kullanıma sunulacak ağ sorunlarını azaltmayı nasıl kullanabileceğiniz açıklanmaktadır.
>

## <a name="securely-access-virtual-machines"></a>Sanal makinelere güvenle erişin

Azure savunma özellikle, genel IP adresi kullanılmadan doğrudan Azure portal Azure sanal ağı içindeki sanal makinelere güvenli ve sorunsuz RDP/SSH bağlantısı sağlar. Azure savunma mimarisi ve temel özellikler hakkında daha fazla bilgi için [Azure](bastion-overview.md)'a göz atın.

Azure savunma, sanal ağ başına dağıtılır, yani şirketler bir Azure sanal ağı içindeki sanal makinelere yönelik olarak uzak Kullanıcı erişimini hızlı bir şekilde desteklemek için bir Azure savunma yapılandırabilir ve yönetebilir. Azure savunma oluşturma ve yönetme hakkında yönergeler için bkz. [bir savunma ana bilgisayarı oluşturma](bastion-create-host-portal.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Portal](bastion-create-host-portal.md), [POWERSHELL](bastion-create-host-powershell.md)veya Azure CLI kullanarak Azure savunma 'yı yapılandırın.

* Ek bilgi için savunma [hakkında SSS](bastion-faq.md) makalesini okuyun.
