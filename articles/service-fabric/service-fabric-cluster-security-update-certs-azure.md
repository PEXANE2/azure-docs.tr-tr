---
title: Azure Hizmet Kumaşı kümesinde sertifikaları yönetme
description: Hizmet Kumaşı kümesine yeni sertifikalar, rollover sertifikası ve sertifikayı nasıl kaldırışla tanımlayın.
ms.topic: conceptual
ms.date: 11/13/2018
ms.openlocfilehash: a3c92e1b39261af32085e4d9b6cb2462d5c0eb64
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75458358"
---
# <a name="add-or-remove-certificates-for-a-service-fabric-cluster-in-azure"></a>Azure'da Service Fabric kümesine sertifika ekleme veya kaldırma
Service Fabric'in X.509 sertifikalarını nasıl kullandığını ve Küme güvenlik [senaryolarını](service-fabric-cluster-security.md)tanımanız önerilir. Daha fazla ilerlemeden önce küme sertifikasının ne olduğunu ve ne için kullanıldığını anlamanız gerekir.

Azure Hizmet Kumaşları SDK'nın varsayılan sertifika yük davranışı, tanımlı sertifikayı, son kullanma tarihi en uzak olan şekilde dağıtmak ve kullanmaktır; birincil veya ikincil yapılandırma tanımlarından bağımsız olarak. Klasik davranışa geri dönmek, önerilmeyen gelişmiş bir eylemdir ve yapılandırmanızda "UseSecondaryIfNewer" ayar parametre değerini yapılandırmanızda false olarak ayarlamayı `Fabric.Code` gerektirir.

Hizmet dokusu, istemci sertifikalarına ek olarak küme oluşturma sırasında sertifika güvenliğini yapılandırdığınızda birincil ve ikincil olmak üzere iki küme sertifikası belirtmenize olanak tanır. Portal [üzerinden azure küme oluşturmaya](service-fabric-cluster-creation-via-portal.md) veya azure [kaynak yöneticisi aracılığıyla azure küme oluşturmaya](service-fabric-cluster-creation-via-arm.md) bakın. Oluşturma zamanında yalnızca bir küme sertifikası belirtirseniz, bu birincil sertifika olarak kullanılır. Küme oluşturmadan sonra, ikincil olarak yeni bir sertifika ekleyebilirsiniz.

> [!NOTE]
> Güvenli bir küme için her zaman en az bir geçerli (iptal edilmedi ve süresi dolmamış) küme sertifikasının (birincil veya ikincil) dağıtılması gerekir (değilse küme çalışmayı durdurur). Tüm geçerli sertifikaların sona erme tarihine gelmeden 90 gün önce, sistem bir uyarı izleme ve düğüm üzerinde bir uyarı durumu olayı oluşturur. Şu anda Hizmet Kumaşı'nın bu makalede gönderdiği e-posta veya başka bir bildirim bulunmamaktadır. 
> 
> 


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="add-a-secondary-cluster-certificate-using-the-portal"></a>Portalı kullanarak ikincil küme sertifikası ekleme
İkincil küme sertifikası Azure portalı üzerinden eklenemez, Azure powershell'i kullanın. İşlem daha sonra bu belgede özetlenmiştir.

## <a name="remove-a-cluster-certificate-using-the-portal"></a>Portalı kullanarak küme sertifikasıkaldırma
Güvenli bir küme için her zaman en az bir geçerli (iptal edilmedi ve süresi dolmamış) sertifikagerekir. Gelecekteki son kullanma tarihine en uzak olan sertifika kullanımda olacak ve sertifikanın kaldırılması kümenizin çalışmasını durduracaktır; yalnızca süresi dolan sertifikayı veya en kısa süresi dolan kullanılmayan sertifikayı kaldırmayı sağlayın.

Kullanılmayan küme güvenlik sertifikasını kaldırmak için Güvenlik bölümüne gidin ve kullanılmayan sertifikadaki bağlam menüsünden 'Sil' seçeneğini belirleyin.

