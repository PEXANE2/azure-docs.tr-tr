---
title: Azure kapsayıcıları görüntüsü için SKU 'Lar | Azure Marketi
description: Azure kapsayıcısı için SKU 'Ları yapılandırın.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/24/2019
ms.author: dsindona
ms.openlocfilehash: 5f37d157e11b8cd7cf093fa558e81d9a1ce345a9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82146196"
---
# <a name="container-skus-tab"></a>Kapsayıcı SKU 'Ları sekmesi

> [!IMPORTANT]
> 13 Nisan 2020 ' den itibaren, Azure Container tekliflerinizin yönetimini Iş Ortağı Merkezi 'ne taşımaya başlayacağız. Geçişten sonra, Iş Ortağı Merkezi 'nde tekliflerinizi oluşturup yönetirsiniz. Geçirilmiş tekliflerinizi yönetmek için [bir Azure kapsayıcı oluşturma teklifi oluşturma](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-azure-container-offer) bölümündeki yönergeleri izleyin.

**Yeni teklif** sayfasının **SKU 'ları** sekmesi bir veya daha fazla SKU oluşturmanıza ve bunları yeni teklifinizle ilişkilendirmenize olanak sağlar.  Bir çözümü Özellik kümelerine, faturalandırma modellerine veya diğer özelliklerle ayırt etmek için farklı SKU 'Lar kullanabilirsiniz.

## <a name="sku-settings"></a>SKU ayarları

Yeni bir teklif oluşturmaya başladığınızda, teklifle ilişkili hiçbir SKU yoktur. Yeni bir SKU oluşturmak için aşağıdaki adımları izleyin:

1. SKU 'Lar sekmesinde **yenı SKU** ' yı seçin.

   ![Yeni SKU istemi](./media/containers-sku-settings.png)

2. Gerekli SKU ve kapsayıcı bilgilerini sağlayın. Her SKU bir kapsayıcı resmine karşılık gelir. SKU 'nun iki bölümü vardır:

    -   SKU meta verileri
    -   Kapsayıcı meta verileri


### <a name="sku-metadata"></a>SKU meta verileri

SKU meta verileri kapsayıcı listesi için storefront görüntü bilgilerini içerir.

![SKU meta verileri](./media/containers-sku-details.png)


### <a name="container-metadata"></a>Kapsayıcı meta verileri

Kapsayıcı meta verileri Azure Container Registry (ACR) içindeki görüntü deposu ayrıntılarınızın başvuru bilgilerine sahiptir. Azure Marketi bu görüntüyü Market 'e özgü, ortak bir kayıt defterine kopyalar ve ardından görüntünün sertifikadan sonra müşterilere açık olmasını sağlar. Azure kullanıcısının Azure Marketi kapsayıcısı görüntüsünü tüketmesi için tüm isteklerin ACR değil, Market 'in ortak kayıt defterinden sunulması.

![Kapsayıcı meta verileri](./media/containers-image-repository.png)
    
Önceki ekran yakalamadaki **görüntü deposu ayrıntıları** aşağıdaki alanları içerir.  Gerekli alanlar bir yıldız işareti (*) ile tanımlanır.

-   **Abonelik kimliği\* ** -ACR 'nin mevcut olduğu Azure abonelik kimliği.
-   **Kaynak grubu adı\* ** -ACR 'nin kaynak grubu adı.
-   **Kayıt defteri\* adı** -ACR adı.
-   **Depo adı\* ** -Depo adı. Bu ad ayarlandıktan sonra bu değer değiştirilemez. Hesabınızdaki diğer tekliflerle çakışmayı önlemek için benzersiz bir ad kullanın.
-   **Kullanıcı\* adı** -ACR görüntüsüyle ilişkili Kullanıcı adı (Yönetici Kullanıcı adı).
-   **Parola\* ** -ACR görüntüsüyle ilişkili parola.

    >[!NOTE]
    >İş ortaklarının yayımlama sürecinde belirtilen ACR 'ye erişimi olduğundan emin olmak için Kullanıcı adı ve parola gereklidir.


### <a name="image-version"></a>Görüntü Sürümü

Bir kapsayıcı görüntüsü yayımlarken bir veya daha fazla resim etiketi ve SHA digests sağlayabilirsiniz.

**Resim etiketi\* veya Özeti**
 
- Bu etiket veya Özet, bir `latest` etiket ve sürüm etiketi içermelidir (örneğin, xx bir sayı olduğu `xx.xx.xx-` için başlangıç). Birden çok platformu hedeflemek için [bildirim etiketleri](https://github.com/estesp/manifest-tool) olmaları gerekir. Bir bildirim etiketi tarafından başvurulan tüm Etiketler de karşıya yüklenebilmemiz için eklenmelidir. 
- Etiketleri kullanarak kapsayıcının birkaç sürümünü ekleyebilirsiniz. Tüm bildirim etiketleri (except `latest`), ya da `X.Y-` `X.Y.Z-` X, Y, Z tamsayılarla başlamalıdır. <br/> Örneğin `latest` , bir etiket `1.0.1-linux-x64`, `1.0.1-linux-arm32`ve `1.0.1-windows-arm32`' a işaret ediyorsa, bu etiketlerin buraya eklenmesi gerekir.

>[!NOTE]
>Test sırasında görüntüyü tanımlayabilmeniz için yansımanıza bir **sınama etiketi** eklemeyi unutmayın.


## <a name="next-steps"></a>Sonraki adımlar

Teklifiniz için Market açıklaması oluşturmak üzere [Market sekmesini](./cpp-marketplace-tab.md) kullanın. 
