---
title: Tek ve birden fazla hesap kamu istemcisi uygulamaları | Azure
description: Tek ve birden çok hesaplı genel istemci uygulamalarına genel bakış.
services: active-directory
author: shoatman
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/26/2019
ms.author: shoatman
ms.custom: aaddev
ms.reviewer: shoatman
ms.openlocfilehash: 89a383aabf3487a0938604bc28ddb06c0541d13e
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80881359"
---
# <a name="single-and-multiple-account-public-client-apps"></a>Tek ve birden çok hesaplı kamu istemcisi uygulamaları

Bu makale, tek hesaplı ortak istemci uygulamalarında kullanılan türleri anlamanıza yardımcı olur ve tek bir hesap ortak istemci uygulamalarına odaklanır. 

Azure Etkin Dizin Kimlik Doğrulama Kitaplığı (ADAL) sunucuyu modeller.  Microsoft Kimlik Doğrulama Kitaplığı (MSAL) bunun yerine istemci uygulamanızı modeller.  Android uygulamalarının çoğu genel istemci olarak kabul edilir. Ortak istemci, güvenli bir şekilde sır tutabilen bir uygulamadır.  

MSAL, aynı `PublicClientApplication` anda yalnızca bir hesabın kullanılmasına izin veren uygulamaların geliştirme deneyimini basitleştirmek ve netleştirmek için API yüzeyini uzmanleştirir. `PublicClientApplication`tarafından alt `SingleAccountPublicClientApplication` sınıflanır `MultipleAccountPublicClientApplication`ve .  Aşağıdaki diyagram, bu sınıflar arasındaki ilişkiyi gösterir.

![SingleAccountPublicClientApplication UML Sınıf Diyagramı](./media/single-multi-account/single-and-multiple-account.png)

## <a name="single-account-public-client-application"></a>Tek hesaplı kamu istemcisi uygulaması

Sınıf, `SingleAccountPublicClientApplication` aynı anda yalnızca tek bir hesabın oturum açmasına izin veren MSAL tabanlı bir uygulama oluşturmanıza olanak tanır. `SingleAccountPublicClientApplication`aşağıdaki şekillerden `PublicClientApplication` farklıdır:

- MSAL, şu anda imzalanan hesabı izler.
  - Uygulamanız bir broker kullanıyorsa (Azure portal uygulama kaydı sırasında varsayılan) ve aracının bulunduğu bir cihaza yüklüyse, MSAL hesabın cihazda hala kullanılabilir olduğunu doğrular.
- `signIn`bir hesapta kapsam istemekten açık ve ayrı olarak oturum açmanızı sağlar.
- `acquireTokenSilent`hesap parametresi gerektirmez.  Bir hesap sağlarsanız ve sağladığınız hesap MSAL tarafından izlenen cari hesapla `MsalClientException` eşleşmiyorsa, bir hesap atılır.
- `acquireToken`kullanıcının hesap değiştirmesine izin vermez. Kullanıcı farklı bir hesaba geçmeye çalışırsa, bir özel durum atılır.
- `getCurrentAccount`aşağıdakileri sağlayan bir sonuç nesnesi döndürür:
  - Hesabın değişip değişmediğini gösteren bir boolean. Örneğin, aygıttan kaldırılması sonucunda bir hesap değiştirilebilir.
  - Önceki hesap. Bu, hesap aygıttan kaldırıldığında veya yeni bir hesap oturum açıldığında yerel veri temizleme yapmanız gerekiyorsa yararlıdır.
  - Cari Hesap.
- `signOut`istemcinizle ilişkili belirteçleri aygıttan kaldırır.  

Cihaza Microsoft Authenticator veya Intune Company Portal gibi bir Android Kimlik Doğrulama aracısı yüklendiğinde `signOut` ve uygulamanız aracıyı kullanacak şekilde yapılandırıldığınızda, hesabı cihazdan kaldırmaz.

## <a name="single-account-scenario"></a>Tek hesap senaryosu

Aşağıdaki sözde kod kullanarak `SingleAccountPublicClientApplication`göstermektedir.

