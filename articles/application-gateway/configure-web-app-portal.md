---
title: Azure Application Gateway-portal ile App Service Web Apps gibi çok kiracılı uygulamalardaki trafiği yönetme
description: Bu makalede, Azure App Service Web Apps 'i var olan veya yeni bir uygulama ağ geçidinde arka uç havuzunda üye olarak yapılandırma hakkında rehberlik sunulmaktadır.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 3/11/2019
ms.author: absha
ms.openlocfilehash: dee4859c57172a703517848510a31b70ff1f24cd
ms.sourcegitcommit: c71306fb197b433f7b7d23662d013eaae269dc9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68370419"
---
# <a name="configure-app-service-with-application-gateway"></a>Application Gateway ile App Service yapılandırma

App Service, bir dizi dağıtım yerine çok kiracılı bir hizmet olduğundan, isteği doğru App Service uç noktasına çözümlemek için gelen istekte ana bilgisayar üst bilgisini kullanır. Genellikle uygulamanın DNS adı, App Service 'i kapsayan uygulama ağ geçidi ile ilişkili DNS adıdır, arka uç App Service 'in etki alanı adından farklıdır. Bu nedenle, uygulama ağ geçidi tarafından alınan özgün istekteki ana bilgisayar üst bilgisi, arka uç hizmetinin ana bilgisayar adıyla aynı değildir. Bu nedenle, uygulama ağ geçidindeki istekteki ana bilgisayar üst bilgisi arka uç hizmetinin ana bilgisayar adına değiştirilmediği için, çok kiracılı arka uçlar isteği doğru uç noktaya çözemeyebilir.

Application Gateway, istek Application Gateway arka `Pick host name from backend address` uca yönlendirilmesinde isteğin ana bilgisayar adıyla birlikte istekteki konak üstbilgisini geçersiz kılan bir anahtar sağlar. Bu özellik, Azure App Service ve API Management gibi çok kiracılı arka uçlar için destek sunar. 

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
>
> - Bir arka uç havuzu oluşturun ve buna bir App Service ekleyin
> - "Seçme ana bilgisayar adı" anahtarları etkinken HTTP ayarları ve özel araştırma oluşturma

## <a name="prerequisites"></a>Önkoşullar

