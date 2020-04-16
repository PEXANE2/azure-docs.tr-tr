---
title: Key Vault sertifikalarını kullanmaya başlama
description: Aşağıdaki senaryolar, anahtar kasanızda ilk sertifikanızı oluşturmak için gereken ek adımlar da dahil olmak üzere Key Vault'un sertifika yönetimi hizmetinin birincil kullanımlarından birkaçını ana hatlar.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: mbaldwin
ms.openlocfilehash: 5881314f0d3c62e7d6181ebd7bb27a5e0e87729a
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81431949"
---
# <a name="get-started-with-key-vault-certificates"></a>Key Vault sertifikalarını kullanmaya başlama
Aşağıdaki senaryolar, anahtar kasanızda ilk sertifikanızı oluşturmak için gereken ek adımlar da dahil olmak üzere Key Vault'un sertifika yönetimi hizmetinin birincil kullanımlarından birkaçını ana hatlar.

Aşağıda özetlenmiştir:
- İlk Key Vault sertifikanızı oluşturma
- Anahtar Vault ile ortak olan sertifika yetkilisi ile sertifika oluşturma
- Anahtar Vault ile ortak olmayan bir Sertifika Yetkilisi ile sertifika oluşturma
- Sertifika alma

## <a name="certificates-are-complex-objects"></a>Sertifikalar karmaşık nesnelerdir
Sertifikalar, Key Vault sertifikası olarak birbirine bağlı üç birbiriyle ilişkili kaynaktan oluşur; sertifika meta verileri, bir anahtar ve bir sır.


![Sertifikalar karmaşıktır](../media/azure-key-vault.png)


## <a name="creating-your-first-key-vault-certificate"></a>İlk Key Vault sertifikanızı oluşturma  
 Bir Anahtar Vault 'ta (KV) bir sertifika oluşturulabilmesi için önce, ön koşul adım 1 ve 2 başarıyla gerçekleştirilmiş olmalı ve bu kullanıcı / kuruluş için anahtar kasası bulunmalıdır.  

**Adım 1** - Sertifika Yetkilisi (CA) Sağlayıcıları  
-   Belirli bir şirket için BT Yöneticisi, PKI Yöneticisi veya C'lerle hesap yöneten herkes olarak biniş. Contoso) Key Vault sertifikalarını kullanmanın ön koşuludur.  
    Aşağıdaki KA'lar Key Vault ile mevcut ortak sağlayıcılar:  
    -   DigiCert - Key Vault, DigiCert ile OV TLS/SSL sertifikaları sunmaktadır.  
    -   GlobalSign - Key Vault, GlobalSign ile OV TLS/SSL sertifikaları sunmaktadır.  

**Adım 2** - CA sağlayıcısının hesap yöneticisi, Key Vault üzerinden TLS/SSL sertifikalarını kaydetmek, yenilemek ve kullanmak için Key Vault tarafından kullanılacak kimlik bilgileri oluşturur.

**Adım 3** - Contoso yöneticisi, CA'ya bağlı olarak sertifika sahibi bir Contoso çalışanı (Key Vault kullanıcısı) ile birlikte yöneticiden veya doğrudan CA hesabından sertifika alabilir.  

