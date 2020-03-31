---
title: RBAC rolleri ve izinleri
description: Azure kapsayıcı kayıt defterindeki kaynaklara ince taneli izinler sağlamak için Azure rol tabanlı erişim denetimi (RBAC) ve kimlik ve erişim yönetimi (IAM) kullanın.
ms.topic: article
ms.date: 12/02/2019
ms.openlocfilehash: 3fb103ac4c4dac736b3c0fc99b2cf49f01e9e005
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74893493"
---
# <a name="azure-container-registry-roles-and-permissions"></a>Azure Kapsayıcı Kayıt Defteri rolleri ve izinleri

Azure Kapsayıcı Kayıt Defteri hizmeti, bir Azure kapsayıcı kayıt defterine farklı düzeyde izin sağlayan yerleşik [Azure rolleri](../role-based-access-control/built-in-roles.md) kümesini destekler. Kullanıcılara, hizmet ilkelerine veya bir kayıt defteriyle etkileşimde olması gereken diğer kimliklere belirli izinler atamak için Azure [rol tabanlı erişim denetimini](../role-based-access-control/index.yml) (RBAC) kullanın. 

| Rol /İzin       | [Access Kaynak Yöneticisi](#access-resource-manager) | [Kayıt defteri oluşturma/silme](#create-and-delete-registry) | [İtme görüntüsü](#push-image) | [Görüntüyü çekme](#pull-image) | [Resim verilerini silme](#delete-image-data) | [İlkeleri değiştirme](#change-policies) |   [İşaret görüntüleri](#sign-images)  |
| ---------| --------- | --------- | --------- | --------- | --------- | --------- | --------- |
| Sahip | X | X | X | X | X | X |  |  
| Katılımcı | X | X | X |  X | X | X |  |  
| Okuyucu | X |  |  | X |  |  |  |
| AcrPush |  |  | X | X | |  |  |  
| AcrPull |  |  |  | X |  |  |  |  
| AcrDelete |  |  |  |  | X |  |  |
| AcrImageSigner |  |  |  |  |  |  | X |

## <a name="differentiate-users-and-services"></a>Kullanıcıları ve hizmetleri ayırt etme

İzinler her zaman uygulandığında, en iyi yöntem, bir görevi gerçekleştirmek için bir kişi veya hizmet için en sınırlı izin kümesini sağlamaktır. Aşağıdaki izin kümeleri, insanlar ve başsız hizmetler tarafından kullanılabilecek bir dizi yeteneği temsil eder.

### <a name="cicd-solutions"></a>CI/CD çözümleri

CI/CD çözümlerinden komutları `docker build` otomatikleştirirken, `docker push` yeteneklere ihtiyacınız vardır. Bu başsız hizmet senaryoları **için, AcrPush** rolünü atamanızı öneririz. Bu rol, daha geniş **Katılımcı** rolünün aksine, hesabın diğer kayıt defteri işlemlerini gerçekleştirmesini veya Azure Kaynak Yöneticisi'ne erişmesini engeller.

### <a name="container-host-nodes"></a>Konteyner ana düğümleri

Aynı şekilde, kaplarınızı çalıştıran düğümlerin **AcrPull** rolüne ihtiyacı vardır, ancak **Reader** özelliklerini gerektirmemelidir.

### <a name="visual-studio-code-docker-extension"></a>Görsel Stüdyo Kodu Docker uzantısı

Visual Studio Code [Docker uzantısı](https://code.visualstudio.com/docs/azure/docker)gibi araçlar için kullanılabilir Azure kapsayıcı kayıt defterlerini listelemek için ek kaynak sağlayıcı erişimi gerekir. Bu durumda, kullanıcılarınızın **Okuyucu** veya **Katılımcı** rolüne erişmelerini sağlayın. Bu roller, `docker push` `az acr list`, `az acr build`, , ve diğer yetenekleri sağlar. `docker pull` 

## <a name="access-resource-manager"></a>Access Kaynak Yöneticisi

[Azure CLI](/cli/azure/)ile Azure portalı ve kayıt defteri yönetimi için Azure Kaynak Yöneticisi erişimi gereklidir. Örneğin, `az acr list` komutu kullanarak kayıt defteri listesini almak için bu izin kümesine ihtiyacınız var. 

## <a name="create-and-delete-registry"></a>Kayıt defteri oluşturma ve silme

Azure kapsayıcı kayıt defterlerini oluşturma ve silme yeteneği.

## <a name="push-image"></a>İtme görüntüsü

Bir görüntünün `docker push` veya Miğfer grafiği gibi desteklenen başka bir [yapının](container-registry-image-formats.md) kayıt defterine itme yeteneği. Yetkili kimliği kullanarak kayıt defteriile [kimlik doğrulaması](container-registry-authentication.md) gerektirir. 

## <a name="pull-image"></a>Görüntüyü çekme

Karantinaya `docker pull` alınamayan bir görüntüye sahip olma veya kayıt defterinden Helm grafiği gibi desteklenen başka bir [yapıyı](container-registry-image-formats.md) çekme yeteneği. Yetkili kimliği kullanarak kayıt defteriile [kimlik doğrulaması](container-registry-authentication.md) gerektirir.

## <a name="delete-image-data"></a>Resim verilerini silme

[Kapsayıcı görüntülerini silme](container-registry-delete.md)veya Miğfer grafikleri gibi desteklenen diğer [yapıları](container-registry-image-formats.md) bir kayıt defterinden silme yeteneği.

## <a name="change-policies"></a>İlkeleri değiştirme

Kayıt defterindeki ilkeleri yapılandırma yeteneği. İlkeler, görüntü temizleme, karantinayı etkinleştirme ve görüntü imzalamayı içerir.

## <a name="sign-images"></a>İşaret görüntüleri

Genellikle bir hizmet ilkesini kullanan otomatik bir işleme atanan görüntüleri imzalama yeteneği. Bu izin genellikle güvenilir bir görüntüyü kayıt defterine itmeye izin vermek için [push görüntüsüyle](#push-image) birleştirilir. Ayrıntılar için [Azure Kapsayıcı Kayıt Defteri'nde İçerik güvenine](container-registry-content-trust.md)bakın.

## <a name="custom-roles"></a>Özel roller

Diğer Azure kaynaklarında olduğu gibi, Azure Kapsayıcı Kayıt Defteri'ne ince taneli izinlerle kendi [özel rollerinizi](../role-based-access-control/custom-roles.md) oluşturabilirsiniz. Ardından, özel rolleri kullanıcılara, hizmet ilkelerine veya bir kayıt defteriyle etkileşimde olması gereken diğer kimliklere atayın. 

Özel bir role hangi izinlerin uygulanacağını belirlemek için Microsoft.ContainerRegistry [eylemlerinin](../role-based-access-control/resource-provider-operations.md#microsoftcontainerregistry)listesine bakın, [yerleşik ACR rollerinin](../role-based-access-control/built-in-roles.md)izin verilen eylemlerini inceleyin veya aşağıdaki komutu yürüt

```azurecli
az provider operation show --namespace Microsoft.ContainerRegistry
```

Özel bir rol tanımlamak [için, özel bir rol oluşturmak için Adımlar'a](../role-based-access-control/custom-roles.md#steps-to-create-a-custom-role)bakın.

> [!IMPORTANT]
> Azure Kapsayıcı Kayıt Defteri, özel bir rolde şu `Microsoft.ContainerRegistry/*` anda eşleşen tüm eylemlere erişim sağlayan joker karakterler gibi joker `Microsoft.ContainerRegistry/registries/*` karakterleri desteklemez. Rolde gerekli herhangi bir eylemi tek tek belirtin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure portalını](../role-based-access-control/role-assignments-portal.md), [Azure CLI'yi](../role-based-access-control/role-assignments-cli.md)veya diğer Azure araçlarını kullanarak RBAC rollerini Azure kimliğine atama hakkında daha fazla bilgi edinin.

* Azure Kapsayıcı Kayıt Defteri için [kimlik doğrulama seçenekleri](container-registry-authentication.md) hakkında bilgi edinin.

* Bir kapsayıcı kayıt [defterinde depo kapsamı yla ilgili izinleri](container-registry-repository-scoped-permissions.md) (önizleme) etkinleştirme hakkında bilgi edinin.