---
title: Model dönüştürme
description: Oluşturma için bir modeli dönüştürme işlemini açıklar
author: jakrams
ms.author: jakras
ms.date: 02/04/2020
ms.topic: how-to
ms.openlocfilehash: 135c58a665779ffaad8750ffe618bdbe38639b66
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80681499"
---
# <a name="convert-models"></a>Modelleri dönüştürme

Azure Uzaktan İşleme, çok karmaşık modeller oluşturmanıza olanak tanır. Maksimum performans elde etmek için, verilerin en uygun biçimde olması için önceden işlenmesi gerekir. Veri miktarına bağlı olarak, bu adım biraz zaman alabilir. Bu süre model yükleme sırasında harcanan olsaydı, pratik olmaz. Ayrıca, birden çok oturum için bu işlemi tekrarlamak savurgan olacaktır. Bu nedenlerden dolayı, ARR hizmeti önceden çalıştırabileceğiniz özel bir *dönüşüm hizmeti*sağlar.
Dönüştürüldükten sonra, bir model Bir Azure Depolama Hesabından yüklenebilir.

## <a name="supported-source-formats"></a>Desteklenen kaynak biçimleri

Dönüştürme hizmeti şu biçimleri destekler:

- **FBX** (sürüm 2011 ve üzeri)
- **GLTF** (sürüm 2.x)
- **GLB** (sürüm 2.x)

Model biçimleri için bölüm [malzeme eşlemesinde](../../reference/material-mapping.md)listelenen malzeme özelliği dönüştürme ile ilgili biçimler arasında küçük farklılıklar vardır.

## <a name="the-conversion-process"></a>Dönüştürme işlemi

1. [İki Azure Blob Depolama kapsayıcısı hazırlayın](blob-storage.md): biri giriş için, diğeri çıktı için
1. Modelinizi giriş kapsayıcısına yükleyin (isteğe bağlı olarak bir alt yol altında)
1. [Model dönüştürme REST API](conversion-rest-api.md) ile dönüştürme işlemini tetikle
1. Dönüşüm ilerlemesi için hizmeti yoklama
1. Tamamlandıktan sonra, bir model yükleyin
    - bağlantılı bir depolama hesabından (bkz. depolama hesabınızı bağlamak için [Hesap Oluştur'daki](../create-an-account.md#link-storage-accounts) "Bağlantı depolama hesapları" adımları)
    - veya *Paylaşılan Erişim İmzası (SAS)* sağlayarak.

Tüm model verileri (giriş ve çıktı) kullanıcı tarafından sağlanan Azure blob depolamasında depolanır. Azure Uzaktan İşleme, varlık yönetiminiz üzerinde tam kontrol sağlar.

## <a name="conversion-parameters"></a>Dönüşüm parametreleri

Çeşitli dönüşüm seçenekleri için [bu bölüme](configure-model-conversion.md)bakın.

## <a name="examples"></a>Örnekler

- [Quickstart: Render için bir modeli dönüştürme,](../../quickstarts/convert-model.md) bir modeli dönüştürmenin nasıl yapılacağını adım adım başlatılır.
- Dönüşüm hizmetinin kullanımını gösteren [Örnek PowerShell komut dosyaları,](../../samples/powershell-example-scripts.md) *Komut Dosyaları* klasöründeki [ARR örnekleri deposunda](https://github.com/Azure/azure-remote-rendering) bulunabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Model dönüştürme için Azure Blob Depolama'yı kullanma](blob-storage.md)
- [Model dönüştürme REST API](conversion-rest-api.md)
- [Model dönüşümyapılandırma](configure-model-conversion.md)
- [Model biçimleri için malzeme eşleme](../../reference/material-mapping.md)
