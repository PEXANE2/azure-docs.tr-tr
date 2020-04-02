---
title: Git - Azure | Microsoft Dokümanlar
description: Git'i kullanarak API Yönetimi hizmet yapılandırmanızı nasıl kaydedin ve yapılandırın.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: mattfarm
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 03/12/2019
ms.author: apimpm
ms.openlocfilehash: b9184808b71cce03882022fd37967fe421e64062
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80548978"
---
# <a name="how-to-save-and-configure-your-api-management-service-configuration-using-git"></a>API Management hizmeti yapılandırmanızı Git kullanarak kaydetme ve yapılandırma

Her API Yönetimi hizmeti örneği, hizmet örneği için yapılandırma ve meta veriler hakkında bilgi içeren bir yapılandırma veritabanı tutar. Azure portalındaki bir ayarı değiştirerek, PowerShell cmdlet kullanarak veya REST API çağrısı yaparak hizmet örneğinde değişiklik yapılabilir. Bu yöntemlere ek olarak, Git'i kullanarak hizmet örneği yapılandırmanızı da yöneterek şu gibi hizmet yönetimi senaryolarını etkinleştirebilirsiniz:

* Yapılandırma sürümü - hizmet yapılandırmanızın farklı sürümlerini indirin ve saklayın
* Toplu yapılandırma değişiklikleri - yerel deponuzda hizmet yapılandırmanızın birden çok parçasında değişiklik yapın ve değişiklikleri tek bir işlemle sunucuya geri tümleştirin
* Tanıdık Git araç zinciri ve iş akışı - zaten aşina olduğunuz Git takımlarını ve iş akışlarını kullanın

Aşağıdaki diyagram, API Yönetimi hizmet örneğini yapılandırmanın farklı yollarının genel görünümünü gösterir.

![Git yapılandırma][api-management-git-configure]

Azure portalını, PowerShell cmdlets'i veya REST API'sini kullanarak hizmetinizde değişiklik yaptığınızda, diyagramın `https://{name}.management.azure-api.net` sağ tarafında gösterildiği gibi son noktayı kullanarak hizmet yapılandırma veritabanınızı yönetirsiniz. Diyagramın sol tarafında, hizmetiniz için Git ve Git deposunu kullanarak hizmet yapılandırmanızı `https://{name}.scm.azure-api.net`nasıl yönetebileceğinizi göstermektedir.

Aşağıdaki adımlar, Git'i kullanarak API Yönetimi hizmet örneğini yönetmeye genel bir bakış sağlar.

1. Hizmetinizde Git yapılandırmanıza erişin
2. Hizmet yapılandırma veritabanınızı Git deponuza kaydetme
3. Git repo'yu yerel makinenize klonlama
4. En son repo'yu yerel makinenize çekin ve değişiklikleri repo'nuza geri itin
5. Repo'nuzdaki değişiklikleri hizmet yapılandırma veritabanınıza dağıtma

Bu makalede, hizmet yapılandırmanızı yönetmek için Git'i nasıl etkinleştireceğimiz ve kullanacağınız açıklanır ve Git deposundaki dosya ve klasörler için bir başvuru sağlar.

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="access-git-configuration-in-your-service"></a>Hizmetinizde Git yapılandırmanıza erişin

Git yapılandırma ayarlarınızı görüntülemek ve yapılandırmak için **Güvenlik** menüsünü tıklatabilir ve **Yapılandırma deposu** sekmesine gidebilirsiniz.

