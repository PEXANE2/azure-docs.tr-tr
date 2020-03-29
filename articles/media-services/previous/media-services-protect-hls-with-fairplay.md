---
title: HLS içeriğini Microsoft PlayReady veya Apple FairPlay ile koruyun - Azure | Microsoft Dokümanlar
description: Bu konu genel bir bakış sağlar ve HTTP Live Streaming (HLS) içeriğinizi Apple FairPlay ile dinamik olarak şifrelemek için Azure Medya Hizmetlerinin nasıl kullanılacağını gösterir. Ayrıca, müşterilere FairPlay lisansları sunmak için Medya Hizmetleri lisans teslim hizmetinin nasıl kullanılacağını da gösterir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: 873bc4ab5e435b91ff4400a39c92db0d0bb9baa8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74968774"
---
# <a name="protect-your-hls-content-with-apple-fairplay-or-microsoft-playready"></a>HLS içeriğinizi Apple FairPlay veya Microsoft PlayReady ile koruyun

> [!NOTE]
> Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).   > Media Services v2'ye yeni özellikler veya işlevler eklenmemektedir. <br/>En son sürümü göz atın, [Medya Hizmetleri v3](https://docs.microsoft.com/azure/media-services/latest/). Ayrıca, [v2'den v3'e geçiş kılavuzuna](../latest/migrate-from-v2-to-v3.md) bakın
>

Azure Medya Hizmetleri, aşağıdaki biçimleri kullanarak HTTP Live Streaming (HLS) içeriğinizi dinamik olarak şifrelemenizi sağlar:  

* **AES-128 zarf açık anahtar**

    Tüm öbek **AES-128 CBC** modu kullanılarak şifrelenir. Akışın şifresinin çözülmesi yerel olarak iOS ve OS X oynatıcı tarafından desteklenir. Daha fazla bilgi için [AES-128 dinamik şifreleme ve anahtar teslim hizmetini kullanma](media-services-protect-with-aes128.md)bilgisine bakın.
* **Apple FairPlay**

    Tek tek video ve ses örnekleri **AES-128 CBC** modu kullanılarak şifrelenir. **FairPlay Streaming** (FPS), iOS ve Apple TV'de yerel destekle aygıt işletim sistemlerine entegre edilmiştir. OS X'teki Safari, Şifreli Ortam Uzantıları (EME) arabirim desteğini kullanarak FPS'yi etkinleştiri.
* **Microsoft PlayReady**

Aşağıdaki resimde **HLS + FairPlay veya PlayReady dinamik şifreleme** iş akışı gösterilmektedir.

![Dinamik şifreleme iş akışı diyagramı](./media/media-services-content-protection-overview/media-services-content-protection-with-FairPlay.png)

Bu makale, HLS içeriğinizi Apple FairPlay ile dinamik olarak şifrelemek için Medya Hizmetlerinin nasıl kullanılacağını göstermektedir. Ayrıca, müşterilere FairPlay lisansları sunmak için Medya Hizmetleri lisans teslim hizmetinin nasıl kullanılacağını da gösterir.

