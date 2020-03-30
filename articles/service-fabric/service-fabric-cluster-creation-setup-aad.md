---
title: İstemci kimlik doğrulaması için Azure Etkin Dizini ayarlama
description: Hizmet Kumaşı kümeleri için istemcilerin kimliğini doğrulamak için Azure Active Directory'yi (Azure AD) nasıl ayarlayabilirsiniz öğrenin.
ms.topic: conceptual
ms.date: 6/28/2019
ms.openlocfilehash: 28c4c65cfcc77607dfe9a463a09ecd10389a6eca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78193398"
---
# <a name="set-up-azure-active-directory-for-client-authentication"></a>İstemci kimlik doğrulaması için Azure Etkin Dizini ayarlama

Azure'da çalışan kümeler için, yönetim bitiş noktalarına erişimi güvence altına almak için Azure Etkin Dizin (Azure AD) önerilir. Bu makalede, Bir Hizmet Kumaşı kümesi için istemcilerin kimliğini doğrulamak için Azure AD'nin nasıl düzenlenenekadar düzenlenenebildiğini açıklanmaktadır.

Bu makalede, "uygulama" terimi, Hizmet Kumaşı uygulamalarına değil, [Azure Etkin Dizin uygulamalarına](../active-directory/develop/developer-glossary.md#client-application)atıfta bulunmak için kullanılacaktır; ayrım gerektiğinde yapılacaktır. Azure AD, kuruluşların (kiracı olarak da bilinir) uygulamalara kullanıcı erişimini yönetmesini sağlar.

Bir Service Fabric küme web tabanlı [Hizmet Kumaş Explorer][service-fabric-visualizing-your-cluster] ve Visual [Studio][service-fabric-manage-application-in-visual-studio]dahil olmak üzere yönetim işlevselliği için çeşitli giriş noktaları sunuyor. Sonuç olarak, kümeye erişimi denetlemek için iki Azure AD uygulaması oluşturursunuz: bir web uygulaması ve bir yerel uygulama. Uygulamalar oluşturulduktan sonra, kullanıcıları salt okunur ve yönetici rollerine atarsınız.

> [!NOTE]
> Linux'ta, kümeoluşturmadan önce aşağıdaki adımları tamamlamanız gerekir. Windows'da, [varolan bir küme için Azure AD kimlik doğrulamasını yapılandırma](https://github.com/Azure/Service-Fabric-Troubleshooting-Guides/blob/master/Security/Configure%20Azure%20Active%20Directory%20Authentication%20for%20Existing%20Cluster.md)seçeneğiniz de vardır.

> [!NOTE]
> Linux AAD özellikli kümelerde uygulamaların ve düğümlerin Azure Portalı'nda görüntülenemeyeceği bilinen bir [konudur.](https://github.com/microsoft/service-fabric/issues/399)



## <a name="prerequisites"></a>Ön koşullar
Bu makalede, zaten bir kiracı oluşturduğunuzu varsayıyoruz. Yoksa, [Azure Etkin Dizin kiracısını nasıl alırsınız][active-directory-howto-tenant]okuyarak başlayın.

Azure AD'yi Hizmet Kumaşı kümesiyle yapılandırmayla ilgili bazı adımları basitleştirmek için bir dizi Windows PowerShell komut dosyası oluşturduk.

1. [Repo'yu](https://github.com/Azure-Samples/service-fabric-aad-helpers) bilgisayarınıza kopyala.
2. Komut dosyaları için tüm ön koşullar alabildiğinizden [emin olun.](https://github.com/Azure-Samples/service-fabric-aad-helpers#getting-started)

## <a name="create-azure-ad-applications-and-assign-users-to-roles"></a>Azure AD uygulamaları oluşturun ve kullanıcıları rollere atama

Komut dosyalarını kümeye erişimi denetlemek için iki Azure AD uygulaması oluşturmak için kullanırız: bir web uygulaması ve bir yerel uygulama. Kümenizi temsil edecek uygulamalar oluşturduktan sonra, Service [Fabric tarafından desteklenen roller](service-fabric-cluster-security-roles.md)için kullanıcılar oluşturursunuz: salt okunur ve yönetici.

Çalıştırın `SetupApplications.ps1`ve kiracı kimliğini, küme adını ve web uygulaması yanıt URL'sini parametreler olarak sağlayın.  Ayrıca kullanıcılar için kullanıcı adları ve parolalar belirtin. Örnek:

```powershell
$Configobj = .\SetupApplications.ps1 -TenantId '0e3d2646-78b3-4711-b8be-74a381d9890c' -ClusterName 'mysftestcluster' -WebApplicationReplyUrl 'https://mysftestcluster.eastus.cloudapp.azure.com:19080/Explorer/index.html' -AddResourceAccess
.\SetupUser.ps1 -ConfigObj $Configobj -UserName 'TestUser' -Password 'P@ssword!123'
.\SetupUser.ps1 -ConfigObj $Configobj -UserName 'TestAdmin' -Password 'P@ssword!123' -IsAdmin
```

> [!NOTE]
> Ulusal bulutlar için (örneğin Azure Devlet, Azure Çin, Azure `-Location` Almanya), parametreyi de belirtmeniz gerekir.

PowerShell komutunu `Get-AzureSubscription`uygulayarak *TenantId'inizi* bulabilirsiniz. Bu komutun yürütülmesi, her abonelik için TenantId'i görüntüler.

*ClusterName,* komut dosyası tarafından oluşturulan Azure AD uygulamalarını önek için kullanılır. Gerçek küme adı tam olarak eşleşmesi gerekmez. Yalnızca Azure AD yapılarının birlikte kullanıldıkları Hizmet Kumaşı kümesiyle eşlemeyi kolaylaştırmak için tasarlanmıştır.

*WebApplicationReplyUrl,* Azure AD'nin oturum açma yı bitirdikten sonra kullanıcılarınıza döndürdettiği varsayılan bitiş noktasıdır. Bu bitiş noktasını kümeniz için Hizmet Kumaş Explorer bitiş noktası olarak ayarlayın. Varolan bir kümeyi temsil etmek için Azure AD uygulamaları oluşturuyorsanız, bu URL'nin varolan kümenizin bitiş noktasıyla eşleştiğinden emin olun. Yeni bir küme için uygulamalar oluşturuyorsanız, kümenizin sahip olacağı bitiş noktasını planlayın ve varolan bir kümenin bitiş noktasını kullanmadığınızdan emin olun. Varsayılan olarak Hizmet Kumaş Explorer bitiş noktası:

https://&lt;&gt;cluster_domain :19080/Explorer

Azure AD kiracısı için yönetim ayrıcalıkları olan bir hesapta oturum açmanız istenir. Oturum açtıktan sonra, komut dosyası Hizmet Kumaşı kümenizi temsil edecek web ve yerel uygulamaları oluşturur. [Azure portalında][azure-portal]kiracının uygulamalarına bakarsanız, iki yeni giriş görmeniz gerekir:

   * *ClusterName*\_Küme
   * *ClusterName*\_İstemci

Komut dosyası, [AAD etkin kümenizi oluştururken](service-fabric-cluster-creation-create-template.md#add-azure-ad-configuration-to-use-azure-ad-for-client-access)Azure Kaynak Yöneticisi şablonu tarafından gerekli olan JSON'u yazdırır, bu nedenle PowerShell penceresini açık tutmak iyi bir fikirdir.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

## <a name="troubleshooting-help-in-setting-up-azure-active-directory"></a>Azure Active Directory kurulumu sorun giderme yardımı
Azure AD'yi ayarlama ve kullanma zor olabilir, bu nedenle sorunu hata ayıklamak için neler yapabileceğinize ilişkin bazı ipuçları nı burada bulabilirsiniz.

### <a name="service-fabric-explorer-prompts-you-to-select-a-certificate"></a>Service Fabric Explorer sertifika seçmeni ister
#### <a name="problem"></a>Sorun
Service Fabric Explorer'da Azure AD'de başarılı bir şekilde oturum açtıktan sonra, tarayıcı ana sayfaya geri döner, ancak bir ileti sertifika seçmenizi ister.

![SFX sertifika iletişim kutusu][sfx-select-certificate-dialog]

#### <a name="reason"></a>Neden
Kullanıcıya Azure AD kümesi uygulamasında bir rol atanmamış. Böylece, Azure AD kimlik doğrulaması Service Fabric kümesinde başarısız olur. Hizmet Kumaş Explorer sertifika kimlik doğrulaması geri düşüyor.

#### <a name="solution"></a>Çözüm
Azure AD'yi ayarlama yönergelerini izleyin ve kullanıcı rolleri atayın. Ayrıca, "Uygulamaya erişmek için gereken kullanıcı atamasını" `SetupApplications.ps1` açmanızı öneririz.

### <a name="connection-with-powershell-fails-with-an-error-the-specified-credentials-are-invalid"></a>PowerShell bağlantısı bir hatayla başarısız olur: "Belirtilen kimlik bilgileri geçersizdir"
#### <a name="problem"></a>Sorun
"AzureActiveDirectory" güvenlik modunu kullanarak kümeye bağlanmak için PowerShell'i kullandığınızda, Azure AD'de başarılı bir şekilde oturum açtıktan sonra bağlantı bir hatayla başarısız olur: "Belirtilen kimlik bilgileri geçersizdir."

#### <a name="solution"></a>Çözüm
Bu çözüm, bir öncekiyle aynıdır.

### <a name="service-fabric-explorer-returns-a-failure-when-you-sign-in-aadsts50011"></a>Hizmet Kumaş Explorer oturum açtığınızda bir hata döndürür: "AADSTS50011"
#### <a name="problem"></a>Sorun
Hizmet Kumaş Explorer'da Azure AD'de oturum açmayı denediğinizde, sayfa bir hata döndürür: &lt;&gt; "AADSTS50011: Yanıt &lt;&gt;adresi url'si uygulama için yapılandırılan yanıt adresleriyle eşleşmiyor: yol gösterici."

![SFX yanıt adresi eşleşmiyor][sfx-reply-address-not-match]

#### <a name="reason"></a>Neden
Service Fabric Explorer'ı temsil eden küme (web) uygulaması Azure AD'ye karşı kimlik doğrulamagirişiminde bulunmayı dener ve isteğin bir parçası olarak yeniden yönlendirme URL'sini sağlar. Ancak URL, Azure REKLAM uygulaması **YANıT URL** listesinde listelenmez.

#### <a name="solution"></a>Çözüm
Kümeniz için Azure AD uygulaması kayıt sayfasında, **Kimlik Doğrulama'yı**seçin ve **Yeniden Yönlendirme URL'leri** bölümünün altında, Listeye Hizmet Kumaşı Gezgini URL'sini ekleyin. Bozuk paranızı kaydedin.

![Web uygulaması yanıt URL'si][web-application-reply-url]

### <a name="connecting-to-the-cluster-using-azure-ad-authentication-via-powershell-gives-an-error-when-you-sign-in-aadsts50011"></a>PowerShell üzerinden Azure AD kimlik doğrulaması kullanarak kümeye bağlanmak oturum açtığınızda bir hata verir: "AADSTS50011"
#### <a name="problem"></a>Sorun
PowerShell üzerinden Azure AD'yi kullanarak bir Hizmet Kumaşı kümesine bağlanmaya çalıştığınızda, oturum açma sayfası bir hata verir: "AADSTS50011: İstekte &lt;&gt;belirtilen yanıt url'si uygulama için yapılandırılan yanıt url'leri ile eşleşmiyor: guid."

#### <a name="reason"></a>Neden
Önceki soruna benzer şekilde PowerShell, Azure REKLAM uygulaması **Yanıt URL'leri** listesinde yer almayan bir yönlendirme URL'si sağlayan Azure AD'ye karşı kimlik doğrulama girişiminde bulunmayı dener.  

#### <a name="solution"></a>Çözüm
Önceki sorundaki yle aynı işlemi kullanın, ancak URL `urn:ietf:wg:oauth:2.0:oob`komut satırı kimlik doğrulaması için özel bir yönlendirme olarak ayarlanmalıdır.

### <a name="connect-the-cluster-by-using-azure-ad-authentication-via-powershell"></a>PowerShell üzerinden Azure AD kimlik doğrulamasını kullanarak kümeyi bağlayın
Service Fabric kümesini bağlamak için aşağıdaki PowerShell komut uyruşu örneğini kullanın:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <endpoint> -KeepAliveIntervalInSec 10 -AzureActiveDirectory -ServerCertThumbprint <thumbprint>
```

Daha fazla bilgi için [connect-ServiceFabricCluster cmdlet'e](https://docs.microsoft.com/powershell/module/servicefabric/connect-servicefabriccluster)bakın.

### <a name="can-i-reuse-the-same-azure-ad-tenant-in-multiple-clusters"></a>Aynı Azure AD kiracısını birden çok kümede yeniden kullanabilir miyim?
Evet. Ancak, Cluster (web) uygulamanıza Service Fabric Explorer URL'sini eklemeyi unutmayın. Aksi takdirde, Service Fabric Explorer çalışmaz.

### <a name="why-do-i-still-need-a-server-certificate-while-azure-ad-is-enabled"></a>Azure AD etkinken neden hala bir sunucu sertifikasına ihtiyacım var?
FabricClient ve FabricGateway karşılıklı kimlik doğrulaması gerçekleştirir. Azure AD kimlik doğrulaması sırasında Azure AD tümleştirmesi sunucuya bir istemci kimliği sağlar ve sunucu sertifikası istemci tarafından sunucunun kimliğini doğrulamak için kullanılır. Service Fabric sertifikaları hakkında daha fazla bilgi için [X.509 sertifikaları ve Servis Kumaşı'na][x509-certificates-and-service-fabric]bakın.

## <a name="next-steps"></a>Sonraki adımlar
Azure Active Directory uygulamalarını ayarladıktan ve kullanıcılar için roller ayarladıktan [sonra, bir kümeyi yapılandırın ve dağıtın.](service-fabric-cluster-creation-via-arm.md)


<!-- Links -->
[azure-CLI]:https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest
[azure-portal]: https://portal.azure.com/
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]:../active-directory/develop/quickstart-create-new-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-setup-aad/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-setup-aad/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-setup-aad/web-application-reply-url.png
