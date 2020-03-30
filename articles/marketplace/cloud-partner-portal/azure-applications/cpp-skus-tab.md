---
title: Azure uygulama teklifi için SDO'ları yapılandırın | Azure Marketi
description: SNU'ları Azure yönetilen bir uygulama ve Azure çözüm şablonu için yapılandırma.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: dsindona
ms.openlocfilehash: 043394a1303456ce5b209bb84b5afaf09f6beba4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80289082"
---
# <a name="azure-application-skus-tab"></a>Azure uygulaması SK'ler sekmesi

Bu makalede, Azure uygulamanız için SNU oluşturmak için SK'ler sekmesinin nasıl kullanılacağı açıklanmaktadır. 

> [!IMPORTANT]
> Yönetilen uygulama teklifi ve Çözüm şablonu teklifi için SKU yapılandırma adımları farklıdır. Bu farklar bu makalede belgelenmiştir. 

## <a name="configure-azure-application-skus"></a>Azure uygulama SDO'larını yapılandırma

### <a name="create-a-new-sku"></a>Yeni bir SKU oluşturun

Yeni bir SKU oluşturmak için şu adımları kullanın:

1. **SK'ler** sekmesini seçin.
2. SKU'lar altında **+ Yeni SKU'ları**seçin.

    ![Yeni SKU istemi](./media/azureapp-plus-sku.png)

3. Yeni SKU açılır penceresinde bir **SKU kimliği**yazın. Bu kimlik 50 karakterle sınırlıdır ve yalnızca küçük, alfasayısal karakterler, tireler veya alt çizgilerden oluşmalıdır. SKU kimliği bir çizgide sona eremez.
4. SKU Kimliği, ürün URL'leri, Kaynak Yöneticisi şablonları (varsa) ve fatura lama raporlarında müşteriler tarafından görülebilir. Teklif yayınlandıktan sonra bu kimliği değiştiremezsiniz.

### <a name="sku-details-for-a-solution-template"></a>Çözüm Şablonu için SKU Ayrıntıları

Bir sonraki ekran yakalama, Çözüm Şablonu için SKU Ayrıntıları formunu gösterir.

![Çözüm şablonu için SKU ayrıntıları formu](./media/azureapp-sku-details-solutiontemplate.png)

Aşağıdaki SKU değerlerini sağlayın.  Yıldız işaretiyle eklenen alanlar gereklidir.

