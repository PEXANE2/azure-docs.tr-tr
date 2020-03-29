---
title: Medya Hizmetleri ve Apple FairPlay lisans desteği - Azure | Microsoft Dokümanlar
description: Bu konu, Apple FairPlay lisans gereksinimlerine ve yapılandırmaya genel bir bakış sağlar.
author: juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2018
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 6d4b7ba842d08723b90a4f2491d9e79e68dd932e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60733581"
---
# <a name="apple-fairplay-license-requirements-and-configuration"></a>Apple FairPlay lisansı gereksinimleri ve yapılandırması 

Azure Medya Hizmetleri, HLS içeriğinizi **Apple FairPlay** (AES-128 CBC) ile şifrelemenizi sağlar. Medya Hizmetleri ayrıca FairPlay lisansları sunmak için bir hizmet de sunmaktadır. Bir oyuncu FairPlay korumalı içeriğinizi oynamaya çalıştığında, lisans almak için lisans teslim hizmetine bir istek gönderilir. Lisans hizmeti isteği onaylarsa, istemciye gönderilen ve belirtilen içeriğin şifresini çözmek ve oynatmak için kullanılan lisansı verir.

Medya Hizmetleri ayrıca FairPlay lisanslarınızı yapılandırmak için kullanabileceğiniz API'ler de sağlar. Bu konu FairPlay lisans gereksinimlerini tartışır ve Medya Hizmetleri API'lerini kullanarak bir **FairPlay** lisansını nasıl yapılandırabileceğinizi gösterir. 

## <a name="requirements"></a>Gereksinimler

HLS içeriğinizi **Apple FairPlay** ile şifrelemek ve FairPlay lisansları sunmak için Medya Hizmetlerini kullanmak için Medya Hizmetleri'ni kullanırken aşağıdakiler gereklidir:

