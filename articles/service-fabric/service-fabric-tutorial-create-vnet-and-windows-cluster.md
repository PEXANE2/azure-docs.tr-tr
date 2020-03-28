---
title: Azure'da Windows çalıştıran Bir Hizmet Kumaşı kümesi oluşturma
description: Bu eğitimde, PowerShell'i kullanarak Bir Windows Service Fabric kümesini Azure sanal ağına ve ağ güvenlik grubuna nasıl dağıtabileceğinizi öğrenirsiniz.
ms.topic: tutorial
ms.date: 07/22/2019
ms.custom: mvc
ms.openlocfilehash: 086379e788966b300f988e06ec42c94b880b8281
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75551735"
---
# <a name="tutorial-deploy-a-service-fabric-cluster-running-windows-into-an-azure-virtual-network"></a>Öğretici: Windows çalıştıran bir Hizmet Kumaşı kümesini Azure sanal ağına dağıtma

Bu öğretici, bir dizinin birinci bölümüdür. PowerShell ve şablonu kullanarak Windows çalıştıran bir Azure Hizmet Dokusu kümesini Azure [sanal ağ](../virtual-network/virtual-networks-overview.md) ve ağ [güvenlik grubuna](../virtual-network/virtual-networks-nsg.md) nasıl dağıtabileceğinizi öğrenirsiniz. İşi bittiğinde, bulutta çalışan ve uygulamaları dağıtabileceğiniz bir kümeniz vardır. Azure CLI'yi kullanan bir Linux kümesi oluşturmak için [bkz.](service-fabric-tutorial-create-vnet-and-linux-cluster.md)

Bu öğreticide bir üretim senaryosu açıklanır. Sınama amacıyla daha küçük bir küme oluşturmak istiyorsanız, [bkz.](./scripts/service-fabric-powershell-create-secure-cluster-cert.md)

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * PowerShell kullanarak Azure’da VNet oluşturma
> * Anahtar kasası oluşturma ve karşıya sertifika yükleme
> * Azure Active Directory kimlik doğrulaması kurulumu
> * Tanılama koleksiyonunu yapılandırma
> * EventStore hizmetini ayarlama
> * Azure Monitör günlüklerini ayarlama
> * Azure PowerShell’de güvenli bir Service Fabric kümesi oluşturma
> * X.509 sertifikasıyla kümenin güvenliğini sağlama
> * PowerShell kullanarak kümeye bağlanma
> * Bir kümeyi kaldırma

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Azure’da güvenli bir küme oluşturma
> * [Bir kümeyi izleme](service-fabric-tutorial-monitor-cluster.md)
> * [Bir kümenin ölçeğini daraltma veya genişletme](service-fabric-tutorial-scale-cluster.md)
> * [Bir kümenin çalışma zamanını yükseltme](service-fabric-tutorial-upgrade-cluster.md)
> * [Küme silme](service-fabric-tutorial-delete-cluster.md)


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce:

