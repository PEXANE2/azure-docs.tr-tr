---
title: Azure portalını kullanarak HTTP ile HTTPS yeniden yönlendirmesi ile bir Ön Kapı oluşturma
description: Azure portalını kullanarak HTTP'den HTTPS'ye yönlendirilmiş trafiği içeren bir Ön Kapı oluşturmayı öğrenin.
services: front-door
author: sharad4u
ms.service: frontdoor
ms.topic: article
ms.date: 5/21/2019
ms.author: sharadag
ms.openlocfilehash: b7385ef27cd17705f2c86b6f57d4780511b6935c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80246866"
---
# <a name="create-a-front-door-with-http-to-https-redirection-using-the-azure-portal"></a>Azure portalını kullanarak HTTP ile HTTPS yeniden yönlendirmesi ile bir Ön Kapı oluşturma

Azure portalını kullanarak SSL sonlandırma sertifikasına sahip bir [Ön Kapı](front-door-overview.md) oluşturabilirsiniz. YÖNLENDIRME kuralı, HTTP trafiğini HTTPS'ye yönlendirmek için kullanılır.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Varolan bir Web Uygulaması kaynağıyla Ön Kapı Oluşturma
> * SSL sertifikasına sahip özel bir etki alanı ekleme 
> * Özel etki alanında HTTPS yeniden yönlendirmekurulum

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="create-a-front-door-with-an-existing-web-app-resource"></a>Varolan bir Web Uygulaması kaynağıyla Ön Kapı Oluşturma

1. Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.
2. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
3. Arama çubuğunu kullanarak **Ön Kapı'yı** arayın ve kaynak türünü bulduğunuzda **Oluştur'u**tıklatın.
4. Bir abonelik seçin ve sonra varolan bir kaynak grubunu kullanın veya yeni bir tane oluşturun. UI'de istenen konum yalnızca kaynak grubu içindir. Ön Kapı yapılandırmanız Azure Ön [Kapı'nın TÜM POP konumlarında](front-door-faq.md#what-are-the-pop-locations-for-azure-front-door)dağıtılır.

    ![Yeni Ön Kapı için temel leri yapılandırın](./media/front-door-url-redirect/front-door-create-basics.png)

5. Yapılandırma sekmesini girmek için **İleri'yi** tıklatın. Ön Kapı yapılandırması üç adımda gerçekleşir - varsayılan ön uç ana bilgisayar ekleme, arka uç havuzuna arka uç ekleme ve ardından ön uç ana bilgisayarı için yönlendirme davranışını eşlemek için yönlendirme kuralları oluşturma. 

     ![Ön Kapı konfigürasyon tasarımcısı](./media/front-door-url-redirect/front-door-designer.png)

6. Frontend**+** ana bilgisayarını oluşturmak için _Frontend ana bilgisayarlarındaki_ ' ' simgesini tıklatın, Ön Kapınız`\<**name**\>.azurefd.net`için varsayılan ön yüz ana cadınız için genel olarak benzersiz bir ad girin ( ). Bir sonraki adıma geçmek için **Ekle'yi** tıklatın.

     ![Ön uç ana bilgisayar ekleme](./media/front-door-url-redirect/front-door-create-fehost.png)

7. Arka uç**+** havuzu oluşturmak için _Arka uç havuzlarında_ ' simgesini tıklatın. Arka uç havuzu için bir ad sağlayın ve ardından '**Arka uç ekle'** seçeneğini tıklayın.
8. _App hizmeti_olarak Arka Uç Ana Bilgisayar Türünü seçin. Web uygulamanızın barındırıldığı aboneliği seçin ve ardından **Backend ana bilgisayar adı**için açılan sayfadan belirli web uygulamasını seçin.
9. Arka uç kaydetmek için **Ekle'yi** tıklatın ve arka uç havuzu config'ini kaydetmek için yeniden **Ekle'yi** tıklatın.   ![Arka uç havuzuna arka uç ekleme](./media/front-door-url-redirect/front-door-create-backendpool.png)

10. Rota oluşturmak**+** için _Yönlendirme kurallarındaki_ ' ' simgesini tıklatın. Rota için bir ad sağlayın, 'HttpToHttpsRedirect' deyin ve _ardından Kabul Edilen Protokoller_ alanını **'yalnızca HTTP'** olarak ayarlayın. Uygun ön _uç ana bilgisayar_ seçildiğinden emin olun.  
11. Rota _Ayrıntıları_ bölümünde, _Rota Türünü_ **Yeniden Yönlendirmek**için ayarlayın, Yönlendirme _türünün_ Bulundu **(302)** olarak ayarlanıp yeniden yönlendirme _protokolünün_ **yalnızca HTTPS'ye**ayarlı olduğundan emin olun. 
12. HTTP'nin yönlendirme kuralını HTTPS yönlendirmesine kaydetmek için Ekle'yi tıklatın.
     ![HTTPS yönlendirme rotasına HTTP ekleme](./media/front-door-url-redirect/front-door-redirect-config-example.png)
13. HTTPS trafiğini işlemek için başka bir yönlendirme kuralı ekleyin. Yönlendirme**+** _kurallarında_ ' ' işaretini tıklatın ve rota için bir ad sağlayın, 'DefaultForwardingRoute' deyin ve sonra _Kabul Edilen Protokoller_ alanını **'yalnızca HTTPS'** olarak ayarlayın. Uygun ön _uç ana bilgisayar_ seçildiğinden emin olun.
14. Rota Ayrıntıları bölümünde, _Route Type_ to **Forward'u**ayarlayın, sağ arka uç havuzunun seçildiğinden ve _Iletme Protokolü'nün_ yalnızca **HTTPS**olarak ayarlandığından emin olun. 
15. Yönlendirme isteği için yönlendirme kuralını kaydetmek için Ekle'yi tıklatın.
     ![HTTPS trafiği için ileri rota ekleme](./media/front-door-url-redirect/front-door-forward-route-example.png)
16. Ön Kapı profilinizi oluşturmak için **Gözden Geçir + oluştur'u** ve ardından **Oluştur'u**tıklatın. Oluşturulduktan sonra oluşturulan kaynağa gidin.

## <a name="add-a-custom-domain-to-your-front-door-and-enable-https-on-it"></a>Ön Kapınıza özel bir etki alanı ekleyin ve üzerinde HTTPS'yi etkinleştirin
Aşağıdaki adımlar, varolan bir Ön Kapı kaynağına nasıl özel bir etki alanı ekleyebileceğinizve ardından http'den https yeniden yönlendirmeye nasıl etkinleştirebileceğinizi sergiler. 

### <a name="add-a-custom-domain"></a>Özel etki alanı ekleme

Bu örnekte, `www` alt etki alanı için bir CNAME `www.contosonews.com`kaydı (örneğin, ) ekleyin.

#### <a name="create-the-cname-record"></a>CNAME kaydı oluşturma

Ön Kapınızın varsayılan ön yüz ana bilgisayarının alt etki alanını`<name>.azurefd.net`eşlemek için bir CNAME kaydı ekleyin (, Ön Kapı profilinizin adı buradadır). `<name>`

`www.contoso.com` Etki alanı için, örnek olarak, adı `www` eşleyen bir CNAME kaydı `<name>.azurefd.net`ekleyin.

CNAME kaydını ekledikten sonra, DNS kayıtları sayfası aşağıdaki örnekte gösterildiği gibi görünür:

![Ön Kapıya CNAME özel etki alanı](./media/front-door-url-redirect/front-door-dns-cname.png)

#### <a name="onboard-the-custom-domain-on-your-front-door"></a>Ön Kapınızdaki özel etki alanında yerleşik

1. Ön Kapı tasarımcısı sekmesinde, yeni bir özel etki alanı eklemek için Frontend ana bilgisayarları bölümündeki '+' simgesine tıklayın. 
2. Özel ana bilgisayar adı alanına tam nitelikli özel DNS adını girin, örnek. `www.contosonews.com` 
3. Etki alanından Ön Kapınıza cname eşleme doğrulandıktan sonra, özel etki alanını eklemek için **Ekle'yi** tıklatın.
4. Değişiklikleri göndermek için **Kaydet'i** tıklatın.

![Özel etki alanı menüsü](./media/front-door-url-redirect/front-door-add-custom-domain.png)

### <a name="enable-https-on-your-custom-domain"></a>Özel etki alanınızda HTTPS'yi etkinleştirme

1. Eklenen özel etki alanına tıklayın ve **Özel etki alanı HTTPS**bölümünün altında, durumu **Etkin**olarak değiştirin.
2. Sertifika yönetim **türünü,** Ön Kapı tarafından tutulan, yönetilen ve otomatik olarak döndürülen ücretsiz sertifika için _yönetilen Ön Kapı'ya_ bırakabilirsiniz. Azure Key Vault ile depolanan kendi özel SSL sertifikanızı da kullanmayı seçebilirsiniz. Bu öğretici, Ön Kapı yönetilen sertifika kullanımı varsayar.
![Özel etki alanı için HTTPS'yi etkinleştirme](./media/front-door-url-redirect/front-door-custom-domain-https.png)

3. Seçimi kaydetmek için **Güncelleştir'e** tıklayın ve ardından **Kaydet'i**tıklatın.
4. Birkaç dakika sonra **Yenile'yi** tıklatın ve ardından sertifika sağlamanın ilerlemesini görmek için özel etki alanını yeniden tıklatın. 

> [!WARNING]
> ÖZEL bir etki alanı için HTTPS'yi etkinleştirmek birkaç dakika sürebilir ve CNAME doğrudan Ön Kapı ana `<name>.azurefd.net`bilgisayarınızla eşlenmezse etki alanı sahipliği doğrulamasına da bağlıdır. [Özel bir etki alanı için HTTPS'yi etkinleştirme](./front-door-custom-domain-https.md)hakkında daha fazla bilgi edinin.

## <a name="configure-the-routing-rules-for-the-custom-domain"></a>Özel etki alanı için yönlendirme kurallarını yapılandırma

1. Daha önce oluşturulan yönlendirme kuralını tıklatın.
2. Frontend ana bilgisayarları için açılır yere tıklayın ve etki alanınız için de bu rotayı uygulamak için özel etki alanınızı seçin.
3. **Güncelleştir**’e tıklayın.
4. Diğer yönlendirme kuralı için de aynı işlemi yapın, diğer, iletme rotanız için özel etki alanı ekleyin.
5. Değişikliklerinizi göndermek için **Kaydet'i** tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.
- [Ön Kapı'da URL yönlendirmesi](front-door-url-redirect.md)hakkında daha fazla bilgi edinin.