* [Apple Geliştirme Programı'na](https://developer.apple.com/)kaydolun.
* Apple, içerik sahibinin [dağıtım paketini](https://developer.apple.com/contact/fps/)almasını gerektirir. Medya Hizmetleri ile Anahtar Güvenlik Modülü 'ni (KSM) zaten uyguladığınızı ve son FPS paketini istediğinizi belirtin. Sertifika oluşturmak ve Uygulama Gizli Anahtarı (ASK) almak için son FPS paketinde talimatlar vardır. FairPlay'i yapılandırmak için ASK'yi kullanırsınız.
* Aşağıdaki şeyler Medya Hizmetleri anahtarı/lisans teslim tarafında ayarlanmalıdır:

    * **App Cert (AC)**: Bu özel anahtar içeren bir .pfx dosyasıdır. Bu dosyayı oluşturur ve bir parola ile şifrelersiniz. .pfx dosyası Base64 formatında olmalıdır.

        Aşağıdaki adımlar, FairPlay için bir .pfx sertifika dosyasının nasıl oluşturacağınızı açıklar:

        1. OpenSSL'den https://slproweb.com/products/Win32OpenSSL.htmlyükleyin.

            FairPlay sertifikasının ve Apple tarafından teslim edilen diğer dosyaların bulunduğu klasöre gidin.
        2. Komut satırından aşağıdaki komutu çalıştırın. Bu, .cer dosyasını .pem dosyasına dönüştürür.

            "C:\OpenSSL-Win32\bin\openssl.exe" x509 -inform der -in FairPlay.cer -out FairPlay-out.pem
        3. Komut satırından aşağıdaki komutu çalıştırın. Bu, .pem dosyasını özel anahtarla bir .pfx dosyasına dönüştürür. .pfx dosyasının şifresi openssl tarafından istenir.

            "C:\OpenSSL-Win32\bin\openssl.exe" pkcs12 -export -out FairPlay-out.pfx -inkey privatekey.pem -in FairPlay-out.pem -passin file:privatekey-pem-pass.txt
            
    * **App Cert şifresi**: .pfx dosyasını oluşturmanın şifresi.
    * **ASK**: Bu anahtar, Apple Developer portalını kullanarak sertifikayı oluşturduğunuzda alınır. Her geliştirme ekibi benzersiz bir ASK alır. ASK'nin bir kopyasını kaydedin ve güvenli bir yerde saklayın. Medya Hizmetleri ile ASK'yi FairPlayAsk olarak yapılandırmanız gerekir.
    
* Aşağıdaki şeyler FPS istemci tarafı tarafından ayarlanmalıdır:

  * **App Cert (AC)**: Bu, işletim sisteminin bazı yükü şifrelemek için kullandığı ortak anahtarı içeren bir .cer/.der dosyasıdır. Medya Hizmetleri bu konuda bilmek gerekir, çünkü oyuncu tarafından gereklidir. Anahtar teslim hizmeti, ilgili özel anahtarı kullanarak şifresini çözer.

* FairPlay şifreli akışını oynatmak için önce gerçek bir ASK alın ve ardından gerçek bir sertifika oluşturun. Bu işlem üç bölümü de oluşturur:

  * .der dosyası
  * .pfx dosyası
  * .pfx için şifre

## <a name="fairplay-and-player-apps"></a>FairPlay ve oyuncu uygulamaları

İçeriğiniz **Apple FairPlay**ile şifrelendiğinde, tek tek video ve ses örnekleri **AES-128 CBC** modu kullanılarak şifrelenir. **FairPlay Streaming** (FPS), iOS ve Apple TV'de yerel destekle aygıt işletim sistemlerine entegre edilmiştir. OS X'teki Safari, Şifreli Ortam Uzantıları (EME) arabirim desteğini kullanarak FPS'yi etkinleştiri.

Azure Media Player, FairPlay oynatmayı da destekler. Daha fazla bilgi için Azure [Media Player belgelerine](https://amp.azure.net/libs/amp/latest/docs/index.html)bakın.

iOS SDK'yı kullanarak kendi oyuncu uygulamalarınızı geliştirebilirsiniz. FairPlay içeriğini oynatabilmek için lisans değişim protokolünü uygulamanız gerekir. Bu protokol Apple tarafından belirtilmemiştir. Anahtar teslim isteklerinin nasıl gönderilen her uygulamaya kalmış. Media Services FairPlay anahtar teslim hizmeti, SPC'nin aşağıdaki formda bir www-form-url kodlanmış posta mesajı olarak gelmesini bekler:

```
spc=<Base64 encoded SPC>
```

## <a name="fairplay-configuration-net-example"></a>FairPlay yapılandırma .NET örneği

FairPlay lisanslarını yapılandırmak için Medya Hizmetleri API'sini kullanabilirsiniz. Oyuncu FairPlay korumalı içeriğinizi oynatmaya çalıştığında, lisansı almak için lisans dağıtım hizmetine bir istek gönderilir. Lisans hizmeti isteği onaylarsa, hizmet lisansı verir. İstemciye gönderilir ve belirtilen içeriğin şifresini çözmek ve oynatmak için kullanılır.

> [!NOTE]
> Genellikle, yalnızca bir sertifika seti ve ask olurdu, çünkü FairPlay ilke seçenekleri yalnızca bir kez yapılandırmak istiyorum.

Aşağıdaki örnekte, lisansı yapılandırmak için [Media Services .NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models?view=azure-dotnet) kullanılır.

```csharp
private static ContentKeyPolicyFairPlayConfiguration ConfigureFairPlayPolicyOptions()
{

    string askHex = "";
    string FairPlayPfxPassword = "";

    var appCert = new X509Certificate2("FairPlayPfxPath", FairPlayPfxPassword, X509KeyStorageFlags.Exportable);

    byte[] askBytes = Enumerable
        .Range(0, askHex.Length)
        .Where(x => x % 2 == 0)
        .Select(x => Convert.ToByte(askHex.Substring(x, 2), 16))
        .ToArray();

    ContentKeyPolicyFairPlayConfiguration fairPlayConfiguration =
    new ContentKeyPolicyFairPlayConfiguration
    {
        Ask = askBytes,
        FairPlayPfx =
                Convert.ToBase64String(appCert.Export(X509ContentType.Pfx, FairPlayPfxPassword)),
        FairPlayPfxPassword = FairPlayPfxPassword,
        RentalAndLeaseKeyType =
                ContentKeyPolicyFairPlayRentalAndLeaseKeyType
                .PersistentUnlimited,
        RentalDuration = 2249
    };

    return fairPlayConfiguration;
}
```

## <a name="next-steps"></a>Sonraki adımlar

DRM ile nasıl [korunacaklarına](protect-with-drm.md) göz atın
