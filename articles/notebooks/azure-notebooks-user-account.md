---
title: Azure Dizüstü Bilgisayarlar önizlemesi nde oturum açın
description: Bir Microsoft hesabı veya bir iş/okul hesabı kullanarak azure not defterleri için kullanıcı hesabınızı yapılandırın.
ms.topic: conceptual
ms.date: 04/15/2019
ms.openlocfilehash: 9401a9e483f3f8db5af827f53937cb0f01b27a28
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75646305"
---
# <a name="your-user-account-for-azure-notebooks-preview"></a>Azure Not Defterleri Önizleme si için kullanıcı hesabınız

Azure Not Defterlerini bir kullanıcı hesabıyla oturum açmadan veya oturum açmadan kullanabilirsiniz:

- Oturum açmadan not defterleri oluşturabilir ve çalıştırabilirsiniz, ancak projelerin bir parçası olarak not defterlerini veya veri dosyalarını tutamazsınız. Örneğin, bir Azure Not Defteri'ne bağlantı alan kullanıcılar, oturum açmalarına gerek kalmadan not defterinin keyfini çıkarabilir.
- Azure Dizüstü Bilgisayarlar oturum açıldığında tüm projelerinizi hesabınızda saklar. Oturum açmış kullanıcıların, projelerini ve not defterlerini başkalarıyla paylaşmalarını sağlayan bir kullanıcı kimliği de vardır.
- Azure Dizüstü Bilgisayarlar için kullanılan hesap bir Azure Aboneliği ile de ilişkilendirildiğinde, daha güçlü sunucularda not defterleri çalıştırmak, özel not defterleri oluşturmak ve tek tek kullanıcılara not defterlerine izin vermek gibi ek avantajlar elde elabilirsiniz.

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

Azure Not Defterleri'nde oturum açma için bir Microsoft Hesabı veya "İş veya Okul" hesabı gerekir. Not Defterleri sayfasının sağ üst tarafındaki **Oturum Aç** komutunu seçerken hesabınız için istenir:

![Azure Dizüstü Bilgisayarlar için oturum açma komutu](media/accounts/sign-in-command.png)

Azure Not Defterleri'nde yaptığınız tüm işler oturum açmada kullandığınız hesapla ilişkilidir. Her hesabın [kullanıcı profilinizde](azure-notebooks-user-profile.md)de benzersiz bir kullanıcı kimliği olması gerekir. Sonuç olarak, ayrı proje kümeleri ve ayrı kimlikler korumanız gerekiyorsa, Azure Not Defterlerinde farklı hesaplarla oturum açabilirsiniz. Örneğin, bir veri bilimi ekibinin her üyesi, çalışmalarını şirket dışındaki kişilere sunmak için kullandıkları paylaşılan bir grup hesabı olarak her iki ayrı hesaba da sahip olabilir. Eğitmenler, benzer şekilde, dış danışmanlık veya açık kaynak çalışmalarında kullanılan bir hesapfarklı öğretim rolü için bir hesap korumak olabilir.

## <a name="microsoft-accounts"></a>Microsoft hesapları

Microsoft hesapları, Windows, Azure, outlook.com, OneDrive ve XBox Live gibi microsoft ürün ve hizmetlerinde oturum açmada kullanılır. Bu hizmetlerden herhangi birini kullanıyorsanız, Azure Not Defterleri ile kullanabileceğiniz bir Microsoft Hesabınız olması olasıdır.

Emin değilseniz, hesap isteminde **Bir Oluştur** komutunu seçin. Herhangi bir sağlayıcıdan gelen herhangi bir e-posta adresini kullanarak yeni bir Microsoft hesabı oluşturabilirsiniz.

![Yeni bir Microsoft hesabı oluşturma komutu](media/accounts/create-new-microsoft-account.png)

> [!Note]
> Zaten ilişkili bir hesabı olan bir e-posta adresine sahip yeni bir hesap oluşturmaya çalışırsanız, "Buraya bir iş veya okul e-posta adresiyle kaydolamazsınız. Gmail veya Yahoo! gibi kişisel bir e-posta kullanın veya yeni bir Outlook e-postası alın." Bu durumda, yeni bir hesap oluşturmadan iş e-posta adresiyle oturum açmayı deneyin.

Alt hesaplarda Azure Not Defterlerine erişim varsayılan olarak engellenir. Alt hesapla oturum açma, aşağıda gösterilen hatayı görüntüler:

![Bir alt hesapla oturum açmaya çalışırken hata: bir şeyler yanlış gitti, ebeveyniniz erişimi engelledi](media/accounts/child-account-error.png)

Erişimi etkinleştirmek için bir üst öğeaşağıdaki adımları yapmalıdır:

1. Bir `https://account.live.com/mk` üst hesapla ziyaret edin ve oturum açın.
1. Söz konusu çocuk için bölümde, **bu çocuğun üçüncü taraf uygulamalara erişimini yönet'i**seçin.
1. Sonraki sayfada erişimi **etkinleştir'i**seçin.
1. Alt hesap Azure Not Defterleri'nde oturum açmak için bir sonraki kullanıldığında, görünen izin isteminde **Evet'i** seçin.

> [!Warning]
> Azure Dizüstü Bilgisayarlar için üçüncü taraf uygulamalara erişimi etkinleştirmek, diğer tüm üçüncü taraf uygulamalara da erişim sağlar. Ebeveynlerin erişimi etkinleştirirken sağduyulu olmaları tavsiye edilir ve çocuklarının faaliyetlerini daha yakından izlemek isteyebilirler.

## <a name="work-or-school-accounts"></a>İş veya okul hesapları

Bir iş veya okul hesabı, kuruluşun bir üyesinin Office 365 gibi Microsoft bulut hizmetlerine erişmesini ve aynı zamanda etki alanı yla birleştirilmiş bir bilgisayarda Windows'da oturum açmak için hesap olarak bir kuruluş yöneticisi tarafından oluşturulur. Bir çalışma veya okul hesabı genellikle kuruluş e-posta adresi kullanır, örneğin. any-user@contoso.com

Azure Not Defterleri'nde bir iş veya okul hesabıyla oturum açma, Hesabın e-posta adresi ve kullanıcının tarayıcı bilgileri gibi bilgileri topladığı veya kullandığı (ancak açıklamadığı) için yönetici onayı gerektirebilir. (Tarayıcı verileri, özellikleri popüler kullanıma göre optimize etmek için kullanılır.)

Bir kuruluş hesabının yöneticisi, kullanıcıların tek tek rıza göstermesi kısıtlanmışsa, kullanıcılar adına onay vermelidir. Bu durumda, kullanıcılar "Bu uygulamaya erişemezsiniz" iletisini görür:

![Bir iş veya okul hesabı kullanırken "Bu uygulamaya erişemezsiniz" iletisi](media/accounts/consent-permissions-denied.png)

Yönetici olarak onay sağlamak için, işlem boyunca size yol gösteren [yönetici onay sayfasını](https://notebooks.azure.com/account/adminConsent)kullanın.

## <a name="next-steps"></a>Sonraki adımlar  

> [!div class="nextstepaction"]
> [Profilinizi ve kullanıcı kimliğinizi edin](azure-notebooks-user-profile.md)
