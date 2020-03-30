---
title: Sertifikaları kullanarak Windows'da küme güvenliğini sağlama
description: Azure Hizmet Kumaşı tek başına veya şirket içi kümede ve istemciler ve küme arasında güvenli iletişim.
author: dkkapur
ms.topic: conceptual
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: 5a18f957dfb7143f403d5ac30ea184023021f12c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75613933"
---
# <a name="secure-a-standalone-cluster-on-windows-by-using-x509-certificates"></a>X.509 sertifikalarını kullanarak Windows'da bağımsız bir kümeyi güvenli hale
Bu makalede, bağımsız Windows kümenizin çeşitli düğümleri arasındaki iletişimin nasıl güvenli hale ne kadar güvenli olduğu açıklanmaktadır. Ayrıca, X.509 sertifikalarını kullanarak bu kümeye bağlanan istemcilerin kimliğini niçin doğrulayabilirsiniz açıklar. Kimlik doğrulama, kümeye ve dağıtılan uygulamalara yalnızca yetkili kullanıcıların erişmesini ve yönetim görevlerini gerçekleştirmesini sağlar. Küme oluşturulduğunda kümede sertifika güvenliği etkinleştirilmelidir.  

Düğümden düğüme güvenlik, istemciden düğüme güvenlik ve rol tabanlı erişim denetimi gibi küme güvenliği hakkında daha fazla bilgi için küme [güvenlik senaryolarına](service-fabric-cluster-security.md)bakın.

