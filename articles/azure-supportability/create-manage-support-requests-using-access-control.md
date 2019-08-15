---
title: Destek isteklerini oluşturmak ve yönetmek için erişim haklarını denetlemek üzere Azure rol tabanlı Access Control (RBAC) | Microsoft Docs
description: Destek isteklerini oluşturmak ve yönetmek için erişim haklarını denetlemek üzere Azure rol tabanlı Access Control (RBAC)
author: ganganarayanan
ms.author: gangan
ms.date: 1/31/2017
ms.topic: article
ms.service: azure
ms.assetid: 58a0ca9d-86d2-469a-9714-3b8320c33cf5
ms.openlocfilehash: 3cf17f6e391608af9d17591a81c579a1db779a6a
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68967800"
---
# <a name="azure-role-based-access-control-rbac-to-control-access-rights-to-create-and-manage-support-requests"></a>Destek isteklerini oluşturmak ve yönetmek için erişim haklarını denetlemek üzere Azure rol tabanlı Access Control (RBAC)

[Rol tabanlı Access Control (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview) , Azure için ayrıntılı erişim yönetimine izin vermez.
Azure portal [portal.azure.com](https://portal.azure.com)destek isteği oluşturma, Azure 'un, destek istekleri oluşturup yönetebileceğini tanımlamak için Azure 'un RBAC modelini kullanır.
Bir abonelik, kaynak grubu veya kaynak olabilen belirli bir kapsamdaki kullanıcılara, gruplara ve uygulamalara uygun RBAC rolü atanarak erişim verilir.

Bir örnek alalım: Abonelik kapsamında okuma izinleri olan bir kaynak grubu sahibi olarak, kaynak grubu altında Web siteleri, sanal makineler ve alt ağlar gibi tüm kaynakları yönetebilirsiniz.
Ancak, sanal makine kaynağına karşı bir destek isteği oluşturmaya çalıştığınızda, aşağıdaki hatayla karşılaşırsınız

![Abonelik hatası](./media/create-manage-support-requests-using-access-control/subscription-error.png)

Destek isteği Yönetimi ' nde, destek istekleri oluşturup yönetebilmek için abonelik kapsamında Microsoft. support/* destek eylemine sahip bir rol yazma iznine sahip olmalıdır.

Aşağıdaki makalede, Azure portal destek istekleri oluşturmak ve yönetmek için Azure 'un özel rol tabanlı Access Control (RBAC) nasıl kullanabileceğiniz açıklanmaktadır.

## <a name="getting-started"></a>Başlarken

Yukarıdaki örneği kullanarak, abonelik sahibi tarafından abonelikte özel bir RBAC rolü atandıysa, kaynağınız için bir destek isteği oluşturabilirsiniz.
Azure PowerShell, Azure komut satırı arabirimi (CLı) ve REST API kullanılarak [özel RBAC rolleri](https://azure.microsoft.com/documentation/articles/role-based-access-control-custom-roles/) oluşturulabilir.

Özel bir rolün Actions özelliği rolün erişim izni verdiği Azure işlemlerini belirtir.
Destek isteği yönetimi için özel bir rol oluşturmak üzere, rol Microsoft. support/* eylemine sahip olmalıdır

Destek isteklerini oluşturmak ve yönetmek için kullanabileceğiniz özel bir rol örneği aşağıda verilmiştir.
Bu rolü "destek Isteği katılımcısı" olarak adlandırdık ve bu makalede özel role nasıl başvurduğumuz.

``` Json
{
    "Name":  "Support Request Contributor",
    "Id":  "1f2aad59-39b0-41da-b052-2fb070bd7942",
    "IsCustom":  true,
    "Description":  "Lets you create and manage support tickets.",
    "Actions":  [
                    "Microsoft.Support/*"
                ],
    "NotActions":  [
                   ],
    "AssignableScopes":  [
                             "/"
                         ]
}
```

Aboneliğiniz için özel rol oluşturma hakkında bilgi edinmek için [Bu videoda](https://www.youtube.com/watch?v=-PaBaDmfwKI) özetlenen adımları izleyin.

## <a name="create-and-manage-support-requests-in-the-azure-portal"></a>Azure portal destek istekleri oluşturma ve yönetme

Bir örnek alalım: "Visual Studio MSDN aboneliği" aboneliğine sahip olursunuz.
Ali, Bu abonelikteki kaynak gruplarının bazılarına kaynak sahibi olan ve abonelik için okuma iznine sahip olan bir eşdir.
Bu abonelik kapsamındaki kaynaklar için destek bileti oluşturma ve yönetme olanağı olan Joe, eşdüzey verilerinize erişim vermek istiyorsunuz.

1. İlk adım aboneliğe gidemez. **Ayarlar**' ın altında, kullanıcıların listesini görürsünüz. Abonelik üzerinde okuyucu erişimi olan ali kullanıcısını seçin. Ali 'e yeni bir özel rol atalim.

    ![Rol ekle](./media/create-manage-support-requests-using-access-control/add-role.png)

2. "Kullanıcılar" dikey penceresinde "Ekle" ye tıklayın. Rol listesinden "destek Isteği katılımcısı" özel rolünü seçin

    ![Destek katılımcısı rolü Ekle](./media/create-manage-support-requests-using-access-control/add-support-contributor-role.png)

3. Rol adını seçtikten sonra, "Kullanıcı Ekle" ye tıklayın ve ali 'nin e-posta kimlik bilgilerini girin. "Seç"e tıklayın

    ![Kullanıcı ekle](./media/create-manage-support-requests-using-access-control/add-users.png)

4. Devam etmek için "Tamam" a tıklayın

    ![Erişim ekle](./media/create-manage-support-requests-using-access-control/add-access.png)

5. Şimdi, sahibi olduğunuz aboneliğin altında yeni eklenen "destek Isteği katılımcısı" özel rolüne sahip kullanıcıyı görürsünüz

    ![Kullanıcı eklendi](./media/create-manage-support-requests-using-access-control/user-added.png)

    Ali portalda oturum açtığında ali, ali 'nin eklendiği aboneliği görür.

7. Ali "yardım ve destek" dikey penceresinde "Yeni Destek isteği" öğesini tıklatır ve "Visual Studio Ultimate with MSDN" için destek istekleri oluşturabilir

    ![Yeni destek isteği](./media/create-manage-support-requests-using-access-control/new-support-request.png)

8. "Tüm destek istekleri" ali ' ne tıklamak, bu abonelik ![durum ayrıntıları görünümü için oluşturulan Destek isteklerinin listesini görebilir](./media/create-manage-support-requests-using-access-control/case-details-view.png)

## <a name="remove-support-request-access-in-the-azure-portal"></a>Azure portal destek isteği erişimini kaldırma

Destek isteklerini oluşturmak ve yönetmek için bir kullanıcıya erişim izni verilmesi mümkün olduğu gibi, Kullanıcı erişimini de kaldırmak mümkündür.

Destek istekleri oluşturma ve yönetme yeteneğini kaldırmak için, aboneliğe gidin, "Ayarlar" a tıklayın ve kullanıcıya (Bu durumda Joe) tıklayın. "Destek Isteği katılımcısı" rol adına sağ tıklayın ve "Kaldır" a tıklayın

![Destek isteği erişimini kaldır](./media/create-manage-support-requests-using-access-control/remove-support-request-access.png)

Ali portalda oturum açtığında ve bir destek isteği oluşturmaya çalıştığında, ali aşağıdaki hatayla karşılaşır:

![Abonelik hatası-2](./media/create-manage-support-requests-using-access-control/subscription-error-2.png)

Ali, Joe "tüm destek istekleri" seçildiğinde herhangi bir destek isteği göremez

![büyük/küçük harf Ayrıntıları görünümü-2](./media/create-manage-support-requests-using-access-control/case-details-view-2.png)
