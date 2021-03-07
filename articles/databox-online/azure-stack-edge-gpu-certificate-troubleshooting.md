---
title: GPU ile Azure Stack Edge Pro ile sertifika sorunlarını giderme | Microsoft Docs
description: Azure Stack Edge Pro GPU cihazındaki sertifika hatalarını giderme işlemini açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: troubleshooting
ms.date: 02/22/2021
ms.author: alkohli
ms.openlocfilehash: 67dd2b35229c15ae4df5ec8acb357aa35621d67c
ms.sourcegitcommit: 5bbc00673bd5b86b1ab2b7a31a4b4b066087e8ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2021
ms.locfileid: "102436643"
---
# <a name="troubleshooting-certificate-errors"></a>Sertifika hatalarını giderme

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

Makale, Azure Stack Edge Pro cihazınıza sertifika yüklerken ortak sertifika hatalarıyla ilgili sorun giderme sağlar.

## <a name="common-certificate-errors"></a>Ortak sertifika hataları

Aşağıdaki tabloda, yaygın Sertifika hataları ve bu hatalar ve olası çözümler hakkında ayrıntılı bilgiler gösterilmektedir:

> [!NOTE]
> &#8220;{0} , {1} ,..., {n} &#8221;, konumsal parametreleri gösterir. Konumsal parametreler, kullanmakta olduğunuz sertifikalara göre değerleri alır.

| Hata Kodu | Açıklama |
|---|---|
| CertificateManagement_UntrustedCertificate | Konu adı olan sertifikada {0} sertifika zinciri kopuk. Bu sertifikayı karşıya yüklemeden önce imzalama zinciri sertifikasını karşıya yükleyin.|
| CertificateManagement_DeviceNotRegistered| Cihazınız etkinleştirilmemiş. Yalnızca etkinleştirme sonrasında bir destek sertifikası yükleyebilirsiniz.|
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
| CertificateManagement_MissingKeyCertSignKeyUsage | Konu adı olan sertifikada {0} , anahtar kullanımında sertifika imzalama yok. Sertifika özelliklerinizi denetleyin ve gerekirse yeni bir sertifika alın.|
| CertificateManagement_MissingKeyEnciphermentKeyUsage | Konu adına sahip sertifika {0} , anahtar kullanımında anahtar şifrelemesi içermiyor. Sertifika özelliklerinizi denetleyin ve gerekirse yeni bir sertifika alın. |
| CertificateManagement_MissingServerOid | Konu adı olan sertifikada {0} sunucu kimlik doğrulaması OID 'si yok. Sertifika özelliklerinizi denetleyin ve gerekirse yeni bir sertifika alın.|
| CertificateManagement_NameMismatch | Sertifika türü uyumsuzluğu. Beklenen kapsam: {0} , bulunan {1} . Uygun sertifikayı karşıya yükleyin.|
| CertificateManagement_NoPrivateKeyPresent | Konu adı olan sertifikada {0} özel anahtar yok. Özel anahtarla bir. pfx sertifikasını karşıya yükleyin.|
| CertificateManagement_NoRSACryptoPrivateKey | Konu adına sahip sertifika için özel anahtara {0} erişilemiyor. Desteklenen bir sertifika kullandığınızdan emin olun. Yalnızca Microsoft RSA/SChannel Şifreleme sağlayıcısı desteklenir. |
| CertificateManagement_NotSelfSignedCertificate | Konu adına sahip sertifika {0} kendinden imzalı değildir. Kök sertifikalar kendinden imzalı olmalıdır |
| CertificateManagement_NotSupportedOnVirtualAppliance | Bu işlem sanal cihazda desteklenmiyor. Bu hata, imzalama işleminin yalnızca, politik bulut gerecinde çalışan Data Box Gateway olduğunu gösterir. Bu hata, cihaz Windows PowerShell aracılığıyla yönetirken oluşur.|
| CertificateManagement_SelfSignedCertificate | Konu adına sahip sertifika {0} kendinden imzalı. Doğru imzalı bir sertifikayı karşıya yükleyin.|
| CertificateManagement_SignatureAlgorithmSha1 | Konu adı olan sertifikada {0} imza algoritması desteklenmiyor. SHA1-RSA desteklenmez. |
| CertificateManagement_SubjectNamesInvalid | Konu adına sahip sertifika {0} , sertifika için doğru konu adına veya konu diğer adlarına sahip değil {1} . Karşıya yüklediğiniz sertifikayı denetleyin ve gerekirse yeni bir sertifika alın. Ayrıca, San adlarıyla eşleşecek DNS adını da denetlemeniz gerekir.|
| CertificateManagement_UnreadableCertificate | Türe sahip sertifika {0} okunamadı. Bu hata, Sertifika okunamaz veya bozuksa oluşur. Yeni bir sertifika alın.|
| CertificateSubjectNotFound | Konu adına sahip bir sertifika {0} bulunamadı. Yeni bir sertifika alın.|
| CertificateRotationGenericFailure | Bir veya daha fazla sertifika döndürme başarısız oldu. Birkaç dakika içinde yeniden deneyin. Sorun devam ederse Microsoft Desteği başvurun.|
| Certificateımportfailure | Parmak izine sahip sertifika {0} düğümde içeri aktarılmadı {1} . Sorun devam ederse Microsoft Desteği başvurun. |
| CertificateApplyFailure | Parmak izine sahip sertifika {0} düğümde uygulanmadı {1} . Sorun devam ederse Microsoft Desteği başvurun.|
| Nonominal olarak erişilebilir | Üzerindeki sertifika doğrulanamadı {0} . Sistem donanımını ve yazılım sistem durumunu denetleyin.|


## <a name="next-steps"></a>Sonraki adımlar

[Sertifika gereksinimleri](azure-stack-edge-gpu-certificate-requirements.md)