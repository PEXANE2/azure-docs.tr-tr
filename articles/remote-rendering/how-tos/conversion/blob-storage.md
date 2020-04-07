---
title: Model dönüştürme için Azure Blob Depolama'yı kullanma
description: Model dönüştürme için blob depolamasını kurmak ve kullanmak için ortak adımları açıklar.
author: jakrams
ms.author: jakras
ms.date: 02/04/2020
ms.topic: how-to
ms.openlocfilehash: 6f0605077bd131c54f27e3bf46240331557fd92e
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80681655"
---
# <a name="use-azure-blob-storage-for-model-conversion"></a>Model dönüştürme için Azure Blob Depolama'yı kullanma

[Model dönüştürme](model-conversion.md) hizmeti, giriş verilerini alabilmek ve çıktı verilerini depolamak için Azure blob depolamasına erişim gerektirir. Bu makalede, en yaygın adımların nasıl yapılacağını açıklanmaktadır.

## <a name="prepare-azure-storage-accounts"></a>Azure Depolama hesapları hazırlama

- Depolama hesabı oluşturma (StorageV2)
- Depolama hesabında bir giriş blob kapsayıcısı oluşturma (örneğin"arrinput" adlı)
- Depolama hesabında bir çıktı blob kapsayıcısı oluşturma (örneğin"arroutput" olarak adlandırılır)

> [!TIP]
> Depolama hesabınızı nasıl ayarlayabilirsiniz adım adım talimatlar [için Quickstart:Render için bir modeli dönüştürün](../../quickstarts/convert-model.md)

Depolama hesabı nın ve blob kaplarının oluşturulması aşağıdaki araçlardan biriyle yapılabilir:

- [Azure portal](https://portal.azure.com)
- [az komut satırı](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
- [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)
- SDK'lar (C#, Python ... )

## <a name="ensure-azure-remote-rendering-can-access-your-storage-account"></a>Azure Uzaktan İşleme'nin depolama hesabınıza erişebilmesini sağlayın

Azure Remote Rending'in depolama hesabınızdan model verileri alması ve verileri ona geri yazması gerekir.

Azure Uzaktan İşleme'ye depolama hesabınıza aşağıdaki iki şekilde erişim izni verebilirsiniz:

### <a name="connect-your-azure-storage-account-with-your-azure-remote-rendering-account"></a>Azure Depolama hesabınızı Azure Uzaktan İşlem Eki hesabınıza bağlayın

[Hesap Oluştur](../create-an-account.md#link-storage-accounts) bölümünde verilen adımları izleyin.

### <a name="retrieve-sas-for-the-storage-containers"></a>Depolama kapları için SAS'ı alın

Depolanan erişim imzaları (SAS), giriş için okuma erişimi sağlamak ve çıktı için erişim yazmak için kullanılır. Bir model her dönüştürüldüğünde yeni URI'ler oluşturmanızı öneririz. URI'ler bir süre sonra sona erdiğinden, bunları daha uzun süre devam etmek uygulamanızı beklenmedik bir şekilde bozabilir.

SAS ile ilgili ayrıntılar [SAS belgelerinde](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)bulunabilir.

Bir SAS URI biri kullanılarak oluşturulabilir:

- az PowerShell modülü
  - örnek [PowerShell komut dosyasına](../../samples/powershell-example-scripts.md) bakın
- [az komut satırı](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
- [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)
  - "Paylaşılan Erişim İmzası Al" konteynerine sağ tıklayın (okuma, giriş kapsayıcısı için liste erişimi, çıkış kapsayıcısı için erişim yazın)
- SDK'lar (C#, Python ... )

Varlık dönüştürmede Paylaşılan Erişim İmzalarını kullanmanın bir örneği [Powershell Örnek Komut Dosyalarının](../../samples/powershell-example-scripts.md#script-conversionps1)Conversion.ps1'inde gösterilir.

## <a name="upload-an-input-model"></a>Giriş modeli yükleme

Bir modeli dönüştürmeye başlamak için, aşağıdaki seçeneklerden birini kullanarak modeli yüklemeniz gerekir:

- [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) - azure blob depolamada dosyaları yüklemek/indirmek/yönetmek için kullanışlı bir kullanıcı arabirimi
- [Azure komut satırı](https://docs.microsoft.com/azure/storage/common/storage-azure-cli)
- [Azure PowerShell modülü](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-2.2.0)
  - Örnek [PowerShell komut dosyalarına](../../samples/powershell-example-scripts.md) bakın
- [Depolama SDK kullanma (Python, C# ... )](https://docs.microsoft.com/azure/storage/)
- [Azure Depolama REST API'lerini kullanma](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api)

Dönüşüm için veri yükleme nin nasıl yüklendiğine bir örnek için [Powershell Örnek Komut Dosyalarının](../../samples/powershell-example-scripts.md#script-conversionps1)Conversion.ps1 bölümüne bakın.

## <a name="get-a-sas-uri-for-the-converted-model"></a>Dönüştürülmüş model için bir SAS URI alın

Bu adım, [depolama kapları için SAS almaya](#retrieve-sas-for-the-storage-containers)benzer. Ancak, bu kez çıkış kapsayıcısına yazılmış model dosyası için bir SAS URI almanız gerekir.

Örneğin, [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)üzerinden bir SAS URI almak için model dosyasına sağ tıklayın ve "Paylaşılan Erişim İmzasını Al"ı seçin.

Depolama hesabınızı Azure Uzaktan İşleme hesabınıza bağlamadıysanız, modelleri yüklemek için Paylaşılan Erişim İmzası (SAS) gereklidir. [Hesap Oluştur'da](../create-an-account.md#link-storage-accounts)hesabınızı nasıl bağlayabileceğinizi öğrenebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Model dönüşümyapılandırma](configure-model-conversion.md)
- [Model dönüştürme REST API](conversion-rest-api.md)