```java
// Construct Single Account Public Client Application
ISingleAccountPublicClientApplication app = PublicClientApplication.createSingleAccountPublicClientApplication(getApplicationContext(), R.raw.msal_config);

String[] scopes = {"User.Read"};
IAccount mAccount = null;

// Acquire a token interactively
// The user will get a UI prompt before getting the token.
app.signIn(getActivity(), scopes, new AuthenticationCallback()
{

        @Override
        public void onSuccess(IAuthenticationResult authenticationResult) 
        {
            mAccount = authenticationResult.getAccount();
        }

        @Override
        public void onError(MsalException exception)
        {
        }

        @Override
        public void onCancel()
        {
        }
    }
);

// Load Account Specific Data
getDataForAccount(account);

// Get Current Account
ICurrentAccountResult currentAccountResult = app.getCurrentAccount();
if (currentAccountResult.didAccountChange())
{
    // Account Changed Clear existing account data
    clearDataForAccount(currentAccountResult.getPriorAccount());
    mAccount = currentAccountResult.getCurrentAccount();
    if (account != null)
    {
        //load data for new account
        getDataForAccount(account);
    }
}

// Sign out
if (app.signOut())
{
    clearDataForAccount(mAccount);
    mAccount = null;
}
```

## <a name="multiple-account-public-client-application"></a>Birden fazla hesap kamu istemcisi uygulaması

Sınıf, `MultipleAccountPublicClientApplication` birden çok hesabın aynı anda oturum açmış olmasını sağlayan MSAL tabanlı uygulamalar oluşturmak için kullanılır. Aşağıdaki gibi hesap almanızı, eklemenizi ve kaldırmanızı sağlar:

### <a name="add-an-account"></a>Hesap ekleme

Bir veya daha fazla kez `acquireToken` arayarak uygulamanızda bir veya daha fazla hesap kullanın.  

### <a name="get-accounts"></a>Hesap alma

- Belirli `getAccount` bir hesap almak için arayın.
- Şu `getAccounts`anda uygulama tarafından bilinen hesapların listesini almak için arayın.

Uygulamanız, broker uygulaması tarafından bilinen cihazdaki tüm Microsoft kimlik platformu hesaplarını sıralayamaz. Yalnızca uygulamanız tarafından kullanılan hesapları sayabilir.  Aygıttan kaldırılan hesaplar bu işlevler tarafından döndürülmez.

### <a name="remove-an-account"></a>Hesabı kaldırma

Hesap tanımlayıcısıyla `removeAccount` arayarak hesabı kaldırın.

Uygulamanız bir broker kullanacak şekilde yapılandırılırsa ve cihaza bir broker yüklenirse, aracıyı `removeAccount`aradığınızda hesap brokerdan kaldırılmaz.  Yalnızca istemcinizle ilişkili belirteçler kaldırılır.

## <a name="multiple-account-scenario"></a>Birden çok hesap senaryosu

Aşağıdaki sözde kod, birden çok hesap uygulamasının nasıl oluşturulup, aygıttaki hesapları nasıl listelenebildiğini ve jeton elde edilebildiğini gösterir.

```java
// Construct Multiple Account Public Client Application
IMultipleAccountPublicClientApplication app = PublicClientApplication.createMultipleAccountPublicClientApplication(getApplicationContext(), R.raw.msal_config);

String[] scopes = {"User.Read"};
IAccount mAccount = null;

// Acquire a token interactively
// The user will be required to interact with a UI to obtain a token
app.acquireToken(getActivity(), scopes, new AuthenticationCallback()
 {

        @Override
        public void onSuccess(IAuthenticationResult authenticationResult) 
        {
            mAccount = authenticationResult.getAccount();
        }

        @Override
        public void onError(MsalException exception)
        {
        }

        @Override
        public void onCancel()
        {
        }
 });

...

// Get the default authority
String authority = app.getConfiguration().getDefaultAuthority().getAuthorityURL().toString();

// Get a list of accounts on the device
List<IAccount> accounts = app.getAccounts();

// Pick an account to obtain a token from without prompting the user to sign in
IAccount selectedAccount = accounts.get(0);

// Get a token without prompting the user
app.acquireTokenSilentAsync(scopes, selectedAccount, authority, new SilentAuthenticationCallback()
{

        @Override
        public void onSuccess(IAuthenticationResult authenticationResult) 
        {
            mAccount = authenticationResult.getAccount();
        }

        @Override
        public void onError(MsalException exception)
        {
        }
});
```
