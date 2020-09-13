---
title: Portalı kullanarak çok kiracılı uygulamalardaki trafiği yönetme
titleSuffix: Azure Application Gateway
description: Bu makalede, Azure App Service Web Apps 'i var olan veya yeni bir uygulama ağ geçidinde arka uç havuzunda üye olarak yapılandırma hakkında rehberlik sunulmaktadır.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: how-to
ms.date: 06/09/2020
ms.author: absha
ms.openlocfilehash: daf4c4ab56e23a212256c2629b7d0ae52a09e7c4
ms.sourcegitcommit: 1b320bc7863707a07e98644fbaed9faa0108da97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89595719"
---
# <a name="configure-app-service-with-application-gateway"></a>Application Gateway ile App Service yapılandırma

App Service, bir dizi dağıtım yerine çok kiracılı bir hizmet olduğundan, isteği doğru App Service uç noktasına çözümlemek için gelen istekte ana bilgisayar üst bilgisini kullanır. Genellikle uygulamanın DNS adı, App Service 'i kapsayan uygulama ağ geçidi ile ilişkili DNS adıdır, arka uç App Service 'in etki alanı adından farklıdır. Bu nedenle, uygulama ağ geçidi tarafından alınan özgün istekteki ana bilgisayar üst bilgisi, arka uç hizmetinin ana bilgisayar adıyla aynı değildir. Bu nedenle, uygulama ağ geçidindeki istekteki ana bilgisayar üst bilgisi arka uç hizmetinin ana bilgisayar adına değiştirilmediği için, çok kiracılı arka uçlar isteği doğru uç noktaya çözemeyebilir.

Application Gateway, istek `Pick host name from backend address` Application Gateway arka uca yönlendirilmesinde isteğin ana bilgisayar adıyla birlikte istekteki konak üstbilgisini geçersiz kılan bir anahtar sağlar. Bu özellik, Azure App Service ve API Management gibi çok kiracılı arka uçlar için destek sunar. 

Bu makalede şunları öğreneceksiniz:

- Bir arka uç havuzu oluşturun ve buna bir App Service ekleyin
- "Seçme ana bilgisayar adı" anahtarları etkinken HTTP ayarları ve özel araştırma oluşturma

## <a name="prerequisites"></a>Ön koşullar

