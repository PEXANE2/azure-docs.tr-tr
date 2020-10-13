---
title: ServicePrincipalSelector Kullanıcı arabirimi öğesi
description: Azure portal için Microsoft. Common. Servicesprincipalselector Kullanıcı arabirimi öğesini açıklar. Bir parola veya sertifika parmak izi girmek için bir uygulama tanımlayıcısı ve metin kutusu seçmek üzere bir açılan liste sağlar.
author: tfitzmac
ms.topic: conceptual
ms.date: 09/29/2020
ms.author: tomfitz
ms.openlocfilehash: 73b242754bfae53b6df5abd9c2c8dee33b973dad
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91576005"
---
# <a name="microsoftcommonserviceprincipalselector-ui-element"></a>Microsoft. Common. Servicesprincipalselector Kullanıcı arabirimi öğesi

Kullanıcıların mevcut bir hizmet sorumlusunu seçmesini veya yenisini kaydetmesini sağlayan bir denetim. **Yeni oluştur**' u seçtiğinizde, yeni bir uygulama kaydetme adımlarını izleyebilirsiniz. Mevcut bir uygulamayı seçtiğinizde, denetim bir parola veya sertifika parmak izi girişi için bir metin kutusu sağlar.

## <a name="ui-sample"></a>UI örneği

Varsayılan görünüm, özelliğindeki değerlere göre belirlenir `defaultValue` . `principalId`Özellik geçerli bir genel benzersiz tanımlayıcı (GUID) içeriyorsa, Denetim uygulamanın nesne kimliğini arar. Kullanıcı açılan listeden seçim yapmazsa varsayılan değer geçerlidir.

:::image type="content" source="./media/managed-application-elements/microsoft-common-serviceprincipal-initial.png" alt-text="Microsoft. Common. Servicesprincipalselector ilk görünüm":::

Açılan listeden yeni veya var olan bir uygulama tanımlayıcı **Oluştur** ' u seçtiğinizde, metin kutusuna bir parola veya sertifika parmak izi girmek Için **kimlik doğrulaması türü** görüntülenir.

:::image type="content" source="./media/managed-application-elements/microsoft-common-serviceprincipal-selection.png" alt-text="Microsoft. Common. Servicesprincipalselector ilk görünüm":::

## <a name="schema"></a>Şema

```json
{
  "name": "ServicePrincipal",
  "type": "Microsoft.Common.ServicePrincipalSelector",
  "label": {
    "principalId": "App Id",
    "password": "Password",
    "certificateThumbprint": "Certificate thumbprint",
    "authenticationType": "Authentication Type",
    "sectionHeader": "Service Principal"
  },
  "toolTip": {
    "principalId": "App Id",
    "password": "Password",
    "certificateThumbprint": "Certificate thumbprint",
    "authenticationType": "Authentication Type"
  },
  "defaultValue": {
    "principalId": "<default guid>",
    "name": "(New) default App Id"
  },
  "constraints": {
    "required": true,
    "regex": "^[a-zA-Z0-9]{8,}$",
    "validationMessage": "Password must be at least 8 characters long, contain only numbers and letters"
  },
  "options": {
    "hideCertificate": false
  },
  "visible": true
}
```

## <a name="remarks"></a>Açıklamalar

- Gerekli özellikler şunlardır:
  - `name`
  - `type`
  - `label`
  - `defaultValue`: Varsayılan ve belirtir `principalId` `name` .

- İsteğe bağlı özellikler şunlardır:
  - `toolTip`: Her etikete bir araç ipucu ekler `infoBalloon` .
  - `visible`: Denetimi gizleyin veya görüntüleyin.
  - `options`: Sertifika parmak izi seçeneğinin kullanılabilir yapılıp yapılmayacağını belirtir.
  - `constraints`: Parola doğrulaması için Regex kısıtlamaları.

## <a name="example"></a>Örnek

Aşağıda, denetimin bir örneği verilmiştir `Microsoft.Common.ServicePrincipalSelector` . `defaultValue`Özelliği `principalId` `<default guid>` Varsayılan uygulama tanımlayıcı GUID 'si için bir yer tutucu olarak ayarlanır.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
  "handler": "Microsoft.Azure.CreateUIDef",
  "version": "0.1.2-preview",
  "parameters": {
    "basics": [],
    "steps": [
      {
        "name": "SPNcontrol",
        "label": "SPNcontrol",
        "elements": [
          {
            "name": "ServicePrincipal",
            "type": "Microsoft.Common.ServicePrincipalSelector",
            "label": {
              "principalId": "App Id",
              "password": "Password",
              "certificateThumbprint": "Certificate thumbprint",
              "authenticationType": "Authentication Type",
              "sectionHeader": "Service Principal"
            },
            "toolTip": {
              "principalId": "App Id",
              "password": "Password",
              "certificateThumbprint": "Certificate thumbprint",
              "authenticationType": "Authentication Type"
            },
            "defaultValue": {
              "principalId": "<default guid>",
              "name": "(New) default App Id"
            },
            "constraints": {
              "required": true,
              "regex": "^[a-zA-Z0-9]{8,}$",
              "validationMessage": "Password must be at least 8 characters long, contain only numbers and letters"
            },
            "options": {
              "hideCertificate": false
            },
            "visible": true
          }
        ]
      }
    ],
    "outputs": {
      "appId": "[steps('SPNcontrol').ServicePrincipal.appId]",
      "objectId": "[steps('SPNcontrol').ServicePrincipal.objectId]",
      "password": "[steps('SPNcontrol').ServicePrincipal.password]",
      "certificateThumbprint": "[steps('SPNcontrol').ServicePrincipal.certificateThumbprint]",
      "newOrExisting": "[steps('SPNcontrol').ServicePrincipal.newOrExisting]",
      "authenticationType": "[steps('SPNcontrol').ServicePrincipal.authenticationType]"
    }
  }
}
```

## <a name="example-output"></a>Örnek çıkış

, `appId` Seçtiğiniz veya oluşturduğunuz uygulama kaydının kimliğidir. , `objectId` Seçilen uygulama kaydı için yapılandırılmış hizmet sorumluları Için ObjectIDs dizisidir.

Açılan listeden seçim yapılmadığınızda, `newOrExisting` özellik değeri **yenidir**:

```json
{
  "appId": {
    "value": "<default guid>"
  },
  "objectId": {
    "value": ["<default guid>"]
  },
  "password": {
    "value": "<password>"
  },
  "certificateThumbprint": {
    "value": ""
  },
  "newOrExisting": {
    "value": "new"
  },
  "authenticationType": {
    "value": "password"
  }
}
```

**Yeni oluştur** veya var olan bir uygulama tanımlayıcısı açılan menüden seçildiğinde, `newOrExisting` özellik değeri **mevcut**olur:

```json
{
  "appId": {
    "value": "<guid>"
  },
  "objectId": {
    "value": ["<guid>"]
  },
  "password": {
    "value": "<password>"
  },
  "certificateThumbprint": {
    "value": ""
  },
  "newOrExisting": {
    "value": "existing"
  },
  "authenticationType": {
    "value": "password"
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

- UI tanımları oluşturmaya giriş için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md).
- UI öğelerindeki ortak özelliklerin açıklaması için bkz. [Createuıdefinition Elements](create-uidefinition-elements.md).
