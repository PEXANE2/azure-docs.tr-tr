---
title: Azure kanıtlama için sanallaştırma tabanlı güvenlik (VBS) protokolü
description: VBS kanıtlama Protokolü
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: reference
ms.date: 07/20/2020
ms.author: mbaldwin
ms.openlocfilehash: e5cc3b5fb7ca38df196119de12d346f5d0346b58
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91346081"
---
# <a name="virtualization-based-security-vbs-attestation-protocol"></a>Sanallaştırma tabanlı güvenlik (VBS) kanıtlama Protokolü 

Microsoft Azure kanıtlama, raporlama yaptığı verilerin gerçek olduğunu garanti etmek için, bellenimden hiper yönetici ve güvenli çekirdek 'nin başlatılması için bir güven zinciri oluşturulması gerekir. Bu Azure kanıtlama elde etmek için güvenli kuşve güvende güven kurmadan önce makinenin önyükleme durumunu atlamanız gerekir. İşletim sistemi, hiper yönetici ve güvenli çekirdek ikilileri, doğru resmi Microsoft yetkilileri tarafından imzalanmalıdır ve güvenli bir şekilde yapılandırılmalıdır. Güvenilir Platform Modülü (TPM) ve hiper yöneticinin sistem durumu arasında güven ilişkisi olduktan sonra, ölçülen önyükleme günlüğünde belirtilen VBS ıDKS öğesine güvenebiliriz. Bu özellik ile, bir anahtar çiftinin, bu anahtarda güveni bağlayan ve güvenlik düzeyi ve önyükleme kanıtlama özellikleri gibi diğer talepler içeren bir kanıtlama raporu ile bir anahtar çiftinin oluşturulduğunu doğrulayabiliriz.

## <a name="protocol-messages"></a>Protokol iletileri

### <a name="init-message"></a>Init iletisi

#### <a name="direction"></a>Yön

İstemci-Azure kanıtlama >

#### <a name="payload"></a>Te

```
{
  "type": "aikcert"
}
```

"Type" (ASCII String): istenen kanıtlama türünü temsil eder. Şu anda yalnızca "aıkcert" desteklenir.

### <a name="challenge-message"></a>Sınama iletisi

#### <a name="direction"></a>Yön

Azure kanıtlama-> Istemcisi

#### <a name="payload"></a>Te

```
{

  "challenge": "<BASE64URL(CHALLENGE)>",
  
  "service_context": "<BASE64URL(SERVICECONTEXT)>"
  
}
```

**Challenge** (BASE64URL (sekizli)): hizmet tarafından verilen rastgele değer.

**service_context** (BASE64URL (sekizli)): hizmet tarafından oluşturulan, diğer kullanıcılar, zorluk ve bu zorluk için süre sonu zamanı içeren donuk, şifreli bağlam.


### <a name="request-message"></a>İstek iletisi

#### <a name="direction"></a>Yön

İstemci-Azure kanıtlama > 

#### <a name="payload"></a>Te

```
{

  "request": "<JWS>"
  
}
```

**istek** (JWS): Istek bir JSON Web imzası (JWS) yapısından oluşur. JWS korumalı üst bilgi ve JWS yükü aşağıda gösterilmiştir. Her JWS yapısında olduğu gibi, son değer aşağıdakilerden oluşur:

BASE64URL (UTF8 (JWS korumalı üst bilgi)) | | '.' ||

BASE64URL (JWS yükü) | | '.' ||

BASE64URL (JWS Imzası)

##### <a name="jws-protected-header"></a>JWS korumalı üst bilgi

```
{
  "alg": "PS256",
  "typ": "attReq"
  // no "kid" parameter as the key specified by attest_key MUST sign this JWS to prove possession.
}
```

##### <a name="jws-payload"></a>JWS yükü

JWS yükü Basic veya VBS türünde olabilir. Temel, kanıtlama kanıtlaması VBS verileri içermediği zaman kullanılır.

Temel örnek

