---
title: Veri Yönetimi Ağ Geçidi için sürüm notları
description: Veri Yönetimi Ağ Geçidi tory sürüm notları
services: data-factory
author: nabhishek
manager: anandsub
ms.assetid: 14762e82-76d9-41c4-ba9f-14a54da29c36
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: abnarain
robots: noindex
ms.openlocfilehash: 838e523f74a21c44958ddb6dc88e4dab3526d81a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80064991"
---
# <a name="release-notes-for-data-management-gateway"></a>Veri Yönetimi Ağ Geçidi için sürüm notları
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'de kendi kendine barındırılan tümleştirme çalışma süresine](../create-self-hosted-integration-runtime.md)bakın.

Modern veri entegrasyonunun zorluklarından biri, verileri şirket içi buluta taşımaktır. Veri Fabrikası, karma veri hareketini etkinleştirmek için şirket içinde yükleyebileceğiniz bir aracı olan Veri Yönetimi Ağ Geçidi ile bu tümleştirmeyi yapar.

Veri Yönetimi Ağ Geçidi ve nasıl kullanılacağı hakkında ayrıntılı bilgi için aşağıdaki makalelere bakın:

*  [Veri Yönetimi Ağ Geçidi](data-factory-data-management-gateway.md)
*  [Azure Veri Fabrikası'nı kullanarak verileri şirket içi ve bulut arasında taşıma](data-factory-move-data-between-onprem-and-cloud.md)


## <a name="current-version"></a>GÜNCEL SÜRÜM 
Artık Sürüm notlarını burada saklayamayız. En son sürüm notlarını [buradan](https://go.microsoft.com/fwlink/?linkid=853077) alın




## <a name="earlier-versions"></a>Önceki sürümler
## <a name="21063477"></a>2.10.6347.7
### <a name="enhancements-"></a>Geliştirmeler-
- Güvenlik duvarınızdaki tüm Azure IP adreslerini beyaz listeye almak yerine (gerekirse) DNS girişlerini beyaz liste servis veri yolunda ekleyebilirsiniz. Azure portalında ilgili DNS girişini bulabilirsiniz (Veri Fabrikası -> 'Author and Deploy' -> 'Ağ Geçitleri' -> "serviceUrls" (JSON'da)
- HDFS bağlayıcısı artık TLS doğrulaması atlayabilmeni sağlayarak kendi imzalı ortak sertifikayı destekler.
- Düzeltildi: Güncelleştirme sırasında ağ geçidi çevrimdışı sorunu (saat çarpıklık nedeniyle)


## <a name="2963132"></a>2.9.6313.2
### <a name="enhancements-"></a>Geliştirmeler-
-   Güvenlik duvarınızdaki tüm Azure IP adreslerini beyaz listeye almak yerine (gerekirse) DNS girişlerini beyaz listehizmet veri yolunda ekleyebilirsiniz. Diğer ayrıntıları burada bulabilirsiniz.
-   Artık tek bir blok blob'una/tek bir blokblosuna veri kopyalayabilirsiniz, bu da blok blobunun maksimum desteklenen boyutudur. (önceki sınır 195 GB idi).
-   Düzeltildi: Kopyalama etkinliği sırasında birkaç küçük dosyayı çözerken bellek sorunu dışında.
-   Sabit: Belge DB'den idempotency özelliğine sahip şirket içi bir SQL Server'a kopyaçekerken kapsama alanı dışında dizin sorunu.
-   Sabit: SQL temizleme komut dosyası, Copy Wizard'ın şirket içi SQL Server'ı ile çalışmaz.
-   Sabit: Sonunda boşluk bulunan sütun adı kopyalama etkinliğinde çalışmaz.

## <a name="28662833"></a>2.8.66283.3
### <a name="enhancements-"></a>Geliştirmeler-
- Düzeltildi: Ağ geçidi makinesi yeniden başlatma eksik kimlik bilgileri ile sorun.
- Düzeltildi: Yedek bir dosya kullanarak ağ geçidi geri yüklemesi sırasında kayıt sorunu.


## <a name="2762401"></a>2.7.6240.1
### <a name="enhancements-"></a>Geliştirmeler-
- Sabit: Oracle'dan kaynak olarak ondalık null değerinin yanlış okunması.

## <a name="2661922"></a>2.6.6192.2
### <a name="whats-new"></a>Yenilikler
- Müşteriler ağ geçidi kayıt deneyimi hakkında geri bildirim sağlayabilir.
- Yeni sıkıştırma biçimini destekleyin: ZIP (Deflate)

### <a name="enhancements-"></a>Geliştirmeler-
- HDFS kaynağı Oracle Sink için performans artışı.
- Ağ geçidi otomatik güncelleştirme, ağ geçidi paralel işleme kapasitesi için hata düzeltmesi.


## <a name="2561641"></a>2.5.6164.1
### <a name="enhancements"></a>sekmesinden 
- Geliştirilmiş ve daha sağlam Ağ Geçidi kayıt deneyimi- Artık kayıt deneyimini daha duyarlı hale getiren Ağ Geçidi kayıt işlemi sırasında ilerleme durumunu izleyebilirsiniz.
- Ağ Geçidi Geri Yükleme İşleminde Geliştirme- Bu güncelleştirmeyle ağ geçidi yedekleme dosyanız olmasa bile ağ geçidini kurtarabilirsiniz. Bunun için Portal'daki Bağlantılı Hizmet kimlik bilgilerini sıfırlamanız gerekir.
- Hata düzeltmesi.

## <a name="2461511"></a>2.4.6151.1

### <a name="whats-new"></a>Yenilikler

- Artık veri kaynağı kimlik bilgilerini yerel olarak depolayabilirsiniz. Kimlik bilgileri şifrelenir. Veri kaynağı kimlik bilgileri, varolan Ağ Geçidi'nden tüm şirket içinde dışa aktarılabilen yedekleme dosyası kullanılarak kurtarılabilir ve geri yüklenebilir.

### <a name="enhancements-"></a>Geliştirmeler-

- Geliştirilmiş ve daha sağlam Ağ Geçidi kayıt deneyimi.
- Kopya sihirbazında Metin biçimi için QuoteChar yapılandırmasının otomatik algılamasını destekleyin ve genel biçim algılama doğruluğunu artırın.

## <a name="2361002"></a>2.3.6100.2

- Şirket içi Dosya sisteminde ve HDFS'deki metin dosyaları için kopyalama sihirbazında ilkRowAsHeader ve SkipLineCount otomatik algılamasını destekleyin.
- Ağ geçidi ve Servis Veri Servisi arasındaki ağ bağlantısının kararlılığını artırın
- Birkaç hata düzeltmeleri


## <a name="2260721"></a>2.2.6072.1

*  Ağ Geçidi Yapılandırma Yöneticisi'ni kullanarak ağ geçidi için HTTP proxy ayarı destekler. Yapılandırıldıysa, Azure Blob, Azure Table, Azure Veri Gölü ve Belge DB'ye HTTP proxy üzerinden erişilir.
*  Azure Blob, Azure Veri Gölü Deposu, şirket içi Dosya Sistemi ve şirket içi HDFS'den veri kopyalarken TextFormat için üstbilgi işlemeyi destekler.
*  Append Blob ve Page Blob'dan gelen verilerin kopyalanmasıyla birlikte zaten desteklenen Block Blob'u destekler.
*  Kopya Sihirbazı için etkileşimli çalışma desteği dışında ağ geçidinin ana işlevinin çalıştığını gösteren yeni bir ağ geçidi durumu **Çevrimiçi (Sınırlı)** sunar.
*  Kayıt anahtarını kullanarak ağ geçidi kaydının sağlamlığını artırır.

## <a name="216040"></a>2.1.6040.

*  DB2 sürücüsü artık ağ geçidi yükleme paketine dahil edilmiştir. Ayrıca yüklemeniz gerekmez.
*  DB2 sürücüsü şimdi z / OS ve DB2 i (AS/400) platformlar zaten desteklenen (Linux, Unix ve Windows) ile birlikte destekler.
*  Azure Cosmos DB'nin şirket içi veri depoları için kaynak veya hedef olarak kullanılmasını destekler
*  Zaten desteklenen genel amaçlı depolama hesabıyla birlikte verilerin soğuk/sıcak blob depolamadan kopyalanmasını destekler.
*  Uzaktan giriş ayrıcalıkları ile ağ geçidi üzerinden şirket içi SQL Server'a bağlanmanızı sağlar.  

## <a name="2060131"></a>2.0.6013.1

*  El ile yükleme sırasında ağ geçidi tarafından kullanılacak dili/kültürü seçebilirsiniz.

*  Ağ geçidi beklendiği gibi çalışmadığında, sorunun sorun gidermesini kolaylaştırmak için son yedi günün ağ geçidi günlüklerini Microsoft'a göndermeyi seçebilirsiniz. Ağ geçidi bulut hizmetine bağlı değilse, ağ geçidi günlüklerini kaydetmeyi ve arşivlemeyi seçebilirsiniz.  

*  Ağ geçidi yapılandırma yöneticisi için kullanıcı arabirimi geliştirmeleri:

    *  Giriş sekmesinde ağ geçidi durumunu daha görünür hale getirin.

    *  Yeniden düzenlenmiş ve basitleştirilmiş denetimler.

    *  [Kodsuz kopyalama aracını](data-factory-copy-data-wizard-tutorial.md)kullanarak bir depolama dan verileri kopyalayabilirsiniz. Genel olarak bu özellik hakkında ayrıntılar için [Aşamalı Kopya'ya](data-factory-copy-activity-performance.md#staged-copy) bakın.
*  Veri Yönetimi Ağ Geçidi'ni, verileri doğrudan şirket içi BIR SQL Server veritabanından Azure Machine Learning'e girmek için kullanabilirsiniz.

*  Performans iyileştirmeleri

    * Kodsuz kopyalama aracında SQL Server'a karşı Şema/Önizleme görüntüleme performansını artırın.

## <a name="11259531"></a>1.12.5953.1

*  Hata düzeltmeleri

## <a name="11159181"></a>1.11.5918.1

*  Ağ geçidi olay günlüğünün maksimum boyutu 1 MB'dan 40 MB'a yükseltildi.

*  Ağ geçidi otomatik güncelleştirmesi sırasında yeniden başlatma gerektiğinde bir uyarı iletişim kutusu görüntülenir. Hemen veya daha sonra yeniden başlatmayı seçebilirsiniz.

*  Otomatik güncelleştirmenin başarısız olması durumunda, ağ geçidi yükleyici söyülücü otomatik güncelleştirmeyi en fazla üç kez yeniden günceller.

*  Performans iyileştirmeleri

    * Şirket içi sunucudan kodsuz kopyalama senaryosunda büyük tabloları yüklemek için performansı artırın.

*  Hata düzeltmeleri

## <a name="11058921"></a>1.10.5892.1

*  Performans iyileştirmeleri

*  Hata düzeltmeleri

## <a name="1958652"></a>1.9.5865.2

*  Sıfır dokunmatik otomatik güncelleme özelliği
*  Ağ geçidi durum göstergelerini içeren yeni tepsi simgesi
*  İstemciden "Şimdi Güncelleştirme" özelliği
*  Güncelleştirme zamanlama süresini ayarlama özelliği
*  Otomatik güncelleştirmeyi açma/kapama da değiştirme için PowerShell komut dosyası
*  JSON formatı için destek  
*  Performans iyileştirmeleri
*  Hata düzeltmeleri

## <a name="1858221"></a>1.8.5822.1

*  Sorun giderme deneyimini geliştirme
*  Performans iyileştirmeleri
*  Hata düzeltmeleri

### <a name="1757951"></a>1.7.5795.1

*  Performans iyileştirmeleri
*  Hata düzeltmeleri

### <a name="1757641"></a>1.7.5764.1

*  Performans iyileştirmeleri
*  Hata düzeltmeleri

### <a name="1657351"></a>1.6.5735.1

*  Şirket içi HDFS Kaynak/Lavabo desteği
*  Performans iyileştirmeleri
*  Hata düzeltmeleri

### <a name="1656961"></a>1.6.5696.1

*  Performans iyileştirmeleri
*  Hata düzeltmeleri

### <a name="1656761"></a>1.6.5676.1

*  Configuration Manager'da tanılama araçlarını destekleme
*  Azure Veri Fabrikası için tablo kaynakları için destek tablosu sütunları
*  Azure Veri Fabrikası için SQL DW'yi destekleyin
*  Azure Veri Fabrikası için BlobSource ve FileSource'da Münzevi'yi Destekleyin
*  Destek CopyBehavior - Birleştirme Dosyaları, Koruma Hiyerarşisi ve Azure Veri Fabrikası için İkili Kopya ile BlobSink ve FileSink Düzten Hiyerarşi
*  Azure Veri Fabrikası için Destek Kopyalama Etkinliği raporlama
*  Azure Veri Fabrikası için Destek Veri Kaynağı Bağlantı Doğrulaması
*  Hata düzeltmeleri

### <a name="1656721"></a>1.6.5672.1

*  Azure Veri Fabrikası için ODBC veri kaynağı için destek tablo adı
*  Performans iyileştirmeleri
*  Hata düzeltmeleri

### <a name="1656581"></a>1.6.5658.1

*  Azure Veri Fabrikası için Destek Dosyası Lavabosu
*  Azure Veri Fabrikası için ikili kopyada hiyerarşiyi korumayı destekleme
*  Azure Veri Fabrikası için Destek Kopyalama Etkinliği Idempotency
*  Hata düzeltmeleri

### <a name="1656401"></a>1.6.5640.1

*  Azure Veri Fabrikası için 3 veri kaynağını daha destekleyin (ODBC, OData, HDFS)
*  Azure Veri Fabrikası için csv parer'de teklif karakterini destekleyin
*  Sıkıştırma desteği (BZip2)
*  Hata düzeltmeleri

### <a name="1556121"></a>1.5.5612.1

*  Azure Veri Fabrikası (MySQL, PostgreSQL, DB2, Teradata ve Sybase) için beş ilişkisel veritabanını destekleyin
*  Sıkıştırma desteği (Gzip ve Deflate)
*  Performans iyileştirmeleri
*  Hata düzeltmeleri

### <a name="1455491"></a>1.4.5549.1

*  Azure Veri Fabrikası için Oracle veri kaynağı desteği ekleme
*  Performans iyileştirmeleri
*  Hata düzeltmeleri

### <a name="1454921"></a>1.4.5492.1

*  Hem Microsoft Azure Veri Fabrikası'nı hem de Office 365 Power BI hizmetlerini destekleyen birleşik ikili
*  Yapılandırma UI ve kayıt işlemini hassaslaştırma
*  Azure Veri Fabrikası – SQL Server veri kaynağı için Azure Ingress ve Çıkış desteği

### <a name="1253031"></a>1.2.5303.1

*  Daha fazla zaman alan veri kaynağı bağlantılarını desteklemek için zaman ekme sorununu düzeltin.

### <a name="1155268"></a>1.1.5526.8

*  Kurulum sırasında bir ön koşul olarak .NET Framework 4.5.1 gerektirir.

### <a name="1051442"></a>1.0.5144.2

*  Azure Veri Fabrikası senaryolarını etkileyen değişiklik yok.