Amacınız birincil olarak işaretlenmiş sertifikayı kaldırmaksa, otomatik devir davranışını etkinleştirmek için birincil sertifikadan daha ileride sona erme tarihi olan ikincil bir sertifika dağıtmanız gerekir; otomatik rollover tamamlandıktan sonra birincil sertifikayı silin.

## <a name="add-a-secondary-certificate-using-resource-manager-powershell"></a>Kaynak Yöneticisi Powershell'i kullanarak ikincil sertifika ekleme
> [!TIP]
> [Add-AzServiceFabricClusterCertificate](/powershell/module/az.servicefabric/add-azservicefabricclustercertificate) cmdlet'i kullanarak ikincil sertifika eklemenin artık daha iyi ve kolay bir yolu vardır. Bu bölümdeki diğer adımları izlemeniz gerekmez.  Ayrıca, [Add-AzServiceFabricClusterCertificate](/powershell/module/az.servicefabric/add-azservicefabricclustercertificate) cmdlet kullanırken kümeoluşturmak ve dağıtmak için başlangıçta kullanılan şablongerekmez.

Bu adımlar, Kaynak Yöneticisi'nin nasıl çalıştığını bildiğinizi ve Kaynak Yöneticisi şablonu kullanarak en az bir Hizmet Dokusu kümesini dağıtdığınızı ve kümeyi kullanışlı olarak ayarlamak için kullandığınız şablona sahip olduğunuzu varsayar. Ayrıca JSON kullanarak rahat olduğu varsayılır.

> [!NOTE]
> Bir örnek şablon ve parametreler ilerler veya bir başlangıç noktası olarak takip etmek için kullanabilirsiniz arıyorsanız, o zaman bu [git-repo](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/Cert-Rollover-Sample)indirin. 
> 
> 

### <a name="edit-your-resource-manager-template"></a>Kaynak Yöneticisi şablonunuzu edin

Birlikte takip kolaylığı için, örnek 5-VM-1-NodeTypes-Secure_Step2.JSON biz yapıyor olacak tüm edinimleri içerir. örnek [git-repo](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/Cert-Rollover-Sample)mevcuttur.

**Tüm adımları izlediğinden emin olun**

1. Cluster'ı dağıtmak için kullandığınız Kaynak Yöneticisi şablonuna açın. (Örneği önceki repodan indirdiyseniz, güvenli bir küme dağıtmak ve ardından bu şablonu açmak için 5-VM-1-NodeTypes-Secure_Step1.JSON kullanın).

2. Şablonunuzun parametre bölümüne "secCertificateThumbprint" ve "secCertificateUrlValue" türünden **"string" iki yeni parametre** ekleyin. Aşağıdaki kod parçacıklarını kopyalayabilir ve şablona ekleyebilirsiniz. Şablonunuzun kaynağına bağlı olarak, bir sonraki adıma geçiyorsanız, bunları zaten tanımlanmış olabilirsiniz. 
 
    ```json
       "secCertificateThumbprint": {
          "type": "string",
          "metadata": {
            "description": "Certificate Thumbprint"
          }
        },
        "secCertificateUrlValue": {
          "type": "string",
          "metadata": {
            "description": "Refers to the location URL in your key vault where the certificate was uploaded, it is should be in the format of https://<name of the vault>.vault.azure.net:443/secrets/<exact location>"
          }
        },
    
    ```

