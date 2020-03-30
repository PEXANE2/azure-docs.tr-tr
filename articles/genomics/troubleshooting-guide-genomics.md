---
title: Sorun Giderme Kılavuzu
titleSuffix: Microsoft Genomics
description: Hata iletileri ve bunların nasıl çözüleceği de dahil olmak üzere Microsoft Genomics'i kullanmayla ilgili sorun giderme stratejileri hakkında bilgi edinin.
keywords: sorun giderme, hata, hata ayıklama
services: genomics
author: ruchir
editor: jasonwhowell
ms.author: ruchir
ms.service: genomics
ms.workload: genomics
ms.topic: troubleshooting
ms.date: 10/29/2018
ms.openlocfilehash: f6ef56e4188a7541036db096e4ab35a1b95fc141
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73486006"
---
# <a name="troubleshooting-guide"></a>Sorun giderme kılavuzu

Microsoft Genomics hizmeti MSGEN'i kullanırken karşılaşabileceğiniz sık karşılaşılan sorunlardan bazıları için birkaç sorun giderme ipucu aşağıda verilmiştir.

 Sorun gidermeyle ilgili olmayan SSS için [sık sorulan sorulara](frequently-asked-questions-genomics.md)bakın.
## <a name="step-1-locate-error-codes-associated-with-the-workflow"></a>Adım 1: İş akışıyla ilişkili hata kodlarını bulma

İş akışıyla ilişkili hata iletilerini aşağıdakiler tarafından bulabilirsiniz:

1. Komut satırını kullanma ve`msgen status`
2. standardoutput.txt içeriğinin incelenmesi.

### <a name="1-using-the-command-line-msgen-status"></a>1. Komut satırını kullanma`msgen status`

```bash
msgen status -u URL -k KEY -w ID 
```




Üç gerekli bağımsız değişken vardır:

* URL - API için temel URI
* KEY - Genomik hesabınız için erişim anahtarı
    * URL'nizi ve KEY'inizi bulmak için Azure portalına gidin ve Microsoft Genomics hesap sayfanızı açın. **Yönetim** başlığı altında **Erişim anahtarlarını**seçin. Burada hem API URL'sini hem de erişim anahtarlarınızı bulabilirsiniz.

  
* ID - iş akışı kimliği
    * `msgen list` Komutta iş akışı kimliği türünü bulmak için. Config dosyanızın URL'yi ve erişim anahtarlarınızı içerdiğini ve bulunduğunu varsayarsak, komut şuna benzer: 
        
        ```bash
        msgen list -f "config.txt"
        ```
        Bu komuttan çıktı şu şekilde görünecektir:
        
        ```bash
            Microsoft Genomics command-line client v0.7.4
                Copyright (c) 2018 Microsoft. All rights reserved.
                
                Workflow List
                -------------
                Total Count  : 1
                
                Workflow ID     : 10001
                Status          : Completed successfully
                Message         :
                Process         : snapgatk-20180730_1
                Description     :
                Created Date    : Mon, 27 Aug 2018 20:27:24 GMT
                End Date        : Mon, 27 Aug 2018 20:55:12 GMT
                Wall Clock Time : 0h 27m 48s
                Bases Processed : 1,348,613,600 (1 GBase)
        ```

  > [!NOTE]
  >  Alternatif olarak, doğrudan URL ve KEY'i girmek yerine config dosyasına giden yolu ekleyebilirsiniz. Bu bağımsız değişkenleri komut satırına ve config dosyasına eklerseniz, komut satırı bağımsız değişkenleri öncelikli olur.  

İş akışı Kimliği 1001 ve msgen yürütülebilir aynı yola yerleştirilen config.txt dosyası için komut şu na benzer:

```bash
msgen status -w 1001 -f "config.txt"
```

### <a name="2--examine-the-contents-of-standardoutputtxt"></a>2. standardoutput.txt içeriğini inceleyin 
Söz konusu iş akışı için çıktı kapsayıcısını bulun. MSGEN, `[workflowfilename].logs.zip` her iş akışı yürütülmesinden sonra bir klasör oluşturur. İçeriğini görüntülemek için klasörün zip'ini açın:

