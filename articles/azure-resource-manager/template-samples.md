---
title: Azure Resource Manager şablonu örnekleri
description: Roller ve kilitler gibi yönetim özelliklerini dağıtmaya yönelik şablon örnekleri Azure Resource Manager.
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.date: 11/16/2018
ms.author: tomfitz
ms.openlocfilehash: e342507b584a405637fc6728dfcd6e49199a154f
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72390174"
---
# <a name="azure-resource-manager-templates-for-management-features"></a>Yönetim özellikleri için Azure Resource Manager şablonları

Aşağıdaki tabloda, Kaynak Yöneticisi tarafından sunulan özellikler için Azure Resource Manager şablonlarının bağlantıları yer almaktadır.

| | |
|-|-|
|**Rol atamaları**||
| [Kaynak grubu için rol ata](https://github.com/Azure/azure-quickstart-templates/tree/master/101-rbac-builtinrole-resourcegroup)| Mevcut bir kaynak grubu için kullanıcıya yerleşik bir rol atar. |
| [Var olan sanal makine için rol ata](https://github.com/Azure/azure-quickstart-templates/tree/master/101-rbac-builtinrole-virtualmachine)| Mevcut bir VM için kullanıcıya yerleşik bir rol atar. |
| [Çeşitli sanal makineler için rol atama](https://github.com/Azure/azure-quickstart-templates/tree/master/201-rbac-builtinrole-multipleVMs)| Birden fazla sanal makine için kullanıcılara yerleşik roller atar. |
| [Azure aboneliği için rol atama](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-level-deployments/subscription-role-assigment)| Bir Azure aboneliği için kullanıcıya bir rol atar. |
|**Rol tanımı**||
| [Özel rol tanımı oluştur](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-level-deployments/create-role-def)| Bir Azure aboneliğinde yeni bir rol tanımı oluşturur. |
|**Kaynak kilidi**||
| [Kaynak grubunu kilitle](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-level-deployments/create-rg-lock-role-assignment)| Bir kaynak grubu oluşturur ve kaynak grubuna bir **Donotdelete** kilidi uygular. Bir kullanıcıya katkıda bulunan rolü atar. |
| | |
