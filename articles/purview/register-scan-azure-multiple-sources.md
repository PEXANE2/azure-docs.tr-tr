---
title: Azure birden çok kaynağı tarama
description: Azure purview veri kataloğunuzda bir Azure aboneliğinin veya kaynak grubunun tamamını taramayı öğrenin.
author: viseshag
ms.author: viseshag
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 2/26/2021
ms.openlocfilehash: 098f62365971fd634001706ab99fd414a6b25056
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102123573"
---
# <a name="register-and-scan-azure-multiple-sources"></a>Azure birden çok kaynağı kaydetme ve tarama

Bu makalede, bir Azure çoklu kaynağını (Azure abonelikleri veya kaynak grupları) purview 'a kaydetme ve üzerinde tarama ayarlama özetlenmektedir.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Azure birden çok kaynak, takip eden pek çok Azure Kaynak türü üzerinde meta verileri ve şemayı yakalamaya yönelik taramaları destekler. Ayrıca, verileri sistem ve özel sınıflandırma kurallarına göre otomatik olarak sınıflandırır.

## <a name="prerequisites"></a>Önkoşullar

- Veri kaynaklarını kaydetmeden önce bir Azure purview hesabı oluşturun. Bir purview hesabı oluşturma hakkında daha fazla bilgi için bkz. [hızlı başlangıç: Azure purview hesabı oluşturma](create-catalog-portal.md).
- Azure purview veri kaynağı Yöneticisi olmanız gerekir
- Aşağıdaki bölümlerde açıklandığı şekilde kimlik doğrulamasını ayarlama

### <a name="setting-up-authentication-for-enumerating-resources-under-a-subscription-or-resource-group"></a>Bir abonelik veya kaynak grubu altında kaynakları listelemek için kimlik doğrulaması ayarlama

