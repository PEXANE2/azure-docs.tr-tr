---
title: Portalı kullanarak trafiği çok kiracılı uygulamalara yönetme
titleSuffix: Azure Application Gateway
description: Bu makalede, Mevcut veya yeni bir uygulama ağ geçidinde arka uç havuzunda Azure App hizmeti web uygulamalarını üye olarak yapılandırma konusunda kılavuz lar sağlanmaktadır.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 11/14/2019
ms.author: absha
ms.openlocfilehash: 0ec417b3c7a025d2d05bdd74ec683a2891c3b0de
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74075171"
---
# <a name="configure-app-service-with-application-gateway"></a>Uygulama Ağ Geçidi ile Uygulama Hizmetini Yapılandırma

Uygulama hizmeti, adanmış dağıtım yerine çok kiracılı bir hizmet olduğundan, isteği doğru uygulama hizmeti bitiş noktasına çözmek için gelen istekte ana bilgisayar üstbilgisini kullanır. Genellikle, uygulamanın DNS adı, hangi sırayla uygulama hizmeti önünde uygulama ağ geçidi ile ilişkili DNS adı, arka uç uygulama hizmetinin etki alanı adı farklıdır. Bu nedenle, uygulama ağ geçidi tarafından alınan özgün istekteki ana bilgisayar üstbilgi, arka uç hizmetinin ana bilgisayarının adı ile aynı değildir. Bu nedenle, uygulama ağ geçidinden arka uça gelen istekteki ana bilgisayar üstbilgi arka uç hizmetinin ana bilgisayarı olarak değiştirilmediği sürece, çok kiracılı arka uçlar isteği doğru bitiş noktasına çözümlemez.

Uygulama Ağ Geçidi, `Pick host name from backend address` istek Uygulama Ağ Geçidi'nden arka uca yönlendirildiğinde, istekteki ana bilgisayar üstbilgisini arka uç ana bilgisayarının ana bilgisayarıyla geçersiz kılan bir anahtar sağlar. Bu özellik, Azure uygulama hizmeti ve API yönetimi gibi çok kiracılı arka uçlar için destek sağlar. 

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
>
> - Arka uç havuzu oluşturun ve buna bir Uygulama Hizmeti ekleyin
> - "Hostname Seç" anahtarları etkinleştirilmiş HTTP Ayarları ve Özel Sonda oluşturma

## <a name="prerequisites"></a>Ön koşullar

