---
title: Azure Veri Kutusu ile ön m'deki HDFS deposundan Azure Depolama'ya geçiş yapın
description: Şirket içi HDFS deposundan Azure Depolama'ya veri geçirme
author: normesta
ms.service: storage
ms.date: 02/14/2019
ms.author: normesta
ms.topic: conceptual
ms.subservice: data-lake-storage-gen2
ms.reviewer: jamesbak
ms.openlocfilehash: c0c6a8637223727a9b0c88245d939605f6a8530e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78302009"
---
# <a name="migrate-from-on-prem-hdfs-store-to-azure-storage-with-azure-data-box"></a>Azure Veri Kutusu ile ön m'deki HDFS deposundan Azure Depolama'ya geçiş yapın

Bir Veri Kutusu aygıtı kullanarak, Hadoop kümenizin şirket içi BIR HDFS deposundan verileri Azure Depolamasına (blob depolama veya Veri Gölü Depolama Gen2) aktarabilirsiniz. Bir 80-TB Veri Kutusu veya 770-TB Veri Kutusu Ağır seçim yapabilirsiniz.

Bu makale, bu görevleri tamamlamanıza yardımcı olur:

> [!div class="checklist"]
> * Verilerinizi geçirmenize hazırlanın.
> * Verilerinizi bir Veri Kutusu'na veya Veri Kutusu Ağır aygıtına kopyalayın.
> * Aygıtı Microsoft'a geri gönderin.
> * Dosyalara ve dizinlere erişim izinleri uygulayın (yalnızca Veri Gölü Depolama Gen2)

## <a name="prerequisites"></a>Ön koşullar

Göçü tamamlamak için bunlara ihtiyacın var.

* Azure Depolama hesabı.

* Kaynak verilerinizi içeren şirket içi Hadoop kümesi.

