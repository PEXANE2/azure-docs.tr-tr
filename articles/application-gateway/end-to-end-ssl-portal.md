---
title: Portalı kullanarak uçlardan uca TLS şifrelemesini yapılandırın
titleSuffix: Azure Application Gateway
description: Uçuça TLS şifrelemesi içeren bir uygulama ağ geçidi oluşturmak için Azure portalını nasıl kullanacağınızı öğrenin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 11/14/2019
ms.author: absha
ms.custom: mvc
ms.openlocfilehash: 6f86f32e64bbbe79ea5a403d04f7d6c29ee6b980
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80133003"
---
# <a name="configure-end-to-end-tls-by-using-application-gateway-with-the-portal"></a>Portalla Uygulama Ağ Geçidi'ni kullanarak uçlardan uca TLS'leri yapılandırın

Bu makalede, Azure Uygulama Ağ Geçidi v1 SKU aracılığıyla daha önce Güvenli Soketler Katmanı (SSL) şifrelemesi olarak bilinen uçtan uca Aktarım Katmanı Güvenliği (TLS) şifrelemesini yapılandırmak için Azure portalının nasıl kullanılacağı açıklanmaktadır.

> [!NOTE]
> Uygulama Ağ Geçidi v2 SKU uçlardan uca yapılandırmayı etkinleştirmek için güvenilir kök sertifikaları gerektirir.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

Bir uygulama ağ geçidi ile uçlardan uca TLS yapılandırmak için ağ geçidi için bir sertifika gerekir. Arka uç sunucular için de sertifikalar gereklidir. Ağ geçidi sertifikası, TLS protokol belirtimine uygun olarak simetrik bir anahtar türetmek için kullanılır. Simetrik anahtar daha sonra ağ geçidine gönderilen trafiği şifrelemek ve çözmek için kullanılır. 

Uçuça TLS şifrelemesi için, uygulama ağ geçidinde sağ arka uç sunucularına izin verilmelidir. Bu erişime izin vermek için, kimlik doğrulama sertifikaları (v1) veya Güvenilen Kök Sertifikaları (v2) olarak da bilinen arka uç sunucularının ortak sertifikasını uygulama ağ geçidine yükleyin. Sertifikanın eklenmesi, uygulama ağ geçidinin yalnızca bilinen arka uç örnekleriyle iletişim kurmasını sağlar. Bu yapılandırma uçlardan uca iletişimi daha da güvenli hale getirmekte.

