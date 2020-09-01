---
title: Azure kanıtlama talep kümeleri
description: Azure kanıtlama talep kümesi.
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: 555dccbd3c2dfe61bac5891514deface6f8a877d
ms.sourcegitcommit: d68c72e120bdd610bb6304dad503d3ea89a1f0f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89237359"
---
# <a name="claim-sets"></a>Talep kümeleri

Microsoft Azure kanıtlama kullanan kuşışları ele geçirme sürecinde oluşturulan talepler aşağıdaki kategorilere ayrılabilir:

- **Gelen talepler**: kanıtlama bulgusu ayrıştırıldıktan sonra Microsoft Azure kanıtlama tarafından oluşturulan talepler.

- **Giden talepler**: Azure kanıtlama tarafından çıkış olarak oluşturulan talepler. Kanıtlama belirtecinde bitmesi gereken tüm talepleri içerir.

- **Özellik talepleri**: Azure kanıtlama tarafından çıkış olarak oluşturulan talepler. Rapor kodlaması, raporun geçerlilik süresi vb. gibi kanıtlama belirtecinin özelliklerini temsil eden tüm talepleri içerir.

JWT RFC tarafından tanımlanan ve yanıt nesnesinde Azure kanıtlama tarafından kullanılan talepler aşağıda verilmiştir:

- **"ISS" (veren) talebi**: "ISS" (veren) TALEBI, JWT veren sorumluyu tanımlar. Bu talebin işlenmesi genellikle uygulamaya özeldir. "ISS" değeri, bir StringOrURI değeri içeren büyük küçük harfe duyarlı bir dizedir.
- **"IAT" (çıkarılan) talebi**: "iat" (çıkarılan) TALEBI, JWT 'ın verildiği saati tanımlar. Bu talep, JWT yaşını belirlemede kullanılabilir. Değeri, bir NumericDate değeri içeren bir sayı OLMALıDıR.
- **"exp" (sona erme saati) talebi**: "exp" (sona erme saati) TALEBI, JWT 'ın işlenmek üzere kabul edilmemelidir. "Exp" talebinin işlenmesi için geçerli tarih/saat, "exp" talebinde listelenen bitiş tarihi/saatinden önce OLMALıDıR.

  Note: Saat çarpıklık hesabına 5 dakikalık bir esneklik için sorun (IAT) eklenmiştir.
- **"NBF" (önüne değil) talebi**: "NBF" (before) TALEBI, JWT 'ın işlenmek üzere kabul edilmeden önce geçen süreyi tanımlar. "NBF" talebinin işlenmesi için geçerli tarih/saatin, "NBF" talebinde listelenen son tarih/saat değerinden önce veya buna eşit olması gerekır.
  Note: Saat çarpıklık hesabına 5 dakikalık bir esneklik için sorun (IAT) eklenmiştir.

## <a name="claims-issued-by-azure-attestation-in-sgx-enclaves"></a>Azure kanıtlama tarafından SGX şifreli zamanlarda verilen talepler

### <a name="incoming-claims"></a>Gelen talepler 

- **$is-hata ayıklanabilir**: kuşın hata ayıklamanın etkin olup olmadığını gösteren bir Boole değeri
- **$SGX-mrimzalayan**: teklifin "mrimzalayan" alanının onaltılık kodlanmış değeri
- **$SGX-mrenclave**: teklifin "mrenclave" alanının onaltılık kodlu değeri
- **$product kimliği**
- **$svn**: teklifte kodlanmış güvenlik sürüm numarası 
- **$tee**: şifreleme türü 

### <a name="outgoing-claims"></a>Giden talepler

- **,-hata ayıklanabilir**: kuşın hata ayıklamanın etkin olup olmadığını gösteren bir Boole değeri
- **SGX-mrimzalayan**: teklifin "mrimzalayan" alanının onaltılık kodlanmış değeri
- **SGX-mrenclave**: teklifin "mrenclave" alanının onaltılık kodlu değeri
- **ürün kimliği**
- **svn**: teklifte kodlanan güvenlik sürüm numarası 
- **t**: şifreleme türü 
- **Maa-EHD**: kanıtlama isteğinde belirtilen "şifreli verileri tutulan verilerin" Base64Url kodlu sürümü 
- **AAS-EHD**: kanıtlama isteğinde belirtilen "Kuşve tutulan verilerin" kodlanmış sürümü Base64Url 

## <a name="claims-issued-by-azure-attestation-in-vbs-enclaves"></a>Azure kanıtlama tarafından verilen talepler VBS enclaven

### <a name="incoming-claims-can-also-be-used-as-outgoing-claims"></a>Gelen talepler (giden talepler olarak da kullanılabilir)

