---
title: Azure kanıtlama talep kümeleri
description: Azure kanıtlama talep kümesi.
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: afe2cf288cd4a15091e8278309b3ecf74a2d35a4
ms.sourcegitcommit: 65cef6e5d7c2827cf1194451c8f26a3458bc310a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2021
ms.locfileid: "98572757"
---
# <a name="claim-sets"></a>Talep kümeleri

Microsoft Azure kanıtlama kullanan kuşışları ele geçirme sürecinde oluşturulan talepler aşağıdaki kategorilere ayrılabilir:

- **Gelen talepler**: kanıtlama bulgusu ayrıştırdıktan sonra Microsoft Azure kanıtlama tarafından oluşturulan talepler ve ilke yazarları tarafından özel bir ilkede yetkilendirme kuralları tanımlamak için kullanılabilir

- **Giden talepler**: Azure kanıtlama tarafından oluşturulan ve kanıtlama belirtecinde biten tüm talepler içeren talepler

- **Özellik talepleri**: Azure kanıtlama tarafından çıkış olarak oluşturulan talepler. Rapor kodlaması, raporun geçerlilik süresi vb. gibi kanıtlama belirtecinin özelliklerini temsil eden tüm talepleri içerir.

### <a name="common-incoming-claims-across-all-attestation-types"></a>Tüm kanıtlama türlerinde ortak gelen talepler

Aşağıdaki talepler Azure kanıtlama tarafından oluşturulmuştur ve bir özel ilkede yetkilendirme kurallarını tanımlamak için kullanılabilir:
- **x-MS-ver**: JWT şema sürümü ("1,0" olması bekleniyor)
- **x-MS-kanıtlama-türü**: kanıtlama türünü temsil eden dize değeri 
- **x-MS-Policy-Hash**: BASE64URL olarak hesaplanan Azure kanıtlama değerlendirme ilkesi KARMASı (SHA256 (UTF8 (BASE64URL (UTF8 (ilke metni)))))
- **x-MS-Policy-imzalayan**: bir müşterinin imzalı bir ilkeyi karşıya yüklemesi durumunda, ilkesini imzalamak için kullanılan bir müşteriyi temsil eden "jwk" üyesine sahip JSON nesnesi

Aşağıdaki talepler kullanım dışı olarak kabul edilir ancak tam olarak desteklenir. Kullanımdan kaldırılmayan talep adlarını kullanmanız önerilir.

Kullanım dışı talep | Önerilen talep 
--- | --- 
ver | x-MS-ver
Tee | x-MS-kanıtlama-türü
Maa-policyHash | x-MS-Policy-Hash
policy_hash | x-MS-Policy-Hash
policy_signer | x-MS-Policy-imzalayan

### <a name="common-outgoing-claims-across-all-attestation-types"></a>Tüm kanıtlama türlerinde ortak giden talepler