``` 
{
  "att_type": "basic",
  "att_data": {
    "rp_id": "<URL>",
    "rp_data": "<BASE64URL(RPCUSTOMDATA)>",
    "challenge": "<BASE64URL(CHALLENGE)>",
    "tpm_att_data": {
      "srtm_boot_log": "<BASE64URL(SRTMBOOTLOG)>",
      "srtm_resume_log": "<BASE64URL(SRTMRESUMELOG)>",
      "drtm_boot_log": "<BASE64URL(DRTMBOOTLOG)>",
      "drtm_resume_log": "<BASE64URL(DRTMRESUMELOG)>",
      "aik_cert": "<BASE64URL(AIKCERTIFICATE)>",
      // aik_pub is represented as a JSON Web Key (JWK) object (RFC 7517).
      "aik_pub": {
        "kty": "RSA",
        "n": "<Base64urlUInt(MODULUS)>",
        "e": "<Base64urlUInt(EXPONENT)>"
      },
      "current_claim": "<BASE64URL(CURRENTCLAIM)>",
      "boot_claim": "<BASE64URL(BOOTCLAIM)>"
    },
    // attest_key is represented as a JSON Web Key (JWK) object (RFC 7517).
    "attest_key": {
      "kty": "RSA",
      "n": "<Base64urlUInt(MODULUS)>",
      "e": "<Base64urlUInt(EXPONENT)>"
    },
    "custom_claims": [
      {
        "name": "<name>",
        "value": "<value>",
        "value_type": "<value_type>"
      },
      {
        "name": "<name>",
        "value": "<value>",
        "value_type": "<value_type>"
      }
    ],
    "service_context": "<BASE64URL(SERVICECONTEXT)>"
  }
}
```

VBS örnek

``` 
{
  "att_type": "vbs",
  "att_data": {
    "report_signed": {
      "rp_id": "<URL>",
      "rp_data": "<BASE64URL(RPCUSTOMDATA)>",
      "challenge": "<BASE64URL(CHALLENGE)>",
      "tpm_att_data": {
        "srtm_boot_log": "<BASE64URL(SRTMBOOTLOG)>",
        "srtm_resume_log": "<BASE64URL(SRTMRESUMELOG)>",
        "drtm_boot_log": "<BASE64URL(DRTMBOOTLOG)>",
        "drtm_resume_log": "<BASE64URL(DRTMRESUMELOG)>",
        "aik_cert": "<BASE64URL(AIKCERTIFICATE)>",
        // aik_pub is represented as a JSON Web Key (JWK) object (RFC 7517).
        "aik_pub": {
          "kty": "RSA",
          "n": "<Base64urlUInt(MODULUS)>",
          "e": "<Base64urlUInt(EXPONENT)>"
        },
        "current_claim": "<BASE64URL(CURRENTCLAIM)>",
        "boot_claim": "<BASE64URL(BOOTCLAIM)>"
      },
      // attest_key is represented as a JSON Web Key (JWK) object (RFC 7517).
      "attest_key": {
        "kty": "RSA",
        "n": "<Base64urlUInt(MODULUS)>",
        "e": "<Base64urlUInt(EXPONENT)>"
      },
      "custom_claims": [
        {
          "name": "<name>",
          "value": "<value>",
          "value_type": "<value_type>"
        },
        {
          "name": "<name>",
          "value": "<value>",
          "value_type": "<value_type>"
        }
      ],
      "service_context": "<BASE64URL(SERVICECONTEXT)>"
    },
    "vbs_report": "<BASE64URL(REPORT)>"
  }
}
``` 

**rp_id** (stringoruri): bağlı olan taraf tanımlayıcısı. Hizmet tarafından makine KIMLIĞI talebi hesaplamasında kullanılır

**rp_data** (BASE64URL (sekizli)): bağlı olan taraf tarafından geçen donuk veri. Bu, genellikle raporun yeniliği güvence altına almak için bir kerelik olarak bağlı olan taraf tarafından kullanılır