Daha fazla bilgi için, [Uygulama Ağ Geçidi ile TLS sonlandırma ve bitiş uca GENEL Bakış](https://docs.microsoft.com/azure/application-gateway/ssl-overview)bölümüne bakın.

## <a name="create-a-new-application-gateway-with-end-to-end-tls"></a>Uçlardan uca TLS ile yeni bir uygulama ağ geçidi oluşturma

Uçtan uca TLS şifrelemesine sahip yeni bir uygulama ağ geçidi oluşturmak için, yeni bir uygulama ağ geçidi oluştururken önce TLS sonlandırmayı etkinleştirmeniz gerekir. Bu eylem, istemci ve uygulama ağ geçidi arasındaki iletişim için TLS şifreleme sağlar. Ardından, HTTP ayarlarındaki arka uç sunucuların sertifikalarını Güvenli Alıcılar listesine koymanız gerekir. Bu yapılandırma, uygulama ağ geçidi ve arka uç sunucuları arasındaki iletişim için TLS şifreleme sağlar. Bu uçlardan uca TLS şifreleme gerçekleştirir.

### <a name="enable-tls-termination-while-creating-a-new-application-gateway"></a>Yeni bir uygulama ağ geçidi oluştururken TLS sonlandırmayı etkinleştirme

Daha fazla bilgi edinmek için, [yeni bir uygulama ağ geçidi oluştururken TLS sonlandırmayı etkinleştirme'ye](https://docs.microsoft.com/azure/application-gateway/create-ssl-portal)bakın.

### <a name="add-authenticationroot-certificates-of-back-end-servers"></a>Arka uç sunucularının kimlik doğrulama/kök sertifikaları ekleme

1. **Tüm kaynakları**seçin ve ardından **myAppGateway'i**seçin.

2. Sol taraftaki menüden **HTTP ayarlarını** seçin. Azure, uygulama ağ geçidini oluşturduğunuzda varsayılan BIR HTTP ayarı olan **appGatewayBackendHttpSettings'i**otomatik olarak oluşturdu. 

3. **AppGatewayBackendHttpSettings**seçin.

4. **Protokol**altında **HTTPS'yi**seçin. Arka uç **kimlik doğrulama sertifikaları veya Güvenilen kök sertifikaları** için bir bölme görüntülenir.

5. **Yeni oluştur**’u seçin.

6. **Ad** alanına uygun bir ad girin.

7. **Yükleme CER sertifika** kutusundaki sertifika dosyasını seçin.

   Standart ve WAF (v1) uygulama ağ geçitleri için arka uç sunucu sertifikanızın ortak anahtarını .cer biçiminde yüklemeniz gerekir.

   ![Sertifika ekleme](./media/end-to-end-ssl-portal/addcert.png)

   Standard_v2 ve WAF_v2 uygulama ağ geçitleri için arka uç sunucu sertifikasının kök sertifikasını .cer biçiminde yüklemeniz gerekir. Arka uç sertifikası tanınmış bir sertifika yetkilisi (CA) tarafından verilmişse, **Iyi Bilinen CA Sertifikasını Kullan** onay kutusunu seçebilirsiniz ve ardından sertifika yüklemeniz gerekmez.

   ![Güvenilen kök sertifikası ekleme](./media/end-to-end-ssl-portal/trustedrootcert-portal.png)

   ![Kök sertifikası](./media/end-to-end-ssl-portal/trustedrootcert.png)

8. **Kaydet'i**seçin.

## <a name="enable-end-to-end-tls-for-an-existing-application-gateway"></a>Varolan bir uygulama ağ geçidi için uçlardan uca TLS'yi etkinleştirme

Varolan bir uygulama ağ geçidini uçlardan uca TLS şifrelemeyle yapılandırmak için öncelikle dinleyicide TLS sonlandırmayı etkinleştirmeniz gerekir. Bu eylem, istemci ve uygulama ağ geçidi arasındaki iletişim için TLS şifreleme sağlar. Ardından, bu sertifikaları arka uç sunucular için Güvenli Alıcılar listesindeki HTTP ayarlarına koyun. Bu yapılandırma, uygulama ağ geçidi ve arka uç sunucuları arasındaki iletişim için TLS şifreleme sağlar. Bu uçlardan uca TLS şifreleme gerçekleştirir.

TLS sonlandırmayı etkinleştirmek için HTTPS protokolüne sahip bir dinleyici ve sertifika kullanmanız gerekir. Bu koşulları karşılayan varolan bir dinleyici kullanabilir veya yeni bir dinleyici oluşturabilirsiniz. Eski seçeneği seçerseniz, aşağıdaki "Varolan bir uygulama ağ geçidinde TLS sonlandırmayı etkinleştir" bölümünü yoksayabilir ve doğrudan "Arka uç sunucuları için kimlik doğrulama/güvenilen kök sertifikaları ekle" bölümüne geçebilirsiniz.

İkinci seçeneği seçerseniz, aşağıdaki yordamdaki adımları uygulayın.
### <a name="enable-tls-termination-in-an-existing-application-gateway"></a>Varolan bir uygulama ağ geçidinde TLS sonlandırmayı etkinleştirme

1. **Tüm kaynakları**seçin ve ardından **myAppGateway'i**seçin.

2. Sol taraftaki menüden **Dinleyiciler'i** seçin.

3. Gereksinimlerinize bağlı olarak **Temel** veya **Çok siteli** dinleyiciyi seçin.

4. **Protokol**altında **HTTPS'yi**seçin. **Sertifika** için bir bölme görüntülenir.

5. İstemci ve uygulama ağ geçidi arasında TLS sonlandırma için kullanmayı planladığınız PFX sertifikasını yükleyin.

   > [!NOTE]
   > Sınama amacıyla, kendi imzalı bir sertifika kullanabilirsiniz. Ancak, bu işlem iş yükleri için tavsiye edilmez, çünkü bunlar yönetmek daha zordur ve tamamen güvenli değildir. Daha fazla bilgi için kendi [imzalı sertifika oluşturmaya](https://docs.microsoft.com/azure/application-gateway/create-ssl-portal#create-a-self-signed-certificate)bakın.

6. Gereksinimlerinize bağlı olarak **Dinleyici**için gerekli diğer ayarları ekleyin.

7. Kaydetmek için **Tamam**’ı seçin.

### <a name="add-authenticationtrusted-root-certificates-of-back-end-servers"></a>Arka uç sunucularının kimlik doğrulama/güvenilen kök sertifikaları ekleme

1. **Tüm kaynakları**seçin ve ardından **myAppGateway'i**seçin.

2. Sol taraftaki menüden **HTTP ayarlarını** seçin. Sertifikaları Güvenli Alıcılar listesine varolan bir arka uç HTTP ayarına koyabilir veya yeni bir HTTP ayarı oluşturabilirsiniz. (Bir sonraki adımda, varsayılan HTTP ayarı için sertifika, **appGatewayBackendHttpSettings**, Güvenli Alıcılar listesine eklenir.)

3. **AppGatewayBackendHttpSettings**seçin.

4. **Protokol**altında **HTTPS'yi**seçin. Arka uç **kimlik doğrulama sertifikaları veya Güvenilen kök sertifikaları** için bir bölme görüntülenir. 

5. **Yeni oluştur**’u seçin.

6. **Ad** alanına uygun bir ad girin.

7. **Yükleme CER sertifika** kutusundaki sertifika dosyasını seçin.

   Standart ve WAF (v1) uygulama ağ geçitleri için arka uç sunucu sertifikanızın ortak anahtarını .cer biçiminde yüklemeniz gerekir.

   ![Sertifika ekleme](./media/end-to-end-ssl-portal/addcert.png)

   Standard_v2 ve WAF_v2 uygulama ağ geçitleri için arka uç sunucu sertifikasının kök sertifikasını .cer biçiminde yüklemeniz gerekir. Arka uç sertifikası tanınmış bir CA tarafından verilmişse, **Iyi Bilinen CA Sertifikasını Kullan** onay kutusunu seçebilirsiniz ve ardından sertifika yüklemeniz gerekmez.

   ![Güvenilen kök sertifikası ekleme](./media/end-to-end-ssl-portal/trustedrootcert-portal.png)

8. **Kaydet'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure CLI kullanarak bir uygulama ağ geçidi ile web trafiğini yönetme](./tutorial-manage-web-traffic-cli.md)
