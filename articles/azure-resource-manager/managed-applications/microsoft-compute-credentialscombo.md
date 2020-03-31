---
title: Kimlik BilgileriCombo UI öğesi
description: Azure portalı için Microsoft.Compute.CredentialsCombo UI öğesini açıklar.
author: tfitzmac
ms.topic: conceptual
ms.date: 09/29/2018
ms.author: tomfitz
ms.openlocfilehash: baee83e08505ddabb4680b5bbb4101479010ea47
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75652144"
---
# <a name="microsoftcomputecredentialscombo-ui-element"></a>Microsoft.Compute.CredentialsCombo UI öğesi

Windows ve Linux parolaları ve SSH ortak anahtarları için yerleşik doğrulamaya sahip bir denetim grubu.

## <a name="ui-sample"></a>UI örneği

Windows için kullanıcılar şunları görür:

![Microsoft.Compute.CredentialsCombo Windows](./media/managed-application-elements/microsoft.compute.credentialscombo-windows.png)

Seçilen parolaile Linux için, kullanıcılar bkz:

![Microsoft.Compute.CredentialsCombo Linux şifresi](./media/managed-application-elements/microsoft.compute.credentialscombo-linux-password.png)

SSH ortak anahtarı nın seçili Olduğu Linux için kullanıcılar şunları görür:

![Microsoft.Compute.CredentialsCombo Linux anahtarı](./media/managed-application-elements/microsoft.compute.credentialscombo-linux-key.png)

## <a name="schema"></a>Şema

Windows için aşağıdaki şema'yı kullanın:

```json
{
  "name": "element1",
  "type": "Microsoft.Compute.CredentialsCombo",
  "label": {
    "password": "Password",
    "confirmPassword": "Confirm password"
  },
  "toolTip": {
    "password": ""
  },
  "constraints": {
    "required": true,
    "customPasswordRegex": "^(?=.*[A-Za-z])(?=.*\\d)[A-Za-z\\d]{12,}$",
    "customValidationMessage": "The password must be alphanumeric, contain at least 12 characters, and have at least 1 letter and 1 number."
  },
  "options": {
    "hideConfirmation": false
  },
  "osPlatform": "Windows",
  "visible": true
}
```

**Linux**için aşağıdaki şemaları kullanın:

```json
{
  "name": "element1",
  "type": "Microsoft.Compute.CredentialsCombo",
  "label": {
    "authenticationType": "Authentication type",
    "password": "Password",
    "confirmPassword": "Confirm password",
    "sshPublicKey": "SSH public key"
  },
  "toolTip": {
    "authenticationType": "",
    "password": "",
    "sshPublicKey": ""
  },
  "constraints": {
    "required": true,
    "customPasswordRegex": "^(?=.*[A-Za-z])(?=.*\\d)[A-Za-z\\d]{12,}$",
    "customValidationMessage": "The password must be alphanumeric, contain at least 12 characters, and have at least 1 letter and 1 number."
  },
  "options": {
    "hideConfirmation": false,
    "hidePassword": false
  },
  "osPlatform": "Linux",
  "visible": true
}
```

## <a name="sample-output"></a>Örnek çıktı

Windows `osPlatform` **Windows**ise veya `osPlatform` **Linux** ise ve kullanıcı SSH ortak anahtarı yerine bir parola sağlamışsa, denetim aşağıdaki çıktıyı döndürür:

```json
{
  "authenticationType": "password",
  "password": "p4ssw0rddem0",
}
```

Linux `osPlatform` **Linux** ise ve kullanıcı bir SSH ortak anahtarı sağlamışsa, denetim aşağıdaki çıktıyı döndürür:

```json
{
  "authenticationType": "sshPublicKey",
  "sshPublicKey": "AAAAB3NzaC1yc2EAAAABIwAAAIEA1on8gxCGJJWSRT4uOrR13mUaUk0hRf4RzxSZ1zRbYYFw8pfGesIFoEuVth4HKyF8k1y4mRUnYHP1XNMNMJl1JcEArC2asV8sHf6zSPVffozZ5TT4SfsUu/iKy9lUcCfXzwre4WWZSXXcPff+EHtWshahu3WzBdnGxm5Xoi89zcE=",
}
```

## <a name="remarks"></a>Açıklamalar

- `osPlatform`belirtilmelidir ve **Windows** veya **Linux**olabilir.
- Doğru `constraints.required` olarak **true**ayarlanmışsa, parola veya SSH ortak anahtar metin kutuları başarıyla doğrulamak için değerlere sahip olmalıdır. Varsayılan değer **doğrudur.**
- Doğru `options.hideConfirmation` olarak **true**ayarlanmışsa, kullanıcının parolasını onaylamak için ikinci metin kutusu gizlenir. Varsayılan değer **false** şeklindedir.
- Doğru `options.hidePassword` olarak **true**ayarlanmışsa, parola kimlik doğrulaması kullanma seçeneği gizlenir. Sadece `osPlatform` **Linux**olduğunda kullanılabilir. Varsayılan değer **false** şeklindedir.
- İzin verilen parolalar üzerindeki ek kısıtlamalar `customPasswordRegex` özelliği kullanılarak uygulanabilir. Parola özel `customValidationMessage` doğrulamabaşarısız olduğunda dize görüntülenir. Her iki özellik için varsayılan değer **null'** dur.

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımlarını oluşturmaya giriş için create [UiDefinition 'a](create-uidefinition-overview.md)bakın.
* UI öğelerindeki ortak özelliklerin açıklaması için [createUiDefinition öğelerine](create-uidefinition-elements.md)bakın.
