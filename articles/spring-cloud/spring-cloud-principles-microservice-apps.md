---
title: Azure Spring Cloud mikro hizmet uygulamaları için Java ve temel işletim sistemi
description: Azure Spring Cloud mikro hizmet uygulamaları için sağlıklı Java ve temel işletim sistemi bulundurma ilkeleri
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 05/27/2020
ms.custom: devx-track-java
ms.openlocfilehash: 8cda46a011ae92f26a15a4e9a918559801ce299c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90906870"
---
# <a name="java-and-base-os-for-spring-microservice-apps"></a>Spring Mikro Hizmet Uygulamaları için Java ve Temel İşletim Sistemi

**Bu makale şu şekilde geçerlidir:** ✔️ Java

Aşağıda, Spring mikro hizmet uygulamalarına yönelik sağlıklı Java ve temel işletim sistemi bulundurma ilkeleri verilmiştir.
## <a name="principles-for-healthy-java-and-base-os"></a>Sağlıklı Java ve temel işletim sistemi ilkeleri
* Katman genelinde aynı temel işletim sistemi olacaktır-temel | Standart | Premium.
    * Şu anda Azure yay bulutu 'ndaki uygulamalar de, 10 ve Ubuntu 18,04 karışımını kullanır.
    * VMware derleme hizmeti Ubuntu 18,04 kullanır.
* Dağıtım başlangıç noktaları-kaynağından bağımsız olarak aynı temel işletim sistemi olacaktır | Van
    * Şu anda Azure yay bulutu 'ndaki uygulamalar de, 10 ve Ubuntu 18,04 karışımını kullanır.
* Temel işletim sistemi güvenlik açıklarından ücretsiz olacaktır.
    * 10 tabanında temel işletim sisteminde 147 açık con vardır.
    * Ubuntu 18,04 temel işletim sistemi 132 açık CVEs 'e sahiptir.
* JRE-gözetimsiz kullanır.
    * Şu anda Azure yay bulutu 'ndaki uygulamalar JDK kullanır. JRE-gözetimsiz, daha küçük bir görüntüdür.
* , En son Java derlemelerini kullanacaktır.
    * Şu anda Azure yay bulutu 'ndaki uygulamalar Java 8 derleme 242 kullanır. Bu, güncel olmayan bir derleme.
 
Azul sistemleri, temel işletim sistemlerindeki değişiklikleri sürekli olarak tarar ve en son oluşturulan görüntüleri güncel tutar. Azure yay bulutu görüntülerde değişiklik yapar ve bunları dağıtımlar genelinde sürekli olarak güncelleştirir.
 
## <a name="faq-for-azure-spring-cloud"></a>Azure Spring Cloud ile ilgili SSS

* Hangi Java sürümleri destekleniyor? Ana sürüm ve yapı numarası.
    * LTS sürümlerini destekler-Java 8 ve 11.
    * En son derlemeyi kullanır. Örneğin, şu anda, Java 8 derleme 252 ve Java 11 derleme 7.
* Bu Java çalışma zamanlarını kim derlediniz?
    * Azul sistemleri.
* Görüntüler için temel işletim sistemi nedir?
    * Ubuntu 20,04 LTS (odak eden). Uygulamalar Ubuntu 'nın en son LTS sürümünde kalmaya devam eder.
    * Bkz. [Ubuntu 20,04 LTS (odak fossa)](http://releases.ubuntu.com/focal/)
* Yerel dev için desteklenen bir Java çalışma zamanını nasıl indirebilirim? 
    * Bkz [. Azure için JDK 'yi ve Azure Stack yüklemeyi](https://docs.microsoft.com/azure/developer/java/fundamentals/java-jdk-install)
* Java çalışma zamanı düzeyinde sorunlar için nasıl destek alabilirim?
    * Azure desteği ile bir destek bileti açın.
 
## <a name="default-deployment-on-azure-spring-cloud"></a>Azure yay bulutu 'nda varsayılan dağıtım

> ![Varsayılan dağıtım](media/spring-cloud-principles/spring-cloud-default-deployment.png)
 
## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç: ilk Azure Spring Cloud uygulamanızı dağıtın](spring-cloud-quickstart.md)
* [Azure ve Azure Stack için uzun süreli Java desteği](https://docs.microsoft.com/azure/developer/java/fundamentals/java-jdk-long-term-support)
