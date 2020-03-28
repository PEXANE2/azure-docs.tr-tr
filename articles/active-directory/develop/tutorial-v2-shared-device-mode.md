---
title: MSAL Android ile paylaşılan cihaz modunu kullanma | Azure
description: Paylaşılan modda çalışacak ve bir firstline worker uygulamasını çalıştıracak bir Android cihazını nasıl hazırlayacağınızı öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 1/15/2020
ms.author: hahamil
ms.reviewer: brandwe
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: bf7e6bb22ce89d6be3f79efad1f1a3679e8780e7
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77086066"
---
# <a name="tutorial-use-shared-device-mode-in-your-android-application"></a>Öğretici: Android uygulamanızda paylaşılan cihaz modunu kullanma

> [!NOTE]
> Bu özellik genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="developer-guide"></a>Geliştirici kılavuzu

Bu kılavuz, Microsoft Kimlik Doğrulama Kitaplığı (MSAL) kullanarak bir Android uygulamasında paylaşılan aygıt modunu uygulamak için geliştirici kılavuzu sağlar. MSAL'ı Android uygulamanızla nasıl entegre edinacağınızı, bir kullanıcıda oturum açmayı, Microsoft grafiğini nasıl arayacağınızı ve bir kullanıcıyı nasıl oturum unuzu tamamlayacağınızı görmek için [MSAL Android öğreticisine](https://docs.microsoft.com/azure/active-directory/develop/tutorial-v2-android) bakın.

### <a name="download-the-sample"></a>Örneği indirme

