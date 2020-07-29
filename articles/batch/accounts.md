---
title: Batch hesapları ve Azure depolama hesapları
description: Azure Batch hesapları ve bunların bir geliştirme açısından nasıl kullanılabilecekleri hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 05/12/2020
ms.openlocfilehash: 20a2a28d0eaa2c7997ea93e66d07ecb99bf297a2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83791151"
---
# <a name="batch-accounts-and-azure-storage-accounts"></a>Batch hesapları ve Azure depolama hesapları

Azure Batch hesap, Batch hizmeti içinde benzersiz olarak tanımlanmış bir varlıktır. Çoğu Batch çözümü, kaynak dosyalarını ve çıkış dosyalarını depolamak için [Azure Storage](../storage/index.yml) kullanır, bu nedenle her Batch hesabı genellikle karşılık gelen bir depolama hesabıyla ilişkilendirilir.

## <a name="batch-accounts"></a>Batch hesapları

Tüm işlem ve kaynaklar bir Batch hesabıyla ilişkilendirilir. Uygulamanız, Batch hizmetinden bir istekte bulunduğunda istek Azure Batch hesabı adı, hesabın URL'si ve erişim anahtarı veya Azure Active Directory belirteci kullanılarak kimlik doğrulamasından geçirilir.

Tek bir Batch hesabında birden fazla Batch iş yükü çalıştırabilirsiniz. İş yüklerinizi aynı abonelikte bulunan ancak farklı Azure bölgelerinde bulunan Batch hesapları arasında da dağıtabilirsiniz.

[!INCLUDE [batch-account-mode-include](../../includes/batch-account-mode-include.md)]

[Batch yönetimi .NET kitaplığı](batch-management-dotnet.md)ile gibi [Azure Portal](batch-account-create-portal.md) veya programlama yoluyla bir Batch hesabı oluşturabilirsiniz. Hesabı oluştururken işle ilgili giriş veya çıkış verilerini veya uygulamaları kaydetmek için bir Azure depolama hesabı ilişkilendirebilirsiniz.

## <a name="azure-storage-accounts"></a>Azure Depolama hesapları

Batch çözümlerinin çoğu, kaynak dosyalarını ve çıkış dosyalarını depolamak için Azure Depolama kullanır. Örneğin, Batch görevleriniz (standart görevler, başlangıç görevleri, iş hazırlama görevleri ve iş bırakma görevleri dahil) genellikle depolama hesabında yer alan kaynak dosyalarını belirtir. Depolama hesapları, işlenen verileri ve oluşturulan tüm çıktı verilerini de depolar.

Batch, aşağıdaki Azure Depolama hesap türlerini destekler:

- Genel amaçlı v2 (GPv2) hesapları
- Genel amaçlı v1 (GPv1) hesapları
- Blob depolama hesapları (şu anda Sanal Makine yapılandırmasındaki havuzlar için desteklenmektedir)

Depolama hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../storage/common/storage-account-overview.md).

Batch hesabını oluşturduğunuzda veya daha sonra Batch hesabınızla bir depolama hesabını ilişkilendirebilirsiniz. Bir depolama hesabı seçerken, maliyet ve performans gereksinimlerinizi göz önünde bulundurun. Örneğin, GPv2 ve blob depolama hesabı seçenekleri, GPv1’e kıyasla daha büyük [kapasite ve ölçeklenebilirlik sınırlarını](https://azure.microsoft.com/blog/announcing-larger-higher-scale-storage-accounts/) destekler. (Depolama limitinin artışını istemek için Azure desteğine başvurun.) Bu hesap seçenekleri, depolama hesabından okuyan veya depolama hesabından yazan çok sayıda paralel görev içeren Batch çözümlerinin performansını iyileştirebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Düğümler ve havuzlar](nodes-and-pools.md)hakkında bilgi edinin.
- [Azure Portal](batch-account-create-portal.md)kullanarak Batch hesabı oluşturmayı öğrenin.
