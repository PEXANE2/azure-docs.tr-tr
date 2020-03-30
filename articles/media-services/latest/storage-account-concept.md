---
title: Azure depolama hesapları
titleSuffix: Azure Media Services
description: Azure Medya Hizmetleri ile kullanmak üzere bir Azure depolama hesabı oluşturmayı öğrenin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 07/01/2019
ms.author: juliako
ms.openlocfilehash: 72aa0762d001c28b21d5e27ed8f6f9d099f62bfb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79499828"
---
# <a name="azure-storage-accounts"></a>Azure Depolama hesapları

Azure'da medya içeriğini yönetmeye, şifrelemeye, kodlamaya, analiz etmeye ve akışa başlamak için bir Medya Hizmetleri hesabı oluşturmanız gerekir. Media Services hesabı oluştururken, bir Azure Depolama hesabı kaynağının adını sağlamanız gerekir. Belirtilen depolama hesabı, Media Services hesabınıza eklenir.

Media Services hesabı ve onunla ilişkili tüm depolama hesaplarının aynı Azure aboneliğinde olması gerekir. Ek gecikme ve veri çıkış maliyetlerini önlemek için Depolama hesaplarının Medya Hizmetleri hesabıyla aynı konumda kullanılması önerilir.

Bir **Birincil** depolama hesabınız olması gerekir ve Medya Hizmetleri hesabınızla ilişkili herhangi bir sayıda **İkincil** depolama hesabınız olabilir. Media Services, **General-purpose v2** (GPv2) veya **General-purpose v1** (GPv1) hesaplarını destekler. Blob yalnızca **hesaplarbirincil**olarak izin verilmez.

En son özelliklerden ve performanstan yararlanabilmeniz için GPv2 kullanmanızı öneririz. Depolama hesapları hakkında daha fazla bilgi edinmek için [Azure Depolama hesabına genel bakış'a](../../storage/common/storage-account-overview.md)bakın.

> [!NOTE]
> Diğer erişim katmanları etkin olarak kullanılmayan içeriklerin depolama maliyetlerini azaltmak için kullanılabilse de, Azure Medya Hizmetleri'nde yalnızca sıcak erişim katmanı desteklenir.

Depolama hesabınız için seçebileceğiniz farklı SUS'lar vardır. Daha fazla bilgi için [depolama hesaplarına](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest)bakın. Depolama hesaplarıyla denemeler yapmak istiyorsanız, 'yi kullanın. `--sku Standard_LRS` Ancak, üretim için bir SKU seçerken, iş sürekliliği için coğrafi çoğaltma sağlayan düşünmelisiniz. `--sku Standard_RAGRS`

## <a name="assets-in-a-storage-account"></a>Depolama hesabındaki varlıklar

Media Services v3'te, Depolama API'leri varlıkları yüklemek için kullanılır. Daha fazla bilgi için Azure [Medya Hizmetleri v3'teki Varlıklar v3'e](assets-concept.md)bakın.

> [!Note]
> Medya Hizmetleri API'leri kullanmadan Media Services SDK tarafından oluşturulan blob kapsayıcılarının içeriğini değiştirmeye çalışmayın.

## <a name="storage-side-encryption"></a>Depolama tarafı şifreleme

Varlıklarınızı istirahatte korumak için, varlıkların depolama tarafındaki şifreleme tarafından şifrelenmeleri gerekir. Aşağıdaki tablo, Media Services v3'te depolama tarafı şifrelemesinin nasıl çalıştığını gösterir:

|Şifreleme seçeneği|Açıklama|Media Services v3|
|---|---|---|
|Medya Hizmetleri depolama şifrelemesi| AES-256 şifreleme, Medya Hizmetleri tarafından yönetilen anahtar. |Desteklenmiyor. <sup>(1) sayılı</sup>|
|[Veriler için depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Azure Depolama tarafından sunulan sunucu tarafı şifrelemesi, anahtar Azure veya müşteri tarafından yönetilir.|Destekleniyor.|
|[Depolama istemci tarafı şifreleme](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Anahtar Vault'ta müşteri tarafından yönetilen Azure depolama tarafından sunulan istemci tarafı şifrelemesi.|Desteklenmiyor.|

<sup>1</sup> Media Services v3'te depolama şifrelemesi (AES-256 şifrelemesi), yalnızca varlıklarınız Media Services v2 ile oluşturulduğunda geriye dönük uyumluluk için desteklenir, bu da v3'ün mevcut depolama şifreli varlıklarıyla çalıştığı ancak yenilerinin oluşturulmasına izin vermeyeceği anlamına gelir.

## <a name="storage-account-errors"></a>Depolama hesabı hataları

Media Services hesabının durumunun "Bağlantı kesildi" olması, depolama erişim anahtarlarının değiştirilmesi nedeniyle hesabın bağlı depolama hesaplarından birine veya daha fazlasına erişim sahibi olmadığını gösterir. Media Services hesabındaki birçok görevin gerçekleştirilmesi için güncel depolama erişim anahtarlarına ihtiyaç duyulur.

Media Services hesabının bağlı depolama hesaplarına erişim sahibi olmamasına neden olabilecek temel senaryolar aşağıda verilmiştir.

|Sorun|Çözüm|
|---|---|
|Media Services hesabı veya bağlı depolama hesapları ayrı aboneliklere geçirildi. |Depolama hesabı(lar) veya Medya Hizmetleri hesabını, hepsinin aynı abonelikte olması için geçirin. |
|Media Services hesabı, farklı bir abonelikteki bağlı depolama hesabını kullanıyor ve bu duruma eski Media Services hesaplarında izin veriliyordu. Tüm erken Medya Hizmetleri hesapları modern Azure Kaynakları Yöneticisi tabanlı hesaplara dönüştürüldü ve bağlantısı kesilmiş bir duruma sahip olacak. |Depolama hesabının veya Medya Hizmetleri hesabını, hepsinin aynı abonelikte olması için geçirin.|

## <a name="azure-storage-firewall"></a>Azure Depolama güvenlik duvarı

Azure Medya Hizmetleri, Azure Depolama güvenlik duvarı veya [Özel Bitiş Noktaları](https://docs.microsoft.com/azure/storage/common/storage-network-security) etkinken depolama hesaplarını desteklemez.

## <a name="next-steps"></a>Sonraki adımlar

Medya Hizmetleri hesabınıza nasıl depolama hesabı ekleyeceklerini öğrenmek için [bkz.](create-account-cli-quickstart.md)