![GIT'i etkinleştir][api-management-enable-git]

> [!IMPORTANT]
> Adlandırılmış Değerler olarak tanımlanmayan tüm sırlar depoda depolanır ve siz Git erişimini devre dışı bırakıp yeniden etkinleştirene kadar geçmişinde kalır. Adlandırılmış Değerler, tüm API yapılandırması ve ilkeleri arasında sırlar da dahil olmak üzere sabit dize değerlerini yönetmek için güvenli bir yer sağlar, böylece bunları doğrudan ilke deyimlerinizde depolamanız gerekmez. Daha fazla bilgi için, [Azure API Yönetimi ilkelerinde Adlandırılmış Değerler'in nasıl kullanılacağına](api-management-howto-properties.md)bakın.
>
>

REST API'sini kullanarak Git erişimini etkinleştirme veya devre dışı bırakma hakkında bilgi için, [REST API'sini kullanarak Git erişimini etkinleştir mecaz veya devre dışı bırakma bilgisine](/rest/api/apimanagement/2019-01-01/tenantaccess?EnableGit)bakın.

## <a name="to-save-the-service-configuration-to-the-git-repository"></a>Hizmet yapılandırmasını Git deposuna kaydetmek için

Depoyu klonlamadan önceki ilk adım, hizmet yapılandırmasının geçerli durumunu depoya kaydetmektir. **Depo için Kaydet'i**tıklatın.

Onay ekranında istediğiniz değişiklikleri yapın ve kaydetmek için **Tamam'a** tıklayın.

Birkaç dakika sonra yapılandırma kaydedilir ve deponun yapılandırma durumu, son yapılandırma değişikliğinin tarihi ve saati ve hizmet yapılandırması ile depo arasındaki son eşitleme de dahil olmak üzere görüntülenir.

Yapılandırma depoya kaydedildikten sonra klonlanabilir.

REST API'yi kullanarak bu işlemi gerçekleştirme hakkında bilgi [için, REST API'sini kullanarak yapılandırma anlık görüntüsünü işleme'ye](/rest/api/apimanagement/2019-01-01/tenantaccess?CommitSnapshot)bakın.

## <a name="to-clone-the-repository-to-your-local-machine"></a>Depoyu yerel makinenize klonlamak için

Bir depoyu klonlamak için, deponuzun URL'sine, kullanıcı adıve parolaya ihtiyacınız var. Kullanıcı adı ve diğer kimlik bilgilerini almak için sayfanın üst kısmındaki **Access kimlik bilgilerini** tıklatın.

Parola oluşturmak için önce **Son Kullanma Süresinin** istenen son kullanma tarihi ve saatine ayarlandığından emin olun ve sonra **Oluştur'u**tıklatın.

> [!IMPORTANT]
> Bu parolayı not alın. Bu sayfadan çıktıktan sonra parola yeniden görüntülenmez.
>

Aşağıdaki [örnekler, Windows için Git'in](https://www.git-scm.com/downloads) Git Bash aracını kullanır, ancak aşina olduğunuz herhangi bir Git aracını kullanabilirsiniz.

Git aracınızı istediğiniz klasörde açın ve Azure portalı tarafından sağlanan komutu kullanarak git deposunu yerel makinenize klonlamak için aşağıdaki komutu çalıştırın.

```
git clone https://{name}.scm.azure-api.net/
```

İstendiğinde kullanıcı adı ve parolayı sağlayın.

Herhangi bir hata alırsanız, `git clone` aşağıdaki örnekte gösterildiği gibi, komutunuzu kullanıcı adı ve parolayı içerecek şekilde değiştirmeyi deneyin.

```
git clone https://username:password@{name}.scm.azure-api.net/
```

Bu bir hata sağlıyorsa, komutun parola bölümünü kodlayan URL'yi deneyin. Bunu yapmanın hızlı bir yolu Visual Studio'yu açmak ve **Hemen Pencere'de**aşağıdaki komutu vermektir. **Hemen Pencereyi**açmak için Visual Studio'da herhangi bir çözümü veya projeyi açın (veya yeni bir boş konsol uygulaması oluşturun) ve **Hata Ayıklama** menüsünden **Hemen** **Windows'u**seçin.

```
?System.Net.WebUtility.UrlEncode("password from the Azure portal")
```

Git komutunu oluşturmak için kodlanmış parolayı kullanıcı adınız ve depo konumunuzla birlikte kullanın.

```
git clone https://username:url encoded password@{name}.scm.azure-api.net/
```

Depo klonlandıktan sonra, yerel dosya sisteminizde görüntüleyebilir ve bu depoyla çalışabilirsiniz. Daha fazla bilgi için yerel [Git deposunun Dosya ve klasör yapısı başvurusuna](#file-and-folder-structure-reference-of-local-git-repository)bakın.

## <a name="to-update-your-local-repository-with-the-most-current-service-instance-configuration"></a>Yerel deponuzu en güncel hizmet örneği yapılandırmasıyla güncelleştirmek için

Azure portalında API Yönetimi hizmet örneğinde değişiklik yaparsanız veya REST API'sini kullanırsanız, yerel deponuzu en son değişikliklerle güncelleştirmeden önce bu değişiklikleri depoya kaydetmeniz gerekir. Bunu yapmak için, Azure portalındaki Yapılandırma **deposu** sekmesinde depo oluşturmak **için yapılandırmayı** kaydet'i tıklatın ve ardından yerel deponuzda aşağıdaki komutu sorun.

```
git pull
```

Çalıştırmadan `git pull` önce yerel deponuzun klasöründe olduğunuzdan emin olun. Komutu `git clone` yeni tamamladıysanız, aşağıdaki gibi bir komut çalıştırarak dizini repo'nuzla değiştirmeniz gerekir.

```
cd {name}.scm.azure-api.net/
```

## <a name="to-push-changes-from-your-local-repo-to-the-server-repo"></a>Yerel repo'nuzdaki değişiklikleri sunucu repo'ya itmek için
Yerel deponuzdaki değişiklikleri sunucu deposuna itmek için değişikliklerinizi işlemeli ve bunları sunucu deposuna itmelisiniz. Değişikliklerinizi işlemek için Git komut aracınızı açın, yerel deponuzun dizinine geçin ve aşağıdaki komutları düzenleyin.

```
git add --all
git commit -m "Description of your changes"
```

Tüm taahhütleri sunucuya itmek için aşağıdaki komutu çalıştırın.

```
git push
```

## <a name="to-deploy-any-service-configuration-changes-to-the-api-management-service-instance"></a>ApI Yönetimi hizmet örneğine herhangi bir hizmet yapılandırma sıcağı değişikliği dağıtmak için

Yerel değişiklikleriniz işlendikten ve sunucu deposuna itildikten sonra, bunları API Yönetimi hizmet örneğinize dağıtabilirsiniz.

REST API'yi kullanarak bu işlemi gerçekleştirme hakkında bilgi için, [REST API'sını kullanarak yapılandırma veritabanına Git değişikliklerini dağıt'a](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/tenantconfiguration)bakın.

## <a name="file-and-folder-structure-reference-of-local-git-repository"></a>Yerel Git deposunun dosya ve klasör yapısı başvurusu

Yerel git deposundaki dosya ve klasörler, hizmet örneği yle ilgili yapılandırma bilgilerini içerir.

| Öğe | Açıklama |
| --- | --- |
| kök api yönetimi klasörü |Hizmet örneği için üst düzey yapılandırma içerir |
| apis klasörü |Hizmet örneğindeki apis yapılandırmasını içerir |
| gruplar klasörü |Hizmet örneğindeki gruplar için yapılandırmayı içerir |
| ilkeler klasörü |Hizmet örneğindeki ilkeleri içerir |
| portalStyles klasörü |Hizmet örneğindeki geliştirici portalı özelleştirmeleri için yapılandırmayı içerir |
| ürünler klasörü |Hizmet örneğindeki ürünler için yapılandırmayı içerir |
| şablonlar klasörü |Hizmet örneğindeki e-posta şablonları için yapılandırmayı içerir |

Her klasör bir veya daha fazla dosya içerebilir ve bazı durumlarda bir veya daha fazla klasör (örneğin, her API, ürün veya grup için bir klasör). Her klasördeki dosyalar, klasör adı tarafından açıklanan varlık türüne özgüdür.

| Dosya türü | Amaç |
| --- | --- |
| json |İlgili varlık la ilgili yapılandırma bilgileri |
| html |Genellikle geliştirici portalında görüntülenen varlık la ilgili açıklamalar |
| xml |İlke deyimleri |
| css |Geliştirici portalı özelleştirmesi için stil sayfaları |

Bu dosyalar yerel dosya sisteminizde oluşturulabilir, silinebilir, düzenlenebilir ve yönetilebilir ve değişiklikler API Yönetimi hizmet örneğinize geri dağıtılabilir.

> [!NOTE]
> Aşağıdaki varlıklar Git deposunda içermez ve Git kullanılarak yapılandırılamaz.
>
> * [Kullanıcılar](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/user)
> * [Abonelikler](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/subscription)
> * [Adlandırılmış Değerler](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/property)
> * Stiller dışındaki geliştirici portal varlıkları
>

### <a name="root-api-management-folder"></a>Kök api yönetimi klasörü
Kök `api-management` klasör, `configuration.json` aşağıdaki biçimde hizmet örneği hakkında üst düzey bilgiler içeren bir dosya içerir.

```json
{
  "settings": {
    "RegistrationEnabled": "True",
    "UserRegistrationTerms": null,
    "UserRegistrationTermsEnabled": "False",
    "UserRegistrationTermsConsentRequired": "False",
    "DelegationEnabled": "False",
    "DelegationUrl": "",
    "DelegatedSubscriptionEnabled": "False",
    "DelegationValidationKey": "",
    "RequireUserSigninEnabled": "false"
  },
  "$ref-policy": "api-management/policies/global.xml"
}
```

İlk dört ayarı `UserRegistrationTerms` `UserRegistrationTermsEnabled`(, `UserRegistrationTermsConsentRequired``RegistrationEnabled`, , ve ) **Güvenlik** bölümündeki **Kimlikler** sekmesinde aşağıdaki ayarların eşlenir.

| Kimlik ayarı | Haritalar için |
| --- | --- |
| RegistrationEnabled |Kullanıcı **adı ve şifre** kimlik sağlayıcısının varlığı |
| UserRegistrationTerms |**Kullanıcı kayıt** textbox kullanım koşulları |
| UserRegistrationTermsEnabled |**Kayıt sayfası** onay kutusunda kullanım koşullarını göster |
| UserRegistrationTermsConsentGerekli |İzin onay kutusu **iste** |
| KullanıcıSigninEtkin gerektirir |**Anonim kullanıcıları oturum açma sayfası** onay kutusuna yönlendirme |

Sonraki dört ayarı `DelegationUrl` `DelegatedSubscriptionEnabled`(, `DelegationValidationKey``DelegationEnabled`, , ve ) **Güvenlik** bölümündeki **Temsilci** sekmesindeki aşağıdaki ayarların haritasını oluşturur.

| Delegasyon ayarı | Haritalar için |
| --- | --- |
| Delegasyon Etkin |**Oturum açma & oturum açma** onay kutusu |
| DelegasyonUrl |**Temsilci uç noktası URL** textbox |
| TemsilciLikAbonelik Etkin |**Temsilci ürün abonelik** onay kutusu |
| DelegasyonValidationKey |**Temsilci Doğrulama Anahtarı** textbox |

Son ayar, `$ref-policy`hizmet örneği için genel ilke deyimleri dosyasına eşler.

### <a name="apis-folder"></a>apis klasörü
Klasör, `apis` hizmet örneğindeki her API için aşağıdaki öğeleri içeren bir klasör içerir.

* `apis\<api name>\configuration.json`- Bu API için yapılandırma ve arka uç hizmet URL ve işlemler hakkında bilgi içerir. Bu, biçimde belirli bir API al'ı aramanız `export=true` durumunda `application/json` döndürülecek [bilgilerle](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/apis/get) aynıdır.
* `apis\<api name>\api.description.html`- Bu API açıklamasıve `description` [API varlığın](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.table.entityproperty)özelliğine karşılık gelir.
* `apis\<api name>\operations\`- Bu `<operation name>.description.html` klasör, API'deki işlemlerin eşlenediğini gösteren dosyalar içerir. Her dosya, REST API'deki işlem `description` [varlığının](https://docs.microsoft.com/rest/api/visualstudio/operations/list#operationproperties) özelliğiyle eşleyen API'deki tek bir işlemin açıklamasını içerir.

### <a name="groups-folder"></a>gruplar klasörü
Klasör, `groups` hizmet örneğinde tanımlanan her grup için bir klasör içerir.

* `groups\<group name>\configuration.json`- Bu grup için yapılandırmadır. Bu, belirli bir grup işlemini al'ı aramanız durumunda döndürülecek [bilgilerle aynıdır.](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/group/get)
* `groups\<group name>\description.html`- Bu grubun açıklamasıdır ve grup `description` [varlığıözelliğine](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-group-entity)karşılık gelir.

### <a name="policies-folder"></a>ilkeler klasörü
Klasör, `policies` hizmet örneğinizin ilke ekstrelerini içerir.

* `policies\global.xml`- hizmet örneğiniz için genel kapsamda tanımlanan ilkeleri içerir.
* `policies\apis\<api name>\`- API kapsamında tanımlanmış herhangi bir ilkeniz varsa, bunlar bu klasörde bulunur.
* `policies\apis\<api name>\<operation name>\`klasör - çalışma kapsamında tanımlanmış herhangi bir ilkeniz varsa, `<operation name>.xml` bunlar bu klasörde her işlem için ilke deyimleri eşleyen dosyalarda bulunur.
* `policies\products\`- Ürün kapsamında tanımlanmış herhangi bir ilkeniz varsa, bunlar `<product name>.xml` her ürün için ilke ekstrelerine eşleyen dosyaları içeren bu klasörde bulunur.

### <a name="portalstyles-folder"></a>portalStyles klasörü
Klasör, `portalStyles` hizmet örneği için geliştirici portalı özelleştirmeleri için yapılandırma ve stil sayfaları içerir.

* `portalStyles\configuration.json`- geliştirici portalı tarafından kullanılan stil sayfalarının adlarını içerir
* `portalStyles\<style name>.css`- `<style name>.css` Her dosya geliştirici portalı`Preview.css` için `Production.css` stilleri içerir (ve varsayılan olarak).

### <a name="products-folder"></a>ürünler klasörü
Klasör, `products` hizmet örneğinde tanımlanan her ürün için bir klasör içerir.

* `products\<product name>\configuration.json`- Bu ürün için yapılandırmadır. Bu, belirli bir ürün işlemini al'ı aramanız durumunda döndürülecek [bilgilerle aynıdır.](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/product/get)
* `products\<product name>\product.description.html`- Bu ürünün tanımıdır ve REST `description` API'deki [ürün varlığının](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-product-entity) özelliğine karşılık gelir.

### <a name="templates"></a>templates
Klasör, `templates` hizmet örneğinin [e-posta şablonları](api-management-howto-configure-notifications.md) için yapılandırma içerir.

* `<template name>\configuration.json`- Bu e-posta şablonu için yapılandırmadır.
* `<template name>\body.html`- Bu e-posta şablonu gövdesidir.

## <a name="next-steps"></a>Sonraki adımlar
Hizmet örneğini yönetmenin diğer yolları hakkında bilgi için bkz:

* Aşağıdaki PowerShell cmdlets kullanarak servis örneğini yönetme
  * [Hizmet dağıtımı PowerShell cmdlet başvurusu](https://docs.microsoft.com/powershell/module/wds)
  * [Hizmet yönetimi PowerShell cmdlet başvurusu](https://docs.microsoft.com/powershell/azure/servicemanagement/overview)
* REST API'yi kullanarak servis örneğini yönetme
  * [API Yönetimi REST API referans](/rest/api/apimanagement/)


[api-management-enable-git]: ./media/api-management-configuration-repository-git/api-management-enable-git.png
[api-management-git-enabled]: ./media/api-management-configuration-repository-git/api-management-git-enabled.png
[api-management-save-configuration]: ./media/api-management-configuration-repository-git/api-management-save-configuration.png
[api-management-save-configuration-confirm]: ./media/api-management-configuration-repository-git/api-management-save-configuration-confirm.png
[api-management-configuration-status]: ./media/api-management-configuration-repository-git/api-management-configuration-status.png
[api-management-configuration-git-clone]: ./media/api-management-configuration-repository-git/api-management-configuration-git-clone.png
[api-management-generate-password]: ./media/api-management-configuration-repository-git/api-management-generate-password.png
[api-management-password]: ./media/api-management-configuration-repository-git/api-management-password.png
[api-management-git-configure]: ./media/api-management-configuration-repository-git/api-management-git-configure.png
[api-management-configuration-deploy]: ./media/api-management-configuration-repository-git/api-management-configuration-deploy.png
[api-management-identity-settings]: ./media/api-management-configuration-repository-git/api-management-identity-settings.png
[api-management-delegation-settings]: ./media/api-management-configuration-repository-git/api-management-delegation-settings.png
[api-management-git-icon-enable]: ./media/api-management-configuration-repository-git/api-management-git-icon-enable.png




