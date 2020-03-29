---
title: IBM DB2 pureScale'i Azure'da dağıtma
description: Bir işletmeyi z/OS'de çalışan IBM DB2 ortamından Azure'da IBM DB2 pureScale'e geçirmek için son zamanlarda kullanılan örnek bir mimariyi nasıl dağıtacaklarını öğrenin.
author: njray
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 11/09/2018
ms.author: edprice
ms.openlocfilehash: 98e912894a4d93a057a2f6a2153d0690deaed250
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78968889"
---
# <a name="deploy-ibm-db2-purescale-on-azure"></a>IBM DB2 pureScale'i Azure'da dağıtma

Bu makalede, bir kurumsal müşterinin z/OS'de çalışan IBM DB2 ortamından Azure'da IBM DB2 pureScale'e geçirmek için son zamanlarda kullandığı [örnek bir mimarinin](ibm-db2-purescale-azure.md) nasıl dağıtılancaör.

Geçiş için kullanılan adımları izlemek için GitHub'daki [DB2onAzure](https://aka.ms/db2onazure) deposundaki yükleme komut dosyalarını görün. Bu komut dosyaları, tipik, orta ölçekli çevrimiçi işlem işleme (OLTP) iş yükünün mimarisine dayanır.

## <a name="get-started"></a>Kullanmaya başlayın

Bu mimariyi dağıtmak için, GitHub'daki [DB2onAzure](https://aka.ms/db2onazure) deposunda bulunan deploy.sh komut dosyasını indirin ve çalıştırın.

Depoda grafana panosu kurmak için komut dosyaları da vardır. DB2 ile birlikte verilen açık kaynak izleme ve uyarı sistemi Prometheus'u sorgulamak için panoyu kullanabilirsiniz.

