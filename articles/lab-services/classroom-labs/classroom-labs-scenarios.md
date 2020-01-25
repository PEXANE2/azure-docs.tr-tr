---
title: Ders laboratuvarları kullanma-Azure Lab Services
description: Bu makalede, eğitim senaryolarında Azure 'da laboratuvarları oluşturmak için Azure DevTest Labs nasıl kullanılacağı açıklanır.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.assetid: 57ff4e30-7e33-453f-9867-e19b3fdb9fe2
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2020
ms.author: spelluru
ms.openlocfilehash: 155806222f9e11fec177487b7147d81054ac06ed
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76717992"
---
# <a name="use-classroom-labs-for-trainings"></a>Ders laboratuvarları kullanma
Seyahat için bir laboratuvar oluşturabilirsiniz. Azure Lab Services derslik Labs, her bir eğitimin eğitim için aynı ve yalıtılmış ortamları kullandığı eğitim için bir laboratuvar oluşturmanıza imkan tanır. Eğitim ortamlarının yalnızca ihtiyaç duydukları ve sanal makineler gibi yeterli kaynak (eğitim için gerekli) içermesi durumunda her bir Trae tarafından kullanılabilir olmasını sağlamak için ilkeler uygulayabilirsiniz. 

![Sınıf Laboratuvarı](../media/classroom-labs-scenarios/classroom.png)

Derslik Labs, herhangi bir sanal ortamda eğitim yapmak için gereken aşağıdaki gereksinimleri karşılar: 

- Eğitim ortamlarını hızlı bir şekilde temin edebilirsiniz
- Her eğitim makinesi aynı olmalıdır
- Diğer işlemlerde oluşturulan VM 'Leri göremez
- Eğitimin, eğitim için ihtiyaç duydukları miktardan daha fazla VM alamaz ve ayrıca onları kullandıklarında sanal makineleri kapatması için maliyeti denetleyin
- Eğitim laboratuvarı her bir Trae ile kolayca paylaşabilirsiniz
- Eğitim laboratuvarı yeniden kullanın ve tekrar

Bu makalede, daha önce açıklanan eğitim gereksinimlerini karşılamak için kullanılabilecek çeşitli Azure Lab Services özellikleri ve eğitim için bir laboratuvar kurmak için izleyebileceğiniz ayrıntılı adımları öğreneceksiniz.  

## <a name="create-the-lab-account-as-a-lab-account-administrator"></a>Laboratuvar hesabını laboratuvar hesabı Yöneticisi olarak oluşturma
Azure Lab Services kullanmanın ilk adımı Azure portal laboratuvar hesabı oluşturmaktır. Laboratuvar hesabı Yöneticisi laboratuvar hesabını oluşturduktan sonra, yönetici, **Laboratuvar Oluşturucu** rolüne Labs oluşturmak isteyen kullanıcıları ekler. Eğitiçler, öğrenciler için sanal makinelerle birlikte laboratuvarları oluşturarak eğitim aldıkları kursa yönelik alıştırmaları yapabilirler. Ayrıntılar için bkz. [Laboratuvar hesabı oluşturma ve yönetme](how-to-manage-lab-accounts.md).

## <a name="create-and-manage-classroom-labs"></a>Sınıf laboratuvarları oluşturma ve yönetme
Laboratuvar hesabındaki laboratuvar Oluşturucu rolünün bir üyesi olan bir Trainer, laboratuvar hesabında bir veya daha fazla Laboratuvarı oluşturabilir. Kursta alýþtýrmalarý gerçekleştirmek için gerekli tüm yazılımlarla bir şablon VM 'si oluşturup yapılandırırsınız. Bir sınıf laboratuvarı oluşturmak için kullanılabilir görüntülerden kullanıma hazır bir görüntü seçer ve ardından Laboratuvar için gereken yazılımı yükleyerek özelleştirebilirsiniz. Ayrıntılar için bkz. [sınıf laboratuvarları oluşturma ve yönetme](how-to-manage-classroom-labs.md).

## <a name="configure-usage-settings-and-policies"></a>Kullanım ayarlarını ve ilkeleri yapılandırma
Laboratuvar Oluşturucu, laboratuvar kullanıcılarına kullanıcı ekleyebilir veya kaldırabilir, laboratuvar kullanıcılarına göndermek için kayıt bağlantısını alabilir, Kullanıcı başına ayrı kotalar ayarlama, laboratuvardaki kullanılabilir sanal makine sayısını güncelleştirme ve daha fazlası gibi ilkeler ayarlama. Ayrıntılar için bkz. [kullanım ayarlarını ve Ilkeleri yapılandırma](how-to-configure-student-usage.md).

## <a name="create-and-manage-schedules"></a>Zamanlamaları oluşturma ve yönetme
Zamanlamalar, laboratuvardaki VM 'Lerin otomatik olarak başlamasını ve belirli bir zamanda kapatılmasını sağlamak için bir derslik Laboratuvarı yapılandırmanıza olanak tanır. Tek seferlik bir zamanlama veya yinelenen bir zamanlama tanımlayabilirsiniz. Ayrıntılar için bkz. [sınıf laboratuvarları için zamanlama oluşturma ve yönetme](how-to-create-schedules.md).

## <a name="set-up-and-publish-a-template-vm"></a>Şablon VM 'si ayarlama ve yayımlama
Laboratuvardaki şablon, tüm kullanıcıların sanal makinelerinin oluşturulduğu bir temel sanal makine görüntüsüdür. Yalnızca eğitim katılımcılarına sağlamak istediğiniz şekilde yapılandırmak için şablon VM 'yi ayarlayın. Laboratuvar kullanıcılarının görebileceği bir ad ve açıklama belirtebilirsiniz. Daha sonra, şablon VM 'nin örneklerini laboratuvar kullanıcılarınız için kullanılabilir hale getirmek üzere şablonu yayımlayabilirsiniz. Bir şablonu yayımladığınızda Azure Lab Services, şablonu kullanarak laboratuvarda sanal makineler oluşturur. Bu işlemde oluşturulan sanal makine sayısı, laboratuvarda izin verilen maksimum kullanıcı sayısıyla aynıdır. Laboratuvarın kullanım ilkesinde bu maksimum değeri ayarlayabilirsiniz. Tüm sanal makineler, şablonla aynı yapılandırmaya sahiptir. Ayrıntılar için bkz. [şablon sanal makinelerini ayarlama ve yayımlama](how-to-create-manage-template.md). 

## <a name="use-vms-in-the-classroom-lab"></a>Sınıf laboratuvarında VM 'Leri kullanma
Bir öğrenci veya eğitim katılımcısı laboratuvara kaydolur ve kursa yönelik alıştırmaları yapmak için VM 'ye bağlanır. Ayrıntılar için bkz. [sınıf laboratuvarına erişme](how-to-use-classroom-lab.md).

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaledeki yönergeleri izleyerek ders laboratuvarlarında bir laboratuvar hesabı oluşturmaya başlayın: [öğretici: Azure Lab Services bir laboratuvar hesabı kurma](tutorial-setup-lab-account.md).