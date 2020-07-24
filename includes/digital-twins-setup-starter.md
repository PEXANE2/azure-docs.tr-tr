---
author: baanders
description: Azure dijital TWINS kurulumu 'nda Başlatıcı bilgileri için dosya ekleme
ms.service: digital-twins
ms.topic: include
ms.date: 7/22/2020
ms.author: baanders
ms.openlocfilehash: 3f19674c0e25ebf5cddc2dfb45580a15d04fad46
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87099886"
---
>[!NOTE]
>Bu işlemlerin, Azure aboneliğinde *sahip* rolü olan bir kullanıcı tarafından tamamlanması amaçlanmıştır. Bu yükseltilmiş izin olmadan bazı parçalar tamamlanabilir, ancak kullanılabilir bir örneği tamamen ayarlamak için sahibin birlikte çalışması gerekecektir. [*Önkoşul: gerekli izinler*](#prerequisites-permission-requirements) bölümünde bunun hakkında daha fazla bilgi görüntüleyin.

Yeni bir Azure dijital TWINS örneği için tam kurulum üç bölümden oluşur:
1. **Örnek oluşturma**
2. Kullanıcılarınızın **erişim Izinlerini ayarlama**: Azure kullanıcılarınızın yönetim etkinliklerini gerçekleştirmek Için örnekte *Azure Digital TWINS Owner (Önizleme)* rolüne sahip olması gerekir. Bu adımda kendinize bu rolü atayacaksınız (Azure aboneliğinizdeki bir sahibiyseniz) ya da aboneliğinizi size atamak için aboneliğinize sahip olursunuz.
3. **İstemci uygulamaları için erişim Izinlerini ayarlama**: örneğinizle etkileşim kurmak için kullandığınız bir istemci uygulaması yazmak yaygındır. Bu istemci uygulamanın Azure dijital TWINS 'nize erişmesi için, istemci uygulamanın örnekte kimlik doğrulamak üzere kullanacağı [Azure Active Directory](../articles/active-directory/fundamentals/active-directory-whatis.md) bir *uygulama kaydı* ayarlamanız gerekir.

Devam etmek için bir Azure aboneliğine ihtiyacınız olacaktır. [Buradan](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)birini ücretsiz olarak ayarlayabilirsiniz.

[!INCLUDE [cloud-shell-try-it.md](cloud-shell-try-it.md)]

## <a name="prerequisites-permission-requirements"></a>Önkoşullar: Izin gereksinimleri

Bu makaledeki tüm adımları tamamlayabilmeniz için, Azure aboneliğinizde bir sahip olarak sınıflandırılmalıdır. 

Bu komutu Cloud Shell ' de çalıştırarak izin düzeyinizi kontrol edebilirsiniz:

```azurecli-interactive
az role assignment list --assignee <your-Azure-email>
```

Sahibiyseniz, `roleDefinitionName` çıkışdaki değer *sahipdir*:

:::image type="content" source="../articles/digital-twins/media/how-to-set-up-instance/owner-role.png" alt-text="Az role atama listesi komutunun çıkışını gösteren Cloud Shell pencere":::

Değerin *katkıda* bulunduğunu veya *sahip*dışında bir şeyi fark ederseniz, abonelik sahibine başvurarak aşağıdaki yollarla devam edebilirsiniz:
* Bu makaledeki adımları gerçekleştirmek için sahip iste
* Size aboneliğin sahibine yönelik olarak sahip olmasını isteyin ve böylece kendinize devam etmek için izinleriniz olur. Bunun uygun olup olmadığı, kuruluşunuza ve onun içindeki rolünüze bağlıdır.
