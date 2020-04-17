---
title: Azure portalında bir Service Fabric kümesi oluşturma
description: Azure portalını ve Azure Anahtar Kasası'nı kullanarak Azure'da güvenli bir Hizmet Dokusu kümesini nasıl ayarlayamanızı öğrenin.
ms.topic: conceptual
ms.date: 09/06/2018
ms.openlocfilehash: e0cd3d5e5a37720134a5bce596bba211b375f19d
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81458326"
---
# <a name="create-a-service-fabric-cluster-in-azure-using-the-azure-portal"></a>Azure portalını kullanarak Azure'da Hizmet Dokusu kümesi oluşturma
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Azure portal](service-fabric-cluster-creation-via-portal.md)
> 
> 

Bu, Azure portalını kullanarak Azure'da Hizmet Kumaşı kümesi (Linux veya Windows) oluşturma adımlarında size yol gösteren adım adım bir kılavuzdur. Bu kılavuz size aşağıdaki adımları iletebilirsiniz:

* Azure portalı üzerinden Azure'da bir küme oluşturun.
* Sertifikaları kullanarak yöneticilerin kimliğini doğrula.

> [!NOTE]
> Azure Active Directory ile kullanıcı kimlik doğrulaması ve uygulama güvenliği için sertifika lar ayarlama gibi daha gelişmiş güvenlik seçenekleri [için, Azure Kaynak Yöneticisi'ni kullanarak kümenizi oluşturun.][create-cluster-arm]
> 
> 

## <a name="cluster-security"></a>Küme güvenliği 
Sertifikalar, Service Fabric’te bir küme ile uygulamalarının çeşitli yönlerini güvenli hale getirmek üzere kimlik doğrulaması ve şifreleme sağlamak için kullanılır. Sertifikaların Service Fabric’te kullanılmasıyla ilgili daha fazla bilgi için bkz. [Service Fabric kümesi güvenlik senaryoları][service-fabric-cluster-security].

İlk defa bir hizmet kumaş kümesi oluşturuyorsanız veya test iş yükleri için bir küme dağıtıyorsanız, bir sonraki bölüme **(Azure Portalı'nda küme oluşturma**) atlayabilir ve sistemin test iş yüklerini çalıştıran kümeleriniz için gerekli sertifikalar oluşturmasını sağlayabilirsiniz. Üretim iş yükleri için bir küme kuruyorsanız, okumaya devam edin.

#### <a name="cluster-and-server-certificate-required"></a>Küme ve sunucu sertifikası (gerekli)
Bu sertifika, bir kümenin güvenliğini sağlamak ve kümeye yetkisiz erişimi önlemek için gereklidir. Küme güvenliğini birkaç şekilde sağlar:

* **Küme kimlik doğrulaması:** Küme federasyonu için düğümden düğüme iletişimi doğrular. Kümeye yalnızca bu sertifikayla kimliklerini kanıtlayabilen düğümler katılabilir.
* **Sunucu kimlik doğrulaması:** Küme yönetiminin uç noktalarını bir yönetim istemcisine doğrular, böylece yönetim istemcisi gerçek kümeyle konuştuğunu bilir. Bu sertifika aynı zamanda HTTPS yönetim API'si ve HTTPS üzerinden Service Fabric Explorer için TLS sağlar.

Bu amaçlara hizmet etmek için sertifikanın aşağıdaki gereksinimleri karşılaması gerekir:

* Sertifika özel bir anahtar içermelidir.
* Sertifika anahtar değişimi için oluşturulmalıdır, Kişisel Bilgi Alışverişi (.pfx) dosyasına ihraç edilebilir.
* Sertifikanın özne adı, Hizmet Kumaşı kümesine erişmek için kullanılan **etki alanıyla eşleşmelidir.** Bu kümenin HTTPS yönetim bitiş noktaları ve Hizmet Kumaş Explorer için TLS sağlamak için gereklidir. `.cloudapp.azure.com` Etki alanı için bir sertifika yetkilisinden (CA) TLS/SSL sertifikası alamazsınız. Kümeniz için özel bir etki alanı adı edinin. CA'dan sertifika istediğinizde, sertifikanın özne adı kümeniz için kullanılan özel alan adıyla eşleşmelidir.

#### <a name="client-authentication-certificates"></a>İstemci kimlik doğrulama sertifikaları
Ek istemci sertifikaları küme yönetimi görevleri için yöneticilerin kimlik doğruluğunu sağlar. Service Fabric'in iki erişim düzeyi vardır: **yönetici** ve **salt okunur kullanıcı.** En azından, yönetim erişimi için tek bir sertifika kullanılmalıdır. Ek kullanıcı düzeyinde erişim için ayrı bir sertifika sağlanmalıdır. Erişim rolleri hakkında daha fazla bilgi için [Service Fabric istemcileri için rol tabanlı erişim denetimine][service-fabric-cluster-security-roles]bakın.

Service Fabric ile çalışmak için Müşteri kimlik doğrulama sertifikalarını Key Vault'a yüklemeniz gerekmez. Bu sertifikaların yalnızca küme yönetimi için yetkilendirilen kullanıcılara sağlanması gerekir. 

> [!NOTE]
> Azure Etkin Dizin, küme yönetimi işlemleri için istemcilerin kimliğini doğrulamanın önerilen yoludur. Azure Etkin Dizin'ini kullanmak için [Azure Kaynak Yöneticisi'ni kullanarak bir küme oluşturmanız][create-cluster-arm]gerekir.
> 
> 

#### <a name="application-certificates-optional"></a>Uygulama sertifikaları (isteğe bağlı)
Uygulama güvenliği amacıyla bir kümeye herhangi bir sayıda ek sertifika yüklenebilir. Kümenizi oluşturmadan önce, düğümlere bir sertifika nın yüklenmesini gerektiren uygulama güvenlik senaryolarını düşünün:

* Uygulama yapılandırma değerlerinin şifresini şifreleme ve çözme
* Çoğaltma sırasında düğümler arasında verilerin şifrelemesi 

Uygulama [sertifikaları, Azure portalı üzerinden küme oluştururken](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/service-fabric/service-fabric-cluster-creation-via-portal.md)yapılandırılamaz. Uygulama sertifikalarını küme kurulum zamanında yapılandırmak için [Azure Kaynak Yöneticisi'ni kullanarak bir küme oluşturmanız][create-cluster-arm]gerekir. Oluşturulduktan sonra kümeye uygulama sertifikaları da ekleyebilirsiniz.

## <a name="create-cluster-in-the-azure-portal"></a>Azure portalında küme oluşturma

Uygulama gereksinimlerinizi karşılamak için bir üretim kümesi oluşturmak, bu konuda size yardımcı olmak için bazı planlamalar içerir, [Hizmet Kumaş Kümesi planlama konuları][service-fabric-cluster-capacity] belgesini okumanız ve anlamanız önerilir. 

### <a name="search-for-the-service-fabric-cluster-resource"></a>Service Fabric küme kaynağını ara

[Azure Portal][azure-portal] oturum açın.
Yeni bir kaynak şablonu eklemek için **kaynak oluştur'u** tıklatın. **Her Şey**altında **Markette** Hizmet Kumaş Küme şablonu arayın.
Listeden **Hizmet Kumaş Kümesi'ni** seçin.

![Azure portalında Hizmet Dokusu küme şablonu arayın.][SearchforServiceFabricClusterTemplate]

**Service Fabric Cluster** bıçağına gidin ve **Oluştur'u**tıklatın.

**Hizmet Kumaşı Oluştur küme** bıçağı aşağıdaki dört adıma sahiptir:

### <a name="1-basics"></a>1. Temel Bilgiler
![Yeni bir kaynak grubu oluşturma ekran görüntüsü.][CreateRG]

Basics bıçak, küme için temel ayrıntıları sağlamanız gerekir.

1. Kümenizin adını girin.
2. VM'ler için Uzak Masaüstü için bir **Kullanıcı adı** ve **Parola** girin.
3. Kümenizin dağıtılmasını istediğiniz **Aboneliği** seçtiğinizden emin olun, özellikle de birden çok aboneliğiniz varsa.
4. Yeni bir **Kaynak grubu**oluşturun. Özellikle dağıtımınızda değişiklik yapmaya veya kümenizi silmeye çalışıyorsanız, kümeyle aynı adı vermek en iyisidir.
   
   > [!NOTE]
   > Varolan bir kaynak grubunu kullanmaya karar verebilirsiniz, ancak yeni bir kaynak grubu oluşturmak iyi bir uygulamadır. Bu, kümeleri ve kullandığı tüm kaynakları silmeyi kolaylaştırır.
   > 
   > 
5. Kümeoluşturmak istediğiniz **Konumu** seçin. Zaten anahtar kasasına yüklediğiniz varolan bir sertifikayı kullanmayı planlıyorsanız, Anahtar kasanızın bulunduğu bölgeyi kullanmanız gerekir. 

### <a name="2-cluster-configuration"></a>2. Küme yapılandırması
![Düğüm türü oluşturma][CreateNodeType]

Küme düğümlerinizi yapılandırın. Düğüm türleri VM boyutlarını, VM sayısını ve özelliklerini tanımlar. Kümenizde birden fazla düğüm türü olabilir, ancak birincil düğüm türü (portalda tanımladığınız ilk düğüm) en az beş VM olmalıdır, çünkü bu Hizmet Kumaşı sistem hizmetlerinin yerleştirildiği düğüm türüdür. "NodeTypeName" varsayılan yerleşim özelliği otomatik olarak eklenmiştir çünkü **Yerleşim Özellikleri** yapılandırmayın.

> [!NOTE]
> Birden çok düğüm türü için yaygın bir senaryo, ön uç hizmeti ve arka uç hizmeti içeren bir uygulamadır. Ön uç hizmetini, Internet'e açık bağlantı noktaları açık olan daha küçük VM'lere (D2_V2 gibi VM boyutları) koymak ve arka uç hizmetini Internet'e bakan bağlantı noktaları açık olmayan daha büyük VM'lere (D3_V2, D6_V2, D15_V2 vb. gibi VM boyutlarıyla) koymak istiyorsunuz.
> 

1. Düğüm türünüz için bir ad seçin (yalnızca harf ve sayı içeren 1 ila 12 karakter).
2. Birincil düğüm türü için en az VM **boyutu** küme için seçtiğiniz **Dayanıklılık katmanı** tarafından yönlendirilir. Dayanıklılık katmanı için varsayılan değer bronzdur. Dayanıklılık hakkında daha fazla bilgi için [Service Fabric küme dayanıklılığını nasıl seçeceğinizi][service-fabric-cluster-durability]görün.
3. Sanal **makine boyutunu**seçin. D serisi VM'ler SSD sürücülere sahiptir ve durum durumu yla ilgili uygulamalar için şiddetle tavsiye edilir. Kısmi çekirdekleri olan veya 10 GB'dan az kullanılabilir disk kapasitesine sahip VM SKU kullanmayın. VM boyutunu seçmede yardım için [hizmet kumaş küme planlama değerlendirme belgesine][service-fabric-cluster-capacity] bakın.
4.  **Tek düğüm kümesi ve üç düğüm kümeleri** yalnızca test kullanımı içindir. Çalışan üretim iş yükleri için desteklenmez.
5. Düğüm türü için **İlk VM ölçek kümesi kapasitesini** seçin. Daha sonra düğüm türündeki VM sayısını büyütebilir veya küçültebilirsiniz, ancak birincil düğüm türünde üretim iş yükleri için en az beştir. Diğer düğüm türleri en az bir VM'ye sahip olabilir. Birincil düğüm türü için en az VM **sayısı** kümenizin **güvenilirliğini** kullanır.  
6. Özel **uç noktalarını**yapılandırın. Bu alan, uygulamalarınız için Azure Yük Dengeleyicisi aracılığıyla herkese açık Internet'e çıkarmak istediğiniz iletişim den ayrılmış bağlantı noktaları nın listesini girmenizi sağlar. Örneğin, kümenize bir web uygulaması dağıtmayı planlıyorsanız, 80 no'daki bağlantı noktası trafiğine kümenize izin vermek için buraya "80" girin. Uç noktalar hakkında daha fazla bilgi için, [uygulamalarla iletişim kurma][service-fabric-connect-and-communicate-with-services]
7. **Ters proxy'yi etkinleştirin.**  [Service Fabric ters proxy,](service-fabric-reverseproxy.md) Service Fabric kümesinde çalışan mikro hizmetlerin http uç noktalarıolan diğer hizmetleri keşfetmeye ve onlarla iletişim kurmasına yardımcı olur.
8. **Küme yapılandırma** bıçak geri, **altında +Show isteğe bağlı ayarları,** küme **tanılama**yapılandırma. Varsayılan olarak, sorun giderme sorunlarına yardımcı olmak için kümenizde tanılama etkinleştirilir. Tanılamadevre devre dışı kalmak **istiyorsanız, Durum** geçişini **Kapalı**olarak değiştirin. Tanılamanın kapatılımı **önerilmez.** Zaten Uygulama Öngörüleri projesini oluşturduysanız, uygulama izlemelerinin ona yönlendirilen anahtarını verin.
9. **DNS hizmetini ekleyin.**  [DNS hizmeti, DNS](service-fabric-dnsservice.md) protokolünü kullanarak başka hizmetler bulmanıza olanak tanıyan isteğe bağlı bir hizmet.
10. Kümenizi ayarlamak istediğiniz **Kumaş yükseltme modunu** seçin. Sistemin en son sürümü otomatik olarak almasını ve kümenizi yükseltmeyi denemesini istiyorsanız **Otomatik'i**seçin. Desteklenen bir sürüm seçmek istiyorsanız modu **El Ile**ayarlayın. Kumaş yükseltme modu hakkında daha fazla bilgi için [Service Fabric Cluster Yükseltme belgesine bakın.][service-fabric-cluster-upgrade]

> [!NOTE]
> Yalnızca Service Fabric'in desteklenen sürümlerini çalıştıran kümeleri destekliyoruz. **El Kitabı** modunu seçerek, kümenizi desteklenen bir sürüme yükseltme sorumluluğunu üstlenirsiniz.
> 

### <a name="3-security"></a>3. Güvenlik
![Azure portalındaki güvenlik yapılandırmalarının ekran görüntüsü.][BasicSecurityConfigs]

Güvenli bir test kümesi oluşturmayı sizin için kolay hale getirmek için **Temel** seçeneğini sayılttın. Zaten bir sertifikanız varsa ve bu sertifikayı [anahtar kasanıza](/azure/key-vault/) yüklediyseniz (ve dağıtım için anahtar kasasını etkinleştirdiyseniz), Özel **seçeneğini** kullanın

#### <a name="basic-option"></a>Temel Seçenek
Varolan bir anahtar kasası eklemek veya yeniden kullanmak ve sertifika eklemek için ekranları izleyin. Sertifikanın eklenmesi eşzamanlı bir işlemdir ve bu nedenle sertifikanın oluşturulmasını beklemeniz gerekir.

Önceki işlem tamamlanana kadar ekrandan uzaklaşmanın cazibesine karşı koyun.

![KeyVault oluşturma]

Artık anahtar kasası oluşturulduğuna göre, anahtar kasanızın erişim ilkelerini edin. 

![KeyVault2 oluşturma]

**Erişim İlkelerini Edit'i**tıklatın, ardından **gelişmiş erişim ilkelerini göster** ve dağıtım için Azure Sanal Makineleri'ne erişimi etkinleştirin. Şablon dağıtımını da etkinleştirmeniz önerilir. Seçimlerinizi yaptıktan sonra **Kaydet** düğmesini tıklatmayı ve **Access ilkeleri** bölmesini kapatmayı unutmayın.

![KeyVault3 oluşturma]

Sertifikanın adını girin ve **Tamam'ı**tıklatın.

![KeyVault4 oluşturma]

#### <a name="custom-option"></a>Özel Seçenek
**Temel** Seçenek'teki adımları zaten gerçekleştirdiyseniz, bu bölümü atlayın.

![SecurityCustomOption]

Güvenlik sayfasını tamamlamak için Kaynak anahtar kasası, Sertifika URL'si ve Sertifika parmak izi bilgilerine ihtiyacınız vardır. Kullanışlı değilseniz, başka bir tarayıcı penceresi açın ve Azure portalında aşağıdakileri yapın

1. Anahtar kasa hizmetine gidin.
2. "Özellikler" sekmesini seçin ve diğer tarayıcı penceresindeki "Kaynak anahtar kasası" olarak "KAYNAK Kimliği"ni kopyalayın 

    ![CertInfo0]

3. Şimdi, "Sertifikalar" sekmesini seçin.
4. Sizi Sürümler sayfasına götüren sertifika parmak izine tıklayın.
5. Geçerli Sürüm'ün altında gördüğünüz GUID'lere tıklayın.

    ![CertInfo1]

6. Şimdi aşağıdaki gibi ekranda olmalıdır. Hexadecimal SHA-1 Parmak Izini diğer tarayıcı penceresindeki "Sertifika parmak izi" olarak kopyalayın
7. Diğer tarayıcı penceresindeki "Sertifika URL'si" için 'Gizli Tanımlayıcı'yı kopyalayın.

    ![CertInfo2]

**Yönetici istemci** ve **salt okunur istemci**için istemci sertifikalarını girmek için Gelişmiş ayarları **Yapılandır'ı** işaretleyin. Bu alanlarda, yönetici istemci sertifikanızın parmak izini ve varsa salt okunur kullanıcı istemci sertifikanızın parmak izini girin. Yöneticiler kümeye bağlanmaya çalıştıklarında, yalnızca burada girilen parmak izi değerlerine uyan parmak izi olan bir sertifikaları varsa erişim hakkı verilir.  

### <a name="4-summary"></a>4. Özet

Artık kümeyi dağıtmaya hazırsınız. Bunu yapmadan önce, sertifikayı indirin, bağlantı için büyük mavi bilgi kutusunun içine bakın. Sertifikayı güvenli bir yerde tuttuğundan emin ol. kümenize bağlanmak için ihtiyacınız var. İndirdiğiniz sertifikanın şifresi olmadığından, parola eklemeniz önerilir.

Küme oluşturmayı tamamlamak için **Oluştur'u**tıklatın. Şablonu isteğe bağlı olarak indirebilirsiniz.

![Özet]

Oluşturma işleminin ilerleme durumunu bildirimler bölümünden görebilirsiniz. (Ekranınızın sağ üst kısmındaki durum çubuğunun yanındaki "Çan" simgesine tıklayın.) Küme oluşturulurken **Başlangıç Panosuna** Sabitle'yi tıklattıysanız, **Başlat** panosuna sabitlenmiş Hizmet Kumaş **Kümesi dağıtmayı** görürsünüz. Bu işlem biraz zaman alacaktır. 

Powershell veya CLI kullanarak kümenizde yönetim işlemleri gerçekleştirmek için, kümenize bağlanmanız ve [kümenize](service-fabric-connect-to-secure-cluster.md)nasıl bağlanabileceğiniz hakkında daha fazla bilgi almanız gerekir.

## <a name="view-your-cluster-status"></a>Küme durumunuzu görüntüleme
![Panodaki küme ayrıntılarının ekran görüntüsü.][ClusterDashboard]

Kümeniz oluşturulduktan sonra, cluster'ınızı portalda inceleyebilirsiniz:

1. **Gözat'a** gidin ve **Servis Kumaş Kümeleri'ni**tıklatın.
2. Kümenizi bulun ve tıklatın.
3. Kümenin genel uç noktası ve bir Service Fabric Explorer bağlantısıyla birlikte kümenizin ayrıntılarını panoda görebilirsiniz.

Kümenin pano bıçağındaki **Düğüm Monitörü** bölümü, sağlıklı ve sağlıklı olmayan VM sayısını gösterir. [Service Fabric sağlık modeli girişte][service-fabric-health-introduction]kümenin sağlığı hakkında daha fazla bilgi bulabilirsiniz.

> [!NOTE]
> Service Fabric kümeleri, kullanılabilirliği korumak ve durumu korumak için her zaman belirli sayıda düğümün "çoğunluk sağlama" olarak adlandırılır. Bu nedenle, [genellikle ilk durum tam][service-fabric-reliable-services-backup-restore]bir yedekleme gerçekleştirmedikçe kümedeki tüm makineleri kapatmak için güvenli değildir.
> 
> 

## <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Sanal Makine Ölçeği ne set örneğine veya küme düğümüne uzaktan bağlanma
Küme sonuçlarınızda belirttiğiniz NodeTypes'ların her biri sanal makine ölçeği kümesinde kurulumu yapılır. <!--See [Remote connect to a Virtual Machine Scale Set instance][remote-connect-to-a-vm-scale-set] for details. -->

## <a name="next-steps"></a>Sonraki adımlar
Bu noktada, yönetim kimlik doğrulaması için sertifikaları kullanan güvenli bir kümeniz vardır. Ardından, [kümenize bağlanın](service-fabric-connect-to-secure-cluster.md) ve uygulama sırlarını nasıl [yönetiniz](service-fabric-application-secret-management.md)gerektiğini öğrenin.  Ayrıca, [Service Fabric destek seçenekleri](service-fabric-support.md)hakkında bilgi edinin.

<!-- Links -->
[azure-powershell]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[azure-portal]: https://portal.azure.com/
[key-vault-get-started]: ../key-vault/general/overview.md
[create-cluster-arm]: service-fabric-cluster-creation-via-arm.md
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[service-fabric-cluster-security-roles]: service-fabric-cluster-security-roles.md
[service-fabric-cluster-capacity]: service-fabric-cluster-capacity.md
[service-fabric-cluster-durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster
[service-fabric-connect-and-communicate-with-services]: service-fabric-connect-and-communicate-with-services.md
[service-fabric-health-introduction]: service-fabric-health-introduction.md
[service-fabric-reliable-services-backup-restore]: service-fabric-reliable-services-backup-restore.md
[remote-connect-to-a-vm-scale-set]: service-fabric-cluster-nodetypes.md
[service-fabric-cluster-upgrade]: service-fabric-cluster-upgrade.md

<!--Image references-->
[SearchforServiceFabricClusterTemplate]: ./media/service-fabric-cluster-creation-via-portal/SearchforServiceFabricClusterTemplate.png
[CreateRG]: ./media/service-fabric-cluster-creation-via-portal/CreateRG.png
[CreateNodeType]: ./media/service-fabric-cluster-creation-via-portal/NodeType.png
[BasicSecurityConfigs]: ./media/service-fabric-cluster-creation-via-portal/BasicSecurityConfigs.PNG
[KeyVault oluşturma]: ./media/service-fabric-cluster-creation-via-portal/CreateKeyVault.PNG
[KeyVault2 oluşturma]: ./media/service-fabric-cluster-creation-via-portal/CreateKeyVault2.PNG
[KeyVault3 oluşturma]: ./media/service-fabric-cluster-creation-via-portal/CreateKeyVault3.PNG
[KeyVault4 oluşturma]: ./media/service-fabric-cluster-creation-via-portal/CreateKeyVault4.PNG
[CertInfo0]: ./media/service-fabric-cluster-creation-via-portal/CertInfo0.PNG
[CertInfo1]: ./media/service-fabric-cluster-creation-via-portal/CertInfo1.PNG
[CertInfo2]: ./media/service-fabric-cluster-creation-via-portal/CertInfo2.PNG
[SecurityCustomOption]: ./media/service-fabric-cluster-creation-via-portal/SecurityCustomOption.PNG
[DownloadCert]: ./media/service-fabric-cluster-creation-via-portal/DownloadCert.PNG
[Özet]: ./media/service-fabric-cluster-creation-via-portal/Summary.PNG
[SecurityConfigs]: ./media/service-fabric-cluster-creation-via-portal/SecurityConfigs.png
[Notifications]: ./media/service-fabric-cluster-creation-via-portal/notifications.png
[ClusterDashboard]: ./media/service-fabric-cluster-creation-via-portal/ClusterDashboard.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
