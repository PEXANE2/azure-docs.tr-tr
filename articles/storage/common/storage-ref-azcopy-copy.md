---
title: azcopy kopya| Microsoft Dokümanlar
description: Bu makalede, azcopy copy komutu için referans bilgileri sağlar.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 04/10/2020
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 0325a71fb069f3d96f05d106afac1639fc38fe42
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81253348"
---
# <a name="azcopy-copy"></a>azcopy kopya

Kaynak verileri hedef konuma kopyalar.

## <a name="synopsis"></a>Özet

Kaynak verileri hedef konuma kopyalar. Desteklenen yol tarifleri şunlardır:

  - yerel < > Azure Blob (SAS veya OAuth kimlik doğrulaması)
  - yerel < > Azure Dosyaları (Paylaş/dizin SAS kimlik doğrulaması)
  - yerel < > ADLS Gen 2 (SAS, OAuth veya SharedKey kimlik doğrulaması)
  - Azure Blob (SAS veya genel) > Azure Blob (SAS veya OAuth kimlik doğrulaması)
  - Azure Blob (SAS veya genel) -Azure Dosyaları (SAS) >
  - Azure Dosyaları (SAS) -> Azure Dosyaları (SAS)
  - Azure Dosyaları (SAS) -> Azure Blob (SAS veya OAuth kimlik doğrulaması)
  - AWS S3 (Access Key) -> Azure Blok Blob (SAS veya OAuth kimlik doğrulaması)

Daha fazla bilgi için lütfen örneklere bakın.

## <a name="related-conceptual-articles"></a>İlgili kavramsal makaleler

