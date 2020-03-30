---
title: Sanal ağ kurallarını kullanın - Azure CLI - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Bu makalede, Azure CLI komut satırını kullanarak PostgreSQL için Azure Veritabanı için VNet hizmet uç noktalarının ve kurallarının nasıl oluşturulup yönetilen anlatılmaktadır.
author: bolzmj
ms.author: mbolz
ms.service: postgresql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 5e80b48e43e72bc3eda16a5dcc64f980065c923f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75974119"
---
# <a name="create-and-manage-vnet-service-endpoints-for-azure-database-for-postgresql---single-server-using-azure-cli"></a>Azure CLI kullanarak PostgreSQL - Single Server için Azure Veritabanı için VNet hizmet bitiş noktaları oluşturma ve yönetme
Sanal Ağ (VNet) hizmetleri bitiş noktaları ve kuralları, Bir Sanal Ağ'ın özel adres alanını PostgreSQL sunucusu için Azure Veritabanınıza genişletir. Kullanışlı Azure Komut Satırı Arabirimi (CLI) komutlarını kullanarak, sunucunuzu yönetmek için VNet hizmet bitiş noktalarını ve kurallarını oluşturabilir, güncelleyebilir, silebilir, listeleyebilir ve gösterebilirsiniz. Sınırlamalar da dahil olmak üzere PostgreSQL VNet hizmet bitiş noktaları için Azure Veritabanı'na genel bakış için, [PostgreSQL Server VNet hizmet bitiş noktaları için Azure Veritabanı'na](concepts-data-access-and-security-vnet.md)bakın. VNet hizmet bitiş noktaları, PostgreSQL için Azure Veritabanı için desteklenen tüm bölgelerde kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılacağını kılavuzunda adım atmak için şunları yapmanız gerekir:
- [Azure CLI'yi yükleyin](/cli/azure/install-azure-cli) veya tarayıcıda Azure Bulut Kabuğu'nu kullanın.
- [PostgreSQL sunucusu ve veritabanı için](quickstart-create-server-database-azure-cli.md)bir Azure Veritabanı.

> [!NOTE]
> VNet hizmet uç noktaları için destek yalnızca Genel Amaç ve Bellek Optimize Edilmiş sunucular içindir.
> VNet'in eşlemesi durumunda, trafik hizmet bitiş noktalarıyla ortak bir VNet Ağ Geçidi'nden akıyorsa ve eşe akması gerekiyorsa, lütfen Ağ Geçidi VNet'teki Azure Sanal Makinelerin PostgreSQL sunucusu için Azure Veritabanı'na erişmesine izin vermek için bir ACL/VNet kuralı oluşturun.


## <a name="configure-vnet-service-endpoints-for-azure-database-for-postgresql"></a>PostgreSQL için Azure Veritabanı için Vnet hizmet bitiş noktalarını yapılandırma
[Az ağ vnet](https://docs.microsoft.com/cli/azure/network/vnet?view=azure-cli-latest) komutları Sanal Ağları yapılandırmak için kullanılır.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Yüklü sürümü görmek için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli). 

CLI’yi yerel olarak çalıştırıyorsanız, [az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest) komutunu kullanarak hesabınızda oturum açmanız gerekir. Komut çıktısındaki ilgili abonelik adına karşılık gelen **id** özelliğinin değerini not edin.
```azurecli-interactive
az login
```

Birden fazla aboneliğiniz varsa kaynağın faturalanacağı uygun aboneliği seçin. [az account set](https://docs.microsoft.com/cli/azure/account?view=azure-cli-latest#az-account-set) komutunu kullanarak hesabınız altındaki belirli bir abonelik kimliğini seçin. Aboneliğinizin **az login** çıktısındaki **id** özelliğini abonelik kimliği yer tutucusuyla değiştirin.

- Hesap, sanal ağ ve hizmet uç noktası oluşturma izinlerine sahip olmalıdır.

Hizmet bitiş noktaları, sanal ağa yazma erişimi olan bir kullanıcı tarafından sanal ağlarda bağımsız olarak yapılandırılabilir.

Azure hizmet kaynaklarını bir VNet'e sabitlemek için, kullanıcının eklenen alt ağlar için "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/" iznine sahip olması gerekir. Bu izin varsayılan olarak yerleşik hizmet yöneticisi rollerinde mevcuttur ve özel roller oluşturularak değiştirilebilir.

[Yerleşik roller](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) ve [özel rollere](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles) belirli izinlerin atanması hakkında daha fazla bilgi edinin.

Sanal ağlar ve Azure hizmet kaynakları aynı ağda veya farklı aboneliklerde olabilir. VNet ve Azure hizmet kaynakları farklı aboneliklerdeyse, kaynakların aynı Etkin Dizin (AD) kiracısı altında olması gerekir. Her iki aboneliğin de **Microsoft.Sql** kaynak sağlayıcısının kayıtlı olduğundan emin olun. Daha fazla bilgi için [kaynak yöneticisi-kayıt][resource-manager-portal]

> [!IMPORTANT]
> Aşağıdaki örnek komut dosyasını çalıştırmadan veya hizmet bitiş noktalarını yapılandırmadan önce hizmet bitiş noktası yapılandırmaları ve dikkat edilmesi gereken hususlar hakkındaki bu makaleyi okumanız önerilir. **Sanal Ağ hizmeti bitiş noktası:** [Sanal Ağ hizmeti bitiş noktası,](../virtual-network/virtual-network-service-endpoints-overview.md) özellik değerleri bir veya daha fazla resmi Azure hizmet türü adlarını içeren bir alt ağdır. VNet hizmetleri bitiş noktaları, SQL Veritabanı adlı Azure hizmetini ifade eden **Microsoft.Sql**hizmet türü adını kullanır. Bu hizmet etiketi, Azure SQL Veritabanı, PostgreSQL için Azure Veritabanı ve MySQL hizmetleri için de geçerlidir. **Microsoft.Sql** hizmet etiketini vnet hizmet bitiş noktasına uygularken, azure SQL Veritabanı, PostgreSQL için Azure Veritabanı ve alt ağdaki MySQL sunucuları için Azure Veritabanı dahil olmak üzere tüm Azure Veritabanı hizmetleri için hizmet bitiş noktası trafiğini yapılandırMaktadır. 
> 

### <a name="sample-script-to-create-an-azure-database-for-postgresql-database-create-a-vnet-vnet-service-endpoint-and-secure-the-server-to-the-subnet-with-a-vnet-rule"></a>PostgreSQL veritabanı için bir Azure Veritabanı oluşturmak, VNet, VNet hizmet bitiş noktası oluşturmak ve sunucuyu VNet kuralıyla alt ağa sabitlemek için örnek komut dosyası
Bu örnek betikte, vurgulanan satırları değiştirerek yönetici kullanıcı adını ve parolasını özelleştirin. `az account set --subscription` Komutta kullanılan Abonelik Kimliği'ni kendi abonelik tanımlayıcınızla değiştirin.
[!code-azurecli-interactive[main](../../cli_scripts/postgresql/create-postgresql-server-vnet/create-postgresql-server.sh?highlight=5,20 "Create an Azure Database for PostgreSQL, VNet, VNet service endpoint, and VNet rule.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Betik örneği çalıştırıldıktan sonra, kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.
[!code-azurecli-interactive[main](../../cli_scripts/postgresql/create-postgresql-server-vnet/delete-postgresql.sh "Delete the resource group.")]

<!-- Link references, to text, Within this same GitHub repo. --> 
[resource-manager-portal]: ../azure-resource-manager/management/resource-providers-and-types.md