- Uygulama ağ geçidi: Mevcut bir uygulama ağ geçidiniz yoksa bkz. [uygulama ağ geçidi oluşturma](https://docs.microsoft.com/azure/application-gateway/quick-create-portal)
- App Service: Mevcut bir App Service yoksa [App Service belgeleri](https://docs.microsoft.com/azure/app-service/)bölümüne bakın.

## <a name="add-app-service-as-backend-pool"></a>App Service 'i arka uç havuzu olarak ekle

1. Azure portal, uygulama ağ geçidinizin yapılandırma görünümünü açın.

2. Yeni bir arka uç havuzu oluşturmak için **arka uç havuzları**altında **Ekle** ' ye tıklayın.

3. Arka uç havuzuna uygun bir ad verin. 

4. **Hedefler**' in altında, açılan listeye tıklayın ve seçenek olarak **uygulama hizmetleri** ' ni seçin.

5. **Hedefler** açılan listesinin hemen altındaki bir açılan liste, uygulama hizmetlerinizin listesini içerecek şekilde görünür. Bu açılan listeden, arka uç havuzu üyesi olarak eklemek istediğiniz App Service seçin ve Ekle ' ye tıklayın.

   ![App Service arka ucu](./media/configure-web-app-portal/backendpool.png)
   
   > [!NOTE]
   > Açılan menü yalnızca Application Gateway aynı abonelikte olan uygulama hizmetlerini doldurur. Application Gateway olduğu sunucudan farklı bir abonelikte bulunan bir App Service kullanmak istiyorsanız, **hedefler** açılan menüsünde **uygulama hizmetleri** ' ni seçmek yerine, **IP adresi veya ana bilgisayar adı** seçeneğini belirleyin ve Ana bilgisayar adı (örnek. azurewebsites.net).

## <a name="create-http-settings-for-app-service"></a>App Service için HTTP ayarları oluşturma

1. **Http ayarları**' nın altında **Ekle** ' ye tıklayarak yeni bir http ayarı oluşturun.

2. HTTP ayarı için bir ad girin ve ihtiyacınıza göre tanımlama bilgisi tabanlı benzeşimi etkinleştirebilir veya devre dışı bırakabilirsiniz.

3. Kullanım çalışmanıza göre Protokolü HTTP veya HTTPS olarak seçin. 

   > [!NOTE]
   > HTTPS ' yi seçerseniz, App Service 'ın güvenilen bir Azure hizmeti olduğundan, uygulama hizmeti arka ucunu beyaz listeye almak için herhangi bir kimlik doğrulama sertifikası veya güvenilen kök sertifikası yüklemeniz gerekmez.

4. **App Service Için kullanım** kutusunu işaretleyin. Anahtarların `Create a probe with pick host name from backend address` ve`Pick host name from backend address` otomatik olarak etkinleştirildiğini unutmayın.`Pick host name from backend address` istek Application Gateway arka uca yönlendirildiğini, istekteki konak üstbilgisini arka ucun ana bilgisayar adıyla geçersiz kılar.  

   `Create a probe with pick host name from backend address`otomatik olarak bir durum araştırması oluşturur ve bu HTTP ayarıyla ilişkilendirir. Bu HTTP ayarı için başka bir sistem durumu araştırması oluşturmanız gerekmez. Bu ada <HTTP Setting name> <Unique GUID> sahip yeni bir araştırmanın sistem durumu araştırmaları listesine eklendiğini ve bu anahtarın zaten anahtara `Pick host name from backend http settings enabled`sahip olduğunu kontrol edebilirsiniz.

   App Service için kullanılmakta olan bir veya daha fazla http ayarı zaten varsa ve bu http ayarları, oluşturmakta olduğunuz bir protokol ile aynı protokolü kullanıyorsa, `Create a probe with pick host name from backend address` anahtar yerine, c 'den birini seçmek üzere bir açılan menü alacaksınız. Özel yoklamalar. Bunun nedeni, App Service ile bir HTTP ayarı zaten var olduğundan, bu nedenle anahtara `Pick host name from backend http settings enabled` sahip bir sistem durumu araştırması de var. Açılan listeden özel araştırmayı seçin.

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

App Service, göreli yoluna (örneğin, contoso.azurewebsites.net/path1 öğesinden contoso.azurewebsites.net/path2 'ye yeniden yönlendirme) yeniden yönlendirmek üzere istemciye yeniden yönlendirme yanıtı gönderdiğinde, yanıtının konum üstbilgisinde aynı ana bilgisayar adını kullanır uygulama ağ geçidinden aldığı istek. Bu nedenle, istemci, uygulama ağ geçidi (contoso.com/path2) boyunca değil, isteği doğrudan contoso.azurewebsites.net/path2 'a yapar. Application Gateway 'i atlamak istenmez.

Kullanım durumunda, App Service 'in istemciye yeniden yönlendirme yanıtı gönderebilmesi gereken senaryolar vardır, [konum başlığını yeniden yazmak için ek adımları](https://docs.microsoft.com/azure/application-gateway/troubleshoot-app-service-redirection-app-service-url#sample-configuration)gerçekleştirin.

## <a name="restrict-access"></a>Erişimi kısıtla

Bu örneklerde dağıtılan Web uygulamaları, doğrudan Internet 'ten erişilebilen genel IP adreslerini kullanır. Bu, yeni bir özellik öğrenirken ve yeni şeyler gerçekleştirmeye çalışırken sorun gidermeye yardımcı olur. Ancak bir özelliği üretime dağıtmayı planlıyorsanız, daha fazla kısıtlama eklemek isteyeceksiniz.

Web uygulamalarınıza erişimi kısıtlayabilmeniz için bir yol [STATIK IP kısıtlamaları Azure App Service](../app-service/app-service-ip-restrictions.md)kullanmaktır. Örneğin, Web uygulamasını yalnızca uygulama ağ geçidinden trafik alacak şekilde kısıtlayabilirsiniz. Uygulama ağ geçidi VIP 'sini erişimi olan tek adres olarak listelemek için App Service IP kısıtlaması özelliğini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Uygulama hizmeti ve Application Gateway ile diğer çok kiracılı destek hakkında daha fazla bilgi edinmek için bkz. [Application Gateway ile çok kiracılı hizmet desteği](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-app-overview).