## <a name="which-certificates-do-you-need"></a>Hangi sertifikalara ihtiyacınız var?
Başlangıç olarak, [Windows Server için Hizmet Dokusu paketini](service-fabric-cluster-creation-for-windows-server.md#download-the-service-fabric-for-windows-server-package) kümenizdeki düğümlerden birine indirin. İndirilen pakette ClusterConfig.X509.MultiMachine.json dosyasını bulabilirsiniz. Dosyayı açın ve özellikler bölümü altındaki güvenlik bölümünü gözden geçirin:

```JSON
"security": {
    "metadata": "The Credential type X509 indicates this cluster is secured by using X509 certificates. The thumbprint format is d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
    "ClusterCredentialType": "X509",
    "ServerCredentialType": "X509",
    "CertificateInformation": {
        "ClusterCertificate": {
            "Thumbprint": "[Thumbprint]",
            "ThumbprintSecondary": "[Thumbprint]",
            "X509StoreName": "My"
        },        
        "ClusterCertificateCommonNames": {
            "CommonNames": [
            {
                "CertificateCommonName": "[CertificateCommonName]",
                "CertificateIssuerThumbprint": "[Thumbprint1,Thumbprint2,Thumbprint3,...]"
            }
            ],
            "X509StoreName": "My"
        },
        "ClusterCertificateIssuerStores": [
            {
                "IssuerCommonName": "[IssuerCommonName]",
                "X509StoreNames" : "Root"
            }
        ],
        "ServerCertificate": {
            "Thumbprint": "[Thumbprint]",
            "ThumbprintSecondary": "[Thumbprint]",
            "X509StoreName": "My"
        },
        "ServerCertificateCommonNames": {
            "CommonNames": [
            {
                "CertificateCommonName": "[CertificateCommonName]",
                "CertificateIssuerThumbprint": "[Thumbprint1,Thumbprint2,Thumbprint3,...]"
            }
            ],
            "X509StoreName": "My"
        },
        "ServerCertificateIssuerStores": [
            {
                "IssuerCommonName": "[IssuerCommonName]",
                "X509StoreNames" : "Root"
            }
        ],
        "ClientCertificateThumbprints": [
            {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": false
            },
            {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": true
            }
        ],
        "ClientCertificateCommonNames": [
            {
                "CertificateCommonName": "[CertificateCommonName]",
                "CertificateIssuerThumbprint": "[Thumbprint1,Thumbprint2,Thumbprint3,...]",
                "IsAdmin": true
            }
        ],
        "ClientCertificateIssuerStores": [
            {
                "IssuerCommonName": "[IssuerCommonName]",
                "X509StoreNames": "Root"
            }
        ]
        "ReverseProxyCertificate": {
            "Thumbprint": "[Thumbprint]",
            "ThumbprintSecondary": "[Thumbprint]",
            "X509StoreName": "My"
        },
        "ReverseProxyCertificateCommonNames": {
            "CommonNames": [
                {
                "CertificateCommonName": "[CertificateCommonName]"
                }
            ],
            "X509StoreName": "My"
        }
    }
},
```

Bu bölümde, bağımsız Windows kümenizi güvence altına almak için gereken sertifikalar açıklanmaktadır. Bir küme sertifikası belirtirseniz, ClusterCredentialType değerini _X509_olarak ayarlayın. Dış bağlantılar için bir sunucu sertifikası belirtirseniz, ServerCredentialType'ı _X509_olarak ayarlayın. Zorunlu olmasa da, düzgün bir şekilde güvenli bir küme için bu sertifikaların her ikisine de sahip olduğunuzu öneririz. Bu değerleri *X509*olarak ayarlarsanız, ilgili sertifikaları belirtmeniz gerekir veya Hizmet Kumaşı bir özel durum oluşturur. Bazı senaryolarda, yalnızca _ClientCertificateThumbprints_ veya _ReverseProxyCertificate_belirtmek isteyebilirsiniz. Bu senaryolarda _ClusterCredentialType_ veya _ServerCredentialType'ı_ _X509_olarak ayarlamanız gerekmez.


> [!NOTE]
> [Parmak izi,](https://en.wikipedia.org/wiki/Public_key_fingerprint) sertifikanın birincil kimliğidir. Oluşturduğunuz sertifikaların parmak izini bulmak için [bkz.](https://msdn.microsoft.com/library/ms734695.aspx)
> 
> 

Aşağıdaki tabloda küme kurulumunuzda gereksinim duyduğunuz sertifikalar listelenir:

| **SertifikaBilgi ayarı** | **Açıklama** |
| --- | --- |
| Kümesertifikası |Test ortamı için önerilir. Bu sertifika, kümedeki düğümler arasındaki iletişimi güvence altına almak için gereklidir. Yükseltme için birincil ve ikincil olmak üzere iki farklı sertifika kullanabilirsiniz. Thumbprint bölümündeki birincil sertifikanın ve ThumbprintSecondary değişkenlerinde ikincil sertifikanın parmak izini ayarlayın. |
| ClusterCertificateCommonNames |Üretim ortamı için önerilir. Bu sertifika, kümedeki düğümler arasındaki iletişimi güvence altına almak için gereklidir. Bir veya iki küme sertifikası ortak adlarını kullanabilirsiniz. SertifikaVerenThumbprint, bu sertifikayı verenin parmak izine karşılık gelir. Aynı ortak ada sahip birden fazla sertifika kullanılırsa, birden çok veren parmak izi belirtebilirsiniz.|
| ClusterCertificateIssuerMağazaları |Üretim ortamı için önerilir. Bu sertifika küme sertifikasının verenine karşılık gelir. ClusterCertificateCommonNames altında veren intibakını belirtmek yerine, bu bölümün altında veren kuruluş ortak adını ve ilgili mağaza adını sağlayabilirsiniz.  Bu, küme veren sertifikaların devrini kolaylaştırır. Birden fazla küme sertifikası kullanılıyorsa, birden çok veren belirtilebilir. Boş bir VerenCommonName, X509StoreNames altında belirtilen ilgili mağazalardaki tüm sertifikaları beyaz listeler.|
| ServerCertificate |Test ortamı için önerilir. Bu sertifika, bu kümeye bağlanmaya çalıştığında istemciye sunulur. Kolaylık sağlamak için ClusterCertificate ve ServerCertificate için aynı sertifikayı kullanmayı seçebilirsiniz. Yükseltme için birincil ve ikincil olmak üzere iki farklı sunucu sertifikası kullanabilirsiniz. Thumbprint bölümündeki birincil sertifikanın ve ThumbprintSecondary değişkenlerinde ikincil sertifikanın parmak izini ayarlayın. |
| ServerCertificateCommonNames |Üretim ortamı için önerilir. Bu sertifika, bu kümeye bağlanmaya çalıştığında istemciye sunulur. SertifikaVerenThumbprint, bu sertifikayı verenin parmak izine karşılık gelir. Aynı ortak ada sahip birden fazla sertifika kullanılırsa, birden çok veren parmak izi belirtebilirsiniz. Kolaylık sağlamak için ClusterCertificateCommonNames ve ServerCertificateCommonNames için aynı sertifikayı kullanmayı seçebilirsiniz. Bir veya iki sunucu sertifikası ortak adlarını kullanabilirsiniz. |
| ServerCertificateIssuerMağazalar |Üretim ortamı için önerilir. Bu sertifika, sunucu sertifikasının verenine karşılık gelir. ServerCertificateCommonNames altında verenin parmak izini belirtmek yerine, bu bölümün altında verenkuruluş ortak adını ve ilgili mağaza adını sağlayabilirsiniz.  Bu, sunucu veren sertifikaların devrini kolaylaştırır. Birden fazla sunucu sertifikası kullanılıyorsa, birden çok veren belirtilebilir. Boş bir VerenCommonName, X509StoreNames altında belirtilen ilgili mağazalardaki tüm sertifikaları beyaz listeler.|
| ClientCertificateThumbprints |Bu sertifika kümesini kimlik doğrulaması yapılan istemcilere yükleyin. Kümeye erişime izin vermek istediğiniz makinelere yüklü birkaç farklı istemci sertifikası na sahip olabilirsiniz. SertifikaThumbprint değişkeninde her sertifikanın parmak izini ayarlayın. IsAdmin'i *doğru*ayarlarsanız, üzerinde bu sertifika yüklü olan istemci kümede yönetici yönetim etkinlikleri yapabilir. IsAdmin *yanlışsa,* bu sertifikaya sahip istemci yalnızca yalnızca kullanıcı erişim hakları için izin verilen eylemleri gerçekleştirebilir, genellikle salt okunur. Roller hakkında daha fazla bilgi için [Bkz. Rol Tabanlı Erişim Denetimi (RBAC)](service-fabric-cluster-security.md#role-based-access-control-rbac). |
| Müşteri SertifikasıOrtak Adları |CertificateCommonName için ilk istemci sertifikasının ortak adını ayarlayın. SertifikaIssuerThumbprint bu sertifikanın veren için parmak izidir. Ortak adlar ve veren hakkında daha fazla bilgi edinmek için [bkz.](https://msdn.microsoft.com/library/ms731899.aspx) |
| ClientCertificateIssuerStores |Üretim ortamı için önerilir. Bu sertifika, istemci sertifikasının verenine (hem yönetici hem de yönetici olmayan roller) karşılık gelir. ClientCertificateCommonNames altında verenin parmak izini belirtmek yerine, bu bölümün altında veren kuruluş ortak adını ve ilgili mağaza adını sağlayabilirsiniz.  Bu, istemci veren sertifikaların devrini kolaylaştırır. Birden fazla istemci sertifikası kullanılıyorsa, birden çok veren belirtilebilir. Boş bir VerenCommonName, X509StoreNames altında belirtilen ilgili mağazalardaki tüm sertifikaları beyaz listeler.|
| Ters Proxy Sertifikası |Test ortamı için önerilir. Ters [proxy'nizi](service-fabric-reverseproxy.md)güvence altına almak istiyorsanız bu isteğe bağlı sertifika belirtilebilir. Bu sertifikayı kullanıyorsanız tersProxyEndpointPort'un düğümtipolarak ayarlandığınızdan emin olun. |
| Ters ProxyCertificateCommonNames |Üretim ortamı için önerilir. Ters [proxy'nizi](service-fabric-reverseproxy.md)güvence altına almak istiyorsanız bu isteğe bağlı sertifika belirtilebilir. Bu sertifikayı kullanıyorsanız tersProxyEndpointPort'un düğümtipolarak ayarlandığınızdan emin olun. |

Burada küme, sunucu ve istemci sertifikaları sağlanmıştır örnek küme yapılandırmasıdır. Küme/sunucu/tersProxy sertifikaları için, parmak izi ve ortak ad aynı sertifika türü için birlikte yapılandırılamaz.

 ```JSON
 {
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "10-2017",
    "nodes": [{
        "nodeName": "vm0",
        "metadata": "Replace the localhost below with valid IP address or FQDN",
        "iPAddress": "10.7.0.5",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "metadata": "Replace the localhost with valid IP address or FQDN",
        "iPAddress": "10.7.0.4",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "10.7.0.6",
        "metadata": "Replace the localhost with valid IP address or FQDN",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],
    "properties": {
        "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
        },
        "security": {
            "metadata": "The Credential type X509 indicates this cluster is secured by using X509 certificates. The thumbprint format is d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
            "ClusterCredentialType": "X509",
            "ServerCredentialType": "X509",
            "CertificateInformation": {
                "ClusterCertificateCommonNames": {
                  "CommonNames": [
                    {
                      "CertificateCommonName": "myClusterCertCommonName"
                    }
                  ],
                  "X509StoreName": "My"
                },
                "ClusterCertificateIssuerStores": [
                    {
                        "IssuerCommonName": "ClusterIssuer1",
                        "X509StoreNames" : "Root"
                    },
                    {
                        "IssuerCommonName": "ClusterIssuer2",
                        "X509StoreNames" : "Root"
                    }
                ],
                "ServerCertificateCommonNames": {
                  "CommonNames": [
                    {
                      "CertificateCommonName": "myServerCertCommonName",
                      "CertificateIssuerThumbprint": "7c fc 91 97 13 16 8d ff a8 ee 71 2b a2 f4 62 62 00 03 49 0d"
                    }
                  ],
                  "X509StoreName": "My"
                },
                "ClientCertificateThumbprints": [{
                    "CertificateThumbprint": "c4 c18 8e aa a8 58 77 98 65 f8 61 4a 0d da 4c 13 c5 a1 37 6e",
                    "IsAdmin": false
                }, {
                    "CertificateThumbprint": "71 de 04 46 7c 9e d0 54 4d 02 10 98 bc d4 4c 71 e1 83 41 4e",
                    "IsAdmin": true
                }]
            }
        },
        "reliabilityLevel": "Bronze",
        "nodeTypes": [{
            "name": "NodeType0",
            "clientConnectionEndpointPort": "19000",
            "clusterConnectionEndpointPort": "19001",
            "leaseDriverEndpointPort": "19002",
            "serviceConnectionEndpointPort": "19003",
            "httpGatewayEndpointPort": "19080",
            "applicationPorts": {
                "startPort": "20001",
                "endPort": "20031"
            },
            "ephemeralPorts": {
                "startPort": "20032",
                "endPort": "20062"
            },
            "isPrimary": true
        }
         ],
        "fabricSettings": [{
            "name": "Setup",
            "parameters": [{
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
            }, {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
            }]
        }]
    }
}
 ```

## <a name="certificate-rollover"></a>Sertifika devri
Küçük parmak izi yerine bir sertifika ortak adı kullandığınızda, sertifika devri küme yapılandırma yükseltmesi gerektirmez. İhraççı parmak izi yükseltmeleri için, yeni parmak izi listesinin eski listeyle kesiştiğinden emin olun. Önce yeni veren parmak izleri ile bir config yükseltme yapmak ve sonra mağazaya yeni sertifikaları (hem küme/ sunucu sertifikası ve veren sertifikaları) yüklemeniz gerekir. Yeni veren sertifikasını yükledikten sonra eski veren sertifikasını sertifika deposunda en az iki saat saklayın.
İhraççı mağazaları kullanıyorsanız, ihraççı sertifikası rollover için config yükseltmesi yapılması gerekir. İlgili sertifika deposuna ikinci bir son kullanma tarihi olan yeni veren sertifikasını yükleyin ve birkaç saat sonra eski veren sertifikasını kaldırın.

## <a name="acquire-the-x509-certificates"></a>X.509 sertifikalarını edinin
Küme içinde iletişimi güvence altına almak için öncelikle küme düğümleriniz için X.509 sertifikaları almanız gerekir. Ayrıca, bu kümeye olan bağlantıyı yetkili makinelerle/kullanıcılarla sınırlamak için istemci makineler için sertifika almanız ve yüklemeniz gerekir.

Üretim iş yüklerini çalıştıran kümeler için, kümeyi korumak için [sertifika yetkilisi (CA)](https://en.wikipedia.org/wiki/Certificate_authority)imzalı X.509 sertifikası kullanın. Bu sertifikaların nasıl alınabildiğini hakkında daha fazla bilgi [için](https://msdn.microsoft.com/library/aa702761.aspx)bkz.

Test amacıyla kullandığınız kümeler için kendi imzalı bir sertifika kullanmayı seçebilirsiniz.

## <a name="optional-create-a-self-signed-certificate"></a>İsteğe bağlı: Kendi imzalı sertifika oluşturma
Doğru şekilde güvenli bir şekilde güvenli bir kendi imzalı sertifika oluşturmanın bir yolu, C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\Secure dizinindeki Service Fabric SDK klasöründeki CertSetup.ps1 komut dosyasını kullanmaktır. Sertifikanın varsayılan adını değiştirmek için bu dosyayı edin. (CN=ServiceFabricDevClusterCert değerini arayın.) Bu komut `.\CertSetup.ps1 -Install`dosyalarını .

Şimdi sertifikayı korumalı parolalı bir .pfx dosyasına dışa aktarın. İlk olarak, sertifikanın parmak izini alın. 
1. **Başlat** menüsünden **bilgisayar sertifikalarını yönet'i**çalıştırın. 

2. **Yerel Bilgisayar\Kişisel** klasörüne gidin ve oluşturduğunuz sertifikayı bulun. 

3. Açmak için sertifikayı çift tıklatın, **Ayrıntılar** sekmesini seçin ve **Thumbprint** alanına gidin. 

4. Boşlukları kaldırın ve parmak izi değerini aşağıdaki PowerShell komutuna kopyalayın. 

5. `String` Değeri korumak için uygun güvenli bir parolayla değiştirin ve PowerShell'de aşağıdakileri çalıştırın:

   ```powershell   
   $pswd = ConvertTo-SecureString -String "1234" -Force –AsPlainText
   Get-ChildItem -Path cert:\localMachine\my\<Thumbprint> | Export-PfxCertificate -FilePath C:\mypfx.pfx -Password $pswd
   ```

6. Makineye yüklenen bir sertifikanın ayrıntılarını görmek için aşağıdaki PowerShell komutunu çalıştırın:

   ```powershell
   $cert = Get-Item Cert:\LocalMachine\My\<Thumbprint>
   Write-Host $cert.ToString($true)
   ```

Alternatif olarak, Bir Azure aboneliğiniz varsa, [Azure Kaynak Yöneticisi'ni kullanarak Hizmet Dokusu Oluştur kümesindeki](service-fabric-cluster-creation-via-arm.md)adımları izleyin.

## <a name="install-the-certificates"></a>Sertifikaları yükleyin
Sertifikalarınız olduktan sonra, bunları küme düğümlerine yükleyebilirsiniz. Düğümlerinizin en son Windows PowerShell 3.x'in yüklü olması gerekir. Küme ve sunucu sertifikaları ve ikincil sertifikalar için her düğümde bu adımları yineleyin.

1. .pfx dosyasını veya dosyalarını düğüme kopyalayın.

2. Yönetici olarak bir PowerShell penceresini açın ve aşağıdaki komutları girin. *$pswd* bu sertifikayı oluşturmak için kullandığınız parolayla değiştirin. *$PfxFilePath* bu düğüme kopyalanan .pfx'in tam yolu ile değiştirin.
   
    ```powershell
    $pswd = "1234"
    $PfxFilePath ="C:\mypfx.pfx"
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\LocalMachine\My -FilePath $PfxFilePath -Password (ConvertTo-SecureString -String $pswd -AsPlainText -Force)
    ```
3. Artık bu sertifikadaki erişim denetimini, Ağ Hizmeti hesabı altında çalışan Hizmet Dokusu işleminin aşağıdaki komut dosyasını çalıştırarak kullanabilmesi için ayarlayın. Hizmet hesabı için sertifikanın ve **NETWORK SERVICE'in** parmak izini sağlayın. **Sertifikadaki** > ALA'ların bilgisayar**sertifikalarını** Başlat'ta sertifikayı açarak ve **Tüm Görevleri** > **Yönet Özel Anahtarları'na**bakarak doğru olup olmadığını kontrol edebilirsiniz.
   
    ```powershell
    param
    (
    [Parameter(Position=1, Mandatory=$true)]
    [ValidateNotNullOrEmpty()]
    [string]$pfxThumbPrint,
   
    [Parameter(Position=2, Mandatory=$true)]
    [ValidateNotNullOrEmpty()]
    [string]$serviceAccount
    )
   
    $cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object -FilterScript { $PSItem.ThumbPrint -eq $pfxThumbPrint; }
   
    # Specify the user, the permissions, and the permission type
    $permission = "$($serviceAccount)","FullControl","Allow"
    $accessRule = New-Object -TypeName System.Security.AccessControl.FileSystemAccessRule -ArgumentList $permission
   
    # Location of the machine-related keys
    $keyPath = Join-Path -Path $env:ProgramData -ChildPath "\Microsoft\Crypto\RSA\MachineKeys"
    $keyName = $cert.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName
    $keyFullPath = Join-Path -Path $keyPath -ChildPath $keyName
   
    # Get the current ACL of the private key
    $acl = (Get-Item $keyFullPath).GetAccessControl('Access')
   
    # Add the new ACE to the ACL of the private key
    $acl.SetAccessRule($accessRule)
   
    # Write back the new ACL
    Set-Acl -Path $keyFullPath -AclObject $acl -ErrorAction Stop
   
    # Observe the access rights currently assigned to this certificate
    get-acl $keyFullPath| fl
    ```
4. Her sunucu sertifikası için önceki adımları yineleyin. Kümeye erişime izin vermek istediğiniz makinelere istemci sertifikalarını yüklemek için de bu adımları kullanabilirsiniz.

## <a name="create-the-secure-cluster"></a>Güvenli küme oluşturma
ClusterConfig.X509.MultiMachine.json dosyasının güvenlik bölümünü yapılandırdıktan sonra düğümleri yapılandırmak ve bağımsız küme oluşturmak için [küme](service-fabric-cluster-creation-for-windows-server.md#create-the-cluster) oluştur bölümüne geçebilirsiniz. Küme oluştururken ClusterConfig.X509.MultiMachine.json dosyasını kullanmayı unutmayın. Örneğin, komutunuzun aşağıdaki gibi görünebilir:

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

Güvenli bağımsız Windows kümesini başarıyla çalıştırdıktan ve ona bağlanmak için kimlik doğrulaması yapılan istemcileri ayarladıktan sonra, bağlanmak [için PowerShell'i kullanarak bir kümeye bağlan](service-fabric-connect-to-secure-cluster.md#connect-to-a-cluster-using-powershell) bölümündeki adımları izleyin. Örnek:

```powershell
$ConnectArgs = @{  ConnectionEndpoint = '10.7.0.5:19000';  X509Credential = $True;  StoreLocation = 'LocalMachine';  StoreName = "MY";  ServerCertThumbprint = "057b9544a6f2733e0c8d3a60013a58948213f551";  FindType = 'FindByThumbprint';  FindValue = "057b9544a6f2733e0c8d3a60013a58948213f551"   }
Connect-ServiceFabricCluster $ConnectArgs
```

Daha sonra bu kümeyle çalışmak için diğer PowerShell komutlarını çalıştırabilirsiniz. Örneğin, bu güvenli kümedeki düğümlerin listesini göstermek için [Get-ServiceFabricNode'u](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) çalıştırabilirsiniz.


Kümeyi kaldırmak için, Hizmet Kumaşı paketini indirdiğiniz kümedeki düğüme bağlanın, bir komut satırı açın ve paket klasörüne gidin. Şimdi aşağıdaki komutu çalıştırın:

```powershell
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

> [!NOTE]
> Yanlış sertifika yapılandırması, kümenin dağıtım sırasında gelmesini engelleyebilir. Güvenlik sorunlarını kendi kendine tanılamak için, Olay Görüntüleyicisi grubu **Uygulamaları ve Hizmetleri Günlükleri** > **Microsoft-Service Fabric'e**bakın.
> 
> 

