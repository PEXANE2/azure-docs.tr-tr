---
title: Azure Service Fabric güvenliği en iyi yöntemleri
description: Azure Hizmet Kumaş kümelerini ve uygulamalarını güvende tutmak için en iyi uygulamalar ve tasarım konuları.
author: peterpogorski
ms.topic: conceptual
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: fa8bb41684271c7d4ebe90e31ce8019994fc1f41
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478747"
---
# <a name="azure-service-fabric-security"></a>Azure Service Fabric güvenliği 

Azure Güvenlik [En İyi Uygulamaları](https://docs.microsoft.com/azure/security/)hakkında daha fazla bilgi için [Azure Hizmet Kumaşı güvenlik en iyi uygulamalarını](https://docs.microsoft.com/azure/security/fundamentals/service-fabric-best-practices) inceleyin

## <a name="key-vault"></a>Key Vault

[Azure Key Vault,](https://docs.microsoft.com/azure/key-vault/) Azure Hizmet Kumaşı uygulamaları ve kümeleri için önerilen sırlar yönetimi hizmetidir.
> [!NOTE]
> Bir Anahtar Kasası'ndaki sertifikalar/sırlar Sanal Makine Ölçeği Kümesi Olarak Sanal Makine Ölçeği Kümesi'ne dağıtılırsa, Anahtar Kasası ve Sanal Makine Ölçeği Kümesi birlikte bulunmalıdır.

## <a name="create-certificate-authority-issued-service-fabric-certificate"></a>Sertifika yetkilisi verilen Hizmet Kumaşı sertifikası oluşturma

Azure Anahtar Kasası sertifikası oluşturulabilir veya Bir Anahtar Kasası'na aktarılabilir. Bir Key Vault sertifikası oluşturulduğunda, özel anahtar Anahtar Kasası içinde oluşturulur ve sertifika sahibine asla maruz kalmaz. Anahtar Kasa'da sertifika oluşturmanın yolları şunlardır:

- Ortak-özel anahtar çifti oluşturmak ve sertifikayla ilişkilendirmek için kendi imzalı bir sertifika oluşturun. Sertifika kendi anahtarı yla imzalanır. 
- Ortak-özel anahtar çifti oluşturmak ve X.509 sertifika imzalama isteği oluşturmak için el ile yeni bir sertifika oluşturun. İmza isteği kayıt yetkilisiniz veya sertifika yetkilisi niz tarafından imzalanabilir. İmzalı x509 sertifikası, Anahtar Kasa'daki KV sertifikasını tamamlamak için bekleyen anahtar çifti ile birleştirilebilir. Bu yöntem daha fazla adım gerektirmesine rağmen, özel anahtar Anahtar Kasası'nda oluşturulduğundan ve anahtar kasasıyla sınırlı olduğundan size daha fazla güvenlik sağlar. Bu aşağıdaki diyagramda açıklanmıştır. 

Ek ayrıntılar için [Azure Keyvault Sertifikası Oluşturma Yöntemlerini](https://docs.microsoft.com/azure/key-vault/create-certificate) gözden geçirin.

## <a name="deploy-key-vault-certificates-to-service-fabric-cluster-virtual-machine-scale-sets"></a>Anahtar Kasa sertifikalarını Service Fabric küme sanal makine ölçek kümelerine dağıtın

Ortak bulunan bir anahtar kasadan Sanal Makine Ölçeği Setine sertifika dağıtmak için Sanal Makine Ölçeği Kümesi [osProfile'ı](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile)kullanın. Kaynak Yöneticisi şablon özellikleri şunlardır:

```json
"secrets": [
   {
       "sourceVault": {
           "id": "[parameters('sourceVaultValue')]"
       },
       "vaultCertificates": [
          {
              "certificateStore": "[parameters('certificateStoreValue')]",
              "certificateUrl": "[parameters('certificateUrlValue')]"
          }
       ]
   }
]
```

> [!NOTE]
> Kaynak Yöneticisi şablon dağıtımı için kasa etkinleştirilmelidir.

## <a name="apply-an-access-control-list-acl-to-your-certificate-for-your-service-fabric-cluster"></a>Hizmet Kumaşı kümeniz için sertifikanıza bir Erişim Denetim Listesi (ACL) uygulayın

[Virtual Machine Scale Set uzantıları](https://docs.microsoft.com/cli/azure/vmss/extension?view=azure-cli-latest) yayıncısı Microsoft.Azure.ServiceFabric Düğümlerinizi Yapılandırmak için kullanılır.
Hizmet Kumaş Kümesi işlemleriniz için sertifikalarınıza Bir ACL uygulamak için aşağıdaki Kaynak Yöneticisi şablon özelliklerini kullanın:

```json
"certificate": {
   "commonNames": [
       "[parameters('certificateCommonName')]"
   ],
   "x509StoreName": "[parameters('certificateStoreValue')]"
}
```

## <a name="secure-a-service-fabric-cluster-certificate-by-common-name"></a>Hizmet Kumaşı küme sertifikasını ortak ada göre güvenli hale

Hizmet Kumaşı kümenizi `Common Name`sertifikaya göre sabitlemek için Kaynak Yöneticisi şablonu özellik [sertifikasıCommonNames'i](https://docs.microsoft.com/rest/api/servicefabric/sfrp-model-clusterproperties#certificatecommonnames)aşağıdaki gibi kullanın:

```json
"certificateCommonNames": {
    "commonNames": [
        {
            "certificateCommonName": "[parameters('certificateCommonName')]",
            "certificateIssuerThumbprint": "[parameters('certificateIssuerThumbprint')]"
        }
    ],
    "x509StoreName": "[parameters('certificateStoreValue')]"
}
```

> [!NOTE]
> Service Fabric kümeleri, ev sahibinizin sertifika deposunda bulduğu ilk geçerli sertifikayı kullanır. Windows'da bu, Ortak Adınız ve Veren parmak izinizle eşleşen en son son bitiş tarihine sahip sertifikadır.

*\<SUBDOMAIN .cloudapp.azure.com veya\> \<SUBDOMAIN\>.trafficmanager.net gibi azure etki alanları Microsoft'a aittir. Sertifika Yetkilileri, alan adları için yetkisiz kullanıcılara sertifika vermez. Bir sertifika yetkilisinin size bu ortak ada sahip bir sertifika verebilmek için çoğu kullanıcının bir kayıt şirketinden bir etki alanı satın almalı veya yetkili bir etki alanı yöneticisi olması gerekir.

Etki alanınızı bir Microsoft IP adresine çözümlemek için DNS Hizmetini yapılandırma hakkında ek ayrıntılar için, [etki alanınızı barındıracak şekilde Azure DNS'yi](https://docs.microsoft.com/azure/dns/dns-delegate-domain-azure-dns)nasıl yapılandırıştırıyorum.

> [!NOTE]
> Etki alanad ad sunucularınızı Azure DNS bölge ad sunucularınıza devretdikten sonra, Aşağıdaki iki kaydı DNS Bölgenize ekleyin:
> - Tüm IP Adresleri için özel etki alanı `Alias record set` çözecek değİldir etki alanı APEX için bir 'A' kaydı.
> - Microsoft alt etki alanları için bir `Alias record set`'C' kaydı değİldir . Örneğin, Trafik Yöneticisi'nizi veya Yük Bakiyesi'nin DNS adını kullanabilirsiniz.

Hizmet Kumaş Kümeniz `"managementEndpoint"`için özel bir DNS adını görüntülemek için portalınızı güncelleştirmek için, aşağıdaki Hizmet Kumaş Kümesi Kaynak Yöneticisi şablon özelliklerini güncelleştirin:

```json
 "managementEndpoint": "[concat('https://<YOUR CUSTOM DOMAIN>:',parameters('nt0fabricHttpGatewayPort'))]",
```

## <a name="encrypting-service-fabric-package-secret-values"></a>Şifreleme Servisi Kumaş paketi gizli değerleri

Hizmet Kumaş Paketleri'nde şifrelenen yaygın değerler arasında Azure Kapsayıcı Kayıt Defteri (ACR) kimlik bilgileri, ortam değişkenleri, ayarlar ve Azure Birim eklentidepolama hesap anahtarları yer alır.

[Bir şifreleme sertifikası ayarlamak ve Windows kümelerinde sırları şifrelemek için:](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management-windows)

Sırrınızı şifrelemek için kendi imzalı bir sertifika oluşturun:

```powershell
New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject mydataenciphermentcert -Provider 'Microsoft Enhanced Cryptographic Provider v1.0'
```

Anahtar Kasa [sertifikalarını Servis Kumaşı küme sanal makine ölçek kümelerine dağıt'taki](#deploy-key-vault-certificates-to-service-fabric-cluster-virtual-machine-scale-sets) yönergeleri kullanarak Anahtar Kasa Sertifikalarını Servis Kumaş Kümenizin Sanal Makine Ölçek Kümelerine dağıtın.

Aşağıdaki PowerShell komutunu kullanarak sırrınızı şifreleyin ve ardından Hizmet Kumaşı uygulama bildiriminizi şifreli değerle güncelleyin:

``` powershell
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint "<thumbprint>" -Text "mysecret" -StoreLocation CurrentUser -StoreName My
```

[Bir şifreleme sertifikası oluşturmak ve Linux kümelerinde sırları şifrelemek için:](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management-linux)

Sırlarınızı şifrelemek için kendi imzalı bir sertifika oluşturun:

```bash
user@linux:~$ openssl req -newkey rsa:2048 -nodes -keyout TestCert.prv -x509 -days 365 -out TestCert.pem
user@linux:~$ cat TestCert.prv >> TestCert.pem
```

Servis Kumaş Kümenizin Sanal Makine Ölçek [Kümelerine Hizmet Kumaş küme sanal makine ölçek kümeleri](#deploy-key-vault-certificates-to-service-fabric-cluster-virtual-machine-scale-sets) için Anahtar Vault sertifikaları dağıtın yönergeleri kullanın.

Aşağıdaki komutları kullanarak sırrınızı şifreleyin ve ardından Hizmet Kumaşı Uygulama Bildiriminizi şifrelenmiş değerle güncelleyin:

```bash
user@linux:$ echo "Hello World!" > plaintext.txt
user@linux:$ iconv -f ASCII -t UTF-16LE plaintext.txt -o plaintext_UTF-16.txt
user@linux:$ openssl smime -encrypt -in plaintext_UTF-16.txt -binary -outform der TestCert.pem | base64 > encrypted.txt
```

Korumalı değerlerinizi şifreledikten [sonra, Hizmet Kumaş Uygulaması'nda şifrelenmiş sırları belirtin](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management#specify-encrypted-secrets-in-an-application)ve [servis kodundan şifrelenmiş sırları niçin çözebilirsiniz.](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management#decrypt-encrypted-secrets-from-service-code)

## <a name="include-certificate-in-service-fabric-applications"></a>Hizmet Kumaş uygulamalarına sertifika ekleme

Uygulamanızın sırlara erişimini sağlamak için, uygulama bildirimine Bir **SecretsCertificate** öğesi ekleyerek sertifikayı ekleyin.

```xml
<ApplicationManifest … >
  ...
  <Certificates>
    <SecretsCertificate Name="MyCert" X509FindType="FindByThumbprint" X509FindValue="[YourCertThumbrint]"/>
  </Certificates>
</ApplicationManifest>
```
## <a name="authenticate-service-fabric-applications-to-azure-resources-using-managed-service-identity-msi"></a>Yönetilen Hizmet Kimliği (MSI) kullanarak Hizmet Kumaşı uygulamalarını Azure Kaynaklarına doğrulama

Azure kaynakları için yönetilen kimlikler hakkında bilgi edinmek için Azure [kaynakları için yönetilen kimlikler nedir?](../active-directory/managed-identities-azure-resources/overview.md#how-does-the-managed-identities-for-azure-resources-work)
Azure Hizmet Kumaş ı kümeleri, Yönetilen Hizmet Kimliğini destekleyen Sanal Makine Ölçeği Kümeleri'nde [barındırılır.](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-msi#azure-services-that-support-managed-identities-for-azure-resources)
MSI'ın kimlik doğrulaması için kullanılabilecek hizmetlerin listesini almak için Azure [Etkin Dizin Kimlik Doğrulaması'nı destekleyen Azure Hizmetleri'ne](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-msi#azure-services-that-support-azure-ad-authentication)bakın.


Sanal makine ölçeği kümesi veya varolan sanal makine ölçeği kümesi nin oluşturulması sırasında yönetilen `"Microsoft.Compute/virtualMachinesScaleSets"` kimlik atanan sistemi etkinleştirmek için aşağıdaki özelliği bildirin:

```json
"identity": { 
    "type": "SystemAssigned"
}
```
Azure [kaynakları için yönetilen kimlikler nedir?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-template-windows-vmss#system-assigned-managed-identity)

Kullanıcı tarafından [atanan yönetilen](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-arm#create-a-user-assigned-managed-identity)bir kimlik oluşturduysanız, şablonunuzda ki aşağıdaki kaynağı sanal makine ölçek kümenize atamak için bildirin. Oluşturduğunuz kullanıcı tarafından atanan yönetilen kimliğin adıyla değiştirin: `\<USERASSIGNEDIDENTITYNAME\>`

```json
"identity": {
    "type": "userAssigned",
    "userAssignedIdentities": {
        "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]": {}
    }
}
```

Service Fabric uygulamanız yönetilen bir kimlikten yararlanabilmesi için, kimlik doğrulaması gereken Azure Kaynaklarına izin verilmesi gerekir.
Aşağıdaki komutlar bir Azure Kaynağına erişim verir:

```bash
principalid=$(az resource show --id /subscriptions/<YOUR SUBSCRIPTON>/resourceGroups/<YOUR RG>/providers/Microsoft.Compute/virtualMachineScaleSets/<YOUR SCALE SET> --api-version 2018-06-01 | python -c "import sys, json; print(json.load(sys.stdin)['identity']['principalId'])")

az role assignment create --assignee $principalid --role 'Contributor' --scope "/subscriptions/<YOUR SUBSCRIPTION>/resourceGroups/<YOUR RG>/providers/<PROVIDER NAME>/<RESOURCE TYPE>/<RESOURCE NAME>"
```

Service Fabric uygulama kodunuzda, aşağıdakilere benzer bir REST yaparak Azure Kaynak Yöneticisi için [bir erişim jetonu edinin:](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/how-to-use-vm-token#get-a-token-using-http)

```bash
access_token=$(curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -H Metadata:true | python -c "import sys, json; print json.load(sys.stdin)['access_token']")

```

Service Fabric uygulamanız daha sonra Etkin Dizin'i destekleyen Azure Kaynaklarına kimlik doğrulamak için erişim belirteci'ni kullanabilir.
Aşağıdaki örnek, Cosmos DB kaynağı için bunun nasıl yapılacağını gösterir:

```bash
cosmos_db_password=$(curl 'https://management.azure.com/subscriptions/<YOUR SUBSCRIPTION>/resourceGroups/<YOUR RG>/providers/Microsoft.DocumentDB/databaseAccounts/<YOUR ACCOUNT>/listKeys?api-version=2016-03-31' -X POST -d "" -H "Authorization: Bearer $access_token" | python -c "import sys, json; print(json.load(sys.stdin)['primaryMasterKey'])")
```
## <a name="windows-security-baselines"></a>Windows güvenlik taban çizgileri
Bir [taban çizgisi oluşturmanın aksine, Microsoft güvenlik taban çizgileri gibi genel olarak bilinen ve iyi sınanan endüstri standardı bir yapılandırma uygulamanızı öneririz;](https://docs.microsoft.com/windows/security/threat-protection/windows-security-baselines) Bunları Sanal Makine Ölçeği Kümelerinizde sağlama seçeneği, Azure İstenilen Durum Yapılandırması (DSC) uzantısını kullanarak, VM'leri çevrimiçi olarak yapılandırarak üretim yazılımını çalıştırıyorlar.

## <a name="azure-firewall"></a>Azure Güvenlik Duvarı
[Azure Güvenlik Duvarı, Azure Sanal Ağ kaynaklarınızı koruyan yönetilen, bulut tabanlı bir ağ güvenlik hizmetidir. Yerleşik yüksek kullanılabilirliğe ve sınırsız bulut ölçeklenebilirliğine sahip bir hizmet olarak tamamen görkemli bir güvenlik duvarıdır.](https://docs.microsoft.com/azure/firewall/overview) bu, giden HTTP/S trafiğini joker kartlar da dahil olmak üzere tam nitelikli alan adlarının (FQDN) belirli bir listesiyle sınırlandırma olanağı sağlar. Bu özellik TLS/SSL sonlandırma gerektirmez. Windows Güncelleştirmeleri için [Azure Güvenlik Duvarı FQDN etiketlerinden](https://docs.microsoft.com/azure/firewall/fqdn-tags) yararlanmanızı ve Microsoft Windows Update uç noktalarına ağ trafiğini etkinleştirmenizi önerilir. [Şablon kullanarak Azure Güvenlik Duvarı'nı dağıtma](https://docs.microsoft.com/azure/firewall/deploy-template) Microsoft.Network/azureFirewalls kaynak şablonu tanımı için bir örnek sağlar. Service Fabric Applications için ortak olan güvenlik duvarı kuralları, kümelerinizin sanal ağı için aşağıdakilere izin vermektir:

- *download.microsoft.com
- *servicefabric.azure.com
- *.core.windows.net

Bu güvenlik duvarı kuralları, sanal ağınızdan izin verilen hedefler olarak ServiceFabric ve Storage'ı da içeren izin verilen giden Ağ Güvenlik Gruplarınızı tamamlar.

## <a name="tls-12"></a>TLS 1.2
[Tsg](https://github.com/Azure/Service-Fabric-Troubleshooting-Guides/blob/master/Security/TLS%20Configuration.md)

## <a name="windows-defender"></a>Windows Defender 

Varsayılan olarak, Windows Defender virüsten koruma yazılımı Windows Server 2016'da yüklenir. Ayrıntılar için [Windows Server 2016'da Windows Defender Antivirus'e](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016)bakın. Kullanıcı arabirimi varsayılan olarak bazı SNU'larda yüklenir, ancak gerekli değildir. Windows Defender tarafından ortaya çıkan performans etkisini ve kaynak tüketimini azaltmak ve güvenlik ilkeleriniz açık kaynak yazılım için işlemleri ve yolları hariç tutmanıza izin veriyorsa, Hizmet Kumaşı kümenizi taramalardan hariç tutmak için aşağıdaki Sanal Makine Ölçeği Kümesi Uzantı Lı Kaynak Yöneticisi şablon özelliklerini bildirin:


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
                "Paths": "[concat(parameters('svcFabData'), ';', parameters('svcFabLogs'), ';', parameters('svcFabRuntime'))]",
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

> [!NOTE]
> Windows Defender kullanmıyorsanız yapılandırma kuralları için Kötü Amaçlı Yazılımdan Koruma belgelerinize bakın. Windows Defender Linux'ta desteklenmez.

## <a name="platform-isolation"></a>Platform Yalıtımı
Varsayılan olarak, Service Fabric uygulamalarına farklı formlarda kendini gösteren Hizmet Kumaşı çalışma süresine erişim hakkı verilir: uygulama ve Kumaş dosyalarına karşılık gelen ana bilgisayardaki dosya yollarını gösteren [ortam değişkenleri,](service-fabric-environment-variables-reference.md) uygulamaya özgü istekleri kabul eden bir süreçler arası iletişim bitiş noktası ve Fabric'in kendisini doğrulamak için uygulamayı kullanmasını beklediği istemci sertifikası. Hizmetin kendisine güvenilmeyen kodu barındırması durumunda, açıkça gerekmedikçe SF çalışma süresine bu erişimi devre dışı bilebilir. Çalışma süresine erişim, uygulama bildiriminin İlkeler bölümündeki aşağıdaki bildirim kullanılarak kaldırılır: 

```xml
<ServiceManifestImport>
    <Policies>
        <ServiceFabricRuntimeAccessPolicy RemoveServiceFabricRuntimeAccess="true"/>
    </Policies>
</ServiceManifestImport>

```

## <a name="next-steps"></a>Sonraki adımlar

* Windows Server: Windows Server için Hizmet Dokusu küme oluşturma yı çalıştıran VM'lerde veya bilgisayarlarda bir [küme oluşturun.](service-fabric-cluster-creation-for-windows-server.md)
* Linux çalıştıran VM'lerde veya bilgisayarlarda bir küme oluşturun: [Linux kümesi oluşturun.](service-fabric-cluster-creation-via-portal.md)
* Service [Fabric destek seçenekleri](service-fabric-support.md)hakkında bilgi edinin.

[Image1]: ./media/service-fabric-best-practices/generate-common-name-cert-portal.png
