---
title: Azaltıcı Etkenler - Microsoft Tehdit Modelleme Aracı - Azure | Microsoft Dokümanlar
description: Microsoft Tehdit Modelleme Aracı'nın en çok maruz kalan tehditlere olası çözümleri vurgulayan azaltıcı etkenler sayfası.
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
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 748d10b994080b667885e5d0d5f4d688269e86ab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68728041"
---
# <a name="microsoft-threat-modeling-tool-mitigations"></a>Microsoft Tehdit Modelleme Aracı azaltma

Tehdit Modelleme Aracı, Microsoft Güvenlik Geliştirme Yaşam Döngüsü'nün (SDL) temel bir öğesidir. Yazılım mimarlarının, çözümlenebilmek için nispeten kolay ve uygun maliyetli olduklarında olası güvenlik sorunlarını erkenden tanımlamasına ve azaltmasına olanak tanır. Sonuç olarak, büyük ölçüde geliştirme toplam maliyetini azaltır. Ayrıca, tehdit modelleri oluşturma ve analiz etme konusunda net rehberlik sağlayarak tehdit modellemeyi tüm geliştiriciler için daha kolay hale getirerek aracı güvenlik dışı uzmanları göz önünde bulundurarak tasarladık.

Bugün başlamak için **[Tehdit Modelleme Aracı'nı](threat-modeling-tool.md)** ziyaret edin!

## <a name="mitigation-categories"></a>Azaltma kategorileri

Tehdit Modelleme Aracı azaltıcı etkenler, aşağıdakilerden oluşan Web Uygulama Güvenlik Çerçevesi'ne göre sınıflandırılır:

| Kategori | Açıklama |
| -------- | ----------- |
| **[Denetim ve Günlük](threat-modeling-tool-auditing-and-logging.md)** | Kim ne zaman ve ne yaptı? Denetim ve günlüğe kaydetme, uygulamanızın güvenlikle ilgili olayları nasıl kaydettiğine ilişkin |
| **[Kimlik doğrulaması](threat-modeling-tool-authentication.md)** | Kimsin? Kimlik doğrulama, bir varlığın genellikle kullanıcı adı ve parola gibi kimlik bilgileri aracılığıyla başka bir varlığın kimliğini kanıtladığı işlemdir. |
| **[Yetkilendirme](threat-modeling-tool-authorization.md)** | Ne yapabilirsin ki? Yetkilendirme, uygulamanızın kaynaklar ve işlemler için erişim denetimlerini sağlama şeklidir |
| **[İletişim Güvenliği](threat-modeling-tool-communication-security.md)** | Kiminle konuşuyorsun? İletişim Güvenliği, yapılan tüm iletişimin mümkün olduğunca güvenli olmasını sağlar |
| **[Yapılandırma Yönetimi](threat-modeling-tool-configuration-management.md)** | Başvurunuz kimler gibi çalışır? Hangi veritabanlarına bağlanır? Başvurunuz nasıl yönetilir? Bu ayarlar nasıl güvenli? Yapılandırma yönetimi, uygulamanızın bu operasyonel sorunları nasıl ele adediyle ele adadığını ifade eder |
| **[Şifreleme](threat-modeling-tool-cryptography.md)** | Nasıl sır saklıyorsun (gizlilik)? Verilerinizi veya kitaplıklarınızı (bütünlük) nasıl kurcalamaya karşı laşıyorsun? Şifreleme açısından güçlü olması gereken rastgele değerler için tohumları nasıl sağlıyorsunuz? Şifreleme, uygulamanızın gizliliği ve bütünlüğü nasıl uyguladığını ifade eder |
| **[İstisna Yönetimi](threat-modeling-tool-exception-management.md)** | Uygulamanızdaki bir yöntem çağrısı başarısız olduğunda, uygulamanız ne yapar? Ne kadarını ortaya çıkarıyorsun? Son kullanıcılara kolay hata bilgileri iade ediyor musunuz? Değerli özel durum bilgilerini arayana geri mi aktarıyorsun? Başvurunuz zarafetle başarısız oluyor mu? |
| **[Giriş Doğrulama](threat-modeling-tool-input-validation.md)** | Uygulamanızın aldığı girişin geçerli ve güvenli olduğunu nasıl anlarsınız? Giriş doğrulama, uygulamanızın ek işleme den önce girişi nasıl filtrelediğini, önlüğü veya reddettiği anlamına gelir. Giriş noktaları üzerinden girdiyi zorlamayı ve çıkış noktalarından çıktıkodlamayı düşünün. Veritabanları ve dosya paylaşımları gibi kaynaklardan gelen verilere güveniyor musunuz? |
| **[Hassas Veriler](threat-modeling-tool-sensitive-data.md)** | Uygulamanız hassas verileri nasıl işler? Hassas veriler, uygulamanızın bellekte, ağ üzerinden veya kalıcı depolarda korunması gereken verileri nasıl işlediğiyle ilgilidir |
| **[Oturum Yönetimi](threat-modeling-tool-session-management.md)** | Uygulamanız kullanıcı oturumlarını nasıl işler ve korur? Oturum, kullanıcı ile Web uygulamanız arasındaki bir dizi ilgili etkileşimi ifade eder |

Bu, şunları belirlemenize yardımcı olur:

* En sık yapılan hatalar nerede?
* En işlem uygulanabilir geliştirmeler nerede

Sonuç olarak, bu kategorileri güvenlik çalışmanızı odaklamak ve önceliklendirmek için kullanırsınız, böylece giriş doğrulama, kimlik doğrulama ve yetkilendirme kategorilerinde en yaygın güvenlik sorunlarının oluştuğunu biliyorsanız, buradan başlayabilirsiniz. Daha fazla bilgi için bu patent bağlantısını ziyaret ** [edin](https://www.google.com/patents/US7818788)**

## <a name="next-steps"></a>Sonraki adımlar

Aracın olası tasarım tehditleri oluşturmak için kullandığı tehdit kategorileri hakkında daha fazla bilgi edinmek için **[Tehdit Modelleme Aracı Tehditleri'ni](threat-modeling-tool-threats.md)** ziyaret edin.