- Application Gateway: mevcut bir uygulama ağ geçidiniz yoksa bkz. [uygulama ağ geçidi oluşturma](https://docs.microsoft.com/azure/application-gateway/quick-create-portal)
- App Service: mevcut bir App Service yoksa [App Service belgeleri](https://docs.microsoft.com/azure/app-service/)bölümüne bakın.

## <a name="add-app-service-as-backend-pool"></a>App Service 'i arka uç havuzu olarak ekle

1. Azure portal, uygulama ağ geçidinizin yapılandırma görünümünü açın.

2. Yeni bir arka uç havuzu oluşturmak için **arka uç havuzları**altında **Ekle** ' ye tıklayın.

3. Arka uç havuzuna uygun bir ad verin. 

4. **Hedefler**' in altında, açılan listeye tıklayın ve seçenek olarak **uygulama hizmetleri** ' ni seçin.

5. **Hedefler** açılan listesinin hemen altındaki bir açılan liste, uygulama hizmetlerinizin listesini içerecek şekilde görünür. Bu açılan listeden, arka uç havuzu üyesi olarak eklemek istediğiniz App Service seçin ve Ekle ' ye tıklayın.

   ![App Service arka ucu](./media/configure-web-app-portal/backendpool.png)
   
   > [!NOTE]
   > Açılan menü yalnızca Application Gateway aynı abonelikte olan uygulama hizmetlerini doldurur. Application Gateway olduğu sunucudan farklı bir abonelikte bulunan bir App Service kullanmak istiyorsanız, **hedefler** açılan menüsünde **uygulama hizmetleri** ' ni seçmek yerine, **IP adresi veya ana bilgisayar adı** seçeneğini belirleyin ve ana bilgisayar adını (örneğin,) girin. azurewebsites.net).

## <a name="create-http-settings-for-app-service"></a>App Service için HTTP ayarları oluşturma

1. **Http ayarları**' nın altında **Ekle** ' ye tıklayarak yeni bir http ayarı oluşturun.

2. HTTP ayarı için bir ad girin ve ihtiyacınıza göre tanımlama bilgisi tabanlı benzeşimi etkinleştirebilir veya devre dışı bırakabilirsiniz.

3. Kullanım çalışmanıza göre Protokolü HTTP veya HTTPS olarak seçin. 

   > [!NOTE]
   > HTTPS ' yi seçerseniz, App Service güvenilen bir Azure hizmeti olduğundan, App Service arka ucuna izin vermek için herhangi bir kimlik doğrulama sertifikası veya güvenilen kök sertifikası yüklemeniz gerekmez.

4. **App Service Için kullanım** kutusunu işaretleyin. Anahtarların  `Create a probe with pick host name from backend address` ve `Pick host name from backend address` otomatik olarak etkinleştirildiğini unutmayın.`Pick host name from backend address` istek Application Gateway arka uca yönlendirildiğini, istekteki konak üstbilgisini arka ucun ana bilgisayar adıyla geçersiz kılar.  

   `Create a probe with pick host name from backend address` otomatik olarak bir durum araştırması oluşturur ve bu HTTP ayarıyla ilişkilendirir. Bu HTTP ayarı için başka bir sistem durumu araştırması oluşturmanız gerekmez. Bu ada sahip yeni bir araştırmanın <HTTP Setting name> <Unique GUID> sistem durumu araştırmaları listesine eklendiğini ve bu anahtarın zaten anahtara sahip olduğunu kontrol edebilirsiniz `Pick host name from backend http settings enabled` .

   App Service için kullanılmakta olan bir veya daha fazla HTTP ayarı zaten varsa ve bu HTTP ayarları, oluşturmakta olduğunuz bir protokol ile aynı protokolü kullanıyorsa, `Create a probe with pick host name from backend address` anahtar yerine, özel araştırmaların birini seçmek için bir açılır liste alırsınız. Bunun nedeni, App Service ile bir HTTP ayarı zaten var olduğundan, bu nedenle anahtara sahip bir sistem durumu araştırması de var `Pick host name from backend http settings enabled` . Açılan listeden özel araştırmayı seçin.

5. HTTP ayarını oluşturmak için **Tamam** ' ı tıklatın.

   ![HTTP-setting1](./media/configure-web-app-portal/http-setting1.png)

   ![HTTP-setting2](./media/configure-web-app-portal/http-setting2.png)



## <a name="create-rule-to-tie-the-listener-backend-pool-and-http-setting"></a>Dinleyiciyi, arka uç havuzunu ve HTTP ayarını bağlamak için kural oluşturma

1. **Kurallar**altında **temel** ' ya tıklayarak yeni bir temel kural oluşturun.

2. Uygun bir ad girin ve uygulama hizmeti için gelen istekleri kabul edecek olan dinleyiciyi seçin.

3. **Arka uç havuzu** açılan menüsünde, yukarıda oluşturduğunuz arka uç havuzunu seçin.

4. **Http ayarı** açılan listesinde, yukarıda oluşturduğunuz http ayarını seçin.

5. Bu kuralı kaydetmek için **Tamam** ' ı tıklatın.

   ![Kural](./media/configure-web-app-portal/rule.png)

## <a name="additional-configuration-in-case-of-redirection-to-app-services-relative-path"></a>App Service 'in göreli yoluna yeniden yönlendirme durumunda ek yapılandırma

App Service, göreli yoluna (örneğin, contoso.azurewebsites.net/path1 öğesinden contoso.azurewebsites.net/path2 'ye yeniden yönlendirme) yeniden yönlendirmek üzere istemciye bir yeniden yönlendirme yanıtı gönderdiğinde, uygulama ağ geçidinden aldığı istekte olduğu gibi yanıtının konum üstbilgisinde aynı ana bilgisayar adını kullanır. Bu nedenle, istemci, uygulama ağ geçidi (contoso.com/path2) boyunca değil, isteği doğrudan contoso.azurewebsites.net/path2 'a yapar. Application Gateway 'i atlamak istenmez.

Kullanım durumunda, App Service 'in istemciye yeniden yönlendirme yanıtı gönderebilmesi gereken senaryolar vardır, [konum başlığını yeniden yazmak için ek adımları](https://docs.microsoft.com/azure/application-gateway/troubleshoot-app-service-redirection-app-service-url#sample-configuration)gerçekleştirin.

## <a name="restrict-access"></a>Erişimi kısıtlama

Bu örneklerde dağıtılan Web uygulamaları, doğrudan Internet 'ten erişilebilen genel IP adreslerini kullanır. Bu, yeni bir özellik öğrenirken ve yeni şeyler gerçekleştirmeye çalışırken sorun gidermeye yardımcı olur. Ancak bir özelliği üretime dağıtmayı planlıyorsanız, daha fazla kısıtlama eklemek isteyeceksiniz.

Web uygulamalarınıza erişimi kısıtlayabilmeniz için bir yol [STATIK IP kısıtlamaları Azure App Service](../app-service/app-service-ip-restrictions.md)kullanmaktır. Örneğin, Web uygulamasını yalnızca uygulama ağ geçidinden trafik alacak şekilde kısıtlayabilirsiniz. Uygulama ağ geçidi VIP 'sini erişimi olan tek adres olarak listelemek için App Service IP kısıtlaması özelliğini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Uygulama hizmeti ve Application Gateway ile diğer çok kiracılı destek hakkında daha fazla bilgi edinmek için bkz. [Application Gateway ile çok kiracılı hizmet desteği](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-app-overview).
