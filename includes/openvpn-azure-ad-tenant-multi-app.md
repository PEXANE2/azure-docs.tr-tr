---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 05/21/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 2eb2dbc43c59f4f6301c7f5073a73462639d35b2
ms.sourcegitcommit: a9784a3fd208f19c8814fe22da9e70fcf1da9c93
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83797147"
---
## <a name="1-create-the-azure-ad-tenant"></a><a name="tenant"></a>1. Azure AD kiracısı oluşturma

[Yeni kiracı oluşturma](../articles/active-directory/fundamentals/active-directory-access-create-new-tenant.md) makalesindeki adımları kullanarak BIR Azure AD kiracısı oluşturun:

* Kuruluş adı
* İlk etki alanı adı

  Örnek:

   ![Yeni Azure AD kiracısı](./media/openvpn-azure-ad-tenant-multi-app/new-tenant.png)

## <a name="2-create-tenant-users"></a><a name="users"></a>2. kiracı kullanıcıları oluşturun

Bu adımda, iki Azure AD kiracı kullanıcısı oluşturacaksınız: bir genel yönetici hesabı ve bir ana Kullanıcı hesabı. Ana Kullanıcı hesabı, ana ekleme hesabınız (hizmet hesabı) olarak kullanılır. Bir Azure AD Kiracı Kullanıcı hesabı oluşturduğunuzda, oluşturmak istediğiniz kullanıcı türü için Dizin rolünü ayarlayın. Azure AD kiracınız için en az iki kullanıcı oluşturmak üzere [Bu makaledeki](../articles/active-directory/fundamentals/add-users-azure-active-directory.md) adımları kullanın. Hesap türlerini oluşturmak için **Dizin rolünü** değiştirdiğinizden emin olun:

* Genel Yönetici
* Kullanıcı

## <a name="3-register-the-vpn-client"></a><a name="register-client"></a>3. VPN Istemcisini kaydetme

VPN istemcisini Azure AD kiracısına kaydettirin.

1. Kimlik doğrulaması için kullanmak istediğiniz dizinin dizin KIMLIĞINI bulun. Active Directory sayfasının Özellikler bölümünde listelenir.

    ![Dizin KIMLIĞI](./media/openvpn-azure-ad-tenant-multi-app/directory-id.png)

2. Dizin kimliği'ni kopyalayın.

3. Azure portal, **genel yönetici** rolüne atanan bir kullanıcı olarak oturum açın.

4. Sonra, yönetici onayı verin. Tarayıcınızın adres çubuğuna dağıtım konumunuza ait URL 'YI kopyalayıp yapıştırın:

    Ortak

    ```
    https://login.microsoftonline.com/common/oauth2/authorize?client_id=41b23e61-6c1e-4545-b367-cd054e0ed4b4&response_type=code&redirect_uri=https://portal.azure.com&nonce=1234&prompt=admin_consent
    ````

    Azure Kamu

    ```
    https://login.microsoftonline.us/common/oauth2/authorize?client_id=51bb15d4-3a4f-4ebf-9dca-40096fe32426&response_type=code&redirect_uri=https://portal.azure.us&nonce=1234&prompt=admin_consent
    ````

    Almanya Microsoft Bulut

    ```
    https://login-us.microsoftonline.de/common/oauth2/authorize?client_id=538ee9e6-310a-468d-afef-ea97365856a9&response_type=code&redirect_uri=https://portal.microsoftazure.de&nonce=1234&prompt=admin_consent
    ````

    Azure Çin 21Vianet

    ```
    https://https://login.chinacloudapi.cn/common/oauth2/authorize?client_id=49f817b6-84ae-4cc0-928c-73f27289b3aa&response_type=code&redirect_uri=https://portal.azure.cn&nonce=1234&prompt=admin_consent
    ```

5. İstenirse **genel yönetici** hesabını seçin.

    ![Dizin KIMLIĞI](./media/openvpn-azure-ad-tenant-multi-app/pick.png)

6. İstendiğinde **kabul et** ' i seçin.

    ![Kabul Et](./media/openvpn-azure-ad-tenant-multi-app/accept.jpg)

7. Azure AD 'nizin altında, **Kurumsal uygulamalarda**, **Azure VPN** ' yi listelenmiş görürsünüz.

     ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/azure-vpn.png)

## <a name="4-register-additional-applications"></a><a name="register-apps"></a>4. ek uygulamaları kaydetme

Bu adımda, çeşitli kullanıcılar ve gruplar için ek uygulamalar kaydedersiniz.

1. Azure Active Directory altında **uygulama kayıtları** ve sonra **+ Yeni kayıt**' ye tıklayın.

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/app1.png)

2. **Uygulama kaydetme** sayfasında, **adı**girin. İstediğiniz **Desteklenen hesap türlerini**seçin ve **Kaydet**' e tıklayın.

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/app2.png)

3. Yeni uygulama kaydedildikten sonra uygulama dikey penceresinde **BIR API 'Yi kullanıma** sunma ' ya tıklayın.

4. **+ Kapsam Ekle**' ye tıklayın.

5. Varsayılan **uygulama KIMLIĞI URI**'sini bırakın. **Kaydet ve devam et**' e tıklayın.

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/app3.png)

6. Gerekli alanları doldurup **durumunun** **etkinleştirildiğinden**emin olun. **Kapsam Ekle**' ye tıklayın.

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/app4.png)

7. **API 'Yi kullanıma** sunma ve ardından **bir istemci uygulaması ekleme**' ye tıklayın.  **ISTEMCI kimliği**için buluta bağlı olarak aşağıdaki değerleri girin:

    - Azure **genel** için **41b23e61-6c1e-4545-b367-cd054e0ed4b4** girin
    - Azure **kamu** için **51bb15d4-3a4f-4EBF-9dca-40096fe32426** girin
    - Azure **Almanya** için **538ee9e6-310A-468d-afef-ea97365856a9** girin
    - Azure **Çin 21Vianet** için **49f817b6-84ae-4cc0-928c-73f27289b3aa** girin

8. **Uygulama Ekle**' ye tıklayın.

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/app5.png)

9. **Genel bakış** sayfasından **uygulama (istemci) kimliğini** kopyalayın. VPN ağ geçidlerinizi yapılandırmak için bu bilgilere ihtiyacınız olacaktır.

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/app6.png)

10. Güvenlik gereksiniminize gereken birçok uygulama oluşturmak için bu [ek uygulamaları kaydetme](#register-apps) bölümündeki adımları yineleyin. Her uygulama, bir VPN ağ geçidiyle ilişkilendirilir ve farklı bir kullanıcı kümesine sahip olabilir. Bir ağ geçidiyle yalnızca bir uygulama ilişkilendirilebilir.

## <a name="5-assign-users-to-applications"></a><a name="assign-users"></a>5. kullanıcıları uygulamalara atama

Kullanıcıları uygulamalarınıza atayın.

1. **Azure AD-> kurumsal uygulamalar**altında, yeni kayıtlı uygulamayı seçin ve **Özellikler**' e tıklayın. **Gerekli Kullanıcı atamasının** **Evet**olarak ayarlandığından emin olun. **Kaydet**’e tıklayın.

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/user2.png)

2. Uygulama sayfasında, **Kullanıcılar ve gruplar**' a ve ardından **+ Kullanıcı Ekle**' ye tıklayın.

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/user3.png)

3. **Atama Ekle**altında **Kullanıcılar ve gruplar**' a tıklayın. Bu VPN uygulamasına erişebilmesini istediğiniz kullanıcıları seçin. **Seç**' e tıklayın.

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/user4.png)
