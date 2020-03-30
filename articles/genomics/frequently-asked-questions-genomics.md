---
title: Sık sorulan sorular - SSS
titleSuffix: Microsoft Genomics
description: Teknik bilgiler, SLA ve faturalandırma dahil olmak üzere Microsoft Genomik hizmetini kullanmayla ilgili sık sorulan soruların yanıtlarını alın.
services: genomics
author: grhuynh
manager: cgronlun
ms.author: grhuynh
ms.service: genomics
ms.topic: troubleshooting
ms.date: 12/07/2017
ms.openlocfilehash: e8806bc4f761214e6740a22093b7e18030fdf881
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76986045"
---
# <a name="microsoft-genomics-common-questions"></a>Microsoft Genomics: Sık sorulan sorular

Bu makalede, Microsoft Genomics ile ilgili olabilecek en iyi sorgular listelenebilmiştir. Microsoft Genomik hizmeti hakkında daha fazla bilgi için [bkz.](overview-what-is-genomics.md) Sorun giderme hakkında daha fazla bilgi için [Sorun Giderme Kılavuzu'na](troubleshooting-guide-genomics.md)bakın. 


## <a name="how-do-i-run-gatk4-workflows-on-microsoft-genomics"></a>Microsoft Genomics'te GATK4 iş akışlarını nasıl çalıştırırım?
Microsoft Genomics hizmetinin config.txt dosyasında, process_name `gatk4`belirtin. Normal faturalandırma oranlarıyla faturalandırılacağınız unutulmamalıdır.

## <a name="how-do-i-enable-output-compression"></a>Çıktı sıkıştırmayı nasıl etkinleştirebilirim?
Çıktı sıkıştırma için isteğe bağlı bir bağımsız değişken kullanarak çıkış vcf veya gvcf sıkıştırabilirsiniz. Bu, vcf `-bgzip` veya `-tabix` gvcf çıktısı üzerinde ,(bgzip çıkışı) `.gz` `.tbi` ve (tabix çıktısı) dosyaları üretmek için çalışmaya eşdeğerdir. `bgzip`vcf veya gvcf dosyasını `tabix` sıkıştırır ve sıkıştırılmış dosya için bir dizin oluşturur. Bağımsız değişken, varsayılan olarak vcf `false` çıktısı için ve `true` varsayılan olarak gcvf çıktısı için ayarlanmış bir boolean'dir. Komut satırında kullanmak için, `--bgzip-output` `true` belirtin `-bz` veya olarak (bgzip ve tabix çalıştırın) veya `false`. Bu bağımsız değişkeni config.txt dosyasında kullanmak için dosyaya ekleyin `bgzip_output: true` veya `bgzip_output: false` ekleyin.

## <a name="what-is-the-sla-for-microsoft-genomics"></a>Microsoft Genomics için SLA nedir?
Microsoft Genomics hizmetinin %99,9'unun iş akışı API isteklerini almak için kullanılabileceğini garanti ediyoruz. Daha fazla bilgi için Bkz. [SLA.](https://azure.microsoft.com/support/legal/sla/genomics/v1_0/)

## <a name="how-does-the-usage-of-microsoft-genomics-show-up-on-my-bill"></a>Microsoft Genomics kullanımı faturamda nasıl yer alalır?
Microsoft Genomik faturaları, iş akışı başına işlenen gigabase sayısına göre faturalanır. Daha fazla bilgi için [Fiyatlandırma'ya](https://azure.microsoft.com/pricing/details/genomics/)bakın.


## <a name="where-can-i-find-a-list-of-all-possible-commands-and-arguments-for-the-msgen-client"></a>`msgen` İstemci için olası tüm komutların ve bağımsız değişkenlerin listesini nerede bulabilirim?
Çalıştırarak kullanılabilir komutların ve bağımsız değişkenlerin `msgen help`tam listesini alabilirsiniz. Başka bağımsız değişken sağlanmazsa, kullanılabilir yardım bölümlerinin `submit`bir `list` `cancel`listesini `status`gösterir, her biri için bir , , ve . Belirli bir komut için yardım `msgen help command`almak için yazın; örneğin, `msgen help submit` tüm gönderme seçeneklerini listeler.

## <a name="what-are-the-most-commonly-used-commands-for-the-msgen-client"></a>`msgen` İstemci için en sık kullanılan komutlar nelerdir?
En sık kullanılan komutlar `msgen` istemci için bağımsız değişkenler şunlardır: 

 |**Komut**          |  **Alan açıklaması** |
 |:--------------------|:-------------         |
 |`list`               |Gönderdiğiniz işlerin listesini verir. Bağımsız değişkenler `msgen help list`için bkz.  |
 |`submit`             |Hizmete bir iş akışı isteği gönderir. Bağımsız değişkenler `msgen help submit`için bkz.|
 |`status`             |`--workflow-id`Tarafından belirtilen iş akışının durumunu verir. Ayrıca `msgen help status`bakınız. |
 |`cancel`             |`--workflow-id`Tarafından belirtilen iş akışının işlenmesini iptal etmek için istek gönderir. Ayrıca `msgen help cancel`bakınız. |

## <a name="where-do-i-get-the-value-for---api-url-base"></a>Değeri nereden `--api-url-base`alabilirim?
Azure portalına gidin ve Genomik hesap sayfanızı açın. **Yönetim** başlığı altında **Erişim anahtarlarını**seçin. Burada hem API URL'sini hem de erişim anahtarlarınızı bulabilirsiniz.

## <a name="where-do-i-get-the-value-for---access-key"></a>Değeri nereden `--access-key`alabilirim?
Azure portalına gidin ve Genomik hesap sayfanızı açın. **Yönetim** başlığı altında **Erişim anahtarlarını**seçin. Burada hem API URL'sini hem de erişim anahtarlarınızı bulabilirsiniz.

## <a name="why-do-i-need-two-access-keys"></a>Neden iki erişim anahtarına ihtiyacım var?
Hizmetin kullanımını kesintiye uğratmadan güncelleştirmek (yeniden oluşturmak) yapmak istediğinizde iki erişim anahtarına ihtiyacınız vardır. Örneğin, ilk anahtarı güncelleştirmek istiyorsanız, tüm yeni iş akışlarının ikinci anahtarı kullanmasını gerekir. Ardından, ilk anahtarı güncelleştirmeden önce bitirmek için ilk anahtarı kullanarak tüm iş akışlarını bekleyin.

## <a name="do-you-save-my-storage-account-keys"></a>Depolama hesabı anahtarlarımı kaydediyor musunuz?
Depolama hesabı anahtarınız, microsoft genomik hizmetinin giriş dosyalarınızı okuması ve çıktı dosyalarını yazması için kısa süreli erişim belirteçleri oluşturmak için kullanılır. Varsayılan belirteç süresi 48 saattir. Belirteç süresi gönder komutu `-sas/--sas-duration` seçeneği ile değiştirilebilir; değeri saat içinde.

## <a name="what-genome-references-can-i-use"></a>Hangi genom referanslarını kullanabilirim?

Bu başvurular desteklenir:

 |Başvuru              | Değeri`-pa/--process-args` |
 |:-------------         |:-------------                 |
 |b37                    | `R=b37m1`                     |
 |hg38                   | `R=hg38m1`                    |      
 |hg38 (alt analiz yok) | `R=hg38m1x`                   |  
 |hg19                   | `R=hg19m1`                    |    

## <a name="how-do-i-format-my-command-line-arguments-as-a-config-file"></a>Komut satırı bağımsız değişkenlerimi config dosyası olarak nasıl biçimlendirebilirim? 

msgen yapılandırma dosyalarını aşağıdaki biçimde anlar:
* Tüm seçenekler, iki nokta üst üste tuşlardan ayrılmış değerlere sahip anahtar değer çiftleri olarak sağlanır.
  Beyaz uzay yoksayılır.
* Satırlarla `#` başlayan satırlar yoksayılır.
* Uzun biçimdeki herhangi bir komut satırı bağımsız değişkeni, önde gelen tirelerini sıyırarak ve sözcükler arasındaki tireleri alt çizgiyle değiştirerek anahtara dönüştürülebilir. Aşağıda bazı dönüşüm örnekleri verilmiştir:

  |Komut satırı bağımsız değişkeni            | Yapılandırma dosya satırı |
  |:-------------                   |:-------------                 |
  |`-u/--api-url-base https://url`  | *api_url_base:https://url*    |
  |`-k/--access-key KEY`            | *access_key:ANAHTAR*              |      
  |`-pa/--process-args R=B37m1`     | *process_args:R-b37m1*        |  

## <a name="next-steps"></a>Sonraki adımlar

Microsoft Genomics'e başlamak için aşağıdaki kaynakları kullanın:
- Microsoft Genomics hizmeti aracılığıyla ilk iş akışınızı çalıştırarak başlayın. [Microsoft Genomics hizmeti nde bir iş akışı çalıştırma](quickstart-run-genomics-workflow-portal.md)
- Microsoft Genomics hizmeti tarafından işlenmek üzere kendi verilerinizi gönderin: [eşleştirilmiş FASTQ](quickstart-input-pair-FASTQ.md) | [BAM](quickstart-input-BAM.md) | [Multiple FASTQ veya BAM](quickstart-input-multiple.md) 

