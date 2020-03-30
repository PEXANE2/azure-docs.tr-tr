---
title: CreateUiDefinition artifakı
description: Azure Yönetilen Uygulama için createUiDefinition artifakı nasıl oluşturulur' u gösterir. Dosya createUiDefinition.json olarak adlandırılır.
ms.topic: conceptual
ms.author: lazinnat
author: lazinnat
ms.date: 07/11/2019
ms.openlocfilehash: 9be1cb02c419314a33b9487bf183ef3659b2b549
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75651494"
---
# <a name="reference-user-interface-elements-artifact"></a>Başvuru: Kullanıcı arabirimi elemanları artifakı

Bu makale, Azure Yönetilen Uygulamalar'daki *createUiDefinition.json* artifakı için bir başvurudur. Kullanıcı arabirimi öğeleriyazma hakkında daha fazla bilgi için [bkz.](create-uidefinition-elements.md)

## <a name="user-interface-elements"></a>Kullanıcı arabirimi öğeleri

Aşağıdaki JSON, Azure Yönetilen Uygulamalar için *createUiDefinition.json* dosyasının bir örneğini gösterir:

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
- [Başvuru: Dağıtım şablonu artefakt](reference-main-template-artifact.md)
- [Başvuru: Tanım artifakı görüntüle](reference-view-definition-artifact.md)