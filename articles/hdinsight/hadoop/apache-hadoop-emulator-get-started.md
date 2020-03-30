---
title: Apache Hadoop kum havuzu, emülatör kullanmayı öğrenin - Azure HDInsight
description: "Apache Hadoop ekosistemini kullanmayı öğrenmek için, Hortonworks'ten bir Hadoop sanal makinesi kurabilirsiniz. "
keywords: hadoop emülatörü,hadoop kum havuzu
ms.reviewer: jasonh
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/29/2019
ms.author: hrasheed
ms.openlocfilehash: 47ee66393e3e1678576b12a70b767f35cb3bc635
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73044762"
---
# <a name="get-started-with-an-apache-hadoop-sandbox-an-emulator-on-a-virtual-machine"></a>Bir Apache Hadoop kum havuzu, sanal bir makine üzerinde bir emülatör ile başlayın

Hadoop ekosistemi hakkında bilgi edinmek için Hortonworks'teki Apache Hadoop kum havuzunu sanal bir makineye nasıl yükleyeceğinizi öğrenin. Kum havuzu, Hadoop, Hadoop Distributed File System (HDFS) ve iş gönderimi hakkında bilgi edinmek için yerel bir geliştirme ortamı sağlar. Hadoop'u tanıdıktan sonra, bir HDInsight kümesi oluşturarak Azure'da Hadoop'u kullanmaya başlayabilirsiniz. Nasıl başlanınizle ilgili daha fazla bilgi için [HDInsight'ta Hadoop ile başlayın.](apache-hadoop-linux-tutorial-get-started.md)

## <a name="prerequisites"></a>Ön koşullar

* [Oracle VirtualBox](https://www.virtualbox.org/). Buradan indirin [here](https://www.virtualbox.org/wiki/Downloads)ve kurun.

## <a name="download-and-install-the-virtual-machine"></a>Sanal makineyi indirin ve kurun

1. [Cloudera indirmegöz](https://www.cloudera.com/downloads/hortonworks-sandbox/hdp.html)atın.

1. En son Hortonworks Sandbox'ı VM'den indirmek için **Yükleme Türünü Seç'in** altındaki **VIRTUALBOX'ı** tıklatın. Ürün leilgili formu oturum açın veya doldurun.

1. İndirmeye başlamak için **HDP SANDBOX (SON)** düğmesini tıklatın.

Kum havuzunun ayarlanmasıyla ilgili talimatlar için Bkz. [Sandbox Dağıtım ve Yükleme Kılavuzu.](https://hortonworks.com/tutorial/sandbox-deployment-and-install-guide/section/1/)

Eski bir HDP sürümünü indirmek için, **Eski Sürümler**altındaki bağlantılara bakın.

## <a name="start-the-virtual-machine"></a>Sanal makineyi başlatın

1. Oracle VM VirtualBox'ı açın.
1. **Dosya** menüsünden **Cihazı İçe Aktar'ı**tıklatın ve ardından Hortonworks Sandbox görüntüsünü belirtin.
1. Hortonworks Sandbox'ı seçin, **Başlat'ı**ve ardından **Normal Başlangıç'ı**tıklatın. Sanal makine önyükleme işlemini tamamladıktan sonra, oturum açma yönergelerini görüntüler.

    ![virtualbox manager normal başlangıç](./media/apache-hadoop-emulator-get-started/virtualbox-normal-start.png)

1. Bir web tarayıcısı açın ve görüntülenen `http://127.0.0.1:8888`URL'ye gidin (genellikle).

## <a name="set-sandbox-passwords"></a>Sandbox parolalarını ayarlama

1. Hortonworks Sandbox sayfasının **başlangıç** adımından **Gelişmiş Seçenekleri Görüntüle'yi**seçin. SSH kullanarak kum havuzuna giriş yapmak için bu sayfadaki bilgileri kullanın. Sağlanan adı ve parolayı kullanın.

   > [!NOTE]
   > Yüklü bir SSH istemciniz yoksa, sanal makine tarafından sağlanan web tabanlı SSH'yi **http://localhost:4200/** kullanabilirsiniz.

    SSH kullanarak ilk bağlandığınızda, kök hesabın parolasını değiştirmeniz istenir. SSH kullanarak oturum açtığınızda kullandığınız yeni bir parola girin.

2. Oturum açtıktan sonra aşağıdaki komutu girin:

        ambari-admin-password-reset

    İstendiğinde, Ambari yönetici hesabı için bir parola sağlayın. This is used when you access the Ambari Web UI.

## <a name="use-hive-commands"></a>Kovan komutlarını kullanma

1. SSH bağlantısından kum havuzuna, Kovan kabuğunu başlatmak için aşağıdaki komutu kullanın:

        hive
2. Kabuk başladıktan sonra, kum havuzu yla birlikte sağlanan tabloları görüntülemek için aşağıdakileri kullanın:

        show tables;
3. `sample_07` Tablodan 10 satır almak için aşağıdakileri kullanın:

        select * from sample_07 limit 10;

## <a name="next-steps"></a>Sonraki adımlar

* [Hortonworks Sandbox ile Visual Studio'yu nasıl kullanacağınızı öğrenin](../hdinsight-hadoop-emulator-visual-studio.md)

* [Hortonworks Sandbox ipleri öğrenme](https://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)

* [Hadoop öğretici - HDP ile başlarken](https://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)
