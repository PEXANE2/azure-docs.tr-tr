---
title: EmptyDir birimini kapsayıcı grubuna bağla
description: Azure Container Instances bir kapsayıcı grubundaki kapsayıcılar arasında veri paylaşmak için emptyDir birimini nasıl bağlayacağınızı öğrenin
ms.topic: article
ms.date: 01/31/2020
ms.openlocfilehash: 64a3c83008f163167528a5e5987fe2316942d5bc
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77117744"
---
# <a name="mount-an-emptydir-volume-in-azure-container-instances"></a>Azure Container Instances bir emptyDir birimi bağlama

Azure Container Instances bir kapsayıcı grubundaki kapsayıcılar arasında veri paylaşmak için *Emptydir* birimini nasıl bağlayacağınızı öğrenin. Kapsayıcılı iş yükleriniz için *Emptydir* birimlerini kısa ömürlü önbellekler olarak kullanın.

> [!NOTE]
> Bir *Emptydir* birimi bağlamak Şu anda Linux kapsayıcılarıyla kısıtlıdır. Tüm özellikleri Windows kapsayıcılarına getirmek için çalıştık, ancak geçerli platform farklarını [genel bakışta](container-instances-overview.md#linux-and-windows-containers)bulabilirsiniz.

## <a name="emptydir-volume"></a>emptyDir birimi

*Emptydir* birimi bir kapsayıcı grubundaki her bir kapsayıcıya erişilebilen yazılabilir bir dizin sağlar. Gruptaki kapsayıcılar birimdeki aynı dosyaları okuyabilir ve yazabilir ve her kapsayıcıda aynı veya farklı yollar kullanılarak bağlanabilir.

Bir *Emptydir* birimi için bazı örnekler kullanılmıştır:

* Karalama alanı
* Uzun süre çalışan görevler sırasında işaret noktası oluşturma
* Bir sepet kapsayıcısı tarafından alınan ve uygulama kapsayıcısı tarafından sunulan verileri depolayın

*Emptydir* birimindeki veriler, kapsayıcı Kilitlenmelerinde kalıcı hale getirilir. Ancak yeniden başlatılan kapsayıcılar, verileri bir *Emptydir* biriminde kalıcı hale getirmek için garanti edilmez. Bir kapsayıcı grubunu durdurursanız *Emptydir* birimi kalıcı olmaz.

Linux *Emptydir* biriminin en büyük boyutu 50 GB 'dir.

## <a name="mount-an-emptydir-volume"></a>EmptyDir birimi bağlama

Bir dizin birimini bir kapsayıcı örneğine bağlamak için, bir [Azure Resource Manager şablonu](/azure/templates/microsoft.containerinstance/containergroups), [YAML dosyası](container-instances-reference-yaml.md)ya da bir kapsayıcı grubunu dağıtmak için diğer programlı yöntemler kullanarak dağıtabilirsiniz.

İlk olarak, dosyanın `volumes` kapsayıcı grubu `properties` bölümünde diziyi doldurun. Ardından, `volumeMounts` *Dizin birimini bağlamak* istediğiniz kapsayıcı grubundaki her bir kapsayıcı için, diziyi kapsayıcı tanımının `properties` bölümünde doldurun.

Örneğin, aşağıdaki Kaynak Yöneticisi şablonu, her biri *Emptydir* birimini bağlayan iki kapsayıcıyı içeren bir kapsayıcı grubu oluşturur:

<!-- https://github.com/Azure/azure-docs-json-samples/blob/master/container-instances/aci-deploy-volume-emptydir.json -->
[!code-json[volume-emptydir](~/azure-docs-json-samples/container-instances/aci-deploy-volume-emptydir.json)]

Kapsayıcı grubu dağıtımına ait örnekleri görmek için bkz. [Kaynak Yöneticisi şablonu kullanarak çok kapsayıcılı bir grup dağıtma](container-instances-multi-container-group.md) ve [YAML dosyası kullanarak çok kapsayıcılı bir grup dağıtma](container-instances-multi-container-yaml.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure Container Instances diğer birim türlerini nasıl bağlayacağınızı öğrenin:

* [Azure Container Instances'ta Azure dosya paylaşımı bağlama](container-instances-volume-azure-files.md)
* [Azure Container Instances bir gitRepo birimi bağlama](container-instances-volume-gitrepo.md)
* [Azure Container Instances bir gizli birimi bağlama](container-instances-volume-secret.md)