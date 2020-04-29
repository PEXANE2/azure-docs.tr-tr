---
title: Azure DevTest Labs 'de ortak ortamları yapılandırma ve kullanma | Microsoft Docs
description: Bu makalede, Azure DevTest Labs ' de genel ortamların (bir git deposunda Azure Resource Manager şablonları) nasıl yapılandırılacağı ve kullanılacağı açıklanmaktadır.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76721702"
---
# <a name="configure-and-use-public-environments-in-azure-devtest-labs"></a>Azure DevTest Labs ortak ortamları yapılandırma ve kullanma
Azure DevTest Labs, sizin tarafınızdan bir dış GitHub kaynağına bağlanmak zorunda kalmadan ortamlar oluşturmak için kullanabileceğiniz [Azure Resource Manager şablonlarının ortak bir deposudur](https://github.com/Azure/azure-devtestlab/tree/master/Environments) . Bu depo, Azure Web Apps, Service Fabric kümesi ve geliştirme SharePoint grubu ortamı gibi sık kullanılan şablonları içerir. Bu özellik, oluşturduğunuz her laboratuvar için dahil edilen yapıların genel deposuna benzer. Ortam deposu, Labs içinde PaaS kaynakları için sorunsuz bir başlangıç deneyimi sunmak üzere en az giriş parametreleriyle önceden yazılmış ortam şablonlarına hızla başlamanızı sağlar. 

## <a name="configuring-public-environments"></a>Ortak ortamları yapılandırma
Laboratuvar sahibi olarak laboratuvar oluşturma sırasında laboratuvarınız için ortak ortam deposunu etkinleştirebilirsiniz. Laboratuvarınız için ortak ortamları etkinleştirmek üzere laboratuvar oluştururken **genel ortamlar** alanı için **Açık** ' ı seçin. 

![Yeni laboratuvar için ortak ortamı etkinleştir](media/devtest-lab-configure-use-public-environments/enable-public-environment-new-lab.png)


Mevcut laboratuvarlarda, ortak ortam deposu etkinleştirilmemiştir. Depodaki şablonları kullanmak için el ile etkinleştirin. Kaynak Yöneticisi şablonlar kullanılarak oluşturulan laboratuvarlar için, depo varsayılan olarak devre dışıdır.

Laboratuvarınız için genel ortamları etkinleştirebilir/devre dışı bırakabilir ve ayrıca aşağıdaki adımları kullanarak yalnızca belirli ortamları laboratuvar kullanıcıları için kullanılabilir hale getirebilirsiniz: 

1. Laboratuvarınız için **yapılandırma ve ilkeler '** i seçin. 
2. **Sanal makıne tabanları** bölümünde **ortak ortamlar**' ı seçin.
3. Laboratuvar için ortak ortamları etkinleştirmek üzere **Evet**' i seçin. Gerekmiyorsa **Hayır**'ı seçin. 
4. Ortak ortamları etkinleştirdiyseniz, depodaki tüm ortamlar varsayılanlar tarafından etkinleştirilir. Laboratuvar kullanıcılarınız için kullanılamaz hale getirmek üzere bir ortamı devre dışı bırakabilirsiniz. 

![Ortak ortamlar sayfası](media/devtest-lab-configure-use-public-environments/public-environments-page.png)

## <a name="use-environment-templates-as-a-lab-user"></a>Ortam şablonlarını laboratuvar kullanıcısı olarak kullanma
Laboratuvar kullanıcısı olarak, laboratuvar sayfasındaki araç çubuğundan **+ Ekle** ' yi seçerek ortam şablonlarının etkin listesinden yeni bir ortam oluşturabilirsiniz. Temellerin listesi, laboratuvar yöneticiniz tarafından etkinleştirilen ortak ortamlar şablonlarını listenin en üstünde içerir.

![Genel ortam şablonları](media/devtest-lab-configure-use-public-environments/public-environment-templates.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu depo, kendi sık kullanılan ve yararlı Kaynak Yöneticisi şablonlarını eklemek için katkıda bulunabileceğiniz açık kaynaklı bir depodur. Katkıda bulunmak için, depoya karşı bir çekme isteği göndermeniz yeterlidir.  
