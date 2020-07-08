---
title: A.Net Core uygulamasını bir uzak Linux kümesine oluşturma ve yayımlama
description: Visual Studio 'dan uzak bir Linux kümesini hedefleyen .Net Core uygulamalar oluşturma ve yayımlama
author: peterpogorski
ms.topic: troubleshooting
ms.date: 5/20/2019
ms.author: pepogors
ms.openlocfilehash: c30eedb6782e4172d677f16e27441f28c78cdd89
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75614358"
---
# <a name="use-visual-studio-to-create-and-publish-net-core-applications-targeting-a-remote-linux-service-fabric-cluster"></a>Uzak bir Linux Service Fabric kümesini hedefleyen .Net Core uygulamalar oluşturmak ve yayımlamak için Visual Studio 'Yu kullanma
Visual Studio Araçları ile bir Linux Service Fabric kümesini hedefleyen Service Fabric .Net Core uygulamalar geliştirebilir ve yayımlayabilirsiniz. Visual Studio 'dan Linux Service Fabric kümelerini hedefleyen bir .Net Core uygulamasını dağıtmak için SDK sürümü 3,4 veya üzeri olmalıdır.

> [!Note]
> Visual Studio, Linux 'u hedefleyen Service Fabric uygulamalarda hata ayıklamayı desteklemez.
>

## <a name="create-a-service-fabric-application-targeting-net-core"></a>Service Fabric uygulama hedefleme .Net Core oluşturma
1. Visual Studio'yu **yönetici** olarak başlatın.
2. **File->New->Project**ile bir proje oluşturun.
3. **Yeni proje** Iletişim kutusunda **Cloud-> Service Fabric uygulaması**' nı seçin.
![oluşturma-uygulama]
4. Uygulamayı adlandırın ve **Tamam**' a tıklayın.
5. **Yeni Service Fabric hizmeti** sayfasında, **.NET Core bölümünde**oluşturmak istediğiniz hizmet türünü seçin.
![hizmet oluştur]

## <a name="deploy-to-a-remote-linux-cluster"></a>Uzak bir Linux kümesine dağıtma
1. Çözüm Gezgini 'nde uygulamaya sağ tıklayın ve **Oluştur**' u seçin.
![yapı-uygulama]
2. Uygulama için derleme işlemi tamamlandıktan sonra, hizmete sağ tıklayın ve **csproj dosyasını**Düzenle ' yi seçin.
![Düzenle-csproj]
3. Hizmet bir **aktör proje türü**Ise, UpdateServiceFabricManifestEnabled özelliğini true değerinden **false** olarak düzenleyin. Uygulamanızda bir aktör hizmeti yoksa 4. adıma atlayın.
```xml
    <UpdateServiceFabricManifestEnabled>False</UpdateServiceFabricManifestEnabled>
```
> [!Note]
> UpdateServiceFabricManifestEnabled özelliği false olarak ayarlandığında, derleme sırasında ServiceManifest.xml güncelleştirmeler devre dışı bırakılır. Hizmet ekleme, kaldırma veya yeniden adlandırma gibi değişiklikler ServiceManifest.xml yansıtılmayacaktır. Herhangi bir değişiklik yapılırsa, ServiceManifest 'i el ile güncelleştirmeniz veya UpdateServiceFabricManifestEnabled ayarını true olarak ayarlamanız gerekir, sonra da ServiceManifest.xml güncelleştirecek ve sonra yeniden false değerine geri döndürecek hizmeti oluşturacaksınız.
>

4. RuntimeIndetifier-x64 konumundan hizmet projesindeki hedef platforma güncelleştirin.
```xml
    <RuntimeIdentifier>ubuntu.16.04-x64</RuntimeIdentifier>
```
5. ServiceManifest 'de,. exe ' yi kaldırmak için EntryPoint programını güncelleştirin. 
```xml
    <EntryPoint> 
    <ExeHost> 
        <Program>Actor1</Program> 
    </ExeHost> 
    </EntryPoint>
```
6. Çözüm Gezgini, uygulamaya sağ tıklayıp **Yayımla**' yı seçin. **Yayımla** iletişim kutusu görüntülenir.
7. **Bağlantı uç noktası**' nda, hedeflemek istediğiniz uzak Service Fabric Linux kümesi için uç noktayı seçin.
![Yayımla-uygulama]

<!--Image references-->
[oluşturma-uygulama]:./media/service-fabric-how-to-vs-remote-linux-cluster/create-application-remote-linux.png
[hizmet oluştur]:./media/service-fabric-how-to-vs-remote-linux-cluster/create-service-remote-linux.png
[yapı-uygulama]:./media/service-fabric-how-to-vs-remote-linux-cluster/build-application-remote-linux.png
[Düzenle-csproj]:./media/service-fabric-how-to-vs-remote-linux-cluster/edit-csproj-remote-linux.png
[Yayımla-uygulama]:./media/service-fabric-how-to-vs-remote-linux-cluster/publish-remote-linux.png

## <a name="next-steps"></a>Sonraki adımlar
* [.NET Core ile Service Fabric kullanmaya](https://azure.microsoft.com/resources/samples/service-fabric-dotnet-core-getting-started/) başlama hakkında bilgi edinin
