---
author: baanders
description: Azure Digital TWINS kurulumunda rol gereksinimi için dosya ekleme
ms.service: digital-twins
ms.topic: include
ms.date: 7/17/2020
ms.author: baanders
ms.openlocfilehash: 2f10a6915a3edf673316ae9151b6052442678ef9
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87832372"
---
[!INCLUDE [cloud-shell-try-it.md](cloud-shell-try-it.md)]

## <a name="prerequisites-permission-requirements"></a>Önkoşullar: Izin gereksinimleri

Bu makaledeki tüm adımları tamamlayabilmeniz için, Azure aboneliğinizde bir sahip olarak sınıflandırılmalıdır. 

Bu komutu Cloud Shell ' de çalıştırarak izin düzeyinizi kontrol edebilirsiniz:

```azurecli-interactive
az role assignment list --assignee <your-Azure-email>
```

> [!NOTE]
> Bu komut, CLı 'nin **grafik veritabanında kullanıcı veya hizmet sorumlusu bulamadığını**söyleyen bir hata döndürürse:
>
> Bu makalenin geri kalanında, e-postanız yerine kullanıcının *nesne kimliğini* kullanın. Bu, kişisel [Microsoft hesaplarındaki (MSAs)](https://account.microsoft.com/account)kullanıcılar için gerçekleşebilir. 
>
> Kullanıcı hesabınızı seçmek ve ayrıntılarını açmak için [Azure Active Directory kullanıcıların Azure Portal sayfasını](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UsersManagementMenuBlade/AllUsers) kullanın. Kullanıcı *ObjectID*'yi kopyalayın:
>
> :::image type="content" source="../articles/digital-twins/media/includes/user-id.png" alt-text="' Nesne KIMLIĞI ' alanındaki GUID 'YI vurgulamak Azure portal Kullanıcı sayfasının görünümü" lightbox="../articles/digital-twins/media/includes/user-id.png":::
>
> Sonra, e-postanız yerine kullanıcının *nesne kimliğini* kullanarak rol atama listesi komutunu tekrarlayın.

Rol atama listesi komutunu çalıştırdıktan sonra, bir sahibiyseniz, `roleDefinitionName` çıktıdaki değer *sahip*olur:

:::image type="content" source="../articles/digital-twins/media/how-to-set-up-instance/cloud-shell/owner-role.png" alt-text="Az role atama listesi komutunun çıkışını gösteren Cloud Shell pencere":::

Değerin *katkıda* bulunduğunu veya *sahip*dışında bir şeyi fark ederseniz aşağıdaki yollarla devam edebilirsiniz:
* Sizin adınıza bu makaledeki adımları tamamlamaya yönelik abonelik sahibine ve sahibine yönelik isteğe başvurun
* Abonelik sahibine veya Kullanıcı erişimi yönetici rolüne sahip birine abone olur ve devam etmek için gereken izinlere sahip olacak şekilde aboneliğinizi aboneliğe sahip olarak yükseltmenizi isteyin. Bunun uygun olup olmadığı, kuruluşunuza ve onun içindeki rolünüze bağlıdır.