3. **Microsoft.ServiceFabric/clusters** kaynağında değişiklik yapın - Şablonunuzda "Microsoft.ServiceFabric/clusters" kaynak tanımını bulun. Bu tanımın özellikleri altında, aşağıdaki JSON snippet gibi bir şey görünmelidir "Sertifika" JSON etiketi bulacaksınız:
   
    ```JSON
          "properties": {
            "certificate": {
              "thumbprint": "[parameters('certificateThumbprint')]",
              "x509StoreName": "[parameters('certificateStoreValue')]"
         }
    ``` 

    Yeni bir etiket "thumbprintSecondary" ekleyin ve bir değer "[parameters('secCertificateThumbprint')]" verin.  

    Yani şimdi kaynak tanımı aşağıdaki gibi görünmelidir (şablonun kaynağına bağlı olarak, tam olarak aşağıdaki snippet gibi olmayabilir). 

    ```JSON
          "properties": {
            "certificate": {
              "thumbprint": "[parameters('certificateThumbprint')]",
              "thumbprintSecondary": "[parameters('secCertificateThumbprint')]",
              "x509StoreName": "[parameters('certificateStoreValue')]"
         }
    ``` 

    **Sertifikanın üzerinde yuvarlanmak**istiyorsanız, yeni sertifikayı birincil olarak belirtin ve geçerli birincil birincili ikincil olarak hareket ettirin. Bu, geçerli birincil sertifikanızın tek bir dağıtım adımında yeni sertifikaya devriyle sonuçlanır.
    
    ```JSON
          "properties": {
            "certificate": {
              "thumbprint": "[parameters('secCertificateThumbprint')]",
              "thumbprintSecondary": "[parameters('certificateThumbprint')]",
              "x509StoreName": "[parameters('certificateStoreValue')]"
         }
    ``` 

4. **Tüm** **Microsoft.Compute/virtualMachineScaleSets** kaynak tanımlarında değişiklik yapın - Microsoft.Compute/virtualMachineScaleSets kaynak tanımını bulun. "virtualMachineProfile" altında "publisher": "Microsoft.Azure.ServiceFabric"e gidin.

    Service Fabric yayımcı ayarlarında böyle bir şey görmeniz gerekir.
    
    ![Json_Pub_Setting1][Json_Pub_Setting1]
    
    Yeni cert girişlerini ekleyin
    
    ```json
                   "certificateSecondary": {
                        "thumbprint": "[parameters('secCertificateThumbprint')]",
                        "x509StoreName": "[parameters('certificateStoreValue')]"
                        }
                      },
    
    ```

    Özellikleri şimdi bu gibi görünmelidir
    
    ![Json_Pub_Setting2][Json_Pub_Setting2]
    
    **Sertifikanın üzerinde yuvarlanmak**istiyorsanız, yeni sertifikayı birincil olarak belirtin ve geçerli birincil birincili ikincil olarak hareket ettirin. Bu, geçerli sertifikanızın tek bir dağıtım adımında yeni sertifikaya devriyle sonuçlanır.     

    ```json
                   "certificate": {
                       "thumbprint": "[parameters('secCertificateThumbprint')]",
                       "x509StoreName": "[parameters('certificateStoreValue')]"
                         },
                   "certificateSecondary": {
                        "thumbprint": "[parameters('certificateThumbprint')]",
                        "x509StoreName": "[parameters('certificateStoreValue')]"
                        }
                      },
    ```

    Özellikleri şimdi bu gibi görünmelidir    
    ![Json_Pub_Setting3][Json_Pub_Setting3]

5. **Tüm** **Microsoft.Compute/virtualMachineScaleSets** kaynak tanımlarında değişiklik yapın - Microsoft.Compute/virtualMachineScaleSets kaynak tanımını bulun. "vaultCertificates"a gidin: "OSProfile" altında. Buna benzer bir şey olmalı.

    ![Json_Pub_Setting4][Json_Pub_Setting4]
    
    Buna secCertificateUrlValue ekleyin. aşağıdaki snippet kullanın:
    
    ```json
                      {
                        "certificateStore": "[parameters('certificateStoreValue')]",
                        "certificateUrl": "[parameters('secCertificateUrlValue')]"
                      }
    
    ```
    Şimdi ortaya çıkan Json buna benzer bir şey olmalı.
    ![Json_Pub_Setting5][Json_Pub_Setting5]


