---
title: Azure SYNAPSE Analytics 'i tarama
description: Bu kılavuzda, Azure SYNAPSE Analytics 'i taramanın ayrıntıları açıklanmaktadır.
author: viseshag
ms.author: viseshag
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 10/22/2020
ms.openlocfilehash: 3111b3a102abd923169cf655f1d71e79b19f7d5d
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104598142"
---
# <a name="register-and-scan-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'i kaydetme ve tarama

Bu makalede, Azure SYNAPSE Analytics 'in (eski adıyla SQL DW) bir örneğinin nasıl kaydedileceği ve taramanın nasıl yapılacağı açıklanır.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Azure SYNAPSE Analytics (eski adıyla SQL DW), meta verileri ve şemayı yakalamak için tam ve artımlı taramaları destekler. Taramalar Ayrıca verileri sistem ve özel sınıflandırma kurallarına göre otomatik olarak sınıflandırır.

### <a name="known-limitations"></a>Bilinen sınırlamalar

Azure purview, Azure SYNAPSE Analytics 'te [görünümlerin](/sql/relational-databases/views/views?view=azure-sqldw-latest&preserve-view=true) taranmasını desteklemez

## <a name="prerequisites"></a>Önkoşullar

- Veri kaynaklarını kaydetmeden önce bir Azure purview hesabı oluşturun. Bir purview hesabı oluşturma hakkında daha fazla bilgi için bkz. [hızlı başlangıç: Azure purview hesabı oluşturma](create-catalog-portal.md).
- Azure purview veri kaynağı Yöneticisi olmanız gerekir
- Purview hesabı ve Azure SYNAPSE Analytics arasında ağ erişimi.
 
## <a name="setting-up-authentication-for-a-scan"></a>Tarama için kimlik doğrulamasını ayarlama

Azure SYNAPSE Analytics için kimlik doğrulaması kurmanın üç yolu vardır:

- Yönetilen Kimlik
- SQL Kimlik Doğrulaması
- Hizmet Sorumlusu

    > [!Note]
    > Yalnızca sunucu düzeyinde asıl oturum açma (sağlama işlemi tarafından oluşturulan) veya `loginmanager` ana veritabanındaki veritabanı rolünün üyeleri yeni oturumlar oluşturabilir. İzin verdikten yaklaşık 15 dakika sürer, purview hesabının kaynakları tarayabilmesi için uygun izinlere sahip olması gerekir.

### <a name="managed-identity-recommended"></a>Yönetilen kimlik (önerilen) 
   
Takip ettiğiniz hesap, sizin oluşturduğunuz sırada önemli bir değer olan kendi yönetilen kimliği vardır. Azure AD [uygulamaları kullanarak Azure AD kullanıcıları oluşturma](../azure-sql/database/authentication-aad-service-principal-tutorial.md)hakkında önkoşulları ve öğreticiyi Izleyerek Azure SYNAPSE Analytics 'te (eskı ADıYLA SQL DW) BIR Azure AD kullanıcısı oluşturmanız gerekir.

Kullanıcı oluşturmak ve izin vermek için örnek SQL söz dizimi:

```sql
CREATE USER [PurviewManagedIdentity] FROM EXTERNAL PROVIDER
GO

EXEC sp_addrolemember 'db_owner', [PurviewManagedIdentity]
GO
```

Kimlik doğrulamanın veritabanı, şemalar ve tablolar için meta verileri almak için izni olmalıdır. Ayrıca sınıflandırmanın örneklemesine yönelik tabloları sorgulayabilmelidir. Bu, kimliğe izin atanamadır `db_owner` .

### <a name="service-principal"></a>Hizmet Sorumlusu

Taramalar için hizmet sorumlusu kimlik doğrulamasını kullanmak için mevcut bir tane kullanabilir veya yeni bir tane oluşturabilirsiniz. 

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

