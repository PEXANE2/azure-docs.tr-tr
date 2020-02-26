---
title: Azure Media Services hesapları olan Azure depolama hesapları | Microsoft Docs
description: Media Services hesabı oluştururken, bir Azure Depolama hesabı kaynağının adını sağlamanız gerekir. Belirtilen depolama hesabı, Media Services hesabınıza eklenir.
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
ms.openlocfilehash: 4bbadd7e10f0fd6896932dd79a5ca42d9906d2a2
ms.sourcegitcommit: 0cc25b792ad6ec7a056ac3470f377edad804997a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77602142"
---
# <a name="azure-storage-accounts"></a>Azure depolama hesapları

Azure 'da medya içeriğini yönetmeye, şifrelemeye, kodlamaya, çözümlemeye ve akışla başlamak için bir Media Services hesabı oluşturmanız gerekir. Media Services hesabı oluştururken, bir Azure Depolama hesabı kaynağının adını sağlamanız gerekir. Belirtilen depolama hesabı, Media Services hesabınıza eklenir. 

Media Services hesabı ve onunla ilişkili tüm depolama hesaplarının aynı Azure aboneliğinde olması gerekir. Ek gecikme ve veri çıkışı maliyetlerinden kaçınmak için, Media Services hesabıyla aynı konumdaki depolama hesaplarının kullanılması önemle önerilir

Tek bir **Birincil** depolama hesabınız olması gerekir, ancak Media Services hesabınızla ilişkili istediğiniz sayıda **İkincil** depolama hesabınız olabilir. Media Services, **General-purpose v2** (GPv2) veya **General-purpose v1** (GPv1) hesaplarını destekler. <br/>Yalnızca blob hesaplarının **Birincil** olmasına izin verilmez. 

En son özellikler ve performans avantajlarından yararlanabilmek için GPv2 kullanmanızı öneririz. Depolama hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../../storage/common/storage-account-overview.md).

> [!NOTE]
> Yalnızca sık erişimli erişim katmanı Azure Media Services ile kullanım için desteklenir, ancak diğer erişim katmanları etkin bir şekilde kullanılmayan içerikte depolama maliyetlerini azaltmak için kullanılabilir.

Depolama hesabınız için seçebileceğiniz farklı SKU 'Lar vardır. Daha fazla bilgi için bkz. [depolama hesapları](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest). Depolama hesaplarıyla denemek istiyorsanız `--sku Standard_LRS`kullanın. Ancak, üretim için bir SKU seçerken, iş sürekliliği için coğrafi çoğaltma sağlayan `--sku Standard_RAGRS`göz önünde bulundurmanız gerekir. 

## <a name="assets-in-a-storage-account"></a>Depolama hesabındaki varlıklar

Media Services v3 'de, depolama API 'Leri dosyaları varlıklara yüklemek için kullanılır. Daha fazla bilgi için bkz. [varlık kavramı](assets-concept.md).

> [!Note]
> Media Services API 'Lerini kullanmadan Media Services SDK 'Sı tarafından oluşturulan blob kapsayıcılarının içeriğini değiştirmeyi denememelisiniz.
 
## <a name="storage-side-encryption"></a>Depolama tarafında şifreleme

Bekleyen veri varlıklarınızı korumanın varlıklar tarafından depolama tarafı şifrelemesi şifrelenmelidir. Aşağıdaki tabloda, depolama tarafı şifrelemesi Media Services v3 sürümünde nasıl çalıştığı gösterilmektedir:

|Şifreleme seçeneği|Açıklama|Media Services v3|
|---|---|---|
|Media Services'ı depolama şifrelemesi| Media Services tarafından yönetilen AES-256 şifreleme anahtarı|Desteklenmiyor<sup>(1)</sup>|
|[Bekleyen veriler için Depolama Hizmeti Şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Sunucu tarafı şifrelemesi, Azure Depolama tarafından sunulan anahtarı Azure tarafından veya müşteri tarafından yönetilen|Destekleniyor|
|[Depolama Istemci tarafı şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Azure depolama, anahtar Kasası'nda müşteri tarafından yönetilen bir kiracı anahtarı tarafından sunulan istemci tarafı şifreleme|Desteklenmiyor|

<sup>1</sup> Media Services v3 'de, depolama ŞIFRELEMESI (AES-256 şifrelemesi) yalnızca varlıklarınız Media Services V2 ile oluşturulduysa geriye dönük uyumluluk için desteklenir. Var olan depolama ile v3 çalışır anlamı varlıklar şifreli ancak yenilerini oluşturulmasına izin vermez.

## <a name="storage-account-errors"></a>Depolama hesabı hataları

Media Services hesabının durumunun "Bağlantı kesildi" olması, depolama erişim anahtarlarının değiştirilmesi nedeniyle hesabın bağlı depolama hesaplarından birine veya daha fazlasına erişim sahibi olmadığını gösterir. Media Services hesabındaki birçok görevin gerçekleştirilmesi için güncel depolama erişim anahtarlarına ihtiyaç duyulur.

Media Services hesabının bağlı depolama hesaplarına erişim sahibi olmamasına neden olabilecek temel senaryolar aşağıda verilmiştir. 

|Sorun|Çözüm|
|---|---|
|Media Services hesabı veya bağlı depolama hesapları ayrı aboneliklere geçirildi. |Depolama hesaplarını veya Media Services hesabını geçirerek ikisinin aynı abonelikte olmasını sağlayın. |
|Media Services hesabı, farklı bir abonelikteki bağlı depolama hesabını kullanıyor ve bu duruma eski Media Services hesaplarında izin veriliyordu. Tüm eski Media Services hesapları modern Azure Resource Manager (ARM) tabanlı hesaplara dönüştürüldü ve durumları "Bağlantı kesildi" şeklinde olacak. |Depolama hesabını veya Media Services hesabını geçirerek ikisinin aynı abonelikte olmasını sağlayın.|

## <a name="azure-storage-firewall"></a>Azure depolama güvenlik duvarı

Azure Media Services, Azure Storage güvenlik duvarı veya [Özel uç noktaları](https://docs.microsoft.com/azure/storage/common/storage-network-security) etkin olan depolama hesaplarını desteklemez.

## <a name="next-steps"></a>Sonraki adımlar

Media Services hesabınıza bir depolama hesabı eklemeyi öğrenmek için bkz. [Hesap oluşturma](create-account-cli-quickstart.md).
