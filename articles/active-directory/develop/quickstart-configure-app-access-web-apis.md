---
title: "Quickstart: Uygulama için web API'leri eriş - Microsoft kimlik platformu | Azure"
description: Bu hızlı başlangıçta, Microsoft kimlik platformuna kayıtlı bir uygulamayı yeniden yönlendirme IU'larını, kimlik bilgilerini veya web API'lerine erişim izinlerini içerecek şekilde yapılandırın.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 03/09/2020
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: lenalepa, aragra, sureshja
ms.openlocfilehash: 5e628626f2db49ff67d6d7ab425a3a19870b1ebd
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79240901"
---
# <a name="quickstart-configure-a-client-application-to-access-web-apis"></a>Hızlı başlatma: Web API'lerine erişmek için istemci uygulamasını yapılandırma

Bu hızlı başlangıçta, uygulamanız için web API'lerine erişmek için yeniden yönlendirme URI'leri, kimlik bilgileri veya izinler eklersiniz. Web veya gizli istemci uygulamasının kimlik doğrulaması gerektiren bir yetkilendirme hibe akışına katılmak için güvenli kimlik bilgileri oluşturması gerekir. Azure portal tarafından desteklenen varsayılan kimlik doğrulaması yöntemi istemci kimliği ve gizli anahtar kullanımıdır. Uygulama bu işlem sırasında bir erişim belirteci elde eder.

İstemci, Microsoft Graph API gibi bir kaynak uygulaması tarafından açığa çıkarılan bir web API'sına erişemeden önce, onay çerçevesi istemcinin istenen izinler için gerekli izin iznini almasını sağlar. Varsayılan olarak, tüm uygulamalar Microsoft Graph API'den izin isteyebilir.

## <a name="prerequisites"></a>Ön koşullar

* [Quickstart'ın Tamamlanması: Bir uygulamayı Microsoft kimlik platformuna kaydedin.](quickstart-register-app.md)
* Microsoft [kimlik platformu bitiş noktasında İzinlerin ve onayların](v2-permissions-and-consent.md)gözden geçirilmesi.
* Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)

## <a name="sign-in-to-the-azure-portal-and-select-the-app"></a>Azure portalında oturum açın ve uygulamayı seçin

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Hesabınız birden fazla kiracıya erişim sağlıyorsa, sağ üst köşedeki hesabınızı seçin. Portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
1. **Azure Active Directory**'yi bulun ve seçin. **Yönet**'in altında **Uygulama kayıtları**nı seçin.
1. Yapılandırmak istediğiniz uygulamayı bulun ve seçin. Uygulamayı seçtikten sonra, uygulamanın **Genel Bakış** veya ana kayıt sayfasını görürsünüz.

Web API'lerine erişmek için uygulamanızı yapılandırmak için aşağıdaki yordamları kullanın.

## <a name="add-redirect-uris-to-your-application"></a>Uygulamanıza yeniden yönlendirme URI’leri ekleme

Özel yönlendirme URI'leri ekleyebilir ve uygulamanız için önerilen yönlendirme URI'leri ekleyebilirsiniz. Web ve genel istemci uygulamaları için özel yönlendirme URI eklemek için:

1. Uygulamaya **Genel Bakış** sayfasından **Kimlik Doğrulama'yı**seçin.
1. **Yönlendirme URI'lerini**bulun. **Eski deneyime Geçiş'i**seçmeniz gerekebilir.
1. Oluşturmakta olduğunuz uygulama türünü seçin: **Web** veya **Public istemci/yerel (mobil & masaüstü)**.
1. Uygulamanız için Yeniden Yönlendirme URI’sini girin.

   * Web uygulamaları için, uygulamanızın temel URL'sini girin. Örneğin `http://localhost:31544` yerel makinenizde çalışan bir web uygulamasının URL'si olabilir. Kullanıcılar, bir web istemci uygulamasında oturum açmak için bu URL'yi kullanır.
   * Genel uygulamalar için, Azure AD'nin belirteç yanıtlarını döndürmek üzere kullandığı URI'yi girin. Örneğin, uygulamanıza özgü bir değer `https://MyFirstApp`girin: .
