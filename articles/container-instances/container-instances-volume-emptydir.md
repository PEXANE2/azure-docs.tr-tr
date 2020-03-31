---
title: Boşolan hacmi konteyner grubuna monte etme
description: Azure Kapsayıcı Örnekleri'ndeki bir kapsayıcı grubundaki kapsayıcılar arasında veri paylaşmak için boşdir hacmini nasıl monte edebilirsiniz öğrenin
ms.topic: article
ms.date: 01/31/2020
ms.openlocfilehash: 64a3c83008f163167528a5e5987fe2316942d5bc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77117744"
---
# <a name="mount-an-emptydir-volume-in-azure-container-instances"></a>Azure Kapsayıcı Örneklerinde boşdir hacmini takma

Azure Kapsayıcı Örnekleri'ndeki bir kapsayıcı grubundaki kapsayıcılar arasında veri paylaşmak için *boş* dir hacmini nasıl monte edebilirsiniz öğrenin. Konteynerleştirilmiş iş yüklerin için geçici önbellek ler olarak *emptyDir* hacimlerini kullanın.

> [!NOTE]
> Boş bir *Dir* hacmine montaj şu anda Linux kapsayıcıları ile sınırlıdır. Tüm özellikleri Windows kapsayıcılarına getirmek için çalışırken, güncel platform farklılıklarını [genel bakışta](container-instances-overview.md#linux-and-windows-containers)bulabilirsiniz.

## <a name="emptydir-volume"></a>emptyDir hacmi

*emptyDir* birimi, bir kapsayıcı grubundaki her kapsayıcıiçin erişilebilir bir yazılabilir dizin sağlar. Gruptaki kapsayıcılar ses biriminde aynı dosyaları okuyabilir ve yazabilir ve her kapsayıcıda aynı veya farklı yollar kullanılarak monte edilebilir.

Bazı *örnek, boşdir* birimi için kullanır:

* Scratch alanı
* Uzun süren görevlerde denetim noktası
* Bir kenar araba kapsayıcısı tarafından alınan ve uygulama kapsayıcısı tarafından sunulan verileri depolama

*Boşbir dir* hacmindeki veriler konteyner çökmeleri ile devam eder. Ancak yeniden başlatılan kapsayıcıların verileri *boşdir* biriminde devam etmesi garanti edilmez. Bir kapsayıcı grubunu durdurursanız, *boşDir* hacmi kalıcı değildir.

Bir Linux *emptyDir* hacminin maksimum boyutu 50 GB'dır.

## <a name="mount-an-emptydir-volume"></a>Boşbir Dir hacmini monte edin

Bir kapsayıcı örneğine boşdir birimi takmak için, bir Azure [Kaynak Yöneticisi şablonu](/azure/templates/microsoft.containerinstance/containergroups),YAML [dosyası](container-instances-reference-yaml.md)veya kapsayıcı grubunu dağıtmak için diğer programlı yöntemleri kullanarak dağıtabilirsiniz.

İlk olarak, `volumes` diziyi dosyanın `properties` kapsayıcı grubu bölümünde doldurun. Ardından, *boşDir* hacmini monte etmek istediğiniz konteyner grubundaki her kapsayıcı için, diziyi `volumeMounts` konteyner tanımının `properties` bölümünde doldurun.

Örneğin, aşağıdaki Kaynak Yöneticisi şablonu, her biri *boşDir* birimine monte edilen iki kapsayıcıdan oluşan bir kapsayıcı grubu oluşturur:

<!-- https://github.com/Azure/azure-docs-json-samples/blob/master/container-instances/aci-deploy-volume-emptydir.json -->
[!code-json[volume-emptydir](~/azure-docs-json-samples/container-instances/aci-deploy-volume-emptydir.json)]

Kapsayıcı grubu dağıtımı örneklerini görmek için kaynak [yöneticisi şablonu kullanarak çok kapsayıcılı bir grup dağıt](container-instances-multi-container-group.md) ve [YAML dosyasını kullanarak çok kapsayıcılı bir grup dağıt'a](container-instances-multi-container-yaml.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Azure Kapsayıcı Örnekleri'nde diğer birim türlerini nasıl monte edebilirsiniz öğrenin:

* [Azure Container Instances'ta Azure dosya paylaşımı bağlama](container-instances-volume-azure-files.md)
* [Azure Kapsayıcı Örneklerinde gitRepo hacmini takma](container-instances-volume-gitrepo.md)
* [Azure Kapsayıcı Örneklerinde gizli bir ses birimi takma](container-instances-volume-secret.md)