**sınama** (BASE64URL (sekizli)): hizmet tarafından verilen rastgele değer

**tpm_att_data**: TPM ile ilgili kanıtlama verileri

- **srtm_boot_log (BASE64URL (sekizli))**: işlev Tbsi_Get_TCG_Log_Ex, günlük türü = TBS_TCGLOG_SRTM_BOOT tarafından alınan SRTM önyükleme günlüğü

- **srtm_resume_log (BASE64URL (sekizli))**: SRTM günlük türü = TBS_TCGLOG_SRTM_RESUME ile işlev Tbsi_Get_TCG_Log_Ex tarafından alınan günlüğü sürdürür

- **drtm_boot_log (BASE64URL (sekizli))**: işlev Tbsi_Get_TCG_Log_Ex tarafından alınan drtm önyükleme günlüğü, günlük türü = TBS_TCGLOG_DRTM_BOOT

- **drtm_resume_log (BASE64URL (sekizli))**: drtm günlük türü = TBS_TCGLOG_DRTM_RESUME ile işlev Tbsi_Get_TCG_Log_Ex tarafından alınan günlüğü sürdürür

- **aik_cert (BASE64URL (sekizli))**: NCryptGetProperty işlevi tarafından döndürülen X. 509.440 sertifikası özelliği = NCRYPT_CERTIFICATE_PROPERTY

- **aik_pub**: AIK 'Nin JSON Web anahtarı (JWK) nesnesi olarak temsil ettiği genel bölümü (RFC 7517)

- **current_claim (BASE64URL (sekizli))**: dwclaimtype = NCRYPT_CLAIM_PLATFORM ve Parameter NCRYPTBUFFER_TPM_PLATFORM_CLAIM_PCR_MASK tüm PTE 'leri içerecek şekilde ayarlanan NCryptCreateClaim işlevi tarafından döndürülen geçerli PCR durumu için kanıtlama talebi. Hizmet tarafından gönderilen sınama bu talebin hesaplamasında de kullanılmalıdır

- **boot_claim (BASE64URL (sekizli))**: dwclaimtype = NCRYPT_CLAIM_PLATFORM ve Parameter NCRYPTBUFFER_TPM_PLATFORM_CLAIM_PCR_MASK tüm PTE 'leri içerecek şekilde ayarlanan NCryptCreateClaim işlevi tarafından döndürülen PCR durumu için kanıtlama talebi

**vbs Report** (BASE64URL (sekizli)): işlev enclavegetattestationreport tarafından döndürülen vbs kuşatma kanıtlama raporu. EnclaveData parametresi, report_signed değerinin SHA-512 karması olmalıdır (açılış ve kapanış ayraçları dahil). Karma işlev girişi UTF8 (report_signed)

**attest_key**: bir JSON Web anahtarı (JWK) nesnesi olarak temsil edilen şifreleme anahtarının genel bölümü (RFC 7517)

**custom_claims**: hizmete gönderilen ve ilke tarafından değerlendirilebilen özel şifreleme talepleri dizisi. Talep

- **ad (dize)**: talebin adı. Bu ad, kanıtlama hizmeti tarafından belirlenen bir URL 'ye eklenir (çakışmaları önlemek için) ve birleştirilmiş dize ilkede kullanılabilecek talebin türü olur

- **değer (dize)**: talebin değeri

- **value_type (dize)**: talep değerinin veri türü

**service_context** (BASE64URL (sekizli)): hizmet tarafından oluşturulan, diğer kullanıcılar, zorluk ve bu zorluk için süre sonu zamanı içeren donuk, şifreli bağlam.

### <a name="report-message"></a>Rapor iletisi

#### <a name="direction"></a>Yön

Azure kanıtlama-> Istemcisi

#### <a name="payload"></a>Te

```
{
  "report": "<JWT>"
}
```

**Report** (JWT): JSON Web token (JWT) biçiminde kanıtlama raporu (RFC 7519).
