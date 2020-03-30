---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 12/11/2018
ms.author: cherylmc
ms.openlocfilehash: 31ccf14c82f6248c74d6af932fe9e338d26d2747
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188233"
---
Bir VNet'e Bağlandığınız ve Site'ye Noktadan Sayfa bağlantısı olan her istemci bilgisayarın bir istemci sertifikası yüklü olması gerekir. Kök sertifikadan oluşturur ve her istemci bilgisayara yüklersiniz. Geçerli bir istemci sertifikası yüklemezseniz, istemci VNet'e bağlanmaya çalıştığında kimlik doğrulama başarısız olur.

Her istemci için benzersiz bir sertifika oluşturabileceğiniz gibi, birden çok istemci için aynı sertifikayı da kullanabilirsiniz. Benzersiz istemci sertifikaları oluşturmanın avantajı, tek bir sertifikayı iptal edebiliyor olmanızdır. Aksi takdirde, birden çok istemci kimlik doğrulaması için aynı istemci sertifikasını kullanırsa ve siz bunu iptal ederseniz, bu sertifikayı kullanan her istemci için yeni sertifikalar oluşturmanız ve yüklemeniz gerekir.

Aşağıdaki yöntemleri kullanarak istemci sertifikaları oluşturabilirsiniz:

- **Kurumsal sertifika:**

  - Kurumsal sertifika çözümü kullanıyorsanız, ortak ad değeri biçimi *adı\@yourdomain.com*olan bir istemci sertifikası oluşturun. *Etki alanı adı\kullanıcı adı* biçimi yerine bu biçimi kullanın.
  - İstemci sertifikasının, kullanıcı listesindeki ilk öğe olarak listelenen *Müşteri Kimlik Doğrulaması* olan bir kullanıcı sertifikası şablonunu temel aldığından emin olun. Sertifikayı çift tıklatarak ve Gelişmiş **Anahtar Kullanımını** **Ayrıntılar** sekmesinde görüntüleyerek kontrol edin.

- **Kendi imzalı kök sertifikası:** Oluşturduğunuz istemci sertifikalarının P2S bağlantılarınızla uyumlu olması için aşağıdaki P2S sertifika makalelerinden birinde ki adımları izleyin. Bu makalelerdeki adımlar uyumlu bir istemci sertifikası oluşturur: 

  * [Windows 10 PowerShell yönergeleri](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientcert): Bu yönergeler, sertifika oluşturmak için Windows 10 ve PowerShell gerektirir. Oluşturulan sertifikalar desteklenen herhangi bir P2S istemcisi üzerinde yüklenebilir.
  * [MakeCert yönergeleri](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md): Sertifika oluşturmak için Windows 10 bilgisayarına erişiminiz yoksa MakeCert'i kullanın. MakeCert amortismana hazır olsa da, sertifika oluşturmak için yine de kullanabilirsiniz. Oluşturulan sertifikaları desteklenen herhangi bir P2S istemcisine yükleyebilirsiniz.
  * [Linux yönergeleri](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-linux.md)

  Kendi imzaladığı kök sertifikadan bir istemci sertifikası oluşturduğunuzda, bu sertifikayı oluşturmak için kullandığınız bilgisayara otomatik olarak yüklenir. İstemci sertifikasını başka bir istemci bilgisayara yüklemek istiyorsanız, tüm sertifika zinciriyle birlikte .pfx dosyası olarak dışa aktarın. Bunu yapmak, istemcinin kimliğini doğrulaması için gereken kök sertifika bilgilerini içeren bir .pfx dosyası oluşturur. 

**Sertifikayı dışa aktarmak için**

Sertifika verme adımları için [PowerShell'i kullanarak Site'ye Puan lı sertifikalar oluştur ve dışa aktar'a](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientexport)bakın.
