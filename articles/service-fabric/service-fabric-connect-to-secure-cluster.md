---
title: Azure Hizmet Kumaşı kümesine güvenli bir şekilde bağlanma
description: Hizmet Kumaşı kümesine istemci erişiminin nasıl doğrusallatılabildiğini ve istemciler ve küme arasındaki iletişimin nasıl güvenli hale yapılacağını açıklar.
ms.topic: conceptual
ms.date: 01/29/2019
ms.openlocfilehash: a1f4abbabe428a09492efefca4a8da9801b9f68d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79258583"
---
# <a name="connect-to-a-secure-cluster"></a>Güvenli bir kümeye bağlanma

İstemci Bir Hizmet Kumaşı küme düğümüne bağlandığında, istemci kimlik doğrulaması olabilir ve sertifika güvenliği veya Azure Etkin Dizini (AAD) kullanılarak kurulan iletişimi güvenli hale getirebilir. Bu kimlik doğrulama, kümeye ve dağıtılan uygulamalara yalnızca yetkili kullanıcıların erişmesini ve yönetim görevlerini gerçekleştirmesini sağlar.  Küme oluşturulduğunda Sertifika veya AAD güvenliği kümede daha önce etkinleştirilmiş olmalıdır.  Küme güvenliği senaryoları hakkında daha fazla bilgi için [bkz.](service-fabric-cluster-security.md) Sertifikalarla güvenli bir kümeye bağlanıyorsanız, [istemci sertifikasını](service-fabric-connect-to-secure-cluster.md#connectsecureclustersetupclientcert) kümeye bağlanan bilgisayarda ayarlayın. 

<a id="connectsecureclustercli"></a> 

## <a name="connect-to-a-secure-cluster-using-azure-service-fabric-cli-sfctl"></a>Azure Servis Kumaşı CLI (sfctl) kullanarak güvenli bir kümeye bağlanma

Servis Kumaşı CLI 'yi (sfctl) kullanarak güvenli bir kümeye bağlanmanın birkaç farklı yolu vardır. Kimlik doğrulaması için bir istemci sertifikası kullanıyorsanız sertifika bilgilerinin küme düğümlerine dağıtılmış olan bir sertifikayla eşleşmesi gerekir. Sertifikanızda Sertifika Yetkilileri (CA) varsa, güvenilen CA'ları ayrıca belirtmeniz gerekir.

`sfctl cluster select` Komutu kullanarak bir kümeye bağlanabilirsiniz.

İstemci sertifikaları, cert ve anahtar çifti olarak veya tek bir PFX dosyası olarak iki farklı şekilde belirtilebilir. Parola korumalı PEM dosyaları için, otomatik olarak parolayı girmeniz istenir. İstemci sertifikasını PFX dosyası olarak aldıysanız, önce PFX dosyasını aşağıdaki komutu kullanarak PEM dosyasına dönüştürün. 

```shell
openssl pkcs12 -in your-cert-file.pfx -out your-cert-file.pem -nodes -passin pass:your-pfx-password
```

.pfx dosyanız parola korumalı değilse, son parametre için -passin pass kullanın.

İstemci sertifikasını pem dosyası olarak belirtmek için, bağımsız değişkendeki dosya yolunu belirtin. `--pem` Örnek:

```shell
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

Parola korumalı pem dosyaları herhangi bir komutu çalıştırmadan önce parola ister.

Bir sertifika belirtmek için, anahtar `--cert` `--key` çifti her ilgili dosya için dosya yollarını belirtmek için ve bağımsız değişkenleri kullanın.

```shell
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --cert ./client.crt --key ./keyfile.key
```

Bazen sınama veya dev kümeleri güvenli sertifika doğrulama başarısız için kullanılan sertifikalar. Sertifika doğrulamasını atlamak için `--no-verify` seçeneği belirtin. Örnek:

> [!WARNING]
> Üretim Servis `no-verify` Kumaş kümelerine bağlanırken seçeneği kullanmayın.

```shell
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem --no-verify
```

Ayrıca, güvenilen CA sertifikalarının veya tek tek sertifikaların dizinlerine giden yolları belirtebilirsiniz. Bu yolları belirtmek için `--ca` bağımsız değişkeni kullanın. Örnek:

```shell
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem --ca ./trusted_ca
```

Bağlandıktan sonra, kümeyle etkileşimde kalmak için [diğer sfctl komutlarını](service-fabric-cli.md) çalıştırabilmelisin.

<a id="connectsecurecluster"></a>

## <a name="connect-to-a-cluster-using-powershell"></a>PowerShell'i kullanarak kümeye bağlanma
PowerShell üzerinden bir küme üzerinde işlemleri gerçekleştirmeden önce, önce kümeye bir bağlantı kurun. Küme bağlantısı, verilen PowerShell oturumundaki sonraki tüm komutlar için kullanılır.

### <a name="connect-to-an-unsecure-cluster"></a>Güvenli olmayan kümeye bağlanma

Güvenli olmayan bir kümeye bağlanmak için Cluster uç noktası adresini **Connect-ServiceFabricCluster** komutuna sağlayın:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 
```

### <a name="connect-to-a-secure-cluster-using-azure-active-directory"></a>Azure Etkin Dizinini kullanarak güvenli kümeye bağlanma

Küme yöneticisi erişimini yetkilendirmek için Azure Active Directory kullanan güvenli bir kümeye bağlanmak için küme sertifikası parmak izini sağlayın ve *AzureActiveDirectory* bayrağını kullanın.  

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
-ServerCertThumbprint <Server Certificate Thumbprint> `
-AzureActiveDirectory
```

### <a name="connect-to-a-secure-cluster-using-a-client-certificate"></a>İstemci sertifikası nı kullanarak güvenli kümeye bağlanma
Yönetici erişimini yetkilendirmek için istemci sertifikalarını kullanan güvenli bir kümeye bağlanmak için aşağıdaki PowerShell komutunu çalıştırın. 

#### <a name="connect-using-certificate-common-name"></a>Sertifika ortak adını kullanarak bağlanma
Küme sertifikası ortak adını ve küme yönetimi için izin verilmiş istemci sertifikasının ortak adını sağlayın. Sertifika ayrıntılarının küme düğümlerinde bir sertifikayla eşleşmesi gerekir.

```powershell
Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCommonName <certificate common name>  `
    -FindType FindBySubjectName `
    -FindValue <certificate common name> `
    -StoreLocation CurrentUser `
    -StoreName My 
```
*ServerCommonName,* küme düğümlerine yüklenen sunucu sertifikasının ortak adıdır. *FindValue,* yönetici istemci sertifikasının ortak adıdır. Parametreler doldurulduğunda, komut aşağıdaki örnek gibi görünür:
```powershell
$ClusterName= "sf-commonnametest-scus.southcentralus.cloudapp.azure.com:19000"
$certCN = "sfrpe2eetest.southcentralus.cloudapp.azure.com"

Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCommonName $certCN  `
    -FindType FindBySubjectName `
    -FindValue $certCN `
    -StoreLocation CurrentUser `
    -StoreName My 
```

#### <a name="connect-using-certificate-thumbprint"></a>Sertifika parmak izini kullanarak bağlanma
Küme yönetimi için izin verilmiş olan istemci sertifikasının küme sertifikası parmak izinini ve parmak izinini sağlayın. Sertifika ayrıntılarının küme düğümlerinde bir sertifikayla eşleşmesi gerekir.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `  
          -KeepAliveIntervalInSec 10 `  
          -X509Credential -ServerCertThumbprint <Certificate Thumbprint> `  
          -FindType FindByThumbprint -FindValue <Certificate Thumbprint> `  
          -StoreLocation CurrentUser -StoreName My
```

*ServerCertThumbprint,* küme düğümlerine yüklenen sunucu sertifikasının parmak izidir. *FindValue,* yönetici istemci sertifikasının parmak izidir.  Parametreler doldurulduğunda, komut aşağıdaki örnek gibi görünür:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint clustername.westus.cloudapp.azure.com:19000 `  
          -KeepAliveIntervalInSec 10 `  
          -X509Credential -ServerCertThumbprint A8136758F4AB8962AF2BF3F27921BE1DF67F4326 `  
          -FindType FindByThumbprint -FindValue 71DE04467C9ED0544D021098BCD44C71E183414E `  
          -StoreLocation CurrentUser -StoreName My 
```

### <a name="connect-to-a-secure-cluster-using-windows-active-directory"></a>Windows Active Directory'yi kullanarak güvenli kümeye bağlanma
Bağımsız kümeniz AD güvenliğini kullanarak dağıtılırsa, "WindowsCredential" anahtarını ekleyerek kümeye bağlanın.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -WindowsCredential
```

<a id="connectsecureclusterfabricclient"></a>

## <a name="connect-to-a-cluster-using-the-fabricclient-apis"></a>FabricClient API'lerini kullanarak kümeye bağlanma
Service Fabric SDK küme yönetimi için [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) sınıfını sağlar. FabricClient API'lerini kullanmak için Microsoft.ServiceFabric NuGet paketini alın.

### <a name="connect-to-an-unsecure-cluster"></a>Güvenli olmayan kümeye bağlanma

Uzak güvenli olmayan kümeye bağlanmak için bir FabricClient örneği oluşturun ve küme adresini sağlayın:

```csharp
FabricClient fabricClient = new FabricClient("clustername.westus.cloudapp.azure.com:19000");
```

Bir küme nin içinden çalışan kod için, örneğin, Güvenilir Hizmet'te, küme adresini *belirtmeden* bir FabricClient oluşturun. FabricClient, kodun şu anda üzerinde çalışmakta olduğu düğümdeki yerel yönetim ağ geçidine bağlanır ve ek bir ağ atlamadan kaçınır.

```csharp
FabricClient fabricClient = new FabricClient();
```

### <a name="connect-to-a-secure-cluster-using-a-client-certificate"></a>İstemci sertifikası nı kullanarak güvenli kümeye bağlanma

Kümedeki düğümlerin, San'daki ortak adı veya DNS adı [FabricClient'da](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient)ayarlanan [RemoteCommonNames özelliğinde](https://docs.microsoft.com/dotnet/api/system.fabric.x509credentials) görünen geçerli sertifikaları olmalıdır. Bu işlemi takiben istemci ve küme düğümleri arasında karşılıklı kimlik doğrulaması sağlar.

```csharp
using System.Fabric;
using System.Security.Cryptography.X509Certificates;

string clientCertThumb = "71DE04467C9ED0544D021098BCD44C71E183414E";
string serverCertThumb = "A8136758F4AB8962AF2BF3F27921BE1DF67F4326";
string CommonName = "www.clustername.westus.azure.com";
string connection = "clustername.westus.cloudapp.azure.com:19000";

var xc = GetCredentials(clientCertThumb, serverCertThumb, CommonName);
var fc = new FabricClient(xc, connection);

try
{
    var ret = fc.ClusterManager.GetClusterManifestAsync().Result;
    Console.WriteLine(ret.ToString());
}
catch (Exception e)
{
    Console.WriteLine("Connect failed: {0}", e.Message);
}

static X509Credentials GetCredentials(string clientCertThumb, string serverCertThumb, string name)
{
    X509Credentials xc = new X509Credentials();
    xc.StoreLocation = StoreLocation.CurrentUser;
    xc.StoreName = "My";
    xc.FindType = X509FindType.FindByThumbprint;
    xc.FindValue = clientCertThumb;
    xc.RemoteCommonNames.Add(name);
    xc.RemoteCertThumbprints.Add(serverCertThumb);
    xc.ProtectionLevel = ProtectionLevel.EncryptAndSign;
    return xc;
}
```

### <a name="connect-to-a-secure-cluster-interactively-using-azure-active-directory"></a>Azure Active Directory'yi kullanarak etkileşimli olarak güvenli bir kümeye bağlanma

Aşağıdaki örnekte, istemci kimliği için Azure Etkin Dizini ve sunucu kimliği için sunucu sertifikası kullanmaktadır.

Kümeye bağlandıktan sonra etkileşimli oturum açmak için otomatik olarak bir iletişim penceresi açılır.

```csharp
string serverCertThumb = "A8136758F4AB8962AF2BF3F27921BE1DF67F4326";
string connection = "clustername.westus.cloudapp.azure.com:19000";

var claimsCredentials = new ClaimsCredentials();
claimsCredentials.ServerThumbprints.Add(serverCertThumb);

var fc = new FabricClient(claimsCredentials, connection);

try
{
    var ret = fc.ClusterManager.GetClusterManifestAsync().Result;
    Console.WriteLine(ret.ToString());
}
catch (Exception e)
{
    Console.WriteLine("Connect failed: {0}", e.Message);
}
```

### <a name="connect-to-a-secure-cluster-non-interactively-using-azure-active-directory"></a>Azure Etkin Dizinini kullanarak etkileşimli olmayan güvenli bir kümeye bağlanma

Aşağıdaki örnek Microsoft.IdentityModel.Clients.ActiveDirectory, Sürüm: 2.19.208020213 dayanır.

AAD belirteç edinimi hakkında daha fazla bilgi için [Microsoft.IdentityModel.Clients.ActiveDirectory](https://msdn.microsoft.com/library/microsoft.identitymodel.clients.activedirectory.aspx)' e bakın.

```csharp
string tenantId = "C15CFCEA-02C1-40DC-8466-FBD0EE0B05D2";
string clientApplicationId = "118473C2-7619-46E3-A8E4-6DA8D5F56E12";
string webApplicationId = "53E6948C-0897-4DA6-B26A-EE2A38A690B4";

string token = GetAccessToken(
    tenantId,
    webApplicationId,
    clientApplicationId,
    "urn:ietf:wg:oauth:2.0:oob");

string serverCertThumb = "A8136758F4AB8962AF2BF3F27921BE1DF67F4326";
string connection = "clustername.westus.cloudapp.azure.com:19000";

var claimsCredentials = new ClaimsCredentials();
claimsCredentials.ServerThumbprints.Add(serverCertThumb);
claimsCredentials.LocalClaims = token;

var fc = new FabricClient(claimsCredentials, connection);

try
{
    var ret = fc.ClusterManager.GetClusterManifestAsync().Result;
    Console.WriteLine(ret.ToString());
}
catch (Exception e)
{
    Console.WriteLine("Connect failed: {0}", e.Message);
}

...

static string GetAccessToken(
    string tenantId,
    string resource,
    string clientId,
    string redirectUri)
{
    string authorityFormat = @"https://login.microsoftonline.com/{0}";
    string authority = string.Format(CultureInfo.InvariantCulture, authorityFormat, tenantId);
    var authContext = new AuthenticationContext(authority);

    var authResult = authContext.AcquireToken(
        resource,
        clientId,
        new UserCredential("TestAdmin@clustenametenant.onmicrosoft.com", "TestPassword"));
    return authResult.AccessToken;
}

```

### <a name="connect-to-a-secure-cluster-without-prior-metadata-knowledge-using-azure-active-directory"></a>Azure Active Directory'yi kullanarak önceden meta veri bilgisi olmadan güvenli bir kümeye bağlanma

Aşağıdaki örnek, etkileşimli olmayan belirteç edinimi kullanır, ancak aynı yaklaşım özel bir etkileşimli belirteç edinme deneyimi oluşturmak için kullanılabilir. Belirteç edinimi için gereken Azure Etkin Dizin meta verileri küme yapılandırmasından okunur.

```csharp
string serverCertThumb = "A8136758F4AB8962AF2BF3F27921BE1DF67F4326";
string connection = "clustername.westus.cloudapp.azure.com:19000";

var claimsCredentials = new ClaimsCredentials();
claimsCredentials.ServerThumbprints.Add(serverCertThumb);

var fc = new FabricClient(claimsCredentials, connection);

fc.ClaimsRetrieval += (o, e) =>
{
    return GetAccessToken(e.AzureActiveDirectoryMetadata);
};

try
{
    var ret = fc.ClusterManager.GetClusterManifestAsync().Result;
    Console.WriteLine(ret.ToString());
}
catch (Exception e)
{
    Console.WriteLine("Connect failed: {0}", e.Message);
}

...

static string GetAccessToken(AzureActiveDirectoryMetadata aad)
{
    var authContext = new AuthenticationContext(aad.Authority);

    var authResult = authContext.AcquireToken(
        aad.ClusterApplication,
        aad.ClientApplication,
        new UserCredential("TestAdmin@clustenametenant.onmicrosoft.com", "TestPassword"));
    return authResult.AccessToken;
}

```

<a id="connectsecureclustersfx"></a>

## <a name="connect-to-a-secure-cluster-using-service-fabric-explorer"></a>Service Fabric Explorer'ı kullanarak güvenli kümeye bağlanma
Belirli bir küme için [Service Fabric Explorer'a](service-fabric-visualizing-your-cluster.md) ulaşmak için tarayıcınızı şu noktaya yönlendirin:

`http://<your-cluster-endpoint>:19080/Explorer`

Tam URL, Azure portalının küme temel bölmesinde de kullanılabilir.

Bir tarayıcı kullanarak Windows veya OS X'te güvenli bir kümeye bağlanmak için istemci sertifikasını içe aktarabilirsiniz ve tarayıcı, sertifikanın kümeye bağlanmak için kullanmasını ister.  Linux makinelerinde, sertifikanın gelişmiş tarayıcı ayarları kullanılarak içe aktarılması (her tarayıcının farklı mekanizmaları vardır) ve diskteki sertifika konumuna işaret etmesi gerekir. Daha fazla bilgi için [istemci sertifikası ayarlama'yı](#connectsecureclustersetupclientcert) okuyun.

### <a name="connect-to-a-secure-cluster-using-azure-active-directory"></a>Azure Etkin Dizinini kullanarak güvenli kümeye bağlanma

AAD ile güvenli bir kümeye bağlanmak için tarayıcınızı şu şekilde işaret edin:

`https://<your-cluster-endpoint>:19080/Explorer`

Otomatik olarak AAD ile oturum açmanız istenir.

### <a name="connect-to-a-secure-cluster-using-a-client-certificate"></a>İstemci sertifikası nı kullanarak güvenli kümeye bağlanma

Sertifikalarla güvenli bir kümeye bağlanmak için tarayıcınızı şu şekilde işaret edin:

`https://<your-cluster-endpoint>:19080/Explorer`

Otomatik olarak bir istemci sertifikası seçmeniz istenir.

<a id="connectsecureclustersetupclientcert"></a>

## <a name="set-up-a-client-certificate-on-the-remote-computer"></a>Uzak bilgisayarda istemci sertifikası ayarlama

Kümenin güvenliğini sağlamak için en az iki sertifika, biri küme ve sunucu sertifikası için, diğeri de istemci erişimi için kullanılmalıdır.  Ayrıca ek ikincil sertifikalar ve istemci erişim sertifikaları kullanmanızı öneririz.  Sertifika güvenliğini kullanarak istemci ve küme düğümü arasındaki iletişimi güvence altına almak için öncelikle istemci sertifikasını almanız ve yüklemeniz gerekir. Sertifika, yerel bilgisayarın veya geçerli kullanıcının Kişisel (My) deposuna yüklenebilir.  İstemcinin kümenin kimliğini doğrulayabilmesi için sunucu sertifikasının parmak izine de ihtiyacınız vardır.

* Windows üzerinde: PFX dosyasına çift tıklayın ve `Certificates - Current User\Personal\Certificates` dizinindeki kişisel deponuza sertifikayı yüklemek için istemleri izleyin. Alternatif olarak, PowerShell komutunu kullanabilirsiniz:

    ```powershell
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
            -FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
            -Password (ConvertTo-SecureString -String test -AsPlainText -Force)
    ```

    Kendi imzalı bir sertifikaysa, güvenli bir kümeye bağlanmak için bu sertifikayı kullanabilmeniz için önce sertifikayı makinenizin "güvenilir kişiler" deposuna aktarmanız gerekir.

    ```powershell
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople `
    -FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
    -Password (ConvertTo-SecureString -String test -AsPlainText -Force)
    ```

* Mac üzerinde: PFX dosyasına çift tıklayın ve Anahtarlığınıza sertifikayı yüklemek için istemleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Hizmet Kumaş Kümesi yükseltme süreci ve sizden beklentiler](service-fabric-cluster-upgrade.md)
* [Service Fabric uygulamalarınızı Visual Studio'da yönetme](service-fabric-manage-application-in-visual-studio.md)
* [Hizmet Kumaş Sağlığı modeli giriş](service-fabric-health-introduction.md)
* [Uygulama Güvenliği ve RunA'lar](service-fabric-application-runas-security.md)
* [Service Fabric CLI ile çalışmaya başlama](service-fabric-cli.md)