Örnek [uygulamayı](https://github.com/Azure-Samples/ms-identity-android-java/) GitHub'dan klonla. Örnek, [tek veya çok hesap modunda](https://docs.microsoft.com/azure/active-directory/develop/single-multi-account)çalışma özelliğine sahiptir.

### <a name="add-the-msal-sdk-to-your-local-maven-repository"></a>MSAL SDK'yı yerel Maven deponuza ekleyin

Örnek uygulamayı kullanmıyorsanız, msal kitaplığını build.gradle dosyanızda bir bağımlılık olarak ekleyin:

```gradle
dependencies{
  implementation 'com.microsoft.identity.client.msal:1.0.+'
}
```

### <a name="configure-your-app-to-use-shared-device-mode"></a>Uygulamanızı paylaşılan aygıt modunu kullanacak şekilde yapılandırın

Config dosyanızı ayarlama hakkında daha fazla bilgi için [yapılandırma belgelerine](https://docs.microsoft.com/azure/active-directory/develop/msal-configuration) bakın.

MSAL `true` yapılandırma dosyanızda ayarlanın. `"shared_device_mode_supported"`

Birden çok hesap modunu desteklemeyi planlayamayabilirsiniz. Paylaşılan bir aygıtı kullanmıyorsanız ve kullanıcı uygulamada aynı anda birden fazla hesapla oturum açabilirse bu olabilir. Eğer öyleyse, `"account_mode"` `"SINGLE"`ayarlayın. Bu, uygulamanızın her zaman `ISingleAccountPublicClientApplication`alacağını garanti eder ve MSAL entegrasyonunuzu önemli ölçüde kolaylaştırır. Varsayılan `"account_mode"` `"MULTIPLE"`değer, bu nedenle mod kullanıyorsanız `"single account"` config dosyasında bu değeri değiştirmek önemlidir.

Örnek uygulamanın **ana**>**main**>**res**>**ham** dizininde yer alan auth_config.json dosyasının bir örneği aşağıda verilmiştir:

```json
{
 "client_id":"Client ID after app registration at https://aka.ms/MobileAppReg",
 "authorization_user_agent":"DEFAULT",
 "redirect_uri":"Redirect URI after app registration at https://aka.ms/MobileAppReg",
 "account_mode":"SINGLE",
 "broker_redirect_uri_registered": true,
 "shared_device_mode_supported": true,
 "authorities":[
  {
   "type":"AAD",
   "audience":{
     "type": "AzureADandPersonalMicrosoftAccount",
     "tenant_id":"common"
   }
  }
 ]
}
```

### <a name="detect-shared-device-mode"></a>Paylaşılan aygıt modunu algılama

Paylaşılan aygıt modu, Android aygıtları birden çok çalışan tarafından paylaşılacak şekilde yapılandırmanıza olanak sağlarken, aygıtın Microsoft Identity destekli yönetimini de sağlar. Çalışanlar cihazlarında oturum açabilir ve müşteri bilgilerine hızlı bir şekilde erişebilir. Vardiyaları veya görevleri tamamlandığında, paylaşılan cihazdaki tüm uygulamaları tek bir tıklamayla oturumdan çıkarabilecekler ve cihaz bir sonraki çalışanın kullanımına hemen hazır olacak.

Bir `isSharedDevice()` uygulamanın paylaşılan aygıt modunda olan bir aygıtta mı çalıştığını belirlemek için kullanın. Uygulamanız, UX'yi buna göre değiştirip değiştirmemesi gerektiğini belirlemek için bu bayrağı kullanabilir.

Burada nasıl kullanabileceğinizi `isSharedDevice()`gösteren bir kod parçacığı.  Örnek uygulamadaki `SingleAccountModeFragment` sınıftan:

```Java
deviceModeTextView.setText(mSingleAccountApp.isSharedDevice() ?"Shared" :"Non-Shared");
```

### <a name="initialize-the-publicclientapplication-object"></a>PublicClientApplication nesnesini başlatma

MSAL `"account_mode":"SINGLE"` config dosyasında ayarlarsanız, döndürülen uygulama nesnesini güvenli `ISingleAccountPublicCLientApplication`bir şekilde '' olarak atabilirsiniz.

```java
private ISingleAccountPublicClientApplication mSingleAccountApp;

/*Configure your sample app and save state for this activity*/ 
PublicClientApplication.create(this.getApplicationCOntext(),
  R.raw.auth_config, 
  new PublicClientApplication.ApplicationCreatedListener(){
  @Override
  public void onCreated(IPublicClientApplication application){
  mSingleAccountApp = (ISingleAccountPublicClientApplication)application;
  loadAccount();
  }
  @Override
  public void onError(MsalException exception{
  /*Fail to initialize PublicClientApplication */
  }
});  
```

### <a name="detect-single-vs-multiple-account-mode"></a>Tek ve çoklu hesap modunu algılama

Paylaşılan bir cihazda yalnızca firstline çalışanları için kullanılacak bir uygulama yazıyorsanız, uygulamanızı yalnızca tek hesap lı modu desteklemek için yazmanızı öneririz. Buna, tıbbi kayıt uygulamaları, fatura uygulamaları ve çoğu iş yeri uygulaması gibi görev odaklı uygulamaların çoğu dahildir. SDK'nın birçok özelliğinin barındırılması gerekmeyeceğinden, bu durum gelişmenizi kolaylaştıracaktır.

Uygulamanız birden fazla hesabı ve paylaşılan aygıt modunu destekliyorsa, aşağıda gösterildiği gibi bir tür denetimi gerçekleştirmeniz ve uygun arabirime döküm uygulamanız gerekir.

```java
private IPublicClientApplication mApplication;

        if (mApplication instanceOf IMultipleAccountPublicClientApplication) {
          IMultipleAccountPublicClientApplication multipleAccountApplication = (IMultipleAccountPublicClientApplication) mApplication;
          ...
        } else if (mApplication instanceOf    ISingleAccountPublicClientApplication) {
           ISingleAccountPublicClientApplication singleAccountApplication = (ISingleAccountPublicClientApplication) mApplication;
            ...
        }
```

### <a name="get-the-signed-in-user-and-determine-if-a-user-has-changed-on-the-device"></a>Oturum açmış kullanıcıyı alın ve cihazda bir kullanıcının değişip değişmediğini belirleyin

Yöntem, `loadAccount` oturum açmış kullanıcının hesabını alır. Yöntem, `onAccountChanged` oturum açmış kullanıcının değişip değişmediğini belirler ve eğer öyleyse, temizleyin:

```java 
private void loadAccount()
{
  mSingleAccountApp.getCurrentAccountAsync(new ISingleAccountPublicClientApplication.CurrentAccountCallback()
  {
    @Overide
    public void onAccountLoaded(@Nullable IAccount activeAccount)
    {
      if (activeAccount != null)
      {
        signedInUser = activeAccount;
        mSingleAccountApp.acquireTokenSilentAsync(SCOPES,"http://login.microsoftonline.com/common",getAuthSilentCallback());
      }
    }
    @Override
    public void on AccountChanged(@Nullable IAccount priorAccount, @Nullable Iaccount currentAccount)
    {
      if (currentAccount == null)
      {
        //Perform a cleanup task as the signed-in account changed.
        updateSingedOutUI();
      }
    }
    @Override 
    public void onError(@NonNull Exception exception) 
    {
    }
  }
}  
```

### <a name="globally-sign-in-a-user"></a>Genel olarak oturum aç

Aygıt genelinde ki bir kullanıcıda, Authenticator Uygulaması yla MSAL kullanan diğer uygulamalara aşağıdaki işaretler:

```java
private void onSignInClicked()
{
  mSingleAccountApp.signIn(getActivity(), SCOPES, null, getAuthInteractiveCallback());
}
```

### <a name="globally-sign-out-a-user"></a>Genel olarak bir kullanıcıyı oturum aç

Aşağıda, oturum açmış hesap kaldırılır ve önbelleğe alınmış belirteçleri yalnızca uygulamadan değil, paylaşılan aygıt modundabulunan cihazdan da temizler:

```java
private void onSignOutClicked()
{
  mSingleAccountApp.signOut(new ISingleAccountPublicClientApplication.SignOutCallback()
  {
    @Override
    public void onSignOut()
    {
      updateSignedOutUI();
    }
    @Override
    public void onError(@NonNull MsalException exception)
    {
      /*failed to remove account with an exception*/
    }
  });
}
```

## <a name="administrator-guide"></a>Yönetici kılavuzu

Aşağıdaki adımlar, uygulamanızı Azure portalında ayarlamayı ve cihazınızı paylaşılan aygıt moduna sokmayı açıklar.

### <a name="register-your-application-in-azure-active-directory"></a>Uygulamanızı Azure Active Directory'ye kaydedin

İlk olarak, başvurunuzu kuruluş kiracınıza kaydedin. Daha sonra uygulamanızın düzgün çalışması için aşağıdaki değerleri auth_config.json olarak sağlayın.

Bunun nasıl yapılacılaştırılacıyla ilgili bilgi için [başvurunuzu kaydedin.](https://docs.microsoft.com/azure/active-directory/develop/tutorial-v2-android#register-your-application)

> [!NOTE]
> Uygulamanızı kaydettirirken, lütfen sol taraftaki hızlı başlangıç kılavuzunu kullanın ve ardından **Android'i**seçin. Bu, uygulamanız için **Paket Adı** ve **İmza Hash'ı** sağlamanızın isteneceği bir sayfaya yönlendirecektir. Bunlar, uygulama yapılandırmanızın işe yarayacağından emin olmak için çok önemlidir. Daha sonra, uygulamanız için kullanabileceğiniz ve auth_config.json dosyanıza kesip yapıştıracağınız bir yapılandırma nesnesi alırsınız.

![Uygulama kayıt](media/tutorial-v2-shared-device-mode/register-app.png) ekranı **Bu değişikliği benim için yapın'ı** seçmeli ve ardından Azure portalında hızlı başlatmanın istediği değerleri sağlamalısınız. Bu iş bittiğinde, ihtiyacınız olan tüm yapılandırma dosyalarını oluştururuz.

![App config bilgi ekranı](media/tutorial-v2-shared-device-mode/config-info.png)

## <a name="set-up-a-tenant"></a>Kiracı ayarlama

Sınama amacıyla, kiracınızda aşağıdakileri ayarlayın: en az iki çalışan, bir Bulut Aygıt Yöneticisi ve bir Global Administrator. Azure portalında, Kurumsal Rolleri değiştirerek Bulut Aygıt Yöneticisi'ni ayarlayın. Azure portalında, **Azure Etkin Dizin** > Rolleri**ve Yöneticiler** > Bulut**Aygıt Yöneticisi'ni**seçerek Kuruluş Rollerinize erişin. Aygıtı paylaşılan moda koyabilecek kullanıcıları ekleyin.

## <a name="set-up-an-android-device-in-shared-mode"></a>Paylaşılan modda bir Android cihazı ayarlama

### <a name="download-the-authenticator-app"></a>Kimlik Doğrulayıcı Uygulamasını İndirin

Microsoft Authenticator Uygulamasını Google Play mağazasından indirin. Uygulamayı zaten indirdiyseniz, uygulamanın en son sürüm olduğundan emin olun.

### <a name="authenticator-app-settings--registering-the-device-in-the-cloud"></a>Kimlik doğrulayıcı uygulama ayarları & aygıtı buluta kaydettirme

Kimlik Doğrulayıcı Uygulamasını başlatın ve ana hesap sayfasına gidin. **Hesap Ekle** sayfasını gördüğünüzde, aygıtın paylaşılamasını yapmaya hazırsınız.

![Kimlik doğrulayıcı hesap ekranı eklemek](media/tutorial-v2-shared-device-mode/authenticator-add-account.png)

 Sağ menü çubuğunu kullanarak **Ayarlar** bölmesine gidin. İş & **Okul hesapları**altında Cihaz **Kaydı'nı** seçin.
 
 ![Kimlik doğrulayıcı hesap ekranı eklemek](media/tutorial-v2-shared-device-mode/authenticator-settings.png)

 Bu düğmeyi tıklattığınızda, aygıt kişilerine erişim yetkisi vermeniz istenir. Bunun nedeni Android'in cihazdaki hesap tümleştirmesidir. **İzin ver'i**seçin.

 ![Kimlik doğrulayıcı hesap ekranı eklemek](media/tutorial-v2-shared-device-mode/authenticator-allow-screen.png)

Bulut Aygıt Yöneticisi, kuruluş e-postalarını altında girmeli **veya paylaşılan bir aygıt olarak kaydolmalıdır.** Ardından **paylaşılan aygıt olarak kaydol** düğmesini tıklatın ve kimlik bilgilerini girin.

![kayıt cihazı ekranı](media/tutorial-v2-shared-device-mode/register-device.png)

![sign-in](media/tutorial-v2-shared-device-mode/sign-in.png)

Aygıt artık paylaşılan modda.

![kayıt cihazı ekranı](media/tutorial-v2-shared-device-mode/shared-device-mode-screen.png)

 Cihazdaki tüm oturum açma ve oturum açmalar genel olacaktır, bu da cihazdaki MSAL ve Microsoft Authenticator ile entegre edilmiş tüm uygulamalar için geçerli olduğu anlamına gelir. Artık uygulamaları paylaşılan aygıt modu özelliklerini kullanan aygıta dağıtabilirsiniz.

## <a name="view-the-shared-device-in-the-azure-portal"></a>Azure portalında paylaşılan aygıtı görüntüleme

Bir aygıtı paylaşılan moda koyduktan sonra, aygıtınız tarafından bilinir ve kuruluş kiracınızda izlenir. Azure portalınızın Azure Etkin Dizin bıçak'ındaki **Birleştirme Türü'ne** bakarak paylaşılan cihazlarınızı görüntüleyebilirsiniz.

![Azure portalındaki tüm aygıtlar bıçak](media/tutorial-v2-shared-device-mode/registered-device-screen.png)

## <a name="running-the-sample-app"></a>Örnek uygulamayı çalıştırma

Örnek Uygulama, kuruluşunuzun Grafik API'sini arayacak basit bir uygulamadır. n ilk çalıştırma, uygulama çalışan hesabınıza yeni olduğu için onay vermeniz istenir.

![App config bilgi ekranı](media/tutorial-v2-shared-device-mode/run-app-permissions-requested.png)

## <a name="next-steps"></a>Sonraki adımlar

Android cihazlar için [Paylaşılan aygıt modunda](shared-device-mode.md) paylaşılan mod hakkında daha fazla bilgi edinin