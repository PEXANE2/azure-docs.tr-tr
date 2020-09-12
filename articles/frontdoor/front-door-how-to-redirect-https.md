---
title: Azure portal kullanarak HTTP ile HTTPS yönlendirmesi arasında ön kapı oluşturma
description: Azure portal kullanarak HTTP 'den HTTPS 'ye yeniden yönlendirilen trafikle ön kapı oluşturmayı öğrenin.
services: front-door
author: duongau
ms.service: frontdoor
ms.topic: how-to
ms.date: 5/21/2019
ms.author: duau
ms.openlocfilehash: fe2159f0eeb9d01081e6a25e7a88ceff4f1e361c
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89399699"
---
# <a name="create-a-front-door-with-http-to-https-redirection-using-the-azure-portal"></a>Azure portal kullanarak HTTP ile HTTPS yönlendirmesi arasında ön kapı oluşturma

TLS sonlandırma sertifikasıyla bir [ön kapı](front-door-overview.md) oluşturmak için Azure Portal kullanabilirsiniz. HTTP trafiğini HTTPS 'ye yönlendirmek için bir yönlendirme kuralı kullanılır.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Mevcut bir Web uygulaması kaynağıyla ön kapı oluşturma
> * TLS/SSL sertifikası ile özel etki alanı ekleme 
> * Özel etki alanında HTTPS yeniden yönlendirme kurulumu

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-a-front-door-with-an-existing-web-app-resource"></a>Mevcut bir Web uygulaması kaynağıyla ön kapı oluşturma

