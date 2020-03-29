---
title: Azure DevTest Labs'da ortak ortamları yapılandırma ve kullanma | Microsoft Dokümanlar
description: Bu makalede, Azure DevTest Labs'da ortak ortamların (Git repo'sundaki Azure Kaynak Yöneticisi şablonları) nasıl yapılandırılabildiğini ve kullanılacağı açıklanmaktadır.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2020
ms.author: spelluru
ms.openlocfilehash: 127a6986e04cf90f69b2a8ec70b90b877e534708
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76721702"
---
# <a name="configure-and-use-public-environments-in-azure-devtest-labs"></a>Azure DevTest Labs'da ortak ortamları yapılandırma ve kullanma
Azure DevTest Labs, harici bir GitHub kaynağına tek başınıza bağlanmak zorunda kalmadan ortam oluşturmak için kullanabileceğiniz [Azure Kaynak Yöneticisi şablonlarının genel deposuna](https://github.com/Azure/azure-devtestlab/tree/master/Environments) sahiptir. Bu depo, Azure Web Uygulamaları, Hizmet Kumaş Kümesi ve geliştirme SharePoint Farm ortamı gibi sık kullanılan şablonları içerir. Bu özellik, oluşturduğunuz her laboratuvar için dahil edilen yapıtların genel deposuna benzer. Ortam deposu, laboratuvarlar içindeki PaaS kaynakları için sorunsuz bir başlangıç deneyimi sağlamak için minimum giriş parametreleri ile önceden yazılmış ortam şablonları ile hızlı bir şekilde başlamanızı sağlar. 

## <a name="configuring-public-environments"></a>Genel ortamları yapılandırma
Bir laboratuvar sahibi olarak, laboratuvar oluşturma sırasında laboratuvarınız için genel ortam deposunu etkinleştirebilirsiniz. Laboratuvarınız için ortak ortamları etkinleştirmek için, bir laboratuvar oluştururken **Genel ortamlar** alanı için **Açık'ı** seçin. 

![Yeni bir laboratuvar için genel ortamı etkinleştirme](media/devtest-lab-configure-use-public-environments/enable-public-environment-new-lab.png)


Varolan laboratuvarlar için, genel ortam deposu etkinleştirilmez. Depodaki şablonları el ile etkinleştirin. Kaynak Yöneticisi şablonları kullanılarak oluşturulan laboratuarlar için depo varsayılan olarak da devre dışı bırakılır.

Laboratuvarınız için ortak ortamları etkinleştirebilir/devre dışı kaçabilir ve ayrıca aşağıdaki adımları kullanarak yalnızca belirli ortamları laboratuvar kullanıcılarının kullanımına sunabilirsiniz: 

1. Laboratuvarınız için **Yapılandırma ve ilkeler'i** seçin. 
2. VIRTUAL **MACHINE BASES** **bölümünde, Genel ortamları**seçin.
3. Laboratuvar için ortak ortamları etkinleştirmek için **Evet'i**seçin. Gerekmiyorsa **Hayır**'ı seçin. 
4. Ortak ortamları etkinleştirdimseniz, depodaki tüm ortamlar varsayılan olarak etkinleştirilir. Laboratuvar kullanıcılarınız için kullanılamaması için bir ortamı niçin seçebilirsiniz. 

![Genel ortamlar sayfası](media/devtest-lab-configure-use-public-environments/public-environments-page.png)

## <a name="use-environment-templates-as-a-lab-user"></a>Ortam şablonlarını laboratuvar kullanıcısı olarak kullanma
Laboratuvar kullanıcısı olarak, yalnızca laboratuvar sayfasındaki araç çubuğundan **+Ekle'yi** seçerek ortam şablonları etkin listesinden yeni bir ortam oluşturabilirsiniz. Üsler listesi, listenin en üstündeki laboratuvar yöneticiniz tarafından etkinleştirilen ortak ortam şablonlarını içerir.

![Genel ortam şablonları](media/devtest-lab-configure-use-public-environments/public-environment-templates.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu depo, sık kullanılan ve yararlı Kaynak Yöneticisi şablonlarını kendi eklemenize katkıda bulunabileceğiniz açık kaynak kodlu bir depodur. Katkıda bulunmak için, depoya karşı çekme isteği göndermeniz yeterlidir.  
