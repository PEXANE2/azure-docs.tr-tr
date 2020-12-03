---
title: Kaydet ve Tara Azure Data Lake Storage (ADLS) Gen1
description: Bu öğreticide Azure Data Lake Storage 1. verileri Azure purview 'a nasıl tarayabileceğiniz açıklanmaktadır.
author: kchandra
ms.author: kchandra
ms.service: data-catalog
ms.subservice: data-catalog-gen2
ms.topic: how-to
ms.date: 11/30/2020
ms.openlocfilehash: ee0b9238deb7805113f0cbfa28d0b60a114820a9
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96555512"
---
# <a name="register-and-scan-azure-data-lake-storage-gen1"></a>Kaydet ve Tara Azure Data Lake Storage 1.

Bu makalede, Azure purview 'da Azure Data Lake Storage 1. veri kaynağı olarak nasıl kaydedileceği ve üzerinde bir tarama ayarlama özetlenmektedir.

> [!Note]
> Azure Data Lake Storage 2. Nesil genel kullanıma sunuldu. Bugün kullanmaya başlamanızı öneririz. Daha fazla bilgi için bkz. [ürün sayfası](https://azure.microsoft.com/services/storage/data-lake-storage/).

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Azure Data Lake Storage 1. veri kaynağı aşağıdaki işlevleri destekler:

- Azure Data Lake Storage 1. meta verileri ve sınıflandırmayı yakalamak için **tam ve artımlı taramalar**

- ADF kopyası/veri akışı etkinlikleri için veri varlıkları arasında **kökenini**

## <a name="prerequisites"></a>Ön koşullar

- Veri kaynaklarını kaydetmeden önce bir Azure purview hesabı oluşturun. Bir purview hesabı oluşturma hakkında daha fazla bilgi için bkz. [hızlı başlangıç: Azure purview hesabı oluşturma](create-catalog-portal.md).
- Azure purview veri kaynağı Yöneticisi olmanız gerekir

## <a name="setting-up-authentication-for-a-scan"></a>Tarama için kimlik doğrulamasını ayarlama

Aşağıdaki kimlik doğrulama yöntemleri Azure Data Lake Storage 1. için desteklenir:

- Yönetilen Kimlik
- Hizmet sorumlusu

### <a name="managed-identity-recommended"></a>Yönetilen kimlik (önerilen)

Kolaylık ve güvenlik için, veri deponuzda kimlik doğrulaması yapmak üzere purview MSI kullanmak isteyebilirsiniz.

Veri kataloğunun MSI kullanarak bir tarama ayarlamak için, önce purview hesabınıza veri kaynağını tarama izni vermeniz gerekir. Bu adım, taramanızı *ayarlamadan önce* gerçekleştirilmelidir veya tarama başarısız olur.

#### <a name="adding-the-data-catalog-msi-to-an-azure-data-lake-storage-gen1-account"></a>Veri Kataloğu MSI bir Azure Data Lake Storage 1. hesabına ekleniyor

Kataloğun MSI 'sini abonelik, kaynak grubu veya kaynak düzeyinde ekleyebilirsiniz. Bu, üzerinde tarama izinlerinin olmasını istediğiniz seçeneğe bağlıdır.

> [!Note]
> Azure kaynağına yönetilen bir kimlik ekleyebilmek için aboneliğin sahibi olmanız gerekir.

1. [Azure Portal](https://portal.azure.com), kataloğun taraymasına izin vermek istediğiniz abonelik, kaynak grubu veya kaynak (örneğin, bir Azure Data Lake Storage 1. depolama hesabı) bulun.

2. **Genel bakış** ' a ve ardından **Veri Gezgini** ' ni seçin.

   :::image type="content" source="./media/register-scan-adls-gen1/access-control.png" alt-text="Erişim denetimini seçme":::

3. Üstteki gezinmede **erişime** tıklayın

   :::image type="content" source="./media/register-scan-adls-gen1/access.png" alt-text="Erişime tıklayın":::

4. **Ekle**'ye tıklayın. Kullanıcı veya Grup Seç seçimine **purview kataloğunu** ekleyin. **Okuma** ve **yürütme** izinlerini seçin. Aşağıdaki ekran görüntüsünde gösterildiği gibi Ekle seçeneğinde **Bu klasörü ve tüm alt öğeleri** seçtiğinizden emin olun ve **Tamam** 
    :::image type="content" source="./media/register-scan-adls-gen1/managed-instance-authentication.png" alt-text="MSI kimlik doğrulaması ayrıntıları"::: ' na tıklayın.

5. Anahtar Kasanızda purview 'a bağlı değilse, [Yeni bir Anahtar Kasası bağlantısı oluşturmanız](manage-credentials.md#create-azure-key-vaults-connections-in-your-azure-purview-account)gerekir.

6. Son olarak, taramanızı ayarlamak için hizmet sorumlusunu kullanarak [Yeni bir kimlik bilgisi oluşturun](manage-credentials.md#create-a-new-credential)
> [!Note]
> Kataloğun MSI 'sini veri kaynağına ekledikten sonra, bir tarama ayarlamadan önce izin vermek üzere en fazla 15 dakika bekleyin.

Yaklaşık 15 dakika sonra kataloğun, kaynakları tarayabilmesi için uygun izinlere sahip olması gerekir.

### <a name="service-principal"></a>Hizmet sorumlusu

Bir hizmet sorumlusu kullanmak için, önce aşağıdaki adımları izleyerek bir tane oluşturmanız gerekir:

1. [Azure Portal](https://portal.azure.com)gidin.

2. Sol taraftaki menüden **Azure Active Directory** ' yi seçin.

3. **Uygulama kayıtları**’nı seçin.

4. **+ Yeni uygulama kaydı**' nı seçin.

5. **Uygulama** için bir ad girin (hizmet asıl adı).

6. **Yalnızca bu kuruluş dizininde hesaplar '** ı seçin.

7. **Yeniden yönlendirme URI 'si** için **Web** ' i SEÇIN ve istediğiniz URL 'yi girin; gerçek veya iş olması gerekmez.

8. Ardından **Kaydet**’i seçin.

9. Değerleri hem görünen ad hem de uygulama KIMLIĞI ' nden kopyalayın.

#### <a name="adding-the-data-catalog-service-principal-to-an-azure-data-lake-storage-gen1-account"></a>Veri Kataloğu hizmet sorumlusu bir Azure Data Lake Storage 1. hesabına ekleniyor
1. [Azure Portal](https://portal.azure.com), kataloğun taraymasına izin vermek istediğiniz abonelik, kaynak grubu veya kaynak (örneğin, bir Azure Data Lake Storage 1. depolama hesabı) bulun.

2. **Genel bakış** ' a ve ardından **Veri Gezgini** ' ni seçin.

   :::image type="content" source="./media/register-scan-adls-gen1/access-control.png" alt-text="Erişim denetimini seçme":::

3. Üstteki gezinmede **erişime** tıklayın

   :::image type="content" source="./media/register-scan-adls-gen1/access.png" alt-text="Erişime tıklayın":::

4. **Ekle**'ye tıklayın. Kullanıcı veya Grup Seç seçimine **hizmet sorumlusu uygulamasını** ekleyin. **Okuma** ve **yürütme** izinlerini seçin. Aşağıdaki ekran görüntüsünde gösterildiği gibi Ekle seçeneğinde **Bu klasörü ve tüm alt öğeleri** seçtiğinizden emin olun ve **Tamam** 
    :::image type="content" source="./media/register-scan-adls-gen1/service-principal-authentication.png" alt-text="hizmet sorumlusu kimlik doğrulaması ayrıntıları"::: ' na tıklayın.

5. Anahtar Kasanızda purview 'a bağlı değilse, [Yeni bir Anahtar Kasası bağlantısı oluşturmanız](manage-credentials.md#create-azure-key-vaults-connections-in-your-azure-purview-account)gerekir.

6. Son olarak, taramanın kurulumunu yapmak için hizmet sorumlusunu kullanarak [Yeni bir kimlik bilgisi oluşturun](manage-credentials.md#create-a-new-credential) .

## <a name="register-azure-data-lake-storage-gen1-data-source"></a>Azure Data Lake Storage 1. veri kaynağını Kaydet

Yeni bir ADLS 1. hesabını veri kataloğunuza kaydetmek için aşağıdakileri yapın:

1. Purview veri kataloğunuza gidin.
2. Sol gezinti çubuğunda **kaynaklar** ' ı seçin.
3. **Kaydol** ' u seçin
4. **Kaynakları kaydet**' de **Azure Data Lake Storage 1.**' yi seçin. 
5. **Devam**’ı seçin.

Kaynakları Kaydet (Azure Data Lake Storage 1.) ekranında şunları yapın:

1. Veri kaynağının katalogda listelenecek bir **ad** girin.
2. Depolama hesaplarını filtrelemek için aboneliğinizi seçin
3. Bir depolama hesabı seçin
4. Bir koleksiyon seçin veya yeni bir tane oluşturun (Isteğe bağlı)
5. Veri kaynağını kaydetmek için son ' a gidin.

[!INCLUDE [create and manage scans](includes/manage-scans.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Azure purview veri kataloğuna gözatın](how-to-browse-catalog.md)
- [Azure purview Veri Kataloğu 'Nda arama](how-to-search-catalog.md)
