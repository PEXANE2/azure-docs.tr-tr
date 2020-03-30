---
title: Azure'da Service Fabric'te Windows kapsayıcı uygulaması oluşturma
description: Bu hızlı başlangıçta, Azure Service Fabric üzerinde ilk Windows kapsayıcı uygulamanızı oluşturursunuz.
ms.topic: quickstart
ms.date: 07/10/2019
ms.author: atsenthi
ms.custom: mvc
ms.openlocfilehash: 477d47fabc59c5718c449418f225d6a38838b270
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75466273"
---
# <a name="quickstart-deploy-windows-containers-to-service-fabric"></a>Hızlı başlangıç: Windows kapsayıcıları Service Fabric'e dağıtma

Azure Service Fabric; ölçeklenebilir ve güvenilir mikro hizmetleri ve kapsayıcıları dağıtmayı ve yönetmeyi sağlayan bir dağıtılmış sistemler platformudur.

Bir Service Fabric kümesindeki Windows kapsayıcısında mevcut olan bir uygulamayı çalıştırmak için uygulamanızda herhangi bir değişiklik yapılması gerekmez. Bu hızlı başlangıç, Service Fabric uygulamasında önceden oluşturulmuş bir Docker kapsayıcısı görüntüsünü dağıtmayı gösterir. İşi bittiğinde çalışan bir Windows Server Core 2016 Server ve IIS kapsayıcınız olur. Bu hızlı başlatma, bir Windows kapsayıcısı dağıtmayı açıklar. Bir Linux kapsayıcısını dağıtmak için [bu hızlı başlatmayı](service-fabric-quickstart-containers-linux.md) okuyun.

![IIS varsayılan web sayfası][iis-default]

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:

* Docker görüntü kapsayıcısını paketleme
* İletişimi yapılandırma
* Service Fabric uygulamasını oluşturma ve paketleme
* Kapsayıcı uygulamasını Azure’a dağıtma


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

