---
title: Model dönüştürmesi için Azure Blob depolamayı kullanma
description: Model dönüştürmesi için blob depolamayı ayarlamaya ve kullanmaya yönelik yaygın adımları açıklar.
author: jakrams
ms.author: jakras
ms.date: 02/04/2020
ms.topic: how-to
ms.openlocfilehash: 6f0605077bd131c54f27e3bf46240331557fd92e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80681655"
---
# <a name="use-azure-blob-storage-for-model-conversion"></a>Model dönüştürmesi için Azure Blob depolamayı kullanma

[Model dönüştürme](model-conversion.md) hizmeti, giriş verilerini almak ve çıkış verilerini depolamak için Azure Blob depolama alanına erişim gerektirir. Bu makalede en sık kullanılan adımların nasıl yapılacağı açıklanır.

## <a name="prepare-azure-storage-accounts"></a>Azure depolama hesaplarını hazırlama

- Depolama hesabı oluşturma (StorageV2)
- Depolama hesabında bir giriş blobu kapsayıcısı oluşturun (örneğin, "arrinput" adlı)
- Depolama hesabında bir çıkış blobu kapsayıcısı oluşturun (örneğin, "arroutput" adlı)

> [!TIP]
> Depolama hesabınızı ayarlama hakkında adım adım yönergeler için [hızlı başlangıç: bir modeli işleme Için dönüştürme](../../quickstarts/convert-model.md) bölümüne bakın

Depolama hesabı ve BLOB kapsayıcıları oluşturma, aşağıdaki araçlardan biriyle yapılabilir:

- [Azure portal](https://portal.azure.com)
- [az komut satırı](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
- [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)
- SDK 'lar (C#, Python...)

## <a name="ensure-azure-remote-rendering-can-access-your-storage-account"></a>Azure uzaktan Işleme 'nin depolama hesabınıza erişebildiğinden emin olun

Azure uzaktan saklama 'nın, depolama hesabınızdan model verilerini alması ve verileri buna geri yazması gerekir.

Aşağıdaki iki yolla depolama hesabınıza Azure uzaktan Işleme erişimi verebilirsiniz:

### <a name="connect-your-azure-storage-account-with-your-azure-remote-rendering-account"></a>Azure depolama hesabınızı Azure uzaktan Işleme hesabınızla bağlama

[Hesap oluşturma](../create-an-account.md#link-storage-accounts) bölümünde verilen adımları izleyin.

### <a name="retrieve-sas-for-the-storage-containers"></a>Depolama kapsayıcıları için SAS alma

Depolanan erişim imzaları (SAS), giriş için okuma erişimi sağlamak ve çıkış için yazma erişimi sağlamak üzere kullanılır. Her model dönüştürülürken yeni URI 'Ler oluşturmanızı öneririz. URI 'Lerin süresi bir süre sonra sona erdiğinden, daha uzun bir süre boyunca kalıcı hale getirerek uygulamanızı beklenmedik şekilde bozabilir.

SAS hakkındaki ayrıntılar [SAS belgelerinde](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)bulunabilir.

Bir SAS URI 'SI aşağıdakilerden biri kullanılarak oluşturulabilir:

- az PowerShell modülü
  - [örnek PowerShell betiklerine](../../samples/powershell-example-scripts.md) bakın
- [az komut satırı](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
- [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)
  - "paylaşılan erişim Imzasını al" kapsayıcısına sağ tıklama (okuma, giriş kapsayıcısı için erişimi listeleme, çıkış kapsayıcısı için yazma erişimi)
- SDK 'lar (C#, Python...)

Varlık dönüştürmesinde paylaşılan erişim Imzalarının kullanılmasına bir örnek, [PowerShell örnek betiklerinin](../../samples/powershell-example-scripts.md#script-conversionps1)dönüştürme. ps1 içinde gösterilir.

## <a name="upload-an-input-model"></a>Giriş modelini karşıya yükleme

Bir modeli dönüştürmeye başlamak için, aşağıdaki seçeneklerden birini kullanarak karşıya yüklemeniz gerekir:

- [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) -Azure Blob Storage 'da dosyaları karşıya yüklemek/indirmek/yönetmek için uygun bir kullanıcı arabirimi
- [Azure komut satırı](https://docs.microsoft.com/azure/storage/common/storage-azure-cli)
- [Azure PowerShell modülü](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-2.2.0)
  - [örnek PowerShell betiklerine](../../samples/powershell-example-scripts.md) bakın
- [Depolama SDK 'sını kullanma (Python, C#...)](https://docs.microsoft.com/azure/storage/)
- [Azure depolama REST API 'Lerini kullanma](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api)

Dönüştürme için verileri karşıya yükleme hakkında bir örnek için [PowerShell örnek betiklerinin](../../samples/powershell-example-scripts.md#script-conversionps1)dönüştürme. ps1 adresine bakın.

## <a name="get-a-sas-uri-for-the-converted-model"></a>Dönüştürülen model için SAS URI 'SI al

Bu adım, [depolama kapsayıcıları IÇIN SAS almaya](#retrieve-sas-for-the-storage-containers)benzer. Ancak, bu süre, çıkış kapsayıcısına yazılmış olan model dosyası için bir SAS URI 'SI almanız gerekir.

Örneğin, [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)aracılığıyla SAS URI 'sini almak için model dosyasına sağ tıklayın ve "paylaşılan erişim imzasını al" seçeneğini belirleyin.

Depolama hesabınızı Azure uzaktan Işleme hesabınıza eklemediğiniz takdirde yük modelleriyle paylaşılan erişim Imzası (SAS) gereklidir. [Hesap oluşturma](../create-an-account.md#link-storage-accounts)bölümünde hesabınızı nasıl bağlayabileceğinizi öğrenebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Model dönüştürmeyi yapılandırma](configure-model-conversion.md)
- [Model dönüştürme REST API](conversion-rest-api.md)
