---
title: Uzak bir Linux Kümesinde A.Net Core uygulaması oluşturma ve yayımla
description: Visual Studio'dan uzak bir Linux kümesini hedefleyen .Net Core uygulamaları oluşturma ve yayınlama
author: peterpogorski
ms.topic: troubleshooting
ms.date: 5/20/2019
ms.author: pepogors
ms.openlocfilehash: c30eedb6782e4172d677f16e27441f28c78cdd89
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75614358"
---
# <a name="use-visual-studio-to-create-and-publish-net-core-applications-targeting-a-remote-linux-service-fabric-cluster"></a>Uzak bir Linux Hizmeti Kumaş kümesini hedefleyen .Net Core uygulamaları oluşturmak ve yayınlamak için Visual Studio'yu kullanın
Visual Studio aracı ile Linux Service Fabric kümesini hedefleyen Service Fabric .Net Core uygulamalarını geliştirebilir ve yayınlayabilirsiniz. SDK sürümü Visual Studio Linux Service Fabric kümeleri hedefleyen bir .Net Core uygulaması dağıtmak için 3.4 veya üzeri olmalıdır.

> [!Note]
> Visual Studio, Linux'u hedefleyen hata ayıklama Hizmeti Kumaş uygulamalarını desteklemez.
>

## <a name="create-a-service-fabric-application-targeting-net-core"></a>.Net Core'u hedefleyen Bir Hizmet Kumaşı uygulaması oluşturma
1. Visual Studio'yu **yönetici** olarak başlatın.
2. Dosya >**Yeni >Projesi**ile proje oluşturun.
3. Yeni **Proje** iletişim kutusunda Bulut **-> Hizmet Kumaş Uygulaması'nı**seçin.
![oluşturma-uygulama]
4. Uygulamayı adlandırın ve **Tamam'ı**tıklatın.
5. Yeni **Hizmet Kumaş Servisi** sayfasında,.Net **Çekirdek Bölümü**altında oluşturmak istediğiniz hizmet türünü seçin.
![oluşturma-hizmet]

## <a name="deploy-to-a-remote-linux-cluster"></a>Uzak bir Linux kümesine dağıtın
1. Çözüm gezgininde, uygulamaya sağ tıklayın ve **Oluştur'u**seçin.
![yapı-uygulama]
2. Uygulama için yapı işlemi tamamlandıktan sonra, hizmete sağ tıklayın ve **csproj dosyasını**seçin.
![edit-csproj]
3. Hizmet bir **aktör proje türüyse,** UpdateServiceFabricManifestEnabled özelliğini True to False'dan **True'ya** güncelleyin. Uygulamanızda bir aktör hizmeti yoksa, adım 4'e atlayın.
```xml
    <UpdateServiceFabricManifestEnabled>False</UpdateServiceFabricManifestEnabled>
```
> [!Note]
> GüncellemeServiceFabricManifestEnabled yanlış ayarı, bir yapı sırasında ServiceManifest.xml güncelleştirmeleri devre dışı bırakacaktır. Hizmete ekleme, kaldırma veya yeniden adlandırma gibi herhangi bir değişiklik ServiceManifest.xml'e yansıtılmaz. Herhangi bir değişiklik yapılırsa ServiceManifest'i el ile güncellemeniz veya UpdateServiceFabricManifestEnabled'ı geçici olarak doğru şekilde ayarlamanız ve ServiceManifest.xml'i güncelleyecek hizmeti oluşturmanız ve ardından yanlışa geri dönmeniz gerekir.
>

4. RuntimeIndetifier'i win7-x64'ten hizmet projesindeki hedef platforma güncelleştirin.
```xml
    <RuntimeIdentifier>ubuntu.16.04-x64</RuntimeIdentifier>
```
5. ServiceManifest'te ,.exe'yi kaldırmak için giriş noktası programını güncelleştirin. 
```xml
    <EntryPoint> 
    <ExeHost> 
        <Program>Actor1</Program> 
    </ExeHost> 
    </EntryPoint>
```
6. Çözüm Gezgini'nde, uygulamaya sağ tıklayın ve **Yayımla'yı**seçin. **Yayımla** iletişim kutusu görüntülenir.
7. **Bağlantı Bitiş Noktası'nda,** hedeflemek istediğiniz uzak Hizmet Kumaşı Linux kümesinin bitiş noktasını seçin.
![yayımlama-uygulama]

<!--Image references-->
[oluşturma-uygulama]:./media/service-fabric-how-to-vs-remote-linux-cluster/create-application-remote-linux.png
[oluşturma-hizmet]:./media/service-fabric-how-to-vs-remote-linux-cluster/create-service-remote-linux.png
[yapı-uygulama]:./media/service-fabric-how-to-vs-remote-linux-cluster/build-application-remote-linux.png
[edit-csproj]:./media/service-fabric-how-to-vs-remote-linux-cluster/edit-csproj-remote-linux.png
[yayımlama-uygulama]:./media/service-fabric-how-to-vs-remote-linux-cluster/publish-remote-linux.png

## <a name="next-steps"></a>Sonraki adımlar
* [.Net Core ile Hizmet Kumaşı ile Başlarken](https://azure.microsoft.com/resources/samples/service-fabric-dotnet-core-getting-started/) Hakkında Bilgi Edinin
