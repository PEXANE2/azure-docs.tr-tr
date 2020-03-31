---
title: Eğitimler için Sınıf Laboratuvarları'nı kullanma - Azure Lab Hizmetleri
description: Bu makalede, eğitim senaryoları için Azure'da laboratuvar oluşturmak için Azure DevTest Labs'ın nasıl kullanılacağı açıklanmaktadır.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76717992"
---
# <a name="use-classroom-labs-for-trainings"></a>Eğitimler için Sınıf Laboratuvarlarını kullanma
Eğitimler için bir laboratuvar kurabilirsiniz. Azure Laboratuvar Hizmetlerinin Sınıf Laboratuvarları, eğitiminiz için her kursiyerin eğitim için aynı ve izole ortamları kullandığı bir laboratuvar oluşturmanıza olanak tanır. Eğitim ortamlarının her kursiyer için yalnızca ihtiyaç duyduklarında ve eğitim için gerekli sanal makineler gibi yeterli kaynakları içerdiğinden emin olmak için ilkeler uygulayabilirsiniz. 

![Sınıf laboratuvarı](../media/classroom-labs-scenarios/classroom.png)

Sınıf Laboratuvarları, herhangi bir sanal ortamda eğitim yapmak için gerekli olan aşağıdaki gereksinimleri karşılar: 

- Kursiyerler eğitim ortamlarını hızlı bir şekilde sağlayabilirler
- Her eğitim makinesi aynı olmalıdır
- Kursiyerler diğer kursiyerler tarafından oluşturulan VM'leri göremez
- Kursiyerlerin eğitim için ihtiyaç duyduklarından daha fazla VM alamamasını ve kullanmadıkları nda VM'leri kapatmalarını sağlayarak maliyeti kontrol edin
- Eğitim laboratuarını her kursiyerle kolayca paylaşın
- Eğitim laboratuarını tekrar tekrar yeniden kullanın

Bu makalede, daha önce açıklanan eğitim gereksinimlerini ve eğitim için bir laboratuvar kurmak için izleyebileceğiniz ayrıntılı adımları karşılamak için kullanılabilecek çeşitli Azure Lab Hizmetleri özellikleri hakkında bilgi edinebilirsiniz.  

## <a name="create-the-lab-account-as-a-lab-account-administrator"></a>Laboratuvar hesabıyöneticisi olarak laboratuvar hesabı oluşturma
Azure Laboratuvar Hizmetleri'ni kullanmanın ilk adımı, Azure portalında bir laboratuvar hesabı oluşturmaktır. Bir laboratuvar hesabı yöneticisi laboratuvar hesabını oluşturduktan sonra, yönetici Laboratuvar **Oluşturucusu** rolüne laboratuvar oluşturmak isteyen kullanıcıları ekler. Eğitmenler, öğrencilerin öğrettikleri kurs için alıştırmalar yapmaları için sanal makinelerle laboratuvarlar oluştururlar. Ayrıntılar için [laboratuvar hesabı oluştur ve yönet'](how-to-manage-lab-accounts.md)e bakın.

## <a name="create-and-manage-classroom-labs"></a>Sınıf laboratuvarları oluşturma ve yönetme
Laboratuvar hesabındaki Lab Creator rolünün bir üyesi olan bir eğitmen, laboratuvar hesabında bir veya daha fazla laboratuvar oluşturabilir. Kursunuzda alıştırmalar yapmak için gerekli tüm yazılımlarla bir şablon VM oluşturur ve yapılandırırsınız. Sınıf laboratuarı oluşturmak için kullanılabilir resimlerden hazır bir görüntü seçin ve ardından laboratuvar için gerekli yazılımı yükleyerek özelleştirin. Ayrıntılar için [sınıf laboratuvarları oluştur ve yönet'](how-to-manage-classroom-labs.md)e bakın.

## <a name="configure-usage-settings-and-policies"></a>Kullanım ayarlarını ve ilkelerini yapılandırma
Laboratuvar oluşturucusu, kullanıcıları laboratuvara ekleyebilir veya kaldırabilir, laboratuvar kullanıcılarına göndermek için kayıt bağlantısı alabilir, kullanıcı başına tek tek kota ayarlama, laboratuvarda bulunan VM sayısını güncelleştirme ve daha fazlası gibi ilkeler ayarlayabilir. Ayrıntılar için [bkz.](how-to-configure-student-usage.md)

## <a name="create-and-manage-schedules"></a>Zamanlamaları oluşturma ve yönetme
Zamanlamalar, laboratuvardaki VM'lerin belirli bir zamanda otomatik olarak başlayıp kapanması nı sağlayacak şekilde bir sınıf laboratuarı yapılandırmanıza olanak sağlar. Tek seferlik zamanlama veya yinelenen bir zamanlama tanımlayabilirsiniz. Ayrıntılar için sınıf [laboratuarları için zamanlama oluştur ve yönet'](how-to-create-schedules.md)e bakın.

## <a name="set-up-and-publish-a-template-vm"></a>Bir şablon ayarlama ve yayımlama VM
Laboratuvardaki şablon, tüm kullanıcıların sanal makinelerinin oluşturulduğu bir temel sanal makine görüntüsüdür. Şablon VM'yi, eğitim katılımcılarına tam olarak ne sağlamak istediğinizle yapılandırılabilmek için ayarlayın. Laboratuvar kullanıcılarının görebileceği bir ad ve açıklama belirtebilirsiniz. Ardından, şablon VM örneklerini laboratuvar kullanıcılarınız için kullanılabilir hale getirmek için şablonu yayımlarsınız. Bir şablonu yayımladığınızda Azure Lab Services, şablonu kullanarak laboratuvarda sanal makineler oluşturur. Bu işlemde oluşturulan sanal makine sayısı, laboratuvarda izin verilen maksimum kullanıcı sayısıyla aynıdır. Laboratuvarın kullanım ilkesinde bu maksimum değeri ayarlayabilirsiniz. Tüm sanal makineler, şablonla aynı yapılandırmaya sahiptir. Ayrıntılar için [şablon sanal makineleri ayarlama ve yayımlama](how-to-create-manage-template.md)hakkında bilgi bakın. 

## <a name="use-vms-in-the-classroom-lab"></a>Sınıf laboratuvarında VM'ler kullanma
Bir öğrenci veya eğitim katılımcısı laboratuvara kaydolur ve kurs için alıştırmalar yapmak için VM'ye bağlanır. Ayrıntılar için [sınıf laboratuvarına nasıl erişilir'](how-to-use-classroom-lab.md)e bakın.

## <a name="next-steps"></a>Sonraki adımlar
Makaledeki yönergeleri izleyerek Classroom Labs'da bir laboratuvar hesabı oluşturmaya başlayın: [Öğretici: Azure Lab Hizmetleri ile bir laboratuvar hesabı oluşturma.](tutorial-setup-lab-account.md)