- **Aıkdoğrulanan**: kanıtlama kimlik anahtarı (AIK) sertifikası doğrulandıktan sonra bilgi içeren Boole değeri.
- **Aıkpubhash**: Base64 (SHA256 (AIK ortak anahtarı, der biçiminde) içeren dize).
- **Tpmversion**: GÜVENILIR Platform Modülü (TPM) ana sürümünü içeren tamsayı değeri.
- **secureBootEnabled**: güvenli önyüklemenin etkin olup olmadığını gösteren Boolean değeri.
- **Iommuenabled**: giriş çıkış belleği yönetim biriminin (IOMMU) etkin olup olmadığını gösteren Boole değeri.
- **bootDebuggingDisabled**: önyükleme hata ayıklamanın devre dışı olup olmadığını belirten Boole değeri.
- **notSafeMode**: Windows 'un güvenli modda çalışıp çalışmadığını gösteren Boole değeri.
- **Notwinpe**: Windows 'un WinPE modunda çalışıp çalışmadığını gösteren Boolean değeri.
- **Vbsenabled**: VBS 'nın etkin olup olmadığını gösteren Boole değeri.
- **Vbsreportsun**: VBS enckıve raporunun kullanılabilir olup olmadığını gösteren Boolean değeri.
- **enclaveauthorıd**: kuşatma Author ID değerinin Base64Url kodlamalı değerini içeren dize değeri-kuşatma için birincil modülün yazar tanımlayıcısı.
- **enclaveımageıd**: kuşatma Image ID 'nin Base64Url kodlamalı değerini içeren dize değeri-kuşatma için birincil modülün görüntü tanımlayıcısı.
- **enclaveownerıd**: kuşatma Owner ID 'nin Base64Url kodlamalı değerini içeren dize değeri-kuşatma için sahibin tanımlayıcısı.
- **enclaveFamilyId**: kuşatma Family ID 'nin Base64Url kodlamalı değerini içeren String değeri. Şifreleme için birincil modülün aile tanımlayıcısı.
- **Enclavesvn**: şifreleme için birincil modülün güvenlik sürüm numarasını içeren tamsayı değeri.
- **Enclaveplatformsvn**: kuşları barındıran platformun güvenlik sürüm numarasını içeren tamsayı değeri.
- **enclaveflags**: enclaveflags Claim, şifreleme için çalışma zamanı Ilkesini tanımlayan bayrakları Içeren bir tamsayı değeridir.
  
### <a name="outgoing-claims"></a>Giden talepler

- **policy_hash**: BASE64URL tarafından hesaplanan Ilke metninin SHA256 karmasını içeren dize DEĞERI (SHA256 (BASE64URL (UTF8 (ilke metni)))).
- **policy_signer**: ortak anahtarla veya imzalı ilke üstbilgisinde bulunan sertifika zinciriyle JWK içeren dize değeri.
- **ver (sürüm)**: raporun sürümünü içeren dize değeri. Şu anda 1,0.
- **CNF (onay) talebi**: "CNF" talebi, elinde bulunan anahtarı belirlemek için kullanılır. RFC 7800 ' de tanımlanan onay talebi, bir JSON Web anahtarı (JWK) nesnesi (RFC 7517) olarak temsil edilen attest kuşkuşve anahtarının genel bölümünü içerir.
- **rp_data (bağlı olan taraf verileri)**: istekte belirtilen, varsa bağlı olan taraf verileri, raporun yeniliği güvence altına almak için bir kerelik anahtar olarak kullanılır.
- **"JTI" (JWT kimliği) talebi**: "JTI" (JWT ID) TALEBI, JWT için benzersiz bir tanımlayıcı sağlar. Tanımlayıcı değeri, aynı değerin yanlışlıkla farklı bir veri nesnesine atanabileceği bir olasılık olmasını sağlayacak şekilde atanır.

### <a name="property-claims"></a>Özellik talepleri

- **report_validity_in_minutes**: belirtecin ne kadar süreyle geçerli olduğunu belirten bir tamsayı talebi.
  - **Varsayılan değer (saat)**: dakika cinsinden bir gün.
  - **En büyük değer (saat)**: dakika cinsinden bir yıl.
- **omit_x5c**: Azure kanıtlama 'nın hizmet özgünlük kanıtı sağlamak için kullanılan sertifikayı atlaması gerekip gerekmediğini belirten bir Boole talebi. True ise, x5t kanıtlama belirtecine eklenecektir. False (varsayılan) ise, x5c kanıtlama belirtecine eklenecektir.

## <a name="next-steps"></a>Sonraki adımlar
- [Kanıtlama ilkesi yazma ve imzalama](author-sign-policy.md)
- [PowerShell kullanarak Azure kanıtlama ayarlama](quickstart-powershell.md)
