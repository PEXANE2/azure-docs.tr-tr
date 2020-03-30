---
title: Azure Kapsayıcıları görüntüsü için SK'ler | Azure Marketi
description: SK'leri azure kapsayıcısı için yapılandırın.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/24/2019
ms.author: dsindona
ms.openlocfilehash: adbe3d4c498c8f4e4968ca903f78c34aedca9a4d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80280006"
---
# <a name="container-skus-tab"></a>Konteyner SKUs sekmesi

**Yeni Teklif** sayfasının **STU** sekmesi, bir veya daha fazla SNU oluşturmanıza ve bunları yeni teklifinize ilişkilendirmenize olanak tanır.  Bir çözümü özellik kümelerine, faturalandırma modellerine veya diğer özelliklere göre ayırt etmek için farklı SCO'lar kullanabilirsiniz.

## <a name="sku-settings"></a>SKU Ayarları

Yeni bir teklif oluşturmaya başladığınızda, teklifle ilişkili herhangi bir STU yoktur. Yeni bir SKU oluşturmak için aşağıdaki adımları izleyin:

1. SKU sekmesinde Yeni **SKU'yu** seçin

   ![Yeni SKU istemi](./media/containers-sku-settings.png)

2. Gerekli SKU ve konteyner bilgilerini sağlayın. Her SKU bir kapsayıcı görüntüsüne karşılık gelir. Bir SKU'nun iki bölümü vardır:

    -   SKU meta verileri
    -   Kapsayıcı meta verileri


### <a name="sku-metadata"></a>SKU meta verileri

SKU meta verileri, kapsayıcı girişi için ön ekran bilgilerini içerir.

![SKU meta verileri](./media/containers-sku-details.png)


### <a name="container-metadata"></a>Kapsayıcı meta verileri

Kapsayıcı meta verileri, Azure Kapsayıcı Kayıt Defteri'nde (ACR) görüntü deposu bilgilerinizin başvuru bilgilerine sahiptir. Azure Marketi bu resmi Pazara özgü genel kayıt defterine kopyalar ve sertifikadan sonra görüntüyü müşteriler için kullanılabilir hale getirir. Azure kullanıcısından bir Azure Marketi kapsayıcı görüntüsünü kullanma istekleri, ACR'den değil, Market'in genel kayıt defterinden sunulur.

![Kapsayıcı meta verileri](./media/containers-image-repository.png)
    
Önceki ekran yakalamadaki **Görüntü Deposu Ayrıntıları** aşağıdaki alanları içerir.  Gerekli alanlar yıldız işareti (*) tarafından suçlanır.

-   **Abonelik\* Kimliği** - ACR'nin bulunduğu Azure abonelik kimliği.
-   **Kaynak grubu\* adı** - ACR'nin kaynak grubu adı.
-   **Kayıt defteri\* adı** - ACR adı.
-   **Depo adı\* ** - Depo adı. Bu ad ayarlandıktan sonra, bu değer değiştirilemez. Hesabınızdaki diğer tekliflerle çakışmayı önlemek için benzersiz bir ad kullanın.
-   **Kullanıcı\* adı** - ACR görüntüsüyle ilişkili kullanıcı adı (yönetici kullanıcı adı).
-   **Parola\* ** - ACR görüntüsüyle ilişkili parola.

    >[!NOTE]
    >Kullanıcı adı ve parola, iş ortaklarının yayımlama işleminde belirtilen ACR'ye erişebilmesini sağlamak için gereklidir.


### <a name="image-version"></a>Görüntü Sürümü

Kapsayıcı görüntü yayınlarken, bir veya daha fazla görüntü etiketi sağlayabilir ve SHA sindirir.

**Resim\* Etiketi veya Özeti**
 
- Bu etiket veya özet `latest` bir etiket ve sürüm etiketi `xx.xx.xx-` içermelidir (örneğin, xx'in bir sayı olduğu yerden başlayarak). Bunlar birden çok platformu hedeflemek için [açık etiketler](https://github.com/estesp/manifest-tool) olmalıdır. Bir bildirim etiketiyle başvurulan tüm etiketlerde de eklenmelidir, böylece bunları yükleyebiliriz. 
- Etiketleri kullanarak kapsayıcının çeşitli sürümlerini ekleyebilirsiniz. Tüm bildirim etiketleri `latest`(hariç) `X.Y-` `X.Y.Z-` ya da X, Y, Z'nin tümseger olduğu yerde başlamalıdır. <br/> Örneğin, `latest` bir etiket `1.0.1-linux-x64`, `1.0.1-linux-arm32`, `1.0.1-windows-arm32`, ve , bu etiketleri işaret ederse buraya eklenmesi gerekir.

>[!NOTE]
>Test sırasında görüntüyü tanımlayabilmek için resminize bir **test etiketi** eklemeyi unutmayın.


## <a name="next-steps"></a>Sonraki adımlar

Teklifiniz için pazar yeri açıklaması oluşturmak için [Market sekmesini](./cpp-marketplace-tab.md) kullanın. 
