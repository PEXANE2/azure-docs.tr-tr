---
title: Azure DevTest Labs'daki bir laboratuvara dahili destek bildirimi ekleme
description: Azure DevTest Labs'daki bir laboratuvara dahili destek bildirimini nasıl yayınlayacağınızı öğrenin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: c9900333-6c5e-4d7d-b0f4-889015e9550c
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2020
ms.author: spelluru
ms.openlocfilehash: 591dcec36dc62143901d3b49db24196e84d58c29
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76170360"
---
# <a name="add-an-internal-support-statement-to-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'daki bir laboratuvara dahili destek bildirimi ekleme

Azure DevTest Labs, kullanıcılara laboratuvar hakkında destek bilgileri sağlayan dahili bir destek bildirimiyle laboratuvarınızı özelleştirmenize olanak tanır. Örneğin, kullanıcının sorun giderme veya laboratuvardaki kaynaklara erişim konusunda yardıma ihtiyaç duyduğunda dahili desteğe nasıl ulaşacağını bilmesi için kişi bilgileri sağlayabilirsiniz. Ayrıca, ekibinizin desteğe başvurmadan önce erişebileceği dahili web sitelerine veya SSS'lere bağlantılar da sağlayabilirsiniz.

Dahili destek deyimi, genellikle çok sık değişmeyen laboratuvar bilgilerini göndermenize izin vermek için tasarlanmıştır. Laboratuvar ilkelerine yönelik son güncellemeler gibi, doğada daha geçici olan laboratuvar bilgileri hakkında kullanıcıları bilgilendirmek için [laboratuvardaki Posta duyurusuna](devtest-lab-announcements.md)bakın.

Bir destek bildirimini artık geçerli olmadıktan sonra kolayca devre dışı layabilir veya edebilirsiniz.

## <a name="steps-to-add-a-support-statement-to-an-existing-lab"></a>Varolan bir laboratuvara destek bildirimi ekleme adımları

1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040)oturum açın.
1. Gerekirse Tüm **Hizmetler'i**seçin ve listeden **DevTest Labs'ı** seçin. (Laboratuvarınız **Tüm Kaynaklar**altında Pano'da zaten gösterilmiş olabilir).
1. Laboratuvarlar listesinden, destek bildirimi eklemek istediğiniz laboratuarı seçin.  
1. Laboratuvarın Genel **Bakış** alanında **Yapılandırma ve ilkeler'i**seçin.  

    ![Yapılandırma ve ilkeler düğmesi](./media/devtest-lab-internal-support-message/devtestlab-config-and-policies.png)

1. **SETTINGS**altında solda, **İç destek**seçin.

    ![Dahili destek düğmesi](./media/devtest-lab-internal-support-message/devtestlab-internal-support.png)

1. Bu laboratuvardaki kullanıcılar için bir iç destek iletisi oluşturmak için Etkin'i **Evet**olarak ayarlayın.

1. Destek **iletisi** alanına, laboratuvar kullanıcılarınıza sunmak istediğiniz dahili destek bildirimini girin. Destek iletisi Markdown'ı kabul eder. İleti metnini girdiğinizde, iletinin kullanıcılara nasıl göründüğünü görmek için ekranın altındaki **Önizleme** alanını görüntüleyebilirsiniz.

    ![İletioluşturmak için iç destek ekranı.](./media/devtest-lab-internal-support-message/devtestlab-add-support-statement.png)


1. Destek ekstreniz yayına hazır olduğunda **Kaydet'i** seçin.

Bu destek iletisini artık laboratuvar kullanıcılarına göstermek istemiyorsanız, **İç destek** sayfasına dönün ve **Etkin'i** **Hayır**olarak ayarlayın.

## <a name="steps-for-users-to-view-the-support-message"></a>Kullanıcıların destek iletisini görüntüleme adımları

1. Azure [portalından](https://go.microsoft.com/fwlink/p/?LinkID=525040)bir laboratuvar seçin.

1. Laboratuvarın Genel **Bakış** **alanında, İç destek'i**seçin.  

    ![Dahili destek düğmesi](./media/devtest-lab-internal-support-message/devtestlab-internal-support.png)


1. Bir destek iletisi deftere nakledilirse, kullanıcı bu iletiyi İç destek bölmesinden görüntüleyebilir.

    ![Gönderilen destek iletisi gösteren iç destek bölmesi](./media/devtest-lab-internal-support-message/devtestlab-view-suport-statement.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
* İç destek deyimleri genellikle bunu sık sık değiştirmeyen destek bilgileri sağlamak için kullanılır. Ayrıca, kullanıcıları geçici değişiklikler veya laboratuvar güncellemeleri hakkında bilgilendirmek için [bir duyuruyu laboratuvara](devtest-lab-announcements.md) nasıl yayınlayabileceğinizi de öğrenebilirsiniz.
* [İlkeler ve zamanlamaları ayarlayın,](devtest-lab-set-lab-policy.md) özelleştirilmiş ilkeler kullanarak aboneliğiniz genelinde diğer kısıtlamaları ve sözleşmeleri nasıl uygulayabileceğiniz hakkında bilgi sağlar.