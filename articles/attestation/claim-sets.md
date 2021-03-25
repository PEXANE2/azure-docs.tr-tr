---
title: Azure kanıtlama talep kümeleri
description: Azure kanıtlama talep kümesi.
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: 0d6d5a08ea85ebb666acc0336f1e1d7ec5e097da
ms.sourcegitcommit: ed7376d919a66edcba3566efdee4bc3351c57eda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2021
ms.locfileid: "105044677"
---
# <a name="claim-sets"></a>Talep kümeleri

Microsoft Azure kanıtlama kullanan kuşışları ele geçirme sürecinde oluşturulan talepler aşağıdaki kategorilere ayrılabilir:

- **Gelen talepler**: kanıtlama kanıtını ayrıştırdıktan sonra Microsoft Azure kanıtlama tarafından oluşturulan talepler ve ilke yazarları tarafından özel bir ilkede yetkilendirme kuralları tanımlamak için kullanılabilir

- **Giden talepler**: Azure kanıtlama tarafından oluşturulan ve kanıtlama belirtecine eklenen talepler

- **Özellik talepleri**: Azure kanıtlama tarafından çıkış olarak oluşturulan talepler. Rapor kodlaması, raporun geçerlilik süresi vb. gibi kanıtlama belirtecinin özelliklerini temsil eden tüm talepleri içerir.

## <a name="incoming-claims"></a>Gelen talepler 

### <a name="sgx-attestation"></a>SGX kanıtlama

İlke yazarları tarafından bir SGX kanıtlama ilkesinde yetkilendirme kurallarını tanımlamak için kullanılacak talepler:

- **x-MS-SGX-,-hata ayıklanabilir**: bir Boolean değeri, kuşmanın hata ayıklamasının etkin olup olmadığını belirtir
- **x-MS-SGX-Product-ID**: SGX kuşatma ürün kimliği değeri 
- **x-MS-SGX-mrimzalayan**: teklifin "mrimzalayan" alanının onaltılık kodlanmış değeri
- **x-MS-SGX-mrenclave**: teklifin "mrenclave" alanının onaltılık kodlu değeri
- **x-MS-SGX-SVN**: teklifte kodlanmış güvenlik sürümü numarası 

Aşağıdaki talepler kullanım dışı olarak kabul edilir, ancak tam olarak desteklenir ve ileride dahil olmaya devam edecektir. Kullanımdan kaldırılmayan talep adlarını kullanmanız önerilir.

Kullanım dışı talep | Önerilen talep
--- | --- 
$is-hata ayıklanabilir | x-MS-SGX-,-hata ayıklanabilir
$product kimliği | x-MS-SGX-ürün kimliği
$sgx-mrimzalayan | x-MS-SGX-mrimzalayan
$sgx-mrenclave | x-MS-SGX-mrenclave
$svn | x-MS-SGX-SVN

### <a name="tpm-attestation"></a>TPM kanıtlama

Bir TPM kanıtlama ilkesinde yetkilendirme kurallarını tanımlamak için ilke yazarları tarafından kullanılacak talepler:

- **Aıkdoğrulanan**: kanıtlama kimlik anahtarı (AIK) sertifikası doğrulandığında veya yoksa bilgi içeren Boole değeri
- **Aıkpubhash**: Base64 (SHA256 (AIK ortak anahtarı, der biçiminde) içeren dize)
- **Tpmversion**: GÜVENILIR Platform Modülü (TPM) ana sürümünü içeren tamsayı değeri
- **secureBootEnabled**: güvenli önyüklemenin etkin olup olmadığını belirten Boole değeri
- **Iommuenabled**: giriş çıkış belleği yönetim biriminin (IOMMU) etkin olup olmadığını gösteren Boole değeri
- **bootDebuggingDisabled**: önyükleme hata ayıklamanın devre dışı olup olmadığını belirten Boole değeri
- **notSafeMode**: Windows 'un güvenli modda çalışıp çalışmadığını belirten Boole değeri
- **Notwinpe**: Windows 'un WinPE modunda çalışıp çalışmadığını gösteren Boolean değeri
- **vbsenabled**: VBS 'nın etkin olup olmadığını gösteren Boole değeri
- **Vbsreportsun**: VBS enckıve raporunun kullanılabilir olup olmadığını gösteren Boole değeri

