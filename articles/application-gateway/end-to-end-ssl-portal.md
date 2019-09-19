---
title: Hızlı başlangıç-Azure Application Gateway ile uçtan uca SSL şifrelemesini Yapılandırma-Azure portal | Microsoft Docs
description: Uçtan uca SSL şifrelemesi ile Azure Application Gateway oluşturmak için Azure portal nasıl kullanacağınızı öğrenin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 4/30/2019
ms.author: absha
ms.custom: mvc
ms.openlocfilehash: a37b313bd808ee0441d84ac92050b087eba7ac9d
ms.sourcegitcommit: cd70273f0845cd39b435bd5978ca0df4ac4d7b2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71097157"
---
# <a name="configure-end-to-end-ssl-by-using-application-gateway-with-the-portal"></a>Portal ile Application Gateway kullanarak uçtan uca SSL 'yi yapılandırma

Bu makalede, bir Application Gateway v1 SKU 'SU ile uçtan uca SSL şifrelemesini yapılandırmak için Azure portal nasıl kullanılacağı gösterilmektedir.  

> [!NOTE]
> Application Gateway v2 SKU 'SU, uçtan uca yapılandırmayı etkinleştirmek için güvenilen kök sertifikaları gerektirir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

Uçtan uca SSL 'yi bir Application Gateway yapılandırmak için, ağ geçidi için bir sertifika gerekir ve arka uç sunucuları için sertifikalar gerekir. Ağ Geçidi sertifikası, her SSL protokol belirtimine göre bir simetrik anahtar türetmek için kullanılır. Daha sonra simetrik anahtar, ağ geçidine gönderilen trafiği şifrelemek ve şifresini çözmek için kullanılır. Uçtan uca SSL şifrelemesi için, uygulama ağ geçidinde doğru arka uç sunucularına izin verilmelidir. Bunu yapmak için, kimlik doğrulama sertifikaları (v1) veya güvenilir kök sertifikalar (v2) olarak da bilinen arka uç sunucularının genel sertifikasını Application Gateway yükleyin. Sertifikayı eklemek Application Gateway yalnızca bilinen arka uç örnekleriyle iletişim kurar. Bu, uçtan uca iletişimin güvenliğini sağlar.

