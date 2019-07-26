---
title: Azure yönetilen uygulaması Createuıdefinition yapıt başvurusu
description: Bu makale Createuıdefinition yapıtı için bir başvurudur.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.author: lazinnat
author: lazinnat
ms.date: 07/11/2019
ms.openlocfilehash: 5168e977a7bc289ec8981ef1df8cddd5cc6df678
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68336484"
---
# <a name="reference-user-interface-elements-artifact"></a>Referans: Kullanıcı arabirimi öğeleri yapıtı

Bu makale, Azure yönetilen uygulamalarında *Createuıdefinition. JSON* yapıtı için bir başvurudur. Kullanıcı arabirimi öğeleri yazma hakkında daha fazla bilgi için bkz. [Kullanıcı arabirimi öğeleri oluşturma](create-uidefinition-elements.md).

## <a name="user-interface-elements"></a>Kullanıcı arabirimi öğeleri

Aşağıdaki JSON, Azure yönetilen uygulamalar için *Createuıdefinition. JSON* dosyasının bir örneğini göstermektedir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
  "handler": "Microsoft.Azure.CreateUIDef",
  "version": "0.1.2-preview",
  "parameters": {
    "basics": [
      {}
    ],
    "steps": [
      {
        "name": "applicationSettings",
        "label": "Application Settings",
        "subLabel": {
          "preValidation": "Configure your application settings",
          "postValidation": "Done"
        },
        "bladeTitle": "Application Settings",
        "elements": [
          {
            "name": "funcname",
            "type": "Microsoft.Common.TextBox",
            "label": "Name of the function to be created",
            "toolTip": "Name of the function to be created",
            "visible": true,
            "constraints": {
              "required": true
            }
          },
          {
            "name": "storagename",
            "type": "Microsoft.Common.TextBox",
            "label": "Name of the storage to be created",
            "toolTip": "Name of the storage to be created",
            "visible": true,
            "constraints": {
              "required": true
            }
          },
          {
            "name": "zipFileBlobUri",
            "type": "Microsoft.Common.TextBox",
            "defaultValue": "https://github.com/Azure/azure-quickstart-templates/tree/master/101-custom-rp-with-function/artifacts/functionzip/functionpackage.zip",
            "label": "The Uri to the uploaded function zip file",
            "toolTip": "The Uri to the uploaded function zip file",
            "visible": true
          }
        ]
      }
    ],
    "outputs": {
      "funcname": "[steps('applicationSettings').funcname]",
      "storageName": "[steps('applicationSettings').storagename]",
      "zipFileBlobUri": "[steps('applicationSettings').zipFileBlobUri]"
    }
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Özel eylemler ve kaynaklarla yönetilen uygulama oluşturma](tutorial-create-managed-app-with-custom-provider.md)
- [Başvurunun Dağıtım şablonu yapıtı](reference-main-template-artifact.md)
- [Başvurunun Tanım yapıtı görüntüle](reference-view-definition-artifact.md)