#### <a name="granting-the-service-principal-access-to-your-azure-synapse-analytics-formerly-sql-dw"></a>Azure SYNAPSE Analytics 'e hizmet sorumlusu erişimi verme (eski adıyla SQL DW)

Ayrıca Azure AD [uygulamalarını kullanarak Azure AD kullanıcıları oluşturma](../azure-sql/database/authentication-aad-service-principal-tutorial.md)hakkında önkoşulları ve öğreticiyi Izleyerek Azure SYNAPSE Analytics 'Te BIR Azure AD kullanıcısı oluşturmanız gerekir. Kullanıcı oluşturmak ve izin vermek için örnek SQL söz dizimi:

```sql
CREATE USER [ServicePrincipalName] FROM EXTERNAL PROVIDER
GO

EXEC sp_addrolemember 'db_owner', [ServicePrincipalName]
GO
```

> [!Note]
> Purview 'in taranması için **uygulama (istemci) kimliği** ve **istemci parolası** gerekir.

### <a name="sql-authentication"></a>SQL kimlik doğrulaması

Henüz bir hesabınız yoksa Azure SYNAPSE Analytics (eski adıyla SQL DW) için oturum açma oluşturmak üzere [oturum oluşturma](/sql/t-sql/statements/create-login-transact-sql?view=azure-sqldw-latest&preserve-view=true#examples-1) ' daki yönergeleri izleyebilirsiniz.

Kimlik doğrulama yöntemi seçiliyken **SQL kimlik doğrulaması** olduğunda, parolanızı ve anahtar kasasında deponuzu almanız gerekir:

1. SQL oturum açmanın parolasını alın
1. Anahtar kasanıza gidin
1. **Gizli dizileri > ayarları** seçin
1. **+ Oluştur/Içeri aktar** ' ı SEÇIN ve SQL oturum açmanın *parolası* olarak **adı** ve **değeri** girin
1. Tamamlanacak **Oluştur** ' u seçin
1. Anahtar Kasanızda purview 'a bağlı değilse, [Yeni bir Anahtar Kasası bağlantısı oluşturmanız](manage-credentials.md#create-azure-key-vaults-connections-in-your-azure-purview-account) gerekir
1. Son olarak, taramayı kurmak için anahtarı kullanarak [Yeni bir kimlik bilgisi oluşturun](manage-credentials.md#create-a-new-credential)

## <a name="register-an-azure-synapse-analytics-instance-formerly-sql-dw"></a>Azure SYNAPSE Analytics örneğini kaydetme (eski adıyla SQL DW)

Yeni bir Azure SYNAPSE Analytics sunucusunu veri kataloğunuza kaydetmek için aşağıdakileri yapın:

1. Purview hesabınıza gidin
1. Sol gezinti bölmesinde **kaynakları** seçin
1. **Kaydol** ' u seçin
1. **Kayıt kaynakları** üzerinde **Azure SYNAPSE Analytics (eskı adıyla SQL DW)** seçeneğini belirleyin.
1. **Devam**'ı seçin

**Kaynakları Kaydet (Azure SYNAPSE Analytics)** ekranında şunları yapın:

1. Veri kaynağının katalogda listelenecek bir **ad** girin.
1. İstediğiniz mantıksal SQL Server nasıl işaret etmek istediğinizi seçin:
   1. **Azure aboneliği ' nden** seçim yapın, **Azure aboneliği** açılan kutusundan ilgili aboneliği ve **sunucu adı** açılan kutusundan uygun sunucuyu seçin.
   1. Veya **El Ile gir** ' i seçip **sunucu adı** girebilirsiniz.
1. Veri kaynağını kaydetmek için **son** ' a gidin.

:::image type="content" source="media/register-scan-azure-synapse-analytics/register-sources.png" alt-text="kaynakları kaydetme seçenekleri" border="true":::

[!INCLUDE [create and manage scans](includes/manage-scans.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Azure purview veri kataloğuna gözatın](how-to-browse-catalog.md)
- [Azure purview Veri Kataloğu 'Nda arama](how-to-search-catalog.md)