1. Azure portal abonelik veya kaynak grubuna gidin.  
1. Sol gezinti menüsünden **Access Control (IAM)** seçeneğini belirleyin   
1. Abonelik veya kaynak grubuna bir rol eklemek için sahip veya Kullanıcı erişimi yöneticisi olmanız gerekir. *+ Ekle* düğmesini seçin. 
1. **Okuyucu** rolünü ayarlayın ve giriş kutusunu seçin altında Azure purview hesabınızın adını (MSI 'sini temsil eder) girin. Rol atamasını son vermek için *Kaydet* ' e tıklayın.

### <a name="setting-up-authentication-to-scan-resources-under-a-subscription-or-resource-group"></a>Bir abonelik veya kaynak grubu altındaki kaynakları taramak için kimlik doğrulaması ayarlama

Azure birden çok kaynak için kimlik doğrulaması kurmanın iki yolu vardır:

- Yönetilen Kimlik
- Hizmet Sorumlusu

> [!NOTE]
> Aboneliğinizi veya kaynak grubunuzun içindeki her bir kaynakta, kaydetmek ve taramak istediğiniz her kaynak için kimlik doğrulamasını ayarlamanız gerekir. Azure depolama kaynak türleri (Azure Blob depolama ve Azure Data Lake Storage 2.), MSI veya hizmet sorumlusunu, abonelik/kaynak grup düzeyinde Depolama Blobu veri okuyucusu olarak eklemenize olanak tanıyarak kolaylaştırır. İzinler daha sonra bu abonelik veya kaynak grubu içindeki her depolama hesabına göre aşağı doğru bir şekilde yapılır. Diğer tüm kaynak türleri için, MSI veya hizmet sorumlusunu her kaynağa ya da cihaza bir komut dosyası uygulamanız gerekir. Bir abonelik veya kaynak grubu içindeki her kaynak türü için izin ekleme yöntemi aşağıda verilmiştir.
    
- [Azure Blob Depolama](register-scan-azure-blob-storage-source.md#setting-up-authentication-for-a-scan)
- [Azure Data Lake Storage Gen1](register-scan-adls-gen1.md#setting-up-authentication-for-a-scan)
- [Azure Data Lake Storage 2. Nesil](register-scan-adls-gen2.md#setting-up-authentication-for-a-scan)
- [Azure SQL Veritabanı](register-scan-azure-sql-database.md)
- [Azure SQL veritabanı yönetilen örneği](register-scan-azure-sql-database-managed-instance.md#setting-up-authentication-for-a-scan)
- [Azure Synapse Analytics](register-scan-azure-synapse-analytics.md#setting-up-authentication-for-a-scan)
 
## <a name="register-an-azure-multiple-source"></a>Azure çoklu kaynağını kaydetme

Yeni bir Azure birden çok kaynağını veri kataloğunuza kaydetmek için aşağıdakileri yapın:

1. Purview hesabınıza gidin
1. Sol gezinti bölmesinde **kaynakları** seçin
1. **Kaydol** ' u seçin
1. **Kayıt kaynakları** üzerinde **Azure (birden çok)** seçeneğini belirleyin
1. **Devam**'ı seçin

   :::image type="content" source="media/register-scan-azure-multiple-sources/register-azure-multiple.png" alt-text="Azure çoklu kaynağını kaydetme":::

**Kaynakları Kaydet (Azure çoklu)** ekranında şunları yapın:

1. Veri kaynağının katalogda listelenecek bir **ad** girin 
1. İsteğe bağlı olarak filtrelemek için bir **Yönetim grubu** seçin
1. Açılan menüdeki belirli bir abonelik altında **bir abonelik veya belirli bir kaynak grubu seçin** . Kayıt kapsamı seçili abonelik veya kaynak grubu olarak ayarlanacak  
1. Bir **koleksiyon** seçin veya yeni bir tane oluşturun (isteğe bağlı)
1. Veri kaynağının kaydedileceği **son**

   :::image type="content" source="media/register-scan-azure-multiple-sources/azure-multiple-source-setup.png" alt-text="Azure çoklu kaynağını ayarlama":::

## <a name="creating-and-running-a-scan"></a>Tarama oluşturma ve çalıştırma

Yeni bir tarama oluşturup çalıştırmak için aşağıdakileri yapın:

1. **Kaynaklar** bölümüne gidin

1. Kaydettiğiniz veri kaynağını seçin

1. Ayrıntıları görüntüle ' ye tıklayın ve **+ Yeni Tara** ' yı seçin ya da kaynak kutucuğunda hızlı eylem Tara simgesini kullanın

1. *Adı* girin ve bu kaynak içinde taramak istediğiniz kaynak türlerini seçin

    1. Tümünü (Bu abonelik veya kaynak grubu içinde mevcut olmayan gelecekteki kaynak türlerini içerir) *Tümü* olarak bırakabilirsiniz.
    1. Taramak istediğiniz **kaynak türlerini özel olarak seçebilirsiniz** . Bu seçeneği belirlerseniz, bu abonelik içinde oluşturulabilecek kaynak türleri veya kaynak grubu, tarama ileride açıkça düzenlenmediği takdirde, taramalar için dahil edilmez
    
    :::image type="content" source="media/register-scan-azure-multiple-sources/multiple-source-scan.png" alt-text="Azure çoklu kaynak taraması":::

1. Veri kaynağınızdaki kaynaklara bağlanacak kimlik bilgisini seçin. 
    1. Bir **kimlik BILGISINI** MSI olarak veya belirli bir hizmet sorumlusu tür kimlik bilgisini seçerek, abonelik veya kaynak grubu altındaki tüm kaynak türleri için kullanmayı seçebilirsiniz
    1. Ayrıca, kaynak türünü özel olarak seçebilir ve bu kaynak türü için **farklı bir kimlik bilgisi uygulayabilirsiniz**
    1. Her kimlik bilgisi, belirli bir tür altındaki tüm kaynaklar için kimlik doğrulama yöntemi olarak değerlendirilir
    1. Yukarıdaki bu [bölümde](#setting-up-authentication-to-scan-resources-under-a-subscription-or-resource-group) açıklandığı şekilde onları başarıyla taramak için kaynaklarda seçili kimlik bilgilerini ayarlamanız gerekir.
1. Her bir tür içinde, tüm kaynakları veya bunların bir alt kümesini ada göre taramayı seçebilirsiniz.
    1. Seçeneği **Tüm** olarak bırakırsanız, bu türden gelecek kaynaklar da gelecekteki tarama çalıştırmaları halinde taranır
    1. Belirli depolama hesapları veya SQL veritabanları ' nı seçerseniz, bu abonelik veya kaynak grubu içinde bu türden oluşturulan kaynaklar, tarama ileride açıkça düzenlenmediği takdirde, taramalar için dahil edilmez
 
1.  Devam etmek için **devam** ' a tıklayın. Purview MSI 'yi abonelik veya kaynak grubu üzerinde bir okuyucu olarak uygulamış olup olmadığınızı denetlemek için erişimi test edeceğiz. Bir hata iletisi oluşturulursa, [buradaki](#setting-up-authentication-for-enumerating-resources-under-a-subscription-or-resource-group) yönergeleri izleyin

1.  Önceki adımda seçilen her kaynak türü için **tarama kural kümelerini** seçin. Ayrıca, tarama kuralı kümelerini satır içi olarak da oluşturabilirsiniz.
  :::image type="content" source="media/register-scan-azure-multiple-sources/multiple-scan-rule-set.png" alt-text="Azure birden çok tarama kuralı kümesi seçimi":::

1. Tarama tetikleyiciyi seçin. Bunu **haftalık/aylık** veya **bir kez** çalışacak şekilde planaktarabilirsiniz

1. Taramayı gözden geçirin ve ayarlamayı tamamladıktan sonra Kaydet ' i seçin   

## <a name="viewing-your-scans-and-scan-runs"></a>Taramalarınızı ve tarama çalıştırmalarını görüntüleme

1. Kaynaklar bölümünün altındaki kutucukta **Ayrıntıları görüntüle** ' ye tıklayarak kaynak ayrıntılarını görüntüleyin. 

      :::image type="content" source="media/register-scan-azure-multiple-sources/multiple-source-detail.png" alt-text="Azure çoklu kaynak ayrıntıları"::: 

1. **Tarama ayrıntıları** sayfasına giderek tarama çalıştırma ayrıntılarını görüntüleyin.
    1. *Durum çubuğu* , alt kaynakların çalışma durumu hakkında kısa bir özettir. Abonelik veya kaynak grubu düzeyinde görüntülenir
    1. Yeşil, kırmızı, başarısız anlamına gelir. Gri, taramanın hala devam ettiği anlamına gelir
    1. Daha ayrıntılı bilgi görüntülemek için her taramaya tıklayabilirsiniz

      :::image type="content" source="media/register-scan-azure-multiple-sources/multiple-scan-full-details.png" alt-text="Azure birden çok tarama ayrıntıları":::

1. Kaynak Ayrıntıları sayfasının en altında başarısız olan en son tarama çalışmalarının özetini görüntüleyin. Ayrıca, Bu çalıştırmaların ilgili daha ayrıntılı ayrıntılarını görüntülemek için de tıklayabilirsiniz.

## <a name="manage-your-scans---edit-delete-or-cancel"></a>Taramalarınızı yönetme-düzenleme, silme veya iptal etme
Bir taramayı yönetmek veya silmek için aşağıdakileri yapın:

- Yönetim merkezine gidin. Kaynaklar ve Tarama bölümünde veri kaynakları ' nı seçin ve ardından istediğiniz veri kaynağını seçin

- Yönetmek istediğiniz taramayı seçin. Taramayı Düzenle 'yi seçerek düzenleyebilirsiniz

- , Sil ' i seçerek taramayı silebilirsiniz.
- Bir tarama çalışıyorsa, bu işlemi de iptal edebilirsiniz

## <a name="next-steps"></a>Sonraki adımlar

- [Azure purview veri kataloğuna gözatın](how-to-browse-catalog.md)
- [Azure purview Veri Kataloğu 'Nda arama](how-to-search-catalog.md)    
