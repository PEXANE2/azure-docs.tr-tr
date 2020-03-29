---
title: Logstash'tan Azure Veri Gezgini'ne veri alma
description: Bu makalede, Logstash'tan Azure Veri Gezgini'ne nasıl veri öttürmeyi (yüklediğinizi) öğrenirsiniz
author: tamirkamara
ms.author: takamara
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 86f6732cbf2409d3c79a3d7709100e8af24988a0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "66494534"
---
# <a name="ingest-data-from-logstash-to-azure-data-explorer"></a>Logstash'tan Azure Veri Gezgini'ne veri alma

[Logstash,](https://www.elastic.co/products/logstash) aynı anda birçok kaynaktan veri alan, verileri dönüştüren ve verileri en sevdiğiniz "zulaya" gönderen açık kaynak kodlu, sunucu tarafındaki bir veri işleme ardışık hattıdır. Bu makalede, bu verileri günlük ve telemetri verileri için hızlı ve yüksek ölçeklenebilir bir veri arama hizmeti olan Azure Veri Gezgini'ne gönderirsiniz. Başlangıçta bir test kümesinde bir tablo ve veri eşleme oluşturur ve ardından logstash'ı tabloya veri göndermek ve sonuçları doğrulamak için yönlendirirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Hesabınız yoksa, başlamadan önce ücretsiz bir [Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* Azure Veri Gezgini [test kümesi ve veritabanı](create-cluster-database-portal.md)
* Logstash sürüm 6+ [Kurulum talimatları](https://www.elastic.co/guide/en/logstash/current/installing-logstash.html)

## <a name="create-a-table"></a>Bir tablo oluşturma

Bir küme ve veritabanına sahip olduktan sonra, bir tablo oluşturma nın zamanı gelmiştir.

1. Tablo oluşturmak için veritabanı sorgu pencerenizde aşağıdaki komutu çalıştırın:

    ```Kusto
    .create table logs (timestamp: datetime, message: string)
    ```

1. Yeni tablonun `logs` oluşturulduğunu ve boş olduğunu doğrulamak için aşağıdaki komutu çalıştırın:
    ```Kusto
    logs
    | count
    ```

## <a name="create-a-mapping"></a>Eşleme oluşturma

Eşleme, Azure Veri Gezgini tarafından gelen verileri hedef tablo şemasına dönüştürmek için kullanılır. Aşağıdaki komut, `basicmsg` gelen json'dan özellikleri ayıklayan `path` ve bunları . `column`

Sorgu penceresinde aşağıdaki komutu çalıştırın:

```Kusto
.create table logs ingestion json mapping 'basicmsg' '[{"column":"timestamp","path":"$.@timestamp"},{"column":"message","path":"$.message"}]'
```

## <a name="install-the-logstash-output-plugin"></a>Logstash çıkış eklentisini yükleyin

Logstash çıktı eklentisi Azure Veri Gezgini ile iletişim kurar ve verileri hizmete gönderir.
Eklentiyi yüklemek için Logstash kök dizininin içindeki aşağıdaki komutu çalıştırın:

```sh
bin/logstash-plugin install logstash-output-kusto
```

## <a name="configure-logstash-to-generate-a-sample-dataset"></a>Örnek bir veri kümesi oluşturmak için Logstash'ı yapılandır

Logstash, uçlardan uca bir ardışık ardışık ardışık hattı sınamak için kullanılabilecek örnek olaylar oluşturabilir.
Logstash'ı zaten kullanıyorsanız ve kendi etkinlik akışınıza erişin, bir sonraki bölüme gidin. 

> [!NOTE]
> Kendi verilerinizi kullanıyorsanız, tabloyu ve önceki adımlarda tanımlanan nesneleri eşlenin.

1. Gerekli ardışık lık ayarlarını içeren yeni bir metin dosyasını (vi kullanarak) edin:

    ```sh
    vi test.conf
    ```

1. Logstash'a 1000 test olayı oluşturmasını söyleyecek aşağıdaki ayarları yapıştırın:

    ```ruby
    input {
        stdin { }
        generator {
            message => "Test Message 123"
            count => 1000
        }
    }
    ```

Bu yapılandırma, `stdin` kendiniz daha fazla ileti yazmanızı sağlayacak giriş eklentisini de içerir (bunları ardışık duruma göndermek için *Enter'u* kullandığınızdan emin olun).

## <a name="configure-logstash-to-send-data-to-azure-data-explorer"></a>Azure Veri Gezgini'ne veri göndermek için Logstash'ı yapılandırma

Aşağıdaki ayarları önceki adımda kullanılan aynı config dosyasına yapıştırın. Tüm yer tutucuları kurulumunuz için ilgili değerlerle değiştirin. Daha fazla bilgi için Bkz. [AAD Uygulaması Oluşturma.](/azure/kusto/management/access-control/how-to-provision-aad-app) 

```ruby
output {
    kusto {
            path => "/tmp/kusto/%{+YYYY-MM-dd-HH-mm-ss}.txt"
            ingest_url => "https://ingest-<cluster name>.kusto.windows.net/"
            app_id => "<application id>"
            app_key => "<application key/secret>"
            app_tenant => "<tenant id>"
            database => "<database name>"
            table => "<target table>" # logs as defined above
            mapping => "<mapping name>" # basicmsg as defined above
    }
}
```

| Parametre Adı | Açıklama |
| --- | --- |
| **Yolu** | Logstash eklentisi, olayları Azure Veri Gezgini'ne göndermeden önce olayları geçici dosyalara yazar. Bu parametre, dosyaların yazılması gereken bir yol ve Azure Veri Gezgini hizmetine yüklemeyi tetiklemek için dosya döndürme için bir zaman ifadesi içerir.|
| **ingest_url** | Yutma ile ilgili iletişim için Kusto bitiş noktası.|
| **app_id**, **app_key**ve **app_tenant**| Azure Veri Gezgini'ne bağlanmak için gereken kimlik bilgileri. Yutma ayrıcalıklarına sahip bir uygulama kullandığınızdan emin olun. |
| **Veritabanı**| Olayları yerleştirmek için veritabanı adı. |
| **Tablo** | Olayları yerleştirmek için tablo adını hedefle. |
| **eşleme** | Eşleme, gelen bir olay json dizesini doğru satır biçimine eşlemek için kullanılır (hangi özelliğin hangi sütuna girdiğini tanımlar). |

## <a name="run-logstash"></a>Run Logstash

Artık Logstash'ı çalıştırmaya ve ayarlarımızı test etmeye hazırız.

1. Logstash kök dizininde aşağıdaki komutu çalıştırın:

    ```sh
    bin/logstash -f test.conf
    ```

    Ekrana yazdırılan bilgileri ve ardından örnek yapılandırmamız tarafından oluşturulan 1000 iletiyi görmeniz gerekir. Bu noktada, el ile daha fazla ileti de girebilirsiniz.

1. Birkaç dakika sonra, tanımladığınız tablodaki iletileri görmek için aşağıdaki Veri Gezgini sorgusunu çalıştırın:

    ```Kusto
    logs
    | order by timestamp desc
    ```

1. Logstash'tan çıkmak için Ctrl+C'yi seçin

## <a name="clean-up-resources"></a>Kaynakları temizleme

Tabloyu temizlemek için veritabanınızda aşağıdaki `logs` komutu çalıştırın:

```Kusto
.drop table logs
```

## <a name="next-steps"></a>Sonraki adımlar

* [Sorgu yazma](write-queries.md)