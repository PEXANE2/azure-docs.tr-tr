---
title: Bir bulut hizmeti için SSL'yi yapılandırma | Microsoft Dokümanlar
description: Bir web rolü için https bitiş noktasını nasıl belirtdiğinizi ve uygulamanızı güvence altına almak için ssl sertifikasını nasıl yükleydiğinizi öğrenin. Bu örnekler Azure portalını kullanır.
services: cloud-services
documentationcenter: .net
author: tgore03
ms.service: cloud-services
ms.topic: article
ms.date: 05/26/2017
ms.author: tagore
ms.openlocfilehash: 6ddb7001f770a9d8aea38d1a4698e15c167aeaa4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79273143"
---
# <a name="configuring-ssl-for-an-application-in-azure"></a>Azure'daki uygulama için SSL'yi yapılandırma

Secure Socket Layer (SSL) şifrelemesi, internet üzerinden gönderilen verileri güvence altına almak için en yaygın olarak kullanılan yöntemdir. Bu yaygın görev, bir web rolü için bir HTTPS bitiş noktasının nasıl belirtilen ve uygulamanızı güvence altına almak için bir SSL sertifikasının nasıl yüklenilebildiğini tartışır.

> [!NOTE]
> Bu görevdeki yordamlar Azure Bulut Hizmetleri için geçerlidir; Uygulama Hizmetleri için [bkz.](../app-service/configure-ssl-bindings.md)
>

Bu görev bir üretim dağıtımı kullanır. Bu konunun sonunda bir hazırlama dağıtımı kullanma hakkında bilgi sağlanır.

Henüz bir bulut hizmeti oluşturmadıysanız [bunu](cloud-services-how-to-create-deploy-portal.md) ilk olarak okuyun.

## <a name="step-1-get-an-ssl-certificate"></a>Adım 1: SSL sertifikası alın
Bir uygulama için SSL'yi yapılandırmak için öncelikle, bu amaçla sertifika veren güvenilir bir üçüncü taraf olan Sertifika Yetkilisi (CA) tarafından imzalanmış bir SSL sertifikası almanız gerekir. Zaten bir sertifikanız yoksa, SSL sertifikası satan bir şirketten bir sertifika almanız gerekir.

Sertifika, Azure'daki SSL sertifikaları için aşağıdaki gereksinimleri karşılamalıdır:

* Sertifika özel bir anahtar içermelidir.
* Sertifika anahtar değişimi için oluşturulmalıdır, Kişisel Bilgi Alışverişi (.pfx) dosyasına ihraç edilebilir.
* Sertifikanın özne adı bulut hizmetine erişmek için kullanılan etki alanıyla eşleşmelidir. cloudapp.net etki alanı için bir sertifika yetkilisinden (CA) SSL sertifikası alamazsınız. Hizmetinize erişirken kullanmak üzere özel bir etki alanı adı edinmeniz gerekir. CA'dan sertifika istediğinizde, sertifikanın özne adı, başvurunuza erişmek için kullanılan özel alan adıile eşleşmelidir. Örneğin, özel alan adınız **contoso.com** ***.contoso.com** veya **www\.contoso.com**için CA'nızdan bir sertifika isteyebilirsiniz.
* Sertifika en az 2048 bit şifreleme kullanmalıdır.

Test amacıyla, kendi imzalı bir sertifika [oluşturabilir](cloud-services-certs-create.md) ve kullanabilirsiniz. Kendi imzalı bir sertifika ca üzerinden doğrulanmaz ve cloudapp.net etki alanını web sitesi URL'si olarak kullanabilir. Örneğin, aşağıdaki görev, sertifikada kullanılan ortak adın (CN) **sslexample.cloudapp.net**olan kendi imzalı bir sertifika kullanır.

Ardından, hizmet tanımınıza ve hizmet yapılandırma dosyalarınıza sertifika yla ilgili bilgileri eklemeniz gerekir.

<a name="modify"> </a>

