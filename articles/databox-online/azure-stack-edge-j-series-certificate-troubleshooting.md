---
title: GPU ile Azure Stack Edge ile sertifika sorunlarını giderme | Microsoft Docs
description: Azure Stack Edge GPU cihazındaki sertifika hatalarını giderme işlemini açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: troubleshooting
ms.date: 08/28/2020
ms.author: alkohli
ms.openlocfilehash: 05338fb70bfa390ff3bd9e14e938edeb40938aeb
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89269446"
---
# <a name="troubleshooting-certificate-errors"></a>Sertifika hatalarını giderme

Makale, Azure Stack Edge cihazınıza sertifika yüklerken ortak sertifika hatalarıyla ilgili sorun giderme sağlar.

## <a name="common-certificate-errors"></a>Ortak sertifika hataları

Aşağıdaki tabloda, yaygın Sertifika hataları ve bu hatalar ve olası çözümler hakkında ayrıntılı bilgiler gösterilmektedir:

> [!NOTE]
> &#8220;{0} , {1} ,..., {n} &#8221;, konumsal parametreleri gösterir. Konumsal parametreler, kullanmakta olduğunuz sertifikalara göre değerleri alır.

| Hata Kodu | Açıklama |
|---|---|
| CertificateManagement_UntrustedCertificate | Konu adı olan sertifikada {0} sertifika zinciri kopuk. Bu sertifikayı karşıya yüklemeden önce imzalama zinciri sertifikasını karşıya yükleyin.|
| CertificateManagement_DeviceNotRegistered| Cihazınız etkinleştirilmemiş. Yalnızca etkinleştirme sonrasında bir destek sertifikası yükleyebilirsiniz.|
| CertificateManagement_EmptySAN | Konu adı olan sertifikanın {0} Konu diğer adı yok. Sertifika özelliklerinizi denetleyin ve yeni bir sertifika alın.|
| CertificateManagement_ExpiredCertificate | Türü olan sertifikanın {0} süresi doldu veya süresi yakında doluyor. Sertifika süre sonunu denetleyin ve gerekirse yeni bir sertifika alın.|
| CertificateManagement_FormatMismatch | Sertifika biçimi desteklenmiyor. Sertifika biçimini denetleyin ve gerekirse yeni bir sertifika alın.  Beklenen {0} , bulunan {1} . |
| CertificateManagement_GenericError | Sertifika Yönetimi işlemi gerçekleştirilemedi. Birkaç dakika içinde bu işlemi yeniden deneyin. Sorun devam ederse Microsoft Desteği başvurun. |
| CertificateManagement_HttpsBindingFailure | Konu adına sahip sertifika {0} güvenli bir HTTPS kanalı oluşturamadı. Karşıya yüklediğiniz sertifikayı denetleyin ve gerekirse yeni bir sertifika alın. Bu hata, cihaz düğüm sertifikasıyla birlikte oluşur.|
| CertificateManagement_IncorrectKeyCertSignKeyUsage | Konu adına sahip sertifika, {0} anahtar kullanımı sertifika imzalaması içermemelidir. Sertifikanın anahtar kullanımını denetleyin ve gerekirse yeni bir sertifika alın. Bu hata imzalama zinciri sertifikasıyla birlikte oluşur. |
| CertificateManagement_IncorrectKeyNumber | Konu adı olan sertifikanın {0} anahtar numarası yanlış {1} . Sertifikanın anahtar numarasını denetleyin ve gerekirse yeni bir sertifika alın.|
| CertificateManagement_InvalidP7b | Karşıya yüklenen sertifika dosyası geçerli değil.  Sertifika biçimini denetleyin ve gerekirse yeni bir sertifika alın.|
| CertificateManagement_KeyAlgorithmNotRSA | Bu sertifika, RSA anahtar algoritmasını kullanmaz. Yalnızca RSA sertifikaları desteklenir. |
| CertificateManagement_KeySizeNotSufficient | Konu adı olan sertifikada {0} anahtar boyutu yetersiz {1} . Minimum anahtar boyutu 4096 ' dir.|
| CertificateManagement_MissingClientOid | Konu adı olan sertifikada {0} istemci kimlik doğrulaması OID 'si yok. Sertifika özelliklerinizi denetleyin ve gerekirse yeni bir sertifika alın.|
| CertificateManagement_MissingDigitalSignatureKeyUsage | Konu adına sahip sertifika {0} , anahtar kullanımında dijital imzaya sahip değil. Sertifika özelliklerinizi denetleyin ve gerekirse yeni bir sertifika alın. |
| CertificateManagement_MissingEntryInSAN | Konu adı olan sertifikanın konu {0} diğer adında bir konu adı girdisi yok. Sertifika özelliklerinizi denetleyin ve yeni bir sertifika alın. |
| CertificateManagement_MissingKeyCertSignKeyUsage | Konu adı olan sertifikada {0} , anahtar kullanımında sertifika imzalama yok. Sertifika özelliklerinizi denetleyin ve gerekirse yeni bir sertifika alın.|
| CertificateManagement_MissingKeyEnciphermentKeyUsage | Konu adına sahip sertifika {0} , anahtar kullanımında anahtar şifrelemesi içermiyor. Sertifika özelliklerinizi denetleyin ve gerekirse yeni bir sertifika alın. |
| CertificateManagement_MissingServerOid | Konu adı olan sertifikada {0} sunucu kimlik doğrulaması OID 'si yok. Sertifika özelliklerinizi denetleyin ve gerekirse yeni bir sertifika alın.|
| CertificateManagement_NameMismatch | Sertifika türü uyumsuzluğu. Beklenen kapsam: {0} , bulunan {1} . Uygun sertifikayı karşıya yükleyin.|
| CertificateManagement_NoPrivateKeyPresent | Konu adı olan sertifikada {0} özel anahtar yok. Özel anahtarla bir. pfx sertifikasını karşıya yükleyin.|
| CertificateManagement_NotSelfSignedCertificate | Konu adına sahip sertifika {0} kendinden imzalı değildir. Kök sertifikalar kendinden imzalı olmalıdır |
| CertificateManagement_NotSupportedOnVirtualAppliance | Bu işlem sanal cihazda desteklenmiyor. Bu hata, imzalama işleminin yalnızca, politik bulut gerecinde çalışan Data Box Gateway olduğunu gösterir. Bu hata, cihaz Windows PowerShell aracılığıyla yönetirken oluşur.|
| CertificateManagement_SelfSignedCertificate | Konu adına sahip sertifika {0} kendinden imzalı. Doğru imzalı bir sertifikayı karşıya yükleyin.|
| CertificateManagement_SignatureAlgorithmSha1 | Konu adı olan sertifikada {0} imza algoritması desteklenmiyor. SHA1-RSA desteklenmez. |
| CertificateManagement_SubjectNamesInvalid | Konu adına sahip sertifika {0} , sertifika için doğru konu adına veya konu diğer adlarına sahip değil {1} . Karşıya yüklediğiniz sertifikayı denetleyin ve gerekirse yeni bir sertifika alın. Ayrıca, San adlarıyla eşleşecek DNS adını da denetlemeniz gerekir.|
| CertificateManagement_UnreadableCertificate | Türe sahip sertifika {0} okunamadı. Bu hata, Sertifika okunamaz veya bozuksa oluşur. Yeni bir sertifika alın.|
| CertificateSubjectNotFound | Konu adına sahip bir sertifika {0} bulunamadı. Yeni bir sertifika alın.|

## <a name="next-steps"></a>Sonraki adımlar

[Sertifika gereksinimleri](azure-stack-edge-j-series-certificate-requirements.md)