> [!NOTE]
> İstemci üzerindeki deploy.sh komut dosyası özel SSH anahtarları oluşturur ve bunları HTTPS üzerinden dağıtım şablonuna geçirir. Daha fazla güvenlik için, sırları, anahtarları ve parolaları depolamak için [Azure Key Vault'u](https://docs.microsoft.com/azure/key-vault/key-vault-overview) kullanmanızı öneririz.

## <a name="how-the-deployment-script-works"></a>Dağıtım komut dosyası nasıl çalışır?

Komut dosyası deploy.sh, bu mimari için Azure kaynaklarını oluşturur ve yapılandırır. Komut dosyası, azure aboneliği ve hedef ortamda kullanılan sanal makineler için sizi ister ve ardından aşağıdaki işlemleri gerçekleştirir:

-   Yükleme için Azure'daki kaynak grubunu, sanal ağı ve alt ağlarını ayarlar.

-   Ortam için ağ güvenlik gruplarını ve SSH'yi ayarlar.

-   Hem paylaşılan depolama hem de DB2 pureScale sanal makinelerde birden çok NIC ayarlar.

-   Paylaşılan depolama sanal makineleri oluşturur. Depolama Alanları Doğrudan veya başka bir depolama çözümü kullanıyorsanız, [Bkz. Depolama Alanları Doğrudan genel bakış.](/windows-server/storage/storage-spaces/storage-spaces-direct-overview)

-   Jumpbox sanal makine oluşturur.

-   DB2 pureScale sanal makineleri oluşturur.

-   DB2 pureScale ping tanık sanal makine oluşturur. Db2 pureScale sürümünüz tanık gerektirmiyorsa dağıtımın bu bölümünü atlayın.

-   Sınama için kullanılacak bir Windows sanal makinesi oluşturur, ancak üzerine hiçbir şey yüklemez.

Ardından, dağıtım komut dosyaları Azure'da paylaşılan depolama için bir iSCSI sanal depolama alanı ağı (vSAN) ayarlar. Bu örnekte, iSCSI paylaşılan depolama kümesine bağlanır. Orijinal müşteri çözümünde GlusterFS kullanılmıştır. Ancak IBM artık bu yaklaşımı desteklemez. IBM'den desteğinizi korumak için desteklenen iSCSI uyumlu bir dosya sistemi kullanmanız gerekir. Microsoft, Depolama Alanları Doğrudan (S2D) seçeneğini sunar.

Bu çözüm ayrıca iSCSI hedeflerini tek bir Windows düğümü olarak yükleme seçeneği de sunar. iSCSI, TCP/IP üzerinden paylaşılan depolama alanına bağlanmak için DB2 pureScale kurulum yordamını kullanarak bir aygıt arabirimi kullanmasına olanak tanıyan paylaşılan bir blok depolama arabirimi sağlar.

Dağıtım komut dosyaları şu genel adımları çalıştırır:

1.  Azure'da paylaşılan bir depolama kümesi ayarlayın. Bu adım, en az iki Linux düğümü içerir.

2.  Paylaşılan depolama kümesi için hedef Linux sunucularında bir iSCSI Direct arabirimi ayarlayın.

3.  Linux sanal makinelerinde iSCSI başlatıcısını ayarlayın. Başlatıcı, bir iSCSI hedefi kullanarak paylaşılan depolama kümesine erişecektir. Kurulum ayrıntıları için RootUsers belgelerinde [Linux'ta iSCSI Hedef ve Başlatıcınasıl Yapılandırılır'](https://www.rootusers.com/how-to-configure-an-iscsi-target-and-initiator-in-linux/) a bakın.

4.  iSCSI arabirimi için paylaşılan depolama katmanını yükleyin.

Komut dosyaları iSCSI aygıtını oluşturduktan sonra, son adım DB2 pureScale'i yüklemektir. DB2 pureScale kurulumunun bir parçası olarak, [IBM Spectrum Scale](https://www.ibm.com/support/knowledgecenter/SSEPGG_11.1.0/com.ibm.db2.luw.qb.server.doc/doc/t0057167.html) (eski adıyla GPFS) Derlenir ve GlusterFS kümesine yüklenir. Bu kümelenmiş dosya sistemi, DB2 pureScale motorini çalıştıran sanal makineler arasında veri paylaşımını Sağlar. Daha fazla bilgi için IBM Web sitesindeki [IBM Spectrum Scale](https://www.ibm.com/support/knowledgecenter/en/STXKQY_4.2.0/ibmspectrumscale42_welcome.html) belgelerine bakın.

## <a name="db2-purescale-response-file"></a>DB2 pureScale yanıt dosyası

GitHub deposu, DB2 pureScale yüklemesi için otomatik bir komut dosyası oluşturmanıza olanak tanıyan bir yanıt (.rsp) dosyası olan DB2server.rsp dosyasını içerir. Aşağıdaki tablo, yanıt dosyasının kurulum için kullandığı DB2 pureScale seçeneklerini listeler. Yanıt dosyasını ortamınız için gerektiği gibi özelleştirebilirsiniz.

> [!NOTE]
> Bir örnek yanıt dosyası, DB2server.rsp, GitHub [DB2onAzure](https://aka.ms/db2onazure) deposunda yer almaktadır. Bu dosyayı kullanıyorsanız, ortamınızda çalışmadan önce dosyayı düzenlemesi gerekir.

| Ekran adı               | Alan                                        | Değer                                                                                                 |
|---------------------------|----------------------------------------------|-------------------------------------------------------------------------------------------------------|
| Hoş Geldiniz                   |                                              | Yeni Yükleme                                                                                           |
| Ürün Seçin          |                                              | DB2 Sürüm 11.1.3.3. DB2 pureScale ile Sunucu Sürümleri                                              |
| Yapılandırma             | Dizin                                    | /data1/opt/ibm/db2/V11.1                                                                              |
|                           | Yükleme türünü seçin                 | Tipik                                                                                               |
|                           | IBM koşullarını kabul ediyorum                     | İşaretli                                                                                               |
| Örnek Sahibi            | Mevcut Kullanıcı Örneğin, Kullanıcı adı        | DB2sdin1                                                                                              |
| Çitle Çevrili Kullanıcı               | Mevcut Kullanıcı, Kullanıcı adı                     | DB2sdfe1                                                                                              |
| Küme Dosya Sistemi       | Paylaşılan disk bölümleme aygıt yolu            | /dev/dm-2                                                                                             |
|                           | Montaj noktası                                  | /DB2sd\_1804a                                                                                         |
|                           | Veriler için paylaşılan disk                         | /dev/dm-1                                                                                             |
|                           | Montaj noktası (Veri)                           | /DB2fs/datafs1                                                                                        |
|                           | Günlük için paylaşılan disk                          | /dev/dm-0                                                                                             |
|                           | Montaj noktası (Log)                            | /DB2fs/logfs1                                                                                         |
|                           | DB2 Küme Hizmetleri Tiebreaker. Aygıt yolu | /dev/dm-3                                                                                             |
| Ev Sahibi Listesi                 | d1 [eth1], d2 [eth1], cf1 [eth1], cf2 [eth1] |                                                                                                       |
|                           | Tercih edilen birincil CF                         | cf1                                                                                                   |
|                           | Tercih edilen ikincil CF                       | cf2                                                                                                   |
| Yanıt Dosyası ve Özeti | ilk seçenek                                 | IBM DB2 pureScale özelliğiyle DB2 Server Edition'ı yükleyin ve ayarlarımı bir yanıt dosyasına kaydedin |
|                           | Yanıt dosyası adı                           | /root/DB2server.rsp                                                                                   |

### <a name="notes-about-this-deployment"></a>Bu dağıtım la ilgili notlar

- /dev-dm0, /dev-dm1, /dev-dm2 ve /dev-dm3 değerleri, kurulumun gerçekleştiği sanal makinede yeniden başladıktan sonra değişebilir (otomatik komut dosyasında d0). Doğru değerleri bulmak için, kurulumun çalışacağı sunucudaki yanıt dosyasını tamamlamadan önce aşağıdaki komutu düzenleyebilirsiniz:

   ```
   [root\@d0 rhel]\# ls -als /dev/mapper
   total 0
   0 drwxr-xr-x 2 root root 140 May 30 11:07 .
   0 drwxr-xr-x 19 root root 4060 May 30 11:31 ..
   0 crw------- 1 root root 10, 236 May 30 11:04 control
   0 lrwxrwxrwx 1 root root 7 May 30 11:07 db2data1 -\> ../dm-1
   0 lrwxrwxrwx 1 root root 7 May 30 11:07 db2log1 -\> ../dm-0
   0 lrwxrwxrwx 1 root root 7 May 30 11:26 db2shared -\> ../dm-2
   0 lrwxrwxrwx 1 root root 7 May 30 11:08 db2tieb -\> ../dm-3
   ```

- Kurulum komut dosyaları, gerçek adların kolayca bulunabilmesi için iSCSI diskleri için takma adlar kullanır.

- Kurulum komut dosyası d0'da çalıştırıldığında, **/dev/dm-\* ** değerleri d1, cf0 ve cf1'de farklı olabilir. Değerler arasındaki fark DB2 pureScale kurulum etkilemez.

## <a name="troubleshooting-and-known-issues"></a>Sorun çözümü ve bilinen sorunlar

GitHub repo yazarlarıkorumak bir bilgi tabanı içerir. Sahip olabileceğiniz olası sorunları ve deneyebileceğiniz çözümleri listeler. Örneğin, bilinen sorunlar şu anda meydana gelebilir:

-   Ağ geçidi IP adresine ulaşmaya çalışıyorsunuz.

-   Genel Kamu Lisansı (GPL) derliyoruz.

-   Ana bilgisayarlar arasındaki güvenlik el sıkışması başarısız olur.

-   DB2 yükleyicisi varolan bir dosya sistemini algılar.

-   IBM Spectrum Scale'i el ile yüklüyorsunuz.

-   IBM Spectrum Scale zaten oluşturulduğunda DB2 pureScale'i yüklüyorsunuz.

-   DB2 pureScale ve IBM Spectrum Scale'i kaldırıyorsunuz.

Bu ve bilinen diğer sorunlar hakkında daha fazla bilgi [için, DB2onAzure](https://aka.ms/DB2onAzure) repo'daki kb.md dosyaya bakın.

## <a name="next-steps"></a>Sonraki adımlar

-   [DB2 pureScale Özellik yüklemesi için gerekli kullanıcı oluşturma](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.qb.server.doc/doc/t0055374.html?pos=2)

-   [DB2icrt - Örnek komutu oluşturma](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.admin.cmd.doc/doc/r0002057.html)

-   [DB2 pureScale Kümeler Veri Çözümü](https://www.ibmbigdatahub.com/blog/db2-purescale-clustered-database-solution-part-1)

-   [IBM Veri Stüdyosu](https://www.ibm.com/developerworks/downloads/im/data/index.html/)

-   [Azure Sanal Veri Merkezi Kaldırma ve Kaydırma Kılavuzu](https://azure.microsoft.com/resources/azure-virtual-datacenter-lift-and-shift-guide/)
