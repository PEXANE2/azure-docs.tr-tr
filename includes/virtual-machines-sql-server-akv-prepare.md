---
title: include dosyası
description: include dosyası
services: virtual-machines-windows
author: rothja
manager: craigg
tags: azure-service-management
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/30/2018
ms.author: jroth
ms.custom: include file
ms.openlocfilehash: 2c7d312910c6d38c54b291da34bfb827246c7dad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "79504325"
---
## <a name="prepare-for-akv-integration"></a>AKV Entegrasyonuna Hazırlanın
SQL Server VM'nizi yapılandırmak için Azure Key Vault Tümleştirmesini kullanmak için birkaç ön koşul vardır: 

1. [Azure Powershell'i yükleme](#install)
2. [Azure Etkin Dizini Oluşturma](#register)
3. [Anahtar kasası oluşturma](#createkeyvault)

Aşağıdaki bölümlerde bu ön koşullar ve powershell cmdlets çalıştırmak için toplamanız gereken bilgiler açıklanmıştır.

[!INCLUDE [updated-for-az](./updated-for-az.md)]

### <a name="install-azure-powershell"></a><a id="install"></a>Azure PowerShell'i yükleme
En son Azure PowerShell modüllerini yüklediğinizden emin olun. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/install-az-ps).

### <a name="register-an-application-in-your-azure-active-directory"></a><a id="register"></a>Azure Etkin Dizininizde bir uygulama kaydedin

Öncelikle aboneliğinizde bir [Azure Etkin Dizin](https://azure.microsoft.com/trial/get-started-active-directory/) (AAD) olması gerekir. Birçok yararları arasında, bu belirli kullanıcılar ve uygulamalar için anahtar kasasına izin vermek için izin sağlar.

Ardından, AAD'ye bir uygulama kaydedin. Bu, vm'inizin ihtiyaç dacağı anahtar kasanıza erişimi olan bir Servis Müdürü hesabı verecektir. Azure Key Vault makalesinde, bu adımları [Azure Etkin Dizin bölümüne kaydet bölümünde](../articles/key-vault/key-vault-manage-with-cli2.md#registering-an-application-with-azure-active-directory) bulabilirsiniz veya [bu blog yazısının](https://blogs.technet.com/b/kv/archive/2015/01/09/azure-key-vault-step-by-step.aspx) **uygulama bölümü için kimlik al'da** ekran görüntüleri olan adımları görebilirsiniz. Bu adımları tamamlamadan önce, SQL VM'nizde Azure Key Vault Tümleştirmesini etkinleştirdiğinizde daha sonra gereken bu kayıt sırasında aşağıdaki bilgileri toplamanız gerekir.

* Uygulama eklendikten **sonra, Kayıtlı uygulama** bıçağında **Uygulama Kimliği'ni** (AAD ClientID veya AppID olarak da bilinir) bulun.
    Uygulama kimliği daha sonra Azure Anahtar Kasası Tümleştirmesi'ni etkinleştirmek için PowerShell komut dosyasındaki **$spName** (Hizmet Sorumlusu adı) parametresine atanır.

   ![Uygulama Kimliği](./media/virtual-machines-sql-server-akv-prepare/aad-application-id.png)

* Anahtarınızı oluşturduğunuzda bu adımlar sırasında, aşağıdaki ekran görüntüsünde gösterildiği gibi anahtarınızın sırrını kopyalayın. Bu anahtar sırrı daha sonra PowerShell komut dosyasındaki **$spSecret** (Servis Sorumlusu gizli) parametresine atanır.

   ![AAD sırrı](./media/virtual-machines-sql-server-akv-prepare/aad-sp-secret.png)

* Uygulama kimliği ve gizli de SQL Server bir kimlik oluşturmak için kullanılacaktır.

* Aşağıdaki erişim izinlerine sahip olmak için bu yeni uygulama kimliğini (veya istemci kimliğini) yetkilendirmeniz gerekir: **get**, **wrapKey**, **unwrapKey**. Bu [Set-AzKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) cmdlet ile yapılır. Daha fazla bilgi için Azure [Key Vault genel bakış](../articles/key-vault/key-vault-overview.md)bakın.

### <a name="create-a-key-vault"></a><a id="createkeyvault"></a>Anahtar kasası oluşturma
VM'nizde şifreleme için kullanacağınız anahtarları depolamak için Azure Key Vault'u kullanmak için bir anahtar kasasına erişmeniz gerekir. Anahtar kasanızı daha önce ayarlamadıysanız, [Azure Anahtar Kasası ile Başlar'daki](../articles/key-vault/key-vault-overview.md) adımları izleyerek bir tane oluşturun. Bu adımları tamamlamadan önce, bu kurulum sırasında toplamanız gereken ve daha sonra SQL VM'nizde Azure Anahtar Kasası Tümleştirmesini etkinleştirdiğinizde gereken bazı bilgiler vardır.

    New-AzKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

Önemli bir kasa oluşturma adımına vardığınızda, anahtar kasa URL'si olan iade edilen **vaultUri** özelliğine dikkat edin. Bu adımda sağlanan örnekte, aşağıda gösterildiği gibi, anahtar tonoz adı ContosoKeyVault, bu nedenle anahtar tonoz URL olacaktır https://contosokeyvault.vault.azure.net/.

Anahtar kasa URL'si daha sonra Azure Anahtar Kasası Tümleştirmesi'ni etkinleştirmek için PowerShell komut dosyasındaki **$akvURL** parametresine atanır.

Anahtar kasası oluşturulduktan sonra, anahtar kasasına bir anahtar eklememiz gerekir, bu anahtar daha sonra SQL Server'da asimetrik bir anahtar oluşturduğımızda sevk edilecektir.
