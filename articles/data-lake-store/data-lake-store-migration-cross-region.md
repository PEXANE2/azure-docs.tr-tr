---
title: Azure Veri Gölü Depolama Gen1 bölgeler arası geçiş | Microsoft Dokümanlar
description: Azure Veri Gölü Depolama Gen1 için bölgeler arası geçiş hakkında bilgi edinin.
services: data-lake-store
documentationcenter: ''
author: swums
manager: amitkul
editor: swums
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: stewu
ms.openlocfilehash: 0bf0843314f38c0de28820c82e95b7921297bf40
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60518446"
---
# <a name="migrate-azure-data-lake-storage-gen1-across-regions"></a>Azure Veri Gölü Depolama Gen1'i bölgeler e göre geçirin

Azure Veri Gölü Depolama Gen1 yeni bölgelerde kullanıma sunulduğunda, yeni bölgeden yararlanmak için tek seferlik geçiş yapmayı seçebilirsiniz. Geçişi planlarken ve tamamlarken neleri göz önünde bulundurmanız gerektiğini öğrenin.

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**. Daha fazla bilgi için [bkz.](https://azure.microsoft.com/pricing/free-trial/)
* **İki farklı bölgede bir Veri Gölü Depolama Gen1 hesabı.** Daha fazla bilgi için bkz: [Azure Veri Gölü Depolama Gen1 ile başlayın.](data-lake-store-get-started-portal.md)
* **Azure Veri Fabrikası**. Daha fazla bilgi için bkz. [Azure Data Factory'ye giriş](../data-factory/introduction.md).


## <a name="migration-considerations"></a>Geçiş sırasında dikkat edilmesi gerekenler

İlk olarak, Veri Gölü Depolama Gen1'de veri yazan, okuyan veya işleyen uygulamanız için en iyi şekilde çalışan geçiş stratejisini belirleyin. Bir strateji seçtiğinizde, uygulamanızın kullanılabilirlik gereksinimlerini ve geçiş sırasında oluşan kapalı kalma süresini göz önünde bulundurun. Örneğin, en basit yaklaşımınız "kaldır ve kaydır" bulut geçiş modelini kullanmak olabilir. Bu yaklaşımda, tüm verileriniz yeni bölgeye kopyalanırken varolan bölgenizdeki uygulamayı duraklatılırsınız. Kopyalama işlemi tamamlandığında, başvurunuzu yeni bölgede devam ettirir ve ardından eski Veri Gölü Depolama Gen1 hesabını silersiniz. Geçiş sırasında kapalı kalma süresi gereklidir.

Kapalı kalma süresini azaltmak için, yeni bölgede hemen yeni veriler almaya başlayabilirsiniz. Gereken minimum veriye sahip olduğunuzda, uygulamanızı yeni bölgede çalıştırın. Arka planda, eski verileri mevcut Veri Gölü Depolama Gen1 hesabındaki yeni bölgedeki yeni Veri Gölü Depolama Gen1 hesabına kopyalamaya devam edin. Bu yaklaşımı kullanarak, çok az kapalı kalma süresi ile yeni bölgeye geçiş yapabilirsiniz. Tüm eski veriler kopyalandığında, eski Veri Gölü Depolama Gen1 hesabını silin.

Geçişinizi planlarken göz önünde bulundurulması gereken diğer önemli ayrıntılar şunlardır:

* **Veri hacmi**. Veri hacmi (gigabaytlarda, dosya ve klasör sayısı vb.) geçiş için gereken zamanı ve kaynakları etkiler.

* **Veri Gölü Depolama Gen1 hesap adı**. Yeni bölgedeki yeni hesap adı genel olarak benzersiz olmalıdır. Örneğin, Doğu ABD 2'deki eski Data Lake Storage Gen1 hesabınızın adı contosoeastus2.azuredatalakestore.net olabilir. Kuzey AB'deki yeni Veri Gölü Depolama Gen1 hesabınızı contosonortheu.azuredatalakestore.net.

* **Araçlar**. Veri Gölü Depolama Gen1 dosyalarını kopyalamak için [Azure Veri Fabrikası Kopyalama Etkinliği'ni](../data-factory/connector-azure-data-lake-store.md) kullanmanızı öneririz. Veri Fabrikası yüksek performans ve güvenilirlikle veri hareketini destekler. Veri Fabrikası'nın yalnızca klasör hiyerarşisini ve dosyaların içeriğini kopyaladığını unutmayın. Eski hesapta kullandığınız erişim denetim listelerini (ALA'lar) yeni hesaba el ile uygulamanız gerekir. En iyi durum senaryoları için performans hedefleri de dahil olmak üzere daha fazla bilgi için [Kopyalama Etkinliği performansı ve ayarı kılavuzuna](../data-factory/copy-activity-performance.md)bakın. Verilerin daha hızlı kopyalanmasını istiyorsanız, ek Bulut Veri Hareketi Birimleri kullanmanız gerekebilir. AdlCopy gibi diğer bazı araçlar, bölgeler arasında veri kopyalamayı desteklemez.  

* **Bant genişliği ücretleri**. Veriler Azure bölgesinden aktarıldığından [bant genişliği ücretleri](https://azure.microsoft.com/pricing/details/bandwidth/) uygulanır.

* **Verilerinizdeki AKRALLAR**. Dosya ve klasörlere ALA'lar uygulayarak yeni bölgedeki verilerinizi güvence altına alanın. Daha fazla bilgi için bkz: [Azure Veri Gölü Depolama Gen1'de depolanan verilerin güvenliğini sağlama](data-lake-store-secure-data.md). ALA'larınızı güncelleştirmek ve ayarlamak için geçişi kullanmanızı öneririz. Geçerli ayarlarınıza benzer ayarları kullanmak isteyebilirsiniz. Azure portalını, [PowerShell cmdlets'i](/powershell/module/az.datalakestore/get-azdatalakestoreitempermission)veya SDK'ları kullanarak herhangi bir dosyaya uygulanan ACM'leri görüntüleyebilirsiniz.  

* **Analitik hizmetlerinin konumu.** En iyi performans için, Azure Veri Gölü Analitiği veya Azure HDInsight gibi analitik hizmetleriniz verilerinizle aynı bölgede olmalıdır.  

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Veri Gölü Depolama Gen1'e Genel Bakış](data-lake-store-overview.md)
