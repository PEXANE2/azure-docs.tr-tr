---
title: Azure Bağlı Makine Aracısı CLI arabirimi
description: Azure Bağlı Makine aracısı CLI için başvuru belgeleri
author: bobbytreed
manager: carmonm
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
ms.topic: reference
ms.date: 11/04/2019
ms.author: robreed
ms.openlocfilehash: d35c5e283f2e1e2f8afd431d83775167dc2a531a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73513204"
---
# <a name="azure-connected-machine-agent-cli-interface"></a>Azure Bağlı Makine Aracısı CLI arabirimi

`Azcmagent` (Azure Bağlı Makine Aracısı) aracı, Azure'a azure olmayan bir makine bağlantısını yapılandırmak ve sorun gidermek için kullanılır.

Aracının kendisi Linux'ta çağrılan `himdsd` bir daemon işlemidir ve Windows'da çağrılan `himds` bir Windows Hizmetidir.

Normal kullanımda, `azcmagent connect` bu makine ile Azure arasında bir `azcmagent disconnect` bağlantı kurmak için kullanılır ve bu bağlantıyı artık istemediğinize karar verirseniz. Diğer komutlar sorun giderme veya diğer özel durumlar içindir.

## <a name="options"></a>Seçenekler

```none
  -h, --help      help for azcmagent
  -v, --verbose   Increase logging verbosity to show all logs
```

## <a name="see-also"></a>AYRıCA BAKıNıZ

