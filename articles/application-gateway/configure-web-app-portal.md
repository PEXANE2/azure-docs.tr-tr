---
title: Portalı kullanarak çok kiracılı uygulamalardaki trafiği yönetme
titleSuffix: Azure Application Gateway
description: Bu makalede, Azure App Service Web Apps 'i var olan veya yeni bir uygulama ağ geçidinde arka uç havuzunda üye olarak yapılandırma hakkında rehberlik sunulmaktadır.
services: application-gateway
author: surajmb
ms.service: application-gateway
ms.topic: how-to
ms.date: 09/23/2020
ms.author: victorh
ms.openlocfilehash: df92e08e91761d77c606ccb5389eee7dc219c101
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91323403"
---
# <a name="configure-app-service-with-application-gateway"></a>Application Gateway ile App Service yapılandırma

App Service, bir dizi dağıtım yerine çok kiracılı bir hizmet olduğundan, isteği doğru App Service uç noktasına çözümlemek için gelen istekte ana bilgisayar üst bilgisini kullanır. Genellikle uygulamanın DNS adı, App Service 'i kapsayan uygulama ağ geçidi ile ilişkili DNS adıdır, arka uç App Service 'in etki alanı adından farklıdır. Bu nedenle, uygulama ağ geçidi tarafından alınan özgün istekteki ana bilgisayar üst bilgisi, arka uç hizmetinin ana bilgisayar adıyla aynı değildir. Bu nedenle, uygulama ağ geçidindeki istekteki ana bilgisayar üst bilgisi arka uç hizmetinin ana bilgisayar adına değiştirilmediği için, çok kiracılı arka uçlar isteği doğru uç noktaya çözemeyebilir.

Application Gateway, istek `Pick host name from backend target` Application Gateway arka uca yönlendirilmesinde isteğin ana bilgisayar adıyla birlikte istekteki konak üstbilgisini geçersiz kılan bir anahtar sağlar. Bu özellik, Azure App Service ve API Management gibi çok kiracılı arka uçlar için destek sunar. 

Bu makalede şunları öğreneceksiniz:

- Bir arka uç havuzunu düzenleyin ve buna bir App Service ekleyin
- ' Seçim konak adı ' anahtarı etkin HTTP ayarlarını Düzenle

## <a name="prerequisites"></a>Önkoşullar

- Application Gateway: arka uç havuzu hedefi olmadan bir uygulama ağ geçidi oluşturun. Daha fazla bilgi için bkz [. hızlı başlangıç: Azure Application Gateway doğrudan web trafiği-Azure Portal](quick-create-portal.md)

- App Service: mevcut bir App Service yoksa [App Service belgeleri](https://docs.microsoft.com/azure/app-service/)bölümüne bakın.

## <a name="add-app-service-as-backend-pool"></a>App Service 'i arka uç havuzu olarak ekle

1. Azure portal, uygulama ağ geçidinizi seçin.

2. **Arka uç havuzları**altında arka uç havuzunu seçin.

4. **Hedef türü**altında **uygulama hizmetleri**' ni seçin.

5. **Hedef** altında App Service seçin.

   :::image type="content" source="./media/configure-web-app-portal/backend-pool.png" alt-text="App Service arka ucu":::
   
   > [!NOTE]
   > Açılan liste yalnızca Application Gateway aynı abonelikte olan uygulama hizmetlerini doldurur. Application Gateway olduğu sunucudan farklı bir abonelikte bulunan bir App Service kullanmak istiyorsanız, **hedefler** açılan menüsünde **uygulama hizmetleri** ' ni seçmek yerine, **IP adresi veya ana bilgisayar adı** seçeneğini belirleyin ve ana bilgisayar adını (örneğin,) girin. azurewebsites.net).
1. **Kaydet**’i seçin.

## <a name="edit-http-settings-for-app-service"></a>App Service için HTTP ayarlarını Düzenle

1. **Http ayarları**altında var olan http ayarını seçin.

2. **Yeni ana bilgisayar adı Ile geçersiz kıl**altında **Evet**' i seçin.
3. **Konak adı geçersiz kılma**altında, **arka uç hedefinden konak adı**Seç ' i seçin.
4. **Kaydet**’i seçin.

   :::image type="content" source="./media/configure-web-app-portal/http-settings.png" alt-text="Arka uç http ayarlarından konak adı Seç":::

## <a name="additional-configuration-in-case-of-redirection-to-app-services-relative-path"></a>App Service 'in göreli yoluna yeniden yönlendirme durumunda ek yapılandırma

App Service, göreli yoluna (örneğin, öğesinden bir yeniden yönlendirme) yeniden yönlendirmek üzere istemciye bir yeniden yönlendirme yanıtı gönderdiğinde `contoso.azurewebsites.net/path1` `contoso.azurewebsites.net/path2` , uygulama ağ geçidinden aldığı istekte olduğu gibi yanıtının konum üstbilgisinde aynı ana bilgisayar adını kullanır. Bu nedenle, istemci, `contoso.azurewebsites.net/path2` uygulama ağ geçidi () boyunca değil, isteği doğrudan bir hale getirir `contoso.com/path2` . Application Gateway 'i atlamak istenmez.

Kullanım durumunda, App Service 'in istemciye yeniden yönlendirme yanıtı gönderebilmesi gereken senaryolar vardır, [konum başlığını yeniden yazmak için ek adımları](https://docs.microsoft.com/azure/application-gateway/troubleshoot-app-service-redirection-app-service-url#sample-configuration)gerçekleştirin.

## <a name="restrict-access"></a>Erişimi kısıtlama

Bu örneklerde dağıtılan Web uygulamaları, doğrudan Internet 'ten erişilebilen genel IP adreslerini kullanır. Bu, yeni bir özellik öğrenirken ve yeni şeyler gerçekleştirmeye çalışırken sorun gidermeye yardımcı olur. Ancak bir özelliği üretime dağıtmayı planlıyorsanız, daha fazla kısıtlama eklemek isteyeceksiniz.

Web uygulamalarınıza erişimi kısıtlayabilmeniz için bir yol [STATIK IP kısıtlamaları Azure App Service](../app-service/app-service-ip-restrictions.md)kullanmaktır. Örneğin, Web uygulamasını yalnızca uygulama ağ geçidinden trafik alacak şekilde kısıtlayabilirsiniz. Uygulama ağ geçidi VIP 'sini erişimi olan tek adres olarak listelemek için App Service IP kısıtlaması özelliğini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Uygulama hizmeti ve Application Gateway ile diğer çok kiracılı destek hakkında daha fazla bilgi edinmek için bkz. [Application Gateway ile çok kiracılı hizmet desteği](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-app-overview).