### <a name="vbs-attestation"></a>VBS kanıtlama

TPM kanıtlama ilkesi taleplerini ek olarak, aşağıdaki talepler ilke yazarları tarafından bir VBS kanıtlama ilkesinde yetkilendirme kurallarını tanımlamak için kullanılabilir.

- **enclaveauthorıd**: kuşatma Author ID değerinin Base64Url kodlamalı değerini içeren dize değeri-kuşatma için birincil modülün yazar tanımlayıcısı
- **enclaveımageıd**: kuşatma Image ID 'nin Base64Url kodlamalı değerini içeren dize değeri-şifreleme için birincil modülün görüntü tanımlayıcısı
- **enclaveownerıd**: kuşatma Owner ID 'nin Base64Url kodlamalı değerini içeren String değeri-kuşatma için sahibin tanımlayıcısı
- **enclaveFamilyId**: kuşatma Family ID 'nin Base64Url kodlamalı değerini içeren String değeri. Şifreleme için birincil modülün aile tanımlayıcısı
- **Enclavesvn**: şifreleme için birincil modülün güvenlik sürüm numarasını içeren tamsayı değeri
- **Enclaveplatformsvn**: şifreleme barındıran platformun güvenlik sürümü numarasını içeren tamsayı değeri
- **enclaveflags**: enclaveflags Claim, şifreleme için çalışma zamanı Ilkesini tanımlayan bayrakları Içeren bir tamsayı değeridir

## <a name="outgoing-claims"></a>Giden talepler 

### <a name="common-for-all-attestation-types"></a>Tüm kanıtlama türleri için ortak

Azure kanıtlama, tüm kanıtlama türleri için kanıtlama belirtecinde aşağıdaki talepleri içerir. 

- **x-MS-ver**: JWT şema sürümü ("1,0" olması bekleniyor)
- **x-MS-kanıtlama-türü**: kanıtlama türünü temsil eden dize değeri 
- **x-MS-Policy-Hash**: BASE64URL olarak hesaplanan Azure kanıtlama değerlendirme ilkesi KARMASı (SHA256 (UTF8 (BASE64URL (UTF8 (ilke metni)))))
- **x-MS-Policy-imzalayan**: bir "jwk" üyesine sahip JSON nesnesi, bir müşterinin ilkesini imzalamak için kullanılan anahtarı temsil eder. Bu, müşteri imzalı bir ilkeyi karşıya yüklediğinde geçerlidir

