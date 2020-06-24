---
title: Anlık ileti hızlı başlangıç
description: Sorunsuz ve Azure SQL veri ambarı ile hızlı bir şekilde çalışmaya başlayın.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 10/12/2018
ms.author: martinle
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 945bcd03bc3bf13517836e7a5624bd5142782183
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85208561"
---
# <a name="striim-azure-sql-dw-marketplace-offering-install-guide"></a>Anlık ileti Azure SQL DW marketi teklif yüklemesi Kılavuzu

Bu hızlı başlangıçta zaten SQL veri ambarı 'nın önceden var olan bir örneğine sahip olduğunuz varsayılır.

Azure Marketi 'nde çarpıcı anlık Ileti arayın ve SQL Data Warehouse 'a veri tümleştirme (hazırlandı) seçeneği için anlık ileti al seçeneğini belirleyin 

![Anlık ileti al][install]

Her şeye, bir anlık ileti kümesi adı, parola ve yönetici parolasını gösteren belirtilen özelliklerle çarpıcı anlık VM 'yi yapılandırın

![Çarpıcı anlık ileti yapılandırma][configure]

Dağıtıldıktan sonra, \<VM Name> Azure Portal sağ masternode ' a tıklayın, Bağlan ' a tıklayın ve VM yerel hesabı 'nı kullanarak oturum açma 'yı kopyalayın 

![SQL Data Warehouse 'a anlık ileti bağlama][connect]

Sqljdbc42. jar dosyasını <https://www.microsoft.com/en-us/download/details.aspx?id=54671> Yerel makinenize indirin. 

Bir komut satırı penceresi açın ve dizinleri, JDBC jar dosyasını indirdiğiniz yere değiştirin. SCP dosyasını, Azure portal adresini ve parolayı alarak, e-mesajlaşma sanal makinenize kaya

![Jar dosyasını sanal makinenize kopyalayın][copy-jar]

Başka bir komut satırı penceresi açın veya çarpıcı anlık ileti kümesine SSH için SSH yardımcı programını kullanın

![Kümeye SSH][ssh]

JDBC jar dosyasını İlerim 'in lib dizinine taşımak ve sunucuyu başlatıp durdurmak için aşağıdaki komutları yürütün.

   1. sudo su
   2. CD/tmp
   3. MV sqljdbc42. jar/seçenek/gün ım/lib
   4. systemctl durdur ım-Node
   5. systemctl durdur anlık ileti-DBMS
   6. systemctl Başlat anlık ileti-DBMS
   7. systemctl başlangıç anlık ileti-düğüm

![Çarpıcı anlık ileti kümesini başlatma][start-striim]

Şimdi, en sevdiğiniz tarayıcıyı açın ve şuraya gidin \<DNS Name> : 9080

![Oturum açma ekranına git][navigate]

Azure portal, Kullanıcı adı ve parola ile oturum açın ve kullanmaya başlamak için tercih ettiğiniz Sihirbazı seçin ya da sürükle ve bırak Kullanıcı arabirimini kullanmaya başlamak için uygulamalar sayfasına gidin

![Sunucu kimlik bilgileriyle oturum açma][login]



[install]: ./media/striim-quickstart/install.png
[configure]: ./media/striim-quickstart/configure.png
[connect]:./media/striim-quickstart/connect.png
[copy-jar]:./media/striim-quickstart/copy-jar.png
[ssh]:./media/striim-quickstart/ssh.png
[start-striim]:./media/striim-quickstart/start-striim.png
[navigate]:./media/striim-quickstart/navigate.png
[login]:./media/striim-quickstart/login.png