## <a name="step-2-modify-the-service-definition-and-configuration-files"></a>Adım 2: Hizmet tanımını ve yapılandırma dosyalarını değiştirme
Başvurunuzun sertifikayı kullanacak şekilde yapılandırılması ve bir HTTPS bitiş noktası eklenmelidir. Sonuç olarak, hizmet tanımı ve hizmet yapılandırma dosyalarının güncelleştirilmesi gerekir.

1. Geliştirme ortamınızda, hizmet tanımı dosyasını (CSDEF) açın, **WebRole** bölümüne **Sertifikalar** bölümü ekleyin ve sertifika (ve ara sertifikalar) hakkında aşağıdaki bilgileri ekleyin:

   ```xml
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Certificates>
            <Certificate name="SampleCertificate"
                        storeLocation="LocalMachine"
                        storeName="My"
                        permissionLevel="limitedOrElevated" />
            <!-- IMPORTANT! Unless your certificate is either
            self-signed or signed directly by the CA root, you
            must include all the intermediate certificates
            here. You must list them here, even if they are
            not bound to any endpoints. Failing to list any of
            the intermediate certificates may cause hard-to-reproduce
            interoperability problems on some clients.-->
            <Certificate name="CAForSampleCertificate"
                        storeLocation="LocalMachine"
                        storeName="CA"
                        permissionLevel="limitedOrElevated" />
        </Certificates>
    ...
    </WebRole>
    ```

   **Sertifikalar** bölümü, sertifikamızın adını, konumunu ve bulunduğu mağazanın adını tanımlar.

   İzinler`permissionLevel` (öznitelik) aşağıdaki değerlerden birine ayarlanabilir:

   | İzin Değeri | Açıklama |
   | --- | --- |
   | limitedOrElevated |**(Varsayılan)** Tüm rol süreçleri özel anahtara erişebilir. |
   | Yükseltilmiş |Özel anahtara yalnızca yükseltilmiş işlemler erişebilir. |

2. Hizmet tanım dosyanıza, HTTPS'yi etkinleştirmek için **Bitiş Noktaları** bölümüne bir Giriş **Nokta noktası** öğesi ekleyin:

   ```xml
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Endpoints>
            <InputEndpoint name="HttpsIn" protocol="https" port="443"
                certificate="SampleCertificate" />
        </Endpoints>
    ...
    </WebRole>
    ```

3. Hizmet tanımı dosyanıza **Siteler** bölümüne bir **Bağlama** öğesi ekleyin. Bu öğe, sitenizin bitiş noktasını eşlemek için bir HTTPS bağlama ekler:

   ```xml
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="HttpsIn" endpointName="HttpsIn" />
                </Bindings>
            </Site>
        </Sites>
    ...
    </WebRole>
    ```

   Hizmet tanım dosyasında gerekli tüm değişiklikler tamamlandı; ancak, yine de hizmet yapılandırma dosyasına sertifika bilgilerini eklemeniz gerekir.
4. Service Configuration.Cloud.cscfg hizmet yapılandırma dosyanızda, sertifikanızla birlikte bir **Sertifika** değeri ekleyin. Aşağıdaki kod örneği, parmak izi değeri dışında **Sertifikalar** bölümünün ayrıntılarını sağlar.

   ```xml
    <Role name="Deployment">
    ...
        <Certificates>
            <Certificate name="SampleCertificate"
                thumbprint="9427befa18ec6865a9ebdc79d4c38de50e6316ff"
                thumbprintAlgorithm="sha1" />
            <Certificate name="CAForSampleCertificate"
                thumbprint="79d4c38de50e6316ff9427befa18ec6865a9ebdc"
                thumbprintAlgorithm="sha1" />
        </Certificates>
    ...
    </Role>
    ```

(Bu örnek, parmak izi algoritması için **sha1** kullanır. Sertifikanızın parmak izi algoritması için uygun değeri belirtin.)

