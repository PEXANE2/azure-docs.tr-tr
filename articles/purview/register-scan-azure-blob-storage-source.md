---
title: Azure Storage blob 'unu tarama
description: Azure takip görünümü veri kataloğunuzda Azure Blob depolamayı taramayı öğrenin.
author: shsandeep123
ms.author: sandeepshah
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 11/25/2020
ms.openlocfilehash: b27b46c68d018d2ddf79d284b20cc05b51640891
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "98880650"
---
# <a name="register-and-scan-azure-blob-storage"></a>Azure Blob Depolamayı kaydetme ve tarama

Bu makalede bir Azure Blob depolama hesabının nasıl kaydedileceği ve bir tarama ayarlandığı özetlenmektedir.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Azure Blob depolama, meta verileri ve şemayı yakalamak için tam ve artımlı taramaları destekler. Ayrıca, verileri sistem ve özel sınıflandırma kurallarına göre otomatik olarak sınıflandırır.

## <a name="prerequisites"></a>Önkoşullar

- Veri kaynaklarını kaydetmeden önce bir Azure purview hesabı oluşturun. Bir purview hesabı oluşturma hakkında daha fazla bilgi için bkz. [hızlı başlangıç: Azure purview hesabı oluşturma](create-catalog-portal.md).
- Azure purview veri kaynağı Yöneticisi olmanız gerekir

## <a name="setting-up-authentication-for-a-scan"></a>Tarama için kimlik doğrulamasını ayarlama

Azure Blob depolama için kimlik doğrulaması kurmanın üç yolu vardır:

- Yönetilen Kimlik
- Hesap anahtarı
- Hizmet Sorumlusu

### <a name="managed-identity-recommended"></a>Yönetilen kimlik (önerilen)

Bağlantıyı kurmak için **yönetilen kimlik**' i seçtiğinizde, önce purview hesabınıza veri kaynağını taramak için izin vermeniz gerekir:

1. Depolama hesabınıza gidin.
1. Sol gezinti menüsünden **Access Control (IAM)** seçeneğini belirleyin. 
1. **+ Ekle**'yi seçin.
1. **Rolü** **Depolama Blobu veri okuyucusu** olarak ayarlayın ve giriş kutusunu **seçin** altında Azure purview hesabınızın adını girin. Ardından, bu rol atamasını purview hesabınıza vermek için **Kaydet** ' i seçin.

> [!Note]
> Daha fazla ayrıntı için lütfen [Azure Active Directory kullanarak bloblara ve kuyruklara erişim yetkisi verme](../storage/common/storage-auth-aad.md) adımları bölümüne bakın

### <a name="account-key"></a>Hesap anahtarı

Kimlik doğrulama yöntemi seçili olduğunda **, anahtar** kasasında erişim anahtarınızı almanız ve depolamanız gerekir:

