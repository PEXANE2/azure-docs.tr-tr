---
title: Azure Hizmet Kumaşı uygulama kaynak modeli
description: Bu makalede, Azure Kaynak Yöneticisi kullanarak bir Azure Hizmet Kumaşı uygulamasını yönetmeye genel bir bakış sağlanmaktadır.
ms.topic: conceptual
ms.date: 10/21/2019
ms.custom: sfrev
ms.openlocfilehash: 69c10b0e9d3b7c29122c8432ab1e4bc06d3a3120
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79481125"
---
# <a name="service-fabric-application-resource-model"></a>Hizmet Kumaş uygulama kaynak modeli

Hizmet Kumaşı kümenizde Azure Hizmet Kumaşı uygulamalarını dağıtmak için birden çok seçeneğiniz vardır. Azure Kaynak Yöneticisi'ni kullanmanızı öneririz. Kaynak Yöneticisi kullanıyorsanız, JSON'daki uygulamaları ve hizmetleri açıklayabilir ve bunları kümenizle aynı Kaynak Yöneticisi şablonunda dağıtabilirsiniz. Uygulamaları dağıtmak ve yönetmek için PowerShell veya Azure CLI'yi kullanmanın aksine, Kaynak Yöneticisi kullanıyorsanız kümenin hazır olmasını beklemeniz gerekmez; uygulama kaydı, sağlama ve dağıtım tek bir adımda gerçekleşebilir. Kaynak Yöneticisi'ni kullanmak, kümenizdeki uygulama yaşam döngüsünü yönetmenin en iyi yoludur. Daha fazla bilgi için [bkz: Kod olarak altyapı.](service-fabric-best-practices-infrastructure-as-code.md#azure-service-fabric-resources)

Uygulamalarınızı Kaynak Yöneticisi'nde kaynak olarak yönetme, bu alanlarda iyileştirmeler elde edebilirsiniz:

* Denetim izi: Kaynak Yöneticisi her işlemi denetler ve ayrıntılı bir etkinlik günlüğü tutar. Etkinlik günlüğü, uygulamalarda ve kümenizde yapılan değişiklikleri izlemenize yardımcı olabilir.
* Rol tabanlı erişim denetimi: Aynı Kaynak Yöneticisi şablonu kullanarak kümelere ve kümede dağıtılan uygulamalara erişimi yönetebilirsiniz.
* Yönetim verimliliği: Kaynak Yöneticisi'ni kullanmak, küme ve kritik uygulama dağıtımlarınızı yönetmek için size tek bir konum (Azure portalı) sağlar.

Bu belgede, nasıl öğreneceksiniz:

> [!div class="checklist"]
>
> * Kaynak Yöneticisi'ni kullanarak uygulama kaynaklarını dağıtın.
> * Kaynak Yöneticisi'ni kullanarak uygulama kaynaklarını yükseltin.
> * Uygulama kaynaklarını silin.

## <a name="deploy-application-resources"></a>Uygulama kaynaklarını dağıtma

Kaynak Yöneticisi uygulama kaynak modelini kullanarak bir uygulamayı ve hizmetlerini dağıtmak için attığınız üst düzey adımlar şunlardır:
1. Uygulama kodunu paketle.
1. Paketi yükleyin.
1. Uygulama kaynağı olarak Kaynak Yöneticisi şablonundaki paketin konumuna başvurun. 

Daha fazla bilgi [için, paket bir uygulamayı](service-fabric-package-apps.md#create-an-sfpkg)görüntüleyin.

Ardından, bir Kaynak Yöneticisi şablonu oluşturur, parametreler dosyasını uygulama ayrıntılarıyla güncelleştirin ve şablonu Hizmet Dokusu kümesine dağıtırsınız. [Örnekleri keşfedin.](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/tree/master/ARM)

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Kaynak Yöneticisi şablonundan bir uygulama dağıtmak için bir depolama hesabınız olması gerekir. Depolama hesabı uygulama görüntüsünü sahnelemek için kullanılır. 

Varolan bir depolama hesabını yeniden kullanabilir veya uygulamalarınızı evrelemek için yeni bir depolama hesabı oluşturabilirsiniz. Varolan bir depolama hesabı kullanıyorsanız, bu adımı atlayabilirsiniz. 

![Depolama hesabı oluşturma][CreateStorageAccount]

### <a name="configure-your-storage-account"></a>Depolama hesabınızı yapılandırma

Depolama hesabı oluşturulduktan sonra, uygulamaların sahnelenebildiği bir blob kapsayıcısı oluşturursunuz. Azure portalında, uygulamalarınızı depolamak istediğiniz Azure Depolama hesabına gidin. **Blobs** > **Ekle Kapsayıcı'yı**seçin. 

Kümenizdeki kaynaklar, genel erişim düzeyini **özel**olarak ayarlayarak güvence altına alınabilir. Birden çok şekilde erişim izni verebilirsiniz:

* Azure Etkin Dizini'ni kullanarak blob'lara ve kuyruklara erişimi [yetkilendirmeyin.](../storage/common/storage-auth-aad-app.md)
* [Azure portalında RBAC'ı](../storage/common/storage-auth-aad-rbac-portal.md)kullanarak Azure blob ve kuyruk verilerine erişim izni verin.
* Paylaşılan bir [erişim imzasını](https://docs.microsoft.com/rest/api/storageservices/delegate-access-with-shared-access-signature)kullanarak temsilci erişimi.

Aşağıdaki ekran görüntüsündeki örnekte, lekeler için anonim okuma erişimi kullanır.

![Leke oluşturma][CreateBlob]

### <a name="stage-the-application-in-your-storage-account"></a>Uygulamayı depolama hesabınızda aşamalı olarak düzenleme

Bir uygulamayı dağıtmadan önce, uygulamayı blob depolama alanında aşamalı olarak düzenlemeniz gerekir. Bu eğitimde, uygulama paketini el ile oluştururuz. Bu adımın otomatikleştirileebileceğini unutmayın. Daha fazla bilgi için [bir uygulamayı Paketle'ye](service-fabric-package-apps.md#create-an-sfpkg)bakın. 

Bu öğreticide, [Oylama örnek uygulamasını](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart)kullanıyoruz.

1. Visual Studio'da **Oylama** projesine sağ tıklayın ve ardından **Paket'i**seçin.

   ![Paket Uygulaması][PackageApplication]  
1. *.\service-fabric-dotnet-quickstart\Voting\pkg\Hata ayıklama* dizinine gidin. İçeriği *Oylama.zip*adlı bir dosyaya gönderin. *ApplicationManifest.xml* dosyası zip dosyasının kökünde olmalıdır.

   ![Zip Uygulaması][ZipApplication]  
1. Uzantısı .zip'ten *.sfpkg'a*değiştirmek için dosyayı yeniden adlandırın.

1. Azure portalında, depolama hesabınıziçin **uygulama** konteynerinde **Yükle'yi**seçin ve ardından **Voting.sfpkg'yi yükleyin.** 

   ![Uygulama Paketi Yükle][UploadAppPkg]

Şimdi, uygulama şimdi aşamalı ve uygulamayı dağıtmak için Kaynak Yöneticisi şablonu oluşturabilirsiniz.

### <a name="create-the-resource-manager-template"></a>Resource Manager şablonu oluşturma

Örnek uygulama, uygulamayı dağıtmak için kullanabileceğiniz [Azure Kaynak Yöneticisi şablonlarını](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/tree/master/ARM) içerir. Şablon dosya adları *UserApp.json* ve *UserApp.Parameters.json' dur.*

> [!NOTE]
> *UserApp.Parameters.json* dosyası kümenizin adı ile güncelleştirilmelidir.
>
>

| Parametre              | Açıklama                                 | Örnek                                                      | Yorumlar                                                     |
| ---------------------- | ------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| clusterName            | Dağıtmak üzere dağıttığınız kümenin adı | sf-küme123                                                |                                                              |
| uygulama            | Uygulamanın adı                 | Oylama                                                       |
| uygulamaTypeName    | Uygulamanın türü adı           | Oylama Türü                                                   | ApplicationManifest.xml ile eşleşmeli                 |
| uygulamaTypeVersion | Uygulama türünün sürümü         | 1.0.0                                                        | ApplicationManifest.xml ile eşleşmeli                 |
| Hizmetadı            | Hizmetin adı         | Oylama ~VotingWeb                                             | ApplicationName~ServiceType biçiminde olmalıdır            |
| serviceTypeName        | Hizmetin türü adı                | VotingWeb                                                    | ServiceManifest.xml ile eşleşmeli                 |
| appPackageUrl          | Uygulamanın blob depolama URL'si     | https://servicefabricapps.blob.core.windows.net/apps/Voting.sfpkg | Blob depolamasında uygulama paketinin URL'si (URL'yi ayarlama yordamı daha sonra makalede açıklanmıştır) |

```json
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applications",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes/versions",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'), '/', parameters('applicationTypeVersion'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applications/services",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'), '/', parameters('serviceName'))]",
    "location": "[variables('clusterLocation')]"
}
```

### <a name="deploy-the-application"></a>Uygulamayı dağıtma

Uygulamayı kümenizi içeren kaynak grubuna dağıtmak için **Yeni Kaynak GrubuDağıtım** cmdlet'ini çalıştırın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName "sf-cluster-rg" -TemplateParameterFile ".\UserApp.Parameters.json" -TemplateFile ".\UserApp.json" -Verbose
```

## <a name="upgrade-the-service-fabric-application-by-using-resource-manager"></a>Kaynak Yöneticisi'ni kullanarak Hizmet Kumaşı uygulamasını yükseltme

Hizmet Kumaşı kümesine zaten dağıtılan bir uygulamayı şu nedenlerden biriyle yükseltebilirsiniz:

* Uygulamaya yeni bir hizmet eklenir. Bir hizmet uygulamaya eklendiğinde *service-manifest.xml* ve *application-manifest.xml* dosyalarına bir hizmet tanımı eklenmelidir. Bir uygulamanın yeni bir sürümünü yansıtmak için [UserApp.Parameters.json'da](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/blob/master/ARM/UserApp.Parameters.json)uygulama türü sürümünü 1.0.0'dan 1.0.1'e değiştirmeniz gerekir:

    ```json
    "applicationTypeVersion": {
        "value": "1.0.1"
    },
    "serviceName2": {
        "value": "Voting~VotingData"
    },
    "serviceTypeName2": {
        "value": "VotingDataType"
    }
    ```

* Varolan bir hizmetin yeni bir sürümü uygulamaya eklenir. Örnekler uygulama kodu değişiklikleri ve uygulama türü sürümü ve adı güncellemeleri içerir. Bu yükseltme için UserApp.Parameters.json'u şu şekilde güncelleyin:

    ```json
     "applicationTypeVersion": {
        "value": "1.0.1"
    },
    ```

## <a name="delete-application-resources"></a>Uygulama kaynaklarını silme

Kaynak Yöneticisi'nde uygulama kaynak modelini kullanarak dağıtılan bir uygulamayı silmek için:

1. Uygulama için kaynak kimliğini almak için [Get-AzResource](https://docs.microsoft.com/powershell/module/az.resources/get-azresource?view=azps-2.5.0) cmdlet'ini kullanın:

    ```powershell
    Get-AzResource  -Name <String> | f1
    ```

1. Uygulama kaynaklarını silmek için [Remove-AzResource](https://docs.microsoft.com/powershell/module/az.resources/remove-azresource?view=azps-2.5.0) cmdlet'ini kullanın:

    ```powershell
    Remove-AzResource  -ResourceId <String> [-Force] [-ApiVersion <String>]
    ```

## <a name="next-steps"></a>Sonraki adımlar

Uygulama kaynak modeli hakkında bilgi alın:

* [Hizmet Kumaşı'nda bir uygulama modeli](service-fabric-application-model.md)
* [Hizmet Kumaş uygulama ve hizmet bildirimleri](service-fabric-application-and-service-manifests.md)
* [En iyi uygulamalar: Kod olarak altyapı](service-fabric-best-practices-infrastructure-as-code.md#azure-service-fabric-resources)
* [Uygulamaları ve hizmetleri Azure kaynakları olarak yönetme](service-fabric-best-practices-infrastructure-as-code.md)


<!--Image references-->
[CreateStorageAccount]: ./media/service-fabric-application-model/create-storage-account.png
[CreateBlob]: ./media/service-fabric-application-model/create-blob.png
[PackageApplication]: ./media/service-fabric-application-model/package-application.png
[ZipApplication]: ./media/service-fabric-application-model/zip-application.png
[UploadAppPkg]: ./media/service-fabric-application-model/upload-app-pkg.png
