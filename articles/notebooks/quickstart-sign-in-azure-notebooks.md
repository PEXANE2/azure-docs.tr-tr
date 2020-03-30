---
title: Azure Dizüstü Bilgisayarlar önizlemesinde oturum açın
description: Azure Notebook Önizleme'de hızlı bir şekilde oturum açın ve kaydedilmiş projelere erişmenizi ve not defterlerini başkalarıyla paylaşma nızı sağlayan bir kullanıcı kimliği ayarlayın.
ms.topic: quickstart
ms.date: 04/15/2019
ms.openlocfilehash: b6572a7c0b965b2b72916db577b47eff4f1921c0
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75647025"
---
# <a name="quickstart-sign-in-and-set-a-user-id-for-azure-notebooks-preview"></a>Quickstart: Azure Dizüstü Bilgisayarlar Önizleme için oturum açın ve kullanıcı kimliği ayarlayın

Oturum açmadan Azure Not Defterlerini her zaman görüntüleyebiliyor olsanız da, not defterlerini çalıştırmak, kaydedilmiş projelere ve not defterlerine erişmek ve not defterlerinizi başkalarıyla paylaşmak için oturum açmanız gerekir.

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

## <a name="sign-in"></a>Oturum aç

1. [notebooks.azure.com](https://notebooks.azure.com/)sağ üst kısmında **Oturum Aç'ı** seçin.

    ![Azure Not Defterlerinde Oturum Açma komutunun konumu](media/accounts/sign-in-command.png)

1. İstendiğinde, bir Microsoft Hesabının veya bir iş veya okul hesabının e-posta adresini girin ve **İleri'yi**seçin. Hesap türleri [Azure Not Defterleri için kullanıcı hesabınızda](azure-notebooks-user-account.md)açıklanmıştır. Microsoft Hesabınız yoksa veya özellikle Azure Notebook'larda kullanmak istiyorsanız, **bir tane oluştur'u**seçin:

    ![Oturum açma isteminde yeni Microsoft hesap komutu oluşturma](media/accounts/create-new-microsoft-account.png)

    > [!Tip]
    > Zaten ilişkili bir hesabı olan bir e-posta adresine sahip yeni bir hesap oluşturmaya çalışırsanız, "Buraya bir iş veya okul e-posta adresiyle kaydolamazsınız. Gmail veya Yahoo! gibi kişisel bir e-posta kullanın veya yeni bir Outlook e-postası alın." Bu durumda, yeni bir hesap oluşturmadan iş e-posta adresiyle oturum açmayı deneyin.

1. İstendiğinde parolanızı girin.

1. Azure Dizüstü Bilgisayarlar ilk kez oturum açtıysanız, hesabınıza erişmek için izin ister. Devam etmek için **Evet'i** seçin:

    ![Hesap izinleri istemi](media/accounts/account-permission-prompt.png)

## <a name="set-a-user-id"></a>Kullanıcı kimliği ayarlama

1. İlk oturum açTınca, "anon-idrca3" gibi geçici bir kullanıcı kimliği atanırsınız. Azure Dizüstü Bilgisayarlar, "anon-" ile başlayan bir kullanıcı kimliğiniz olduğunda, sizden kendi kimliğinizi oluşturmanızı ister. Kullanıcı kimliğiniz, projelerinizi ve not defterlerinizi paylaşmak için elde ettiğiniz herhangi bir URL'de kullanılır, bu nedenle sizin için benzersiz ve anlamlı bir şey seçin.

    ![Azure Dizüstü Bilgisayarlar için kullanıcı kimliği girme istemi](media/accounts/create-user-id.png)

    **Teşekkür Yok'u**seçerseniz, Azure Not Defterleri her oturum açtığınızda sizden kullanıcı kimliği isteyebilir. Kullanıcı kimliğiniz kullanıcı [profilinizde](azure-notebooks-user-profile.md)herhangi bir zamanda da ayarlanabilir.

1. Azure Dizüstü Bilgisayarlar başarılı bir şekilde oturum açıldıktan sonra herkese açık profil sayfanıza giderek bilgilerinizin geri kalanını doldurmak için **Profil Bilgilerini Edit'i** seçebilirsiniz (daha fazla bilgi için [profilinize ve kullanıcı kimliğinize](azure-notebooks-user-profile.md)bakın):

    ![Azure Notebook'lar profil sayfasının ilk görünümü](media/accounts/profile-page-new.png)

> [!NOTE]
> "Kullanıcı Kimliği zaten kullanılıyor" iletisini görüyorsanız, farklı bir kimlik deneyin. Kullanıcı adları tüm Azure Not Defterleri hesaplarında benzersizdir ve Azure Not Defterleri Microsoft marka adları gibi belirli kullanıcı adlarını da saklı tutar.

## <a name="sign-out"></a>Oturumu kapat

Oturumunuzu imzalamak için sayfanın sağ üst kısmındaki kullanıcı adınızı seçin ve ardından **Oturum Aç'ı**seçin:

![Azure Not Defterleri'nde oturum açma komutunun konumu](media/accounts/sign-out-command.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hızlı başlatma: Not defteri oluşturma ve paylaşma](quickstart-create-share-jupyter-notebook.md)