* [azcmagent connect](#azcmagent-connect) - Bu makineyi Azure'a bağlar
* [azcmagent bağlantısı -](#azcmagent-disconnect) Bu makineyi Azure'dan keser
* [azcmagent yeniden bağlanma](#azcmagent-reconnect) - Bu makineyi Azure'a yeniden bağlar
* [azcmagent show](#azcmagent-show) - Makine meta veri ve Ajan durumunu alır. Bu, öncelikle sorun giderme için yararlıdır.
* [azcmagent sürümü](#azcmagent-version) - Hybrid Management Agent sürümünü görüntüle

## <a name="azcmagent-connect"></a>azcmagent bağlamak

Bu makineyi Azure'a bağlar

### <a name="synopsis"></a>Özet

Azure'da bu makineyi temsil eden bir kaynak oluşturur.

Bu, bu makineyi temsil eden Azure Kaynak Yöneticisi'nde bir kaynak oluşturmak için sağlanan kimlik doğrulama seçeneklerini kullanır. Kaynak istenen abonelik ve kaynak grubundadır ve makineyle ilgili veriler konum parametresi tarafından belirtilen Azure bölgesinde depolanır.
Varsayılan kaynak adı, geçersiz kılınmış değilse bu makinenin ana bilgisayar adıdır.

Bu makinenin Sistem Lekânlı Kimliğine karşılık gelen bir sertifika daha sonra indirilir ve yerel olarak depolanır. Bu adım tamamlandığında **Azure Bağlı Makine Meta Veri** Hizmeti ve Konuk Yapılandırma Aracısı Azure bulutuyla eşitlemeye başlar.

Kimlik doğrulama seçenekleri:

* Erişim Belirteci`azcmagent connect --access-token <> --subscription-id <> --resource-group <> --location <>`
* Hizmet Müdürü Kimliği ve gizli`azcmagent connect --service-principal-id <> --service-principal-secret <> --tenant-id <tenantid> --subscription-id <> --resource-group <> --location <>`
* Aygıt oturum açma (Etkileşimli)`azcmagent connect --tenant-id <> --subscription-id <> --resource-group <> --location <>`

### <a name="syntax"></a>Sözdizimi

```none
azcmagent connect [flags]
```

### <a name="options"></a>Seçenekler

```none
      --access-token string               Access token
  -h, --help                              help for connect
  -l, --location string                   Location of the resource [Required]
      --physical-location string          Physical location of the resource
  -g, --resource-group string             Name of the resource group. [Required]
  -n, --resource-name string              Name of the resource. Defaults to Host Name
  -i, --service-principal-id string       Service Principal Id
  -p, --service-principal-secret string   Service Principal Secret
  -s, --subscription-id string            Subscription Id [Required]
  -t, --tags string                       Tags for resource
      --tenant-id string                  Tenant Id
```

## <a name="azcmagent-disconnect"></a>azcmagent bağlantısı

Bu makineyi Azure'dan keser

### <a name="synopsis"></a>Özet

Azure'da bu sunucuyu temsil eden kaynağı siler.

Bu komut, bu makineyi temsil eden Azure Kaynak Yöneticisi kaynağını kaldırmak için sağlanan kimlik doğrulama seçeneklerini kullanır. Bu noktadan sonra **Azure Bağlı Makine Meta veri Hizmeti** ve Konuk Yapılandırma Aracısı kesilir. Bu komut hizmetleri durdurmaz veya kaldırmaz: bunu yapmak için paketi kaldırın.

Bu komut, "Azure Bağlı Makine Onboarding" rolünden daha yüksek ayrıcalıklar gerektirir.

Bir makinenin bağlantısı kesildikten `azcmagent reconnect` sonra, Azure'da bunun için yeni bir kaynak oluşturmak istemiyorsanız kullanın. `azcmagent connect`

Kimlik Doğrulama Seçenekleri:

* Erişim Belirteci`azcmagent disconnect --access-token <>`
* Hizmet Müdürü Kimliği ve gizli`azcmagent disconnect --service-principal-id <> --service-principal-secret <> --tenant-id <tenantid>`
* Etkileşimli Aygıt oturum açma`azcmagent disconnect --tenant-id <>`

### <a name="syntax"></a>Sözdizimi

```none
azcmagent disconnect [flags]
```

### <a name="options"></a>Seçenekler

```none
      --access-token string               Access token
  -h, --help                              help for disconnect
  -r, --resource-group string             Name of the resource group
  -n, --resource-name string              Name of the resource
  -i, --service-principal-id string       Service Principal Id
  -p, --service-principal-secret string   Service Principal Secret
  -s, --subscription-id string            Subscription Id
  -t, --tenant-id string                  Tenant Id
```

## <a name="azcmagent-reconnect"></a>azcmagent yeniden bağlanma

Bu makineyi Azure'a yeniden bağlar

### <a name="synopsis"></a>Özet

Geçersiz kimlik bilgilerine sahip makineyi Azure'a yeniden bağlayın.

Bir makinenin Azure'da zaten bir kaynağı varsa ancak bu makinenin kimliğini doğrulayamazsa, bu komut kullanılarak yeniden bağlanabilir. Bu, bir makinesertifikasının süresinin dolması için yeterince uzun süre kapalıysa (en az 45 gün) mümkündür.

Bir makineyle `azcmagent disconnect`bağlantısı kesildiyse, bunun yerine kullanın. `azcmagent connect`

Bu komut, bu makineyi temsil eden Azure Kaynak Yöneticisi kaynağına karşılık gelen yeni kimlik bilgilerini almak için sağlanan kimlik doğrulama seçeneklerini kullanır.

Bu komut, **Azure Bağlı Makine Onboarding** rolünden daha yüksek ayrıcalıklar gerektirir.

Kimlik Doğrulama Seçenekleri

* Erişim Belirteci`azcmagent reconnect --access-token <>`
* Hizmet Müdürü Kimliği ve gizli`azcmagent reconnect --service-principal-id <> --service-principal-secret <> --tenant-id <tenantid>`
* Etkileşimli Aygıt oturum açma`azcmagent reconnect --tenant-id <>`

### <a name="syntax"></a>Sözdizimi

```none
azcmagent reconnect [flags]
```

### <a name="options"></a>Seçenekler

```none
      --access-token string               Access token
  -h, --help                              help for reconnect
  -l, --location string                   Location of the resource
  -g, --resource-group string             Name of the resource group.
  -n, --resource-name string              Name of the resource. Defaults to Host Name
  -i, --service-principal-id string       Service Principal Id
  -p, --service-principal-secret string   Service Principal Secret
  -s, --subscription-id string            Subscription Id
      --tenant-id string                  tenant id
```

## <a name="azcmagent-show"></a>azcmagent gösterisi

Makine meta verilerini ve Aracı durumunu alır. Bu, öncelikle sorun giderme için yararlıdır.

### <a name="synopsis"></a>Özet

Makine meta verilerini ve Aracı durumunu alır. Bu, öncelikle sorun giderme için yararlıdır.


### <a name="syntax"></a>Sözdizimi

```
azcmagent show [flags]
```

### <a name="options"></a>Seçenekler

```
  -h, --help   help for show
```

## <a name="azcmagent-version"></a>azcmagent versiyonu

Hibrit Yönetim Aracısı sürümünü görüntüleme

### <a name="synopsis"></a>Özet

Hibrit Yönetim Aracısı sürümünü görüntüleme

### <a name="syntax"></a>Sözdizimi

```none
azcmagent version [flags]
```

### <a name="options"></a>Seçenekler

```none
  -h, --help   help for version
```