* Bir Azure aboneliği ([ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturabilirsiniz).
* Şunları çalıştıran bir geliştirme bilgisayarı:
  * Visual Studio 2019 veya Windows 2019.
  * [Service Fabric SDK’sı ve araçları](service-fabric-get-started.md).

## <a name="package-a-docker-image-container-with-visual-studio"></a>Visual Studio ile Docker görüntü kapsayıcısını paketleme

Service Fabric SDK’sı ve araçları, bir kapsayıcıyı Service Fabric kümesine dağıtmanıza yardımcı olan bir hizmet şablonu sağlar.

Visual Studio'yu “Yönetici” olarak başlatın.  **Dosya** > **Yeni** > **Proje'yi**seçin.

**Service Fabric uygulamasını**seçin, "MyFirstContainer" adını koyun ve **Oluştur'u**tıklatın.

**Barındırılan Kapsayıcılar ve Uygulamalar** şablonlarından **Kapsayıcı**’yı seçin.

**Resim Adı'nda**, Windows [Server Core Server ve IIS temel görüntüsüolan](https://hub.docker.com/_/microsoft-windows-servercore-iis)"mcr.microsoft.com/windows/servercore/iis:windowsservercore-ltsc2016"yi girin.

80 numaralı bağlantı noktasında hizmete gelen isteklerin, kapsayıcı üzerindeki 80 numaralı bağlantı noktasıyla eşlenmesi için kapsayıcının bağlantı noktasından konağa bağlantı noktası eşlenmesini yapılandırın.  **Kapsayıcı Bağlantı Noktası**’nı "80" olarak ve **Ana Bilgisayar Bağlantı Noktası**’nı "80" olarak ayarlayın.  

Hizmeti "MyContainerService" olarak adlandırın ve **Tamam**’a tıklayın.

![Yeni hizmet iletişim kutusu][new-service]

## <a name="specify-the-os-build-for-your-container-image"></a>Kapsayıcı görüntünüz için OS derlemesini belirtme

Windows Server'ın belirli bir sürümüyle derlenen kapsayıcılar, Windows Server'ın farklı sürümünü çalıştıran bir konakta çalışmayabilir. Örneğin, Windows Server sürüm 1709 kullanılarak oluşturulmuş kapsayıcılar, Windows Server 2016 çalıştıran ana bilgisayarlarda çalışmaz. Daha fazla bilgi için bkz. [Windows Server kapsayıcı işletim sistemi ve ana bilgisayar işletim sistemi uyumluluğu](service-fabric-get-started-containers.md#windows-server-container-os-and-host-os-compatibility). 

Service Fabric çalışma zamanının sürüm 6.1 veya daha yeni bir sürümüyle, kapsayıcı başına birden çok işletim sistemi görüntüsü belirtebilir ve her birini dağıtılacağı işletim sisteminin derleme sürümüyle etiketleyebilirsiniz. Bu, uygulamanızın Windows işletim sisteminin farklı sürümlerini çalıştıran konaklar arasında çalıştırılabilmesine yardımcı olur. Daha fazla bilgi edinmek için bkz. [İşletim sistemi derlemesine özgü kapsayıcı görüntüleri belirtme](service-fabric-get-started-containers.md#specify-os-build-specific-container-images). 

Microsoft, Windows Server'ın farklı sürümleri üzerinde oluşturulmuş IIS sürümleri için farklı görüntüler yayımlar. Service Fabric'in, uygulamanızın dağıtıldığı küme düğümlerinde çalıştırılan Windows Server sürümüyle uyumlu bir kapsayıcı dağıttığından emin olmak için, *ApplicationManifest.xml* dosyasına aşağıdaki satırları ekleyin. WIndows Server 2016 için derleme sürümü 14393, Windows Server 1709 sürümü için derleme sürümü 16299’dur.

```xml
    <ContainerHostPolicies CodePackageRef="Code"> 
      <ImageOverrides> 
        ...
          <Image Name="mcr.microsoft.com/windows/servercore/iis:windowsservercore-1803" /> 
          <Image Name= "mcr.microsoft.com/windows/servercore/iis:windowsservercore-ltsc2016" Os="14393" /> 
          <Image Name="mcr.microsoft.com/windows/servercore/iis:windowsservercore-1709" Os="16299" /> 
      </ImageOverrides> 
    </ContainerHostPolicies> 
```

Hizmet bildirimi, `mcr.microsoft.com/windows/servercore/iis:windowsservercore-ltsc2016` nano sunucusu için tek bir görüntü belirtmeye devam eder.

Ayrıca *ApplicationManifest.xml* dosyasında, **PasswordEncrypted'ı false**olarak değiştirin. **PasswordEncrypted** Hesap ve parola, Docker Hub'daki ortak kapsayıcı görüntüsü için boştur, bu nedenle boş bir parolayı şifrelemek bir yapı hatası oluşturacağı için şifrelemeyi kapatıyoruz.

```xml
<RepositoryCredentials AccountName="" Password="" PasswordEncrypted="false" />
```

## <a name="create-a-cluster"></a>Küme oluşturma

Aşağıdaki örnek komut dosyası, X.509 sertifikasıyla güvenli beş düğümlü Service Fabric kümesi oluşturur. Bu komut otomatik olarak imzalanan bir sertifika oluşturur ve bunu yeni bir anahtar kasasına yükler. Sertifika aynı zamanda bir yerel dizine de kopyalanır. [Hizmet Kumaşı küme oluştur'da](scripts/service-fabric-powershell-create-secure-cluster-cert.md)bu komut dosyalarını kullanarak küme oluşturma hakkında daha fazla bilgi edinebilirsiniz.

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/overview)bulunan yönergeleri kullanarak Azure PowerShell'i yükleyin.

Aşağıdaki komut dosyasını çalıştırmadan önce, Azure ile bağlantı oluşturmak için PowerShell'de çalıştırın. `Connect-AzAccount`

Aşağıdaki komut dosyasını panoya kopyalayın ve **Windows PowerShell ISE'yi**açın.  İçeriği boş İsimsiz1.ps1 penceresine yapıştırın. Ardından komut dosyasındaki değişkenler için `subscriptionId` `certpwd`değerler `certfolder` `adminuser`sağlayın: , , , `adminpwd`, ve benzeri.  Komut dosyasını çalıştırmadan önce belirttiğiniz dizin `certfolder` var olmalıdır.

[!code-powershell[main](../../powershell_scripts/service-fabric/create-secure-cluster/create-secure-cluster.ps1 "Create a Service Fabric cluster")]

Değişkenler için değerlerinizi sağladıktan sonra komut dosyasını çalıştırmak için **F5** tuşuna basın.

Komut dosyası çalışır ve küme oluşturulduktan `ClusterEndpoint` sonra çıktıyı bulun. Örnek:

```powershell
...
ClusterEndpoint : https://southcentralus.servicefabric.azure.com/runtime/clusters/b76e757d-0b97-4037-a184-9046a7c818c0
```

### <a name="install-the-certificate-for-the-cluster"></a>Küme için sertifikayı yükleme

Şimdi PFX'i *CurrentUser\My* sertifika deposuna yükleyeceğiz. PFX dosyası, yukarıdaki PowerShell komut dosyasında ortam değişkenini `certfolder` kullanarak belirttiğiniz dizinde olacaktır.

Bu dizini değiştirin ve ardından dizininizde `certfolder` bulunan PFX dosyasının adını ve değişkende belirttiğiniz parolayı değiştirerek `certpwd` aşağıdaki PowerShell komutunu çalıştırın. Bu örnekte, geçerli dizini PowerShell komut dosyasındaki `certfolder` değişken tarafından belirtilen dizine ayarlanır. Buradan `Import-PfxCertificate` komut çalıştırılır:

```powershell
PS C:\mycertificates> Import-PfxCertificate -FilePath .\mysfclustergroup20190130193456.pfx -CertStoreLocation Cert:\CurrentUser\My -Password (ConvertTo-SecureString Password#1234 -AsPlainText -Force)
```

Komut, Parmak İzi'ni döndürür:

```powershell
  ...
  PSParentPath: Microsoft.PowerShell.Security\Certificate::CurrentUser\My

Thumbprint                                Subject
----------                                -------
0AC30A2FA770BEF566226CFCF75A6515D73FC686  CN=mysfcluster.SouthCentralUS.cloudapp.azure.com
```

Aşağıdaki adımın parmak izi değerini hatırlayın.

## <a name="deploy-the-application-to-azure-using-visual-studio"></a>Visual Studio kullanarak uygulamayı Azure’a dağıtma

Uygulama hazır olduğuna göre, doğrudan Visual Studio'dan bir kümeye dağıtabilirsiniz.

Çözüm Gezgini'nde **MyFirstContainer**’a sağ tıklayın ve **Yayımla**’yı seçin. Yayımla iletişim kutusu görüntülenir.

Yukarıdaki `Import-PfxCertificate` komutu çalıştırdığınızda PowerShell penceresinde **CN=** aşağıdaki içeriği `19000` kopyalayın ve bağlantı noktası ekleyin. Örneğin, `mysfcluster.SouthCentralUS.cloudapp.azure.com:19000`. Bağlantı Bitiş **Noktası** alanına kopyalayın. Bu değeri unutmayın, çünkü ileribir adımda gereksinim duymanız gerekir.

**Gelişmiş Bağlantı Parametrelerine** tıklayıp bağlantı parametresi bilgilerini doğrulayın.  *FindValue* ve *ServerCertThumbprint* değerleri, önceki adımda çalıştırdığınızda `Import-PfxCertificate` yüklenen sertifikanın parmak izine uygun olmalıdır.

![Yayımla İletişim Kutusu](./media/service-fabric-quickstart-containers/publish-app.png)

**Yayımla**’ta tıklayın.

Kümedeki her uygulamanın benzersiz bir adı olmalıdır. Bir ad çakışması varsa, Visual Studio projesini yeniden adlandırın ve yeniden dağıtın.

Bir tarayıcı açın ve önceki adımda **Bağlantı Bitiş Noktası** alanına koyduğunuz adrese gidin. İsteğe bağlı olarak, URL’nin başına düzen tanımlayıcısını (`http://`) ve sonuna bağlantı noktasını (`:80`) ekleyebilirsiniz. Örneğin, http:\//mysfcluster.SouthCentralUS.cloudapp.azure.com:80.

 IIS varsayılan web sayfasını görmeniz gerekir: ![IIS varsayılan web sayfası][iis-default]

## <a name="clean-up"></a>Temizleme

Küme çalışırken ücretlendirmeyapmaya devam edeyim. [Kümenizi silmeyi](service-fabric-cluster-delete.md)düşünün.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta şunları öğrendiniz:

* Docker görüntü kapsayıcısını paketleme
* İletişimi yapılandırma
* Service Fabric uygulamasını oluşturma ve paketleme
* Kapsayıcı uygulamasını Azure’a dağıtma

Service Fabric’te Windows kapsayıcılarıyla çalışma hakkında daha fazla bilgi için Windows kapsayıcı uygulamaları öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Windows kapsayıcı uygulaması oluşturma](./service-fabric-host-app-in-a-container.md)

[iis-default]: ./media/service-fabric-quickstart-containers/iis-default.png
[publish-dialog]: ./media/service-fabric-quickstart-containers/publish-dialog.png
[new-service]: ./media/service-fabric-quickstart-containers/NewService.png