- Uygulama ağ geçidi: Varolan bir uygulama ağ geçidiniz yoksa, [uygulama ağ geçidinin](https://docs.microsoft.com/azure/application-gateway/quick-create-portal) nasıl oluşturulabildiğini görün
- Uygulama hizmeti: Mevcut bir Uygulama hizmetiniz yoksa, [Uygulama hizmeti belgelerine](https://docs.microsoft.com/azure/app-service/)bakın.

## <a name="add-app-service-as-backend-pool"></a>Arka uç havuzu olarak Uygulama hizmeti ekleme

1. Azure portalında, uygulama ağ geçidinizin yapılandırma görünümünü açın.

2. **Arka uç havuzlarının**altında, yeni bir arka uç havuzu oluşturmak için **Ekle'ye** tıklayın.

3. Arka uç havuzuna uygun bir ad sağlayın. 

4. **Hedefler**altında, açılır yere tıklayın ve seçenek olarak **Uygulama Hizmetleri'ni** seçin.

5. **Hedef** açılır listesinin hemen altında, Uygulama Hizmetlerinizin bir listesini içeren bir açılır düşüş görüntülenir. Bu açılır dosyadan, arka uç havuzu üyesi olarak eklemek istediğiniz Uygulama Hizmeti'ni seçin ve Ekle'yi tıklatın.

   ![Uygulama hizmeti arka uç](./media/configure-web-app-portal/backendpool.png)
   
   > [!NOTE]
   > Açılır kapı yalnızca Uygulama Ağ Geçidinizle aynı abonelikte olan uygulama hizmetlerini dolduracaktır. Uygulama Ağ Geçidi'nin bulunduğu abonelikten farklı bir abonelikte olan bir uygulama hizmetini kullanmak istiyorsanız, **Hedefler** açılır düşüşünde **Uygulama Hizmetlerini** seçmek yerine IP adresi veya ana **bilgisayar adı** seçeneğini seçin ve ev sahibi adını girin (örnek). azurewebsites.net) uygulama hizmeti.

## <a name="create-http-settings-for-app-service"></a>Uygulama hizmeti için HTTP ayarları oluşturma

1. **HTTP Ayarları'nın**altında, yeni bir HTTP Ayarı oluşturmak için **Ekle'yi** tıklatın.

2. HTTP Ayarı için bir ad girdiniz ve gereksiniminize göre Çerez Tabanlı Affinity'yi etkinleştirebilir veya devre dışı bırakabilirsiniz.

3. Kullanım durumunuza göre protokolü HTTP veya HTTPS olarak seçin. 

   > [!NOTE]
   > HTTPS'yi seçerseniz, uygulama hizmeti güvenilir bir Azure hizmeti olduğundan, uygulama hizmetinin arka ucunu beyaz listeye almak için herhangi bir kimlik doğrulama sertifikası veya güvenilen kök sertifikası yüklemeniz gerekmez.

4. Uygulama Hizmeti **için Kullanım** kutusunu işaretleyin. Anahtarların `Create a probe with pick host name from backend address` otomatik `Pick host name from backend address` olarak etkinleştirilen olacağını unutmayın.`Pick host name from backend address` istek Uygulama Ağ Geçidi'nden arka uca yönlendirildiğinde, arka uç ana bilgisayar adı ile istek ana bilgisayar üstbilgigeçersiz kılar.  

   `Create a probe with pick host name from backend address`otomatik olarak bir sağlık sondası oluşturur ve bu HTTP Ayarı ile ilişkilendirin. Bu HTTP ayarı için başka bir sağlık sondası oluşturmanız gerekmez. Sağlık sondaları listesine adında <HTTP Setting name> <Unique GUID> yeni bir sonda eklendiğini ve anahtarın `Pick host name from backend http settings enabled`zaten bu olduğunu kontrol edebilirsiniz.

   Zaten App hizmeti için kullanılan bir veya daha fazla HTTP Ayarları varsa ve bu HTTP ayarları oluşturduğunuz bir kullanmakta kullandığınız `Create a probe with pick host name from backend address` protokolle aynı protokolü kullanıyorsa, anahtar yerine, özel sondalardan birini seçmek için açılır bir açılır alırsınız. Bunun nedeni, uygulama hizmeti ile birlikte bir HTTP Ayarı olduğundan, bu nedenle, `Pick host name from backend http settings enabled` anahtarı olan bir sağlık sondası da olacaktır. Açılır açılır daki özel sondayı seçin.

5. HTTP ayarını oluşturmak için **Tamam'ı** tıklatın.

   ![HTTP-ayar1](./media/configure-web-app-portal/http-setting1.png)

   ![HTTP-ayar2](./media/configure-web-app-portal/http-setting2.png)



## <a name="create-rule-to-tie-the-listener-backend-pool-and-http-setting"></a>Dinleyici, Arka Uç Havuzu ve HTTP Ayarını bağlamak için Kural Oluşturma

1. **Kurallar**altında, yeni bir Temel kuralı oluşturmak için **Temel'i** tıklatın.

2. Uygun bir ad sağlayın ve Uygulama hizmeti için gelen istekleri kabul edecek dinleyiciyi seçin.

3. **Backend havuz** açılır, yukarıda oluşturduğunuz arka uç havuzunu seçin.

4. HTTP **ayar** açılır açılır durumunda, yukarıda oluşturduğunuz HTTP ayarını seçin.

5. Bu kuralı kaydetmek için **Tamam'ı** tıklatın.

   ![Kural](./media/configure-web-app-portal/rule.png)

## <a name="additional-configuration-in-case-of-redirection-to-app-services-relative-path"></a>Uygulama hizmetinin göreli yoluna yeniden yönlendirme durumunda ek yapılandırma

Uygulama hizmeti, istemciye göreli yoluna yeniden yönlendirmesi için bir yeniden yönlendirme yanıtı gönderdiğinde (Örneğin, contoso.azurewebsites.net/path1'dan contoso.azurewebsites.net/path2'a yönlendirme), yanıtın konum üstbilgisinde uygulama ağ geçidinden aldığı istekle aynı ana bilgisayar adını kullanır. Bu nedenle istemci, başvuru ağ geçidinden (contoso.com/path2) geçmek yerine isteği doğrudan contoso.azurewebsites.net/path2 yapacaktır. Uygulama ağ geçidini atlayarak istenmez.

Kullanım durumunuzda, Uygulama hizmetinin istemciye yeniden yönlendirme yanıtı göndermesi, [konum üstbilgisini yeniden yazmak için ek adımları](https://docs.microsoft.com/azure/application-gateway/troubleshoot-app-service-redirection-app-service-url#sample-configuration)gerçekleştirmesi gereken senaryolar varsa.

## <a name="restrict-access"></a>Erişimi kısıtlama

Bu örneklerde dağıtılan web uygulamaları, doğrudan Internet'ten erişilebilen genel IP adreslerini kullanır. Bu, yeni bir özellik öğrenirken ve yeni şeyler denerken sorun gidermede yardımcı olur. Ancak bir özelliği üretime dağıtmak istiyorsanız, daha fazla kısıtlama eklemek isteyebilirsiniz.

Web uygulamalarınıza erişimi kısıtlamanın bir yolu, [Azure Uygulama Hizmeti statik IP kısıtlamalarını](../app-service/app-service-ip-restrictions.md)kullanmaktır. Örneğin, web uygulamasını yalnızca uygulama ağ geçidinden trafik alması için kısıtlayabilirsiniz. Uygulama hizmetip kısıtlama özelliğini kullanarak uygulama ağ geçidi VIP'yi erişimi olan tek adres olarak listele.

## <a name="next-steps"></a>Sonraki adımlar

Uygulama ağ geçidi ile Uygulama hizmeti ve diğer çok kiracılı destek hakkında daha fazla bilgi edinmek için uygulama [ağ geçidi ile çok kiracılı hizmet desteğine](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-app-overview)bakın.
