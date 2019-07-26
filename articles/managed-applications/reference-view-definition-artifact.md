---
title: Azure yönetilen uygulama görünümü tanımı yapıt başvurusu
description: Bu makale, görünüm tanımı yapıtı için bir başvurudur.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.author: lazinnat
author: lazinnat
ms.date: 07/11/2019
ms.openlocfilehash: e60f26fe0a7144d768bac020d62c61cb92594914
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68336549"
---
# <a name="reference-view-definition-artifact"></a>Referans: Tanım yapıtını görüntüleme

Bu makale, Azure yönetilen uygulamalarında *ViewDefinition. JSON* yapıtı için bir başvurudur. Yazma görünümleri yapılandırması hakkında daha fazla bilgi için bkz. [View definition yapay](concepts-view-definition.md).

## <a name="view-definition"></a>Tanımı görüntüle

Aşağıdaki JSON, Azure yönetilen uygulamalar için *ViewDefinition. JSON* dosyasının bir örneğini göstermektedir:

```json
{
  "views": [{
    "kind": "Overview",
    "properties": {
      "header": "Welcome to your Demo Azure Managed Application",
      "description": "This Managed application with Custom Provider is for demo purposes only.",
      "commands": [{
          "displayName": "Ping Action",
          "path": "/customping",
          "icon": "LaunchCurrent"
      }]
    }
  },
  {
    "kind": "CustomResources",
    "properties": {
      "displayName": "Users",
      "version": "1.0.0.0",
      "resourceType": "users",
      "createUIDefinition": {
        "parameters": {
          "steps": [{
            "name": "add",
            "label": "Add user",
            "elements": [{
              "name": "name",
              "label": "User's Full Name",
              "type": "Microsoft.Common.TextBox",
              "defaultValue": "",
              "toolTip": "Provide a full user name.",
              "constraints": { "required": true }
            },
            {
              "name": "location",
              "label": "User's Location",
              "type": "Microsoft.Common.TextBox",
              "defaultValue": "",
              "toolTip": "Provide a Location.",
              "constraints": { "required": true }
            }]
          }],
          "outputs": {
            "name": "[steps('add').name]",
            "properties": {
              "FullName": "[steps('add').name]",
              "Location": "[steps('add').location]"
            }
          }
        }
      },
      "commands": [{
        "displayName": "Custom Context Action",
        "path": "users/contextAction",
        "icon": "Start"
      }],
      "columns": [
        { "key": "properties.FullName", "displayName": "Full Name" },
        { "key": "properties.Location", "displayName": "Location", "optional": true }
      ]
    }
  }]
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Özel eylemler ve kaynaklarla yönetilen uygulama oluşturma](tutorial-create-managed-app-with-custom-provider.md)
- [Başvurunun Kullanıcı arabirimi öğeleri yapıtı](reference-createuidefinition-artifact.md)
- [Başvurunun Dağıtım şablonu yapıtı](reference-main-template-artifact.md)