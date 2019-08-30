---
title: Risk algılama başvurusunu Azure Active Directory Kimlik Koruması | Microsoft Docs
description: Risk algılamaları Azure Active Directory Kimlik Koruması.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: reference
ms.date: 01/25/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: a1005ac1853568222dea83a0f12293945825cf6d
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70127586"
---
# <a name="azure-active-directory-identity-protection-risk-detections-reference"></a>Azure Active Directory Kimlik Koruması risk algılamaları başvurusu

Saldırganlar bir kullanıcının kimliğini çalarak bir ortama erişim kazanıyorsa güvenlik ihlallerinin büyük çoğunluğu bu şekilde gerçekleşir. Güvenliği aşılmış kimlikleri bulma işlemi kolay bir görev değildir. Azure Active Directory, kullanıcı hesaplarınızla ilgili şüpheli eylemleri algılamak için uyarlamalı makine öğrenimi algoritmaları ve buluşsal yöntemler kullanır. Algılanan her şüpheli eylem, risk algılama adlı bir kayıtta saklanır.

## <a name="anonymous-ip-address"></a>Anonim IP adresi

**Algılama türü:** Gerçek zamanlı  
**Eski ad:** Anonim IP adresinden oturum açma işlemleri

Bu risk algılama türü, anonim bir IP adresinden oturum açma işlemlerini belirtir (örneğin, Tor tarayıcısı, anonimleştirici VPN 'Ler).
Bu IP adresleri genellikle kötü amaçlı olabilecek bir amaç için oturum açma telemetrisini (IP adresi, konum, cihaz vb.) gizlemek isteyen aktörler tarafından kullanılır.

## <a name="atypical-travel"></a>Alışılmadık seyahat

**Algılama türü:** Çevrimdışı  
**Eski ad:** Alışılmadık konumlara imkansız seyahat

Bu risk algılama türü, coğrafi olarak uzak konumlardan kaynaklanan iki oturum açma işlemini tanımlar. Bu konumlar, konumların en az birinin Kullanıcı için de normal bir şekilde (geçmiş davranışa göre) de olabileceğini belirtir. Bu makine öğrenimi algoritması, bu iki oturum açma işlemi arasındaki süreyi ve kullanıcının ilk konumdan ikincisine geçebileceği süreyi, farklı bir kullanıcının aynı kullandığını belirten şekilde hesaba götürüyordu. Credentials.

Algoritma, kuruluştaki diğer kullanıcılar tarafından düzenli olarak kullanılan VPN 'Ler ve konumlar gibi mümkün olmayan seyahat koşullarına katkıda bulunan açık "yanlış pozitif sonuçlar" i yoksayar. Sistemin ilk öğrenme süresi en fazla 14 gün veya 10 oturum açma, yeni bir kullanıcının oturum açma davranışını öğreniyor.

## <a name="leaked-credentials"></a>Sızdırılan kimlik bilgileri

**Algılama türü:** Çevrimdışı  
**Eski ad:** Sızan kimlik bilgilerine sahip kullanıcılar

Bu risk algılama türü, kullanıcının geçerli kimlik bilgilerinin sızdırdığını gösterir.
Sidolandırıcılar meşru kullanıcıların geçerli parolalarını tehlikeye ayorsa, Dolandırıcılar genellikle bu kimlik bilgilerini paylaşır. Bu, genellikle koyu Web üzerinde herkese açık bir şekilde gönderilir veya siteleri yapıştırabilir ya da siyah Pazar üzerine kimlik bilgileri satıyor veya satarak yapılır. Microsoft sızdırılan kimlik bilgileri hizmeti, genel ve koyu Web sitelerini izleyerek ve ile çalışarak Kullanıcı adı/parola çiftleri alır:

- Araştırmacılar
- Yasalar zorlaması
- Microsoft 'ta güvenlik ekipleri
- Diğer güvenilir kaynaklar

Hizmet koyu Web 'ten Kullanıcı kimlik bilgilerini edindiğinde, siteleri veya yukarıdaki kaynakları yapıştırarak, geçerli eşleşmeleri bulmak için Azure AD kullanıcılarının geçerli geçerli kimlik bilgileriyle denetlenir.

## <a name="malware-linked-ip-address"></a>Kötü amaçlı yazılım bağlantısı içeren IP adresi

**Algılama türü:** Çevrimdışı  
**Eski ad:** Bulaşma olan cihazlardan oturum açma işlemleri

Bu risk algılama türü, bir bot sunucusuyla etkin bir şekilde iletişim kuran bilinen kötü amaçlı yazılımdan etkilenen IP adreslerinden oturum açma işlemlerini gösterir. Bu, Kullanıcı cihazının IP adreslerinin, bot sunucusu etkinken bir bot sunucusuyla iletişim içinde olan IP adresleriyle ilişkili olduğunu belirler.

## <a name="unfamiliar-sign-in-properties"></a>Bilmediğiniz oturum açma özellikleri

**Algılama türü:** Gerçek zamanlı  
**Eski ad:** Alışılmadık konumlardan oturum açma işlemleri

Bu risk algılama türü, anormal oturum açma işlemlerini aramak için geçen oturum açma geçmişini (IP, Enlem/Boylam ve ASN) dikkate alır. Sistem, bir kullanıcı tarafından kullanılan önceki konumlara ilişkin bilgileri depolar ve bu "tanıdık" konumları dikkate alır. Risk algılama, oturum açma, zaten tanıdık konumlar listesinde olmayan bir konumdan gerçekleştirildiğinde tetiklenir. Yeni oluşturulan kullanıcılar "öğrenme modunda", algoritmalarımız kullanıcının davranışını öğrenirken, bilinmeyen oturum açma özelliklerinin risk algılamalarının devre dışı bırakıldığına ilişkin bir süre için "öğrenme modunda" olacaktır. Öğrenme modu süresi dinamiktir ve kullanıcının oturum açma desenleriyle ilgili yeterli bilgi toplamak için algoritmanın ne kadar zaman aldığını bağlıdır. En kısa süre beş gündür. Bir Kullanıcı, uzun bir süre etkinlik dışı kaldıktan sonra öğrenme moduna geçebilir. Sistem ayrıca tanıdık cihazlardan oturum açma işlemlerini ve coğrafi olarak tanıdık bir konuma yakın konumları yoksayar. 

Bu algılamayı Ayrıca temel kimlik doğrulaması (veya eski protokoller) için de çalıştırdık. Bu protokollerin istemci KIMLIĞI gibi modern özellikleri olmadığından, hatalı pozitif sonuçları azaltmak için sınırlı telemetri vardır. Müşterilerimizin modern kimlik doğrulamasına taşınmasını öneririz.

## <a name="azure-ad-threat-intelligence"></a>Azure AD tehdit bilgileri

**Algılama türü:** Çevrimdışı <br>
**Eski ad:** Bu algılama, eski Azure AD Kimlik Koruması raporlarında (risk için işaretlenen kullanıcılar, risk algılamaları) ' sızdırılan kimlik bilgilerine sahip kullanıcılar ' olarak gösterilir

Bu risk algılama türü, belirli bir kullanıcı için olağandışı olan veya Microsoft 'un dahili ve dış tehdit bilgileri kaynaklarına dayanan bilinen saldırı desenleriyle tutarlı olan kullanıcı etkinliklerini gösterir.

## <a name="admin-confirmed-user-compromised"></a>Yönetici, kullanıcının güvenliğinin tehlikeye girdiğini onayladı

**Algılama türü:** Çevrimdışı <br>
Bu algılama, bir yöneticinin riskli kullanıcılar Kullanıcı arabiriminde veya riskyUsers API 'sini kullanarak ' Kullanıcı güvenliğinin aşılmasına karşı ' yı seçtiğinizi gösterir. Bu kullanıcının hangi yönetici tarafından tehlikede olduğunu görmek için kullanıcının risk geçmişini (UI veya API aracılığıyla) denetleyin.

## <a name="malicious-ip-address"></a>Kötü amaçlı IP adresi

**Algılama türü:** Çevrimdışı <br>
Bu algılama, kötü amaçlı bir IP adresinden oturum açma işlemini gösterir. Aşağıdaki temel alınarak bir IP adresi kötü amaçlı olarak kabul edilir:
-   Yüksek hata ücretleri (IP adresinden alınan geçersiz kimlik bilgileri nedeniyle)
-   Diğer IP saygınlığı kaynakları

## <a name="additional-risk-detected"></a>Ek risk algılandı

**Algılama türü:** Gerçek zamanlı veya çevrimdışı <br>
Bu algılama, yukarıdaki Premium algılamalardan birinin algılandığını gösterir. Premium algılamalar yalnızca Azure AD Premium P2 müşterilerine görünür olduğundan, P2 olmayan müşteriler için "ek risk algılandı" olarak yer alırlar.
