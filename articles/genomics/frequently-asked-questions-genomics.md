---
title: Sık sorulan sorular-SSS
titleSuffix: Microsoft Genomics
description: Teknik bilgiler, SLA ve faturalandırma dahil olmak üzere Microsoft Genomiks hizmeti kullanımı ile ilgili sık sorulan soruların yanıtlarını alın.
services: genomics
author: grhuynh
manager: cgronlun
ms.author: grhuynh
ms.service: genomics
ms.topic: troubleshooting
ms.date: 12/07/2017
ms.openlocfilehash: e8806bc4f761214e6740a22093b7e18030fdf881
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2020
ms.locfileid: "76986045"
---
# <a name="microsoft-genomics-common-questions"></a>Microsoft Genomiks: genel sorular

Bu makalede, Microsoft Genomiks ile ilgili olabilecek en iyi sorgular listelenmektedir. Microsoft Genomiks hizmeti hakkında daha fazla bilgi için bkz. [Microsoft Genomiks nedir?](overview-what-is-genomics.md). Sorun giderme kılavuzu hakkında daha fazla bilgi için [sorun giderme kılavuzumuzu](troubleshooting-guide-genomics.md)inceleyin. 


## <a name="how-do-i-run-gatk4-workflows-on-microsoft-genomics"></a>GATK4 iş akışlarını Microsoft Genomiks üzerinde Nasıl yaparım? çalıştırmak istiyor musunuz?
Microsoft Genomiks hizmetinin config. txt dosyasında `gatk4`process_name belirtin. Düzenli faturalandırma ücretleri üzerinden faturalandırılabileceğinizi unutmayın.

## <a name="how-do-i-enable-output-compression"></a>Çıkış sıkıştırması etkinleştirilsin mi Nasıl yaparım??
Çıkış sıkıştırması için isteğe bağlı bir bağımsız değişken kullanarak, çıkış VCF veya gvcf 'yi sıkıştırabilirsiniz. Bu, `.gz` (bgzıp çıkışı) ve `.tbi` (tabx çıkışı) dosyalarını oluşturmak için, `-tabix` ve VCF veya gvcf çıktısında `-bgzip` çalışan eşdeğerdir. `bgzip` VCF veya gvcf dosyasını sıkıştırır ve `tabix` sıkıştırılmış dosya için bir dizin oluşturur. Bağımsız değişkeni, varsayılan olarak VCF çıktısı için `false` ve gcvf çıktısı için varsayılan olarak `true` olarak ayarlanan bir Boole değeri. Komut satırında kullanmak için, `true` (bgzip ve tabx çalıştırma) veya `false`olarak `-bz` veya `--bgzip-output` belirtin. Bu bağımsız değişkeni config. txt dosyasında kullanmak için dosyaya `bgzip_output: true` veya `bgzip_output: false` ekleyin.