[IETF JWT](https://tools.ietf.org/html/rfc7519) tarafından tanımlanan ve yanıt nesnesinde Azure kanıtlama tarafından kullanılan talepler aşağıda verilmiştir:

- **"JTI" (JWT KIMLIĞI) talebi**
- **"ISS" (veren) talebi**
- **"IAT" (çıkarılan) talebi**
- **"exp" (sona erme saati) talebi**
- **"NBF" (önüne değil) talebi**

[IETF yemek](https://tools.ietf.org/html/draft-ietf-rats-eat-03#page-9) tarafından tanımlanan ve yanıt nesnesinde Azure kanıtlama tarafından kullanılan talepler aşağıda verilmiştir:
- **"Nonce talebi" (nonce)**

## <a name="claims-specific-to-sgx-enclaves"></a>SGX şifrelere özgü talepler

### <a name="incoming-claims-specific-to-sgx-attestation"></a>SGX kanıtlama için özel gelen talepler

Aşağıdaki talepler, hizmet tarafından SGX kanıtlama için oluşturulur ve bir özel ilkede yetkilendirme kurallarını tanımlamak için kullanılabilir:
- **x-MS-SGX-,-hata ayıklanabilir**: bir Boolean değeri, kuşmanın hata ayıklamasının etkin olup olmadığını belirtir
- **x-MS-SGX-ürün kimliği**
- **x-MS-SGX-mrimzalayan**: teklifin "mrimzalayan" alanının onaltılık kodlanmış değeri
- **x-MS-SGX-mrenclave**: teklifin "mrenclave" alanının onaltılık kodlu değeri
- **x-MS-SGX-SVN**: teklifte kodlanmış güvenlik sürümü numarası 

### <a name="outgoing-claims-specific-to-sgx-attestation"></a>SGX kanıtlama için özel giden talepler

Aşağıdaki talepler hizmet tarafından oluşturulur ve SGX kanıtlama için yanıt nesnesine dahil edilmiştir:
- **x-MS-SGX-,-hata ayıklanabilir**: bir Boolean değeri, kuşmanın hata ayıklamasının etkin olup olmadığını belirtir
- **x-MS-SGX-ürün kimliği**
- **x-MS-ver**
- **x-MS-SGX-mrimzalayan**: teklifin "mrimzalayan" alanının onaltılık kodlanmış değeri
- **x-MS-SGX-mrenclave**: teklifin "mrenclave" alanının onaltılık kodlu değeri
- **x-MS-SGX-SVN**: teklifte kodlanmış güvenlik sürümü numarası 
- **x-MS-SGX-EHD**: BASE64URL olarak biçimlendirilen verileri tutulan (kuşve tutulan verileri şifreleme)
- **x-MS-SGX-yardımcı**: kanıtlama gerçekleştirmek için kullanılan yardımcı malzemeleri açıklayan JSON nesnesi. X-MS-SGX-yardımcı talep değeri, aşağıdaki anahtar/değer çiftlerine sahip iç içe geçmiş bir JSON nesnesidir:
    - **qeidcertshash**: SHA256 değeri, QE kimliği veren sertifikaları
    - **qeidcrlhash**: SHA256 değeri, QE kimliği veren sertifikalar CRL listesi
    - **qeidhash**: QE kimliği yardımcı 'nın SHA256 değeri
    - **quotehash**: değerlendirilen teklifin SHA256 değeri
    - **tcbinfocertshash**: SHA256 sertifikası veren sertifikaları
    - **tcbinfocrlhash**: TCB bilgi veren CERT CRL listesinin SHA256 değeri
    - **tcbinfohash**: kanıtlama gerçekleştirmek için kullanılan yardımcı malzemeleri açıklayan JSON nesnesi

Aşağıdaki talepler kullanım dışı olarak kabul edilir, ancak tam olarak desteklenir ve ileride dahil olmaya devam edecektir. Kullanımdan kaldırılmayan talep adlarını kullanmanız önerilir.

Kullanım dışı talep | Önerilen talep
--- | --- 
$is-hata ayıklanabilir | x-MS-SGX-,-hata ayıklanabilir
$sgx-mrimzalayan | x-MS-SGX-mrimzalayan
$sgx-mrenclave | x-MS-SGX-mrenclave
$product kimliği | x-MS-SGX-ürün kimliği
$svn | x-MS-SGX-SVN
$tee | x-MS-kanıtlama-türü
Maa-EHD | x-MS-SGX-EHD
AAS-EHD | x-MS-SGX-EHD
maa-attestationcollateral | x-MS-SGX-yardımcı

## <a name="claims-issued-specific-to-trusted-platform-module-tpm-attestation"></a>Güvenilir Platform Modülü (TPM) kanıtlaması için verilen talepler

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
  
### <a name="outgoing-claims-specific-to-tpm-attestation"></a>TPM kanıtlama için özel giden talepler

- **policy_hash**: BASE64URL tarafından hesaplanan Ilke metninin SHA256 karmasını içeren dize DEĞERI (SHA256 (BASE64URL (UTF8 (ilke metni)))).
- **policy_signer**: imzalı ilke üstbilgisinde ortak anahtara veya sertifika zincirine sahip bir JWK içeriyor.
- **ver (sürüm)**: raporun sürümünü içeren dize değeri. Şu anda 1,0.
- **CNF (onay) talebi**: "CNF" talebi, elinde bulunan anahtarı belirlemek için kullanılır. RFC 7800 ' de tanımlanan onay talepleri, bir JSON Web anahtarı (JWK) nesnesi (RFC 7517) olarak temsil edilen atsınıtik kuşve anahtarının genel bölümünü içerir.
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
