---
title: Azure CredentialsCombo Kullanıcı arabirimi öğesi | Microsoft Docs
description: Azure portal için Microsoft. COMPUTE. CredentialsCombo UI öğesini açıklar.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/29/2018
ms.author: tomfitz
ms.openlocfilehash: 3036e5882e236dbbb9cf4f9aae17617822422a82
ms.sourcegitcommit: d060947aae93728169b035fd54beef044dbe9480
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2019
ms.locfileid: "68742099"
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

## <a name="remarks"></a>Açıklamalar
- `osPlatform`belirtilmelidir ve **Windows** ya da **Linux**olabilir.
- True olarak ayarlanırsa, parola veya SSH ortak anahtar metin kutularının başarıyla doğrulanacak değerleri olmalıdır. `constraints.required` Varsayılan değer **true**'dur.
- True olarak ayarlanırsa, kullanıcının parolasını onaylamak için ikinci metin kutusu gizlenir. `options.hideConfirmation` Varsayılan değer **false**.
- True olarak ayarlanırsa, parola kimlik doğrulamasını kullanma seçeneği gizlenir. `options.hidePassword` Bu, yalnızca `osPlatform` **Linux**olduğunda kullanılabilir. Varsayılan değer **false**.
- İzin verilen parolalarla ilgili ek kısıtlamalar `customPasswordRegex` özelliği kullanılarak uygulanabilir. Bir parola özel `customValidationMessage` doğrulama başarısız olduğunda içindeki dize görüntülenir. Her iki özellik için varsayılan değer **null**.

## <a name="sample-output"></a>Örnek çıktı
Windows veya **Linux ise** ve Kullanıcı SSH ortak anahtarı yerine bir parola sağladıysa, Denetim aşağıdaki çıktıyı döndürür: `osPlatform` `osPlatform`

```json
{
  "authenticationType": "password",
  "password": "p4ssw0rddem0",
}
```

Linux `osPlatform` ise ve Kullanıcı SSH ortak anahtarı sağladıysa, Denetim aşağıdaki çıktıyı döndürür:

```json
{
  "authenticationType": "sshPublicKey",
  "sshPublicKey": "AAAAB3NzaC1yc2EAAAABIwAAAIEA1on8gxCGJJWSRT4uOrR13mUaUk0hRf4RzxSZ1zRbYYFw8pfGesIFoEuVth4HKyF8k1y4mRUnYHP1XNMNMJl1JcEArC2asV8sHf6zSPVffozZ5TT4SfsUu/iKy9lUcCfXzwre4WWZSXXcPff+EHtWshahu3WzBdnGxm5Xoi89zcE=",
}
```

## <a name="next-steps"></a>Sonraki adımlar
* UI tanımları oluşturmaya giriş için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* UI öğelerindeki ortak özelliklerin açıklaması için bkz. [Createuıdefinition Elements](create-uidefinition-elements.md).
