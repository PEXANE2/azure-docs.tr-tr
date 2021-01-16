---
title: Azure SQL mantıksal sunucunuz için Azure kanıtlama yapılandırma
description: Azure SQL veritabanı 'nda güvenli şifreli Always Encrypted için Azure kanıtlama 'nı yapılandırın.
keywords: verileri şifreleme, SQL şifreleme, veritabanı şifreleme, hassas veriler, Always Encrypted, güvenli şifreleme, SGX, kanıtlama
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.devlang: ''
ms.topic: how-to
author: jaszymas
ms.author: jaszymas
ms.reviwer: vanto
ms.date: 01/15/2021
ms.openlocfilehash: e8cb423d4d700c4b6b6caa30a02eac3e7ef10cb6
ms.sourcegitcommit: 25d1d5eb0329c14367621924e1da19af0a99acf1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98253578"
---
# <a name="configure-azure-attestation-for-your-azure-sql-logical-server"></a>Azure SQL mantıksal sunucunuz için Azure kanıtlama yapılandırma

[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

> [!NOTE]
> Azure SQL veritabanı için güvenli şifreleme ile Always Encrypted Şu anda **genel önizleme** aşamasındadır.

[Microsoft Azure kanıtlama](../../attestation/overview.md) , Intel Software Guard uzantıları (Intel SGX) şifrelemesi de dahil olmak üzere, güvenilir yürütme ortamlarının (TEEs) sızmasını sağlayan bir çözümdür. 

Azure SQL veritabanı 'nda güvenli kuşışları [olan Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-enclaves) Için kullanılan Intel SGX şifrelemesi Için Azure kanıtlama 'nı kullanmak üzere şunları yapmanız gerekir:

1. Bir [kanıtlama sağlayıcısı](../../attestation/basic-concepts.md#attestation-provider) oluşturun ve önerilen kanıtlama ilkesiyle yapılandırın.

2. Azure SQL mantıksal sunucunuza kanıtlama sağlayıcınıza erişim izni verin.

> [!NOTE]
> Kanıtlama yapılandırması, kanıtlama yöneticisinin sorumluluğundadır. [SGX şifreleme ve kanıtlama yapılandırılırken rol ve sorumluluklara](always-encrypted-enclaves-plan.md#roles-and-responsibilities-when-configuring-sgx-enclaves-and-attestation)bakın.

## <a name="requirements"></a>Gereksinimler

Azure SQL mantıksal sunucusu ve kanıtlama sağlayıcısı aynı Azure Active Directory kiracısına ait olmalıdır. Çapraz kiracı etkileşimleri desteklenmez. 

Azure SQL mantıksal sunucusuna atanmış bir Azure AD kimliği olmalıdır. Kanıtlama Yöneticisi olarak, sunucunun Azure AD kimliğini bu sunucu için Azure SQL veritabanı yöneticisinden edinmeniz gerekir. Kimliği, kanıtlama sağlayıcısına sunucuya erişim vermek için kullanacaksınız. 

PowerShell ve Azure CLı kullanarak var olan bir sunucuya kimlik ile sunucu oluşturma veya kimlik atama hakkında yönergeler için bkz. [sunucunuza bir Azure ad kimliği atama](transparent-data-encryption-byok-configure.md#assign-an-azure-active-directory-azure-ad-identity-to-your-server).

## <a name="create-and-configure-an-attestation-provider"></a>Kanıtlama sağlayıcısı oluşturma ve yapılandırma

[Kanıtlama sağlayıcısı](../../attestation/basic-concepts.md#attestation-provider) , kanıtlama [ilkelerine](../../attestation/basic-concepts.md#attestation-request) ve sorunlar [kanıtlama belirteçlerine](../../attestation/basic-concepts.md#attestation-token)karşı [kanıtlama isteklerini](../../attestation/basic-concepts.md#attestation-request) değerlendiren Azure kanıtlama 'nda bir kaynaktır. 

Kanıtlama ilkeleri, [talep kuralı dilbilgisi](../../attestation/claim-rule-grammar.md)kullanılarak belirtilir.

Microsoft, Azure SQL veritabanı 'nda Always Encrypted için kullanılan Intel SGX şifrelemesi için aşağıdaki ilkeyi önerir:

```output
version= 1.0;
authorizationrules 
{
       [ type=="x-ms-sgx-is-debuggable", value==false ]
        && [ type=="x-ms-sgx-product-id", value==4639 ]
        && [ type=="x-ms-sgx-svn", value>= 0 ]
        && [ type=="x-ms-sgx-mrsigner", value=="e31c9e505f37a58de09335075fc8591254313eb20bb1a27e5443cc450b6e33e5"] 
    => permit();
};
```

Yukarıdaki ilke şunları doğrular:

- Azure SQL veritabanı 'nın içindeki kuşatma, hata ayıklamayı desteklemez (Bu, kuşmanın sağladığı koruma düzeyini azaltır).
- Kuşın içindeki kitaplığın ürün KIMLIĞI, güvenli şifreleme (4639) ile Always Encrypted atanan ürün KIMLIĞIDIR.
- Kitaplığın sürüm KIMLIĞI (SVN) 0 ' dan büyük.
- Kuşdaki kitaplık, Microsoft imzalama anahtarı kullanılarak imzalanmış (x-MS-SGX-mrimzalayan talebinin değeri İmzalama anahtarının karmasıdır).

> [!IMPORTANT]
> Bir kanıtlama sağlayıcısı, şifreleme içinde çalışan kodu doğrulamakta olan Intel SGX enclaven için varsayılan ilkeyle oluşturulur. Microsoft, yukarıda önerilen ilkeyi ayarlamanıza ve güvenli şifreleme ile Always Encrypted için varsayılan ilkeyi kullanmamanız önerilir.

Kanıtlama sağlayıcısı oluşturma ve kullanarak bir kanıtlama ilkesiyle yapılandırma yönergeleri için:

- [Hızlı başlangıç: Azure portal ile Azure kanıtlama ayarlama](../../attestation/quickstart-portal.md)
    > [!IMPORTANT]
    > Kanıtlama ilkenizi Azure portal ile yapılandırdığınızda, kanıtlama türünü olarak ayarlayın `SGX-IntelSDK` .
- [Hızlı başlangıç: Azure PowerShell ile Azure kanıtlama ayarlama](../../attestation/quickstart-powershell.md)
    > [!IMPORTANT]
    > Kanıtlama ilkenizi Azure PowerShell ile yapılandırdığınızda, `Tee` parametresini olarak ayarlayın `SgxEnclave` .
- [Hızlı başlangıç: Azure CLı ile Azure kanıtlama ayarlama](../../attestation/quickstart-azure-cli.md)
    > [!IMPORTANT]
    > Kanıtlama ilkenizi Azure CLı ile yapılandırdığınızda, `attestation-type` parametresini olarak ayarlayın `SGX-IntelSDK` .

## <a name="determine-the-attestation-url-for-your-attestation-policy"></a>Kanıtlama ilkeniz için kanıtlama URL 'sini belirleme

Bir kanıtlama ilkesini yapılandırdıktan sonra, Azure SQL veritabanı 'nda güvenli şifreleme ile Always Encrypted kullanan uygulama yöneticileri ilkesine başvurarak kanıtlama URL 'sini paylaşmanız gerekir. Uygulama yöneticileri veya/ve uygulama kullanıcılarının, güvenli şifreleme kullanan deyimler çalıştırabilmeleri için kendi uygulamalarını kanıtlama URL 'siyle yapılandırması gerekir.

### <a name="use-powershell-to-determine-the-attestation-url"></a>Kanıtlama URL 'sini öğrenmek için PowerShell 'i kullanma

Kanıtlama URL 'nizi öğrenmek için aşağıdaki betiği kullanın:

```powershell
$attestationProvider = Get-AzAttestation -Name $attestationProviderName -ResourceGroupName $attestationResourceGroupName 
$attestationUrl = $attestationProvider.AttestUri + “/attest/SgxEnclave”
Write-Host "Your attestation URL is: " $attestationUrl 
```

### <a name="use-azure-portal-to-determine-the-attestation-url"></a>Kanıtlama URL 'sini öğrenmek için Azure portal kullanın

1. Kanıtlama sağlayıcınızın Genel Bakış bölmesinde, attest URI özelliğinin değerini panoya kopyalayın. Attest URI 'SI şöyle görünmelidir: `https://MyAttestationProvider.us.attest.azure.net` .

2. Şunu attest URI 'sine ekleyin: `/attest/SgxEnclave` . 

Elde edilen kanıtlama URL 'SI şöyle görünmelidir: `https://MyAttestationProvider.us.attest.azure.net/attest/SgxEnclave`

## <a name="grant-your-azure-sql-logical-server-access-to-your-attestation-provider"></a>Azure SQL mantıksal sunucunuza kanıtlama sağlayıcınıza erişim izni verin

Kanıtlama iş akışı sırasında, veritabanınızı içeren Azure SQL mantıksal sunucusu, kanıtlama isteği göndermek için kanıtlama sağlayıcısını çağırır. Azure SQL mantıksal sunucusu 'nun kanıtlama istekleri gönderebilmesi için, sunucunun `Microsoft.Attestation/attestationProviders/attestation/read` kanıtlama sağlayıcısı üzerinde eylem için bir izni olması gerekir. İzin vermek için önerilen yol, kanıtlama sağlayıcısının yöneticisinin, kanıtlama sağlayıcısı için kanıtlama okuyucusu rolüne veya kapsayan kaynak grubuna atamasını sağlar.

### <a name="use-azure-portal-to-assign-permission"></a>İzin atamak için Azure portal kullanın

Bir Azure SQL sunucusunun kimliğini kanıtlama sağlayıcısına yönelik kanıtlama okuyucusu rolüne atamak için, [Azure Portal kullanarak Azure rol atamaları ekleme veya kaldırma](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)konusundaki genel yönergeleri izleyin. **Rol ataması Ekle** bölmesindeyken:

1. **Rol** açılır penceresinde, **kanıtlama okuyucusu** rolünü seçin.
1. **Seç** alanına, arama yapmak IÇIN Azure SQL sunucunuzun adını girin.

Örnek için aşağıdaki ekran görüntüsüne bakın.

![kanıtlama okuyucusu rol ataması](./media/always-encrypted-enclaves/attestation-provider-role-assigment.png)

> [!NOTE]
> Bir sunucunun **rol ataması Ekle** bölmesinde görünmesi için, sunucuda BIR Azure ad kimliği atanmış olması gerekir. bkz. [gereksinimleri](#requirements).

### <a name="use-powershell-to-assign-permission"></a>İzin atamak için PowerShell 'i kullanma

1. Azure SQL mantıksal sunucunuzu bulun.

```powershell
$serverResourceGroupName = "<server resource group name>"
$serverName = "<server name>" 
$server = Get-AzSqlServer -ServerName $serverName -ResourceGroupName
```
 
2. Kanıtlama sağlayıcınızı içeren kaynak grubu için sunucuyu kanıtlama okuyucusu rolüne atayın.

```powershell
$attestationResourceGroupName = "<attestation provider resource group name>"
New-AzRoleAssignment -ObjectId $server.Identity.PrincipalId -RoleDefinitionName "Attestation Reader" -ResourceGroupName $attestationResourceGroupName
```

Daha fazla bilgi için bkz. [Azure PowerShell kullanarak Azure rol atamaları ekleme veya kaldırma](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell#add-a-role-assignment).

## <a name="next-steps"></a>Sonraki Adımlar

- [Güvenli kuşatmalarla Always Encrypted için anahtarları yönetme](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-enclaves-manage-keys)

## <a name="see-also"></a>Ayrıca bkz.

- [Öğretici: Azure SQL veritabanında güvenli şifreleme ile Always Encrypted kullanmaya başlama](always-encrypted-enclaves-getting-started.md)