> [!NOTE]
> HLS içeriğinizi PlayReady ile de şifrelemek istiyorsanız, ortak bir içerik anahtarı oluşturmanız ve bunu kıymetinizle ilişkilendirmeniz gerekir. [PlayReady dinamik ortak şifrelemeyi kullanma'da](media-services-protect-with-playready-widevine.md)açıklandığı gibi içerik anahtarının yetkilendirme ilkesini de yapılandırmanız gerekir.
>
>

## <a name="requirements-and-considerations"></a>Gereksinimler ve hususlar

HlS'i FairPlay ile şifrelenmiş olarak sunmak ve FairPlay lisansları sunmak için Medya Hizmetleri'ni kullanırken aşağıdakiler gereklidir:

  * Bir Azure hesabı. Ayrıntılar için [Azure ücretsiz deneme sürümüne](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)bakın.
  * Bir Media Services hesabı. Bir tane oluşturmak için azure [portalını kullanarak bir Azure Medya Hizmetleri hesabı oluşturma'ya](media-services-portal-create-account.md)bakın.
  * [Apple Geliştirme Programı'na](https://developer.apple.com/)kaydolun.
  * Apple, içerik sahibinin [dağıtım paketini](https://developer.apple.com/contact/fps/)almasını gerektirir. Medya Hizmetleri ile Anahtar Güvenlik Modülü 'ni (KSM) zaten uyguladığınızı ve son FPS paketini istediğinizi belirtin. Sertifika oluşturmak ve Uygulama Gizli Anahtarı (ASK) almak için son FPS paketinde talimatlar vardır. FairPlay'i yapılandırmak için ASK'yi kullanırsınız.
  * Azure Medya Hizmetleri .NET SDK sürümü **3.6.0** veya sonrası.

Aşağıdaki şeyler Media Services anahtar teslim tarafında ayarlanmalıdır:

  * **App Cert (AC)**: Bu özel anahtar içeren bir .pfx dosyasıdır. Bu dosyayı oluşturur ve bir parola ile şifrelersiniz.

       Önemli bir teslim ilkesini yapılandırdığınızda, bu parolayı ve .pfx dosyasını Base64 biçiminde sağlamanız gerekir.

      Aşağıdaki adımlar, FairPlay için bir .pfx sertifika dosyasının nasıl oluşturacağınızı açıklar:

    1. OpenSSL'den https://slproweb.com/products/Win32OpenSSL.htmlyükleyin.

        FairPlay sertifikasının ve Apple tarafından teslim edilen diğer dosyaların bulunduğu klasöre gidin.
    2. Komut satırından aşağıdaki komutu çalıştırın. Bu, .cer dosyasını .pem dosyasına dönüştürür.

        "C:\OpenSSL-Win32\bin\openssl.exe" x509 -inform der -in FairPlay.cer -out FairPlay-out.pem
    3. Komut satırından aşağıdaki komutu çalıştırın. Bu, .pem dosyasını özel anahtarla bir .pfx dosyasına dönüştürür. .pfx dosyasının şifresi openssl tarafından istenir.

        "C:\OpenSSL-Win32\bin\openssl.exe" pkcs12 -export -out FairPlay-out.pfx -inkey privatekey.pem -in FairPlay-out.pem -passin file:privatekey-pem-pass.txt
  * **App Cert şifresi**: .pfx dosyasını oluşturmanın şifresi.
  * **App Cert şifre kimliği**: Diğer Medya Hizmetleri anahtarlarını yükleme şekline benzer şekilde parolayı yüklemeniz gerekir. Medya Hizmetleri Kimliğini almak için **ContentKeyType.FairPlayPfxPassword** enum değerini kullanın. Anahtar teslim ilkesi seçeneği içinde kullanmaları gereken budur.
  * **iv**: Bu rasgele 16 bayt değeridir. Varlık teslim politikasındaki IV ile eşleşmelidir. IV'ü oluşturur sunuz ve her iki yere de koyarsınız: varlık teslim ilkesi ve anahtar teslim ilkesi seçeneği.
  * **ASK**: Bu anahtar, Apple Developer portalını kullanarak sertifikayı oluşturduğunuzda alınır. Her geliştirme ekibi benzersiz bir ASK alır. ASK'nin bir kopyasını kaydedin ve güvenli bir yerde saklayın. Ask'ı Daha sonra Medya Hizmetlerine FairPlayAsk olarak yapılandırmanız gerekir.
  * **ASK ID**: Bu kimlik, MEDYA Hizmetlerine ASK yüklediğinizde elde edilir. **ContentKeyType.FairPlayAsk** enum değerini kullanarak ASK'yi yüklemeniz gerekir. Sonuç olarak, Medya Hizmetleri Kimliği döndürülür ve anahtar teslim ilkesi seçeneğini ayarlarken bu kullanılmalıdır.

Aşağıdaki şeyler FPS istemci tarafı tarafından ayarlanmalıdır:

  * **App Cert (AC)**: Bu, işletim sisteminin bazı yükü şifrelemek için kullandığı ortak anahtarı içeren bir .cer/.der dosyasıdır. Medya Hizmetleri bu konuda bilmek gerekir, çünkü oyuncu tarafından gereklidir. Anahtar teslim hizmeti, ilgili özel anahtarı kullanarak şifresini çözer.

FairPlay şifreli akışını oynatmak için önce gerçek bir ASK alın ve ardından gerçek bir sertifika oluşturun. Bu işlem üç bölümü de oluşturur:

  * .der dosyası
  * .pfx dosyası
  * .pfx için şifre

Aşağıdaki istemciler HLS'yi **AES-128 CBC** şifrelemesiyle destekler: OS X'te Safari, Apple TV, iOS.

## <a name="configure-fairplay-dynamic-encryption-and-license-delivery-services"></a>FairPlay dinamik şifreleme ve lisans teslim hizmetlerini yapılandırma
Aşağıda, Medya Hizmetleri lisans teslim hizmetini kullanarak ve ayrıca dinamik şifreleme kullanarak FairPlay ile varlıklarınızı korumak için genel adımlar ve bunlar yer alametleri ve bunlar yer alametleri yer aınızveedin.

1. Bir varlık oluşturun ve dosyaları varlığa yükleyin.
2. Dosyayı içeren varlığı, bit hızı uyarlamalı MP4 kümesine kodlayın.
3. Bir içerik anahtarı oluşturup kodlanmış varlıkla ilişkilendirin.  
4. İçerik anahtarının yetkilendirme ilkesini yapılandırın. Aşağıdakileri belirtin:

   * Teslimat yöntemi (bu durumda, FairPlay).
   * FairPlay politika seçenekleri yapılandırması. FairPlay'in nasıl yapılandırılabildiğini öğrenmek için aşağıdaki örnekte **ConfigureFairPlayPolicyOptions()** yöntemine bakın.

     > [!NOTE]
     > Genellikle, yalnızca bir sertifika seti ve ask olurdu, çünkü FairPlay ilke seçenekleri yalnızca bir kez yapılandırmak istiyorum.
     >
     >
   * Kısıtlamalar (açık veya belirteç).
   * Anahtarın istemciye nasıl teslim edilebildiğini tanımlayan anahtar teslim türüne özgü bilgiler.
5. Varlık teslim ilkesini yapılandırın. Teslim ilkesi yapılandırması şunları içerir:

   * Teslimat protokolü (HLS).
   * Dinamik şifreleme türü (ortak CBC şifreleme).
   * Lisans edinme URL'si.

     > [!NOTE]
     > FairPlay ve başka bir Dijital Haklar Yönetimi (DRM) sistemiyle şifrelenmiş bir akış sunmak istiyorsanız, ayrı teslimat ilkeleri yapılandırmanız gerekir:
     >
     > * Ortak Şifreleme (CENC) (PlayReady + Widevine) ile HTTP (DASH) üzerinden Dinamik Adaptif Akış yapılandırmak için bir IAssetDeliveryPolicy ve PlayReady ile Pürüzsüz
     > * HLS için FairPlay yapılandırmak için başka bir IAssetDeliveryPolicy
     >
     >
6. Akış URL’si almak için bir OnDemand bulucu oluşturun.

## <a name="use-fairplay-key-delivery-by-player-apps"></a>Oyuncu uygulamalarına göre FairPlay anahtar teslimini kullanma
iOS SDK'yı kullanarak oyuncu uygulamaları geliştirebilirsiniz. FairPlay içeriğini oynatabilmek için lisans değişim protokolünü uygulamanız gerekir. Bu protokol Apple tarafından belirtilmemiştir. Anahtar teslim isteklerinin nasıl gönderilen her uygulamaya kalmış. Media Services FairPlay anahtar teslim hizmeti, SPC'nin aşağıdaki formda bir www-form-url kodlanmış posta mesajı olarak gelmesini bekler:

    spc=<Base64 encoded SPC>

> [!NOTE]
> Azure Media Player FairPlay oynatmayı destekler. Daha fazla bilgi için [Azure Media Player belgelerine](https://amp.azure.net/libs/amp/latest/docs/index.html) bakın.
>
>

## <a name="streaming-urls"></a>Akış URL'leri
Varlığınız birden fazla DRM ile şifrelenmişse, akış URL'sinde bir şifreleme etiketi kullanmanız gerekir: (format='m3u8-aapl', encryption='xxx').

Aşağıdaki noktalara dikkat edilmelidir:

* Yalnızca sıfır veya bir şifreleme türü belirtilebilir.
* Varlığa yalnızca bir şifreleme uygulandıysa, şifreleme türünün URL'de belirtilmesi gerekmez.
* Şifreleme türü büyük/küçük harf duyarsız.
* Aşağıdaki şifreleme türleri belirtilebilir:  
  * **cenc**: Ortak şifreleme (PlayReady veya Widevine)
  * **cbcs-aapl**: FairPlay
  * **cbc**: AES zarf şifreleme

## <a name="create-and-configure-a-visual-studio-project"></a>Visual Studio projesi oluşturup yapılandırma

1. Geliştirme ortamınızı kurun ve app.config dosyanızı [.NET ile Media Services geliştirme](media-services-dotnet-how-to-use.md) bölümünde açıklandığı gibi bağlantı bilgileriyle doldurun. 
2. App.config dosyanızda tanımlanan **appSettings**’e aşağıdaki öğeleri ekleyin:

    ```xml
    <add key="Issuer" value="http://testissuer.com"/>
    <add key="Audience" value="urn:test"/>
    ```

## <a name="example"></a>Örnek

Aşağıdaki örnek, FairPlay ile şifrelenmiş içeriğinizi sunmak için Medya Hizmetlerini kullanma yeteneğini göstermektedir. Bu işlev,.NET sürüm 3.6.0 için Azure Medya Hizmetleri SDK'da tanıtıldı. 

Bu bölümde gösterilen kodu Program.cs dosyanızdaki kodun üzerine yazın.

>[!NOTE]
>Farklı AMS ilkeleri için sınır 1.000.000 ilkedir (örneğin, Bulucu ilkesi veya ContentKeyAuthorizationPolicy için). Uzun süre boyunca kullanılmak için oluşturulan bulucu ilkeleri gibi aynı günleri / erişim izinlerini sürekli olarak kullanıyorsanız, aynı ilke kimliğini kullanmalısınız (karşıya yükleme olmayan ilkeler için). Daha fazla bilgi için [bu makaleye](media-services-dotnet-manage-entities.md#limit-access-policies) bakın.

Değişkenleri, giriş dosyalarınızın bulunduğu klasörlere işaret edecek şekilde güncelleştirdiğinizden emin olun.

```csharp
using System;
using System.Collections.Generic;
using System.Configuration;
using System.IO;
using System.Linq;
using System.Threading;
using Microsoft.WindowsAzure.MediaServices.Client;
using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
using Microsoft.WindowsAzure.MediaServices.Client.FairPlay;
using Newtonsoft.Json;
using System.Security.Cryptography.X509Certificates;

namespace DynamicEncryptionWithFairPlay
{
    class Program
    {
        // Read values from the App.config file.
        private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AMSAADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["AMSRESTAPIEndpoint"];
        private static readonly string _AMSClientId =
            ConfigurationManager.AppSettings["AMSClientId"];
        private static readonly string _AMSClientSecret =
            ConfigurationManager.AppSettings["AMSClientSecret"];

        private static readonly Uri _sampleIssuer =
            new Uri(ConfigurationManager.AppSettings["Issuer"]);
        private static readonly Uri _sampleAudience =
            new Uri(ConfigurationManager.AppSettings["Audience"]);

        // Field for service context.
        private static CloudMediaContext _context = null;

        private static readonly string _mediaFiles =
            Path.GetFullPath(@"../..\Media");

        private static readonly string _singleMP4File =
            Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");

        static void Main(string[] args)
        {
            AzureAdTokenCredentials tokenCredentials =
                new AzureAdTokenCredentials(_AADTenantDomain,
                    new AzureAdClientSymmetricKey(_AMSClientId, _AMSClientSecret),
                    AzureEnvironments.AzureCloudEnvironment);

            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            bool tokenRestriction = false;
            string tokenTemplateString = null;

            IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
            Console.WriteLine("Uploaded asset: {0}", asset.Id);

            IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
            Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);

            IContentKey key = CreateCommonCBCTypeContentKey(encodedAsset);
            Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
            Console.WriteLine("FairPlay License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay));
            Console.WriteLine();

            if (tokenRestriction)
                tokenTemplateString = AddTokenRestrictedAuthorizationPolicy(key);
            else
                AddOpenAuthorizationPolicy(key);

            Console.WriteLine("Added authorization policy: {0}", key.AuthorizationPolicyId);
            Console.WriteLine();

            CreateAssetDeliveryPolicy(encodedAsset, key);
            Console.WriteLine("Created asset delivery policy. \n");
            Console.WriteLine();

            if (tokenRestriction && !String.IsNullOrEmpty(tokenTemplateString))
            {
                // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
                // back into a TokenRestrictionTemplate class instance.
                TokenRestrictionTemplate tokenTemplate =
                    TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

                // Generate a test token based on the data in the given TokenRestrictionTemplate.
                // Note, you need to pass the key id Guid because we specified
                // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
                Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
                string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey,
                                            DateTime.UtcNow.AddDays(365));
                Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
                Console.WriteLine();
            }

            string url = GetStreamingOriginLocator(encodedAsset);
            Console.WriteLine("Encrypted HLS URL: {0}/manifest(format=m3u8-aapl)", url);

            Console.ReadLine();
        }

        static public IAsset UploadFileAndCreateAsset(string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
                Console.WriteLine("File does not exist.");
                return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.None);

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Created assetFile {0}", assetFile.Name);

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            return inputAsset;
        }

        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset inputAsset)
        {
            var encodingPreset = "Adaptive Streaming";

            IJob job = _context.Jobs.Create(String.Format("Encoding {0}", inputAsset.Name));

            var mediaProcessors =
            _context.MediaProcessors.Where(p => p.Name.Contains("Media Encoder Standard")).ToList();

            var latestMediaProcessor =
            mediaProcessors.OrderBy(mp => new Version(mp.Version)).LastOrDefault();

            ITask encodeTask = job.Tasks.AddNew("Encoding", latestMediaProcessor, encodingPreset, TaskOptions.None);
            encodeTask.InputAssets.Add(inputAsset);
            encodeTask.OutputAssets.AddNew(String.Format("{0} as {1}", inputAsset.Name, encodingPreset), AssetCreationOptions.StorageEncrypted);

            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();

            return job.OutputMediaAssets[0];
        }

        static public IContentKey CreateCommonCBCTypeContentKey(IAsset asset)
        {
            // Create HLS SAMPLE AES encryption content key
            Guid keyId = Guid.NewGuid();
            byte[] contentKey = GetRandomBuffer(16);

            IContentKey key = _context.ContentKeys.Create(
                        keyId,
                        contentKey,
                        "ContentKey",
                        ContentKeyType.CommonEncryptionCbcs);

            // Associate the key with the asset.
            asset.ContentKeys.Add(key);

            return key;
        }


        static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
        {
            // Create ContentKeyAuthorizationPolicy with Open restrictions
            // and create authorization policy          

            List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                    {
                    new ContentKeyAuthorizationPolicyRestriction
                    {
                        Name = "Open",
                        KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                        Requirements = null
                    }
                    };


            // Configure FairPlay policy option.
            string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();

            IContentKeyAuthorizationPolicyOption FairPlayPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
            ContentKeyDeliveryType.FairPlay,
            restrictions,
            FairPlayConfiguration);


            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common CBC Content Key with no restrictions").
                Result;

            contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);

            // Associate the content key authorization policy with the content key.
            contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
            contentKey = contentKey.UpdateAsync().Result;
        }

        public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
        {
            string tokenTemplateString = GenerateTokenRequirements();

            List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                    {
                    new ContentKeyAuthorizationPolicyRestriction
                    {
                        Name = "Token Authorization Policy",
                        KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                        Requirements = tokenTemplateString,
                    }
                    };

            // Configure FairPlay policy option.
            string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();


            IContentKeyAuthorizationPolicyOption FairPlayPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                   ContentKeyDeliveryType.FairPlay,
                   restrictions,
                   FairPlayConfiguration);

            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common CBC Content Key with token restrictions").
                Result;

            contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);

            // Associate the content key authorization policy with the content key
            contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
            contentKey = contentKey.UpdateAsync().Result;

            return tokenTemplateString;
        }

        private static string ConfigureFairPlayPolicyOptions()
        {
            // For testing you can provide all zeroes for ASK bytes together with the cert from Apple FPS SDK.
            // However, for production you must use a real ASK from Apple bound to a real prod certificate.
            byte[] askBytes = Guid.NewGuid().ToByteArray();
            var askId = Guid.NewGuid();
            // Key delivery retrieves askKey by askId and uses this key to generate the response.
            IContentKey askKey = _context.ContentKeys.Create(
                        askId,
                        askBytes,
                        "askKey",
                        ContentKeyType.FairPlayASk);

            //Customer password for creating the .pfx file.
            string pfxPassword = "<customer password for creating the .pfx file>";
            // Key delivery retrieves pfxPasswordKey by pfxPasswordId and uses this key to generate the response.
            var pfxPasswordId = Guid.NewGuid();
            byte[] pfxPasswordBytes = System.Text.Encoding.UTF8.GetBytes(pfxPassword);
            IContentKey pfxPasswordKey = _context.ContentKeys.Create(
                        pfxPasswordId,
                        pfxPasswordBytes,
                        "pfxPasswordKey",
                        ContentKeyType.FairPlayPfxPassword);

            // iv - 16 bytes random value, must match the iv in the asset delivery policy.
            byte[] iv = Guid.NewGuid().ToByteArray();

            //Specify the .pfx file created by the customer.
            var appCert = new X509Certificate2("path to the .pfx file created by the customer", pfxPassword, X509KeyStorageFlags.Exportable);

            string FairPlayConfiguration =
            Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
                appCert,
                pfxPassword,
                pfxPasswordId,
                askId,
                iv);

            return FairPlayConfiguration;
        }

        static private string GenerateTokenRequirements()
        {
            TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);

            template.PrimaryVerificationKey = new SymmetricVerificationKey();
            template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();
            template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);

            return TokenRestrictionTemplateSerializer.Serialize(template);
        }

        static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
        {
            var kdPolicy = _context.ContentKeyAuthorizationPolicies.Where(p => p.Id == key.AuthorizationPolicyId).Single();

            var kdOption = kdPolicy.Options.Single(o => o.KeyDeliveryType == ContentKeyDeliveryType.FairPlay);

            FairPlayConfiguration configFP = JsonConvert.DeserializeObject<FairPlayConfiguration>(kdOption.KeyDeliveryConfiguration);

            // Get the FairPlay license service URL.
            Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay);

            // The reason the below code replaces "https://" with "skd://" is because
            // in the IOS player sample code which you obtained in Apple developer account,
            // the player only recognizes a Key URL that starts with skd://.
            // However, if you are using a customized player,
            // you can choose whatever protocol you want.
            // For example, "https".

            Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
            {
                    {AssetDeliveryPolicyConfigurationKey.FairPlayLicenseAcquisitionUrl, acquisitionUrl.ToString().Replace("https://", "skd://")},
                    {AssetDeliveryPolicyConfigurationKey.CommonEncryptionIVForCbcs, configFP.ContentEncryptionIV}
            };

            var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryptionCbcs,
            AssetDeliveryProtocol.HLS,
            assetDeliveryPolicyConfiguration);

            // Add AssetDelivery Policy to the asset
            asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        }


        /// <summary>
        /// Gets the streaming origin locator.
        /// </summary>
        /// <param name="assets"></param>
        /// <returns></returns>
        static public string GetStreamingOriginLocator(IAsset asset)
        {

            // Get a reference to the streaming manifest file from the  
            // collection of files in the asset.

            var assetFile = asset.AssetFiles.LoList().Where(f => f.Name.ToLower().
                         EndsWith(".ism")).
                         FirstOrDefault();

            // Create a 30-day readonly access policy.
            IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

            // Create a locator to the streaming content on an origin.
            ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

            // Create a URL to the manifest file.
            return originLocator.Path + assetFile.Name;
        }

        static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
            ((IJob)sender).Name,
            e.CurrentState,
            DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
        }

        static private byte[] GetRandomBuffer(int length)
        {
            var returnValue = new byte[length];

            using (var rng =
            new System.Security.Cryptography.RNGCryptoServiceProvider())
            {
                rng.GetBytes(returnValue);
            }

            return returnValue;
        }
    }
}
```

## <a name="additional-notes"></a>Ek notlar

* Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="next-steps-media-services-learning-paths"></a>Sonraki adımlar: Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
