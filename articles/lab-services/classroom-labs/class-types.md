---
title: Azure Laboratuvar Hizmetlerinde örnek sınıf türleri | Microsoft Dokümanlar
description: Azure Laboratuvar Hizmetleri'ni kullanarak laboratuvarlar kurabileceğiniz bazı sınıf türleri sağlar.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/30/2019
ms.author: spelluru
ms.openlocfilehash: 27619a69a1f7fbded8ce6430afc2b8e9a8b4a00c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79296736"
---
# <a name="class-types-overview---azure-lab-services"></a>Sınıf türlerine genel bakış - Azure Lab Hizmetleri

Azure Laboratuvar Hizmetleri, bulutta sınıf laboratuvar ortamlarını hızla ayarlamanızı sağlar. Bu bölümdeki makaleler, Azure Lab Hizmetlerini kullanarak çeşitli sınıf laboratuvarları nasıl ayarlanıncaya kadar yol gösterici dir.

## <a name="deep-learning-in-natural-language-processing"></a>Doğal dil işlemede derin öğrenme

Azure Laboratuvar Hizmetleri'ni kullanarak doğal dil işlemede (NLP) derin öğrenmeye odaklanmış bir laboratuvar kurabilirsiniz. Doğal dil işleme (NLP), çeviri, konuşma tanıma ve diğer dil anlama özelliklerine sahip bilgisayarlara olanak tanıyan bir yapay zeka (AI) biçimidir. NLP sınıfına giren öğrenciler, yazılı insan dilini analiz etmek için kullanılan derin öğrenme modelleri geliştirmek için sinir ağı algoritmalarını nasıl uygulayacaklarını öğrenmek için bir Linux sanal makinesi (VM) alır.

Bu tür bir laboratuvarın nasıl kurulacak hakkında ayrıntılı bilgi [için](class-type-deep-learning-natural-processing.md)bkz.

## <a name="shell-scripting-on-linux"></a>Linux üzerinde kabuk betiği oluşturma

Linux'ta kabuk komut dosyası öğretmek için bir laboratuvar kurabilirsiniz. Komut dosyası, yöneticilerin yinelenen görevlerden kaçınmasına olanak tanıyan sistem yönetiminin yararlı bir parçasıdır. Bu örnek senaryoda, sınıf geleneksel bash komut dosyalarını ve gelişmiş komut dosyalarını kapsar. Geliştirilmiş komut dosyaları bash komutları ve Ruby birleştiren komut dosyalarıvardır. Bu yaklaşım, Ruby'nin verileri çevreye aktarmasını ve komutların kabukla etkileşimkurmasını sağlar.

Bu komut dosyası sınıflarını alan öğrenciler, Linux'un temellerini öğrenmek için bir Linux sanal makine olsun, ve aynı zamanda bash kabuk komut dosyası aşina olsun. Linux sanal makine uzak masaüstü erişimi etkin ve [gedit](https://help.gnome.org/users/gedit/stable/) ve [Visual Studio Code](https://code.visualstudio.com/) metin editörleri yüklü ile birlikte gelir.

Bu tür bir laboratuvarın nasıl kurulabildiğini hakkında ayrıntılı bilgi için [Linux'ta Shell komut dosyası na](class-type-shell-scripting-linux.md)bakın.

## <a name="ethical-hacking"></a>Etik korsanlık

Etik hacklemenin adli tıp tarafına odaklanan bir sınıf için bir laboratuvar kurabilirsiniz. Etik korsanlık topluluğu tarafından kullanılan bir uygulama olan penetrasyon testi, birisi kötü amaçlı bir saldırganın yararlanabileceği güvenlik açıklarını göstermek için sisteme veya ağa erişmeye çalıştığında oluşur.

Etik bir hack sınıfında, öğrenciler güvenlik açıklarına karşı savunmak için modern teknikler öğrenebilirler. Her öğrenci iki iç içe sanal makine olan bir Windows Server ana bilgisayar sanal makine alır - [Metasploitable3](https://github.com/rapid7/metasploitable3) görüntü ve [Kali Linux](https://www.kali.org/) görüntü ile başka bir makine ile bir sanal makine. Metasploitable sanal makine yararlanma amaçlı kullanılır.  Kali Linux sanal makine adli görevleri yürütmek için gerekli araçlara erişim sağlar.

Bu tür bir laboratuvarın nasıl kurulabildiğini hakkında ayrıntılı bilgi için [bkz.](class-type-ethical-hacking.md)

## <a name="database-management"></a>Veritabanı yönetimi
Veritabanları kavramları, üniversitedeki Bilgisayar Bilimleri bölümlerinin çoğunda öğretilen giriş derslerinden biridir. Azure Lab Hizmetleri'nde temel veritabanları yönetimi sınıfı için bir laboratuvar ayarlayabilirsiniz. Örneğin, [MySQL](https://www.mysql.com/) Database Server veya [SQL Server 2019](https://www.microsoft.com/sql-server/sql-server-2019) sunucusu yla bir laboratuvarda sanal makine şablonu ayarlayabilirsiniz.

Bu tür bir laboratuvarın nasıl ayarlanılabilen ayrıntılı bilgi için [bkz.](class-type-database-management.md)

## <a name="python-and-jupyter-notebooks"></a>Python ve Jupyter dizüstü bilgisayarlar
Öğrencilere [Jupyter Notebook'ları](http://jupyter-notebook.readthedocs.io)nasıl kullanacaklarını öğretmek için gereken araçlarla Azure Lab Hizmetleri'nde bir şablon makinesi ayarlayabilirsiniz. Jupyter Notebooks, zengin metin ve çalıştırılabilir [Python](https://www.python.org/) kaynak kodunu not defteri adı verilen tek bir tuvalde kolayca birleştirmenizi sağlayan açık kaynaklı bir projedir. Not defterini çalıştırmak, girdi ve çıktıların doğrusal bir kaydıyla sonuçlanır.  Bu çıktılar metin, bilgi tabloları, dağılım çizimleri ve daha fazlasını içerebilir.

Bu tür bir laboratuvarın nasıl kurulabildiğini hakkında ayrıntılı bilgi için python [ve Jupyter Notebook'larla veri bilimi öğretmek üzere bir laboratuvar kur'a](class-type-jupyter-notebook.md)bakın.

## <a name="mobile-app-development-with-android-studio"></a>Android Studio ile mobil uygulama geliştirme
Azure Lab Hizmetleri'nde bir giriş mobil uygulama geliştirme sınıfı öğretmek için bir laboratuvar kurabilirsiniz. Bu [sınıf, Google Play Store'da](https://play.google.com/store/apps)yayınlanabilen Android mobil uygulamalarına odaklanır.  Öğrenciler uygulama oluşturmak için [Android Studio'yu](https://developer.android.com/studio) nasıl kullanacaklarını öğrenirler.  [Android için Visual Studio Emülatörü](https://visualstudio.microsoft.com/vs/msft-android-emulator/) yerel uygulama test etmek için kullanılır.

Bu tür bir laboratuvarın nasıl kurulacağı hakkında ayrıntılı bilgi [için, Android Studio ile mobil uygulama gelişimini öğretmek için bir laboratuvar düzenle](class-type-mobile-dev-android-studio.md)


## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelere bakın:

- [Azure Lab Hizmetlerini kullanarak doğal dil işlemede derin öğrenmeye odaklanan bir laboratuvar kurun](class-type-deep-learning-natural-processing.md)
- [Linux üzerinde kabuk betiği oluşturma](class-type-shell-scripting-linux.md)
- [Etik korsanlık](class-type-ethical-hacking.md)