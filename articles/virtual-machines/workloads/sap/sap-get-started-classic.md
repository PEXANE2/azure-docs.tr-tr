---
title: LINUX sanal makinelerde SAP kullanma | Microsoft Dokümanlar
description: Microsoft Azure’daki Linux sanal makinelerinde (VM) SAP çözümlerini kullanma hakkında bilgi edinin
services: virtual-machines-linux,virtual-network,storage
documentationcenter: saponazure
author: MSSedusch
manager: gwallace
editor: ''
tags: azure-service-management
keywords: ''
ms.assetid: f9cd93dc-71ad-48a4-8778-4e48aec484a6
ms.service: virtual-machines-linux
ms.topic: conceptual
ms.tgt_pltfrm: vm-linux
ms.workload: na
ms.date: 10/04/2016
ms.author: sedusch
ms.openlocfilehash: 9525f339861b5de8dc22da753f7c36dcc6eede8a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70078983"
---
# <a name="using-sap-on-linux-virtual-machines-in-azure"></a>Azure'da Linux sanal makinelerinde SAP'yi kullanma
Bulut Bilgi İşlem, BT sektöründeki küçük ölçekli şirketlerden büyük ve çok uluslu kuruluşlara kadar çok geniş ölçekte kullanılan ve her geçen gün daha önemli hale gelen bir terimdir. Microsoft Azure, Microsoft tarafından sunulan ve birçok yeni özelliğe sahip olan Bulut Hizmetleri Platformudur. Bu özellikler sayesinde müşteriler artık teknik veya mali kısıtlamalara takılmadan uygulamalarını Bulut Hizmetleri olarak hazırlayabilir ve kullanımdan kaldırabilir. Şirketler, zamanlarını ve bütçelerini donanım altyapısına harcamak yerine uygulamanın kendisine, iş süreçlerine ve hem müşteriler hem de kullanıcılar için fayda sağlamaya odaklanabilir.

Microsoft, Microsoft Azure sanal makineleriyle Hizmet Olarak Kapsamlı Bir Altyapı (IaaS) platformu sunar. SAP NetWeaver tabanlı uygulamalar, Azure Sanal Makinelerinde (IaaS) desteklenmektedir. Aşağıdaki teknik incelemelerde, Azure'daki Windows sanal makinelerde SAP NetWeaver tabanlı uygulamaların nasıl planlanıp uygulanacağı açıklanmaktadır. Ayrıca [Windows sanal makinelerde](../../virtual-machines-windows-classic-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)SAP NetWeaver tabanlı uygulamalar uygulayabilirsiniz.

[!INCLUDE [virtual-machines-common-classic-sap-get-started](../../../../includes/virtual-machines-common-classic-sap-get-started.md)]

## <a name="sap-netweaver-on-azure-suse-linux-virtual-machines"></a>SAP NetWeaver Üzerinde Azure SUSE Linux Sanal Makineler
Başlık: Microsoft Azure SUSE Linux VM'lerde SAP NetWeaver'ı Test Etme

Özet: Şu anda Azure Linux VM'lerinde SAP NetWeaver çalıştırmak için resmi bir SAP desteği yoktur. Bununla birlikte, müşteriler sap desteğine başvurmaya gerek olmadığı sürece bazı testler yapmak isteyebilir veya Azure Linux VM'lerinde SAP demo veya eğitim sistemleri çalıştırmayı düşünebilir. Bu makale, SAP çalıştırmak için Azure SUSE Linux VM'lerinin kurulmasına yardımcı olmalıdır ve yaygın olası tuzakları önlemek için bazı temel ipuçları verir.

Güncelleme: Aralık 2015

[Bu makaleburada bulabilirsiniz](suse-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