1. Depolama hesabınıza gidin
1. **Erişim anahtarlarına > ayarları** seçin
1. *Anahtarınızı* kopyalayın ve sonraki adımlar için bir yere kaydedin
1. Anahtar kasanıza gidin
1. **Gizli dizileri > ayarları** seçin
1. **+ Oluştur/Içeri aktar** ' ı seçin ve depolama hesabınızdan *anahtar* olarak **ad** ve **değer** girin
1. Tamamlanacak **Oluştur** ' u seçin
1. Anahtar Kasanızda purview 'a bağlı değilse, [Yeni bir Anahtar Kasası bağlantısı oluşturmanız](manage-credentials.md#create-azure-key-vaults-connections-in-your-azure-purview-account) gerekir
1. Son olarak, taramayı kurmak için anahtarı kullanarak [Yeni bir kimlik bilgisi oluşturun](manage-credentials.md#create-a-new-credential)

### <a name="service-principal"></a>Hizmet sorumlusu

Hizmet sorumlusu kullanmak için mevcut bir tane kullanabilir veya yeni bir tane oluşturabilirsiniz. 

> [!Note]
> Yeni bir hizmet sorumlusu oluşturmanız gerekiyorsa lütfen şu adımları izleyin:
> 1. [Azure Portal](https://portal.azure.com)gidin.
> 1. Sol taraftaki menüden **Azure Active Directory** ' yi seçin.
> 1. **Uygulama kayıtları**’nı seçin.
> 1. **+ Yeni uygulama kaydı**' nı seçin.
> 1. **Uygulama** için bir ad girin (hizmet asıl adı).
> 1. **Yalnızca bu kuruluş dizininde hesaplar '** ı seçin.
> 1. Yeniden yönlendirme URI 'SI için **Web** ' i seçin ve istediğiniz URL 'yi girin; gerçek veya iş olması gerekmez.
> 1. Ardından **Kaydet**’i seçin.

Hizmet sorumlusunun uygulama KIMLIĞI ve gizli anahtarı almak için gereklidir:

1. [Azure Portal](https://portal.azure.com) hizmet sorumlusuna gidin
1. **Uygulama (istemci) kimliğinin** değerlerini, **Sertifikalar & Gizliliklerden** **genel bakış** ve **istemci gizliliğine** kopyalayın.
1. Anahtar kasanıza gidin
1. **Gizli dizileri > ayarları** seçin
1. **+ Oluştur/Içeri aktar** ' ı seçin ve seçtiğiniz **adı** ve **değeri** hizmet sorumlınızdan **istemci gizli anahtarı** olarak girin
1. Tamamlanacak **Oluştur** ' u seçin
1. Anahtar Kasanızda purview 'a bağlı değilse, [Yeni bir Anahtar Kasası bağlantısı oluşturmanız](manage-credentials.md#create-azure-key-vaults-connections-in-your-azure-purview-account) gerekir
1. Son olarak, taramanızı ayarlamak için hizmet sorumlusunu kullanarak [Yeni bir kimlik bilgisi oluşturun](manage-credentials.md#create-a-new-credential)

#### <a name="granting-the-service-principal-access-to-your-blob-storage"></a>BLOB depolama alanınızı hizmet sorumlusu erişimi verme

1. Depolama hesabınıza gidin.
1. Sol gezinti menüsünden **Access Control (IAM)** seçeneğini belirleyin. 
1. **+ Ekle**'yi seçin.
1. **Rolü** **Depolama Blobu veri okuyucusu** olarak ayarlayın ve giriş kutusunu **seçin** altında hizmet asıl adınızı veya nesne kimliğinizi girin. Ardından, bu rol için hizmet sorumlusu atamasını sağlamak üzere **Kaydet** ' i seçin.

## <a name="firewall-settings"></a>Güvenlik duvarı ayarları

> [!NOTE]
> Depolama hesabı için güvenlik duvarınız etkinse, bir tarama ayarlarken **yönetilen kimlik** kimlik doğrulama yöntemini kullanmanız gerekir.

1. [Azure Portal](https://portal.azure.com) ' de depolama hesabınıza gidin
1. **Ayarlar > ağ iletişimi** ' ne gidin ve
1. **Erişime Izin ver** altında **Seçili ağları** seçin
1. **Güvenlik duvarı** bölümünde, **Güvenilen Microsoft hizmetlerinin bu depolama hesabına erişmesine izin ver** ' i seçin ve **Kaydet** ' e tıklayın

:::image type="content" source="./media/register-scan-azure-blob-storage-source/firewall-setting.png" alt-text="Güvenlik duvarı ayarını gösteren ekran görüntüsü":::

## <a name="register-an-azure-blob-storage-account"></a>Azure Blob depolama hesabı kaydetme

Veri kataloğunuza yeni bir blob hesabı kaydetmek için aşağıdakileri yapın:

1. Purview hesabınıza gidin
1. Sol gezinti bölmesinde **kaynakları** seçin
1. **Kaydol** ' u seçin
1. **Kayıt kaynakları** üzerinde **Azure Blob depolama** ' yı seçin.
1. **Devam**'ı seçin

**Kaynakları Kaydet (Azure Blob depolama)** ekranında şunları yapın:

1. Veri kaynağının katalogda listelenecek bir **ad** girin. 
1. Depolama hesaplarını filtrelemek için aboneliğinizi seçin
1. Bir depolama hesabı seçin
1. Bir koleksiyon seçin veya yeni bir tane oluşturun (Isteğe bağlı)
1. Veri kaynağını kaydetmek için **son** ' a gidin.

:::image type="content" source="media/register-scan-azure-blob-storage-source/register-sources.png" alt-text="kaynakları kaydetme seçenekleri" border="true":::

[!INCLUDE [create and manage scans](includes/manage-scans.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Azure purview veri kataloğuna gözatın](how-to-browse-catalog.md)
- [Azure purview Veri Kataloğu 'Nda arama](how-to-search-catalog.md)