Daha fazla bilgi edinmek için bkz. [SSL sonlandırması ve uçtan uca SSL](https://docs.microsoft.com/azure/application-gateway/ssl-overview).

## <a name="create-a-new-application-gateway-with-end-to-end-ssl"></a>Uçtan uca SSL ile yeni bir uygulama ağ geçidi oluşturma

Uçtan uca SSL şifrelemesi ile yeni bir uygulama ağ geçidi oluşturmak için, ilk olarak yeni bir uygulama ağ geçidi oluştururken SSL sonlandırmasını etkinleştirmeniz gerekir. Bu, istemci ve uygulama ağ geçidi arasındaki iletişim için SSL şifrelemesini etkinleştirir. Daha sonra, uygulama ağ geçidi ve arka uç sunucuları arasındaki iletişim için SSL şifrelemesini etkinleştirmek üzere HTTP ayarlarındaki arka uç sunucuları için sertifikaları beyaz listeye almanız gerekir. Bu, uçtan uca SSL şifrelemesini gerçekleştirmeye yöneliktir.

### <a name="enable-ssl-termination-while-creating-a-new-application-gateway"></a>Yeni bir uygulama ağ geçidi oluştururken SSL sonlandırmasını etkinleştir

[Yeni bir uygulama ağ geçidi oluştururken SSL sonlandırmasını nasıl etkinleştireceğinizi](https://docs.microsoft.com/azure/application-gateway/create-ssl-portal)öğrenmek için bu makaleye başvurun.

### <a name="add-authenticationroot-certificate-of-back-end-servers"></a>Arka uç sunucularının kimlik doğrulaması/kök sertifikası Ekle

1. **Tüm kaynaklar**' ı ve ardından **myappgateway**' i seçin.

2. Sol menüden **http ayarları** ' nı seçin. Azure, uygulama ağ geçidini oluştururken **Appgatewaybackendhttpsettings**varsayılan HTTP ayarını otomatik olarak oluşturdu. 

3. **Appgatewaybackendhttpsettings**öğesini seçin.

4. **Protokol**altında **https**' yi seçin. **Arka uç kimlik doğrulama sertifikaları veya güvenilen kök sertifikaları** için bir bölme görüntülenir. 

5. **Yeni oluştur**' a tıklayın.

6. Uygun bir **ad**girin.

7. **Cer sertifikasını karşıya yükle** kutusunu kullanarak sertifika dosyasını seçin.

   Standart ve WAF (v1) uygulama ağ geçitleri için, arka uç sunucu sertifikanızın ortak anahtarını. cer biçiminde yüklemeniz gerekir.

   ![addcert](./media/end-to-end-ssl-portal/addcert.png)

   Standard_v2 ve WAF_v2 uygulama ağ geçitleri için, arka uç sunucu sertifikasının **kök sertifikasını** . cer biçiminde yüklemeniz gerekir. Arka uç sertifikası iyi bilinen bir CA tarafından verildiyse, "Iyi bilinen CA sertifikasını kullan" kutusunu denetleyebilir ve sertifikayı karşıya yüklemeye gerek yoktur.

   ![addtrustedrootcert](./media/end-to-end-ssl-portal/trustedrootcert-portal.png)

   ![rootcert](./media/end-to-end-ssl-portal/trustedrootcert.png)

8. **Kaydet**’i seçin.

## <a name="enable-end-to-end-ssl-for-existing-application-gateway"></a>Mevcut uygulama ağ geçidi için uçtan uca SSL 'yi etkinleştirme

Uçtan uca SSL şifrelemesi ile mevcut bir uygulama ağ geçidini yapılandırmak için öncelikle dinleyicide SSL sonlandırmasını etkinleştirmeniz gerekir. Bu, istemci ve uygulama ağ geçidi arasındaki iletişim için SSL şifrelemesini etkinleştirir. Daha sonra, uygulama ağ geçidi ve arka uç sunucuları arasındaki iletişim için SSL şifrelemesini etkinleştirmek üzere HTTP ayarlarındaki arka uç sunucuları için sertifikaları beyaz listeye almanız gerekir. Bu, uçtan uca SSL şifrelemesini gerçekleştirmeye yöneliktir.

SSL sonlandırmasını etkinleştirmek için HTTPS protokolü ve sertifikasıyla bir dinleyici kullanmanız gerekir. Bu nedenle, HTTPS protokolü ve sertifikasıyla mevcut bir dinleyiciyi kullanmayı seçebilir ya da yeni bir dinleyici oluşturabilirsiniz. Önceki seçeneği tercih ederseniz, **mevcut uygulama ağ GEÇIDINDE SSL sonlandırmasını etkinleştirmek** için aşağıdaki belirtilen adımları yoksayabilirsiniz ve **arka uç sunucuları için kimlik doğrulaması/güvenilen kök sertifikaları Ekle** bölümüne doğrudan geçiş yapabilirsiniz. İkincisini seçerseniz, bu adımları kullanın.

### <a name="enable-ssl-termination-in-existing-application-gateway"></a>Mevcut uygulama ağ geçidinde SSL sonlandırmasını etkinleştir

1. **Tüm kaynaklar**' ı ve ardından **myappgateway**' i seçin.

2. Sol menüden **dinleyiciler** ' i seçin.

3. Gereksiniminize göre **temel** ve **çok** siteli dinleyici arasında seçim yapın.

4. **Protokol**altında **https**' yi seçin. **Sertifika** bölmesi görüntülenir.

5. İstemci ve uygulama ağ geçidi arasında SSL sonlandırma için kullanmayı düşündüğünüz PFX sertifikasını karşıya yükleyin.

   > [!NOTE]
   > Sınama amacıyla, kendinden imzalı bir sertifika kullanabilirsiniz. Ancak, yönetimi daha zor olan ve tamamen güvenli olmayan üretim iş yükleri için önerilmez. [Kendinden imzalı bir sertifika oluşturmayı](https://docs.microsoft.com/azure/application-gateway/create-ssl-portal#create-a-self-signed-certificate)öğrenin.

6. İhtiyacınıza göre **dinleyici** için diğer gerekli ayarları ekleyin.

7. Kaydetmek için **Tamam**’ı seçin.

### <a name="add-authenticationtrusted-root-certificates-of-back-end-servers"></a>Arka uç sunucularının kimlik doğrulaması/güvenilen kök sertifikalarını ekleyin

1. **Tüm kaynaklar**' ı ve ardından **myappgateway**' i seçin.

2. Sol menüden **http ayarları** ' nı seçin. Sertifikaları varolan bir arka uç HTTP ayarında beyaz listeye girebilir veya yeni bir HTTP ayarı oluşturabilirsiniz. Aşağıdaki adımda, varsayılan HTTP ayarı olan **Appgatewaybackendhttpsettings**için sertifikayı beyaz listeye göndereceğiz.

3. **Appgatewaybackendhttpsettings**öğesini seçin.

4. **Protokol**altında **https**' yi seçin. **Arka uç kimlik doğrulama sertifikaları veya güvenilen kök sertifikaları** için bir bölme görüntülenir. 

5. **Yeni oluştur**' a tıklayın.

6. Uygun **adı**girin.

7. **Cer sertifikasını karşıya yükle** kutusunu kullanarak sertifika dosyasını seçin.

   Standart ve WAF (v1) uygulama ağ geçitleri için, arka uç sunucu sertifikanızın ortak anahtarını. cer biçiminde yüklemeniz gerekir.

   ![addcert](./media/end-to-end-ssl-portal/addcert.png)

   Standard_v2 ve WAF_v2 uygulama ağ geçitleri için, arka uç sunucu sertifikasının **kök sertifikasını** . cer biçiminde yüklemeniz gerekir. Arka uç sertifikası iyi bilinen bir CA tarafından verildiyse, "Iyi bilinen CA sertifikasını kullan" kutusunu denetleyebilir ve sertifikayı karşıya yüklemeye gerek yoktur.

   ![addtrustedrootcert](./media/end-to-end-ssl-portal/trustedrootcert-portal.png)

8. **Kaydet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure CLI kullanarak bir uygulama ağ geçidi ile web trafiğini yönetme](./tutorial-manage-web-traffic-cli.md)
