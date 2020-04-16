---
title: SSH ile SFTP sunucusuna bağlanın
description: SSH ve Azure Mantık Uygulamaları kullanarak bir SFTP sunucusu için dosyaları izleyen, oluşturan, yöneten, gönderen ve alan görevleri otomatikleştirin
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.reviewer: estfan, logicappspm
ms.topic: article
ms.date: 04/13/2020
tags: connectors
ms.openlocfilehash: d7fafdd5830ec2825771d4d611a5f4bd5d87260a
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81393627"
---
# <a name="monitor-create-and-manage-sftp-files-by-using-ssh-and-azure-logic-apps"></a>SSH ve Azure Mantık Uygulamalarını kullanarak SFTP dosyalarını izleme, oluşturma ve yönetme

Güvenli Dosya [Aktarım Protokolü (SFTP) protokolü kullanarak Güvenli Dosya Aktarım Protokolü (SFTP)](https://www.ssh.com/ssh/sftp/) sunucusunda dosyaları izleyen, oluşturan, gönderen ve alan görevleri otomatikleştirmek için Azure Logic Apps ve SFTP-SSH bağlayıcısını kullanarak tümleştirme iş akışlarını oluşturabilir ve otomatikleştirebilirsiniz. [Secure Shell (SSH)](https://www.ssh.com/ssh/protocol/) SFTP, herhangi bir güvenilir veri akışı üzerinde dosya erişimi, dosya aktarımı ve dosya yönetimi sağlayan bir ağ protokolüdür. Otomatikleştirebileceğiniz bazı örnek görevler şunlardır:

* Dosyalar eklendiğinde veya değiştirildiğinde izleyin.
* Dosyaları alın, oluşturun, kopyalayın, yeniden adlandırın, güncelleyin, listeleyin ve silin.
* Klasörler oluşturun.
* Dosya içeriği ve meta veriler alın.
* Arşivleri klasörlere ayıklayın.

SFTP sunucunuzdaki olayları izleyen ve çıktıyı diğer eylemler için kullanılabilir hale getiren tetikleyiciler kullanabilirsiniz. SFTP sunucunuzda çeşitli görevleri gerçekleştiren eylemleri kullanabilirsiniz. Ayrıca, mantık uygulamanızda SFTP eylemlerinden elde edilen çıktıyı kullanarak başka eylemler de alabilirsiniz. Örneğin, SFTP sunucunuzdan düzenli olarak dosya alırsanız, Office 365 Outlook bağlayıcısını veya Outlook.com bağlayıcısını kullanarak bu dosyalar ve içerikleri hakkında e-posta uyarıları gönderebilirsiniz. Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları nedir'yi inceleyin?](../logic-apps/logic-apps-overview.md)

SFTP-SSH konektörü ile SFTP konektörü arasındaki farklar için, bu konunun ilerleyen bölümlerinde [SFTP-SSH ile SFTP](#comparison) arasındaki karşılaştır bölümünü inceleyin.

## <a name="limits"></a>Sınırlar

* [Öbeklenmeyi](../logic-apps/logic-apps-handle-large-messages.md) destekleyen SFTP-SSH eylemleri 1 GB'a kadar dosyaları işleyebilirken, öbeklenmeyi desteklemeyen SFTP-SSH eylemleri 50 MB'a kadar dosyaları işleyebilir. Varsayılan yığın boyutu 15 MB olmasına rağmen, ağ gecikmesi, sunucu yanıt süresi ve benzeri etkenlere bağlı olarak, bu boyut 5 MB'dan başlayarak kademeli olarak maksimum 50 MB'a kadar artarak dinamik olarak değişebilir.

  > [!NOTE]
  > [Bir entegrasyon hizmeti ortamındaki (İmKB)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)mantık uygulamaları için, bu bağlayıcının İmKB etiketli sürümü bunun yerine [İmKB ileti sınırlarını](../logic-apps/logic-apps-limits-and-config.md#message-size-limits) kullanır.

  Bunun yerine kullanmak için [sabit bir yığın boyutu belirttiğiniz](#change-chunk-size) zaman bu uyarlanabilir davranışı geçersiz kılabilirsiniz. Bu boyut 5 MB ile 50 MB arasında değişebilir. Örneğin, 45 MB dosyanız ve bu dosya boyutunu gecikmeden desteklenebilen bir ağınız olduğunu varsayalım. Uyarlanabilir ötme, birkaç çağrıyla sonuçlanır, daha çok bir arama. Arama sayısını azaltmak için 50 MB'lık bir yığın boyutu ayarlamayı deneyebilirsiniz. Farklı senaryoda, mantık uygulamanız zamanlama yıkıyorsa, örneğin 15 MB'lık parçalar kullanırken, boyutu 5 MB'a düşürmeyi deneyebilirsiniz.

  Yığın boyutu bir bağlantıyla ilişkilidir, bu da öbeklenmeyi destekleyen eylemler ve ardından ötmeyi desteklemeyen eylemler için aynı bağlantıyı kullanabileceğiniz anlamına gelir. Bu durumda, öbeklenmeyi desteklemeyen eylemler için yığın boyutu 5 MB ile 50 MB arasında değişir. Bu tablo, Hangi SFTP-SSH eylemlerinin ötme yi desteklediğini gösterir:

  | Eylem | Chunking desteği | Yığın boyutu desteğini geçersiz kılma |
  |--------|------------------|-----------------------------|
  | **Dosyayı kopyalama** | Hayır | Uygulanamaz |
  | **Dosya oluşturma** | Evet | Evet |
  | **Klasör oluşturma** | Uygulanamaz | Uygulanamaz |
  | **Dosyayı silme** | Uygulanamaz | Uygulanamaz |
  | **Arşivi klasöre ayıklama** | Uygulanamaz | Uygulanamaz |
  | **Dosya içeriğini alma** | Evet | Evet |
  | **Yolu kullanarak dosya içeriğini alma** | Evet | Evet |
  | **Dosya meta verilerini alma** | Uygulanamaz | Uygulanamaz |
  | **Yolu kullanarak dosya meta verilerini alma** | Uygulanamaz | Uygulanamaz |
  | **Klasördeki dosyaları listele** | Uygulanamaz | Uygulanamaz |
  | **Dosyayı yeniden adlandır** | Uygulanamaz | Uygulanamaz |
  | **Dosyayı güncelleştir** | Hayır | Uygulanamaz |
  ||||

* SFTP-SSH tetikleyicileri ileti ödentiyi desteklemez. Dosya içeriği isteğinde bulunurken, yalnızca 15 MB veya daha küçük dosyaları seçin. 15 MB'dan büyük dosyaları almak için aşağıdaki deseni izleyin:

  1. **Bir dosya eklendiğinde veya değiştirildiğinde (yalnızca özellikler)** gibi yalnızca dosya özelliklerini döndüren bir SFTP-SSH tetikleyicisi kullanın.

  1. Dosyanın tamamını okuyan ve dolaylı olarak ileti ödeneği kullanan SFTP-SSH **Get dosya içeriği** eylemi ile tetikleyiciyi izleyin.

<a name="comparison"></a>

## <a name="compare-sftp-ssh-versus-sftp"></a>SFTP-SSH ile SFTP'yi karşılaştırın

SFTP-SSH konektörü ile SFTP-SSH konektörün bu özelliklere sahip olduğu SFTP-SSH konektörü arasındaki diğer önemli farklar şunlardır:

* .NET'i destekleyen açık kaynak kodlu Secure Shell (SSH) kitaplığı olan [SSH.NET kitaplığını](https://github.com/sshnet/SSH.NET)kullanır.

* SFTP sunucusunda belirtilen yolda bir klasör oluşturan **Klasör** Oluştur eylemini sağlar.

* SFTP sunucusundaki bir dosyayı yeniden adlandıran **Yeniden Adlandırma dosyası** eylemini sağlar.

* SFTP sunucusuna olan bağlantıyı *1 saate kadar*önbelleğe alan bu da performansı artırır ve sunucuya bağlanma girişimsayısını azaltır. Bu önbelleğe alma davranışının süresini ayarlamak için, SFTP sunucunuzdaki SSH [**yapılandırmasındaClientAliveInterval**](https://man.openbsd.org/sshd_config#ClientAliveInterval) özelliğini düzenleme.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Mantık uygulamanızın SFTP hesabınıza erişmesini sağlayan SFTP sunucu adresiniz ve hesap kimlik bilgileriniz. Ayrıca bir SSH özel anahtar ve SSH özel anahtar şifre erişimi gerekir. Büyük dosyaları yüklerken yığın kullanmak için, SFTP sunucunuzdaki kök klasör için hem okuma hem de yazma izinleri gerekir. Aksi takdirde, bir "401 Yetkisiz" hata alırsınız.

  > [!IMPORTANT]
  >
  > SFTP-SSH bağlayıcısı *yalnızca* bu özel anahtar biçimlerini, algoritmaları ve parmak izlerini destekler:
  >
  > * **Özel anahtar biçimleri**: RSA (Rivest Shamir Adleman) ve DSA (Dijital İmza Algoritması) tuşları hem OpenSSH hem de ssh.com formatlarında. Özel anahtarınız PuTTY (.ppk) dosya biçimindeyse, önce [anahtarı OpenSSH (.pem) dosya biçimine dönüştürün.](#convert-to-openssh)
  >
  > * **Şifreleme algoritmaları**: DES-EDE3-CBC, DES-EDE3-CFB, DES-CBC, AES-128-CBC, AES-192-CBC ve AES-256-CBC
  >
  > * **Parmak İzi**: MD5
  >
  > Mantık uygulamanıza istediğiniz SFTP-SSH tetikleyicisini veya eylemini ekledikten sonra, SFTP sunucunuz için bağlantı bilgileri sağlamanız gerekir. Bu bağlantı için SSH özel anahtarınızı sağladığınızda, bağlantının arızaolmasına neden olabilecek ***anahtarı el ile girmeyin veya da*** Bunun yerine, ***anahtarı*** SSH özel anahtar dosyanızdan kopyaladığınızdan ve bu anahtarı bağlantı ayrıntılarına ***yapıştırdığınızdan*** emin olun. 
  > Daha fazla bilgi için bu makalenin ilerleyen bölümlerinde [SFTP'ye Bağlan](#connect) bölümüne bakın.

* [Mantık uygulamaları oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgiler

* SFTP hesabınıza erişmek istediğiniz mantık uygulaması. Bir SFTP-SSH tetikleyicisi ile başlamak için [boş bir mantık uygulaması oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md) Bir SFTP-SSH eylemi kullanmak için, mantık uygulamanızı başka bir tetikleyiciyle başlatın, **örneğin, Yineleme** tetikleyicisi.

## <a name="how-sftp-ssh-triggers-work"></a>SFTP-SSH nasıl çalışır?

SFTP-SSH, SFTP dosya sistemini yoklayarak ve son anketten bu yana değiştirilen herhangi bir dosyayı arayarak çalışmayı tetikler. Bazı araçlar, dosyalar değiştiğinde zaman damgasını korumanızı sağlar. Bu gibi durumlarda, tetikleyicinizin çalışabilmesi için bu özelliği devre dışı beşinizgerekir. Bazı yaygın ayarlar şunlardır:

| SFTP istemcisi | Eylem |
|-------------|--------|
| Winscp | **Seçeneklere** > Git**Tercihleri** > **Aktar** > **Değiştir** > Zaman**damgası** > Devre**Dışı** Bırak |
| Filezilla | **AktarAn** > dosyaların > **Disable** devre dışı**bırak'ın zaman damgalarını kaydet** |
|||

Tetikleyici yeni bir dosya bulduğunda, tetikleyici yeni dosyanın tamamlanıp kısmen yazılmadığını denetler. Örneğin, tetikleyici dosya sunucusunu denetlediğinde bir dosyanın devam eden değişiklikleri olabilir. Kısmen yazılmış bir dosyayı döndürmeyi önlemek için, tetikleyici son değişiklikler eki olan ancak bu dosyayı hemen döndürmeyen dosyanın zaman damgasını not eder. Tetikleyici, yalnızca sunucuyu yeniden yoklarken dosyayı döndürür. Bazen, bu davranış, tetikleyicinin yoklama aralığının iki katına kadar olan bir gecikmeye neden olabilir.

<a name="convert-to-openssh"></a>

## <a name="convert-putty-based-key-to-openssh"></a>PuTTY tabanlı anahtarı OpenSSH'e dönüştürün

Özel anahtarınız .ppk (PuTTY Private Key) dosya adı uzantısını kullanan PuTTY biçimindeyse, önce anahtarı .pem (Privacy Enhanced Mail) dosya adı uzantısını kullanan OpenSSH biçimine dönüştürün.

### <a name="unix-based-os"></a>Unix tabanlı işletim sistemi

1. PuTTY araçları sisteminizde zaten yüklü değilse, örneğin bunu şimdi yapın:

   `sudo apt-get install -y putty`

1. SFTP-SSH bağlayıcısıyla kullanabileceğiniz bir dosya oluşturan bu komutu çalıştırın:

   `puttygen <path-to-private-key-file-in-PuTTY-format> -O private-openssh -o <path-to-private-key-file-in-OpenSSH-format>`

   Örneğin:

   `puttygen /tmp/sftp/my-private-key-putty.ppk -O private-openssh -o /tmp/sftp/my-private-key-openssh.pem`

### <a name="windows-os"></a>Windows İşletim Sistemi

1. Bunu zaten yapmadıysanız, [en son PuTTY Generator (puttygen.exe) aracını indirin](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)ve sonra aracı başlatın.

1. Bu ekranda **Yükle'yi**seçin.

   !["Yükle" seçeneğini belirleyin](./media/connectors-sftp-ssh/puttygen-load.png)

1. PuTTY formatında özel anahtar dosyanıza göz atın ve **Aç'ı**seçin.

1. **Dönüşümler** menüsünden **OpenSSH tuşunu dışa aktar'ı**seçin.

   !["OpenSSH tuşunu dışa aktar" seçeneğini belirleyin](./media/connectors-sftp-ssh/export-openssh-key.png)

1. Özel anahtar dosyasını `.pem` dosya adı uzantısı ile kaydedin.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Bu bölümde, bu bağlayıcının tetikleyicileri ve eylemleri için gözden geçirilmesi gereken hususlar açıklanmaktadır.

<a name="create-file"></a>

### <a name="create-file"></a>Dosya oluşturma

SFTP sunucunuzda bir dosya oluşturmak için SFTP-SSH **Oluştur dosya** eylemini kullanabilirsiniz. Bu eylem dosyayı oluşturduğunda, Logic Apps hizmeti de dosyanın meta verilerini almak için Otomatik olarak SFTP sunucunuzu arar. Ancak, Mantık Uygulamaları hizmeti meta verileri almak için arama yapmadan önce yeni oluşturulan `404` dosyayı `'A reference was made to a file or folder which does not exist'`taşırsanız, bir hata iletisi alırsınız. Dosya oluşturmadan sonra dosyanın meta verilerini okumayı atlamak için, eklemek için adımları izleyin [ve tüm dosya meta **veri** özelliğini **Hayır**olarak ayarlayın.](#file-does-not-exist)

<a name="connect"></a>

## <a name="connect-to-sftp-with-ssh"></a>SSH ile SFTP'ye bağlanın

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. [Azure portalında](https://portal.azure.com)oturum açın ve mantık uygulamanızı zaten açık değilse Mantık Uygulama Tasarımcısı'nda açın.

1. Boş mantık uygulamaları için, arama `sftp ssh` kutusuna filtreniz olarak girin. Tetikleyiciler listesinin altında, istediğiniz tetikleyiciyi seçin.

   -veya-

   Varolan mantık uygulamaları için, eylem eklemek istediğiniz son adım altında **Yeni adım'ı**seçin. Arama kutusuna filtreniz olarak girin. `sftp ssh` Eylemler listesinin altında, istediğiniz eylemi seçin.

   Adımlar arasında eylem eklemek için işaretçinizin üzerine adımların arasında ilerleyin. Görünen artı işaretini (**+**) seçin ve ardından eylem **ekle'yi**seçin.

1. Bağlantınız için gerekli ayrıntıları sağlayın.

   > [!IMPORTANT]
   >
   > SSH özel anahtar anahtarınızı **SSH özel anahtar** özelliğine girdiğinizde, bu özellik için tam ve doğru değeri sağladığınızdan emin olmak için aşağıdaki ek adımları izleyin. Geçersiz bir anahtar, bağlantının başarısız olmasını neden olur.

   Herhangi bir metin düzenleyicisini kullanabiliyor olsanız da, notepad.exe'yi örnek olarak kullanarak anahtarınızı doğru şekilde kopyalamayı ve yapıştırmayı gösteren örnek adımlar aşağıda verilmiştir.

   1. SSH özel anahtar dosyanızı bir metin düzenleyicisinde açın. Bu adımlar örnek olarak Not Defteri'ni kullanır.

   1. Not Defteri **Edit** menüsünde **Tümünü Seç'i**seçin.

   1. **Kopyayı** **Edit'i** > seçin.

   1. Eklediğiniz SFTP-SSH tetikleyicisinde veya eyleminde, kopyaladığınız *anahtarın tamamını* birden çok satırı destekleyen **SSH özel anahtar** özelliğine yapıştırın.  Anahtarı ***yapıştırdığınızdan emin olun.*** ***Anahtarı el ile girmeyin veya atmayın.***

1. Bağlantı ayrıntılarını girmeyi bitirdikten sonra **Oluştur'u**seçin.

1. Şimdi seçtiğiniz tetikleyici veya eylem için gerekli ayrıntıları sağlayın ve mantık uygulamanızın iş akışını oluşturmaya devam edin.

<a name="change-chunk-size"></a>

## <a name="override-chunk-size"></a>Yığın boyutunu geçersiz kılma

Chunking'in kullandığı varsayılan uyarlamalı davranışı geçersiz kılmak için, 5 MB'tan 50 MB'a sabit bir yığın boyutu belirtebilirsiniz.

1. Eylemin sağ üst köşesinde, elips düğmesini (**...**) seçin ve ardından **Ayarlar'ı**seçin.

   ![SFTP-SSH ayarlarını aç](./media/connectors-sftp-ssh/sftp-ssh-connector-setttings.png)

1. **İçerik Aktarımı**altında, **Öbek boyutu** özelliğine, `5` örneğin `50`aşağıdakilerden bir insital değeri girin: 

   ![Bunun yerine kullanılacak yığın boyutunu belirtin](./media/connectors-sftp-ssh/specify-chunk-size-override-default.png)

1. İşiniz bittiğinde **Bitti**'yi seçin.

## <a name="examples"></a>Örnekler

<a name="file-added-modified"></a>

### <a name="sftp---ssh-trigger-when-a-file-is-added-or-modified"></a>SFTP - SSH tetikleyicisi: Bir dosya eklendiğinde veya değiştirildiğinde

Bu tetikleyici, bir SFTP sunucusuna bir dosya eklendiğinde veya değiştirildiğinde bir mantık uygulaması iş akışı başlatır. Örneğin, dosyanın içeriğini denetleyen ve içeriğin belirtilen bir koşulu karşılayıp karşılamadığını temel alan bir koşul ekleyebilirsiniz. Daha sonra, dosyanın içeriğini alan ve bu içeriği SFTP sunucusundaki bir klasöre koyan bir eylem ekleyebilirsiniz.

**Kurumsal örnek**: Müşteri siparişlerini temsil eden yeni dosyalar için bir SFTP klasörünü izlemek için bu tetikleyiciyi kullanabilirsiniz. Daha **sonra, siparişin içeriğini** daha fazla işlemek için almak ve bu siparişi bir sipariş veritabanında depolamak için dosya içeriğini al gibi bir SFTP eylemi kullanabilirsiniz.

<a name="get-content"></a>

### <a name="sftp---ssh-action-get-file-content-using-path"></a>SFTP - SSH eylemi: Yol kullanarak dosya içeriğini alın

Bu eylem, dosya yolunu belirterek bir SFTP sunucusundaki bir dosyadaki içeriği alır. Örneğin, önceki örnekteki tetikleyiciyi ve dosyanın içeriğinin karşılaması gereken bir koşul ekleyebilirsiniz. Koşul doğruysa, içeriği alan eylem çalıştırılabilir.

<a name="troubleshooting-errors"></a>

## <a name="troubleshoot-errors"></a>Sorun giderme hataları

Bu bölümde, sık karşılaşılan hataların veya sorunların olası çözümleri açıklanmaktadır.

<a name="file-does-not-exist"></a>

### <a name="404-error-a-reference-was-made-to-a-file-or-folder-which-does-not-exist"></a>404 hatası: "Var olmayan bir dosya veya klasöre başvuru yapıldı"

Bu hata, mantık uygulamanız SFTP-SSH **Oluştur dosya** eylemi aracılığıyla SFTP sunucunuzda yeni bir dosya oluşturduğunda ortaya çıkabilir, ancak yeni oluşturulan dosya, Logic Apps hizmeti dosyanın meta verilerini alamadan hemen taşınır. Mantık uygulamanız **Dosya Oluşturma** eylemini çalıştırdığında, Logic Apps hizmeti de dosyanın meta verilerini almak için Otomatik olarak SFTP sunucunuzu arar. Ancak, dosya taşınırsa, Hata iletisini `404` aldığınız için Mantık Uygulamaları hizmeti artık dosyayı bulamaz.

Dosyanın taşınmasını önleyemiyor veya geciktirmiyorsanız, aşağıdaki adımları izleyerek dosya oluşturmadan sonra dosyanın meta verilerini okumayı atlayabilirsiniz:

1. Dosya **oluştur** eyleminde, **yeni parametre ekle** listesini açın, tüm dosya meta veri özelliğini **al'ı** seçin ve değeri **Hayır**olarak ayarlayın.

1. Bu dosya meta verilerine daha sonra ihtiyacınız olursa, **dosya meta veri** eylemini al'ı kullanabilirsiniz.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler, eylemler ve konektörün Swagger dosyasında açıklandığı gibi sınırlar gibi bu bağlayıcı hakkında daha fazla teknik ayrıntı için [bağlayıcının başvuru sayfasına](https://docs.microsoft.com/connectors/sftpwithssh/)bakın.

> [!NOTE]
> [Bir entegrasyon hizmeti ortamındaki (İmKB)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)mantık uygulamaları için, bu bağlayıcının İmKB etiketli sürümü bunun yerine [İmKB ileti sınırlarını](../logic-apps/logic-apps-limits-and-config.md#message-size-limits) kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
