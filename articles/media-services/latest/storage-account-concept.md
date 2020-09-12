---
title: Azure depolama hesapları
titleSuffix: Azure Media Services
description: Azure Media Services ile kullanmak için bir Azure depolama hesabı oluşturmayı öğrenin.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: conceptual
ms.date: 08/31/2020
ms.author: inhenkel
ms.openlocfilehash: f37b453a294a0d0a7b9a99bfebe8f3eff09e8956
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89291203"
---
# <a name="azure-storage-accounts"></a>Azure Depolama hesapları

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Azure 'da medya içeriğini yönetmeye, şifrelemeye, kodlamaya, çözümlemeye ve akışla başlamak için bir Media Services hesabı oluşturmanız gerekir. Media Services hesabı oluştururken, bir Azure Depolama hesabı kaynağının adını sağlamanız gerekir. Belirtilen depolama hesabı, Media Services hesabınıza eklenir.

Media Services hesabı ve onunla ilişkili tüm depolama hesaplarının aynı Azure aboneliğinde olması gerekir. Ek gecikme ve veri çıkışı maliyetlerini önlemek için, depolama hesaplarının Media Services hesabıyla aynı konumda kullanılması önemle önerilir.

Bir **birincil** depolama hesabınız olmalıdır ve Media Services hesabınızla Ilişkili birkaç **İkincil** depolama hesabı olabilir. Media Services, **General-purpose v2** (GPv2) veya **General-purpose v1** (GPv1) hesaplarını destekler. Yalnızca blob hesaplarına **birincil**olarak izin verilmez.

En son özellikler ve performans avantajlarından yararlanabilmek için GPv2 kullanmanızı öneririz. Depolama hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../../storage/common/storage-account-overview.md).

> [!NOTE]
> Yalnızca sık erişimli erişim katmanı Azure Media Services ile kullanım için desteklenir, ancak diğer erişim katmanları etkin bir şekilde kullanılmamış içerikte depolama maliyetlerini azaltmak için kullanılabilir.

Depolama hesabınız için seçebileceğiniz farklı SKU 'Lar vardır. Daha fazla bilgi için bkz. [depolama hesapları](/cli/azure/storage/account?view=azure-cli-latest). Depolama hesaplarıyla denemek istiyorsanız kullanın `--sku Standard_LRS` . Ancak, üretim için bir SKU seçerken, `--sku Standard_RAGRS` iş sürekliliği için coğrafi çoğaltma sağlayan göz önünde bulundurmanız gerekir.

## <a name="assets-in-a-storage-account"></a>Depolama hesabındaki varlıklar

Media Services v3 'de, depolama API 'Leri dosyaları varlıklara yüklemek için kullanılır. Daha fazla bilgi için bkz. [Azure Media Services v3 Içindeki varlıklar](assets-concept.md).

> [!Note]
> Media Services API 'Leri kullanılmadan Media Services SDK 'Sı tarafından oluşturulan blob kapsayıcılarının içeriğini değiştirmeyi denemeyin.

## <a name="storage-side-encryption"></a>Depolama tarafı şifrelemesi

Varlıkları bekleyen bir şekilde korumak için, varlıkların depolama tarafı şifrelemesi tarafından şifrelenmesi gerekir. Aşağıdaki tabloda, Media Services v3 'de depolama tarafı şifrelemesinin nasıl çalıştığı gösterilmektedir:

|Şifreleme seçeneği|Description|Media Services v3|
|---|---|---|
|Media Services depolama şifrelemesi| AES-256 şifrelemesi, anahtar Media Services tarafından yönetiliyor. |Desteklenmez. <sup>(1)</sup>|
|[Bekleyen veriler için depolama hizmeti şifrelemesi](../../storage/common/storage-service-encryption.md)|Azure depolama tarafından sunulan ve Azure tarafından yönetilen veya müşteri tarafından yönetilen sunucu tarafı şifrelemesi.|Destekleniyor.|
|[Depolama istemci tarafı şifrelemesi](../../storage/common/storage-client-side-encryption.md)|Azure depolama tarafından sunulan ve Key Vault ' de müşteri tarafından yönetilen anahtar olan istemci tarafı şifreleme.|Desteklenmez.|

<sup>1</sup> Media Services v3 'de, depolama ŞIFRELEMESI (AES-256 şifrelemesi) yalnızca, varlıklarınız Media Services V2 ile oluşturulduysa, bu da v3 'in mevcut depolama şifreli varlıklarıyla çalıştığı ancak yeni olanların oluşturulmasına izin vermediği durumlarda geriye dönük uyumluluk için desteklenir.

## <a name="storage-account-errors"></a>Depolama hesabı hataları

Media Services hesabının durumunun "Bağlantı kesildi" olması, depolama erişim anahtarlarının değiştirilmesi nedeniyle hesabın bağlı depolama hesaplarından birine veya daha fazlasına erişim sahibi olmadığını gösterir. Media Services hesabındaki birçok görevin gerçekleştirilmesi için güncel depolama erişim anahtarlarına ihtiyaç duyulur.

Media Services hesabının bağlı depolama hesaplarına erişim sahibi olmamasına neden olabilecek temel senaryolar aşağıda verilmiştir.

|Sorun|Çözüm|
|---|---|
|Media Services hesabı veya bağlı depolama hesapları ayrı aboneliklere geçirildi. |Depolama hesaplarını veya Media Services hesabını aynı abonelikte olacak şekilde geçirin. |
|Media Services hesabı, farklı bir abonelikteki bağlı depolama hesabını kullanıyor ve bu duruma eski Media Services hesaplarında izin veriliyordu. Tüm erken Media Services hesapları modern Azure kaynakları yöneticisi tabanlı hesaplara dönüştürüldü ve bağlantısı kesilmiş duruma sahip olacak. |Depolama hesabını veya Media Services hesabını aynı abonelikte olacak şekilde geçirin.|

## <a name="azure-storage-firewall"></a>Azure depolama güvenlik duvarı

Azure Media Services, Azure Storage güvenlik duvarı veya [Özel uç noktaları](../../storage/common/storage-network-security.md) etkin olan depolama hesaplarını desteklemez.

## <a name="next-steps"></a>Sonraki adımlar

Media Services hesabınıza bir depolama hesabı eklemeyi öğrenmek için bkz. [Hesap oluşturma](./create-account-howto.md).
