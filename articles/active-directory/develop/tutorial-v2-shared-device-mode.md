---
title: MSAL Android ile paylaşılan cihaz modunu kullanma | Mavisi
description: Bir Android cihazını paylaşılan modda çalışacak şekilde hazırlama ve Firstline çalışan uygulamasını çalıştırma hakkında bilgi edinin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: tutorial
ms.workload: identity
ms.date: 1/15/2020
ms.author: hahamil
ms.reviewer: brandwe
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: b2f74d2d441007f195abd38ca26ca7fa73605318
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80886441"
---
# <a name="tutorial-use-shared-device-mode-in-your-android-application"></a>Öğretici: Android uygulamanızda paylaşılan cihaz modunu kullanma

> [!NOTE]
> Bu özellik genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="developer-guide"></a>Geliştirici kılavuzu

Bu kılavuz, Microsoft kimlik doğrulama kitaplığı 'nı (MSAL) kullanarak bir Android uygulamasında paylaşılan cihaz modunu uygulamaya yönelik Geliştirici Kılavuzu sağlar. MSAL Android uygulamanızla tümleştirme, bir Kullanıcı oturum açma, Microsoft Graph 'i çağırma ve bir kullanıcının oturumunu kapatma hakkında bilgi edinmek için [msal Android öğreticisine](https://docs.microsoft.com/azure/active-directory/develop/tutorial-v2-android) bakın.

### <a name="download-the-sample"></a>Örneği indirme

[Örnek uygulamayı](https://github.com/Azure-Samples/ms-identity-android-java/) GitHub ' dan kopyalayın. Örnek, [tek veya çoklu hesap modunda](https://docs.microsoft.com/azure/active-directory/develop/single-multi-account)çalışma özelliğine sahiptir.

### <a name="add-the-msal-sdk-to-your-local-maven-repository"></a>MSAL SDK 'sını yerel Maven deponuza ekleyin

Örnek uygulamayı kullanmıyorsanız, MSAL kitaplığını Build. Gradle dosyanıza bağımlılık olarak ekleyin, örneğin:

```gradle
dependencies{
  implementation 'com.microsoft.identity.client.msal:1.0.+'
}
```

### <a name="configure-your-app-to-use-shared-device-mode"></a>Uygulamanızı paylaşılan cihaz modunu kullanacak şekilde yapılandırma

Yapılandırma dosyanızı ayarlama hakkında daha fazla bilgi için [yapılandırma belgelerine](https://docs.microsoft.com/azure/active-directory/develop/msal-configuration) bakın.

MSAL `"shared_device_mode_supported"` yapılandırma `true` dosyanızda olarak ayarlayın.

Birden çok hesap modunu desteklemeyi planlamayabilir. Bu, paylaşılan bir cihaz kullanmıyorsanız ve Kullanıcı uygulamada aynı anda birden fazla hesapla oturum açabilir. Öyleyse, olarak `"SINGLE"`ayarlayın `"account_mode"` . Bu, uygulamanızı her zaman alır `ISingleAccountPublicClientApplication`ve msal tümleştirmenizi önemli ölçüde basitleştirir. Varsayılan değeri `"account_mode"` olduğundan `"MULTIPLE"`, mod kullanıyorsanız `"single account"` bu değerin yapılandırma dosyasında değiştirilmesi önemlidir.

Örnek uygulamanın **uygulama**>**ana**>**res**>**Ham** dizinine eklenen auth_config. JSON dosyasına bir örnek aşağıda verilmiştir:

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

### <a name="detect-shared-device-mode"></a>Paylaşılan cihaz modunu Algıla

Paylaşılan cihaz modu, Android cihazları birden çok çalışan tarafından paylaşılmak üzere yapılandırmanıza olanak sağlarken, Microsoft kimliği tarafından desteklenen cihaz yönetimini de sağlar. Çalışanlar cihazlarında oturum açabilir ve müşteri bilgilerine hızlıca erişebilir. Bunlar, vardiyası veya göreviyle bitdiklerinde, tek tıklamayla paylaşılan cihazdaki tüm uygulamaların oturumunu açabiliyor ve cihaz bir sonraki çalışanın kullanımına hemen hazır olacaktır.

Paylaşılan `isSharedDevice()` cihaz modundaki bir cihazda bir uygulamanın çalışıp çalışmadığını anlamak için kullanın. Uygulamanız, UX 'e göre değişiklik gerekip gerekmediğini öğrenmek için bu bayrağı kullanabilir.

İşte nasıl kullanabileceğinizi `isSharedDevice()`gösteren bir kod parçacığı.  Örnek uygulamadaki `SingleAccountModeFragment` sınıfından olur:

```Java
deviceModeTextView.setText(mSingleAccountApp.isSharedDevice() ?"Shared" :"Non-Shared");
```

### <a name="initialize-the-publicclientapplication-object"></a>PublicClientApplication nesnesini başlatma

MSAL yapılandırma dosyasında `"account_mode":"SINGLE"` ayarlarsanız, döndürülen uygulama nesnesini güvenli bir `ISingleAccountPublicCLientApplication`şekilde olarak çevirebilirsiniz.

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

### <a name="detect-single-vs-multiple-account-mode"></a>Tek ve birden çok hesap modunu Algıla

Paylaşılan bir cihazda yalnızca Firstline çalışanları için kullanılacak bir uygulama yazıyorsanız, uygulamanızı yalnızca tek hesap modunu destekleyecek şekilde yazmanızı öneririz. Bu, tıp kayıtları uygulamaları, fatura uygulamaları ve çoğu iş kolu uygulaması gibi görev odaklı birçok uygulamayı içerir. Bu, SDK 'nın birçok özelliğinin konamesi gerekmeyeceği için geliştirmeyi basitleştirir.

Uygulamanız birden çok hesabı ve paylaşılan cihaz modunu destekliyorsa, bir tür denetimi gerçekleştirmeniz ve aşağıda gösterildiği gibi uygun arabirime dönüştürmeniz gerekir.

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

### <a name="get-the-signed-in-user-and-determine-if-a-user-has-changed-on-the-device"></a>Oturum açan kullanıcıyı al ve cihazda bir kullanıcının değişip değişmediğini belirleme

Yöntemi `loadAccount` , oturum açmış kullanıcının hesabını alır. `onAccountChanged` Yöntemi, oturum açmış kullanıcının değişip değişmediğini belirler ve gerekiyorsa temizler:

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

### <a name="globally-sign-in-a-user"></a>Bir kullanıcı için genel olarak oturum açma

Aşağıdaki, bir kullanıcının, kimlik doğrulayıcı uygulamasıyla MSAL kullanan diğer uygulamalara, cihaz genelinde oturum açtığı:

```java
private void onSignInClicked()
{
  mSingleAccountApp.signIn(getActivity(), SCOPES, null, getAuthInteractiveCallback());
}
```

### <a name="globally-sign-out-a-user"></a>Kullanıcı oturumunu küresel olarak kapatma

Aşağıdakiler, oturum açmış olan hesabı kaldırır ve önbelleğe alınmış belirteçleri yalnızca uygulamadan değil, aynı zamanda paylaşılan cihaz modundaki cihazdan temizler:

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

Aşağıdaki adımlarda Azure portal uygulamanızı ayarlama ve cihazınızı paylaşılan-cihaz moduna geçirme açıklanır.

### <a name="register-your-application-in-azure-active-directory"></a>Uygulamanızı Azure Active Directory kaydetme

İlk olarak, uygulamanızı kuruluş kiracınıza kaydedin. Daha sonra uygulamanızın doğru çalışması için auth_config. json ' de aşağıdaki değerleri sağlayın.

Bunun nasıl yapılacağı hakkında daha fazla bilgi için, bkz. [Uygulamanızı kaydetme](https://docs.microsoft.com/azure/active-directory/develop/tutorial-v2-android#register-your-application).

> [!NOTE]
> Uygulamanızı kaydettiğinizde lütfen sol taraftaki hızlı başlangıç kılavuzunu kullanın ve ardından **Android**' i seçin. Bu, size uygulamanız için **paket adı** ve **imza karması** sağlamak istediğiniz bir sayfaya yol açacaktır. Bunlar, uygulama yapılandırmanızın çalışmasını sağlamak için çok önemlidir. Ardından, uygulamanız için kullanabileceğiniz ve auth_config. JSON dosyanıza yapıştırdığınız bir yapılandırma nesnesi alacaksınız.

![Uygulama kaydı ekranı](media/tutorial-v2-shared-device-mode/register-app.png) **Bu değişikliği bana yap** ' ı seçmeniz ve ardından hızlı başlangıç için Azure Portal için gereken değerleri sağlamanız gerekir. Bu işlem tamamlandığında, ihtiyacınız olan tüm yapılandırma dosyalarını oluşturacağız.

![Uygulama yapılandırma bilgisi ekranı](media/tutorial-v2-shared-device-mode/config-info.png)

## <a name="set-up-a-tenant"></a>Kiracı ayarlama

Sınama amacıyla kiracınızda şunları ayarlayın: en az iki çalışan, bir bulut cihaz yöneticisi ve bir genel yönetici. Azure portal, kuruluş rollerini değiştirerek bulut cihazı yöneticisini ayarlayın. Azure Portal, **Azure Active Directory** > **Roller ve yöneticiler** > **bulut Cihaz Yöneticisi**' ni seçerek kurumsal rollerinizi erişin. Paylaşılan moda bir cihaz koyabileceğiniz kullanıcıları ekleyin.

## <a name="set-up-an-android-device-in-shared-mode"></a>Paylaşılan modda bir Android cihazı ayarlama

### <a name="download-the-authenticator-app"></a>Authenticator uygulamasını indirin

Google Play mağazasından Microsoft Authenticator uygulamasını indirin. Uygulamayı zaten yüklediyseniz, en son sürüm olduğundan emin olun.

### <a name="authenticator-app-settings--registering-the-device-in-the-cloud"></a>Cihaza kimlik doğrulayıcı uygulama ayarları & cihazı buluta kaydetme

Authenticator uygulamasını başlatın ve ana hesap sayfasına gidin. **Hesap Ekle** sayfasını gördüğünüzde, cihazı paylaşılan duruma getirmek için hazırsınız demektir.

![Kimlik doğrulayıcı hesap ekleme ekranı](media/tutorial-v2-shared-device-mode/authenticator-add-account.png)

 Sağ menü çubuğunu kullanarak **Ayarlar** bölmesine gidin. **İş & okul hesapları**altında **cihaz kaydı** ' nı seçin.

 ![Kimlik doğrulayıcı hesap ekleme ekranı](media/tutorial-v2-shared-device-mode/authenticator-settings.png)

 Bu düğmeye tıkladığınızda, cihaz kişilerine erişim yetkisi vermeniz istenir. Bunun nedeni, Android 'in cihazdaki hesap Tümleştirmesidir. **İzin ver**' i seçin.

 ![Kimlik doğrulayıcı hesap ekleme ekranı](media/tutorial-v2-shared-device-mode/authenticator-allow-screen.png)

Bulut Cihaz Yöneticisi, kurumsal e-postalarını, **paylaşılan bir cihaz olarak girin veya kaydeder**. Ardından **paylaşılan cihaz olarak kaydet** düğmesine tıklayın ve kimlik bilgilerini girin.

![kayıt-cihaz ekranı](media/tutorial-v2-shared-device-mode/register-device.png)

![sign-in](media/tutorial-v2-shared-device-mode/sign-in.png)

Cihaz artık paylaşılan modda.

![kayıt-cihaz ekranı](media/tutorial-v2-shared-device-mode/shared-device-mode-screen.png)

 Cihazdaki tüm oturum açma işlemleri ve oturum açma işlemleri genel olacaktır, yani MSAL ve cihazdaki Microsoft Authenticator tümleştirilmiş tüm uygulamalar için geçerlidir. Artık, paylaşılan cihaz modu özelliklerini kullanan cihaza uygulamalar dağıtabilirsiniz.

## <a name="view-the-shared-device-in-the-azure-portal"></a>Azure portal paylaşılan cihazı görüntüleme

Paylaşılan moda bir cihaz yerleştirdikten sonra, kuruluşunuz tarafından bilinir ve kuruluş kiracınızda izlenir. Paylaşılan cihazlarınızı, Azure portal Azure Active Directory dikey penceresinde bulunan **JOIN türüne** bakarak görüntüleyebilirsiniz.

![Azure portal tüm cihazlar dikey penceresi](media/tutorial-v2-shared-device-mode/registered-device-screen.png)

## <a name="running-the-sample-app"></a>Örnek uygulamayı çalıştırma

Örnek uygulama, kuruluşunuzun Graph API çağıramayacak basit bir uygulamadır. İlk çalıştırmada, uygulama çalışan hesabınıza yeni olduğu için onay istenir.

![Uygulama yapılandırma bilgisi ekranı](media/tutorial-v2-shared-device-mode/run-app-permissions-requested.png)

## <a name="next-steps"></a>Sonraki adımlar

[Android cihazlar Için paylaşılan cihaz modunda](msal-android-shared-devices.md) paylaşılan mod hakkında daha fazla bilgi edinin