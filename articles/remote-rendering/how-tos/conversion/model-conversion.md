---
title: Model dönüştürme
description: Bir modeli işleme için dönüştürme işlemini açıklar
author: jakrams
ms.author: jakras
ms.date: 02/04/2020
ms.topic: how-to
ms.openlocfilehash: db05c7a5a45221485ecb62c2c90d56be52d5ef48
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84808617"
---
# <a name="convert-models"></a>Modelleri dönüştürme

Azure uzaktan Işleme, çok karmaşık modeller oluşturmanıza olanak sağlar. En yüksek performans elde etmek için, verilerin en uygun biçimde olması için önceden işlenmesi gerekir. Veri miktarına bağlı olarak, bu adım biraz zaman alabilir. Bu süre, model yüklemesi sırasında harcandıysa pratik hale gelir. Ayrıca, birden çok oturum için bu işlemi tekrarlamak de beklenebilir. Bu nedenlerden dolayı ARR hizmeti, zaman içinde çalıştırabileceğiniz özel bir *dönüştürme hizmeti*sağlar.
Dönüştürüldükten sonra bir model Azure Storage hesabından yüklenebilir.

## <a name="supported-source-formats"></a>Desteklenen kaynak biçimleri

Dönüştürme hizmeti şu biçimleri destekler:

- **FBX** (sürüm 2011 ve üzeri)
- **Gltf** (sürüm 2. x)
- **GLB** (sürüm 2. x)

[Model biçimleri için bölüm malzeme eşleme](../../reference/material-mapping.md)bölümünde listelendiği gibi, malzemeler Özellik dönüştürmesiyle ilgili biçimler arasında küçük farklar vardır.

## <a name="the-conversion-process"></a>Dönüştürme işlemi

1. [Iki Azure Blob depolama kapsayıcısı hazırlayın](blob-storage.md): diğeri çıkış için bir giriş
1. Modelinizi giriş kapsayıcısına yükleme (isteğe bağlı olarak bir alt yol altında)
1. Dönüştürme işlemini [model dönüştürme](conversion-rest-api.md) aracılığıyla tetikleyin REST API
1. Dönüştürme işlemi için hizmeti yoklayın
1. İşiniz bittiğinde bir model yükleyin
    - bağlı bir depolama hesabından (depolama hesabınızı bağlamak için [Hesap oluşturma](../create-an-account.md#link-storage-accounts) hakkındaki "depolama hesaplarını bağlama" adımlarına bakın)
    - veya *paylaşılan erişim imzası (SAS)* sağlar.

Tüm model verileri (giriş ve çıkış) Kullanıcı tarafından sağlanmış Azure Blob depolama alanında depolanır. Azure uzaktan Işleme, varlık yönetibilme üzerinde tam denetim sağlar.

## <a name="pricing"></a>Fiyatlandırma

Dönüştürme fiyatlandırması hakkında daha fazla bilgi için, [Uzaktan işleme fiyatlandırma](https://azure.microsoft.com/pricing/details/remote-rendering) sayfasına bakın.


## <a name="conversion-parameters"></a>Dönüştürme parametreleri

Çeşitli dönüştürme seçenekleri için [Bu bölüme](configure-model-conversion.md)bakın.

## <a name="examples"></a>Örnekler

- [Hızlı başlangıç: bir modeli işleme Için dönüştürme](../../quickstarts/convert-model.md) , bir modeli nasıl dönüştürebileceğiniz adım adım bir tanıtım niteliğindedir.
- Dönüştürme hizmetinin kullanımını gösteren [örnek PowerShell betikleri](../../samples/powershell-example-scripts.md), *betikler* klasöründeki [ARR örnekleri deposunda](https://github.com/Azure/azure-remote-rendering) bulunabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Model dönüştürmesi için Azure Blob depolamayı kullanma](blob-storage.md)
- [Model dönüştürme REST API](conversion-rest-api.md)
- [Model dönüştürmeyi yapılandırma](configure-model-conversion.md)
- [Model biçimleri için malzeme eşleme](../../reference/material-mapping.md)