- Sertifika veren kaynak ayarlayarak anahtar kasasına kimlik bilgisi ekleme [işlemini](/rest/api/keyvault/setcertificateissuer/setcertificateissuer) başlatın. Sertifika veren kuruluş, Azure Key Vault'ta (KV) Sertifika Veren kaynağı olarak temsil edilen bir varlıktır. Bir KV sertifikasının kaynağı hakkında bilgi sağlamak için kullanılır; verenin adı, sağlayıcı, kimlik bilgileri ve diğer yönetim ayrıntıları.
  - Örn. MyDigiCertIssuer  
    -   Sağlayıcı  
    -   Kimlik bilgileri – CA hesap kimlik bilgileri. Her CA'nın kendine özgü verileri vardır.  

    CA Sağlayıcıları ile hesap oluşturma hakkında daha fazla bilgi için [Key Vault blogundaki](https://aka.ms/kvcertsblog)ilgili gönderiye bakın.  

**Adım 3.1** - Bildirimler için [sertifika kişileri](/rest/api/keyvault/setcertificatecontacts/setcertificatecontacts) ayarlayın. Bu Key Vault kullanıcıiçin temas olduğunu. Anahtar Vault bu adımı uygulamaz.  

Not - Bu işlem, adım 3.1 aracılığıyla, bir kerelik bir işlemdir.  

## <a name="creating-a-certificate-with-a-ca-partnered-with-key-vault"></a>Key Vault ile ortak bir CA ile sertifika oluşturma

![Key Vault ortak sertifika yetkilisi ne sahip bir sertifika oluşturma](../media/certificate-authority-2.png)

**Adım 4** - Aşağıdaki açıklamalar, önceki diyagramdaki yeşil numaralı adımlara karşılık gelir.  
  (1) - Yukarıdaki diyagramda, başvurunuz, anahtar kasanızda bir anahtar oluşturarak dahili olarak başlayan bir sertifika oluşturmaktır.  
  (2) - Key Vault CA'ya TLS/SSL Sertifika İsteği gönderir.  
  (3) - Sertifika tamamlama için Key Vault için bir döngü ve bekleme sürecinde, başvuru anketleri. Anahtar Kasası x509 sertifikasıyla CA yanıtını aldığında sertifika oluşturma işlemi tamamlanır.  
  (4) - CA, Key Vault'un TLS/SSL Sertifika Talebine X509 TLS/SSL Sertifikası ile cevap verir.  
  (5) - Yeni sertifika oluşturma ca için X509 Sertifikası birleşme ile tamamlar.  

  Key Vault kullanıcı - bir ilke belirterek bir sertifika oluşturur

  -   Gerektiği gibi tekrarlayın  
  -   İlke kısıtlamaları  
      -   X509 özellikleri  
      -   Temel özellikler  
      -   Sağlayıcı referans - > ex. MyDigiCertIssure  
      -   Yenileme bilgileri - > ex. Vade tarihinden 90 gün önce  

  - Sertifika oluşturma işlemi genellikle eşzamanlı bir işlemdir ve oluşturma sertifikası işleminin durumu için anahtar kasanızı yoklamayı içerir.  
[Sertifika işlemi alın](/rest/api/keyvault/getcertificateoperation/getcertificateoperation)  
      -   Durum: tamamlanmış, hata bilgileriyle başarısız olan veya iptal edilmiş  
      -   Oluşturulacak gecikme nedeniyle, bir iptal işlemi başlatılabilir. İptal etkili olabilir veya olmayabilir.  

## <a name="import-a-certificate"></a>Sertifika alma  
 Alternatif olarak - bir sertifika Key Vault - PFX veya PEM içine ithal edilebilir.  

 İthalat sertifikası – diskte pem veya PFX gerektirir ve özel bir anahtara sahip olur. 
-   Belirtmelisiniz: kasa adı ve sertifika adı (ilke isteğe bağlıdır)

-   PEM / PFX dosyaları, KV'nin ayrıştını ve sertifika ilkesini doldurmak için kullanabileceği öznitelikleri içerir. Sertifika ilkesi zaten belirtilmişse, KV PFX / PEM dosyasından gelen verileri eşleştirmeye çalışır.  

-   Alma işlemi son halini aldıktan sonra, sonraki işlemler yeni ilkeyi (yeni sürümler) kullanır.  

-   Başka işlem yoksa, Key Vault'un yaptığı ilk şey bir son kullanma bildirimi göndermektir. 

-   Ayrıca, kullanıcı alma sırasında işlevsel olan ancak alma sırasında hiçbir bilginin belirtilmeyen varsayılanları içeren ilkeyi de edinebilir. Örn. veren bilgi yok  

### <a name="formats-of-import-we-support"></a>Desteklediğimiz İthalat Biçimleri
PEM dosya biçimi için aşağıdaki Alma türünü destekliyoruz. PKCS#8 kodlanmış, şifresiz bir anahtarla birlikte tek bir PEM kodlanmış sertifika aşağıdaki

-----BAŞLATSERTIFIKA----- -----SON SERTIFIKA-----

-----BEGIN ÖZEL ANAHTAR----- -----END ÖZEL ANAHTAR-----

Sertifika birleştirme de 2 PEM tabanlı biçimleri destekliyoruz. Tek bir PKCS#8 kodlanmış sertifikayı veya base64 kodlanmış bir P7B dosyasini birleştirebilirsiniz. -----BAŞLATSERTIFIKA----- -----SON SERTIFIKA-----

Şu anda PEM formatında AT tuşlarını destekliyoruz.

## <a name="creating-a-certificate-with-a-ca-not-partnered-with-key-vault"></a>Key Vault ile ortak olmayan bir CA ile sertifika oluşturma  
 Bu yöntem, Key Vault'un ortak sağlayıcılarından başka diğer CA'larla çalışmaya olanak tanır, bu da kuruluşunuzun kendi seçtiği bir CA ile çalışabileceği anlamına gelir.  

![Kendi sertifika yetkinizle sertifika oluşturma](../media/certificate-authority-1.png)  

 Aşağıdaki adım açıklamaları, önceki diyagramdaki yeşil harfli adımlara karşılık gelir.  

  (1) - Yukarıdaki diyagramda, başvurunuz, anahtar kasanızda bir anahtar oluşturarak dahili olarak başlayan bir sertifika oluşturuyor.  

  (2) - Key Vault başvurunuza sertifika imzalama isteği (CSR) iade eder.  

  (3) - Başvurunuz KSS'yi seçtiğiniz CA'ya geçirir.  

  (4) - Seçtiğiniz CA bir X509 Sertifikası ile yanıt verir.  

  (5) - Başvurunuz, ca'nızdan X509 Sertifikası'nın birleşmesi ile yeni sertifika oluşturmayı tamamlar.