* outputFileList.txt - iş akışı sırasında üretilen çıktı dosyalarının listesi
* standarderror.txt - Bu dosya boş.
* standardoutput.txt - iş akışını çalıştırırken oluşan hatalar da dahil olmak üzere tüm üst düzey durum iletilerini kaydeder.
* GATK günlük dosyaları - klasördeki `logs` diğer tüm dosyalar

Sorun giderme için standardoutput.txt'nin içeriğini inceleyin ve görünen hata iletilerini not edin.


## <a name="step-2-try-recommended-steps-for-common-errors"></a>Adım 2: Sık karşılaşılan hatalar için önerilen adımları deneyin

Bu bölümde, Microsoft Genomics hizmetinin (msgen) sık karşılaşılan hataları ve bunları çözmek için kullanabileceğiniz stratejiler kısaca vurgulanır. 

Microsoft Genomik hizmeti (msgen) aşağıdaki iki tür hata atabilir:

1. İç Hizmet Hataları: Hizmetin dahili olan, parametreleri veya giriş dosyaları düzeltilerek çözülmeyen hatalar. Bazen iş akışını yeniden göndermek bu hataları düzeltebilir.
2. Giriş Hataları: Doğru bağımsız değişkenler kullanılarak veya dosya biçimlerini düzelterek çözülebilen hatalar.

### <a name="1-internal-service-errors"></a>1. Dahili hizmet hataları

Dahili hizmet hatası kullanıcı tarafından işlenebilir değildir. İş akışını yeniden gönderebilirsiniz, ancak bu işe yaramazsa Microsoft Genomics desteğine başvurun

| Hata iletisi                                                                                                                            | Önerilen sorun giderme adımları                                                                                                                                   |
|------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Bir iç hata oluştu. İş akışını yeniden göndermeyi deneyin. Bu hatayı tekrar görürseniz, yardım için Microsoft Genomics desteğine başvurun | İş akışını yeniden gönderin. Bir destek [bileti](file-support-ticket-genomics.md )oluşturarak sorun devam ederse yardım için Microsoft Genomics desteğine başvurun. |

### <a name="2-input-errors"></a>2. Giriş hataları

Bu hatalar kullanıcı tarafından işlenebilir. Microsoft Genomics hizmeti, dosya nın türüne ve hata koduna bağlı olarak farklı hata kodları verir. Aşağıda listelenen önerilen sorun giderme adımlarını izleyin.

