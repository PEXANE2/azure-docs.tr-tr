---
title: Azure DevTest Labs bir laboratuvara iç destek bildirisi ekleme
description: Azure DevTest Labs ' de bir laboratuvara iç destek beyanı nakletmeyi öğrenin
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 1c9920e6fe7fbfe2a8d0aeacb896150b342981b0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85480482"
---
# <a name="add-an-internal-support-statement-to-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs bir laboratuvara iç destek bildirisi ekleme

Azure DevTest Labs, laboratuvarınızı laboratuvar hakkında Destek bilgileri sunan dahili bir destek bildirimiyle özelleştirmenizi sağlar. Örneğin, bir kullanıcının laboratuvardaki kaynaklara sorun giderme veya erişim ihtiyacı olduğunda iç desteğe nasıl ulaşabulacağını bilmesi için iletişim bilgileri sağlayabilirsiniz. Ayrıca, ekibinizin destek ile iletişime geçmeden önce erişebileceği iç Web siteleri veya SSS bağlantılarını da sağlayabilirsiniz.

Bir iç destek bildirisi, genellikle çok sık değişmeyen laboratuvar bilgileri göndermenize imkan tanır. Laboratuvar ilkelerine yönelik son güncelleştirmeler gibi kullanıcılara daha geçici olan laboratuvar bilgilerini bildirmek için, bkz. [bir laboratuvarda duyuru gönderme](devtest-lab-announcements.md).

Artık geçerli olmadığında bir destek bildirisini kolayca devre dışı bırakabilir veya düzenleyebilirsiniz.

## <a name="steps-to-add-a-support-statement-to-an-existing-lab"></a>Mevcut laboratuvara destek beyanı ekleme adımları

1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Gerekirse, **tüm hizmetler**' i seçin ve ardından listeden **DevTest Labs** ' i seçin. (Laboratuvarınız panoda zaten **tüm kaynaklar**altında gösteriliyor olabilir).
1. Laboratuvarlar listesinden, bir destek ekstresi eklemek istediğiniz Laboratuvarı seçin.  
1. Laboratuvarın **genel bakış** alanında **yapılandırma ve ilkeler**' i seçin.  

    ![Yapılandırma ve ilkeler düğmesi](./media/devtest-lab-internal-support-message/devtestlab-config-and-policies.png)

1. Sol taraftaki **Ayarlar**' ın altında **iç destek**' i seçin.

    ![İç destek düğmesi](./media/devtest-lab-internal-support-message/devtestlab-internal-support.png)

1. Bu laboratuvardaki kullanıcılar için bir iç destek iletisi oluşturmak için, etkin ' i **Evet**olarak ayarlayın.

1. **Destek iletisi** alanına, laboratuvar kullanıcılarınıza sunmak istediğiniz iç destek bildirisini girin. Destek iletisi markın kabul eder. İleti metnini girerken, iletinin kullanıcılara nasıl göründüğünü görmek için ekranın altındaki **Önizleme** alanını görüntüleyebilirsiniz.

    ![İletiyi oluşturmak için iç destek ekranı.](./media/devtest-lab-internal-support-message/devtestlab-add-support-statement.png)


1. Destek deyiminiz gönderi için hazırsanız **Kaydet** ' i seçin.

Artık bu destek iletisini laboratuvar kullanıcılarına göstermek istemediğinizde, **iç destek** sayfasına dönün ve **etkin** ' i **Hayır**olarak ayarlayın.

## <a name="steps-for-users-to-view-the-support-message"></a>Destek iletisini görüntülemek için kullanıcılara yönelik adımlar

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040)bir laboratuvar seçin.

1. Laboratuvarın **genel bakış** alanında **iç destek**' i seçin.  

    ![İç destek düğmesi](./media/devtest-lab-internal-support-message/devtestlab-internal-support.png)


1. Bir destek iletisi gönderildiğinde, Kullanıcı bunu dahili destek bölmesinden görüntüleyebilir.

    ![Gönderilen destek iletisini gösteren iç destek bölmesi](./media/devtest-lab-internal-support-message/devtestlab-view-suport-statement.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
* İç destek deyimleri genellikle bu sıklıkla değişmeyen destek bilgilerini sağlamak için kullanılır. Ayrıca, kullanıcıları geçici değişiklikler veya laboratuvarda güncelleştirme hakkında bilgilendirmek için [bir laboratuvara duyuru nakletmeyi](devtest-lab-announcements.md) da öğrenebilirsiniz.
* [İlke ve zamanlamalar ayarlama](devtest-lab-set-lab-policy.md) , özelleştirilmiş ilkeleri kullanarak aboneliğinizde diğer kısıtlamaları ve kuralları nasıl uygulayabileceğiniz hakkında bilgi sağlar.