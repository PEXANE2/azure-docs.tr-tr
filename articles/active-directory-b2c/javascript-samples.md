---
title: JavaScript örnekleri
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de JavaScript'i kullanma hakkında bilgi edinin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/10/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: a26f6c5e69ca083335580a0368459e062de3941e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78187670"
---
# <a name="javascript-samples-for-use-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de kullanılmak üzere JavaScript örnekleri

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Azure Active Directory B2C (Azure AD B2C) uygulamalarınıza kendi JavaScript istemci yan kodunuzu ekleyebilirsiniz.

Uygulamalarınız için JavaScript'i etkinleştirmek için:

* [Özel politikanıza](custom-policy-overview.md) bir öğe ekleme
* Sayfa [düzeni](page-layout.md) seçme
* İsteklerinizde [b2clogin.com](b2clogin.md) kullanın

Bu makalede, komut dosyası yürütmeetkinleştirmek için özel ilkenizi nasıl değiştirebileceğiniz açıklanmaktadır.

> [!NOTE]
> Kullanıcı akışları için JavaScript'i etkinleştirmek istiyorsanız, [Azure Active Directory B2C'deki JavaScript ve sayfa düzeni sürümlerine](user-flow-javascript-overview.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

### <a name="select-a-page-layout"></a>Sayfa düzeni seçme

* Uygulamanızın kullanıcı arabirimi öğeleri için bir [sayfa düzeni](contentdefinitions.md#select-a-page-layout) seçin.

    JavaScript'i kullanmayı planlıyorsanız, özel politikanızdaki *tüm* `contract` içerik tanımları için sayfa sürümü içeren [bir sayfa düzeni sürümü tanımlamanız](contentdefinitions.md#migrating-to-page-layout) gerekir.

## <a name="add-the-scriptexecution-element"></a>ScriptExecution öğesini ekleme

**ScriptExecution** öğesini [RelyingParty](relyingparty.md) öğesine ekleyerek komut dosyası yürütmeyi etkinleştirin.

1. Özel ilke dosyanızı açın. Örneğin, *SignUpOrSignin.xml*.
2. **RelyingParty'nin** **UserJourneyBehaviors** öğesine **ScriptExecution** öğesini ekleyin:

    ```XML
    <RelyingParty>
      <DefaultUserJourney ReferenceId="B2CSignUpOrSignInWithPassword" />
      <UserJourneyBehaviors>
        <ScriptExecution>Allow</ScriptExecution>
      </UserJourneyBehaviors>
      ...
    </RelyingParty>
    ```
3. Dosyayı kaydedin ve yükleyin.

[!INCLUDE [active-directory-b2c-javascript-guidelines](../../includes/active-directory-b2c-javascript-guidelines.md)]

## <a name="javascript-samples"></a>JavaScript örnekleri

### <a name="show-or-hide-a-password"></a>Parolayı gösterme veya gizleme

Müşterilerinize kaydolma başarıları konusunda yardımcı olmanın yaygın bir yolu, parolaolarak girdiklerini görmelerine olanak sağlamaktır. Bu seçenek, kullanıcıların gerektiğinde parolalarını kolayca görmelerini ve düzeltmeyapmalarını sağlayarak kaydolmalarına yardımcı olur. Herhangi bir tür parolası **alanında, parolayı göster** etiketiiçeren bir onay kutusu vardır.  Bu, kullanıcının parolayı düz metin olarak görmesini sağlar. Kendi kendine ileri sayılan bir sayfa için kaydolma veya oturum açma şablonuna bu kod parçacıkını ekleyin:

```Javascript
function makePwdToggler(pwd){
  // Create show-password checkbox
  var checkbox = document.createElement('input');
  checkbox.setAttribute('type', 'checkbox');
  var id = pwd.id + 'toggler';
  checkbox.setAttribute('id', id);

  var label = document.createElement('label');
  label.setAttribute('for', id);
  label.appendChild(document.createTextNode('show password'));

  var div = document.createElement('div');
  div.appendChild(checkbox);
  div.appendChild(label);

  // Add show-password checkbox under password input
  pwd.insertAdjacentElement('afterend', div);

  // Add toggle password callback
  function toggle(){
    if(pwd.type === 'password'){
      pwd.type = 'text';
    } else {
      pwd.type = 'password';
    }
  }
  checkbox.onclick = toggle;
  // For non-mouse usage
  checkbox.onkeydown = toggle;
}

function setupPwdTogglers(){
  var pwdInputs = document.querySelectorAll('input[type=password]');
  for (var i = 0; i < pwdInputs.length; i++) {
    makePwdToggler(pwdInputs[i]);
  }
}

setupPwdTogglers();
```

### <a name="add-terms-of-use"></a>Kullanım koşulları ekleme

**Kullanım Koşulları** onay kutusu eklemek istediğiniz sayfanıza aşağıdaki kodu ekleyin. Bu onay kutusu genellikle yerel hesap kayıt ve sosyal hesap kayıt sayfalarında gereklidir.

```Javascript
function addTermsOfUseLink() {
    // find the terms of use label element
    var termsOfUseLabel = document.querySelector('#api label[for="termsOfUse"]');
    if (!termsOfUseLabel) {
        return;
    }

    // get the label text
    var termsLabelText = termsOfUseLabel.innerHTML;

    // create a new <a> element with the same inner text
    var termsOfUseUrl = 'https://docs.microsoft.com/legal/termsofuse';
    var termsOfUseLink = document.createElement('a');
    termsOfUseLink.setAttribute('href', termsOfUseUrl);
    termsOfUseLink.setAttribute('target', '_blank');
    termsOfUseLink.appendChild(document.createTextNode(termsLabelText));

    // replace the label text with the new element
    termsOfUseLabel.replaceChild(termsOfUseLink, termsOfUseLabel.firstChild);
}
```

Kodda, kullanım `termsOfUseUrl` koşulları anlaşmanızın bağlantısını değiştirin. Dizininiz için **termsOfUse** adında yeni bir kullanıcı özniteliği oluşturun ve ardından kullanıcı özniteliği olarak **termsOfUse'u** ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Active Directory B2C'de özel bir ilke kullanarak uygulamanızın kullanıcı arabirimini özelleştirmede uygulamalarınızın](custom-policy-ui-customization.md)kullanıcı arabirimini nasıl özelleştirebileceğiniz hakkında daha fazla bilgi edinin.
