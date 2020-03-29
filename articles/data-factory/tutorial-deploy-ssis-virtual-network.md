---
title: Sanal ağa katılmak için Azure-SSIS tümleştirme çalışma süresini yapılandırma kalım
description: Azure sanal ağında bir Azure-SSIS tümleştirme çalışma süresine nasıl katılacağınızı öğrenin.
services: data-factory
documentationcenter: ''
author: chugugrace
ms.author: chugu
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 1/10/2020
ms.openlocfilehash: 065610a9de4898d012cef8a16849c09a81f0774c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76841106"
---
# <a name="configure-an-azure-sql-server-integration-services-ssis-integration-runtime-ir-to-join-a-virtual-network"></a>Sanal ağa katılmak için Azure-SQL Server Integration Services (SSIS) tümleştirme çalışma süresini (IR) yapılandırma

Bu öğretici, azure portalını kullanarak sanal ağa katılmak üzere bir Azure-SQL Server Integration Services (SSIS) tümleştirme çalışma süresini (IR) yapılandırmak için temel adımlar sağlar.

Adımlar şunlardır:

- Sanal ağı yapılandırın.
- Azure Veri Fabrikası portalından sanal ağa Azure-SSIS IR'ye katılın.

## <a name="prerequisites"></a>Ön koşullar

