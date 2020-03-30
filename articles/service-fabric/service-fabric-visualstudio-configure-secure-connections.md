---
title: Güvenli Azure Hizmet Kumaşı küme bağlantılarını yapılandırma
description: Azure Hizmet Kumaşı kümesi tarafından desteklenen güvenli bağlantıları yapılandırmak için Visual Studio'yu nasıl kullanacağınızı öğrenin.
author: cawaMS
ms.topic: conceptual
ms.date: 8/04/2017
ms.author: cawa
ms.openlocfilehash: 11f76153726d3fc92118fb46cc61b4627ab6a1b2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75464099"
---
# <a name="configure-secure-connections-to-a-service-fabric-cluster-from-visual-studio"></a>Visual Studio'dan Service Fabric kümesine güvenli bağlantıları yapılandırma
Yapılandırılmış erişim denetim ilkeleriyle azure hizmet kumaşı kümesine güvenli bir şekilde erişmek için Visual Studio'yı nasıl kullanacağınızı öğrenin.

## <a name="cluster-connection-types"></a>Küme bağlantı türleri
Azure Hizmet Dokusu kümesi tarafından desteklenen iki tür bağlantı vardır: **güvenli olmayan** bağlantılar ve **x509 sertifika tabanlı** güvenli bağlantılar. (Şirket içinde barındırılan Service Fabric kümeleri için **Windows** ve **dSTS** kimlik doğrulamaları da desteklenir.) Küme oluşturulurken küme bağlantı türünü yapılandırmanız gerekir. Oluşturulduktan sonra bağlantı türü değiştirilemez.

Visual Studio Service Fabric araçları, yayımlama için bir kümeye bağlanmak için tüm kimlik doğrulama türlerini destekler. Güvenli bir Hizmet Kumaşı kümesinin nasıl ayarlanaacağına ilişkin talimatlar için [Azure portalından Hizmet Kumaşı kümesi](service-fabric-cluster-creation-via-portal.md) ayarlama ya da bkz.

## <a name="configure-cluster-connections-in-publish-profiles"></a>Yayımlama profillerindeki küme bağlantılarını yapılandırma
Visual Studio'dan bir Hizmet Kumaşı projesi yayımlıyorsanız, Azure Hizmet Kumaşı kümesini seçmek için **Hizmet Kumaş Uygulaması Yayımla** iletişim kutusunu kullanın. **Bağlantı bitiş noktası**altında, aboneliğiniz altında varolan bir küme seçin.

![**Hizmet Kumaş Uygulamasını Yayımla** iletişim kutusu, Service Fabric bağlantısını yapılandırmak için kullanılır.][publishdialog]

**Hizmet Kumaş Uygulama Yayımla** iletişim kutusu küme bağlantısını otomatik olarak doğrular. İstenirse, Azure hesabınızda oturum açın. Doğrulama geçerse, sisteminizin kümeye güvenli bir şekilde bağlanmak için yüklü doğru sertifikalara sahip olduğu veya kümenizin güvenli olmadığı anlamına gelir. Doğrulama hataları, ağ sorunlarından veya sisteminizin güvenli bir kümeye bağlanmak üzere doğru şekilde yapılandırılmamasından kaynaklanabilir.

![**Hizmet Kumaş Uygulamasını Yayımla** iletişim kutusu varolan, doğru yapılandırılmış Service Fabric küme bağlantısını doğrular.][selectsfcluster]

### <a name="to-connect-to-a-secure-cluster"></a>Güvenli bir kümeye bağlanmak için
1. Hedef kümenin güvendiği istemci sertifikalarından birine erişebildiğinizden emin olun. Sertifika genellikle Kişisel Bilgi Alışverişi (.pfx) dosyası olarak paylaşılır. Bkz. Sunucuyu bir istemciye erişim sağlayacak şekilde yapılandırmak için [Azure portalından Hizmet Kumaşı kümesi ayarlama.](service-fabric-cluster-creation-via-portal.md)
2. Güvenilen sertifikayı yükleyin. Bunu yapmak için .pfx dosyasını çift tıklatın veya sertifikaları almak için PowerShell komut dosyası Alma-PfxCertificate'ı kullanın. Sertifikayı **Cert:\LocalMachine\My**' ye yükleyin. Sertifikayı alırken tüm varsayılan ayarları kabul etmek sorun değil.
3. **Azure Uygulamasını Yayımla** iletişim kutusunu açmak için projenin kısayol menüsündeki **Yayımla...** komutunu seçin ve ardından hedef kümeyi seçin. Araç bağlantıyı otomatik olarak çözer ve yayımlama profilindeki güvenli bağlantı parametrelerini kaydeder.
4. İsteğe bağlı: Güvenli küme bağlantısı belirtmek için yayımlama profilini edinebilirsiniz.
   
   Sertifika bilgilerini belirtmek için Profili Yazdır XML dosyasını el ile düzenlediğinize göre, sertifika deposunun adını, mağaza konumunu ve sertifika parmak izinizini not aldığınızdan emin olun. Sertifikanın mağaza adı ve mağaza konumu için bu değerleri sağlamanız gerekir. [Bkz. Nasıl?](https://msdn.microsoft.com/library/ms734695\(v=vs.110\).aspx)
   
   Service Fabric kümesine bağlanırken kullanılacak PowerShell parametrelerini belirtmek için *ClusterConnectionParametreler* parametrelerini kullanabilirsiniz. Geçerli parametreler Connect-ServiceFabricCluster cmdlet tarafından kabul edilen parametrelerdir. Kullanılabilir parametrelerin listesi için [Connect-ServiceFabricCluster'a](https://docs.microsoft.com/powershell/module/servicefabric/connect-servicefabriccluster) bakın.
   
   Uzak bir kümede yayımlıyorsanız, bu belirli küme için uygun parametreleri belirtmeniz gerekir. Aşağıda, güvenli olmayan bir kümeye bağlanma örneği verilmiştir:
   
   `<ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com:19000" />`
   
   X509 sertifika tabanlı güvenli kümeye bağlanmaya yönelik bir örnek aşağıda verilmiştir:
   
   ```xml
   <ClusterConnectionParameters
   ConnectionEndpoint="mycluster.westus.cloudapp.azure.com:19000"
   X509Credential="true"
   ServerCertThumbprint="0123456789012345678901234567890123456789"
   FindType="FindByThumbprint"
   FindValue="9876543210987654321098765432109876543210"
   StoreLocation="CurrentUser"
   StoreName="My" />
   ```
5. Yükseltme parametreleri ve Uygulama Parametre dosya konumu gibi diğer gerekli ayarları da güncelleyin ve ardından uygulamanızı Visual Studio'daki **Hizmet Kumaş Uygulaması** oturum kutusundan yayımlayın.

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric kümelerine erişim hakkında daha fazla bilgi için [Service Fabric Explorer'ı kullanarak kümenizi görselleştirme'ye](service-fabric-visualizing-your-cluster.md)bakın.

<!--Image references-->
[publishdialog]:./media/service-fabric-visualstudio-configure-secure-connections/publishdialog.png
[selectsfcluster]:./media/service-fabric-visualstudio-configure-secure-connections/selectsfcluster.png
