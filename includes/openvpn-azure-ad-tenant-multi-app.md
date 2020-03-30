---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/18/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: e950d194ab48cec1a70c7bd17617332cb858a55d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77485675"
---
## <a name="1-create-the-azure-ad-tenant"></a><a name="tenant"></a>1. Azure AD kiracısını oluşturma

Yeni bir kiracı oluştur makalesindeki adımları kullanarak bir Azure AD [kiracısı oluşturun:](../articles/active-directory/fundamentals/active-directory-access-create-new-tenant.md)

* Kuruluş adı
* İlk etki alanı adı

  Örnek:

   ![Yeni Azure AD kiracı](./media/openvpn-azure-ad-tenant-multi-app/new-tenant.png)

## <a name="2-create-tenant-users"></a><a name="users"></a>2. Kiracı kullanıcıları oluşturma

Bu adımda, iki Azure AD kiracısı kullanıcı oluşturursunuz: Bir Global Yönetici hesabı ve bir ana kullanıcı hesabı. Ana kullanıcı hesabı, ana gömme hesabınız (hizmet hesabı) olarak kullanılır. Bir Azure AD kiracı kullanıcı hesabı oluşturduğunuzda, oluşturmak istediğiniz kullanıcı türüne göre Dizin rolünü ayarlarsınız. Azure AD kiracınız için en az iki kullanıcı oluşturmak için [bu makaledeki](../articles/active-directory/fundamentals/add-users-azure-active-directory.md) adımları kullanın. Hesap türlerini oluşturmak için **Dizin Rolünü** değiştirdiğinden emin olun:

* Genel Yönetici
* Kullanıcı

## <a name="3-register-the-vpn-client"></a><a name="register-client"></a>3. VPN İstemciyi Kaydedin

VPN istemcisini Azure AD kiracıya kaydedin.

1. Kimlik doğrulaması için kullanmak istediğiniz dizinin Dizin Kimliğini bulun. Active Directory sayfasının özellikleri bölümünde listelenir.

    ![Dizin Kimliği](./media/openvpn-azure-ad-tenant-multi-app/directory-id.png)

2. Dizin kimliği'ni kopyalayın.

3. **Azure portalında, Global yönetici** rolüne atanmış bir kullanıcı olarak oturum açın.

4. Sonra, yönetici onayı verin. Tarayıcınızın adres çubuğunda dağıtım konumunuza ait URL'yi kopyalayın ve yapıştırın:

    Genel

    ```
    https://login.microsoftonline.com/common/oauth2/authorize?client_id=41b23e61-6c1e-4545-b367-cd054e0ed4b4&response_type=code&redirect_uri=https://portal.azure.com&nonce=1234&prompt=admin_consent
    ````

    Azure Kamu

    ```
    https://login-us.microsoftonline.com/common/oauth2/authorize?client_id=51bb15d4-3a4f-4ebf-9dca-40096fe32426&response_type=code&redirect_uri=https://portal.azure.us&nonce=1234&prompt=admin_consent
    ````

    Microsoft Cloud Almanya

    ```
    https://login-us.microsoftonline.de/common/oauth2/authorize?client_id=538ee9e6-310a-468d-afef-ea97365856a9&response_type=code&redirect_uri=https://portal.microsoftazure.de&nonce=1234&prompt=admin_consent
    ````

    Azure Çin 21Vianet

    ```
    https://https://login.chinacloudapi.cn/common/oauth2/authorize?client_id=49f817b6-84ae-4cc0-928c-73f27289b3aa&response_type=code&redirect_uri=https://portal.azure.cn&nonce=1234&prompt=admin_consent
    ```

5. İstenirse **Global Admin** hesabını seçin.

    ![Dizin Kimliği](./media/openvpn-azure-ad-tenant-multi-app/pick.png)

6. İstendiğinde **Kabul Et'i** seçin.

    ![Kabul Et](./media/openvpn-azure-ad-tenant-multi-app/accept.jpg)

7. Azure REKLAM'ınız altında, **Kurumsal uygulamalarda** **Azure VPN'inin** listelenmiş olduğunu görürsünüz.

     ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/azure-vpn.png)

## <a name="4-register-additional-applications"></a><a name="register-apps"></a>4. Ek uygulamaları kaydedin

Bu adımda, çeşitli kullanıcılar ve gruplar için ek uygulamalar kaydedin.

1. Azure Etkin Dizininizin **altında, Uygulama kayıtlarını** ve ardından **+ Yeni kaydı**tıklatın.

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/app1.png)

2. Bir başvuru sayfası **kaydol,** **Adı**girin. İstenilen **Desteklenen hesap türlerini**seçin, ardından **Kaydol'u**tıklatın.

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/app2.png)

3. Yeni uygulama kaydedildikten sonra, uygulama bıçağının altındaki **API'yi ortaya çıkar'ı** tıklatın.

4. + **Kapsam ekle'yi**tıklatın.

5. Varsayılan **Uygulama Kimliği URI**bırakın. **Kaydet'i**tıklatın ve devam edin.

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/app3.png)

6. Gerekli alanları doldurun ve **State** **Durum'un Etkin**olduğundan emin olun. **Kapsam Ekle'yi**tıklatın.

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/app4.png)

7. **Bir API'yi göster'i** tıklatın sonra + **İstemci uygulaması ekle.**  **İstemci Kimliği**için buluta bağlı olarak aşağıdaki değerleri girin:

    - Azure **Geneli** için **41b23e61-6c1e-4545-b367-cd054e0ed4b4** girin
    - Azure **Hükümeti** için **51bb15d4-3a4f-4ebf-9dca-40096fe32426** girin
    - Azure **Almanya** **için 538ee9e6-310a-468d-afef-ea97365856a9** girin
    - Azure **China 21Vianet** için **49f817b6-84ae-4cc0-928c-73f27289b3aa'yı** girin

8. **Uygulama Ekle'yi**tıklatın.

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/app5.png)

9. **Genel Bakış** sayfasından **Uygulama (istemci) kimliğini** kopyalayın. VPN ağ geçidi(ler) yapılandırmak için bu bilgilere ihtiyacınız olacaktır.

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/app6.png)

10. Güvenlik gereksiniminiz için gereken sayıda uygulama oluşturmak için bu [kayıt ek uygulamalar](#register-apps) bölümündeki adımları yineleyin. Her uygulama bir VPN ağ geçidi ile ilişkilendirilecektir ve farklı bir kullanıcı kümesine sahip olabilir. Yalnızca bir uygulama bir ağ geçidi ile ilişkilendirilebilir.

## <a name="5-assign-users-to-applications"></a><a name="assign-users"></a>5. Kullanıcıları uygulamalara atama

Kullanıcıları uygulamalarınız için atayın.

1. **Azure AD -> Kurumsal uygulamaları**altında, yeni kaydedilmiş uygulamayı seçin ve **Özellikler'i**tıklatın. Kullanıcı **ataması gerekli** olduğundan **yes**emin olun? **Kaydet**'e tıklayın.

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/user2.png)

2. Uygulama **sayfasında, Kullanıcılar ve gruplar'ı**tıklatın ve ardından **+Kullanıcı Ekle'yi**tıklatın.

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/user3.png)

3. **Atama Ekle** **altında, Kullanıcılar ve gruplar'ı**tıklatın. Bu VPN uygulamasına erişmek istediğiniz kullanıcıları seçin. **Seç'i**tıklatın.

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/user4.png)