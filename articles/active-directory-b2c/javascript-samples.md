---
title: JavaScript örnekleri
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de JavaScript kullanma hakkında bilgi edinin.
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
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78187670"
---
# <a name="javascript-samples-for-use-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'yi kullanmak için JavaScript örnekleri

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Kendi JavaScript istemci tarafı kodunuzu Azure Active Directory B2C (Azure AD B2C) uygulamalarınıza ekleyebilirsiniz.

Uygulamalarınız için JavaScript 'ı etkinleştirmek üzere:

* [Özel ilkenize](custom-policy-overview.md) bir öğe ekleyin
* [Sayfa düzeni](page-layout.md) seçin
* İsteklerinizi [b2clogin.com](b2clogin.md) kullanma

Bu makalede, betik yürütmeyi etkinleştirmek için özel ilkenizi nasıl değiştirebileceğiniz açıklanır.

> [!NOTE]
> Kullanıcı akışları için JavaScript 'ı etkinleştirmek istiyorsanız, bkz. [Azure Active Directory B2C JavaScript ve sayfa düzeni sürümleri](user-flow-javascript-overview.md).

## <a name="prerequisites"></a>Önkoşullar

### <a name="select-a-page-layout"></a>Sayfa düzeni seçin

* Uygulamanızın Kullanıcı arabirimi öğeleri için bir [sayfa düzeni](contentdefinitions.md#select-a-page-layout) seçin.

    JavaScript kullanmayı düşünüyorsanız, özel ilkenizde *Tüm* içerik tanımlarının sayfa `contract` sürümüyle sayfa [düzeni sürümü tanımlamanız](contentdefinitions.md#migrating-to-page-layout) gerekir.

## <a name="add-the-scriptexecution-element"></a>ScriptExecution öğe ekleyin

**Scriptexecution** öğesini [RelyingParty](relyingparty.md) öğesine ekleyerek betik yürütmeyi etkinleştirirsiniz.

1. Özel ilke dosyanızı açın. Örneğin, *Signuporsignın. xml*.
2. **Scriptexecution** öğesini **RelyingParty**öğesinin **userscripts newydavranışlar** öğesine ekleyin:

    ```XML
    <RelyingParty>
      <DefaultUserJourney ReferenceId="B2CSignUpOrSignInWithPassword" />
      <UserJourneyBehaviors>
        <ScriptExecution>Allow</ScriptExecution>
      </UserJourneyBehaviors>
      ...
    </RelyingParty>
    ```
3. Kaydedin ve dosyayı karşıya yükleyin.

[!INCLUDE [active-directory-b2c-javascript-guidelines](../../includes/active-directory-b2c-javascript-guidelines.md)]

## <a name="javascript-samples"></a>JavaScript örnekleri

### <a name="show-or-hide-a-password"></a>Bir parolayı Gizle veya Göster

Kaydolma başarılarını ile müşterilerinize yardımcı olmak için yaygın bir yolu, bunları ne bunlar kendi parolanızı girdikten görmek izin vermektir. Bu seçenek, kullanıcıların kolayca görebilir ve gerekirse parolasını düzeltmeler yapmak sağlayarak kaydolma yardımcı olur. Parola türünde herhangi bir alanda, **parolayı göster** etiketi olan bir onay kutusu vardır.  Bu düz metin parolayı görmesine olanak sağlar. Bu kod parçacığı uygulamasına kaydolma veya oturum açma şablonunuz için bir otomatik olarak onaylanan sayfa şunları içerir:

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

Aşağıdaki kodu sayfanıza bir **kullanım koşulları** onay kutusu eklemek istediğiniz yere ekleyin. Bu onay kutusunu, genellikle, yerel hesap kaydolma ve sosyal hesap kaydolma sayfalarında gereklidir.

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

Kodda `termsOfUseUrl` ' yi kullanım koşullarınızın bağlantısı ile değiştirin. Dizininiz için, **termsofuse** adlı yeni bir kullanıcı özniteliği oluşturun ve ardından Kullanıcı özniteliği olarak **termsofuse** ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Active Directory B2C ' de özel bir ilke kullanarak uygulamanızın kullanıcı arabirimini özelleştirme](custom-policy-ui-customization.md)bölümünde uygulamalarınızın Kullanıcı arabirimini özelleştirme hakkında daha fazla bilgi edinin.