* Azure aboneliğiniz yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)oluşturun.
* Servis [Kumaşı SDK ve PowerShell modüllerini](service-fabric-get-started.md)yükleyin.
* [Azure Powershell'i](https://docs.microsoft.com/powershell/azure/install-Az-ps)yükleyin.
* Azure kümelerinin temel kavramlarını gözden [geçirin.](service-fabric-azure-clusters-overview.md)
* Bir üretim kümesi dağıtımı [planlayın ve hazırlayın.](service-fabric-cluster-azure-deployment-preparation.md)

Aşağıdaki yordamlar yedi düğümlü Hizmet Kumaş kümesi oluşturur. Azure'da bir Hizmet Kumaşı kümesini çalıştırarak oluşan maliyeti hesaplamak için [Azure Fiyatlandırma Hesaplayıcısı'nı](https://azure.microsoft.com/pricing/calculator/) kullanın.

## <a name="download-and-explore-the-template"></a>Şablonu indirin ve keşfedin

Aşağıdaki Azure Kaynak Yöneticisi şablon dosyalarını indirin:

* [azuredeploy.json][template]
* [azuredeploy.parameters.json][parameters]

Bu şablon, bir sanal ağa ve ağ güvenlik grubuna yedi sanal makine ve üç düğüm türünden oluşan güvenli bir küme dağıtMaktadır.  Diğer örnek şablonlar [GitHub](https://github.com/Azure-Samples/service-fabric-cluster-templates)'da bulunabilir. [Azuredeploy.json][template] aşağıdakiler de dahil olmak üzere bir dizi kaynak dağıtmaktadır.

### <a name="service-fabric-cluster"></a>Service Fabric kümesi

**Microsoft.ServiceFabric/clusters** kaynağında şu özelliklere sahip bir Windows kümesi yapılandırılır:

* Üç düğüm türü.
* Birincil düğüm türünde beş düğüm (şablon parametrelerinde yapılandırılabilir) ve diğer iki düğüm türünün her birinde bir düğüm.
* İşletim Sistemi: Windows Server 2016 Kapsayıcılı Veri Merkezi (şablon parametrelerinde yapılandırılabilir).
* Sertifika güvenli (şablon parametrelerinde yapılandırılabilir).
* [Ters proxy](service-fabric-reverseproxy.md) etkinleştirildi.
* [DNS hizmeti](service-fabric-dnsservice.md) etkindir.
* Bronz [dayanıklılık düzeyi](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) (şablon parametrelerinde yapılandırılabilir).
* Gümüş [güvenilirlik düzeyi](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) (şablon parametrelerinde yapılandırılabilir).
* İstemci bağlantı bitiş noktası: 19000 (şablon parametrelerinde yapılandırılabilir).
* HTTP ağ geçidi bitiş noktası: 19080 (şablon parametrelerinde yapılandırılabilir).

### <a name="azure-load-balancer"></a>Azure Load Balancer

**Microsoft.Network/loadBalancers** kaynağında yük dengeleyicisi yapılandırılır. Sondalar ve kurallar aşağıdaki bağlantı noktaları için ayarlanır:

* İstemci bağlantı bitiş noktası: 19000
* HTTP ağ geçidi uç noktası: 19080
* Uygulama portu: 80
* Uygulama portu: 443
* Service Fabric ters proxy’si: 19081

Başka uygulama bağlantı noktaları gerekiyorsa, trafiğin girmesine izin verecek şekilde **Microsoft.Network/loadBalancers** kaynağını ve **Microsoft.Network/networkSecurityGroups** kaynağını ayarlamanız gerekir.

### <a name="virtual-network-subnet-and-network-security-group"></a>Sanal ağ, alt ağ ve ağ güvenlik grubu

Sanal ağ, alt ağ ve ağ güvenlik grubunun adları şablon parametrelerinde bildirilir. Sanal ağın ve alt ağın adres alanları da şablon parametrelerinde bildirilir ve **Microsoft.Network/virtualNetworks** kaynağında yapılandırılır:

* Sanal ağ adres alanı: 172.16.0.0/20
* Service Fabric alt ağ adres alanı: 172.16.2.0/23

**Microsoft.Network/networkSecurityGroups** kaynağında aşağıdaki gelen trafik kuralları etkinleştirilir. Şablon değişkenlerini değiştirerek bağlantı noktası değerlerini değiştirebilirsiniz.

* ClientConnectionEndpoint (TCP): 19000
* HttpGatewayEndpoint (HTTP/TCP): 19080
* Kobİ: 445
* İletişim: 1025, 1026, 1027
* Geçici bağlantı noktası aralığı: 49152 - 65534 (en az 256 bağlantı noktası gerekir).
* Uygulamanın kullanımına yönelik bağlantı noktaları: 80 ve 443
* Uygulama bağlantı noktası aralığı: 49152 - 65534 (servis iletişimi için kullanılır. Yük bakiyesi üzerinde diğer bağlantı noktaları açılmaz).
* Diğer tüm bağlantı noktalarını engelleyin

Başka uygulama bağlantı noktaları gerekiyorsa, trafiğin girmesine izin verecek şekilde **Microsoft.Network/loadBalancers** kaynağını ve **Microsoft.Network/networkSecurityGroups** kaynağını ayarlamanız gerekir.

### <a name="windows-defender"></a>Windows Defender
Varsayılan olarak, [Windows Defender virüsten koruma programı](/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016) Windows Server 2016'da yüklenir ve işlevseldir. Kullanıcı arabirimi varsayılan olarak bazı SNU'larda yüklenir, ancak gerekli değildir. Şablonda bildirilen her düğüm türü/VM ölçeği kümesi [için, Azure VM Kötü Amaçlı Yazılımdan Koruma uzantısı](/azure/virtual-machines/extensions/iaas-antimalware-windows) Hizmet Dokusu dizinlerini ve işlemlerini hariç tutmak için kullanılır:

```json
{
"name": "[concat('VMIaaSAntimalware','_vmNodeType0Name')]",
"properties": {
        "publisher": "Microsoft.Azure.Security",
        "type": "IaaSAntimalware",
        "typeHandlerVersion": "1.5",
        "settings": {
        "AntimalwareEnabled": "true",
        "Exclusions": {
                "Paths": "D:\\SvcFab;D:\\SvcFab\\Log;C:\\Program Files\\Microsoft Service Fabric",
                "Processes": "Fabric.exe;FabricHost.exe;FabricInstallerService.exe;FabricSetup.exe;FabricDeployer.exe;ImageBuilder.exe;FabricGateway.exe;FabricDCA.exe;FabricFAS.exe;FabricUOS.exe;FabricRM.exe;FileStoreService.exe"
        },
        "RealtimeProtectionEnabled": "true",
        "ScheduledScanSettings": {
                "isEnabled": "true",
                "scanType": "Quick",
                "day": "7",
                "time": "120"
        }
        },
        "protectedSettings": null
}
}
```

## <a name="set-template-parameters"></a>Şablon parametrelerini ayarlama

[azuredeploy.parameters.json][parameters] parametre dosyası, kümenin ve ilişkili kaynakların dağıtılması için kullanılan birçok değeri bildirir. Dağıtımınız için değiştirilen parametreler şunlardır:

**Parametre** | **Örnek değer** | **Notlar** 
|---|---|---|
|adminUserName|vmadmin| Küme VM’leri için yönetici kullanıcı adı. [VM için kullanıcı adı gereksinimleri.](https://docs.microsoft.com/azure/virtual-machines/windows/faq#what-are-the-username-requirements-when-creating-a-vm) |
|adminPassword|Password#1234| Küme VM’leri için yönetici parolası. [VM için parola gereksinimleri.](https://docs.microsoft.com/azure/virtual-machines/windows/faq#what-are-the-password-requirements-when-creating-a-vm)|
|clusterName|mysfcluster123| Kümenin adı. Yalnızca harf ve sayı içerebilir. Uzunluğu 3 ile 23 karakter arasında olmalıdır.|
|location|southcentralus| Kümenin konumu. |
|certificateThumbprint|| <p>Otomatik olarak imzalanan bir sertifika oluşturuluyor veya sertifika dosyası sağlanıyorsa değer boş olmalıdır.</p><p>Daha önce bir anahtar kasasına yüklenmiş mevcut bir sertifikayı kullanmak için sertifika SHA1 parmak izi değerini girin. Örneğin: "6190390162C988701DB5676EB81083EA608DCCF3".</p> |
|certificateUrlValue|| <p>Otomatik olarak imzalanan bir sertifika oluşturuluyor veya sertifika dosyası sağlanıyorsa değer boş olmalıdır. </p><p>Daha önce bir anahtar kasasına yüklenmiş mevcut bir sertifikayı kullanmak için sertifika URL’sini girin. Örneğin, "https:\//mykeyvault.vault.azure.net:443/secrets/mycertificate/02bea722c9ef4009a76c5052bcbf8346".</p>|
|sourceVaultValue||<p>Otomatik olarak imzalanan bir sertifika oluşturuluyor veya sertifika dosyası sağlanıyorsa değer boş olmalıdır.</p><p>Daha önce bir anahtar kasasına yüklenmiş mevcut bir sertifikayı kullanmak için kaynak kasa değerini girin. Örneğin: "/subscriptions/333cc2c84-12fa-5778-bd71-c71c07bf873f/resourceGroups/MyTestRG/providers/Microsoft.KeyVault/vaults/MYKEYVAULT".</p>|

## <a name="set-up-azure-active-directory-client-authentication"></a>Azure Etkin Dizin istemci kimlik doğrulaması ayarlama
Azure'da barındırılan ortak bir ağda dağıtılan Hizmet Kumaşı kümeleri için istemciden düğüme karşılıklı kimlik doğrulaması için öneri şudur:
* İstemci kimliği için Azure Etkin Dizini'ni kullanın.
* Sunucu kimliği ve HTTP iletişiminin SSL şifrelemesi için bir sertifika kullanın.

Bir Hizmet Dokusu kümesi için istemcilerin kimliğini doğrulamak için Azure Active Directory 'i (Azure AD) [ayarlamanın kümeoluşturmadan](#createvaultandcert)önce yapılması gerekir. Azure AD, kuruluşların (kiracı olarak da bilinir) uygulamalara kullanıcı erişimini yönetmesini sağlar. 

Bir Service Fabric küme web tabanlı [Hizmet Kumaş Explorer](service-fabric-visualizing-your-cluster.md) ve Visual [Studio](service-fabric-manage-application-in-visual-studio.md)dahil olmak üzere yönetim işlevselliği için çeşitli giriş noktaları sunuyor. Sonuç olarak, kümeye erişimi denetlemek için iki Azure REKLAM uygulaması oluşturursunuz: bir web uygulaması ve bir yerel uygulama.  Uygulamalar oluşturulduktan sonra, kullanıcıları salt okunur ve yönetici rollerine atarsınız.

> [!NOTE]
> Kümeoluşturmadan önce aşağıdaki adımları tamamlamanız gerekir. Komut dosyaları küme adları ve uç noktalar beklediğinden, değerler önceden oluşturduğunuz değerler değil, planlanmalıdır.

Bu makalede, zaten bir kiracı oluşturduğunuzu varsayıyoruz. Yapmadıysanız, [Azure Etkin Dizin kiracısını nasıl alırsınız](../active-directory/develop/quickstart-create-new-tenant.md)okuyarak başlayın.

Azure AD'yi Hizmet Kumaşı kümesiyle yapılandırma adımlarını basitleştirmek için bir dizi Windows PowerShell komut dosyası oluşturduk. [Komut dosyalarını](https://github.com/Azure-Samples/service-fabric-aad-helpers) bilgisayarınıza indirin.

### <a name="create-azure-ad-applications-and-assign-users-to-roles"></a>Azure AD uygulamaları oluşturun ve kullanıcıları rollere atama
Kümeye erişimi denetlemek için iki Azure REKLAM uygulaması oluşturun: bir web uygulaması ve bir yerel uygulama. Kümenizi temsil edecek uygulamaları oluşturduktan sonra, kullanıcılarınızı [Service Fabric tarafından desteklenen rollere](service-fabric-cluster-security-roles.md)atayın: salt okunur ve yönetici.

Çalıştırın `SetupApplications.ps1`ve kiracı kimliğini, küme adını ve web uygulaması yanıt URL'sini parametreler olarak sağlayın. Kullanıcılar için kullanıcı adlarını ve parolaları belirtin. Örnek:

```powershell
$Configobj = .\SetupApplications.ps1 -TenantId '<MyTenantID>' -ClusterName 'mysfcluster123' -WebApplicationReplyUrl 'https://mysfcluster123.eastus.cloudapp.azure.com:19080/Explorer/index.html' -AddResourceAccess
.\SetupUser.ps1 -ConfigObj $Configobj -UserName 'TestUser' -Password 'P@ssword!123'
.\SetupUser.ps1 -ConfigObj $Configobj -UserName 'TestAdmin' -Password 'P@ssword!123' -IsAdmin
```

> [!NOTE]
> Ulusal bulutlar için (örneğin Azure Kamu, Azure Çin, Azure Almanya), parametreyi belirtin. `-Location`

*Kiracı Kimliğinizi*veya dizin kimliğinizi [Azure portalında](https://portal.azure.com)bulabilirsiniz. **Azure Etkin Dizin** > **Özellikleri'ni** seçin ve **Dizin Kimliği** değerini kopyalayın.

*ClusterName,* komut dosyası tarafından oluşturulan Azure AD uygulamalarını önek için kullanılır. Gerçek küme adı yla tam olarak eşleşmesi gerekmez. Azure AD yapılarını kullanımdaki Hizmet Kumaşı kümesiyle eşlemenizi kolaylaştırır.

*WebApplicationReplyUrl,* Azure AD'nin oturum açma yı bitirdikten sonra kullanıcılarınıza döndürdettiği varsayılan bitiş noktasıdır. Bu bitiş noktasını kümeniz için Hizmet Kumaşı Gezgini bitiş noktası olarak ayarlayın, bu da varsayılan olarak:

https://&lt;&gt;cluster_domain :19080/Explorer

Azure AD kiracısı için yönetim ayrıcalıkları olan bir hesapta oturum açmanız istenir. Oturum açtıktan sonra, komut dosyası Hizmet Kumaşı kümenizi temsil edecek web ve yerel uygulamaları oluşturur. [Azure portalındaki](https://portal.azure.com)kiracı uygulamalarında iki yeni giriş görmeniz gerekir:

   * *ClusterName*\_Küme
   * *ClusterName*\_İstemci

Komut dosyası, kümeoluştururken Kaynak Yöneticisi şablonu tarafından gerekli olan JSON'u yazdırır, bu nedenle PowerShell penceresini açık tutmak iyi bir fikirdir.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

### <a name="add-azure-ad-configuration-to-use-azure-ad-for-client-access"></a>İstemci erişimi için Azure AD'yi kullanmak için Azure AD yapılandırması ekleme
[Azuredeploy.json'da][template]Azure AD'yi **Microsoft.ServiceFabric/clusters** bölümünde yapılandırın. Kiracı kimliği, küme uygulama kimliği ve istemci uygulama kimliği için parametreler ekleyin.  

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    ...

    "aadTenantId": {
      "type": "string",
      "defaultValue": "0e3d2646-78b3-4711-b8be-74a381d9890c"
    },
    "aadClusterApplicationId": {
      "type": "string",
      "defaultValue": "cb147d34-b0b9-4e77-81d6-420fef0c4180"
    },
    "aadClientApplicationId": {
      "type": "string",
      "defaultValue": "7a8f3b37-cc40-45cc-9b8f-57b8919ea461"
    }
  },

...

{
  "apiVersion": "2018-02-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

[Azuredeploy.parameters.json][parameters] parametreleri dosyasına parametre değerlerini ekleyin. Örnek:

```json
"aadTenantId": {
"value": "0e3d2646-78b3-4711-b8be-74a381d9890c"
},
"aadClusterApplicationId": {
"value": "cb147d34-b0b9-4e77-81d6-420fef0c4180"
},
"aadClientApplicationId": {
"value": "7a8f3b37-cc40-45cc-9b8f-57b8919ea461"
}
```
<a id="configurediagnostics" name="configurediagnostics_anchor"></a>

## <a name="configure-diagnostics-collection-on-the-cluster"></a>Kümedeki tanılama koleksiyonunu yapılandırma
Bir Hizmet Kumaş kümesi çalıştırırken, tüm düğümlerden günlükleri merkezi bir konumda toplamak iyi bir fikirdir. Günlüklerin merkezi bir konumda olması, kümenizdeki sorunları veya bu kümede çalışan uygulama ve hizmetlerdeki sorunları çözümlemenize ve sorun gidermenize yardımcı olur.

Günlükleri yüklemenin ve toplamanın bir yolu, günlükleri Azure Depolama'ya yükleyen ve günlükleri Azure Uygulama Bilgileri'ne veya Etkinlik Hub'larına gönderme seçeneğine sahip olan Azure Tanılama (WAD) uzantısını kullanmaktır. Ayrıca, olayları depolama alanından okumak ve Azure Monitor günlükleri veya başka bir günlük ayrıştma çözümü gibi bir analiz platformu ürününe yerleştirmek için harici bir işlem de kullanabilirsiniz.

Bu öğreticiyi takip ediyorsanız, tanılama koleksiyonu [şablonda][template]zaten yapılandırıldı.

Tanılama dağıtılmamasa varolan bir kümeniz varsa, küme şablonu aracılığıyla ekleyebilir veya güncelleştirebilirsiniz. Varolan kümeyi oluşturmak veya şablonu portaldan indirmek için kullanılan Kaynak Yöneticisi şablonuna değiştirin. Aşağıdaki görevleri gerçekleştirerek template.json dosyasını değiştirin:

Şablondaki kaynaklar bölümüne yeni bir depolama kaynağı ekleyin:
```json
"resources": [
...
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[parameters('applicationDiagnosticsStorageAccountName')]",
  "location": "[parameters('computeLocation')]",
  "sku": {
    "accountType": "[parameters('applicationDiagnosticsStorageAccountType')]"
  },
  "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
  }
},
...
]
```

Ardından, depolama hesabı adı için parametreler ekleyin ve şablonun parametreler bölümüne yazın. Yer tutucu metin depolama hesabı adını istediğiniz depolama hesabının adı ile değiştirin.

```json
"parameters": {
...
"applicationDiagnosticsStorageAccountType": {
    "type": "string",
    "allowedValues": [
    "Standard_LRS",
    "Standard_GRS"
    ],
    "defaultValue": "Standard_LRS",
    "metadata": {
    "description": "Replication option for the application diagnostics storage account"
    }
},
"applicationDiagnosticsStorageAccountName": {
    "type": "string",
    "defaultValue": "**STORAGE ACCOUNT NAME GOES HERE**",
    "metadata": {
    "description": "Name for the storage account that contains application diagnostics data from the cluster"
    }
},
...
}
```

Ardından, kümedeki her **Microsoft.Compute/virtualMachineScaleSets** kaynağının **VirtualMachineProfile** özelliğinin uzantıları dizisine **IaaSDiagnostics** uzantısını ekleyin.  [Örnek şablonu][template]kullanıyorsanız, üç sanal makine ölçek kümesi vardır (kümedeki her düğüm türü için bir tane).

```json
"apiVersion": "2018-10-01",
"type": "Microsoft.Compute/virtualMachineScaleSets",
"name": "[variables('vmNodeType1Name')]",
"properties": {
    ...
    "virtualMachineProfile": {
        "extensionProfile": {
            "extensions": [
                {
                    "name": "[concat(parameters('vmNodeType0Name'),'_Microsoft.Insights.VMDiagnosticsSettings')]",
                    "properties": {
                        "type": "IaaSDiagnostics",
                        "autoUpgradeMinorVersion": true,
                        "protectedSettings": {
                        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
                        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
                        "storageAccountEndPoint": "https://core.windows.net/"
                        },
                        "publisher": "Microsoft.Azure.Diagnostics",
                        "settings": {
                        "WadCfg": {
                            "DiagnosticMonitorConfiguration": {
                            "overallQuotaInMB": "50000",
                            "EtwProviders": {
                                "EtwEventSourceProviderConfiguration": [
                                {
                                    "provider": "Microsoft-ServiceFabric-Actors",
                                    "scheduledTransferKeywordFilter": "1",
                                    "scheduledTransferPeriod": "PT5M",
                                    "DefaultEvents": {
                                    "eventDestination": "ServiceFabricReliableActorEventTable"
                                    }
                                },
                                {
                                    "provider": "Microsoft-ServiceFabric-Services",
                                    "scheduledTransferPeriod": "PT5M",
                                    "DefaultEvents": {
                                    "eventDestination": "ServiceFabricReliableServiceEventTable"
                                    }
                                }
                                ],
                                "EtwManifestProviderConfiguration": [
                                {
                                    "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                                    "scheduledTransferLogLevelFilter": "Information",
                                    "scheduledTransferKeywordFilter": "4611686018427387904",
                                    "scheduledTransferPeriod": "PT5M",
                                    "DefaultEvents": {
                                    "eventDestination": "ServiceFabricSystemEventTable"
                                    }
                                }
                                ]
                            }
                            }
                        },
                        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
                        },
                        "typeHandlerVersion": "1.5"
                    }
                }
            ...
            ]
        }
    }
}
```
<a id="configureeventstore" name="configureeventstore_anchor"></a>

## <a name="configure-the-eventstore-service"></a>EventStore hizmetini yapılandırma
EventStore hizmeti, Service Fabric'te bir izleme seçeneğidir. EventStore, kümenizin durumunu veya iş yüklerinizi belirli bir zamanda anlamanın bir yolunu sağlar. EventStore, kümedeki olayları koruyan, devlete hizmet veren bir Hizmet Kumaşı hizmetidir. Olay Service Fabric Explorer, REST ve API'ler aracılığıyla ortaya çıkarır. EventStore kümenizdeki herhangi bir varlık üzerinde tanılama verileri almak için doğrudan kümesorgular ve yardımcı olmak için kullanılmalıdır:

* Geliştirme veya sınama sorunları veya izleme ardışık bir denetim hattı kullanıyor olabileceğiniz sorunları tanılama
* Kümenizde gerçekleştirdiğiniz yönetim eylemlerinin doğru şekilde işlendiğini doğrulayın
* Service Fabric'in belirli bir varlıkla nasıl etkileşimde bulunduklarının bir "anlık görüntüsünü" alın



Kümenizdeki EventStore hizmetini etkinleştirmek için **Microsoft.ServiceFabric/clusters** kaynağının **fabricSettings** özelliğine aşağıdakileri ekleyin:

```json
"apiVersion": "2018-02-01",
"type": "Microsoft.ServiceFabric/clusters",
"name": "[parameters('clusterName')]",
"properties": {
    ...
    "fabricSettings": [
        ...
        {
            "name": "EventStoreService",
            "parameters": [
                {
                "name": "TargetReplicaSetSize",
                "value": "3"
                },
                {
                "name": "MinReplicaSetSize",
                "value": "1"
                }
            ]
        }
    ]
}
```
<a id="configureloganalytics" name="configureloganalytics_anchor"></a>

## <a name="set-up-azure-monitor-logs-for-the-cluster"></a>Küme için Azure Monitor günlüklerini ayarlama

Azure Monitor günlükleri küme düzeyindeki olayları izlemek için tavsiyemizdir. Kümenizi izlemek için Azure Monitor günlükleri ayarlamak için, [küme düzeyindeki olayları görüntülemek için tanılamanın etkinleştirilmiş](#configure-diagnostics-collection-on-the-cluster)olması gerekir.  

Çalışma alanının kümenizden gelen tanılama verilerine bağlanması gerekir.  Bu günlük verileri *uygulamaDiagnosticsStorageAccountName* depolama hesabında, WADServiceFabric*EventTable, WADWindowsEventLogsTable ve WADETWEventTable tablolarında saklanır.

Azure Günlük Analizi çalışma alanını ekleyin ve çözümü çalışma alanına ekleyin:

```json
"resources": [
    ...
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[parameters('omsWorkspacename')]",
        "type": "Microsoft.OperationalInsights/workspaces",
        "properties": {
            "sku": {
                "name": "Free"
            }
        },
        "resources": [
            {
                "apiVersion": "2015-11-01-preview",
                "name": "[concat(variables('applicationDiagnosticsStorageAccountName'),parameters('omsWorkspacename'))]",
                "type": "storageinsightconfigs",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]",
                    "[concat('Microsoft.Storage/storageAccounts/', variables('applicationDiagnosticsStorageAccountName'))]"
                ],
                "properties": {
                    "containers": [],
                    "tables": [
                        "WADServiceFabric*EventTable",
                        "WADWindowsEventLogsTable",
                        "WADETWEventTable"
                    ],
                    "storageAccount": {
                        "id": "[resourceId('Microsoft.Storage/storageaccounts/', variables('applicationDiagnosticsStorageAccountName'))]",
                        "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('applicationDiagnosticsStorageAccountName')),'2015-06-15').key1]"
                    }
                }
            },
            {
                "apiVersion": "2015-11-01-preview",
                "type": "datasources",
                "name": "sampleWindowsPerfCounter",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
                ],
                "kind": "WindowsPerformanceCounter",
                "properties": {
                    "objectName": "Memory",
                    "instanceName": "*",
                    "intervalSeconds": 10,
                    "counterName": "Available MBytes"
                }
            },
            {
                "apiVersion": "2015-11-01-preview",
                "type": "datasources",
                "name": "sampleWindowsPerfCounter2",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
                ],
                "kind": "WindowsPerformanceCounter",
                "properties": {
                    "objectName": "Service Fabric Service",
                    "instanceName": "*",
                    "intervalSeconds": 10,
                    "counterName": "Average milliseconds per request"
                }
            }
        ]
    },
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[variables('solution')]",
        "type": "Microsoft.OperationsManagement/solutions",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
        ],
        "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
        },
        "plan": {
            "name": "[variables('solution')]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('solutionName'))]",
            "promotionCode": ""
        }
    }
]
```

Sonra, parametre eklemek
```json
"parameters": {
    ...
    "omsWorkspacename": {
        "type": "string",
        "defaultValue": "mysfomsworkspace",
        "metadata": {
            "description": "Name of your OMS Log Analytics Workspace"
        }
    },
    "omsRegion": {
        "type": "string",
        "defaultValue": "West Europe",
        "allowedValues": [
            "West Europe",
            "East US",
            "Southeast Asia"
        ],
        "metadata": {
            "description": "Specify the Azure Region for your OMS workspace"
        }
    }
}
```

Ardından, değişkenler ekleyin:
```json
"variables": {
    ...
    "solution": "[Concat('ServiceFabric', '(', parameters('omsWorkspacename'), ')')]",
    "solutionName": "ServiceFabric"
}
```

Kümedeki her sanal makine ölçeğine Log Analytics aracı uzantısını ekleyin ve aracıyı Log Analytics çalışma alanına bağlayın. Bu, kapsayıcılar, uygulamalar ve performans izleme hakkında tanılama verilerinin toplanmasını sağlar. Azure Kaynak Yöneticisi, sanal makine ölçeği kümesi kaynağına uzantı olarak ekleyerek, kümeyi ölçeklendirirken bile her düğüme yüklenmesini sağlar.

```json
"apiVersion": "2018-10-01",
"type": "Microsoft.Compute/virtualMachineScaleSets",
"name": "[variables('vmNodeType1Name')]",
"properties": {
    ...
    "virtualMachineProfile": {
        "extensionProfile": {
            "extensions": [
                {
                    "name": "[concat(variables('vmNodeType0Name'),'OMS')]",
                    "properties": {
                        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
                        "type": "MicrosoftMonitoringAgent",
                        "typeHandlerVersion": "1.0",
                        "autoUpgradeMinorVersion": true,
                        "settings": {
                            "workspaceId": "[reference(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename')), '2015-11-01-preview').customerId]"
                        },
                        "protectedSettings": {
                            "workspaceKey": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename')),'2015-11-01-preview').primarySharedKey]"
                        }
                    }
                }
            ...
            ]
        }
    }
}
```

<a id="createvaultandcert" name="createvaultandcert_anchor"></a>

## <a name="deploy-the-virtual-network-and-cluster"></a>Sanal ağı ve kümeyi dağıtma

Ardından, ağ topolojisini ayarlayın ve Service Fabric kümesini dağıtın. [azuredeploy.json][template] Kaynak Yöneticisi şablonu, Hizmet Kumaşı için sanal ağ, alt ağ ve ağ güvenlik grubu oluşturur. Şablon tarafından sertifika güvenliği etkin bir küme de dağıtılır. Üretim kümeleri için, küme sertifikası olarak bir sertifika yetkilisinden bir sertifika kullanın. Test kümelerinin güvenliğinin sağlanması için otomatik olarak imzalanan bir sertifika kullanılabilir.

Bu makaledeki şablon, küme sertifikasını tanımlamak için sertifika parmak izini kullanan bir küme dağır. Hiçbir iki sertifika aynı parmak izine sahip olamaz ve bu da sertifika yönetimini zorlaştırır. Dağıtılan bir kümeyi sertifika parmak baskılarından sertifika ortak adlarına geçiş sertifika yönetimini kolaylaştırır. Sertifika yönetimi için sertifika ortak adlarını kullanmak için kümeyi nasıl güncelleştireceğinizi öğrenmek için, [ortak ad yönetimini sertifikalamak için değiştir kümesini](service-fabric-cluster-change-cert-thumbprint-to-cn.md)okuyun.

### <a name="create-a-cluster-by-using-an-existing-certificate"></a>Varolan bir sertifikayı kullanarak küme oluşturma

Aşağıdaki komut dosyası, Azure'da yeni bir küme dağıtmak için [Yeni AzServiceFabricCluster](/powershell/module/az.servicefabric/New-azServiceFabricCluster) cmdlet ve şablon kullanır. Cmdlet Azure'da yeni bir anahtar kasası oluşturur ve sertifikanızı yükler.

```powershell
# Variables.
$groupname = "sfclustertutorialgroup"
$clusterloc="southcentralus"  # Must match the location parameter in the template
$templatepath="C:\temp\cluster"

$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
$clustername = "mysfcluster123"  # Must match the clustername parameter in the template
$vaultname = "clusterkeyvault123"
$vaultgroupname="clusterkeyvaultgroup123"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# Sign in to your Azure account and select your subscription
Connect-AzAccount
Get-AzSubscription
Set-AzContext -SubscriptionId <guid>

# Create a new resource group for your deployment, and give it a name and a location.
New-AzResourceGroup -Name $groupname -Location $clusterloc

# Create the Service Fabric cluster.
New-AzServiceFabricCluster  -ResourceGroupName $groupname -TemplateFile "$templatepath\azuredeploy.json" `
-ParameterFile "$templatepath\azuredeploy.parameters.json" -CertificatePassword $certpwd `
-KeyVaultName $vaultname -KeyVaultResourceGroupName $vaultgroupname -CertificateFile $certpath
```

### <a name="create-a-cluster-by-using-a-new-self-signed-certificate"></a>Yeni, kendi imzalı bir sertifika kullanarak küme oluşturma

Aşağıdaki komut dosyası, Azure'da yeni bir küme dağıtmak için [Yeni AzServiceFabricCluster](/powershell/module/az.servicefabric/New-azServiceFabricCluster) cmdlet ve şablon kullanır. Cmdlet Azure'da yeni bir anahtar kasası oluşturur, anahtar kasasına kendi imzalı yeni bir sertifika ekler ve sertifika dosyasını yerel olarak karşıdan yükler.

```powershell
# Variables.
$groupname = "sfclustertutorialgroup"
$clusterloc="southcentralus"  # Must match the location parameter in the template
$templatepath="C:\temp\cluster"

$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
$certfolder="c:\mycertificates\"
$clustername = "mysfcluster123"
$vaultname = "clusterkeyvault123"
$vaultgroupname="clusterkeyvaultgroup123"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# Sign in to your Azure account and select your subscription
Connect-AzAccount
Get-AzSubscription
Set-AzContext -SubscriptionId <guid>

# Create a new resource group for your deployment, and give it a name and a location.
New-AzResourceGroup -Name $groupname -Location $clusterloc

# Create the Service Fabric cluster.
New-AzServiceFabricCluster  -ResourceGroupName $groupname -TemplateFile "$templatepath\azuredeploy.json" `
-ParameterFile "$templatepath\azuredeploy.parameters.json" -CertificatePassword $certpwd `
-CertificateOutputFolder $certfolder -KeyVaultName $vaultname -KeyVaultResourceGroupName $vaultgroupname -CertificateSubjectName $subname

```

## <a name="connect-to-the-secure-cluster"></a>Güvenli kümeye bağlanma

Service Fabric SDK ile yüklü Service Fabric PowerShell modülünü kullanarak kümeye bağlanın.  İlk olarak sertifikayı bilgisayarınızda geçerli kullanıcının Kişisel (My) deposuna yükleyin. Aşağıdaki PowerShell komutunu çalıştırın:

```powershell
$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\mysfcluster20170531104310.pfx `
        -Password $certpwd
```

Artık güvenli kümenize bağlanmaya hazırsınız.

**Service Fabric** PowerShell modülü, Service Fabric kümelerini, uygulamalarını ve hizmetlerini yönetmek için birçok cmdlet sağlar. Güvenli kümeye bağlanmak için [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) cmdlet'ini kullanın. Sertifika SHA1 parmak izi ve bağlantı uç noktası ayrıntıları önceki adımda elde edilen çıktıdan bulunabilir.

Azure AD istemci kimlik doğrulamasını daha önce ayarladıysanız, aşağıdaki komutu çalıştırın: 
```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster123.southcentralus.cloudapp.azure.com:19000 `
        -KeepAliveIntervalInSec 10 `
        -AzureActiveDirectory `
        -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10
```

Azure AD istemci kimlik doğrulaması ayarlamadıysanız aşağıdaki komutu çalıştırın:
```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster123.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

[Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) cmdlet'i kullanarak bağlı olup olmadığınızı ve kümenin sağlıklı olup olmadığını kontrol edin.

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğretici serideki diğer makaleler oluşturduğunuz kümeyi kullanır. Hemen bir sonraki makaleye geçmiyorsanız, ücret ödememek için [kümeyi silmek](service-fabric-cluster-delete.md) isteyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Kümenizi nasıl ölçeklendireceklerini öğrenmek için aşağıdaki öğreticiye ilerleyin.

> [!div class="checklist"]
> * PowerShell kullanarak Azure’da VNet oluşturma
> * Anahtar kasası oluşturma ve karşıya sertifika yükleme
> * Azure Active Directory kimlik doğrulaması kurulumu
> * Tanılama koleksiyonunu yapılandırma
> * EventStore hizmetini ayarlama
> * Azure Monitör günlüklerini ayarlama
> * Azure PowerShell’de güvenli bir Service Fabric kümesi oluşturma
> * X.509 sertifikasıyla kümenin güvenliğini sağlama
> * PowerShell kullanarak kümeye bağlanma
> * Bir kümeyi kaldırma

Ardından, kümenizi nasıl izleyeceğinizi öğrenmek için aşağıdaki öğreticiye ilerleyin.
> [!div class="nextstepaction"]
> [Bir Kümeyi İzle](service-fabric-tutorial-monitor-cluster.md)

[template]:https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/7-VM-Windows-3-NodeTypes-Secure-NSG/AzureDeploy.json
[parameters]:https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/7-VM-Windows-3-NodeTypes-Secure-NSG/AzureDeploy.Parameters.json
