---
title: Tek ve birden çok hesap genel istemci uygulamaları | Mavisi
description: Tek ve birden çok hesaba genel istemci uygulamalarına genel bakış.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80881359"
---
# <a name="single-and-multiple-account-public-client-apps"></a>Tek ve birden çok hesap genel istemci uygulamaları

Bu makale, tek hesap genel istemci uygulamalarına odaklanarak tek ve çoklu hesap genel istemci uygulamalarında kullanılan türleri anlamanıza yardımcı olur. 

Azure Active Directory kimlik doğrulama kitaplığı (ADAL), sunucuyu modelleyen.  İstemci uygulamanızı modelleyen, Microsoft kimlik doğrulama kitaplığı (MSAL).  Android uygulamalarının çoğu ortak istemci olarak kabul edilir. Ortak istemci, gizli anahtarı güvenli bir şekilde saklayamıyorum bir uygulamadır.  

MSAL, tek seferde yalnızca bir hesabın `PublicClientApplication` kullanılmasına izin veren uygulamalar için geliştirme deneyimini basitleştirecek ve açıklığa KAVUŞTURMAYA yönelik API yüzeyini uzmanlar. `PublicClientApplication`, ve `SingleAccountPublicClientApplication` `MultipleAccountPublicClientApplication`tarafından alt sınıflanmış.  Aşağıdaki diyagramda bu sınıflar arasındaki ilişki gösterilmektedir.

![SingleAccountPublicClientApplication UML sınıf diyagramı](./media/single-multi-account/single-and-multiple-account.png)

## <a name="single-account-public-client-application"></a>Tek hesap ortak istemci uygulaması

Sınıfı `SingleAccountPublicClientApplication` , aynı anda yalnızca tek bir hesabın oturum açmasına ızın veren msal tabanlı bir uygulama oluşturmanıza olanak tanır. `SingleAccountPublicClientApplication``PublicClientApplication` aşağıdaki yollarla farklılık gösterir:

- MSAL Şu anda oturum açmış olan hesabı izler.
  - Uygulamanız bir aracı kullanıyorsa (Azure portal uygulama kaydı sırasında varsayılan) ve bir aracısının bulunduğu bir cihaza yüklüyse, MSAL hesabın cihazda hala kullanılabilir olduğunu doğrular.
- `signIn`bir hesapta bir hesabı açık ve ayrı olarak oturum açmanıza olanak tanır.
- `acquireTokenSilent`Hesap parametresi gerektirmez.  Bir hesap sağlarsanız ve sağladığınız hesap, MSAL tarafından izlenen geçerli hesapla eşleşmiyorsa, bir `MsalClientException` oluşturulur.
- `acquireToken`kullanıcının hesaplara geçiş yapmasına izin vermez. Kullanıcı farklı bir hesaba geçiş yapmayı denerse, bir özel durum oluşturulur.
- `getCurrentAccount`aşağıdakileri sağlayan bir sonuç nesnesi döndürür:
  - Hesabın değişip değişmediğini gösteren bir Boole değeri. Örneğin, cihazdan kaldırılmakta olan bir hesap değişebilir.
  - Önceki hesap. Bu, hesap cihazdan kaldırıldığında veya yeni bir hesap oturum açtığınızda herhangi bir yerel veri temizliği yapmanız gerekiyorsa yararlıdır.
  - CurrentAccount.
- `signOut`cihazınızdan istemciyle ilişkili belirteçleri cihazdan kaldırır.  

Microsoft Authenticator veya Intune Şirket Portalı gibi bir Android kimlik doğrulama Aracısı cihaza yüklendiğinde ve uygulamanız aracıyı kullanmak üzere yapılandırıldığında, `signOut` hesabı cihazdan kaldırmaz.

## <a name="single-account-scenario"></a>Tek hesap senaryosu

Aşağıdaki sözde kod kullanılarak `SingleAccountPublicClientApplication`gösterilmektedir.

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

## <a name="multiple-account-public-client-application"></a>Birden çok hesap ortak istemci uygulaması

`MultipleAccountPublicClientApplication` Sınıfı, birden fazla hesabın aynı anda oturum açmaya ızın veren msal tabanlı uygulamalar oluşturmak için kullanılır. Aşağıdaki gibi hesapları almanıza, eklemenize ve kaldırmanıza olanak tanır:

### <a name="add-an-account"></a>Hesap ekleme

Bir veya daha fazla kez çağırarak `acquireToken` uygulamanızdaki bir veya daha fazla hesabı kullanın.  

### <a name="get-accounts"></a>Hesapları al

- Belirli `getAccount` bir hesabı almak için çağrısı yapın.
- Uygulamanın `getAccounts`Şu anda bilinen hesaplarının bir listesini almak için çağrısı yapın.

Uygulamanız, aracı uygulaması tarafından bilinen cihazdaki tüm Microsoft kimlik platformu hesaplarını numaralandıramıyor. Yalnızca uygulamanız tarafından kullanılan hesapları numaralandırabilirler.  Cihazdan kaldırılmış olan hesaplar bu işlevler tarafından döndürülmez.

### <a name="remove-an-account"></a>Hesap kaldırma

Hesap tanımlayıcısıyla çağırarak `removeAccount` bir hesabı kaldırın.

Uygulamanız bir aracı kullanacak şekilde yapılandırıldıysa ve cihaza bir aracı yüklenirse, çağırdığınızda `removeAccount`hesap aracıdan kaldırılmaz.  Yalnızca istemciyle ilişkili belirteçler kaldırılır.

## <a name="multiple-account-scenario"></a>Çoklu hesap senaryosu

Aşağıdaki sahte kod, birden çok hesap uygulamasının nasıl oluşturulduğunu, cihazdaki hesapların nasıl ekleneceğini ve belirteçlerin nasıl alınacağını gösterir.

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
