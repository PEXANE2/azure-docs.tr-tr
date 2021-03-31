---
title: Redsıs için Azure önbelleğindeki yenilikler
description: Redsıs için Azure önbelleği için son güncelleştirmeler
author: yegu-ms
ms.service: cache
ms.topic: reference
ms.date: 09/28/2020
ms.author: yegu
ms.openlocfilehash: b30e83b89b25e6400b8c7e0419406631fa1edcd0
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "91492542"
---
# <a name="whats-new-in-azure-cache-for-redis"></a>Redsıs için Azure önbelleğindeki yenilikler

## <a name="azure-tls-certificate-change"></a>Azure TLS sertifikası değişikliği

Microsoft, Azure hizmetlerini farklı bir sertifika yetkilileri (CA) kümesinden TLS sunucu sertifikaları kullanacak şekilde güncelleştiriyor. Bu değişiklik 13 Ağustos 2020 ' de 26 Ekim 2020 ' e (tahmini) göre yapılır. [GEÇERLI CA SERTIFIKALARı CA/tarayıcı Forum temel gereksinimlerinden biriyle uyumlu olmadığı için](https://bugzilla.mozilla.org/show_bug.cgi?id=1649951)Azure bu değişikliği yapıyor. Bu sorun 1 Temmuz 2020 ' de raporlanır ve dünya çapındaki birden çok popüler ortak anahtar altyapısı (PKI) sağlayıcısı için geçerlidir. Azure hizmetleri tarafından günümüzde kullanılan çoğu TLS sertifikası, *Baltimore CyberTrust kök* PKI 'dan geliyor. Redsıs hizmeti için Azure önbelleği, Baltimore CyberTrust köküne zincirolmaya devam edecektir. Bununla birlikte, TLS sunucu sertifikaları, 12 Ekim 2020 ' den itibaren yeni ara sertifika yetkilileri (ICAS) tarafından verilecek.

> [!NOTE]
> Bu değişiklik, genel [Azure bölgelerinde](https://azure.microsoft.com/global-infrastructure/geographies/)bulunan hizmetlerle sınırlıdır. Sogeign (ör. Çin) veya kamu bulutlarını dışlar.
>
>

### <a name="does-this-change-affect-me"></a>Bu değişiklik beni etkiler mi?

Redsıs müşterilerinin en çok Azure önbelleğinin değişiklikten etkilenmemesi beklenmez. Uygulamanız, "sertifika sabitleme" olarak bilinen, kabul edilebilir sertifikaların bir listesini açıkça belirtiyorsa, bu durum etkilenebilir. Bu, Baltimore CyberTrust kökü yerine bir ara veya yaprak sertifikaya sabitlenmişse, sertifika yapılandırmasını değiştirmek için **hemen işlem** yapmanız gerekir.

Aşağıdaki tabloda, toplanan sertifikalar hakkında bilgi verilmektedir. Uygulamanızın kullandığı sertifikaya bağlı olarak, Redsıs örneği için Azure önbelleğinize bağlantı kaybını engellemek için güncelleştirmeniz gerekebilir.

| CA türü | Geçerli | Gönderi (12 Ekim 2020) | Eylem |
| ----- | ----- | ----- | ----- |
| Root | Parmak izi: d4de20d05e66fc53fe1a50882c78db2852cae474<br><br> Süre sonu: Pazartesi, Mayıs 12, 2025, 4:59:00 PM<br><br> Konu adı:<br> CN = Baltimore CyberTrust kökü<br> OU = CyberTrust<br> O = Baltimore<br> C = IE | Değiştirilmiyor | Yok |
| Hammaddeleri | Parmak izleri<br> CN = Microsoft IT TLS CA 1<br> Parmak izi: 417e225037fbfaa4f95761d5ae729e1aea7e3a42<br><br> CN = Microsoft IT TLS CA 2<br> Parmak izi: 54d9d20239080c32316ed9ff980a48988f4adf2d<br><br> CN = Microsoft IT TLS CA 4<br> Parmak izi: 8a38755d0996823fe8fa3116a277ce446eac4e99<br><br> CN = Microsoft IT TLS CA 5<br> Parmak izi: Ad898ac73df333eb60ac1f5fc6c4b2219ddb79b7<br><br> Süre sonu: Cuma, 20 Mayıs 2024 5:52:38<br><br> Konu adı:<br> OU = Microsoft IT<br> O = Microsoft Corporation<br> L = Redmond<br> S = Washington<br> C = US<br> | Parmak izleri<br> CN = Microsoft RSA TLS CA 01<br> Parmak izi: 703d7a8f0ebf55aaa59f98eaf4a206004eb2516a<br><br> CN = Microsoft RSA TLS CA 02<br> Parmak izi: b0c2d2d13cdd56cdaa6ab6e2c04440be4a429c75<br><br> Süre sonu: Salı, 8 Ekim 2024 12:00:00<br><br> Konu adı:<br> O = Microsoft Corporation<br> C = US<br> | Gerekli |

### <a name="what-actions-should-i-take"></a>Hangi eylemleri gerçekleştirmeniz gerekir?

Uygulamanız işletim sistemi sertifika deposunu kullanıyorsa veya diğer bir deyişle Baltimore kökünü başka bir şekilde sabitlerseniz, hiçbir eylemde bulunmanız gerekmez. Öte yandan, uygulamanız herhangi bir ara veya yaprak TLS sertifikasını sabitleyebilir, aşağıdaki kökleri sabitlemesini öneririz:

| Sertifika | Parmak izi |
| ----- | ----- |
| [Baltimore kök CA](https://cacerts.digicert.com/BaltimoreCyberTrustRoot.crt) | d4de20d05e66fc53fe1a50882c78db2852cae474 |
| [Microsoft RSA kök sertifika yetkilisi 2017](https://www.microsoft.com/pkiops/certs/Microsoft%20RSA%20Root%20Certificate%20Authority%202017.crt) | 73a5e64a3bff8316ff0edccc618a906e4eae4d74 |
| [DigiCert genel kök G2](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt) | df3c24f9bfd666761b268073fe06d1cc8d4f82a4 |

> [!TIP]
> Ara ve yaprak sertifikaların her ikisinin de sık olarak değiştirilmesi beklenmektedir. Bunlara bir bağımlılık yapmanız önerilmez. Bunun yerine uygulamanızı daha az sıklıkta bir kök sertifikaya sabitleyin.
>
>

Ara sertifikaları sabitlemeye devam etmek için, gelecekteki değişiklikleri en aza indirmek üzere birkaç ek içeren sabitlenmiş ara sertifikalar listesine aşağıdakileri ekleyin:

| CA 'nın ortak adı | Parmak izi |
| ----- | ----- |
| [Microsoft RSA TLS CA 01](https://www.microsoft.com/pki/mscorp/Microsoft%20RSA%20TLS%20CA%2001.crt) | 703d7a8f0ebf55aaa59f98eaf4a206004eb2516a |
| [Microsoft RSA TLS CA 02](https://www.microsoft.com/pki/mscorp/Microsoft%20RSA%20TLS%20CA%2002.crt) | b0c2d2d13cdd56cdaa6ab6e2c04440be4a429c75 |
| [Microsoft Azure TLS veren CA 01](https://www.microsoft.com/pkiops/certs/Microsoft%20Azure%20TLS%20Issuing%20CA%2001.cer) | 2f2877c5g778c31e0f29c7e371df5471bd673173 |
| [Microsoft Azure TLS veren CA 02](https://www.microsoft.com/pkiops/certs/Microsoft%20Azure%20TLS%20Issuing%20CA%2002.cer) | e7eea674ca718e3befd90858e09f8372ad0ae2aa |
| [Microsoft Azure TLS veren CA 05](https://www.microsoft.com/pkiops/certs/Microsoft%20Azure%20TLS%20Issuing%20CA%2005.cer) | 6c3af02e7f269aa73afd0eff2a88a4a1f04ed1e5 |
| [Microsoft Azure TLS veren CA 06](https://www.microsoft.com/pkiops/certs/Microsoft%20Azure%20TLS%20Issuing%20CA%2006.cer) | 30e01761ab97e59a06b41ef20af6f2de7ef4f7b0 |

Uygulamanız kodda sertifikayı doğrularıyorsa, yeni sabitlenmiş sertifikaların özelliklerini (ör. verenler, Parmak Izi) tanımak için onu değiştirmeniz gerekir. Bu ek doğrulama, daha sonra gelecek prova olması için sabitlenmiş tüm sertifikaları kapsamalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Başka sorularınız varsa [destek](https://azure.microsoft.com/support/options/)aracılığıyla bizimle iletişim kurun.  
