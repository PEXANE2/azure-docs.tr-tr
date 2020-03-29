---
title: Şifreleme - Microsoft Tehdit Modelleme Aracı - Azure | Microsoft Dokümanlar
description: Tehdit Modelleme Aracı'nda açığa çıkan tehditler için azaltıcı etkenler
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.subservice: security-develop
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: jegeib
ms.openlocfilehash: c9116472af5b400ded0fea24f98b07bad9d9039b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68728198"
---
# <a name="security-frame-cryptography--mitigations"></a>Güvenlik Çerçevesi: Şifreleme | Azaltıcı etken 

| Ürün / Hizmet | Makale |
| --------------- | ------- |
| **Web Uygulaması** | <ul><li>[Yalnızca onaylanmış simetrik blok şifrelerini ve anahtar uzunluklarını kullanma](#cipher-length)</li><li>[Simetrik şifreler için onaylı blok şifreleme modlarını ve başlatma vektörlerini kullanma](#vector-ciphers)</li><li>[Onaylı asimetrik algoritmalar, anahtar uzunlukları ve dolgu](#padding)</li><li>[Onaylı rasgele sayı üreteçleri kullanma](#numgen)</li><li>[Simetrik akış şifreleri kullanmayın](#stream-ciphers)</li><li>[Onaylı MAC/HMAC/anahtarlı karma algoritmaları kullanma](#mac-hash)</li><li>[Yalnızca onaylı şifreleme karma işlevlerini kullanma](#hash-functions)</li></ul> |
| **Database** | <ul><li>[Veritabanındaki verileri şifrelemek için güçlü şifreleme algoritmaları kullanın](#strong-db)</li><li>[SSIS paketleri şifrelenmeli ve dijital olarak imzalanmalıdır](#ssis-signed)</li><li>[Kritik veritabanı securables dijital imza ekleme](#securables-db)</li><li>[Şifreleme anahtarlarını korumak için SQL server EKM'i kullanın](#ekm-keys)</li><li>[Şifreleme anahtarları Veritabanı altyapısına açıklanmazsa AlwaysEncrypted özelliğini kullanın](#keys-engine)</li></ul> |
| **IoT Cihazı** | <ul><li>[Şifreleme Tuşlarını IoT Aygıtında güvenli bir şekilde saklayın](#keys-iot)</li></ul> | 
| **IoT Bulut Ağ Geçidi** | <ul><li>[IoT Hub'a kimlik doğrulama için yeterli uzunlukta rastgele bir simetrik anahtar oluşturma](#random-hub)</li></ul> | 
| **Dynamics CRM Mobil İstemci** | <ul><li>[Bir aygıt yönetim ilkesinin, kullanım PIN'i gerektiren ve uzaktan silmeye izin veren bir sistem olduğundan emin olun](#pin-remote)</li></ul> | 
| **Dynamics CRM Outlook İstemci** | <ul><li>[PIN/parola/otomatik kilit gerektiren ve tüm verileri şifreleyen bir cihaz yönetim ilkesinin yerinde olduğundan emin olun (örn. BitLocker)](#bitlocker)</li></ul> | 
| **Kimlik Sunucusu** | <ul><li>[Identity Server'ı kullanırken imzalama tuşlarının üzerinde olduğundan emin olun](#rolled-server)</li><li>[Identity Server'da şifreleme açısından güçlü istemci kimliğinin, istemci sırrının kullanıldığından emin olun](#client-server)</li></ul> | 

## <a name="use-only-approved-symmetric-block-ciphers-and-key-lengths"></a><a id="cipher-length"></a>Yalnızca onaylanmış simetrik blok şifrelerini ve anahtar uzunluklarını kullanma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | <p>Ürünler, yalnızca kuruluşunuzdaki Kripto Danışman tarafından açıkça onaylanan simetrik blok şifrelerini ve ilişkili anahtar uzunluklarını kullanmalıdır. Microsoft'ta onaylanan simetrik algoritmalar aşağıdaki blok şifrelerini içerir:</p><ul><li>Yeni kod AES-128, AES-192 ve AES-256 kabul edilebilir</li><li>Varolan kodla geriye dönük uyumluluk için, üç anahtarlı 3DES kabul edilebilir</li><li>Simetrik blok şifreleri kullanan ürünler için:<ul><li>Yeni kod için Gelişmiş Şifreleme Standardı (AES) gereklidir</li><li>Üç anahtarlı üçlü Veri Şifreleme Standardı (3DES), geriye dönük uyumluluk için varolan kodda izin verilir</li><li>RC2, DES, 2 Key 3DES, DESX ve Skipjack dahil olmak üzere diğer tüm blok şifreleri yalnızca eski verilerin şifresini çözmek için kullanılabilir ve şifreleme için kullanılırsa değiştirilmelidir</li></ul></li><li>Simetrik blok şifreleme algoritmaları için en az 128 bit anahtar uzunluğu gereklidir. Yeni kod için önerilen tek blok şifreleme algoritması AES'dir (AES-128, AES-192 ve AES-256 kabul edilebilir)</li><li>Üç anahtarlı 3DES şu anda mevcut kodda zaten kullanılıyorsa kabul edilebilir; AES'ye geçiş önerilir. DES, DESX, RC2 ve Skipjack artık güvenli olarak kabul edilememaktadır. Bu algoritmalar yalnızca geriye dönük uyumluluk amacıyla varolan verilerin şifresini çözmek için kullanılabilir ve veriler önerilen blok şifresi kullanılarak yeniden şifrelenmelidir</li></ul><p>Tüm simetrik blok şifrelerinin uygun bir başlatma vektörü (IV) kullanılmasını gerektiren onaylanmış bir şifreleme modu ile kullanılması gerektiğini lütfen unutmayın. Uygun bir IV, genellikle rasgele bir sayıdır ve hiçbir zaman sabit bir değer</p><p>Kuruluşunuzun Kripto Kurulu incelemesinden sonra eski veya onaylanmamış kripto algoritmalarının ve varolan verileri okumak için daha küçük anahtar uzunluklarının (yeni veri yazmanın aksine) kullanılmasına izin verilebilir. Ancak, bu gereksinime karşı bir özel durum için başvuruda bulunmanız gerekir. Ayrıca, kurumsal dağıtımlarda, verileri okumak için zayıf kripto kullanıldığında ürünler uyarı yöneticileri dikkate almalıdır. Bu tür uyarılar açıklayıcı ve uygulanabilir olmalıdır. Bazı durumlarda, Grup İlkesi'nin zayıf kripto kullanımını kontrol etmesi uygun olabilir</p><p>Yönetilen kripto çevikliği için izin verilen .NET algoritmaları (tercih sırasına göre)</p><ul><li>AesCng (FIPS uyumlu)</li><li>AuthenticatedAesCng (FIPS uyumlu)</li><li>AESCryptoServiceProvider (FIPS uyumlu)</li><li>AESManaged (FIPS uyumlu olmayan)</li></ul><p>Lütfen bu algoritmaların hiçbirinin machine.config dosyasında değişiklik yapmadan `SymmetricAlgorithm.Create` veya `CryptoConfig.CreateFromName` yöntemlerle belirtilemediğini unutmayın. Ayrıca, .NET 3.5'ten önce .NET sürümlerindeki `RijndaelManaged`AES'nin adının geçtiğini ve `AesCng` CodePlex üzerinden >kullanılabildiğini ve `AuthenticatedAesCng` temel işletim sistemi içinde CNG gerektirdiğini unutmayın</p>

## <a name="use-approved-block-cipher-modes-and-initialization-vectors-for-symmetric-ciphers"></a><a id="vector-ciphers"></a>Simetrik şifreler için onaylı blok şifreleme modlarını ve başlatma vektörlerini kullanma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Tüm simetrik blok şifreleri onaylanmış bir simetrik şifreleme modu ile kullanılmalıdır. Onaylanan tek modcbc ve CTS vardır. Özellikle, elektronik kod defteri (ECB) çalışma modu kaçınılmalıdır; ECB'nin kullanımı için kuruluşunuzun Kripto Kurulu incelemesi gerekmektedir. OFB, CFB, TO, CCM ve GCM veya diğer şifreleme modlarının tüm kullanımı kuruluşunuzun Kripto Kurulu tarafından gözden geçirilmelidir. Aynı başlatma vektörü (IV) to gibi "akış şifreleme modlarında" blok şifreleri ile yeniden kullanmak, şifreli verilerin açığa çıkarılmasına neden olabilir. Tüm simetrik blok şifreleri de uygun bir başlatma vektörü (IV) ile kullanılmalıdır. Uygun bir IV, kriptografik olarak güçlü, rastgele bir sayıdır ve asla sabit bir değer değildir. |

## <a name="use-approved-asymmetric-algorithms-key-lengths-and-padding"></a><a id="padding"></a>Onaylı asimetrik algoritmalar, anahtar uzunlukları ve dolgu

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | <p>Yasaklı şifreleme algoritmalarının kullanımı ürün güvenliği için önemli risk ler oluşturur ve kaçınılmalıdır. Ürünler yalnızca kuruluşunuzun Kripto Kurulu tarafından açıkça onaylanan şifreleme algoritmalarını ve ilişkili anahtar uzunluklarını ve dolgularını kullanmalıdır.</p><ul><li>**RSA-** şifreleme, anahtar değişimi ve imza için kullanılabilir. RSA şifreleme yalnızca OAEP veya RSA-KEM doldurma modlarını kullanmalıdır. Varolan kod, yalnızca uyumluluk için PKCS #1 v1.5 doldurma modunu kullanabilir. Null dolgu kullanımı açıkça yasaklanmıştır. Yeni kod için >= 2048 bit anahtarları gereklidir. Varolan kod, 2048 bit< anahtarları yalnızca kuruluşunuzun Kripto Kurulu tarafından yapılan bir incelemeden sonra geriye dönük uyumluluk için destekleyebilir. 1024 bit< tuşları yalnızca eski verilerin şifresini çözmek/doğrulamak için kullanılabilir ve şifreleme veya imzalama işlemleri için kullanılırsa değiştirilmelidir</li><li>**ECDSA-** sadece imza için kullanılabilir. Yeni kod için >=256 bit tuşlu ECDSA gereklidir. ECDSA tabanlı imzalar, NIST onaylı üç eğriden birini (P-256, P-384 veya P521) kullanmalıdır. Ayrıntılı olarak analiz edilmiş eğriler, ancak kuruluşunuzun Kripto Kurulu ile yapılan bir incelemeden sonra kullanılabilir.</li><li>**ECDH-** sadece anahtar değişimi için kullanılabilir. Yeni kod için >=256 bit anahtarlı ECDH gereklidir. ECDH tabanlı anahtar değişimi, NIST onaylı üç eğriden birini (P-256, P-384 veya P521) kullanmalıdır. Ayrıntılı olarak analiz edilmiş eğriler, ancak kuruluşunuzun Kripto Kurulu ile yapılan bir incelemeden sonra kullanılabilir.</li><li>**DSA-** kuruluşunuzun Kripto Kurulu'nun incelemesi ve onayı sonrasında kabul edilebilir. Kuruluşunuzun Kripto Yönetim Kurulu incelemesini planlamak için güvenlik danışmanınıza başvurun. DSA kullanımınız onaylanırsa, 2048 bit'ten daha kısa uzunlukta anahtarların kullanılmasını yasaklamanız gerekeceğini unutmayın. CNG, Windows 8 itibariyle 2048 bit ve daha büyük anahtar uzunluklarını destekler.</li><li>**Diffie-Hellman-** yalnızca oturum anahtar yönetimi için kullanılabilir. Anahtar uzunluğu >= 2048 bit yeni kod için gereklidir. Varolan kod, kuruluşunuzun Kripto Kurulu tarafından yapılan bir incelemeden sonra yalnızca geriye dönük uyumluluk için 2048 bit< anahtar uzunluklarını destekleyebilir. 1024 bit < tuşları kullanılmayabilir.</li><ul>

## <a name="use-approved-random-number-generators"></a><a id="numgen"></a>Onaylı rasgele sayı üreteçleri kullanma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | <p>Ürünler onaylı rasgele sayı jeneratörleri kullanmalıdır. C çalışma zamanı işlevi rand, .NET Framework class System.Random veya GetTickCount gibi sistem işlevleri gibi pseudorandom işlevleri, bu nedenle, bu tür kod asla kullanılmamalıdır. Çift eliptik eğri rastgele sayı üreteci (DUAL_EC_DRBG) algoritmasının kullanılması yasaktır</p><ul><li>**CNG-** BCryptGenRandom (arayan 0 'dan büyük herhangi bir IRQL [yani, PASSIVE_LEVEL]) çalıştırabilirsiniz sürece önerilen BCRYPT_USE_SYSTEM_PREFERRED_RNG bayrağı nın kullanımı)</li><li>**CAPI-** cryptGenRandom</li><li>**Win32/64-** RtlGenRandom (yeni uygulamalar BCryptGenRandom veya CryptGenRandom kullanmanız gerekir) * rand_s * SystemPrng (çekirdek modu için)</li><li>**. NET-** RNGCryptoServiceProvider veya RNGCng</li><li>**Windows Mağazası Uygulamaları-** Windows.Security.Cryptography.CryptographicBuffer.GenerateRandom veya . RandomNumber oluşturma</li><li>**Apple OS X (10.7+)/iOS(2.0+)-** int SecRandomCopyBytes (SecRandomRef \*rastgele, size_t sayısı, uint8_t baytlar)</li><li>**Apple OS X (<10.7)-** Rastgele sayıları almak için /dev/random'u kullanma</li><li>**Java (Google Android Java kodu dahil)-** java.security.SecureRandom sınıfı. Android 4.3 (Jelly Bean) için geliştiricilerin Android önerilen geçici çözüme uyması ve PRNG'yi /dev/urandom veya /dev/random adresinden entropi ile açıkça başlatması için uygulamalarını güncellemesi gerektiğini unutmayın</li></ul>|

## <a name="do-not-use-symmetric-stream-ciphers"></a><a id="stream-ciphers"></a>Simetrik akış şifreleri kullanmayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | RC4 gibi simetrik akış şifreleri kullanılmamalıdır. Ürünler, simetrik akış şifreleri yerine, özellikle en az 128 bit anahtar uzunluğuna sahip AES olmak üzere bir blok şifresi kullanmalıdır. |

## <a name="use-approved-machmackeyed-hash-algorithms"></a><a id="mac-hash"></a>Onaylı MAC/HMAC/anahtarlı karma algoritmaları kullanma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | <p>Ürünler yalnızca onaylı ileti kimlik doğrulama kodu (MAC) veya karma tabanlı ileti kimlik doğrulama kodu (HMAC) algoritmaları kullanmalıdır.</p><p>İleti kimlik doğrulama kodu (MAC), alıcının gizli bir anahtar kullanarak hem gönderenin orijinalliğini hem de iletinin bütünlüğünü doğrulamasına olanak tanıyan bir iletiye iliştirilmiş bir bilgi dir. Karma tabanlı MAC[(HMAC)](https://csrc.nist.gov/publications/nistpubs/800-107-rev1/sp800-107-rev1.pdf)veya blok [şifreleme tabanlı MAC'in](https://csrc.nist.gov/publications/nistpubs/800-38B/SP_800-38B.pdf) kullanımı, altta yatan tüm karma veya simetrik şifreleme algoritmaları da kullanım için onaylandığı sürece izin verilir; Şu anda bu HMAC-SHA2 fonksiyonları (HMAC-SHA256, HMAC-SHA384 ve HMAC-SHA512) ve CMAC/OMAC1 ve OMAC2 blok şifreleme tabanlı MaCs (bu AES dayanmaktadır) içerir.</p><p>HMAC-SHA1'in kullanımı platform uyumluluğu için caiz olabilir, ancak bu yordamiçin bir istisna dosyalamanız ve kuruluşunuzun Crypto incelemesinden geçmeniz gerekecektir. HMAK'ların 128 bitten az kesilmesine izin verilmez. Bir anahtarı ele geçirmek için müşteri yöntemlerini kullanmak ve veriler onaylanmaz ve kullanmadan önce kuruluşunuzun Kripto Yönetim Kurulu incelemesinden geçmelidir.</p>|

## <a name="use-only-approved-cryptographic-hash-functions"></a><a id="hash-functions"></a>Yalnızca onaylı şifreleme karma işlevlerini kullanma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | <p>Ürünler SHA-2 karma algoritmaları (SHA256, SHA384 ve SHA512) ailesini kullanmalıdır. Daha kısa bir karma gerekirse, örneğin daha kısa MD5 karma göz önünde bulundurularak tasarlanmış bir veri yapısına sığdırmak için 128 bit çıkış uzunluğu gerekiyorsa, ürün ekipleri SHA2 karmalarından birini (genellikle SHA256) azaltabilir. SHA384 SHA512 kesilmiş bir sürümüdür unutmayın. Güvenlik amacıyla şifreleme karmalarının 128 bitten az olmasına izin verilmez. Yeni kod MD2, MD4, MD5, SHA-0, SHA-1 veya RIPEMD karma algoritmalarını kullanmamalıdır. Karma çarpışmalar, bu algoritmalar için hesaplama açısından uygulanabilir ve bu da onları etkili bir şekilde bozar.</p><p>Yönetilen kripto çevikliği için izin verilen .NET karma algoritmaları (tercih sırasına göre):</p><ul><li>SHA512Cng (FIPS uyumlu)</li><li>SHA384Cng (FIPS uyumlu)</li><li>SHA256Cng (FIPS uyumlu)</li><li>SHA512Managed (FIPS uyumlu olmayan) (HashAlgorithm.Create veya CryptoConfig.CreateFromName aramalarında algoritma adı olarak SHA512 kullanın)</li><li>SHA384Managed (FIPS uyumlu olmayan) (HashAlgorithm.Create veya CryptoConfig.CreateFromName aramalarında algoritma adı olarak SHA384 kullanın)</li><li>SHA256Managed (FIPS uyumlu olmayan) (HashAlgorithm.Create veya CryptoConfig.CreateFromName aramalarında algoritma adı olarak SHA256 kullanın)</li><li>SHA512CryptoServiceProvider (FIPS uyumlu)</li><li>SHA256CryptoServiceProvider (FIPS uyumlu)</li><li>SHA384CryptoServiceProvider (FIPS uyumlu)</li></ul>| 

## <a name="use-strong-encryption-algorithms-to-encrypt-data-in-the-database"></a><a id="strong-db"></a>Veritabanındaki verileri şifrelemek için güçlü şifreleme algoritmaları kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Şifreleme algoritması seçme](https://technet.microsoft.com/library/ms345262(v=sql.130).aspx) |
| **Adımlar** | Şifreleme algoritmaları, yetkisiz kullanıcılar tarafından kolayca geri döndürülemeyen veri dönüşümlerini tanımlar. SQL Server, yöneticilerin ve geliştiricilerin DES, Triple DES, TRIPLE_DES_3KEY, RC2, RC4, 128-bit RC4, DESX, 128-bit AES, 192-bit AES ve 256-bit AES gibi çeşitli algoritmalar arasından seçim yapmasına olanak tanır |

## <a name="ssis-packages-should-be-encrypted-and-digitally-signed"></a><a id="ssis-signed"></a>SSIS paketleri şifrelenmeli ve dijital olarak imzalanmalıdır

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Dijital İmzalı Paketlerin Kaynağını,](https://msdn.microsoft.com/library/ms141174.aspx) [Tehdit ve Güvenlik Açığı azaltmayı (Entegrasyon Hizmetleri)](https://msdn.microsoft.com/library/bb522559.aspx) belirleme |
| **Adımlar** | Paketin kaynağı, paketi oluşturan kişi veya kuruluştur. Bilinmeyen veya güvenilmeyen bir kaynaktan paket çalıştırmak riskli olabilir. SSIS paketlerinin yetkisiz olarak tahrif edilebilmiş olmasını önlemek için dijital imzalar kullanılmalıdır. Ayrıca, depolama/taşıma sırasında paketlerin gizliliğini sağlamak için, SSIS paketlerinin şifrelenmeleri gerekmektedir |

## <a name="add-digital-signature-to-critical-database-securables"></a><a id="securables-db"></a>Kritik veritabanı securables dijital imza ekleme

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [İmZA EKLE (Transact-SQL)](https://msdn.microsoft.com/library/ms181700) |
| **Adımlar** | Kritik bir veritabanının bütünlüğünün doğrulanmış olması gereken durumlarda, dijital imzalar kullanılmalıdır. Depolanan yordam, işlev, derleme veya tetikleyici gibi veritabanı securables dijital olarak imzalanabilir. Aşağıda bunun ne zaman yararlı olabileceğine ilişkin bir örnek verilmiştir: Bir ISV'nin (Bağımsız Yazılım Satıcısı) müşterilerinden birine teslim edilen bir yazılıma destek sağladığını varsayalım. Destek sağlamadan önce ISV, yazılımda tedavi edilebilir bir veritabanının yanlışlıkla veya kötü amaçlı bir girişimle değiştirilmediğinden emin olmak ister. Securable dijital olarak imzalanırsa, ISV dijital imzasını doğrulayabilir ve bütünlüğünü doğrulayabilir.| 

## <a name="use-sql-server-ekm-to-protect-encryption-keys"></a><a id="ekm-keys"></a>Şifreleme anahtarlarını korumak için SQL server EKM'i kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [SQL Server Genişletilebilir Anahtar Yönetimi (EKM)](https://msdn.microsoft.com/library/bb895340), [Azure Key Vault (SQL Server) kullanarak Genişletilebilir Anahtar Yönetimi](https://msdn.microsoft.com/library/dn198405) |
| **Adımlar** | SQL Server Genişletilebilir Anahtar Yönetimi, veritabanı dosyalarını koruyan şifreleme anahtarlarının akıllı kart, USB aygıtı veya EKM/HSM modülü gibi kapalı kutu aygıtında depolanmasını sağlar. Bu, veritabanı yöneticilerine (sysadmin grubunun üyeleri hariç) veri koruması da sağlar. Veriler, yalnızca veritabanı kullanıcısının harici EKM/HSM modülünde erişebildiği şifreleme anahtarları kullanılarak şifrelenebilir. |

## <a name="use-alwaysencrypted-feature-if-encryption-keys-should-not-be-revealed-to-database-engine"></a><a id="keys-engine"></a>Şifreleme anahtarları Veritabanı altyapısına açıklanmazsa AlwaysEncrypted özelliğini kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | SQL Azure, OnPrem |
| **Öznitelikler**              | SQL Sürüm - V12, MsSQL2016 |
| **Başvurular**              | [Her Zaman Şifrelenmiş (Veritabanı Motoru)](https://msdn.microsoft.com/library/mt163865) |
| **Adımlar** | Her Zaman Şifrelenmiş, Azure SQL Veritabanı veya SQL Server veritabanlarında saklanan kredi kartı numaraları veya ulusal kimlik numaraları (örneğin ABD sosyal güvenlik numaraları) gibi hassas verileri korumak için tasarlanmış bir özelliktir. Her Zaman Şifrelenmiş istemcilerin istemci uygulamaları içinde hassas verileri şifrelemek ve Veritabanı Motoru (SQL Database veya SQL Server) için şifreleme anahtarlarını asla ortaya sağlar. Sonuç olarak, Her Zaman Şifrelenmiş, verilere sahip olanlar (ve görüntüleyebilir) ile verileri yönetenler (ancak erişimi olmaması gerekenler) arasında bir ayrım sağlar |

## <a name="store-cryptographic-keys-securely-on-iot-device"></a><a id="keys-iot"></a>Şifreleme Tuşlarını IoT Aygıtında güvenli bir şekilde saklayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IoT Cihazı | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Aygıt İşletim Sistemi - Windows IoT Core, Aygıt Bağlantısı - Azure IoT aygıt SDK'ları |
| **Başvurular**              | [Windows IoT Core'da TPM](https://developer.microsoft.com/windows/iot/docs/tpm), [Windows IoT Core'da TPM'yi ayarlama](https://docs.microsoft.com/windows/iot-core/secure-your-device/setuptpm), [Azure IoT Cihazı SDK TPM](https://github.com/Azure/azure-iot-hub-vs-cs/wiki/Device-Provisioning-with-TPM) |
| **Adımlar** | TPM veya Smart Card yongaları gibi donanım korumalı bir depolama alanında simetrik veya Sertifika Özel tuşları güvenli bir şekilde. Windows 10 IoT Core bir TPM kullanıcısını destekler ve kullanılabilecek birkaç https://docs.microsoft.com/windows/iot-core/secure-your-device/tpm#discrete-tpm-dtpmuyumlu TPM vardır: . Firmware veya Ayrık TPM kullanılması önerilir. Bir Yazılım TPM'si yalnızca geliştirme ve sınama amacıyla kullanılmalıdır. Bir TPM kullanılabilir ve anahtarlar içinde sağlanmış bir kez, belirteci oluşturan kod, içinde hassas bilgiler sabit kodlamadan yazılmalıdır. | 

### <a name="example"></a>Örnek
```
TpmDevice myDevice = new TpmDevice(0);
// Use logical device 0 on the TPM 
string hubUri = myDevice.GetHostName(); 
string deviceId = myDevice.GetDeviceId(); 
string sasToken = myDevice.GetSASToken(); 

var deviceClient = DeviceClient.Create( hubUri, AuthenticationMethodFactory. CreateAuthenticationWithToken(deviceId, sasToken), TransportType.Amqp); 
```
Görüldüğü gibi, aygıt birincil anahtarı kodda bulunmaz. Bunun yerine, 0 yuvasında TPM'de depolanır. TPM aygıtı, daha sonra IoT Hub'ına bağlanmak için kullanılan kısa ömürlü bir SAS belirteci oluşturur. 

## <a name="generate-a-random-symmetric-key-of-sufficient-length-for-authentication-to-iot-hub"></a><a id="random-hub"></a>IoT Hub'a kimlik doğrulama için yeterli uzunlukta rastgele bir simetrik anahtar oluşturma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IoT Bulut Ağ Geçidi | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Ağ geçidi seçimi - Azure IoT Hub |
| **Başvurular**              | Yok  |
| **Adımlar** | IoT Hub bir aygıt Kimlik Kaydı içerir ve bir aygıtı sağlarken otomatik olarak rasgele bir Simetrik anahtar oluşturur. Kimlik doğrulama için kullanılan anahtarı oluşturmak için Azure IoT Hub Kimlik Kayıt Defteri'nin bu özelliğini kullanmanız önerilir. IoT Hub, aygıtı oluştururken bir anahtarın belirtilmesine de izin verir. Aygıt sağlama sırasında IoT Hub dışında bir anahtar oluşturulursa, rasgele bir simetrik anahtar veya en az 256 bit oluşturulması önerilir. |

## <a name="ensure-a-device-management-policy-is-in-place-that-requires-a-use-pin-and-allows-remote-wiping"></a><a id="pin-remote"></a>Bir aygıt yönetim ilkesinin, kullanım PIN'i gerektiren ve uzaktan silmeye izin veren bir sistem olduğundan emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Dynamics CRM Mobil İstemci | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | Bir aygıt yönetim ilkesinin, kullanım PIN'i gerektiren ve uzaktan silmeye izin veren bir sistem olduğundan emin olun |

## <a name="ensure-a-device-management-policy-is-in-place-that-requires-a-pinpasswordauto-lock-and-encrypts-all-data-eg-bitlocker"></a><a id="bitlocker"></a>PIN/parola/otomatik kilit gerektiren ve tüm verileri şifreleyen bir cihaz yönetim ilkesinin yerinde olduğundan emin olun (örn. BitLocker)

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Dynamics CRM Outlook İstemci | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | PIN/parola/otomatik kilit gerektiren ve tüm verileri şifreleyen bir cihaz yönetim ilkesinin yerinde olduğundan emin olun (örn. BitLocker) |

## <a name="ensure-that-signing-keys-are-rolled-over-when-using-identity-server"></a><a id="rolled-server"></a>Identity Server'ı kullanırken imzalama tuşlarının üzerinde olduğundan emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Kimlik Sunucusu | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Identity Server - Anahtarlar, İmzalar ve Şifreleme](https://identityserver.github.io/Documentation/docsv2/configuration/crypto.html) |
| **Adımlar** | Identity Server'ı kullanırken imzalama anahtarlarının devrildiğinden emin olun. Başvurular bölümündeki bağlantı, Identity Server'a dayanan uygulamalarda kesintilere neden olmadan bunun nasıl planlanması gerektiğini açıklar. |

## <a name="ensure-that-cryptographically-strong-client-id-client-secret-are-used-in-identity-server"></a><a id="client-server"></a>Identity Server'da şifreleme açısından güçlü istemci kimliğinin, istemci sırrının kullanıldığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Kimlik Sunucusu | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | <p>Identity Server'da şifreleme açısından güçlü istemci kimliği, istemci sırrı kullanıldığından emin olun. Aşağıdaki yönergeler istemci kimliği ve gizli oluştururken kullanılmalıdır:</p><ul><li>İstemci kimliği olarak rasgele bir GUID oluşturma</li><li>Gizli olarak şifreleme rastgele 256 bit lik bir anahtar oluşturma</li></ul>|