- [AzCopy’yi kullanmaya başlama](storage-use-azcopy-v10.md)
- [AzCopy ve Blob depolama ile veri aktarımı](storage-use-azcopy-blobs.md)
- [AzCopy ve dosya depolama ile veri aktarımı](storage-use-azcopy-files.md)
- [AzCopy'i yapılandırma, en iyi duruma getirme ve sorun giderme](storage-use-azcopy-configure.md)

## <a name="advanced"></a>Gelişmiş

AzCopy, dosya uzantısına veya içeriğe bağlı olarak yerel diskten yükleme yaparken dosyaların içerik türünü otomatik olarak algılar (uzantı belirtilmemişse).

Yerleşik arama tablosu küçüktür, ancak Unix'te, bu adlardan biri veya birkaçı altında varsa yerel sistemin mim.types dosyası(lar) tarafından artırılır:

- /etc/mime.types
- /etc/apache2/mime.types
- /etc/apache/mime.types

Windows'da MIME türleri kayıt defterinden ayıklanır. Bu özellik bir bayrak yardımıyla kapatılabilir. Lütfen bayrak bölümüne bakın.

Komut satırını kullanarak bir ortam değişkeni ayarlarsanız, bu değişken komut satırı geçmişinizde okunabilir. Komut satırı geçmişinizden kimlik bilgilerini içeren değişkenleri temizlemeyi düşünün. Değişkenlerin geçmişinizde görünmesini engellemek için, kullanıcıdan kimlik bilgilerini almak ve ortam değişkenini ayarlamak için bir komut dosyası kullanabilirsiniz.

```
azcopy copy [source] [destination] [flags]
```

## <a name="examples"></a>Örnekler

OAuth kimlik doğrulamasını kullanarak tek bir dosya yükleyin. Henüz AzCopy'ye giriş yapmadıysanız, aşağıdaki komutu çalıştırmadan önce lütfen azcopy giriş komutunu çalıştırın.

- azcopy cp "/path/to/file.txt" "https://[account].blob.core.windows.net/[konteyner]/[path/to/blob]"

Yukarıdaki gibi, ama bu kez de dosya içeriğinin MD5 karma hesap ve blob İçerik-MD5 özelliği olarak kaydedin:

- azcopy cp "/path/to/file.txt" "https://[account].blob.core.windows.net/[konteyner]/[path/to/blob]" --put-md5

SAS belirteci kullanarak tek bir dosya yükleme:

- azcopy cp "/path/to/file.txt" "https://[account].blob.core.windows.net/[konteyner]/[path/to/blob]? [SAS]"

SAS belirteci ve boru kullanarak tek bir dosya yükleyin (yalnızca blobs'ı engelleyin):
  
- kedi "/path/to/file.txt" | azcopy cp "https://[hesap].blob.core.windows.net/[konteyner]/[path/to/blob]? [SAS]"

SAS belirteci kullanarak tüm bir dizin yükleyin:
  
- azcopy cp "/path/to/dir" "https://[account].blob.core.windows.net/[konteyner]/[path/to/directory]? [SAS]" --özyinelemeli=gerçek

or

- azcopy cp "/path/to/dir" "https://[account].blob.core.windows.net/[konteyner]/[path/to/directory]? [SAS]" --özyinelemeli=true--put-md5

SAS belirteci ve joker karakter (*) karakterleri kullanarak bir dosya kümesi yükleyin:

- azcopy cp "/path/*foo/* bar/*.pdf" "https://[account].blob.core.windows.net/[konteyner]/[path/to/directory]? [SAS]"

SAS belirteci ve joker karakter (*) karakterleri kullanarak dosya ve dizin yükleyin:

- azcopy cp "/path/*foo/* bar*" "https://[account].blob.core.windows.net/[konteyner]/[path/to/directory]? [SAS]" --özyinelemeli=gerçek

OAuth kimlik doğrulamasını kullanarak tek bir dosyayı indirin. Henüz AzCopy'ye giriş yapmadıysanız, aşağıdaki komutu çalıştırmadan önce lütfen azcopy giriş komutunu çalıştırın.

- azcopy cp "https://[hesap].blob.core.windows.net/[konteyner]/[path/to/blob]" "/path/to/file.txt"

SAS belirteci kullanarak tek bir dosyayı indirin:

- azcopy cp "https://[hesap].blob.core.windows.net/[konteyner]/[path/to/blob]? [SAS]" "/path/to/file.txt"

SAS belirteci kullanarak ve çıktıyı bir dosyaya borulayarak tek bir dosyayı indirin (yalnızca blobs'u engelleyin):
  
- azcopy cp "https://[hesap].blob.core.windows.net/[konteyner]/[path/to/blob]? [SAS]" > "/path/to/file.txt"

SAS belirteci kullanarak tüm bir dizin indirin:
  
- azcopy cp "https://[account].blob.core.windows.net/[konteyner]/[path/to/directory]? [SAS]" "/path/to/dir" --özyinelemeli=true

URL'lerde joker karakter (*) kullanma yla ilgili bir not:

Bir URL'de joker karakter kullanmanın yalnızca iki desteklenen yolu vardır. 

- Bir URL'nin son ileri eğik çizgi (/) sonra bir kullanabilirsiniz. Bu, dizindeki tüm dosyaları bir alt dizine yerleştirmeden doğrudan hedefe kopyalar.

- URL yalnızca bir kapsayıcıya atıfta bulunduğu sürece, bir blob için değil, bir kapsayıcı adına da kullanabilirsiniz. Bu yaklaşımı, kapsayıcıların bir alt kümesinden dosya almak için kullanabilirsiniz.

İçeren dizinin kendisini kopyalamadan dizinin içeriğini indirin.

- azcopy cp "https://[srcaccount].blob.core.windows.net/[konteyner]/[path/to/folder]/*? [SAS]" "/yol/to/dir"

Tüm depolama hesabını indirin.

- azcopy cp "https://[srcaccount].blob.core.windows.net/" "/path/to/dir" --özyinelemeli

Kapsayıcı adına joker karakter simgesi (*) kullanarak depolama hesabı içindeki kapsayıcıların bir alt kümesini indirin.

- azcopy cp "https://[srcaccount].blob.core.windows.net/[konteyner*adı]" "/path/to/dir" --özyinelemeli

SAS belirteci kullanarak tek bir blob'u başka bir blob'a kopyalayın.

- azcopy cp "https://[srcaccount].blob.core.windows.net/[konteyner]/[path/to/blob]? [SAS]" "https://[destaccount].blob.core.windows.net/[konteyner]/[path/to/blob]? [SAS]"

Bir SAS belirteci ve OAuth belirteci kullanarak tek bir blob'u başka bir blob'a kopyalayın. Kaynak hesap URL'sinin sonunda bir SAS belirteci kullanmanız gerekir, ancak azcopy giriş komutunu kullanarak AzCopy'ye giriş yaptığınızda hedef hesabın adedine gerek yoktur. 

- azcopy cp "https://[srcaccount].blob.core.windows.net/[konteyner]/[path/to/blob]? [SAS]" "https://[destaccount].blob.core.windows.net/[konteyner]/[path/to/blob]"

Bir blob sanal dizinini Bir SAS belirteci kullanarak diğerine kopyalayın:

- azcopy cp "https://[srcaccount].blob.core.windows.net/[konteyner]/[path/to/directory]? [SAS]" "https://[destaccount].blob.core.windows.net/[konteyner]/[path/to/directory]? [SAS]" --özyinelemeli=gerçek

SAS belirteci kullanarak depolama hesabından diğerine tüm blob kapları, dizinleri ve blobları kopyalayın:

- azcopy cp "https://[srcaccount].blob.core.windows.net? [SAS]" "https://[destaccount].blob.core.windows.net? [SAS]" --özyinelemeli=gerçek

Bir erişim anahtarı ve SAS belirteci kullanarak Amazon Web Services (AWS) S3'ten Blob Depolama'ya tek bir nesneyi kopyalayın. İlk olarak, AWS S3 kaynağı için ortam değişkenini AWS_ACCESS_KEY_ID ve AWS_SECRET_ACCESS_KEY ayarlayın.
  
- azcopy cphttps://s3.amazonaws.com/" [kova]/[nesne]" "https://[destaccount].blob.core.windows.net/[konteyner]/[path/to/blob]? [SAS]"

Bir erişim anahtarı ve SAS belirteci kullanarak Tüm dizini AWS S3'ten Blob Depolama'ya kopyalayın. İlk olarak, AWS S3 kaynağı için ortam değişkenini AWS_ACCESS_KEY_ID ve AWS_SECRET_ACCESS_KEY ayarlayın.

- azcopy cphttps://s3.amazonaws.com/" [kova]/[klasör]" "https://[destaccount].blob.core.windows.net/[konteyner]/[path/to/directory]? [SAS]" --özyinelemeli=gerçek

Lütfen [klasör] yer tutucuyu daha iyi anlamak için https://docs.aws.amazon.com/AmazonS3/latest/user-guide/using-folders.html bakın.

Bir erişim anahtarı ve SAS belirteci kullanarak tüm kovaları Amazon Web Services'ten (AWS) Blob Depolama'ya kopyalayın. İlk olarak, AWS S3 kaynağı için ortam değişkenini AWS_ACCESS_KEY_ID ve AWS_SECRET_ACCESS_KEY ayarlayın.

- azcopy cphttps://s3.amazonaws.com/" " "https://[destaccount].blob.core.windows.net? [SAS]" --özyinelemeli=gerçek

Bir erişim anahtarı ve SAS belirteci kullanarak tüm kovaları bir Amazon Web Hizmetleri (AWS) bölgesinden Blob Depolama'ya kopyalayın. İlk olarak, AWS S3 kaynağı için ortam değişkenini AWS_ACCESS_KEY_ID ve AWS_SECRET_ACCESS_KEY ayarlayın.

- azcopy cphttps://s3-" [region].amazonaws.com/" "https://[destaccount].blob.core.windows.net? [SAS]" --özyinelemeli=gerçek

Kova adına joker karakter simgesi (*) kullanarak bir kova alt kümesini kopyalayın. Önceki örneklerde olduğu gibi, bir erişim anahtarı ve Bir SAS belirteci gerekir. AWS S3 kaynağı için ortam değişkenini AWS_ACCESS_KEY_ID ve AWS_SECRET_ACCESS_KEY ayarladıklarından emin olun.

- azcopy cphttps://s3.amazonaws.com/" [bucket*name]/" "https://[destaccount].blob.core.windows.net? [SAS]" --özyinelemeli=gerçek

## <a name="options"></a>Seçenekler

**--yedekleme**                               Yüklemeler için Windows'un SeBackupPrivilege'ını veya indirmeler için SeRestorePrivilege'ı etkinleştirerek, AzCopy'nin dosya sistemi izinlerinden bağımsız olarak tüm dosyaları okumasını görmesine ve tüm izinleri geri yüklemesine olanak sağlar. AzCopy çalıştıran hesabın zaten bu izinlere sahip olması (örn. yönetici haklarına sahip veya 'Yedek Operatörler' grubunun bir üyesidir). Bu bayrağın yaptığı tek şey, hesabın zaten sahip olduğu ayrıcalıkları etkinleştirmektir.

**--blob tipi** dize Hedefteki blob türünü tanımlar. Bu, blob'ları yüklemek ve hesaplar arasında kopyalama yaparken kullanılır (varsayılan 'Algılama'). Geçerli değerler 'Algıla', 'BlockBlob', 'PageBlob', ve 'AppendBlob'u içerir. Hesaplar arasında kopyalama yaparken, 'Algıla' değeri AzCopy'nin hedef blob türünü belirlemek için kaynak blob türünü kullanmasına neden olur. Bir dosya yüklerken, 'Algıla' dosyanın dosya uzantısına dayalı bir VHD veya VHDX dosyası olup olmadığını belirler. Dosya bir VHD veya VHDX dosyasıe eter sayılsa, AzCopy dosyayı bir sayfa blob'u olarak ele alar. (varsayılan "Algılama")

**--block-blob-tier** string Upload blok blobs doğrudan seçtiğiniz [erişim katmanına.](../blobs/storage-blob-storage-tiers.md) (varsayılan 'Yok'). Geçerli değerler 'Yok', 'Sıcak', 'Cool' ve 'Arşiv' içerir. 'Yok' veya hiçbir katman geçirilirse, blob depolama hesabının katmanını devralır.

**--block-size-mb** float Azure Depolama'ya yüklerken ve Azure Depolama'dan indirirken bu blok boyutunu (MiB'de belirtilir) kullanın. Varsayılan değer, dosya boyutuna göre otomatik olarak hesaplanır. Ondalık kesirlere izin verilir (Örneğin: 0.25).

**--önbellek denetimi** dizesi Önbellek denetimi üstbilgisini ayarlayın. İndirme tarihinde iade edildi.

**--çek uzunluğu**                         Aktarımdan sonra hedefteki bir dosyanın uzunluğunu kontrol edin. Kaynak ve hedef arasında bir uyumsuzluk varsa, aktarım başarısız olarak işaretlenir. (varsayılan true)

**--check-md5** dizesi İndirilirken MD5 hashes'in ne kadar kesin olarak doğrulanması gerektiğini belirtir. Yalnızca indirirken kullanılabilir. Kullanılabilir seçenekler: NoCheck, LogOnly, FailifDifferent, FailifDifferentOrMissing. (varsayılan "FailIfDifferent")

**--içerik-disposition** dizesi İçerik-disposition üstbilgisini ayarlayın. İndirme tarihinde iade edildi.

**--içerik kodlama** dizesi İçerik kodlama üstbilgisini ayarlayın. İndirme tarihinde iade edildi.

**--içerik dili** dizesi İçerik-dil üstbilgisini ayarlayın. İndirme tarihinde iade edildi.

**--içerik türü** dize dosyanın içerik türünü belirtir. No-guess-mime-type anlamına gelir. İndirme tarihinde iade edildi.

**--dekompresyon**                           İçerik kodlamaları sıkıştırılmış olduklarını gösteriyorsa, dosyaları indirirken otomatik olarak dekomprese edin. Desteklenen içerik kodlama değerleri 'gzip' ve 'deflate' vardır. '.gz'/'.gzip' veya '.zz' dosya uzantıları gerekli değildir, ancak varsa kaldırılır.

**--öznitelik dizesini dışla (Yalnızca** Windows) Öznitelikleri öznitelik listesiyle eşleşen dosyaları hariç tut. Örneğin: A; S; R

**--blob tipi** dize İsteğe bağlı olarak blob türünü (BlockBlob/ PageBlob/ AppendBlob) kapsayıcıdan veya hesaptan lekeler kopyalarken hariç tutmak için belirtir. Bu bayrağın kullanımı, azure hizmeti olmayan hizmetlerden hizmete veri kopyalamak için geçerli değildir. Birden fazla blob ';' ile ayrılmalıdır.

**--exclude-path** string Kopyalanırken bu yolları hariç tut. Bu seçenek joker karakter (*) desteklemez. Göreli yol önekini denetler (Örneğin: myFolder;myFolder/subDirName/file.pdf). Hesap geçişi ile birlikte kullanıldığında, yollar kapsayıcı adını içermez.

**--exclude-desen dizesi** Kopyalanırken bu dosyaları hariç tut. Bu seçenek joker karakter destekler (*)

**--takip-symlinks**                      Yerel dosya sisteminden yüklerken sembolik bağlantıları izleyin.

**--from-to** string İsteğe bağlı olarak kaynak hedef birleşimini belirtir. Örneğin: LocalBlob, BlobLocal, LocalBlobFS.

**-h, --kopyalama** için yardım

**--include-öznitelikler** dizesi (yalnızca Windows) Öznitelikleri öznitelik listesiyle eşleşen dosyaları ekleyin. Örneğin: A; S; R

**--include-path** string Kopyalama yaparken yalnızca bu yolları ekleyin. Bu seçenek joker karakter (*) desteklemez. Göreli yol önekini denetler (Örneğin: myFolder;myFolder/subDirName/file.pdf).

**--include-desen dizesi** Kopyalarken yalnızca bu dosyaları ekleyin. Bu seçenek joker karakter (*) destekler. ';' kullanarak dosyaları ayırın.

**--günlük düzeyi** dizesi Günlük dosyasının günlük ayrıntılılığını, kullanılabilir düzeyleri: INFO(tüm istek/yanıtlar), UYARI(yavaş yanıtlar), HATA (yalnızca başarısız istekler) ve NONE(çıkış günlüğü yok) olarak tanımlayın. (varsayılan "BİlGİ")

**--meta** veri olarak bu anahtar değer çiftleri ile Azure Depolama'ya yükleme meta veri.

**--no-guess-mime-type**                   AzCopy'nin dosyanın uzantısına veya içeriğine bağlı olarak içerik türünü algılamasını engeller.

**--bu** bayrak doğru ayarlanmışsa, dize üzerine çakışan dosyaları ve blobs hedef yazma. Olası değerler 'true', 'false', 'ifSourceNewer' ve 'prompt' içerir. (varsayılan "true")

**--sayfa-blob katmanlı** string Bu blob katmanını kullanarak Azure Depolama'ya sayfa yükleme sayfası blob. (varsayılan "Yok")

**--koruma-son-değiştirilmiş-zaman**          Yalnızca hedef dosya sistemi olduğunda kullanılabilir.

**--koruma-smb-izinleri** dizesi False varsayılan olarak. Farkında kaynaklar (Windows ve Azure Dosyaları) arasında Kobİ AÇ'ları korur. İndirmeler için, yeni SahibiazCopy çalıştıran kullanıcı olmayacak izinleri geri yüklemek için `--backup` de bayrağı kullanmanız gerekir. Bu bayrak, yalnızca dosya filtresi belirtilmediği sürece (örn. `include-pattern`hem dosyalar hem de klasörler için geçerlidir).

**--varsayılan olarak false-smb-info dizekoruyun.** Kobİ'lere duyarlı kaynaklar (Windows ve Azure Dosyaları) arasındaki Kobİ özellik bilgilerini (son yazma süresi, oluşturma süresi, öznitelik bitleri) korur. Yalnızca Azure Dosyaları tarafından desteklenen öznitelik bitleri aktarılır; diğerleri göz ardı edilecektir. Bu bayrak, yalnızca dosya filtresi belirtilmediği sürece (örn. dahil deseni) hem dosyalar hem de klasörler için geçerlidir. Klasörler için aktarılan bilgiler, klasörler için hiçbir zaman korunamayan Son Yazma Süresi dışında dosyalar için olanbilgilerle aynıdır.

**--koruma-sahibi**                       Yalnızca veri indirirken ve yalnızca `--preserve-smb-permissions` kullanıldığında bir etkisi vardır. Doğruysa (varsayılan), Dosya Sahibi ve Grubu karşıdan yüklemelerde korunur. Bu bayrak yanlış olarak `--preserve-smb-permissions` ayarlanırsa, yine de ALA'ları korur, ancak Sahibi ve Grubu AzCopy çalıştıran kullanıcıya dayalı olacaktır.

**--put-md5**                             Her dosyanın bir MD5 karma sını oluşturun ve karmayı hedef blob veya dosyanın İçerik-MD5 özelliği olarak kaydedin. (Varsayılan olarak karma oluşturulmadı.) Yalnızca yükleme yaparken kullanılabilir.

**--özyinelemeli**                            Yerel dosya sisteminden yükleme yaparken alt dizinlere özyinelemeli olarak bakın.

**--s2s-detect-source-changed**           Sayısallöldükten sonra kaynağın değişip değişmediğini denetleyin.

**--s2s-işlemi-geçersiz-meta veri** dizesi Geçersiz meta veri anahtarlarının nasıl işleneceğini belirtir. Kullanılabilir seçenekler: ExcludeIfGeçersiz, FailIfInvalid, RenameIfInvalid. (varsayılan "ExcludeIfGeçersiz")

**--s2s-koruma-erişim katmanı**             Hizmet kopyalamaya hizmet sırasında erişim katmanını koruyun. Hedef depolama hesabının erişim katmanı ayarlamayı desteklediğinden emin olmak için lütfen Azure Blob depolama alanına [bakın: sıcak, serin ve arşiv erişim katmanları.](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers) Erişim katmanı ayarlamanın desteklenmediğini zedeleme durumlarında, lütfen kopyalama erişim katmanını atlamak için s2sPreserveAccessTier=false'u kullanın. (varsayılan true)

**--s2s-koruma-özellikleri**              Hizmet kopyalamaya hizmet sırasında tüm özellikleri koruyun. AWS S3 ve Azure Dosyası tek dosya kaynağı olmayan için liste işlemi nesnelerin ve dosyaların tüm özelliklerini döndürmez. Tüm özellikleri korumak için AzCopy'nin nesne veya dosya başına bir ek istek göndermesi gerekir. (varsayılan true)

## <a name="options-inherited-from-parent-commands"></a>Üst komutlardan devralınan seçenekler

**--kap-mbps uint32**      Transfer hızını saniyede megabit olarak kaplar. Anlık iş artışı kapaktan biraz farklı olabilir. Bu seçenek sıfıra ayarlanmışsa veya atlanırsa, iş elde etme kapaklı değildir.

**--output türü** dize Komutun çıktısının biçimi. Seçenekler şunlardır: metin, json. Varsayılan değer 'metin'dir. (varsayılan "metin")

## <a name="see-also"></a>Ayrıca bkz.

- [azcopy](storage-ref-azcopy.md)
