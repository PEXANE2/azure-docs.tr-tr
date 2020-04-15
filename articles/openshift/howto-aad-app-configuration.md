---
title: Azure Red Hat OpenShift için Azure Active Directory tümleştirmesi
description: Microsoft Azure Red Hat OpenShift kümenizde uygulamaları test etmek için bir Azure REKLAM güvenlik grubu ve kullanıcı nasıl oluşturabilirsiniz öğrenin.
author: jimzim
ms.author: jzim
ms.service: container-service
ms.topic: conceptual
ms.date: 05/13/2019
ms.openlocfilehash: f6c4fb5caf746650f95872d50afe31e5693422be
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81382907"
---
# <a name="azure-active-directory-integration-for-azure-red-hat-openshift"></a>Azure Red Hat OpenShift için Azure Active Directory tümleştirmesi

Azure Etkin Dizin (Azure AD) kiracısı oluşturmadıysanız, bu yönergeleri devam etmeden önce [Azure Red Hat OpenShift için Azure AD kiracısı oluştur'daki](howto-create-tenant.md) yönergeleri izleyin.

Microsoft Azure Red Hat OpenShift, kümeniz adına görevleri gerçekleştirmek için izinlere ihtiyaç duyar. Kuruluşunuzun hizmet sorumlusu olarak kullanmak üzere zaten bir Azure AD kullanıcısı, Azure AD güvenlik grubu veya Azure AD uygulama kaydı yoksa, bunları oluşturmak için bu yönergeleri izleyin.

## <a name="create-a-new-azure-active-directory-user"></a>Yeni bir Azure Active Directory kullanıcısı oluşturma

Azure [portalında,](https://portal.azure.com)kiracınızın portalın sağ üst ağında ki kullanıcı adınız altında göründüğünden emin olun:

![Sağ](./media/howto-create-tenant/tenant-callout.png) üstte kiracı nın listelendiği portalın ekran görüntüsü Yanlış kiracı görüntüleniyorsa, sağ üstteki kullanıcı adınızı tıklatın, ardından **Dizin Değiştir'i**tıklatın ve **Tüm Dizinler** listesinden doğru kiracıyı seçin.

Azure Red Hat OpenShift kümenizde oturum açmak için yeni bir Azure Active Directory 'Sahibi' kullanıcısı oluşturun.

1. [Kullanıcılar-Tüm kullanıcılar](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UsersManagementMenuBlade/AllUsers) blade gidin.
2. **Kullanıcı** bölmesini açmak için **+Yeni kullanıcıyı** tıklatın.
3. Bu kullanıcı için bir **Ad** girin.
4. Oluşturduğunuz **User name** kiracının adını temel alan ve `.onmicrosoft.com` sonunda eklenen bir Kullanıcı adı oluşturun. Örneğin, `yourUserName@yourTenantName.onmicrosoft.com`. Bu kullanıcı adını yazın. Kümenizde oturum açabilmek için buna ihtiyacınız olacak.
5. Dizin rol bölmesini açmak için **Dizin rolünü** tıklatın ve **Sahibi'ni** seçin ve ardından bölmenin altındaki **Tamam'ı** tıklatın.
6. **Kullanıcı** bölmesinde **Parolayı Göster'i** tıklatın ve geçici parolayı kaydedin. İlk kez oturum açtıktan sonra sıfırlamanız istenir.
7. Bölmenin alt kısmında, kullanıcıyı oluşturmak için **Oluştur'u** tıklatın.

## <a name="create-an-azure-ad-security-group"></a>Azure AD güvenlik grubu oluşturma

Küme yöneticisierişimi sağlamak için, bir Azure REKLAM güvenlik grubundaki üyelikler OpenShift grubuna "osa-müşteri yöneticileri" eşitlenir. Belirtilmezse, küme yöneticisi erişimi verilmez.

1. Azure [Active Directory gruplarını](https://portal.azure.com/#blade/Microsoft_AAD_IAM/GroupsManagementMenuBlade/AllGroups) açın.
2. **+Yeni Grup'u**tıklatın.
3. Bir grup adı ve açıklaması sağlayın.
4. **Grup türünü** **Güvenlik**olarak ayarlayın.
5. **Üyelik türünü** **Atanmış**olarak ayarlayın.

    Önceki adımda oluşturduğunuz Azure AD kullanıcısını bu güvenlik grubuna ekleyin.

6. **Üyeleri Seç** bölmesini açmak için **Üyeler'i** tıklatın.
7. Üyeler listesinde, yukarıda oluşturduğunuz Azure AD kullanıcısını seçin.
8. Portalın alt kısmında, güvenlik grubu oluşturmak için **Seç'e** ve ardından **Oluştur'a** tıklayın.

    Grup kimliği değerini yazın.

9. Grup oluşturulduğunda, bunu tüm grupların listesinde görürsünüz. Yeni gruba tıklayın.
10. Görünen sayfada **Nesne Kimliğini**kopyalayın. Bu değere Azure `GROUPID` Kırmızı [Şapka OpenShift küme](tutorial-create-cluster.md) oluşturma öğreticisinde olduğu gibi atıfta bulunacağız.

> [!IMPORTANT]
> Bu grubu osa-customer-admins OpenShift grubuyla senkronize etmek için, Azure CLI'yi kullanarak kümeyi oluşturun. Azure portalı şu anda bu grubu ayarlamak için bir alandan yoksundur.

## <a name="create-an-azure-ad-app-registration"></a>Azure AD uygulama kaydı oluşturma

`--aad-client-app-id` Bayrağı `az openshift create` komuta atlayarak kümeoluşturmanın bir parçası olarak otomatik olarak bir Azure Etkin Dizin (Azure AD) uygulama kayıt istemcisi oluşturabilirsiniz. Bu öğretici, tamlık için Azure AD uygulama kaydının nasıl oluşturulabileceğinizi gösterir.

Kuruluşunuzun hizmet sorumlusu olarak kullanmak üzere zaten bir Azure Etkin Dizin (Azure AD) uygulama kaydı yoksa, bir tane oluşturmak için aşağıdaki yönergeleri izleyin.

1. Uygulama [kayıtları bıçak](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview) açın ve **+Yeni kayıt**tıklayın.
2. Bir başvuru bölmesini **kaydedin,** başvuru kaydınız için bir ad girin.
3. **Desteklenen hesap türleri** altında **bu kuruluş dizinindeki Hesapların yalnızca** seçildiğinden emin olun. Bu en güvenli seçimdir.
4. Kümenin URI'sini anladığımızda uri'yi daha sonra yeniden yönlendireceğiz. Azure AD uygulama kaydını oluşturmak için **Kaydol** düğmesini tıklatın.
5. Görünen **sayfada, Uygulama (istemci) kimliğini**kopyalayın. Bu değere Azure `APPID` Kırmızı [Şapka OpenShift küme](tutorial-create-cluster.md) oluşturma öğreticisinde olduğu gibi atıfta bulunacağız.

![Uygulama nesnesayfasının ekran görüntüsü](./media/howto-create-tenant/get-app-id.png)

### <a name="create-a-client-secret"></a>İstemci sırrı oluşturma

Uygulamanızın kimliğini Azure Active Directory olarak doğrulamak için bir istemci sırrı oluşturun.

1. Uygulama kayıtları sayfasının **Yönet** **bölümünde, Sertifikalar & sırları**tıklayın.
2. **Sertifikalar & sırlar** bölmesine **+Yeni istemci sırrını**tıklatın.  **İstemci gizli bölmeekle** görüntülenir.
3. **Açıklama**Sağlayın.
4. Set, örneğin **2 Yıl Içinde,** tercih ettiğiniz süreye **kadar sona erer.**
5. **Ekle'yi** tıklatın ve anahtar değeri sayfanın **İstemci sırları** bölümünde görünür.
6. Anahtar değerini kopyalayın. Bu değere Azure `SECRET` Kırmızı [Şapka OpenShift küme](tutorial-create-cluster.md) oluşturma öğreticisinde olduğu gibi atıfta bulunacağız.

![Sertifikalar ve sırlar bölmesinin ekran görüntüsü](./media/howto-create-tenant/create-key.png)

Azure Uygulama Nesneleri hakkında daha fazla bilgi için [Azure Etkin Dizini'ndeki Uygulama ve hizmet ana nesneleri'ne](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals)bakın.

Yeni bir Azure AD uygulaması oluşturma yla ilgili ayrıntılar [için](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-add-azure-ad-app)bkz.

## <a name="add-api-permissions"></a>API izinleri ekleme

[//]: # (Microsoft Graph olarak değiştirmeyin. Microsoft Graph ile çalışmaz.)
1. **Yönet** bölümünde **API izinlerini** tıklatın
2. **İzin Ekle'yi** tıklatın ve Azure **Etkin Dizin Grafiği'ni** seçin ve ardından **Temsilciler Dairesi'ne izin ler ver.**
> [!NOTE]
> "Microsoft Graph" döşemesini değil, "Azure Etkin Dizin Grafiği"ni seçtiğinizden emin olun.

3. Aşağıdaki listede **Kullanıcı'yı** genişletin ve **User.Read** iznini etkinleştirin. **Kullanıcı.Read** varsayılan olarak etkinse, Azure **Etkin Dizin Grafiği** izni Kullanıcı olduğundan emin **olun.Oku**.
4. Yukarı kaydırın ve **Uygulama izinlerini**seçin.
5. Aşağıdaki listede **Dizin** genişletin ve **Directory.ReadAll**etkinleştirin.
6. Değişiklikleri kabul etmek için **İzin Ekle'yi** tıklatın.
7. API izinleri paneli artık hem *User.Read* hem de *Directory.ReadAll'ı*göstermelidir. Lütfen **Admin onayı gerekli** *sütunda Uyarı Dizin.ReadAll*yanında unutmayın.
8. Azure Abonelik *Yöneticisiyseniz,* aşağıdaki **Abonelik *Adı* için Yönetici Onayı Ver'i** tıklatın. *Azure Abonelik Yöneticisi*değilseniz, yöneticinizden onay isteyin.

![API izinleri panelinin ekran görüntüsü. User.Read and Directory.ReadTüm izinler eklendi, yönetici onayı dizin için gerekli.ReadAll](./media/howto-aad-app-configuration/permissions-required.png)

> [!IMPORTANT]
> Küme yöneticileri grubunun eşitlenmesi yalnızca onay verildikten sonra çalışır. *Yönetici onayı için gerekli* sütunda bir onay işareti ve *"Abonelik Adı*için verilen" iletisi içeren yeşil bir daire görürsünüz.

Yöneticilerin ve diğer rollerin yönetimi yle ilgili ayrıntılar için Azure [abonelik yöneticileri ekle veya değiştir'](https://docs.microsoft.com/azure/billing/billing-add-change-azure-subscription-administrator)bölümüne bakın.

## <a name="resources"></a>Kaynaklar

* [Azure Active Directory'deki uygulamalar ve hizmet temel nesneleri](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals)
* [Hızlı Başlangıç: Azure Active Directory v1.0 uç noktasına uygulama kaydetme](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-add-azure-ad-app)

## <a name="next-steps"></a>Sonraki adımlar

[Tüm Azure Red Hat OpenShift ön koşullarıyla](howto-setup-environment.md)karşılaştıysanız, ilk kümenizi oluşturmaya hazırsınız!

Öğreticiyi deneyin:
> [!div class="nextstepaction"]
> [Azure Red Hat OpenShift kümesi oluşturma](tutorial-create-cluster.md)
