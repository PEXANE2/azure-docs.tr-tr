---
title: CredentialsCombo UI öğesi
description: Azure portal için Microsoft. COMPUTE. CredentialsCombo UI öğesini açıklar.
author: tfitzmac
ms.topic: conceptual
ms.date: 09/29/2018
ms.author: tomfitz
ms.openlocfilehash: baee83e08505ddabb4680b5bbb4101479010ea47
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75652144"
---
# <a name="microsoftcomputecredentialscombo-ui-element"></a>Microsoft. COMPUTE. CredentialsCombo UI öğesi

Windows ve Linux parolaları ve SSH ortak anahtarları için yerleşik doğrulamaya sahip bir denetim grubu.

## <a name="ui-sample"></a>UI örneği

Windows için kullanıcılar şunları görürler:

![Microsoft. COMPUTE. CredentialsCombo pencere](./media/managed-application-elements/microsoft.compute.credentialscombo-windows.png)

Parola seçiliyken Linux için kullanıcılar şunları görür:

![Microsoft. COMPUTE. CredentialsCombo Linux parolası](./media/managed-application-elements/microsoft.compute.credentialscombo-linux-password.png)

SSH ortak anahtarı seçiliyken Linux için kullanıcılar şunları görür:

![Microsoft. COMPUTE. CredentialsCombo Linux anahtarı](./media/managed-application-elements/microsoft.compute.credentialscombo-linux-key.png)

## <a name="schema"></a>Şema

Windows için aşağıdaki şemayı kullanın:

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

**Linux**için aşağıdaki şemayı kullanın:

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

`osPlatform` **Windows**veya `osPlatform` **Linux** ise ve Kullanıcı SSH ortak anahtarı yerine bir parola sağladıysa, Denetim aşağıdaki çıktıyı döndürür:

```json
{
  "authenticationType": "password",
  "password": "p4ssw0rddem0",
}
```

`osPlatform` **Linux** ise ve Kullanıcı SSH ortak anahtarı sağladıysa, Denetim aşağıdaki çıktıyı döndürür:

```json
{
  "authenticationType": "sshPublicKey",
  "sshPublicKey": "AAAAB3NzaC1yc2EAAAABIwAAAIEA1on8gxCGJJWSRT4uOrR13mUaUk0hRf4RzxSZ1zRbYYFw8pfGesIFoEuVth4HKyF8k1y4mRUnYHP1XNMNMJl1JcEArC2asV8sHf6zSPVffozZ5TT4SfsUu/iKy9lUcCfXzwre4WWZSXXcPff+EHtWshahu3WzBdnGxm5Xoi89zcE=",
}
```

## <a name="remarks"></a>Açıklamalar

- `osPlatform` belirtilmelidir ve **Windows** ya da **Linux**olabilir.
- `constraints.required` **true**olarak ayarlanırsa, parola veya SSH ortak anahtar metin kutularının başarıyla doğrulanacak değerleri olmalıdır. Varsayılan değer **true**'dur.
- `options.hideConfirmation` **true**olarak ayarlanırsa, kullanıcının parolasını onaylamak için ikinci metin kutusu gizlenir. Varsayılan değer **false**.
- `options.hidePassword` **true**olarak ayarlanırsa, parola kimlik doğrulamasını kullanma seçeneği gizlenir. Bu, yalnızca `osPlatform` **Linux**olduğunda kullanılabilir. Varsayılan değer **false**.
- İzin verilen parolalarla ilgili ek kısıtlamalar `customPasswordRegex` özelliği kullanılarak uygulanabilir. `customValidationMessage` dize, bir parola özel doğrulama başarısız olduğunda görüntülenir. Her iki özellik için varsayılan değer **null**.

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımları oluşturmaya giriş için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* UI öğelerindeki ortak özelliklerin açıklaması için bkz. [Createuıdefinition Elements](create-uidefinition-elements.md).