1. [https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.
2. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
3. Arama çubuğunu kullanarak **ön kapı** araması yapın ve kaynak türünü bulduktan sonra **Oluştur**' a tıklayın.
4. Bir abonelik seçin ve ardından var olan bir kaynak grubunu kullanın veya yeni bir tane oluşturun. Kullanıcı arabiriminde sorulduğu konum yalnızca kaynak grubu içindir. Ön kapı yapılandırmanız, tüm [Azure ön KAPıSıNıN pop konumlarına](front-door-faq.md#what-are-the-pop-locations-for-azure-front-door)dağıtılır.

    ![Yeni ön kapıya yönelik temelleri yapılandırın](./media/front-door-url-redirect/front-door-create-basics.png)

5. Yapılandırma sekmesini girmek için **İleri** ' ye tıklayın. Ön kapıya yönelik yapılandırma üç adımda gerçekleşir. varsayılan ön uç konak ekleyerek, arka uç havuzunda arka uçlar ekleyerek ve ardından ön uç ana bilgisayar için yönlendirme davranışını eşlemek üzere yönlendirme kuralları oluşturma. 

     ![Ön kapı yapılandırma Tasarımcısı](./media/front-door-url-redirect/front-door-designer.png)

6. Ön uç **+** konak oluşturmak için ön _uç konaklarındaki_ ' ' simgesine tıklayın, ön kapılarınız için varsayılan ön uç ana bilgisayarınız için genel olarak benzersiz bir ad girin ( `\<**name**\>.azurefd.net` ). Sonraki adıma geçmek için **Ekle** ' ye tıklayın.

     ![Ön uç konağı ekleme](./media/front-door-url-redirect/front-door-create-fehost.png)

7. Arka uç **+** havuzu oluşturmak Için _arka uç havuzlarındaki_ ' ' simgesine tıklayın. Arka uç havuzu için bir ad girin ve ardından '**arka uç Ekle**' seçeneğine tıklayın.
8. Arka uç konak türünü _App Service_olarak seçin. Web uygulamanızın barındırıldığı aboneliği seçin ve ardından **arka uç ana bilgisayar adı**açılır listesinden belirli bir Web uygulamasını seçin.
9. Arka ucu kaydetmek için **Ekle** ' ye tıklayın ve arka uç havuzu yapılandırmasını kaydetmek Için yeniden **Ekle** ' ye tıklayın.   ![Arka uç havuzunda arka uç ekleme](./media/front-door-url-redirect/front-door-create-backendpool.png)

10. **+** _Yönlendirme kuralları_ ' ' simgesine tıklayarak bir yol oluşturun. Yol için bir ad girin, ' HttpToHttpsRedirect ' deyin ve ardından _kabul edilen protokoller_ alanını **' http Only '** olarak ayarlayın. Uygun _ön uç konağın_ seçili olduğundan emin olun.  
11. _Rota ayrıntıları_ bölümünde _rota türünü_ **yeniden yönlendir**olarak ayarlayın, _yeniden yönlendirme türünün_ **bulunan (302)** olarak ayarlandığından ve _yönlendirme protokolünün_ **yalnızca https**olarak ayarlandığından emin olun. 
12. HTTP için yönlendirme kuralını HTTPS yeniden yönlendirmeye kaydetmek için Ekle ' ye tıklayın.
     ![Bir HTTP-HTTPS yeniden yönlendirme yolu ekleyin](./media/front-door-url-redirect/front-door-redirect-config-example.png)
13. HTTPS trafiğini işlemek için başka bir yönlendirme kuralı ekleyin. **+** _Yönlendirme kurallarında_ ' ' işaretine tıklayın ve yol için bir ad girin, ' DefaultForwardingRoute ' deyin ve ardından _kabul edilen protokoller_ alanını **' https Only '** olarak ayarlayın. Uygun _ön uç konağın_ seçili olduğundan emin olun.
14. Rota ayrıntıları bölümünde, _rota türünü_ **ilet**olarak ayarlayın, doğru arka uç havuzunun seçili olduğundan ve _iletme protokolünün_ **yalnızca https**olarak ayarlandığından emin olun. 
15. İstek iletimi için yönlendirme kuralını kaydetmek üzere Ekle ' ye tıklayın.
     ![HTTPS trafiği için bir iletme yolu ekleyin](./media/front-door-url-redirect/front-door-forward-route-example.png)
16. Ön kapı profilinizi oluşturmak için **gözden geçir + oluştur** ' a ve ardından **Oluştur**' a tıklayın. Oluşturulduktan sonra kaynağa gidin.

## <a name="add-a-custom-domain-to-your-front-door-and-enable-https-on-it"></a>Ön kapıya özel bir etki alanı ekleyin ve üzerinde HTTPS 'yi etkinleştirin
Aşağıdaki adımlarda, var olan bir ön kapıya kaynak üzerinde nasıl özel bir etki alanı ekleyebileceğiniz ve sonra HTTP ile HTTPS yönlendirmesi nasıl etkinleştirileceği gösterilmektedir. 

### <a name="add-a-custom-domain"></a>Özel etki alanı ekleme

Bu örnekte, alt etki alanı için bir CNAME kaydı eklersiniz `www` (örneğin, `www.contosonews.com` ).

#### <a name="create-the-cname-record"></a>CNAME kaydı oluşturma

Bir alt etki alanını ön kapısının varsayılan ön uç konağına eşlemek için bir CNAME kaydı ekleyin ( `<name>.azurefd.net` , burada, `<name>` ön kapı profilinizin adıdır).

`www.contoso.com`Etki alanı için, örneğin adını eşleyen BIR CNAME kaydı ekleyin `www` `<name>.azurefd.net` .

CNAME kaydını ekledikten sonra, DNS kayıtları sayfası aşağıdaki örnekte gösterildiği gibi görünür:

![CNAME özel etki alanından ön kapıya](./media/front-door-url-redirect/front-door-dns-cname.png)

#### <a name="onboard-the-custom-domain-on-your-front-door"></a>Özel etki alanını ön kapıya ekleme

1. Ön kapı Tasarımcısı sekmesinde, ön uç konaklar bölümünde ' + ' simgesine tıklayarak yeni bir özel etki alanı ekleyin. 
2. Özel ana bilgisayar adı alanına, örnek olarak tam özel DNS adını girin `www.contosonews.com` . 
3. Etki alanından ön kapıya yönelik CNAME eşlemesi doğrulandıktan sonra, özel etki alanını eklemek için **Ekle** ' ye tıklayın.
4. Değişiklikleri göndermek için **Kaydet** ' e tıklayın.

![Özel etki alanı menüsü](./media/front-door-url-redirect/front-door-add-custom-domain.png)

### <a name="enable-https-on-your-custom-domain"></a>Özel etki alanınız üzerinde HTTPS 'yi etkinleştirme

1. Eklenen özel etki alanına tıklayın ve **özel etki alanı https**bölümünde, durumu **etkin**olarak değiştirin.
2. **Sertifika yönetimi türünü** , ön kapıya göre tutulan, yönetilen ve oto tarafından döndürülen ücretsiz sertifika Için _yönetilen ön kapı_ olarak ayarlayabilirsiniz. Ayrıca, Azure Key Vault ile depolanan özel TLS/SSL sertifikanızı kullanmayı da seçebilirsiniz. Bu öğreticide, ön kapılı yönetilen sertifikanın kullanılması varsayılmaktadır.
![Özel etki alanı için HTTPS 'yi etkinleştirme](./media/front-door-url-redirect/front-door-custom-domain-https.png)

3. Seçimi kaydetmek için **Güncelleştir** ' e tıklayın ve ardından **Kaydet**' e tıklayın.
4. Birkaç dakika sonra **Yenile** ' ye tıklayın ve ardından sertifika sağlama ilerleme durumunu görmek için özel etki alanı ' na tıklayın. 

> [!WARNING]
> Özel bir etki alanı için HTTPS 'nin etkinleştirilmesi birkaç dakika sürebilir ve ayrıca, CNAME doğrudan ön kapı konağınız ile eşlenmişse etki alanı sahipliği doğrulamasına bağlıdır `<name>.azurefd.net` . [Özel bir etki alanı IÇIN https 'yi etkinleştirme](./front-door-custom-domain-https.md)hakkında daha fazla bilgi edinin.

## <a name="configure-the-routing-rules-for-the-custom-domain"></a>Özel etki alanı için yönlendirme kurallarını yapılandırma

1. Daha önce oluşturulan yeniden yönlendirme yönlendirme kuralına tıklayın.
2. Ön uç Konakları açılan listesine tıklayın ve bu yolu etki alanınız için de uygulamak üzere özel etki alanınızı seçin.
3. **Güncelleştir**’e tıklayın.
4. Diğer yönlendirme kuralı için de aynı işlemi, diğer bir deyişle, iletme yönlendirmenize özel etki alanını eklemek için de yapın.
5. Değişikliklerinizi göndermek için **Kaydet** ' e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.
- [Ön kapıda URL yeniden yönlendirme](front-door-url-redirect.md)hakkında daha fazla bilgi edinin.
