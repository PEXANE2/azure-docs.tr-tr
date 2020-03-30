---
title: Birleştirme güvenlik yapılandırması
description: Elastik ölçek için bölme/birleştirme hizmetiyle şifreleme için x409 sertifikaları ayarlayın.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: VanMSFT
ms.author: vanto
ms.reviewer: sstein
ms.date: 12/18/2018
ms.openlocfilehash: a916645f153f73a98e7fc5d4046bdf557e8acf2b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73823523"
---
# <a name="split-merge-security-configuration"></a>Birleştirme güvenlik yapılandırması

Bölme/Birleştirme hizmetini kullanmak için güvenliği doğru şekilde yapılandırmanız gerekir. Hizmet, Microsoft Azure SQL Veritabanı'nın Elastik Ölçek özelliğinin bir parçasıdır. Daha fazla bilgi için [Elastik Ölçek Bölme ve Birleştirme Hizmeti Öğretici'ye](sql-database-elastic-scale-configure-deploy-split-and-merge.md)bakın.

## <a name="configuring-certificates"></a>Sertifikaları yapılandırma

Sertifikalar iki şekilde yapılandırılır. 

1. [SSL Sertifikasını Yapılandırmak için](#to-configure-the-ssl-certificate)
2. [İstemci Sertifikalarını Yapılandırmak İçin](#to-configure-client-certificates) 

## <a name="to-obtain-certificates"></a>Sertifika almak için

Sertifikalar genel Sertifika Yetkilileri'nden (CA) veya [Windows Sertifika Hizmeti'nden](https://msdn.microsoft.com/library/windows/desktop/aa376539.aspx)alınabilir. Sertifika almak için tercih edilen yöntemler bunlardır.

Bu seçenekler kullanılamıyorsa, **kendi imzalı sertifikalar**oluşturabilirsiniz.

## <a name="tools-to-generate-certificates"></a>Sertifika oluşturmak için araçlar

* [Makecert.exe](https://msdn.microsoft.com/library/bfsktky3.aspx)
* [pvk2pfx.exe](https://msdn.microsoft.com/library/windows/hardware/ff550672.aspx)

### <a name="to-run-the-tools"></a>Araçları çalıştırmak için

* Visual Studios için Geliştirici Komut Komut Komut Ustem'den Visual [Studio Command Prompt'a](https://msdn.microsoft.com/library/ms229859.aspx) bakın 
  
    Yüklüyse, şu na gidin:
  
        %ProgramFiles(x86)%\Windows Kits\x.y\bin\x86 
* WDK'yı [Windows 8.1'den alın: Kitleri ve araçları indirin](https://msdn.microsoft.com/windows/hardware/gg454513#drivers)

## <a name="to-configure-the-ssl-certificate"></a>SSL sertifikasını yapılandırmak için

İletişimi şifrelemek ve sunucunun kimliğini doğrulamak için bir SSL sertifikası gereklidir. Aşağıdaki üç senaryodan en uygununu seçin ve tüm adımları uygulayın:

### <a name="create-a-new-self-signed-certificate"></a>Yeni bir kendi imzalı sertifika oluşturma

1. [Kendi İmzalı Sertifika Oluşturma](#create-a-self-signed-certificate)
2. [Kendi İmzalı SSL Sertifikası için PFX dosyası oluşturma](#create-pfx-file-for-self-signed-ssl-certificate)
3. [SSL Sertifikasını Bulut Hizmetine Yükleyin](#upload-ssl-certificate-to-cloud-service)
4. [Hizmet Yapılandırma Dosyasında SSL Sertifikasını Güncelleştirme](#update-ssl-certificate-in-service-configuration-file)
5. [İthalat SSL Belgelendirme Yetkilisi](#import-ssl-certification-authority)

### <a name="to-use-an-existing-certificate-from-the-certificate-store"></a>Sertifika deposundan varolan bir sertifikayı kullanmak için
1. [Sertifika Deposundan SSL Belgesi İhracatı](#export-ssl-certificate-from-certificate-store)
2. [SSL Sertifikasını Bulut Hizmetine Yükleyin](#upload-ssl-certificate-to-cloud-service)
3. [Hizmet Yapılandırma Dosyasında SSL Sertifikasını Güncelleştirme](#update-ssl-certificate-in-service-configuration-file)

### <a name="to-use-an-existing-certificate-in-a-pfx-file"></a>PFX dosyasında varolan bir sertifikayı kullanmak için
1. [SSL Sertifikasını Bulut Hizmetine Yükleyin](#upload-ssl-certificate-to-cloud-service)
2. [Hizmet Yapılandırma Dosyasında SSL Sertifikasını Güncelleştirme](#update-ssl-certificate-in-service-configuration-file)

## <a name="to-configure-client-certificates"></a>İstemci sertifikalarını yapılandırmak için
Hizmete istekleri doğrulamak için istemci sertifikaları gereklidir. Aşağıdaki üç senaryodan en uygununu seçin ve tüm adımları uygulayın:

### <a name="turn-off-client-certificates"></a>İstemci sertifikalarını kapatma
1. [İstemci Sertifikası Tabanlı Kimlik Doğrulamayı Kapatma](#turn-off-client-certificate-based-authentication)

### <a name="issue-new-self-signed-client-certificates"></a>Yeni kendi imzalı istemci sertifikaları verme
1. [Kendi İmzalı Sertifika Yetkilisi Oluşturun](#create-a-self-signed-certification-authority)
2. [CA Sertifikasını Bulut Hizmetine Yükleyin](#upload-ca-certificate-to-cloud-service)
3. [Hizmet Yapılandırma Dosyasında CA Sertifikasını Güncelleştirme](#update-ca-certificate-in-service-configuration-file)
4. [İstemci Sertifikaları Verme](#issue-client-certificates)
5. [İstemci Sertifikaları için PFX dosyaları oluşturma](#create-pfx-files-for-client-certificates)
6. [İstemci Sertifikasını Alma](#import-client-certificate)
7. [İstemci Sertifikası Parmak Izlerini Kopyala](#copy-client-certificate-thumbprints)
8. [Hizmet Yapılandırma Dosyasında İzin Verilen İstemleri Yapılandırma](#configure-allowed-clients-in-the-service-configuration-file)

### <a name="use-existing-client-certificates"></a>Varolan istemci sertifikalarını kullanma
1. [CA Ortak Anahtarı nı Bul](#find-ca-public-key)
2. [CA Sertifikasını Bulut Hizmetine Yükleyin](#upload-ca-certificate-to-cloud-service)
3. [Hizmet Yapılandırma Dosyasında CA Sertifikasını Güncelleştirme](#update-ca-certificate-in-service-configuration-file)
4. [İstemci Sertifikası Parmak Izlerini Kopyala](#copy-client-certificate-thumbprints)
5. [Hizmet Yapılandırma Dosyasında İzin Verilen İstemleri Yapılandırma](#configure-allowed-clients-in-the-service-configuration-file)
6. [Yapılandırma İstemci Sertifikası İptal Denetimi](#configure-client-certificate-revocation-check)

## <a name="allowed-ip-addresses"></a>İzin verilen IP adresleri
Hizmet bitiş noktalarına erişim, belirli IP adresleri aralıkları ile sınırlandırılabilir.

## <a name="to-configure-encryption-for-the-store"></a>Mağaza için şifrelemeyi yapılandırmak için
Meta veri deposunda depolanan kimlik bilgilerini şifrelemek için bir sertifika gereklidir. Aşağıdaki üç senaryodan en uygununu seçin ve tüm adımları uygulayın:

### <a name="use-a-new-self-signed-certificate"></a>Yeni bir kendi imzalı sertifika kullanma
1. [Kendi İmzalı Sertifika Oluşturma](#create-a-self-signed-certificate)
2. [Kendi İmzalı Şifreleme Sertifikası için PFX dosyası oluşturma](#create-pfx-file-for-self-signed-ssl-certificate)
3. [Şifreleme Sertifikasını Bulut Hizmetine Yükleme](#upload-encryption-certificate-to-cloud-service)
4. [Hizmet Yapılandırma Dosyasında Şifreleme Sertifikasını Güncelleştirme](#update-encryption-certificate-in-service-configuration-file)

### <a name="use-an-existing-certificate-from-the-certificate-store"></a>Sertifika deposundan varolan bir sertifikakullanma
1. [Sertifika Deposundan Şifreleme Sertifikası İhraç](#export-encryption-certificate-from-certificate-store)
2. [Şifreleme Sertifikasını Bulut Hizmetine Yükleme](#upload-encryption-certificate-to-cloud-service)
3. [Hizmet Yapılandırma Dosyasında Şifreleme Sertifikasını Güncelleştirme](#update-encryption-certificate-in-service-configuration-file)

### <a name="use-an-existing-certificate-in-a-pfx-file"></a>PFX dosyasında varolan bir sertifika kullanma
1. [Şifreleme Sertifikasını Bulut Hizmetine Yükleme](#upload-encryption-certificate-to-cloud-service)
2. [Hizmet Yapılandırma Dosyasında Şifreleme Sertifikasını Güncelleştirme](#update-encryption-certificate-in-service-configuration-file)

## <a name="the-default-configuration"></a>Varsayılan yapılandırma
Varsayılan yapılandırma, HTTP bitiş noktasına tüm erişimi reddeder. Bu uç noktalara gelen istekler veritabanı kimlik bilgileri gibi hassas bilgileri taşıyabileceğinden, bu önerilen ayardır.
Varsayılan yapılandırma, tüm HTTPS bitiş noktasına erişim sağlar. Bu ayar daha da kısıtlanabilir.

### <a name="changing-the-configuration"></a>Yapılandırmayı Değiştirme
Hizmet **yapılandırma dosyasındaki** ** \<EndpointAcls>** bölümünde, geçerli olan erişim denetim kuralları grubu ve bitiş noktası yapılandırılır.

```xml
<EndpointAcls>
    <EndpointAcl role="SplitMergeWeb" endPoint="HttpIn" accessControl="DenyAll" />
    <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="AllowAll" />
</EndpointAcls>
```

Erişim denetim grubundaki kurallar \<AccessControl adı=""> bölümünde yapılandırılır. 

Biçim, Ağ Erişim Denetim Listeleri belgelerinde açıklanmıştır.
Örneğin, 100.100.0.0 ile 100.100.255.255 aralığındaki IP'lerin HTTPS bitiş noktasına erişmesine izin vermek için kurallar şu şekilde görünür:

```xml
<AccessControl name="Retricted">
    <Rule action="permit" description="Some" order="1" remoteSubnet="100.100.0.0/16"/>
    <Rule action="deny" description="None" order="2" remoteSubnet="0.0.0.0/0" />
</AccessControl>
<EndpointAcls>
    <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="Restricted" />
</EndpointAcls>
```

## <a name="denial-of-service-prevention"></a>Hizmet önleme nin reddi
Hizmet Reddi saldırılarını algılamak ve önlemek için desteklenen iki farklı mekanizma vardır:

* Uzak ana bilgisayar başına eşzamanlı istek sayısını kısıtlama (varsayılan olarak kapalı)
* Uzak ana bilgisayar başına erişim oranını kısıtlama (varsayılan olarak)

Bunlar, IIS'de Dynamic IP Security'de belgelenen özelliklere dayanmaktadır. Bu yapılandırmayı değiştirirken aşağıdaki etkenlere dikkat edin:

* Yakınlık ve Ağ Adresi Çeviri aygıtlarının uzak ana bilgisayar bilgileri üzerindeki davranışı
* Web rolündeki herhangi bir kaynağa yapılan her istek dikkate alınr (örneğin, komut dosyalarının, görüntülerin vb. yüklenmesi)

## <a name="restricting-number-of-concurrent-accesses"></a>Eşzamanlı erişim sayısını kısıtlama
Bu davranışı yapılandıran ayarlar şunlardır:

```xml
<Setting name="DynamicIpRestrictionDenyByConcurrentRequests" value="false" />
<Setting name="DynamicIpRestrictionMaxConcurrentRequests" value="20" />
```

Bu korumayı etkinleştirmek için DynamicIpRestrictionDenyByConcurrentRequests'i true olarak değiştirin.

## <a name="restricting-rate-of-access"></a>Erişim oranını kısıtlama
Bu davranışı yapılandıran ayarlar şunlardır:

```xml
<Setting name="DynamicIpRestrictionDenyByRequestRate" value="true" />
<Setting name="DynamicIpRestrictionMaxRequests" value="100" />
<Setting name="DynamicIpRestrictionRequestIntervalInMilliseconds" value="2000" />
```

## <a name="configuring-the-response-to-a-denied-request"></a>Reddedilen bir isteğe yanıtı yapılandırma
Aşağıdaki ayar, reddedilen bir isteğe yanıtı yapılandırır:

```xml
<Setting name="DynamicIpRestrictionDenyAction" value="AbortRequest" />
```

Desteklenen diğer değerler için IIS'deki Dinamik IP Güvenliği belgelerine bakın.

## <a name="operations-for-configuring-service-certificates"></a>Hizmet sertifikalarını yapılandırma işlemleri
Bu konu yalnızca başvuru içindir. Aşağıda belirtilen yapılandırma adımlarını izleyin:

* SSL sertifikasını yapılandırma
* İstemci sertifikalarını yapılandırma

## <a name="create-a-self-signed-certificate"></a>Otomatik olarak imzalanan sertifika oluşturma
Yürütmek:

    makecert ^
      -n "CN=myservice.cloudapp.net" ^
      -e MM/DD/YYYY ^
      -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1" ^
      -a sha256 -len 2048 ^
      -sv MySSL.pvk MySSL.cer

Özelleştirmek için:

* -n hizmet URL'si ile. Joker karakterler ("CN=*.cloudapp.net") ve alternatif adlar ("CN=myservice1.cloudapp.net, CN=myservice2.cloudapp.net") desteklenir.
* -e sertifika son kullanma tarihi ile güçlü bir parola oluşturun ve istendiğinde belirtin.

## <a name="create-pfx-file-for-self-signed-ssl-certificate"></a>Kendi imzalı SSL sertifikası için PFX dosyası oluşturma
Yürütmek:

        pvk2pfx -pvk MySSL.pvk -spc MySSL.cer

Aşağıdaki seçeneklerle parola girin ve ardından sertifika ihracını girin:

* Evet, özel anahtarı dışa aktarma
* Tüm genişletilmiş özellikleri dışa aktarma

## <a name="export-ssl-certificate-from-certificate-store"></a>Sertifika deposundan SSL sertifikası ihracı
* Sertifika yı bulma
* Eylemler -tüm görevler> > Dışa Aktarma'yı tıklatın...
* Bir . Bu seçenekleri ile PFX dosyası:
  * Evet, özel anahtarı dışa aktarma
  * Mümkünse tüm sertifikaları sertifika yoluna ekleyin *Tüm genişletilmiş özellikleri dışa aktarma

## <a name="upload-ssl-certificate-to-cloud-service"></a>SSL sertifikasını bulut hizmetine yükleme
Varolan veya oluşturulan sertifikayı yükleyin. SSL anahtar çifti ile PFX dosyası:

* Özel anahtar bilgilerini koruyan parolayı girin

## <a name="update-ssl-certificate-in-service-configuration-file"></a>Hizmet yapılandırma dosyasında SSL sertifikasını güncelleştirme
Bulut hizmetine yüklenen sertifikanın parmak izi ile hizmet yapılandırma dosyasında aşağıdaki ayarın parmak izi değerini güncelleştirin:

    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="import-ssl-certification-authority"></a>SSL belgelendirme yetkilisini içe aktarma
Hizmetle iletişim kuracak tüm hesap/makinede aşağıdaki adımları izleyin:

* Çift tıklayın. Windows Gezgini'nde CER dosyası
* Sertifika iletişim kutusunda, Sertifikayı Yükle'yi tıklatın...
* Güvenilir Kök Sertifika Yetkilileri deposuna sertifika alma

## <a name="turn-off-client-certificate-based-authentication"></a>İstemci sertifikası tabanlı kimlik doğrulamayı kapatma
Yalnızca istemci sertifikası tabanlı kimlik doğrulaması desteklenir ve diğer mekanizmalar (örneğin, Microsoft Azure Sanal Ağı) yerinde olmadığı sürece hizmet uç noktalarına genel erişime izin verir.

Özelliği kapatmak için bu ayarları hizmet yapılandırma dosyasında false olarak değiştirin:

```xml
<Setting name="SetupWebAppForClientCertificates" value="false" />
<Setting name="SetupWebserverForClientCertificates" value="false" />
```

Ardından, CA sertifikası ayarında SSL sertifikasıyla aynı parmak izini kopyalayın:

```xml
<Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />
```

## <a name="create-a-self-signed-certification-authority"></a>Kendi imzalı bir sertifika yetkilisi oluşturun
Sertifika Yetkilisi olarak hareket etmek için kendi imzalı bir sertifika oluşturmak için aşağıdaki adımları uygulayın:

    makecert ^
    -n "CN=MyCA" ^
    -e MM/DD/YYYY ^
     -r -cy authority -h 1 ^
     -a sha256 -len 2048 ^
      -sr localmachine -ss my ^
      MyCA.cer

Özelleştirmek için

* -e sertifika son kullanma tarihi ile

## <a name="find-ca-public-key"></a>CA ortak anahtarını bulma
Tüm istemci sertifikaları, hizmet tarafından güvenilen bir Sertifika Yetkilisi tarafından verilmiş olmalıdır. Bulut hizmetine yüklemek için kimlik doğrulaması için kullanılacak istemci sertifikalarını veren Sertifika Yetkilisi'nin ortak anahtarını bulun.

Ortak anahtara sahip dosya kullanılamıyorsa, sertifika deposundan dışa aktarın:

* Sertifika yı bulma
  * Aynı Sertifika Yetkilisi tarafından verilen istemci sertifikasını arama
* Sertifikayı çift tıklatın.
* Sertifika iletişim kutusundaki Sertifika Yolu sekmesini seçin.
* Yoldaki CA girişini çift tıklatın.
* Sertifika özelliklerinin notlarını alın.
* **Sertifika** iletişim kutusunu kapatın.
* Sertifika yı bulma
  * Yukarıda belirtilen CA'yı arayın.
* Eylemler -tüm görevler> > Dışa Aktarma'yı tıklatın...
* Bir . Bu seçenekleri ile CER:
  * **Hayır, özel anahtarı dışa aktarma**
  * Mümkünse tüm sertifikaları sertifika yoluna ekleyin.
  * Tüm genişletilmiş özellikleri dışa aktarın.

## <a name="upload-ca-certificate-to-cloud-service"></a>CA sertifikasını bulut hizmetine yükleme
Varolan veya oluşturulan sertifikayı yükleyin. CA ortak anahtarı ile CER dosyası.

## <a name="update-ca-certificate-in-service-configuration-file"></a>Hizmet yapılandırma dosyasında CA sertifikasını güncelleştirme
Bulut hizmetine yüklenen sertifikanın parmak izi ile hizmet yapılandırma dosyasında aşağıdaki ayarın parmak izi değerini güncelleştirin:

```xml
<Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />
```

Aşağıdaki ayarın değerini aynı parmak izi ile güncelleştirin:

```xml
<Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />
```

## <a name="issue-client-certificates"></a>İstemci sertifikaları verme
Hizmete erişmeye yetkili her bireyin özel kullanımları için düzenlenmiş bir istemci sertifikasına sahip olması ve özel anahtarını korumak için kendi güçlü parolasını seçmesi gerekir. 

Aşağıdaki adımlar, kendi imzalı CA sertifikasının oluşturulduğu ve depolandığı aynı makinede yürütülmelidir:

    makecert ^
      -n "CN=My ID" ^
      -e MM/DD/YYYY ^
      -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.2" ^
      -a sha256 -len 2048 ^
      -in "MyCA" -ir localmachine -is my ^
      -sv MyID.pvk MyID.cer

Özelleştirme:

* -n bu sertifika ile kimlik doğrulaması olacak istemciiçin bir kimlik ile
* -e sertifikanın son kullanma tarihi ile
* MyID.pvk ve MyID.cer bu istemci sertifikası için benzersiz dosya adları ile

Bu komut, bir parolanın oluşturulmasını ve bir kez kullanılmasını ister. Güçlü bir parola kullanın.

## <a name="create-pfx-files-for-client-certificates"></a>İstemci sertifikaları için PFX dosyaları oluşturma
Oluşturulan her istemci sertifikası için şunları uygulayın:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Özelleştirme:

    MyID.pvk and MyID.cer with the filename for the client certificate

Aşağıdaki seçeneklerle parola girin ve ardından sertifika ihracını girin:

* Evet, özel anahtarı dışa aktarma
* Tüm genişletilmiş özellikleri dışa aktarma
* Bu sertifikanın verildiği kişi dışa aktarma parolasını seçmelidir

## <a name="import-client-certificate"></a>İstemci sertifikasını alma
İstemci sertifikası verilen her birey, hizmetle iletişim kurmak için kullanacakları makinelerdeki anahtar çiftini içe aktarmalıdır:

* Çift tıklayın. Windows Gezgini'nde PFX dosyası
* En az bu seçenekle Kişisel mağazaya sertifika alma:
  * Denetlenen tüm genişletilmiş özellikleri ekleme

## <a name="copy-client-certificate-thumbprints"></a>İstemci sertifikası parmak izlerini kopyalama
İstemci sertifikası verilen her birey, hizmet yapılandırma dosyasına eklenecek sertifikanın küçük izini almak için aşağıdaki adımları izlemelidir:

* Çalıştır ın certmgr.exe
* Kişisel sekmesini seçin
* Kimlik doğrulama için kullanılacak istemci sertifikasını çift tıklatın
* Açılan Sertifika iletişim kutusunda Ayrıntılar sekmesini seçin
* Göster'in Tümünü gösterdiğinden emin olun
* Listede Thumbprint adlı alanı seçin
* Parmak izinin değerini kopyalama
  * İlk basamak önünde görünmeyen Unicode karakterlerini silme
  * Tüm boşlukları silme

## <a name="configure-allowed-clients-in-the-service-configuration-file"></a>Hizmet yapılandırma dosyasında İzin Verilen istemcileri yapılandırma
Hizmet yapılandırma dosyasındaki aşağıdaki ayarın değerini, hizmete erişime izin verilen istemci sertifikalarının parmak izlerinin virgülle ayrılmış bir listesiyle güncelleştirin:

```xml
<Setting name="AllowedClientCertificateThumbprints" value="" />
```

## <a name="configure-client-certificate-revocation-check"></a>İstemci sertifikası iptal denetimini yapılandırma
Varsayılan ayar, istemci sertifikası iptal durumu için Sertifika Yetkilisi'ne iletmez. Çekleri açmak için, istemci sertifikalarını veren Sertifika Yetkilisi bu tür çekleri destekliyorsa, X509RevocationMode Numaralandırmasında tanımlanan değerlerden biriyle aşağıdaki ayarı değiştirin:

```xml
<Setting name="ClientCertificateRevocationCheck" value="NoCheck" />
```

## <a name="create-pfx-file-for-self-signed-encryption-certificates"></a>Kendi imzalı şifreleme sertifikaları için PFX dosyası oluşturma
Şifreleme sertifikası için şunları uygulayın:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Özelleştirme:

    MyID.pvk and MyID.cer with the filename for the encryption certificate

Aşağıdaki seçeneklerle parola girin ve ardından sertifika ihracını girin:

* Evet, özel anahtarı dışa aktarma
* Tüm genişletilmiş özellikleri dışa aktarma
* Sertifikayı bulut hizmetine yüklerken parolaya ihtiyacınız olacaktır.

## <a name="export-encryption-certificate-from-certificate-store"></a>Sertifika deposundan şifreleme sertifikası dışa aktarma
* Sertifika yı bulma
* Eylemler -tüm görevler> > Dışa Aktarma'yı tıklatın...
* Bir . Bu seçenekleri ile PFX dosyası: 
  * Evet, özel anahtarı dışa aktarma
  * Mümkünse tüm sertifikaları sertifika yoluna ekleme 
* Tüm genişletilmiş özellikleri dışa aktarma

## <a name="upload-encryption-certificate-to-cloud-service"></a>Şifreleme sertifikasını bulut hizmetine yükleme
Varolan veya oluşturulan sertifikayı yükleyin. Şifreleme anahtar çifti ile PFX dosyası:

* Özel anahtar bilgilerini koruyan parolayı girin

## <a name="update-encryption-certificate-in-service-configuration-file"></a>Hizmet yapılandırma dosyasında şifreleme sertifikasını güncelleştirme
Bulut hizmetine yüklenen sertifikanın parmak izi ile hizmet yapılandırma dosyasında aşağıdaki ayarların parmak izi değerini güncelleştirin:

```xml
<Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />
```

## <a name="common-certificate-operations"></a>Yaygın sertifika işlemleri
* SSL sertifikasını yapılandırma
* İstemci sertifikalarını yapılandırma

## <a name="find-certificate"></a>Sertifika yı bulma
Şu adımları uygulayın:

1. çalıştırın mmc.exe.
2. Dosya -> Ekle/Kaldır Snap-in...
3. **Sertifikaları**seçin.
4. **Ekle**’ye tıklayın.
5. Sertifika deposu konumunu seçin.
6. **Son**'a tıklayın.
7. **Tamam**'a tıklayın.
8. **Sertifikaları**Genişletin.
9. Sertifika deposu düğümini genişletin.
10. Sertifika alt düğüm'üne genişletin.
11. Listede bir sertifika seçin.

## <a name="export-certificate"></a>Sertifikayı dışarı aktarma
Sertifika **Verme**Sihirbazında:

1. **İleri**'ye tıklayın.
2. **Evet'i**seçin, ardından **özel anahtarı dışa aktarın.**
3. **İleri**'ye tıklayın.
4. İstenilen çıktı dosya biçimini seçin.
5. İstenilen seçenekleri kontrol edin.
6. **Parolayı Denetle.**
7. Güçlü bir parola girin ve onaylayın.
8. **İleri**'ye tıklayın.
9. Sertifikanın depolandığı bir dosya adı yazın veya göz atın (. PFX uzantısı).
10. **İleri**'ye tıklayın.
11. **Son**'a tıklayın.
12. **Tamam**'a tıklayın.

## <a name="import-certificate"></a>Sertifikayı içeri aktarma
Sertifika Alma Sihirbazında:

1. Mağaza konumunu seçin.
   
   * Yalnızca geçerli kullanıcı altında çalışan işlemler hizmete erişecekse **Geçerli Kullanıcı'yı** seçin
   * Bu bilgisayardaki diğer işlemler hizmete erişecekse **Yerel Makine'yi** seçin
2. **İleri**'ye tıklayın.
3. Bir dosyadan içe aktarıyorsanız, dosya yolunu onaylayın.
4. Bir . PFX dosyası:
   1. Özel anahtarı koruyan parolayı girin
   2. Alma seçeneklerini seçin
5. Aşağıdaki mağazada "Yer" sertifikalarını seçin
6. **Gözat**’a tıklayın.
7. İstediğin mağazayı seçin.
8. **Son**'a tıklayın.
   
   * Güvenilen Kök Sertifika Yetkilisi deposu seçildiyse, **Evet'i**tıklatın.
9. Tüm iletişim pencerelerinde **Tamam'ı** tıklatın.

## <a name="upload-certificate"></a>Sertifikayı karşıya yükleme
[Azure portalında](https://portal.azure.com/)

1. **Bulut Hizmetlerini**seçin.
2. Bulut hizmetini seçin.
3. Üst menüde **Sertifikalar'ı**tıklatın.
4. Alt takiben **Yükle'yi**tıklatın.
5. Sertifika dosyasını seçin.
6. Eğer bir . PFX dosyası, özel anahtarın şifresini girin.
7. Tamamlandıktan sonra, listedeki yeni girişten sertifika parmak izini kopyalayın.

## <a name="other-security-considerations"></a>Diğer güvenlik konuları
Bu belgede açıklanan SSL ayarları, HTTPS bitiş noktası kullanıldığında hizmet ve istemcileri arasındaki iletişimi şifreler. Veritabanı erişimi ve diğer hassas bilgiler iletişimde yer alabilmesi için bu önemlidir. Ancak, hizmetin Microsoft Azure aboneliğinizde meta veri depolaması için sağladığınız Microsoft Azure SQL veritabanındaki dahili tablolarında kimlik bilgileri de dahil olmak üzere dahili durumunu sürdürüldünü unutmayın. Bu veritabanı, hizmet yapılandırma dosyanızda aşağıdaki ayarın bir parçası olarak tanımlanmıştır (. CSCFG dosyası): 

```xml
<Setting name="ElasticScaleMetadata" value="Server=…" />
```

Bu veritabanında depolanan kimlik bilgileri şifrelenir. Ancak, en iyi uygulama olarak, her ikisi de meta veri veritabanına ve depolanan kimlik bilgilerinin şifresini çözmek için kullanılan sertifikaya erişebildiklerinden, hizmet dağıtımlarınızın hem web hem de çalışan rollerinin güncel ve güvenli olduğundan emin olun. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