> [!NOTE]
> Şablonunuzdaki tüm Düğüm tipleri/Microsoft.Compute/virtualMachineScaleSets kaynak tanımları için yinelenen 4 ve 5 adımlarını olduğundan emin olun. Bunlardan birini kaçırırsanız, sertifika o sanal makine ölçeği kümesine yüklenmez ve kümenin düşmesi de dahil olmak üzere kümenizde öngörülemeyen sonuçlar elde erecektir (kümenin güvenlik için kullanabileceği geçerli sertifikalarınız yoksa. Yani daha fazla devam etmeden önce, çift kontrol edin.
> 
> 

### <a name="edit-your-template-file-to-reflect-the-new-parameters-you-added-above"></a>Yukarıda eklediğiniz yeni parametreleri yansıtacak şekilde şablon dosyanızı edin
Takip etmek için [git-repo'dan](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/Cert-Rollover-Sample) gelen örneği kullanıyorsanız, 5-VM-1-NodeTypes-Secure.parameters_Step2.JSON örneğinde değişiklik yapmaya başlayabilirsiniz. 

Kaynak Yöneticisi Şablon parametre Dosyanızı edin, secCertificateThumbprint ve secCertificateUrlValue için iki yeni parametre ekleyin. 

```JSON
    "secCertificateThumbprint": {
      "value": "thumbprint value"
    },
    "secCertificateUrlValue": {
      "value": "Refers to the location URL in your key vault where the certificate was uploaded, it is should be in the format of https://<name of the vault>.vault.azure.net:443/secrets/<exact location>"
     },

```

### <a name="deploy-the-template-to-azure"></a>Şablonu Azure'a dağıtma

- Artık şablonunuzu Azure'a dağıtmaya hazırsınız. Azure PS sürüm 1+ komut istemini açın.
- Azure Hesabınızda oturum açın ve belirli azure aboneliğini seçin. Bu, birden fazla azure aboneliğine erişimi olan kişiler için önemli bir adımdır.

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionId <Subscription ID> 

```

Şablonu dağıtmadan önce sınama. Kümenizin şu anda dağıtıldığı aynı Kaynak Grubunu kullanın.

```powershell
Test-AzResourceGroupDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>

```

Şablonu kaynak grubunuza dağıtın. Kümenizin şu anda dağıtıldığı aynı Kaynak Grubunu kullanın. Yeni-AzResourceGroupDeployment komutunu çalıştırın. Varsayılan değer **artımlı**olduğundan modu belirtmeniz gerekmez.

> [!NOTE]
> Modu Tamamla'ya ayarlarsanız, şablonunuzda olmayan kaynakları yanlışlıkla silebilirsiniz. Bu yüzden bu senaryoda kullanmayın.
> 
> 

```powershell
New-AzResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>
```

Burada aynı powershell dolu bir örnektir.

```powershell
$ResourceGroup2 = "chackosecure5"
$TemplateFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure_Step2.json"
$TemplateParmFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure.parameters_Step2.json"

New-AzResourceGroupDeployment -ResourceGroupName $ResourceGroup2 -TemplateParameterFile $TemplateParmFile -TemplateUri $TemplateFile -clusterName $ResourceGroup2

```

Dağıtım tamamlandıktan sonra, yeni Sertifika'yı kullanarak kümenize bağlanın ve bazı sorgular gerçekleştirin. Eğer yapabilirsen. Ardından eski sertifikayı silebilirsiniz. 

Kendi imzalı bir sertifika kullanıyorsanız, bunları yerel TrustedPeople sertifika mağazanıza aktarmayı unutmayın.

```powershell
######## Set up the certs on your local box
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)

```
Burada hızlı başvuru için güvenli bir kümeye bağlanmak için komut 

```powershell
$ClusterName= "chackosecure5.westus.cloudapp.azure.com:19000"
$CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7SD1D630F8F3" 

Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCertThumbprint $CertThumbprint  `
    -FindType FindByThumbprint `
    -FindValue $CertThumbprint `
    -StoreLocation CurrentUser `
    -StoreName My
```
Hızlı başvuru için burada küme sağlık almak için komutu

```powershell
Get-ServiceFabricClusterHealth 
```

## <a name="deploying-client-certificates-to-the-cluster"></a>İstemci sertifikalarını kümeye dağıtma.

Sertifikaların bir anahtar kasasından Düğümlere dağıtılması için önceki Adımlar 5'te belirtilen adımları kullanabilirsiniz. Sadece tanımlamak ve farklı parametreleri kullanmanız gerekir.


## <a name="adding-or-removing-client-certificates"></a>İstemci sertifikaları ekleme veya kaldırma

Küme sertifikalarına ek olarak, Hizmet Kumaşı kümesinde yönetim işlemleri gerçekleştirmek için istemci sertifikaları ekleyebilirsiniz.

İki tür istemci sertifikası ekleyebilirsiniz - Yönetici veya Salt Okunur. Bunlar daha sonra kümedeki yönetici işlemlerine ve Sorgu işlemlerine erişimi denetlemek için kullanılabilir. Varsayılan olarak, küme sertifikaları izin verilen Yönetici sertifikaları listesine eklenir.

istediğiniz sayıda istemci sertifikası belirtebilirsiniz. Her ekleme/silme işlemi, Service Fabric kümesine bir yapılandırma güncelleştirmesi ile sonuçlanır


### <a name="adding-client-certificates---admin-or-read-only-via-portal"></a>İstemci sertifikaları ekleme - Portal üzerinden Yönetici veya Salt Okunur

1. Güvenlik bölümüne gidin ve güvenlik bölümünün üstündeki '+ Kimlik Doğrulama' düğmesini seçin.
2. 'Kimlik Doğrulama Ekle' bölümünde, 'Kimlik Doğrulama Türü' - 'Salt okunur istemci' veya 'Yönetici istemci' seçeneğini belirleyin
3. Şimdi Yetkilendirme yöntemini seçin. Bu, Hizmet Kumaşı'na özne adını veya parmak izini kullanarak bu sertifikayı araması gerekip gerekmediğini gösterir. Genel olarak, konu adı yetkilendirme yöntemini kullanmak iyi bir güvenlik uygulaması değildir. 

![İstemci sertifikası ekle][Add_Client_Cert]

### <a name="deletion-of-client-certificates---admin-or-read-only-using-the-portal"></a>İstemci Sertifikalarının Silinmesi - Portalı kullanarak Yönetici veya Salt Okunur

İkincil bir sertifikanın küme güvenliği için kullanılmasından kaldırmak için Güvenlik bölümüne gidin ve belirli sertifikadaki bağlam menüsünden 'Sil' seçeneğini belirleyin.

## <a name="adding-application-certificates-to-a-virtual-machine-scale-set"></a>Sanal makine ölçeği kümesine uygulama sertifikaları ekleme

Uygulamalarınız için kullandığınız bir sertifikayı kümenize dağıtmak için [bu örnek Powershell komut dosyasına](scripts/service-fabric-powershell-add-application-certificate.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar
Küme yönetimi hakkında daha fazla bilgi için bu makaleleri okuyun:

* [Hizmet Kumaş Kümesi yükseltme süreci ve sizden beklentiler](service-fabric-cluster-upgrade.md)
* [İstemciler için rol tabanlı erişim](service-fabric-cluster-security-roles.md)

<!--Image references-->
[Add_Client_Cert]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_13.PNG
[Json_Pub_Setting1]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_14.PNG
[Json_Pub_Setting2]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_15.PNG
[Json_Pub_Setting3]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_16.PNG
[Json_Pub_Setting4]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_17.PNG
[Json_Pub_Setting5]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_18.PNG