|    Alan         |       Açıklama                                                            |
|  ---------       |     ---------------                                                          |
|  **Başlık\***     | SKU için bir başlık. Bu başlık, bu öğenin galerisinde görüntülenir.   |
| **Özet\***    | SKU kısa bir özet açıklaması. (Maksimum uzunluk 100 karakterdir.)  |
| **Açıklama\*** | SKU'nun ayrıntılı bir açıklaması. Temel HTML desteklenir.                 | 
| **SKU Tipi\***   | Azure uygulama çözümü türü, bu senaryo için ***Çözüm Şablonu'yu** seçin. |
| **Bulut Kullanılabilirliği\*** | SKU'nun yeri. Varsayılan olan **Ortak Azure'dur.**  <b/>   **Ortak Azure** - Uygulama, pazar yeri tümleştirmesi olan tüm ortak Azure bölgelerindeki müşterilere dağıtılabilir.  <b/>   **Azure Kamu Bulutu** - Uygulama Azure Devlet Bulutu'nda dağıtılacaktır. Microsoft, [Azure Kamu'da](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-marketplace-partners)yayımlamadan önce yayımlamadan önce yayımcılara çözüm çalışmalarını bu ortamda beklendiği gibi test etmelerini ve doğrulamalarını önerir. Sahnelemek ve test etmek için bir [deneme hesabı](https://azure.microsoft.com/offers/ms-azr-usgov-0044p/)isteyin.  |
| **Bu özel bir SKU mu?\*** | Bu SKU yalnızca belirli bir müşteri grubu tarafından kullanılabilen bir sku'ysa **Evet'i** seçin. |
|   |   |

  > [!NOTE] 
  > Microsoft Azure Kamu, ABD Federal, Eyalet, yerel veya kabile iş ortaklarından müşteriler için bu kuruluşlara hizmet vermeye uygun kontrollü erişime sahip bir devlet topluluğu bulutudur.


### <a name="sku-details-for-managed-application"></a>Yönetilen Uygulama için SKU Detayları

Bir sonraki ekran yakalama, Yönetilen Uygulama için SKU Ayrıntıları formunu gösterir.

   ![Yönetilen uygulama için SKU detay formu](./media/azureapp-sku-details-managedapplication.png)

Aşağıdaki SKU ayarlarını yapılandırın. Yıldız işaretiyle eklenen alanlar gereklidir.

|    Alan         |       Açıklama                                                            |
|  ---------       |     ---------------                                                          |
|  **Başlık\***     | SKU için bir başlık. Bu başlık, bu öğenin galerisinde görüntülenir.   |
| **Özet\***    | SKU kısa bir özet açıklaması. (Maksimum uzunluk 100 karakterdir.)  |
| **Açıklama\*** | SKU'nun ayrıntılı bir açıklaması. Temel HTML desteklenir.                 | 
| **SKU Tipi\***   | Azure uygulama çözümü türü, bu senaryo için ***Yönetilen Uygulama'yı** seçin. 
| **Bulut Kullanılabilirliği\*** | SKU'nun yeri. Varsayılan olan **Ortak Azure'dur.**  <b/>   **Ortak Azure** - Uygulama, pazar yeri tümleştirmesi olan tüm ortak Azure bölgelerindeki müşterilere dağıtılabilir.  <b/>   **Azure Kamu Bulutu** - Uygulama Azure Devlet Bulutu'nda dağıtılacaktır. Microsoft, [Azure Kamu'da](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-marketplace-partners)yayımlamadan önce yayımlamadan önce yayımcılara çözüm çalışmalarını bu ortamda beklendiği gibi test etmelerini ve doğrulamalarını önerir. Sahnelemek ve test etmek için bir [deneme hesabı](https://azure.microsoft.com/offers/ms-azr-usgov-0044p/)isteyin.   Microsoft Azure Kamu, ABD Federal, Eyalet, yerel veya kabile iş ortaklarından müşteriler için bu kuruluşlara hizmet vermeye uygun kontrollü erişime sahip bir devlet topluluğu bulutudur. |
| **Bu özel bir SKU mu?\*** | Bu SKU yalnızca belirli bir müşteri grubu tarafından kullanılabilen bir sku'ysa **Evet'i** seçin. |
| **Ülke/Bölge kullanılabilirliği\*** | Kullanılabilen ülkelerin/bölgelerin listesini görüntülemek için **Bölgeleri Seç'i** kullanın. Her ülkeyi/bölgeyi kontrol edin ve ardından seçimlerinizi kaydetmek için **Tamam'ı** seçin.  <b/>   ![Ülke ve bölge kullanılabilirlik listesi](./media/azure-app-select-country-region.png)  |
| **Eski Fiyatlandırma\*** | SKU için fiyat, USD aylık. Fiyatlar, yapılandırma üzerine geçerli döviz kurları kullanılarak yerel para birimi cinsinden ayarlanır. Sonuçta bu ayarları kendi beri bunları doğrulayın. Her ülkenin/bölgenin fiyatını ayrı ayrı ayarlamak veya görüntülemek için, lütfen fiyatlandırma tablosunu dışa aktarın ve özel fiyatlandırmayla içe aktarın.  Fiyatlandırma verilerinin dışa aktarılmasını/aktarılmasını etkinleştirmek için fiyatlandırma değişikliklerinizi kaydetmeniz gerekir.  |
| **Basitleştirilmiş Döviz Fiyatlandırması\*** | SKU için fiyat, USD aylık. Bu, Eski Fiyatlandırma ile aynı olmalıdır. Daha fazla bilgi için, [Basitleştirilmiş Para Birimi](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-update-existing-offer)Fiyatlandırması'na bakın. |
|  |  |


### <a name="package-details-for-solution-template"></a>Çözüm Şablonu için Paket Ayrıntıları

![Çözüm şablonu için paket ayrıntıları](./media/azureapp-sku-pkgdetails-solutiontemplate.png)

Aşağıdaki **Paket Ayrıntıları** değerlerini sağlayın.  Yıldız işaretiyle eklenen alanlar gereklidir.

- **Sürüm\* ** - Yükleyeceğiniz paketin sürümü. Sürüm etiketleri X, Y ve Z'nin tümseger olduğu X.Y.Z formunda olmalıdır.
- **Paket dosyası (.zip)\* ** - Bu paket, bir .zip dosyasına kaydedilmiş aşağıdaki dosyaları içerir.
  - **mainTemplate.json\* ** - Çözümü/uygulamayı dağıtmak ve çözüm için tanımlanan kaynakları oluşturmak için kullanılan dağıtım şablondosyası. Daha fazla bilgi için [dağıtım şablonu dosyalarının nasıl yazılabildiğini](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template)öğrenin.
  - **createUIDefinition.json\* ** - Bu dosya, Azure portalı tarafından bu çözümü/uygulamayı sağlamak için kullanıcı arabirimi oluşturmak için kullanılır. Daha fazla bilgi için, [yönetilen uygulamanız için Azure portalı kullanıcı arabirimi oluştur'a](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview)bakın.
  - Komut dosyaları (gerekirse) - Şablonu çalıştırırken gerekli olabilecek ek komut `Microsoft.Compute/virtualMachines/extensions`dosyaları, örneğin.
  - İç içe şablonlar (gerekirse) - Ek iç içe şablonlar.

  > [!IMPORTANT] 
  > Bu paket, bu uygulamayı sağlamak için gereken iç içe herhangi bir şablon veya komut dosyası içermelidir. mainTemplate.json dosyası ve createUIDefinition.json dosyası kök klasöründe olmalıdır. Dağıtım yapıları hakkında daha fazla bilgi için [Azure Kaynak Yöneticisi Şablonları - En İyi Uygulamalar Kılavuzu'na](https://github.com/Azure/azure-quickstart-templates/blob/master/1-CONTRIBUTION-GUIDE/best-practices.md#deployment-artifacts-nested-templates-scripts)bakın.


### <a name="package-details-for-managed-application"></a>Yönetilen Uygulama için Paket Detayları

   ![Yönetilen uygulama için paket ayrıntıları](./media/azureapp-sku-pkgdetails-managedapplication.png)

Aşağıdaki Paket Ayrıntılarını sağlayın.  Yıldız işaretiyle eklenen alanlar gereklidir.

- **Sürüm\* ** - Yükleyeceğiniz paketin sürümü. Sürüm etiketleri X, Y ve Z'nin tümseger olduğu X.Y.Z formunda olmalıdır.
- **Paket dosyası (.zip)\* ** - Bu paket, bir .zip dosyasına kaydedilmiş aşağıdaki dosyaları içerir.
  - applianceMainTemplate.json - Çözümü/uygulamayı dağıtmak ve tanımlanan kaynakları oluşturmak için kullanılan dağıtım şablondosyası. Daha fazla bilgi için [Bkz. Hızlı Başlangıç: Azure portalını kullanarak Azure Kaynak Yöneticisi şablonları oluşturun ve dağıtın.](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal) 
  - applianceCreateUIDefinition.json - Bu dosya, Azure portalı tarafından bu çözümü/uygulamayı sağlamak için kullanıcı arabirimi oluşturmak için kullanılır. Daha fazla bilgi için, [yönetilen uygulamanız için Azure portalı kullanıcı arabirimi oluştur'a](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview)bakın.
  - mainTemplate.json - Yalnızca Microsoft.Solution/appliances kaynağını içeren şablon dosyası. Daha fazla bilgi için bkz. Azure [Kaynak Yöneticisi Şablonlarının yapısını ve sözdizimini anlayın.](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) <br>
Bu kaynağın aşağıdaki önemli özelliklerine dikkat edin:
    - "tür" - Pazar-Yönetilen uygulama durumunda değer "Marketplace" olmalıdır.
    - "ManagedResourceGroupId" - Müşterinin aboneliğinde, applianceMainTemplate.json'da tanımlanan tüm kaynakların dağıtılacayabildiği kaynak grubu.
    - "PublisherPackageId"- Paketi benzersiz olarak tanımlayan dize. Bu değer aşağıdaki gibi inşa edilmelidir: [publisherId] bir concatenation's. [OfferId]-önizleme[SKUID]. [PackageVersion].

  >[!IMPORTANT] 
  >Bu paket, bu uygulamayı sağlamak için gereken iç içe herhangi bir şablon veya komut dosyası içermelidir. Bu dosyalar kök klasöründe olmalıdır: MainTemplate.json, applianceMainTemplate.json ve applianceCreateUIDefinition.json.

- **Kiracı\* Kimliği** - Kuruluşunuzun Azure Etkin Dizin kiracı kimliği.
- **JIT erişimini etkinleştirin? \* ** - Bu teklifi kullanarak müşteri dağıtımları için Zamanında Tam yönetim erişimi sağlamak için **Evet'i** seçin.

  >[!NOTE] 
  >JIT'yi etkinleştiriyorsanız, JIT erişimini desteklemek için CreateUiDefinition.json dosyasını güncelleştirmeniz gerekir.

Yönetilen bir uygulama için Yetkilendirme ve İlke Ayarlarını yapılandırmanız gerekir.


#### <a name="authorization"></a>Yetkilendirme

Yönetilen kaynak grubuna izin vermek istediğiniz kullanıcı, grup veya uygulamanın Azure Etkin Dizin Tanımlayıcısını ekleyin. Verilen izin, rol tanımı Kimliği ile gösterilir. Bir Sahibi, Katkıda Bulunan veya herhangi bir özel rol olabilir.


#### <a name="policy-settings"></a>İlke Ayarlar

Yönetilen Uygulama'nın uyumlu olduğu ilkeleri ekleyin. Azure Kaynak ilkeleri hakkında daha fazla bilgi edinin, [bkz.](../../../governance/policy/overview.md)

   ![Yönetilen bir uygulama için yetkilendirme ve ilke ayarları](./media/azureapp-sku-details-managedapp-auth-policy.png)

**Yeni bir yetkilendirme oluşturmak için:**

1. **Yetkilendirme**altında , seçin **+ Yeni Yetkilendirme**.
2. **Asıl Kimlik**için, yönetilen kaynak grubuna izin vermek istediğiniz kullanıcı, grup veya uygulamanın Azure Etkin Dizin Tanımlayıcısı'nı yazın. Verilen izin Rol Tanımı ile gösterilir.
3. **Rol Tanımı**için, açılır listeden bu seçeneklerden birini seçin: Sahip veya Katılımcı. Daha fazla bilgi için bkz. [Azure kaynakları için yerleşik roller](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

>[!NOTE] 
>Birden çok yetkilendirme eklenebilir. Ancak, Bir Active Directory kullanıcı grubu oluşturmak ve kimliğini "PrincipalId" olarak belirtmeniz önerilir. Bu, SKU'yu güncelleştirmek zorunda kalmadan kullanıcı grubuna daha fazla kullanıcı eklenmesini sağlar.

**Yeni bir ilke oluşturmak için:**

1. **İlke Ayarları**altında , + **Yeni İlke**seçin.
2. **İlke Adı**için, ilke için bir ad girin. Adın maksimum uzunluğu 50 karakterdir.
3. **İlkeler**için açılır listeden seçeneklerden birini seçin. Uygulama verileri kullandığında veri sağlayıcısının etkinleştirmek istediği ilkeyi seçin. Daha fazla bilgi için [Azure İlkesi Örnekleri'ne](https://docs.microsoft.com/azure/governance/policy/samples/index)bakın.

    ![Yönetilen bir uygulama için ilke ayarları](./media/azureapp-sku-policy-settings.png)

4. **İlke SKU**için, ilke SKU türü olarak Ücretsiz veya Standart'ı seçin. Standart SKU denetim politikaları için gereklidir.


## <a name="next-steps"></a>Sonraki adımlar

Ayrıca teklif inizi açıklayacak ve [Market sekmesinde](./cpp-marketplace-tab.md)pazarlama varlıklarını tedarik edin. 