- **Azure-SSIS tümleştirme çalışma zamanı.** Bir Azure-SSIS tümleştirme çalışma saatiniz yoksa, başlamadan önce [Azure Veri Fabrikası'nda bir Azure-SSIS tümleştirme çalışma süresi sağlayın.](tutorial-deploy-ssis-packages-azure.md)

- **Kullanıcı izni**. Azure-SSIS IR'yi oluşturan kullanıcı, aşağıdaki seçeneklerden biriyle en azından Azure Veri Fabrikası kaynağında [rol ataması](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-list-portal#list-role-assignments-for-a-user-at-a-scope) olmalıdır:

    - Yerleşik Ağ Katılımcısı rolünü kullanın. Bu rol, gerekenden çok daha büyük bir kapsama sahip olan _Microsoft.Network/\* _ izniyle birlikte gelir.
    - Yalnızca gerekli _Microsoft.Network/virtualNetworks/\*/join/action_ iznini içeren özel bir rol oluşturun. Azure Kaynak Yöneticisi sanal ağına katılırken Azure-SSIS IR için kendi genel IP adreslerinizi de getirmek istiyorsanız, lütfen role _Microsoft.Network/publicIPAddresses/*/join/action_ izni ekleyin.

- **Sanal ağ**.

    - Sanal ağınız yoksa, [Azure portalını kullanarak sanal ağ oluşturun.](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

    - Sanal ağın kaynak grubunun belirli Azure ağ kaynaklarını oluşturup silebileceğinden emin olun.
    
        Azure-SSIS IR'nin sanal ağla aynı kaynak grubu altında belirli ağ kaynakları oluşturması gerekir. Bu kaynaklar şunlardır:
        - * \<Guid>-azurebatch-cloudserviceloadbalancer adını taşıyan bir Azure yük dengeleyicisi*
        - *\<Guid>-azurebatch-cloudservicenetworksecuritygroup adında bir ağ güvenlik grubu
        - -azurebatch-cloudservicepublicip adında bir Azure genel IP adresi
    
        Bu kaynaklar Azure-SSIS IR'niz başladığında oluşturulur. Azure-SSIS IR'niz durduğunda silinirler. Azure-SSIS IR'nizin durmasını engellemek için bu ağ kaynaklarını diğer kaynaklarınızda yeniden kullanmayın.

    - Sanal ağın ait olduğu kaynak grubunda/aboneliğinde [kaynak kilidi](https://docs.microsoft.com/azure/azure-resource-manager/management/lock-resources) olmadığından emin olun. Salt okunur/sil kilidi yapılandırırsanız, Azure-SSIS IR'nizi başlatma ve durdurma başarısız olur veya yanıt vermeyi durdurur.

    - Sanal ağın ait olduğu kaynak grubu/aboneliği altında aşağıdaki kaynakların oluşturulmasını engelleyen bir Azure ilkeniz olmadığından emin olun:
        - Microsoft.Network/LoadBalancers
        - Microsoft.Network/AğGüvenliği Grupları

- Aşağıda **ağ yapılandırma** senaryoları bu öğreticide yer alan değildir:
    - Azure-SSIS IR için kendi genel IP adreslerinizi getirirseniz.
    - Kendi Etki Alanı Adı Sistemi (DNS) sunucunuzu kullanıyorsanız.
    - Alt ağda bir ağ güvenlik grubu (NSG) kullanıyorsanız.
    - Azure ExpressRoute veya kullanıcı tanımlı bir rota (UDR) kullanıyorsanız.
    - Özelleştirilmiş Azure-SSIS IR kullanıyorsanız.
    
    Daha fazla bilgi için [sanal ağ yapılandırmayı](join-azure-ssis-integration-runtime-virtual-network.md#virtual-network-configuration)denetleyin.

## <a name="configure-a-virtual-network"></a>Sanal ağ yapılandırma

Azure-SSIS IR'ye katılmaya çalışmadan önce sanal ağı yapılandırmak için Azure portalını kullanın.

1. Microsoft Edge veya Google Chrome'u başlatın. Şu anda, yalnızca bu web tarayıcıları Veri Fabrikası UI'yi destekler.

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. **Diğer hizmetleri**seçin. **Sanal ağlar**için filtre uygulayın ve seçin.

1. Listedeki sanal ağınıza filtre uygulayın ve seçin.

1. Sanal **ağ** sayfasında **Özellikler'i**seçin.

1. Sanal ağ için kaynak kimliğini panoya kopyalamak için **KAYNAK Kimliği** için kopyalama düğmesini seçin. Kimliği OneNote'taki panodan veya bir dosyadan kaydedin.

1. Sol menüde **Alt Ağlar'ı**seçin.

    - Seçtiğiniz alt ağın Azure-SSIS IR'nin kullanabileceği yeterli kullanılabilir adres alanına sahip olduğundan emin olun. Kullanılabilir IP adreslerini IR düğümü numarasının en az iki katı olarak bırakın. Azure, her alt ağda bazı IP adresleri ayırır. Bu adresler kullanılamaz. Alt ağların ilk ve son IP adresleri protokol uyumluluğu için ayrılmıştır ve Azure hizmetleri için üç adres daha kullanılır. Daha fazla bilgi için, bu [alt ağlar içinde IP adresleri nin kullanılmasıyla ilgili herhangi bir kısıtlama var mı?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)
    - Azure-SSIS IR dağıtmak için GatewaySubnet'i seçmeyin. Sanal ağ ağ geçitleri için ayrılmıştır.
    - Yalnızca diğer Azure hizmetleri tarafından işgal edilen bir alt ağ kullanmayın (örneğin, SQL Veritabanı yönetilen örneği, Uygulama Hizmeti vb.).

1. Azure Toplu İş sağlayıcısının sanal ağa sahip Azure aboneliğinde kayıtlı olduğunu doğrulayın. Veya Azure Toplu İş sağlayıcısını kaydedin. Aboneliğinizde zaten bir Azure Toplu İş hesabınız varsa, aboneliğiniz Azure Toplu İş için kaydedilir. (Veri Fabrikası portalında Azure-SSIS IR oluşturursanız, Azure Toplu İşlem sağlayıcısı sizin için otomatik olarak kaydedilir.)

   1. Azure portalında, sol menüde **Abonelikler'i**seçin.

   1. Aboneliğinizi seçin.

   1. Solda, Kaynak **sağlayıcılarını**seçin ve **Microsoft.Batch'In** kayıtlı bir sağlayıcı olduğunu onaylayın.

   !["Kayıtlı" durumunun teyidi](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   **Microsoft.Batch'i** listede göremiyorsanız, kaydolmak için aboneliğinizde [boş bir Azure Toplu İş hesabı oluşturun.](../batch/batch-account-create-portal.md) Daha sonra silebilirsiniz.

## <a name="join-the-azure-ssis-ir-to-a-virtual-network"></a>Azure-SSIS IR'ye sanal ağda katılma

Azure Kaynak Yöneticisi sanal ağınızı veya klasik sanal ağınızı yapılandırdıktan sonra, Azure-SSIS IR'ye sanal ağa katılabilirsiniz:

1. Microsoft Edge veya Google Chrome'u başlatın. Şu anda, yalnızca bu web tarayıcıları Veri Fabrikası UI'yi destekler.

1. Sol menüdeki [Azure portalında](https://portal.azure.com) **Veri fabrikalarını**seçin. Menüde **Veri fabrikalarını** görmüyorsanız, Daha **fazla hizmet**seçin ve ardından INTELLIGENCE **+ ANALYTICS** bölümünde **Veri fabrikalarını**seçin.

   ![Veri fabrikaları listesi](media/join-azure-ssis-integration-runtime-virtual-network/data-factories-list.png)

1. Listede Azure-SSIS IR ile veri fabrikanızı seçin. Veri fabrikanızın ana sayfasını görürsünüz. Yazar **& Monitör** döşemesini seçin. Ayrı bir sekmede Veri Fabrikası UI'sini görürsünüz.

   ![Data factory giriş sayfası](media/join-azure-ssis-integration-runtime-virtual-network/data-factory-home-page.png)

1. Veri Fabrikası UI'sinde, **Edit** sekmesine geçin, **Bağlantılar'ı**seçin ve **Tümleştirme Runtimes** sekmesine geçin.

   !["Tümleştirme çalışma saatleri" sekmesi](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtimes-tab.png)

1. Azure-SSIS IR'niz çalışıyorsa, **Tümleştirme Çalışma Saatleri** listesinde, **Eylemler** sütununda, Azure-SSIS IR'niz için **Durdur** düğmesini seçin. Durdurana kadar Azure-SSIS IR'nizi kaldıramazsınız.

   ![IR'yi durdurun](media/join-azure-ssis-integration-runtime-virtual-network/stop-ir-button.png)

1. **Tümleştirme Çalışma Saatleri** listesinde, **Eylemler** sütununda Azure-SSIS IR'niz için **Edit** düğmesini seçin.

   ![Tümleştirme çalışma süresini yönetme](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtime-edit.png)

1. Tümleştirme çalışma zamanı kurulum panelinde, **Sonraki** düğmesini seçerek **Genel Ayarlar** ve **SQL Ayarları** bölümlerinde ilerleyin.

1. Gelişmiş **Ayarlar** bölümünde:
   1. Katılmak **için Azure-SSIS Tümleştirme Çalışma süreniz için Bir VNet Seçin' i seçin, ADF' nin belirli ağ kaynakları oluşturmasına izin verin ve isteğe bağlı olarak kendi statik genel IP adreslerinizi** onay kutlayın.

   1. **Abonelik**için, sanal ağınıza sahip Azure aboneliğini seçin.

   1. **Konum**için, tümleştirme çalışma sürenizin aynı konumu seçilir.

   1. **Türü**için sanal ağınızın türünü seçin: klasik veya Azure Kaynak Yöneticisi. Klasik sanal ağlar yakında küçümseneceği için bir Azure Kaynak Yöneticisi sanal ağı seçmenizi öneririz.

   1. **VNet Adı**için sanal ağınızın adını seçin. Sanal ağ hizmeti uç noktaları veya yönetilen örneği ile Azure SQL Veritabanı sunucunuz için kullanılan ın aynısı olmalıdır. Ya da şirket içi ağınıza bağlı olanın aynısı olmalıdır. Aksi takdirde, Azure-SSIS IR için kendi statik genel IP adreslerinizi getirmek herhangi bir sanal ağ olabilir.

   1. **Subnet Adı**için, sanal ağınız için alt ağ adını seçin. SSISDB'yi barındırmak için sanal ağ hizmeti uç noktalarına sahip Azure SQL Veritabanı sunucunuz için kullanılanın aynısı olmalıdır. Ya da SSISDB'yi barındırmak için özel bitiş noktası olan yönetilen örneğiniz için kullanılandan farklı bir alt ağ olmalıdır. Aksi takdirde, Azure-SSIS IR için kendi statik genel IP adreslerinizi getirmek herhangi bir alt ağ olabilir.

   1. **VNet Doğrulama'yı**seçin. Doğrulama başarılı olursa, **Devam et'i**seçin.

   ![Bir sanal ağda gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)

1. **Özet** bölümünde, Azure-SSIS IR'nizin tüm ayarlarını gözden geçirin. Sonra **Güncelleştir'i**seçin.

1. Azure-SSIS IR'niz için **Eylemler** sütunundaki **Başlat** düğmesini seçerek Azure-SSIS IR'nizi başlatın. Sanal bir ağa katılan Azure-SSIS IR'yi başlatmak yaklaşık 20 ila 30 dakika sürer.

## <a name="next-steps"></a>Sonraki Adımlar

[Azure-SSIS IR'yi sanal ağa katılma](join-azure-ssis-integration-runtime-virtual-network.md)hakkında daha fazla bilgi edinin.