Aşağıdaki talep adlarının altında [IETF JWT belirtimi](https://tools.ietf.org/html/rfc7519) kullanılır

- **"JTI" (JWT kimliği) talebi** -JWT için benzersiz tanımlayıcı
- **"ISS" (veren) talebi** -JWT veren sorumlu 
- **"IAT" (çıkarılan) talebi** : JWT 'nin verildiği zaman 
- **"exp" (sona erme saati) talebi** -sonrasında JWT 'ın işlenmek üzere kabul edilmeden önce geçen süre sonu
- **"NBF" (daha önce değil) talep** -önce JWT 'nin işlenmek üzere kabul edilmeden önce değil 

Aşağıdaki talep adları, [IETF yemek taslak belirtiminden](https://tools.ietf.org/html/draft-ietf-rats-eat-03#page-9) kullanılır

- **"Nonce talebi" (nonce)** -bir istemci tarafından sunulan isteğe bağlı bir nonce değerinin dönüştürülmedi bir doğrudan kopyası 

Aşağıdaki talepler kullanım dışı olarak kabul edilir, ancak tam olarak desteklenir ve ileride dahil olmaya devam edecektir. Kullanımdan kaldırılmayan talep adlarını kullanmanız önerilir.

Kullanım dışı talep | Önerilen talep
--- | --- 
ver | x-MS-ver
Tee | x-MS-kanıtlama-türü
policy_hash | x-MS-Policy-Hash
Maa-policyHash | x-MS-Policy-Hash
policy_signer  | x-MS-Policy-imzalayan

### <a name="sgx-attestation"></a>SGX kanıtlama 

Aşağıdaki talepler oluşturulup SGX kanıtlama için hizmet tarafından kanıtlama belirtecine eklenmiştir.

- **x-MS-SGX-,-hata ayıklanabilir**: bir Boolean değeri, kuşmanın hata ayıklamasının etkin olup olmadığını belirtir
- **x-MS-SGX-Product-ID**: SGX kuşatma ürün kimliği değeri 
- **x-MS-SGX-mrimzalayan**: teklifin "mrimzalayan" alanının onaltılık kodlanmış değeri
- **x-MS-SGX-mrenclave**: teklifin "mrenclave" alanının onaltılık kodlu değeri
- **x-MS-SGX-SVN**: teklifte kodlanmış güvenlik sürümü numarası 
- **x-MS-SGX-EHD**: BASE64URL olarak biçimlendirilen verileri tutulan (kuşve tutulan verileri şifreleme)
- **x-MS-SGX-yardımcı**: kanıtlama gerçekleştirmek için kullanılan yardımcı malzemeleri açıklayan JSON nesnesi. X-MS-SGX-yardımcı talep değeri, aşağıdaki anahtar/değer çiftlerine sahip iç içe geçmiş bir JSON nesnesidir:
    - **qeidcertshash**: SHA256 değeri, alıntı veren kuşatma (QE) kimlik veren sertifikaları
    - **qeidcrlhash**: SHA256 değeri, QE kimliği veren sertifikalar CRL listesi
    - **qeidhash**: QE kimliği yardımcı 'nın SHA256 değeri
    - **quotehash**: değerlendirilen teklifin SHA256 değeri
    - **tcbinfocertshash**: SHA256 sertifikası veren sertifikaları
    - **tcbinfocrlhash**: TCB bilgi veren CERT CRL listesinin SHA256 değeri
    - **tcbinfohash**: TCB bilgi yardımcı değerinin SHA256 değeri

Aşağıdaki talepler kullanım dışı olarak kabul edilir, ancak tam olarak desteklenir ve ileride dahil olmaya devam edecektir. Kullanımdan kaldırılmayan talep adlarını kullanmanız önerilir.

Kullanım dışı talep | Önerilen talep
--- | --- 
$is-hata ayıklanabilir | x-MS-SGX-,-hata ayıklanabilir
$product kimliği | x-MS-SGX-ürün kimliği
$sgx-mrimzalayan | x-MS-SGX-mrimzalayan
$sgx-mrenclave | x-MS-SGX-mrenclave
$svn | x-MS-SGX-SVN
$maa-EHD | x-MS-SGX-EHD
$aas-EHD | x-MS-SGX-EHD
$maa-attestationcollateral | x-MS-SGX-yardımcı

### <a name="tpm-and-vbs-attestation"></a>TPM ve VBS kanıtlama

- **CNF (onay)**: "CNF" talebi, elinde bulunan anahtarı belirlemek için kullanılır. RFC 7800 ' de tanımlanan onay talebi, bir JSON Web anahtarı (JWK) nesnesi (RFC 7517) olarak temsil edilen attest kuşkuşve anahtarının genel bölümünü içerir.
- **rp_data (bağlı olan taraf verileri)**: istekte belirtilen, varsa bağlı olan taraf verileri, raporun yeniliği güvence altına almak için bir kerelik anahtar olarak kullanılır. rp_data yalnızca varsa eklenir rp_data

## <a name="property-claims"></a>Özellik talepleri

### <a name="tpm-and-vbs-attestation"></a>TPM ve VBS kanıtlama

- **report_validity_in_minutes**: belirtecin ne kadar süreyle geçerli olduğunu belirtmek için bir tamsayı talebi.
  - **Varsayılan değer (saat)**: dakika cinsinden bir gün.
  - **En büyük değer (saat)**: dakika cinsinden bir yıl.
- **omit_x5c**: Azure kanıtlama 'nın hizmet özgünlük kanıtı sağlamak için kullanılan sertifikayı atlaması gerekip gerekmediğini belirten bir Boole talebi. True ise, x5t kanıtlama belirtecine eklenecektir. False (varsayılan) ise, x5c kanıtlama belirtecine eklenecektir.

## <a name="next-steps"></a>Sonraki adımlar
- [Kanıtlama ilkesi yazma ve imzalama](author-sign-policy.md)
- [PowerShell kullanarak Azure kanıtlama ayarlama](quickstart-powershell.md)