| Dosya türü | Hata kodu | Hata iletisi                                                                           | Önerilen sorun giderme adımları                                                                                         |
|--------------|------------|-----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|
| Herhangi biri          | 701        | Read [readId] [numberOfBases] üsleri vardır, ancak sınır [maxReadLength]           | Bu hatanın en yaygın nedeni, iki okumanın biraraya gelmelerine yol açan dosya bozulmasıdır. Giriş dosyalarınızı kontrol edin. |
| BAM          | 200        |   '[yourFileName]' dosyasını okuyamıyor.                                                                                       | BAM dosyasının biçimini denetleyin. Düzgün biçimlendirilmiş bir dosyayla iş akışını yeniden gönderin.                                                                           |
| BAM          | 201        |  BAM dosyası okunamıyor [File_name].                                                                                      |BAM dosyasının biçimini denetleyin.  İş akışını doğru biçimlendirilmiş bir dosyayla gönderin.                                                                            |
| BAM          | 202        | BAM dosyası okunamıyor [File_name]. Dosya çok küçük ve eksik üstbilgi.                                                                                        | BAM dosyasının biçimini denetleyin.  İş akışını doğru biçimlendirilmiş bir dosyayla gönderin.                                                                            |
| BAM          | 203        |   BAM dosyası okunamıyor [File_name]. Dosya üstbilgisi bozuktu.                                                                                      |BAM dosyasının biçimini denetleyin.  İş akışını doğru biçimlendirilmiş bir dosyayla gönderin.                                                                           |
| BAM          | 204        |    BAM dosyası okunamıyor [File_name]. Dosya üstbilgisi bozuktu.                                                                                     | BAM dosyasının biçimini denetleyin.  İş akışını doğru biçimlendirilmiş bir dosyayla gönderin.                                                                           |
| BAM          | 205        |    BAM dosyası okunamıyor [File_name]. Dosya üstbilgisi bozuktu.                                                                                     | BAM dosyasının biçimini denetleyin.  İş akışını doğru biçimlendirilmiş bir dosyayla gönderin.                                                                            |
| BAM          | 206        |   BAM dosyası okunamıyor [File_name]. Dosya üstbilgisi bozuktu.                                                                                      | BAM dosyasının biçimini denetleyin.  İş akışını doğru biçimlendirilmiş bir dosyayla gönderin.                                                                            |
| BAM          | 207        |  BAM dosyası okunamıyor [File_name]. Dosya ofset [ofset] yakınında kesildi.                                                                                       | BAM dosyasının biçimini denetleyin.  İş akışını doğru biçimlendirilmiş bir dosyayla gönderin.                                                                            |
| BAM          | 208        |   Geçersiz BAM dosyası. ReadID [Read_Id] dosyasında [File_name] sırası yoktur.                                                                                      | BAM dosyasının biçimini denetleyin.  İş akışını doğru biçimlendirilmiş bir dosyayla gönderin.                                                                             |
| FASTQ        | 300        |  FASTQ dosyasını okuyamıyor. [File_name] yeni bir çizgi ile bitmiyor.                                                                                     | FASTQ dosyasının biçimini düzeltin ve iş akışını yeniden gönderin.                                                                           |
| FASTQ        | 301        |   FASTQ dosyasını okuyamıyor [File_name]. FASTQ kaydı ofset arabellek boyutundan daha büyüktür: [_offset]                                                                                      | FASTQ dosyasının biçimini düzeltin ve iş akışını yeniden gönderin.                                                                         |
| FASTQ        | 302        |     FASTQ Sözdizimi hatası. Dosya [File_name] boş bir satırı vardır.                                                                                    | FASTQ dosyasının biçimini düzeltin ve iş akışını yeniden gönderin.                                                                         |
| FASTQ        | 303        |       FASTQ Sözdizimi hatası. Dosya[File_name] ofset geçersiz bir başlangıç karakteri vardır: [_offset], satır türü: [line_type], karakter: [_char]                                                                                  | FASTQ dosyasının biçimini düzeltin ve iş akışını yeniden gönderin.                                                                         |
| FASTQ        | 304      |  READID'de FASTQ Sözdizimi hatası [_ReadID].  Toplu iş ilk okuma /1 dosyada biten readID yok [File_name]                                                                                       | FASTQ dosyasının biçimini düzeltin ve iş akışını yeniden gönderin.                                                                         |
| FASTQ        | 305        |  READID'de FASTQ Sözdizimi hatası [_readID]. Toplu iş ikinci okuma dosyada /2 ile biten readID yok [File_name]                                                                                      | FASTQ dosyasının biçimini düzeltin ve iş akışını yeniden gönderin.                                                                          |
| FASTQ        | 306        |  READID'de FASTQ Sözdizimi hatası [_ReadID]. Çiftin ilk okumadosyasında /1 ile biten bir kimliği yok [File_name]                                                                                       | FASTQ dosyasının biçimini düzeltin ve iş akışını yeniden gönderin.                                                                          |
| FASTQ        | 307        |   READID'de FASTQ Sözdizimi hatası [_ReadID]. ReadID /1 veya/2 ile bitmez. Dosya [File_name] eşleştirilmiş FASTQ dosyası olarak kullanılamaz.                                                                                      |FASTQ dosyasının biçimini düzeltin ve iş akışını yeniden gönderin.                                                                          |
| FASTQ        | 308        |  FASTQ okuma hatası. Her iki ucunun okumaları farklı yanıt verdi. Doğru FASTQ dosyalarını seçtiniz mi?                                                                                       | FASTQ dosyasının biçimini düzeltin ve iş akışını yeniden gönderin.                                                                         |
|        |       |                                                                                        |                                                                           |

## <a name="step-3-contact-microsoft-genomics-support"></a>Adım 3: Microsoft Genomics desteğine başvurun

İş hatanız devam ederse veya başka sorularınız varsa Azure portalından Microsoft Genomics desteğine başvurun. Destek isteğinin nasıl gönderilene ilişkin ek bilgileri [burada](file-support-ticket-genomics.md)bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Microsoft Genomics hizmetiyle ilgili sık karşılaşılan sorunları nasıl gidereceğinive nasıl çözeceğinizöğrenilmiştir. Daha fazla bilgi ve daha genel SSS için [genel sorulara](frequently-asked-questions-genomics.md)bakın. 