* Azure [Veri Kutusu aygıtı.](https://azure.microsoft.com/services/storage/databox/)

  * Veri Kutunuzu veya [Veri Kutunuzu Ağır](https://docs.microsoft.com/azure/databox/data-box-heavy-deploy-ordered) [Sipariş Edin.](https://docs.microsoft.com/azure/databox/data-box-deploy-ordered) 

  * [Veri Kutunuzu](https://docs.microsoft.com/azure/databox/data-box-deploy-set-up) veya [Veri Kutusu Ağır'ınızı](https://docs.microsoft.com/azure/databox/data-box-heavy-deploy-set-up) şirket içi bir ağa bağlayın ve bağlayın.

Hazırsan, başlayalım.

## <a name="copy-your-data-to-a-data-box-device"></a>Verilerinizi bir Veri Kutusu aygıtına kopyalama

Verileriniz tek bir Veri Kutusu aygıtına uyuyorsa, verileri Veri Kutusu aygıtına kopyalarsınız. 

Veri boyutunuz Veri Kutusu aygıtının kapasitesini aşıyorsa, [verileri birden çok Veri Kutusu aygıtına bölmek için isteğe bağlı yordamı](#appendix-split-data-across-multiple-data-box-devices) kullanın ve ardından bu adımı gerçekleştirin. 

Şirket içi HDFS deponuzdaki verileri bir Veri Kutusu aygıtına kopyalamak için birkaç şey ayarlar sınız ve ardından [DistCp](https://hadoop.apache.org/docs/stable/hadoop-distcp/DistCp.html) aracını kullanırsınız.

Blob/Object depolamanın REST API'leri aracılığıyla Verileri Veri Kutusu aygıtınıza kopyalamak için aşağıdaki adımları izleyin. REST API arabirimi, aygıtın kümenizde bir HDFS deposu olarak görünmesini sağlar.

1. Verileri REST aracılığıyla kopyalamadan önce, Veri Kutusu veya Veri Kutusu Ağır'daki REST arabirimine bağlanmak için güvenlik ve bağlantı ilkellerini tanımlayın. Veri Kutusu'nun yerel web ui'sinde oturum açın ve **Bağlan ve kopyala** sayfasına gidin. Cihazınızın Azure depolama hesabına karşı, **Access ayarları**altında, REST'i bulun ve **REST'i**seçin.

    !["Bağlan ve kopyala" sayfası](media/data-lake-storage-migrate-on-premises-HDFS-cluster/data-box-connect-rest.png)

2. Access depolama hesabı ve yükleme veri iletişim kutusunda **Blob hizmet bitiş noktasını** ve **Depolama hesabı anahtarını**kopyalayın. Blob servisi bitiş noktasından, ve `https://` sondaki eğik çizgiat atla.

    Bu durumda, bitiş noktası: `https://mystorageaccount.blob.mydataboxno.microsoftdatabox.com/`. Uri'nin kullanacağınız ana bilgisayar bölümü: `mystorageaccount.blob.mydataboxno.microsoftdatabox.com`. Örneğin, http üzerinden [REST'e](/azure/databox/data-box-deploy-copy-data-via-rest)nasıl bağlanılabildiğini görün. 

     !["Depolama hesabına erişin ve veri yükleme" iletişim kutusu](media/data-lake-storage-migrate-on-premises-HDFS-cluster/data-box-connection-string-http.png)

3. Her düğüme bitiş noktası ve Veri Kutusu veya `/etc/hosts` Veri Kutusu Ağır düğüm IP adresini ekleyin.

    ```    
    10.128.5.42  mystorageaccount.blob.mydataboxno.microsoftdatabox.com
    ```

    DNS için başka bir mekanizma kullanıyorsanız, Veri Kutusu bitiş noktasının çözülebileceğinden emin olmalısınız.

4. Kabuk değişkenini `azjars` `hadoop-azure` ve `azure-storage` kavanoz dosyalarının konumuna ayarlayın. Bu dosyaları Hadoop yükleme dizininin altında bulabilirsiniz.

    Bu dosyaların var olup olmadığını belirlemek `ls -l $<hadoop_install_dir>/share/hadoop/tools/lib/ | grep azure`için aşağıdaki komutu kullanın: . Hadoop'u `<hadoop_install_dir>` yüklediğiniz yer tutucuyu dizine giden yol ile değiştirin. Tam nitelikli yollar kullandığınızdan emin olun.

    Örnekler:

    `azjars=$hadoop_install_dir/share/hadoop/tools/lib/hadoop-azure-2.6.0-cdh5.14.0.jar` `azjars=$azjars,$hadoop_install_dir/share/hadoop/tools/lib/microsoft-windowsazure-storage-sdk-0.6.0.jar`

5. Veri kopyalama için kullanmak istediğiniz depolama kapsayıcısını oluşturun. Bu komutun bir parçası olarak bir hedef dizini de belirtmelisiniz. Bu noktada bir kukla hedef dizini olabilir.

    ```
    hadoop fs -libjars $azjars \
    -D fs.AbstractFileSystem.wasb.Impl=org.apache.hadoop.fs.azure.Wasb \
    -D fs.azure.account.key.<blob_service_endpoint>=<account_key> \
    -mkdir -p  wasb://<container_name>@<blob_service_endpoint>/<destination_directory>
    ```

    * Yer `<blob_service_endpoint>` tutucuyu blob servis bitiş noktanızın adı ile değiştirin.

    * Yer `<account_key>` tutucuyu hesabınızın erişim anahtarıyla değiştirin.

    * Yer `<container-name>` tutucuyu kabınızın adı ile değiştirin.

    * Yer `<destination_directory>` tutucuyu verilerinizi kopyalamak istediğiniz dizinin adı ile değiştirin.

6. Kapsayıcınızın ve dizininizin oluşturulduğundan emin olmak için bir liste komutu çalıştırın.

    ```
    hadoop fs -libjars $azjars \
    -D fs.AbstractFileSystem.wasb.Impl=org.apache.hadoop.fs.azure.Wasb \
    -D fs.azure.account.key.<blob_service_endpoint>=<account_key> \
    -ls -R  wasb://<container_name>@<blob_service_endpoint>/
    ```

   * Yer `<blob_service_endpoint>` tutucuyu blob servis bitiş noktanızın adı ile değiştirin.

   * Yer `<account_key>` tutucuyu hesabınızın erişim anahtarıyla değiştirin.

   * Yer `<container-name>` tutucuyu kabınızın adı ile değiştirin.

7. Hadoop HDFS'den Data Box Blob depolama alanına, daha önce oluşturduğunuz kapsayıcıya verileri kopyalayın. Kopyaladığınız dizini bulunamazsa, komut otomatik olarak oluşturur.

    ```
    hadoop distcp \
    -libjars $azjars \
    -D fs.AbstractFileSystem.wasb.Impl=org.apache.hadoop.fs.azure.Wasb \
    -D fs.azure.account.key.<blob_service_endpoint<>=<account_key> \
    -filters <exclusion_filelist_file> \
    [-f filelist_file | /<source_directory> \
           wasb://<container_name>@<blob_service_endpoint>/<destination_directory>
    ```

    * Yer `<blob_service_endpoint>` tutucuyu blob servis bitiş noktanızın adı ile değiştirin.

    * Yer `<account_key>` tutucuyu hesabınızın erişim anahtarıyla değiştirin.

    * Yer `<container-name>` tutucuyu kabınızın adı ile değiştirin.

    * Yer `<exlusion_filelist_file>` tutucuyu dosya hariç tutma listenizi içeren dosyanın adı ile değiştirin.

    * Yer `<source_directory>` tutucuyu, kopyalamak istediğiniz verileri içeren dizin adı ile değiştirin.

    * Yer `<destination_directory>` tutucuyu verilerinizi kopyalamak istediğiniz dizinin adı ile değiştirin.

    Bu `-libjars` seçenek, bağımlı `hadoop-azure*.jar` `azure-storage*.jar` dosyaları kullanılabilir hale `distcp`getirmek için kullanılır. Bu zaten bazı kümeler için oluşabilir.

    Aşağıdaki örnek, komutun `distcp` verileri kopyalamak için nasıl kullanıldığını gösterir.

    ```
     hadoop distcp \
    -libjars $azjars \
    -D fs.AbstractFileSystem.wasb.Impl=org.apache.hadoop.fs.azure.Wasb \
    -D fs.azure.account.key.mystorageaccount.blob.mydataboxno.microsoftdatabox.com=myaccountkey \
    -filter ./exclusions.lst -f /tmp/copylist1 -m 4 \
    /data/testfiles \
    wasb://hdfscontainer@mystorageaccount.blob.mydataboxno.microsoftdatabox.com/data
    ```
  
    Kopyalama hızını artırmak için:

    * Haritaperlerin sayısını değiştirmeyi deneyin. (Yukarıdaki örnekte `m` = 4 mappers kullanır.)

    * Birden çok `distcp` paralel çalıştırmayı deneyin.

    * Büyük dosyaların küçük dosyalardan daha iyi performans gösterdiğini unutmayın.

## <a name="ship-the-data-box-to-microsoft"></a>Veri Kutusunu Microsoft'a gönder

Veri Kutusu aygıtını hazırlamak ve Microsoft'a sevk etmek için aşağıdaki adımları izleyin.

1. İlk olarak, [Veri Kutusu veya Veri Kutusu Ağır gemi hazırlayın.](https://docs.microsoft.com/azure/databox/data-box-deploy-copy-data-via-rest)

2. Cihaz hazırlığı tamamlandıktan sonra, ÜRÜN DOSYASI dosyalarını indirin. Azure'a yüklenen verileri doğrulamak için bu PROJE veya bildirim dosyalarını daha sonra kullanırsınız.

3. Cihazı kapatın ve kabloları çıkarın.

4. UPS ile teslim alma zamanı planlayın.

    * Veri Kutusu aygıtları için Bkz. [Veri Kutunuzu İkiyle.](https://docs.microsoft.com/azure/databox/data-box-deploy-picked-up)

    * Veri Kutusu Ağır cihazları için [bkz.](https://docs.microsoft.com/azure/databox/data-box-heavy-deploy-picked-up)

5. Microsoft aygıtınızı aldıktan sonra veri merkezi ağına bağlanır ve veriler aygıt siparişini verirken belirttiğiniz depolama hesabına yüklenir. Tüm verilerinizin Azure'a yüklendiğini BOM dosyalarına karşı doğrulayın. 

## <a name="apply-access-permissions-to-files-and-directories-data-lake-storage-gen2-only"></a>Dosyalara ve dizinlere erişim izinleri uygulayın (yalnızca Veri Gölü Depolama Gen2)

Azure Depolama hesabınızda zaten veri var. Şimdi dosyalara ve dizinlere erişim izinleri uygulayacaksınız.

> [!NOTE]
> Bu adım, yalnızca veri deponuz olarak Azure Veri Gölü Depolama Gen2'yi kullanıyorsanız gereklidir. Veri deponuz olarak hiyerarşik ad alanı olmayan yalnızca bir blob depolama hesabı kullanıyorsanız, bu bölümü atlayabilirsiniz.

### <a name="create-a-service-principal-for-your-azure-data-lake-storage-gen2-account"></a>Azure Veri Gölü Depolama Gen2 hesabınız için bir hizmet ilkesi oluşturun

Bir hizmet ilkesi oluşturmak için [bkz: Kaynaklara erişebilen bir Azure AD uygulaması ve hizmet ilkesi oluşturmak için portalı kullanma.](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal)

* Uygulamanın bir [rol bölümüne atamasını](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#assign-a-role-to-the-application) sağlarken, **Depolama Blob Veri Katılımcısı** rolünü hizmet ilkesine atadığından emin olun.

* Makalenin bölümünde [imzalamak için Alma değerlerindeki](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) adımları gerçekleştirirken, uygulama kimliğini ve istemci gizli değerlerini bir metin dosyasına kaydedin. Yakında bunlara ihtiyacın olacak.

### <a name="generate-a-list-of-copied-files-with-their-permissions"></a>İzinleriyle kopyalanan dosyaların listesini oluşturma

Şirket içi Hadoop kümesinden şu komutu çalıştırın:

```bash

sudo -u hdfs ./copy-acls.sh -s /{hdfs_path} > ./filelist.json
```

Bu komut, kendi izinleri ile kopyalanan dosyaların bir listesini oluşturur.

> [!NOTE]
> HDFS'deki dosya sayısına bağlı olarak, bu komutun çalıştırılaması uzun sürebilir.

### <a name="generate-a-list-of-identities-and-map-them-to-azure-active-directory-add-identities"></a>Kimliklerin bir listesini oluşturun ve bunları Azure Etkin Dizini (ADD) kimliklerle eşleyin

1. Komut `copy-acls.py` dosyasını indirin. İndir [yardımcı komut dosyalarını görün ve kenar düğümünüzü](#download-helper-scripts) bu makalenin bir bölümünü çalıştırmak için ayarlayın.

2. Benzersiz kimliklerin bir listesini oluşturmak için bu komutu çalıştırın.

   ```bash
   
   ./copy-acls.py -s ./filelist.json -i ./id_map.json -g
   ```

   Bu komut dosyası, `id_map.json` ADD tabanlı kimlikler için eşlenebilmeniz gereken kimlikleri içeren adlandırılmış bir dosya oluşturur.

3. Dosyayı `id_map.json` metin düzenleyicisinde açın.

4. Dosyada görünen her JSON nesnesi `target` için, uygun eşlenen kimlikle bir AAD Kullanıcı Adı (UPN) veya ObjectId (OID) özniteliğini güncelleştirin. Bitirdikten sonra dosyayı kaydedin. Bir sonraki adımda bu dosyaya ihtiyacınız olacak.

### <a name="apply-permissions-to-copied-files-and-apply-identity-mappings"></a>Kopyalanan dosyalara izin uygulama ve kimlik eşlemeleri uygulama

Veri Gölü Depolama Gen2 hesabına kopyaladığınız verilere izin uygulamak için bu komutu çalıştırın:

```bash
./copy-acls.py -s ./filelist.json -i ./id_map.json  -A <storage-account-name> -C <container-name> --dest-spn-id <application-id>  --dest-spn-secret <client-secret>
```

* Yer `<storage-account-name>` tutucuyu depolama hesabınızın adı ile değiştirin.

* Yer `<container-name>` tutucuyu kabınızın adı ile değiştirin.

* Hizmet `<application-id>` ilkesini oluşturduğunuzda topladığınız uygulama kimliği ve istemci sırrıile yer `<client-secret>` tutucuları değiştirin.

## <a name="appendix-split-data-across-multiple-data-box-devices"></a>Ek: Verileri birden çok Veri Kutusu aygıtıarasında bölme

Verilerinizi bir Veri Kutusu aygıtına taşımadan önce, bazı yardımcı komut dosyaları indirmeniz, verilerinizin bir Veri Kutusu aygıtına sığacak şekilde düzenlendiğinden emin olmanız ve gereksiz dosyaları hariç tutmanız gerekir.

<a id="download-helper-scripts" />

### <a name="download-helper-scripts-and-set-up-your-edge-node-to-run-them"></a>Yardımcı komut dosyalarını indirin ve kenar düğümünüzü çalıştıracak şekilde ayarlayın

1. Şirket içi Hadoop kümenizin kenar veya baş düğümünüzden şu komutu çalıştırın:

   ```bash
   
   git clone https://github.com/jamesbak/databox-adls-loader.git
   cd databox-adls-loader
   ```

   Bu komut, yardımcı komut ları içeren GitHub deposunu klonlar.

2. JQ paketinin [jq](https://stedolan.github.io/jq/) yerel bilgisayarınıza yüklendiğinden emin olun.

   ```bash
   
   sudo apt-get install jq
   ```

3. [İstekler](https://2.python-requests.org/en/master/) python paketini yükleyin.

   ```bash
   
   pip install requests
   ```

4. Gerekli komut dosyaları üzerinde yürütme izinleri ayarlayın.

   ```bash
   
   chmod +x *.py *.sh

   ```

### <a name="ensure-that-your-data-is-organized-to-fit-onto-a-data-box-device"></a>Verilerinizin bir Veri Kutusu aygıtına sığacak şekilde düzenlendiğinden emin olun

Verilerinizin boyutu tek bir Veri Kutusu aygıtının boyutunu aşıyorsa, dosyaları birden çok Veri Kutusu aygıtında depolayabildiğiniz gruplara bölebilirsiniz.

Verileriniz bir singe Veri Kutusu aygıtının boyutunu aşarsa, bir sonraki bölüme geçebilirsiniz.

1. Yükseltilmiş izinlerle, önceki `generate-file-list` bölümdeki kılavuzu izleyerek indirdiğiniz komut dosyasını çalıştırın.

   Komut parametrelerinin açıklaması aşağıda verilmiştir:

   ```
   sudo -u hdfs ./generate-file-list.py [-h] [-s DATABOX_SIZE] [-b FILELIST_BASENAME]
                    [-f LOG_CONFIG] [-l LOG_FILE]
                    [-v {DEBUG,INFO,WARNING,ERROR}]
                    path

   where:
   positional arguments:
   path                  The base HDFS path to process.

   optional arguments:
   -h, --help            show this help message and exit
   -s DATABOX_SIZE, --databox-size DATABOX_SIZE
                        The size of each Data Box in bytes.
   -b FILELIST_BASENAME, --filelist-basename FILELIST_BASENAME
                        The base name for the output filelists. Lists will be
                        named basename1, basename2, ... .
   -f LOG_CONFIG, --log-config LOG_CONFIG
                        The name of a configuration file for logging.
   -l LOG_FILE, --log-file LOG_FILE
                        Name of file to have log output written to (default is
                        stdout/stderr)
   -v {DEBUG,INFO,WARNING,ERROR}, --log-level {DEBUG,INFO,WARNING,ERROR}
                        Level of log information to output. Default is 'INFO'.
   ```

2. Oluşturulan dosya [listelerini, DIsCp](https://hadoop.apache.org/docs/stable/hadoop-distcp/DistCp.html) işi için erişilebilmeleri için HDFS'ye kopyalayın.

   ```
   hadoop fs -copyFromLocal {filelist_pattern} /[hdfs directory]
   ```

### <a name="exclude-unnecessary-files"></a>Gereksiz dosyaları hariç tutma

Bazı dizinleri DisCp işinden çıkarmanız gerekir. Örneğin, kümenin çalışmasını sağlayan durum bilgilerini içeren dizinleri hariç tutun.

DistCp işini başlatmayı planladığınız şirket içi Hadoop kümesinde, hariç tutmak istediğiniz dizinlistesini belirten bir dosya oluşturun.

Bir örneği aşağıda verilmiştir:

```
.*ranger/audit.*
.*/hbase/data/WALs.*
```

## <a name="next-steps"></a>Sonraki adımlar

Data Lake Storage Gen2'nin HDInsight kümeleriyle nasıl çalıştığını öğrenin. Bkz. [Azure HDInsight kümeleriyle Azure Veri Gölü Depolama Gen2'yi kullanın.](../../hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2.md)