## <a name="what-is-the-sla-for-microsoft-genomics"></a>Microsoft Genomiks için SLA nedir?
Microsoft Genomiks hizmeti 'nin iş akışı API 'SI isteklerini almak için kullanılabilir olduğu sürenin% 99,9 olacağını garanti ediyoruz. Daha fazla bilgi için bkz. [SLA](https://azure.microsoft.com/support/legal/sla/genomics/v1_0/).

## <a name="how-does-the-usage-of-microsoft-genomics-show-up-on-my-bill"></a>Microsoft Genomiks kullanımı faturamda nasıl görünür?
Microsoft Genomiks, iş akışı başına işlenen gigabaz sayısına göre faturalandırılır. Daha fazla bilgi için bkz. [fiyatlandırma](https://azure.microsoft.com/pricing/details/genomics/).


## <a name="where-can-i-find-a-list-of-all-possible-commands-and-arguments-for-the-msgen-client"></a>`msgen` istemcisi için tüm olası komutların ve bağımsız değişkenlerin listesini nerede bulabilirim?
`msgen help`çalıştırarak, kullanılabilir komutların ve bağımsız değişkenlerin tam listesini alabilirsiniz. Başka bir bağımsız değişken sağlanmadıysa, `submit`, `list`, `cancel`ve `status`her biri için bir tane olmak üzere kullanılabilir yardım bölümlerinin bir listesini gösterir. Belirli bir komut hakkında yardım almak için `msgen help command`yazın. Örneğin, `msgen help submit` tüm gönderme seçeneklerini listeler.

## <a name="what-are-the-most-commonly-used-commands-for-the-msgen-client"></a>`msgen` istemcisi için en sık kullanılan komutlar nelerdir?
En sık kullanılan komutlar `msgen` istemci için bağımsız değişkendir: 

 |**Komut**          |  **Alan açıklaması** |
 |:--------------------|:-------------         |
 |`list`               |Gönderdiğiniz işlerin listesini döndürür. Bağımsız değişkenler için bkz. `msgen help list`.  |
 |`submit`             |Hizmetine bir iş akışı isteği gönderir. Bağımsız değişkenler için bkz. `msgen help submit`.|
 |`status`             |`--workflow-id`tarafından belirtilen iş akışının durumunu döndürür. Ayrıca bkz. `msgen help status`. |
 |`cancel`             |`--workflow-id`tarafından belirtilen iş akışının işlenmesini iptal etmek için bir istek gönderir. Ayrıca bkz. `msgen help cancel`. |

## <a name="where-do-i-get-the-value-for---api-url-base"></a>`--api-url-base`için değeri nereden alabilirim?
Azure portal gidin ve Genomiks hesap sayfanızı açın. **Yönetim** başlığı altında **erişim tuşları**' nı seçin. Burada, API URL 'SI ve erişim anahtarlarınız bulunur.

## <a name="where-do-i-get-the-value-for---access-key"></a>`--access-key`için değeri nereden alabilirim?
Azure portal gidin ve Genomiks hesap sayfanızı açın. **Yönetim** başlığı altında **erişim tuşları**' nı seçin. Burada, API URL 'SI ve erişim anahtarlarınız bulunur.

## <a name="why-do-i-need-two-access-keys"></a>Neden iki erişim anahtarına ihtiyacım var?
Hizmetin kullanımını kesintiye uğramadan güncelleştirmek (yeniden oluşturmak) istemeniz durumunda iki erişim anahtarınız olması gerekir. Örneğin, ilk anahtarı güncelleştirmek istiyorsanız, tüm yeni iş akışlarının ikinci anahtarı kullanması gerekir. Ardından, ilk anahtarı güncelleştirmeden önce, ilk anahtarı kullanarak tüm iş akışlarının bitmesini bekleyin.

## <a name="do-you-save-my-storage-account-keys"></a>Depolama hesabı anahtarlarımı Kaydet?
Depolama hesabı anahtarınız, giriş dosyalarınızı okumak ve çıktı dosyalarını yazmak üzere Microsoft Genomiks hizmeti için kısa süreli erişim belirteçleri oluşturmak üzere kullanılır. Varsayılan belirteç süresi 48 saattir. Belirteç süresi, Gönder komutunun `-sas/--sas-duration` seçeneğiyle değiştirilebilir; değer saat cinsinden.

## <a name="what-genome-references-can-i-use"></a>Hangi genom başvurularını kullanabilirim?

Bu başvurular desteklenir:

 |Başvuru              | `-pa/--process-args` değeri |
 |:-------------         |:-------------                 |
 |b37                    | `R=b37m1`                     |
 |hg38                   | `R=hg38m1`                    |      
 |hg38 (alt analiz yok) | `R=hg38m1x`                   |  
 |hg19                   | `R=hg19m1`                    |    

## <a name="how-do-i-format-my-command-line-arguments-as-a-config-file"></a>Komut satırı bağımsız değişkenlerimin bir yapılandırma dosyası olarak biçimlendirilmesi Nasıl yaparım?? 

msgen 'i yapılandırma dosyalarını aşağıdaki biçimde anlamıştır:
* Tüm seçenekler, anahtar-değer çiftleri olarak anahtarlardan virgülle ayrılmış değerlerle birlikte sağlanır.
  Boşluk yoksayıldı.
* `#` başlayan satırlar yok sayılır.
* Uzun biçimdeki herhangi bir komut satırı bağımsız değişkeni, önde gelen çizgileri gerçekleştirerek ve alt çizgi içeren kelimeler arasındaki tireleri değiştirerek bir anahtara dönüştürülebilir. Bazı dönüştürme örnekleri şunlardır:

  |Komut satırı bağımsız değişkeni            | Yapılandırma dosyası satırı |
  |:-------------                   |:-------------                 |
  |`-u/--api-url-base https://url`  | *api_url_base: https://url*    |
  |`-k/--access-key KEY`            | *access_key: anahtar*              |      
  |`-pa/--process-args R=B37m1`     | *process_args: R-b37m1*        |  

## <a name="next-steps"></a>Sonraki adımlar

Microsoft Genomiks ile çalışmaya başlamak için aşağıdaki kaynakları kullanın:
- İlk iş akışınızı Microsoft Genomiks hizmeti aracılığıyla çalıştırarak başlayın. [Microsoft Genomiks hizmeti üzerinden iş akışı çalıştırma](quickstart-run-genomics-workflow-portal.md)
- Microsoft Genomiks hizmeti tarafından işlenmek üzere kendi verilerinizi gönderme: [EŞLEŞTIRILMIŞ fastq](quickstart-input-pair-FASTQ.md) | [Bad](quickstart-input-BAM.md) | [birden çok fastq veya Baa](quickstart-input-multiple.md) 