Hizmet tanımı ve hizmet yapılandırma dosyaları güncelleştirildikten sonra, dağıtımınızı Azure'a yüklemek üzere paketleyin. **Cspack**kullanıyorsanız, eklediğiniz sertifika bilgilerinin üzerine yazacağı için **/generateConfigurationFile** bayrağını kullanmayın.

## <a name="step-3-upload-a-certificate"></a>Adım 3: Sertifika yükleme
Azure portalına bağlanın ve...

1. Portalın **Tüm Kaynaklar** bölümünde bulut hizmetinizi seçin.

    ![Bulut hizmetinizi yayımlayın](media/cloud-services-configure-ssl-certificate-portal/browse.png)

2. **Sertifikalar'ı**tıklatın.

    ![Sertifikalar simgesine tıklayın](media/cloud-services-configure-ssl-certificate-portal/certificate-item.png)

3. Sertifikalar alanının en üstündeki **Yükle'yi** tıklatın.

    ![Yükle menüsü öğesini tıklatın](media/cloud-services-configure-ssl-certificate-portal/Upload_menu.png)

4. **Dosya**, **Parola,** sonra veri giriş alanının altındaki **Yükle'yi** tıklatın.

## <a name="step-4-connect-to-the-role-instance-by-using-https"></a>Adım 4: HTTPS'yi kullanarak rol örneğine bağlanma
Azure'da dağıtımınız çalışmaya devam ettiğine göre, HTTPS'yi kullanarak dağıtıma bağlanabilirsiniz.

1. Web tarayıcısını açmak için **Site URL'sini** tıklatın.

   ![Site URL'sini tıklatın](media/cloud-services-configure-ssl-certificate-portal/navigate.png)

2. Web tarayıcınızda, **https**yerine **https'yi** kullanmak için bağlantıyı değiştirin ve sayfayı ziyaret edin.

   > [!NOTE]
   > Kendi imzalı bir sertifika kullanıyorsanız, kendi imzalı sertifikayla ilişkili bir HTTPS bitiş noktasına göz attığınızda tarayıcıda bir sertifika hatası görebilirsiniz. Güvenilir bir sertifika yetkilisi tarafından imzalanmış bir sertifika kullanmak bu sorunu ortadan kaldırır; bu arada, hatayı yoksayabilirsiniz. (Başka bir seçenek, kullanıcının güvenilen sertifika yetkilisi sertifika deposuna kendi imzalı sertifikayı eklemektir.)
   >
   >

   ![Site önizleme](media/cloud-services-configure-ssl-certificate-portal/show-site.png)

   > [!TIP]
   > Üretim dağıtımı yerine hazırlama dağıtımı için SSL'yi kullanmak istiyorsanız, öncelikle hazırlama dağıtımı için kullanılan URL'yi belirlemeniz gerekir. Bulut hizmetiniz dağıtıldıktan sonra, hazırlama ortamının URL'si **dağıtım kimliği** GUID tarafından bu biçimde belirlenir:`https://deployment-id.cloudapp.net/`  
   >
   > GUID tabanlı URL'ye eşit ortak ada (CN) sahip bir sertifika oluşturun (örneğin, **328187776e774ceda8fc57609d404462.cloudapp.net).** Sertifikayı aşamalı bulut hizmetinize eklemek için portalı kullanın. Ardından, sertifika bilgilerini CSDEF ve CSCFG dosyalarınıza ekleyin, uygulamanızı yeniden paketleyin ve yeni paketi kullanmak üzere aşamalı dağıtımınızı güncelleyin.
   >

## <a name="next-steps"></a>Sonraki adımlar
* [Bulut hizmetinizin genel yapılandırması.](cloud-services-how-to-configure-portal.md)
* [Bulut hizmetini](cloud-services-how-to-create-deploy-portal.md)nasıl dağıtılayın öğrenin.
* Özel bir [etki alanı adı](cloud-services-custom-domain-name-portal.md)yapılandırın.
* [Bulut hizmetinizi yönetin.](cloud-services-how-to-manage-portal.md)



