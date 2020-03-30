---
title: include dosyası
description: include dosyası
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 12/14/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 9e4f2e355240ba8682cbe9f86f2be94e7dd0d92d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70032359"
---
## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Kapsayıcı kayıt defterinize erişimi olan bir hizmet sorumlusu oluşturmak için, Aşağıdaki komut dosyasını [Azure Bulut Kabuğu'nda](../articles/cloud-shell/overview.md) veya [Azure CLI'nin](/cli/azure/install-azure-cli)yerel yüklemesinde çalıştırın. Komut dosyası Bash kabuğu için biçimlendirilmiştir.

Komut dosyasını çalıştırmadan `ACR_NAME` önce, değişkeni kapsayıcı kayıt defterinizin adıyla güncelleştirin. Değer, `SERVICE_PRINCIPAL_NAME` Azure Etkin Dizin kiracınızda benzersiz olmalıdır. " " hatası`'http://acr-service-principal' already exists.`alırsanız, servis sorumlusu için farklı bir ad belirtin.

Farklı izinler vermek `--role` istiyorsanız, [az reklam sp create-for-rbac][az-ad-sp-create-for-rbac] komutundaki değeri isteğe bağlı olarak değiştirebilirsiniz. Rollerin tam listesi için [ACR rolleri ve izinleri](https://github.com/Azure/acr/blob/master/docs/roles-and-permissions.md)bölümüne bakın.

Komut dosyasını çalıştırdıktan sonra, servis müdürünün **kimliğini** ve **parolasını**not alın. Kimlik bilgilerine sahip olduktan sonra, uygulamalarınızı ve hizmetlerinizi hizmet sorumlusu olarak kapsayıcı kayıt defterinize doğrulayacak şekilde yapılandırabilirsiniz.

<!-- https://github.com/Azure-Samples/azure-cli-samples/blob/master/container-registry/service-principal-create/service-principal-create.sh -->
[!code-azurecli-interactive[acr-sp-create](~/cli_scripts/container-registry/service-principal-create/service-principal-create.sh)]

### <a name="use-an-existing-service-principal"></a>Varolan bir hizmet ilkesini kullanma

Varolan bir hizmet ilkesine kayıt defterierişimi vermek için, hizmet ilkesine yeni bir rol atamanız gerekir. Yeni bir hizmet ilkesi oluşturmada olduğu gibi, diğerlerinin yanı sıra çekme, itme ve çekme ve sahip erişimi verebilirsiniz.

Aşağıdaki komut dosyası, değişkende belirttiğiniz bir hizmet müdürüne *çekme* izinleri vermek için [az rol ataması oluşturma][az-role-assignment-create] komutunu `SERVICE_PRINCIPAL_ID` kullanır. Farklı `--role` bir erişim düzeyi vermek istiyorsanız değeri ayarlayın.


<!-- https://github.com/Azure-Samples/azure-cli-samples/blob/master/container-registry/service-principal-assign-role/service-principal-assign-role.sh -->
[!code-azurecli-interactive[acr-sp-role-assign](~/cli_scripts/container-registry/service-principal-assign-role/service-principal-assign-role.sh)]

<!-- LINKS - Internal -->
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
