---
title: Striim hızlı başlangıç
description: Striim ve Azure SQL Veri Ambarı ile hızlı bir şekilde başlayın.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 10/12/2018
ms.author: martinle
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: e963d62df8d2c416726852ee3d46daf1cd22936e
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80349936"
---
# <a name="striim-azure-sql-dw-marketplace-offering-install-guide"></a>Striim Azure SQL DW Marketplace Teklif Yükleme Kılavuzu

Bu hızlı başlatma, zaten SQL Veri Ambarı'nın önceden varolan bir örneğine sahip olduğunuzu varsayar.

Azure Marketi'nde Striim'i arayın ve SQL Veri Ambarına Veri Tümleştirmesi (Aşamalı) seçeneğini seçin 

![Striim'i yükle][install]

Striim VM'yi belirtilen özelliklerle yapılandırArak Striim küme adını, parolasını ve yönetici parolasını belirtin

![Striim'i yapılandır][configure]

Dağıtıldıktan sonra Azure \<portalındaki VM Adı> masternode'una tıklayın, Bağlan'ı tıklatın ve VM yerel hesabını kullanarak Giriş'i kopyalayın 

![Striim'i SQL Veri Ambarına Bağla][connect]

Sqljdbc42.jar'ı <https://www.microsoft.com/en-us/download/details.aspx?id=54671> yerel makinenize indirin. 

Komut satırı penceresini açın ve dizinleri JDBC jarını karşıdan yüklediğiniz yerle değiştirin. SCP jar dosyasını Striim VM'nize, adresi ve parolayı Azure portalından alın

![Jar dosyasını VM'nize kopyalayın][copy-jar]

Başka bir komut satırı penceresi açın veya Striim kümesine ssh için bir ssh yardımcı programı kullanın

![Kümeye SSH][ssh]

JDBC jar dosyasını Striim'in lib dizinine taşımak ve sunucuyu başlatmak ve durdurmak için aşağıdaki komutları uygulayın.

   1. sudo su
   2. cd /tmp
   3. mv sqljdbc42.jar /opt/striim/lib
   4. systemctl stop striim-düğüm
   5. systemctl durdurmak striim-dbms
   6. systemctl başlangıç striim-dbms
   7. systemctl başlangıç striim-düğüm

![Striim kümesini başlatın][start-striim]

Şimdi, en sevdiğiniz tarayıcıyı \<açın ve DNS Adı>:9080 gidin

![Giriş ekranına gidin][navigate]

Azure portalında ayarladığınız kullanıcı adı ve parolayla oturum açın ve başlamak için tercih ettiğiniz sihirbazı seçin veya sürükle ve bırakma Kullanıcı Arasını kullanmaya başlamak için Uygulamalar sayfasına gidin

![Sunucu kimlik bilgileriyle giriş yapın][login]



[install]: ./media/striim-quickstart/install.png
[configure]: ./media/striim-quickstart/configure.png
[connect]:./media/striim-quickstart/connect.png
[copy-jar]:./media/striim-quickstart/copy-jar.png
[ssh]:./media/striim-quickstart/ssh.png
[start-striim]:./media/striim-quickstart/start-striim.png
[navigate]:./media/striim-quickstart/navigate.png
[login]:./media/striim-quickstart/login.png