1. **Kaydet'i**seçin.

Ortak istemciler için önerilen yönlendirme URL'leri arasından seçim yapmak için aşağıdaki adımları izleyin:

1. Uygulamaya **Genel Bakış** sayfasından **Kimlik Doğrulama'yı**seçin.
1. **Ortak istemciler (mobil, masaüstü) için Önerilen Yönlendirme URL'lerini**bulun. **Eski deneyime Geçiş'i**seçmeniz gerekebilir.
1. Uygulamanız için bir veya daha fazla yönlendirme URI'sini seçin. Ayrıca özel bir yeniden yönlendirme URI girebilirsiniz. Ne kullanacağından emin değilseniz, kitaplık belgelerine bakın.
1. **Kaydet'i**seçin.

UUİ'leri yönlendirmek için bazı kısıtlamalar uygulanır. Daha fazla bilgi için bkz: [URI/yanıtURL kısıtlamalarını ve sınırlamalarını yeniden yönlendirin.](https://docs.microsoft.com/azure/active-directory/develop/reply-url)

> [!NOTE]
> Hedeflemek istediğiniz platforma veya cihaza göre uygulamanızın ayarlarını yapılandırabileceğiniz yeni **Kimlik Doğrulama** ayarları deneyimini deneyin.
>
> Bu görünümü görmek için **Kimlik Doğrulama** sayfasından **yeni deneyimi deneyin'i** seçin.
>
> ![Platform yapılandırma görünümünü görmek için "Yeni deneyimi deneyin" seçeneğini tıklayın](./media/quickstart-update-azure-ad-app-preview/authentication-try-new-experience-cropped.png)
>
> Bu sizi [yeni **Platform yapılandırmaları** sayfasına](#configure-platform-settings-for-your-application)götürür.

### <a name="configure-advanced-settings-for-your-application"></a>Uygulamanız için gelişmiş ayarları yapılandırma

Kaydettiğiniz uygulamaya bağlı olarak, yapılandırmanız gereken bazı ek ayarlar vardır( örneğin:

* **Çıkış URL'si**.
* Tek sayfalı uygulamalar da **Örtülü hibeyi** etkinleştirebilir ve yetkilendirme bitiş noktasının vermesini istediğiniz belirteçleri seçebilirsiniz.
* **Varsayılan istemci türü** bölümünde Tümleşik Windows Kimlik Doğrulaması, aygıt kodu akışı veya kullanıcı adı/parola kullanarak belirteçler elde eden masaüstü uygulamaları **için, Genel İstemci** ayarı olarak Uygulama Yı **Evet**olarak ayarlayın.
* Microsoft hesap hizmetiyle tümleştirmek için Live SDK'yı kullanan eski uygulamalar için **Live SDK desteğini**yapılandırın. Yeni uygulamaların bu ayara ihtiyacı yoktur.
* **Varsayılan istemci türü**.
* **Desteklenen hesap türleri.**

### <a name="modify-supported-account-types"></a>Desteklenen hesap türlerini değiştirme

**Desteklenen hesap türleri,** uygulamayı kimlerin kullanabileceğini veya API'ye kimlerin erişebileceğini belirtir.

Uygulamayı kaydettirirken desteklenen hesap türlerini yapılandırmışsanız, bu ayarı yalnızca aşağıdakiler aşağıdakileri varsa uygulama bildirimi düzenleyicisini kullanarak değiştirebilirsiniz:

* Hesap türlerini **AzureADMyOrg** veya **AzureADMultipleOrgs'tan** **AzureADandPersonalMicrosoftAccount'a**veya diğer yollarla değiştirirsiniz veya
* Hesap türlerini **AzureADMyOrg'dan** **AzureADMultipleOrgs'a**veya başka bir şekilde değiştirirsiniz.

Varolan bir uygulama kaydı için desteklenen hesap `signInAudience` türlerini değiştirmek için anahtarı güncelleştirin. Daha fazla bilgi için [bkz.](reference-app-manifest.md#configure-the-app-manifest)

## <a name="configure-platform-settings-for-your-application"></a>Uygulamanız için platform ayarlarını yapılandırın

![Uygulamanızın ayarlarını platforma veya cihaza göre yapılandırın](./media/quickstart-update-azure-ad-app-preview/authentication-new-platform-configurations.png)

Uygulama ayarlarını platforma veya aygıta göre yapılandırmak için aşağıdakileri hedefliyorsunuz:

1. Platform **yapılandırmaları** sayfasında, **platform ekle'yi** seçin ve kullanılabilir seçenekler arasından seçim yapın.

   ![Yapılandırma platformları sayfasını gösterir](./media/quickstart-update-azure-ad-app-preview/authentication-platform-configurations-configure-platforms.png)

1. Seçtiğiniz platforma göre ayarlar bilgilerini girin.

   | Platform                | Yapılandırma ayarları            |
   |-------------------------|-----------------------------------|
   | **Web**              | Uygulamanız için **Uri'yi Yeniden Yönlendirme'yi** girin. |
   | **iOS / macOS**              | Info.plist'te XCode'da bulabileceğiniz uygulama **Paket Kimliği'ni**veya Yapı Ayarları'nı girin. Paket kimliğinin eklenmesi, uygulama için otomatik olarak yeniden yönlendirme URI oluşturur. |
   | **Android**          | AndroidManifest.xml dosyasında bulabileceğiniz uygulama **Paketi adını**sağlayın.<br/>**İmza karmasını**oluşturun ve girin. İmza karma ekleme otomatik olarak uygulama için bir yönlendirme URI oluşturur.  |
   | **Mobil ve masaüstü uygulamaları**  | İsteğe bağlı. Masaüstü ve aygıtlar için uygulamalar oluşturuyorsanız **önerilen önerilen yeniden yönlendirme URL'lerinden** birini seçin.<br/>İsteğe bağlı. Azure AD'nin kimlik doğrulama isteklerine yanıt olarak kullanıcıları yönlendireceği konum olarak kullanılan **Özel yeniden yönlendirme URI'yi**girin. Örneğin, etkileşim istediğiniz .NET Core uygulamaları için `https://localhost`. |

   > [!IMPORTANT]
   > En son Microsoft Kimlik Doğrulama Kitaplığını (MSAL) kullanmayan veya bir aracı kullanmayan mobil uygulamalar için, bu uygulamalar için yeniden yönlendirme URL'lerini **Masaüstü + aygıtlarında**yapılandırmanız gerekir.

Seçtiğiniz platforma bağlı olarak, yapılandırabileceğiniz ek ayarlar olabilir. **Web** uygulamaları için şunları yapabilirsiniz:

* Daha fazla yönlendirme URI'si ekleyin
* Yetkilendirme bitiş noktası tarafından verilmesini istediğiniz belirteçleri seçmek için **Örtülü hibeyi** yapılandırın:

  * Tek sayfalı uygulamalar için hem **Erişim belirteçleri** hem de **kimlik belirteçleri** seçin
  * Web uygulamaları için **kimlik belirteçlerini** seçin

## <a name="add-credentials-to-your-web-application"></a>Web uygulamanıza kimlik bilgileri ekleme

Web uygulamanıza bir kimlik bilgisi eklemek için, bir sertifika ekleyin veya bir istemci sırrı oluşturun. Sertifika eklemek için:

1. Uygulamaya **Genel Bakış** sayfasından **Sertifikalar & sırları** bölümünü seçin.
1. **Sertifikayı karşıya yükle**’yi seçin.
1. Yüklemek istediğiniz dosyayı seçin. Şu dosya türlerinden biri olmalıdır: .cer, .pem, .crt.
1. **Ekle'yi**seçin.

İstemci sırrı eklemek için:

1. Uygulamaya **Genel Bakış** sayfasından **Sertifikalar & sırları** bölümünü seçin.
1. **Yeni istemci gizli dizisi**’ni seçin.
1. İstemci gizli diziniz için bir açıklama ekleyin.
1. Bir süre seçin.
1. **Ekle'yi**seçin.

> [!NOTE]
> Yapılandırmada yaptığınız değişiklikleri kaydettikten sonra en sağdaki sütunda istemci gizli dizi değeri gösterilir. İstemci uygulamanızın kodunda kullanmak üzere **değeri kopyalamayı unutmayın**. Bu sayfadan ayrıldıktan sonra bu değere erişemezsiniz.

## <a name="add-permissions-to-access-web-apis"></a>Web API’lerine erişim izinleri ekleme

[Grafik API oturum açma ve okuma kullanıcı profili izni](https://developer.microsoft.com/graph/docs/concepts/permissions_reference#user-permissions) varsayılan olarak seçilir. Her web [API'si](developer-glossary.md#permissions) için iki tür izin arasından seçim yapabilirsiniz:

* **Uygulama izinleri**. İstemci uygulamanızın web API'sine kullanıcı bağlamı olmadan doğrudan kendisi olarak erişmesi gerekir. Bu tür bir izin, yöneticinin onayını gerektirir. Bu izin masaüstü ve mobil istemci uygulamaları için kullanılamaz.
* **Temsilcilik izinleri.** İstemci uygulamanızın oturum açmış kullanıcı olarak web API'sine erişmesi gerekir, ancak erişim seçili izinle sınırlıdır. Yönetici onayı gerektirmediği sürece bu izin türü bir kullanıcı tarafından verilebilir.

  > [!NOTE]
  > Bir uygulamaya temsilcili izin eklenmesi kiracı içindeki kullanıcılara otomatik olarak onay vermez. Yöneticinin tüm kullanıcıların adına onay vermemesi durumunda kullanıcıların yine çalışma zamanında eklenen temsilcili izinler için el ile onay vermesi gerekir.

İstemcinizden kaynak API'lerine erişmek için izin eklemek için:

1. Uygulamaya **Genel Bakış** sayfasından **API izinlerini**seçin.
1. **Yapılandırılmış izinler**altında, **İzin ekle'yi**seçin.
1. Varsayılan olarak, görünüm **Microsoft API’leri** arasından seçim yapmanızı sağlar. İlgilendiğiniz API’leri seçin:

    * **Microsoft API'leri**. Microsoft Graph gibi Microsoft API'leri için izin seçmenize olanak tanır.
    * **Kuruluşumun kullandığı API'ler.** Kuruluşunuzun açığa çıkardığı API'ler veya kuruluşunuzun tümleşik olduğu API'ler için izinleri seçmenize olanak tanır.
    * **Apis'lerim.** Açığa çıkardığınız API'ler için izinleri seçmenizi sağlar.

1. API’leri seçtikten sonra, **API İzinleri İsteme** sayfasını görürsünüz. API hem temsilci hem de uygulama izinlerini kullanıma sunuyorsa, uygulamanız için gereken izin türünü seçin.
1. İşiniz bittiğinde, **İzinleri ekle** seçeneğini belirleyin.

**API izinleri** sayfasına geri dönersiniz. İzinler kaydedildi ve tabloya eklendi.

## <a name="understanding-api-permissions-and-admin-consent-ui"></a>API izinlerini anlama ve yönetici onayı UI

### <a name="configured-permissions"></a>Yapılandırılmış izinler

Bu bölümde, uygulama nesnesi üzerinde açıkça yapılandırılan izinler gösterilmektedir. Bu izinler, uygulamanın gerekli kaynak erişim listesinin bir parçasıdır. İzinleri bu tablodan ekleyebilir veya kaldırabilirsiniz. Yönetici olarak, bir API'nin izinleri veya bireysel izinleri kümesi için yönetici onayı verebilir veya iptal edebilirsiniz.

### <a name="other-permissions-granted"></a>Verilen diğer izinler

Başvurunuz kiracıya kayıtlıysa, Kiracı için verilen **diğer izinler**başlıklı ek bir bölüm görebilirsiniz. Bu bölümde, kiracı için verilen ve uygulama nesnesi üzerinde açıkça yapılandırılmamış izinler gösterilmektedir. Bu izinler dinamik olarak talep edildi ve onaylandı. Bu bölüm yalnızca geçerli olan en az bir izin varsa görüntülenir.

**Yapılandırılmış izinler** bölümüne bu bölümde görünen bir API izinleri kümesi veya tek tek izinler ekleyebilirsiniz. Yönetici olarak, bu bölümdeki tek tek API'ler veya izinler için yönetici onayı da iptal edebilirsiniz.

### <a name="admin-consent-button"></a>Yönetici onayı düğmesi

Başvurunuz kiracıda kayıtlıysa, Kiracı **için Grant yöneticisi onayı** görürsünüz. Yönetici değilseniz veya uygulama için hiçbir izin yapılandırılmamışsa devre dışı bırakılır.
Bu düğme, yöneticinin uygulama için yapılandırılan izinlere yönetici onayı vermesini sağlar. Yönetici onayı düğmesini tıklattığınızda, yapılandırılan tüm izinleri gösteren bir onay istemi içeren yeni bir pencere açılır.

> [!NOTE]
> Uygulama için yapılandırılan izinler ile onay isteminde görünen izinler arasında bir gecikme vardır. Yapılandırılan tüm izinleri onay isteminde görmüyorsanız, kapatın ve yeniden başlatın.

Verilmiş ancak yapılandırılmamış izinleriniz varsa, yönetici onayı düğmesi bu izinleri işlemenizi ister. Bunları yapılandırılmış izinlere ekleyebilir veya kaldırabilirsiniz.

Onay istemi **kabul** etme veya **iptal**etme seçeneği sağlar. Yönetici onayı vermek için **Kabul et'i** seçin. **İptal'i**seçerseniz, yönetici onayı verilmez. Bir hata iletisi, onay reddedildi devletler.

> [!NOTE]
> Onay isteminde **Kabul'ü** seçerek yönetici onayı vermekle portala yansıtılan yönetici onayı durumu arasında bir gecikme vardır.

## <a name="next-steps"></a>Sonraki adımlar

Web API'lerini nasıl açığa çıkarılabildiğini öğrenmek için bir sonraki makaleye ilerleyin.
> [!div class="nextstepaction"]
> [Quickstart: Web API'lerini ortaya çıkarmak için bir uygulamayı yapılandırma](quickstart-configure-app-expose-web-apis.md)

* Kayıtlı uygulamayı temsil eden iki Azure AD nesnesi ve aralarındaki ilişki hakkında daha fazla bilgi edinmek için bkz. [Uygulama nesneleri ve hizmet sorumlusu nesneleri](app-objects-and-service-principals.md).

* Azure Active Directory ile uygulama geliştirirken kullanmanız gereken markalama yönergeleri hakkında daha fazla bilgi edinmek için bkz. [Uygulamalar için markalama yönergeleri](howto-add-branding-in-azure-ad-apps.md).

* [Quickstart: Microsoft kimlik platformuile bir uygulama kaydetme](quickstart-register-app.md)

* [Hızlı başlatma: Bir uygulama tarafından desteklenen hesapları değiştirme](quickstart-modify-supported-accounts.md)

* [Quickstart: Microsoft kimlik platformuna kayıtlı bir uygulamayı kaldırma](quickstart-remove-app.md)
