---
title: Sanal makine sertifikasyonu - sorunlar ve çözümler
description: Bu makalede VM görüntülerinin yaygın hata iletileri açıklanmaktadır. Ayrıca ilgili çözümleri de anlatmaktadır
author: v-miegge
ms.author: v-krmall
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: troubleshooting
ms.date: 06/16/2020
ms.openlocfilehash: 7bd3f1a5b242ee5196e92456cb3fc8c97f8f5b27
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85958540"
---
# <a name="virtual-machine-certification---issues-and-solutions"></a>Sanal makine sertifikasyonu - sorunlar ve çözümler

Sanal makine (VM) görüntülerinizi Azure Marketi 'ne yayımlarken, Azure ekibi, önyükleme, güvenlik ve Azure uyumluluğunu sağlamak için VM görüntüsünü doğrular. Yüksek kaliteli testlerin herhangi biri başarısız olursa, yayımlama hatayı içeren bir iletiyle başarısız olur.

Bu makalede VM görüntülerinin yaygın hata iletileri açıklanmaktadır. Ayrıca ilgili çözümleri de anlatmaktadır:

> [!NOTE]
> Geliştirme için sorularınız veya geri bildiriminiz varsa, [Iş Ortağı Merkezi desteğiyle](https://partner.microsoft.com/support/v2/?stage=1)iletişim kurun.

## <a name="approved-base-image"></a>Onaylanan temel görüntü

Görüntünüzü güncelleştirmelerle yeniden yayımlamak için bir istek gönderdiğinizde, bölüm numarası doğrulama test çalışması başarısız olabilir. Bu örnekte, görüntünüz onaylanmayacaktır.

Bu hata, başka bir yayımcıya ait olan ve görüntüyü güncelleştirdiğiniz bir temel görüntü kullandığınızda oluşur. Bu durumda, görüntünüzü yayımlamanıza izin verilmez.

Bu sorunu onarmak için Azure Marketi 'nden en son görüntünüzü alın ve bu görüntüde değişiklik yapın. Görüntünüzü arayabilecek onaylanan temel görüntüleri görüntülemek için aşağıdakilere bakın:

- [Linux-görüntüler](../../virtual-machines/linux/endorsed-distros.md?toc=/azure/virtual-machines/linux/toc.json)
- [Windows-görüntüler](create-azure-vm-technical-asset.md#create-a-vm-image-using-an-approved-base))

## <a name="vm-extension-failure"></a>VM Uzantısı hatası

Görüntünüzün VM uzantısını destekleyip desteklemediğini denetlemek için şu adımları izleyin:

VM uzantılarını etkinleştir:

1. Linux VM 'yi seçin.
2. **Tanılama ayarları**' na gidin.
3. **Depolama hesabını**güncelleştirerek taban matrislerini etkinleştirin.
4. **Kaydet**'i seçin.

   ![Konuk düzeyinde izlemeyi etkinleştir](./media/vm-certification-issues-solutions-1.png)

VM uzantılarının düzgün etkinleştirilip etkinleştirilmediğini denetleyin:

5. VM 'nin **VM uzantıları** sekmesine gidin ve **Linux Tanılama uzantısını**doğrulayın.
6. Durum **sağlama başarılı** olursa, uzantılar test çalışması geçirilir.
7. Durum **sağlama başarısız** olursa, uzantılar test çalışması başarısız olur ve sağlamlaştırılmış bayrağını ayarlamanız gerekir.

   ![Sağlama başarılı oldu](./media/vm-certification-issues-solutions-2.png)

   VM Uzantısı başarısız olursa, etkinleştirmek üzere [ölçümleri ve günlükleri izlemek Için Linux Tanılama uzantısı 'Nı kullanın](../../virtual-machines/extensions/diagnostics-linux.md) bölümüne gidin. VM uzantısının etkinleştirilmesini istemiyorsanız, destek ekibine başvurun ve uzantıyı devre dışı bırakamalarını isteyin.

## <a name="virtual-machine-provisioning-issue"></a>Sanal makine sağlama sorunu

Teklifinizi göndermeden önce, sağlama işleminin VM için gerekli olduğundan emin olun. VM 'yi sağlamaya yönelik JSON biçimini görüntülemek için [Azure sanal makine (VM) görüntü sertifikası](azure-vm-image-certification.md)' na gidin.

Sağlama sorunları aşağıdaki başarısızlık senaryolarını içerebilir:

|S.NO|error|reason|çözümden|
|---|---|---|---|
|1|Geçersiz sanal sabit disk (VHD)|VHD altbilgisinde belirtilen tanımlama bilgisi değeri doğru değilse, VHD geçersiz olarak kabul edilir.|Görüntüyü yeniden oluşturun ve isteği iletin.|
|2|Geçersiz blob türü|Kullanılan blok, bir sayfa türü yerine bir blob türü olduğu için VM sağlama başarısız oldu.|Görüntüyü yeniden oluşturun ve isteği iletin.|
|3|Sağlama zaman aşımı veya düzgün Genelleştirilmiş|VM Genelleştirme ile ilgili bir sorun var.|Yansımayı Genelleştirme ile yeniden oluşturun ve isteği iletin.|

> [!NOTE]
> VM Genelleştirme ile ilgili belgeler için bu bağlantıları izleyin:
> - [Linux](create-azure-vm-technical-asset.md#generalize-the-image))
> - [Windows](../../virtual-machines/windows/capture-image-resource.md#generalize-the-windows-vm-using-sysprep))

## <a name="software-compliance-for-windows"></a>Windows için yazılım uyumluluğu

Windows görüntü isteğiniz yazılım uyumluluğu nedeniyle reddedildiyse, Azure Marketi 'nden ilgili SQL sürümü temel görüntüsünü almak yerine SQL Server yüklü bir Windows görüntüsü oluşturmuş olabilirsiniz.

Üzerinde SQL Server yüklü olan kendi Windows görüntünüzü oluşturmayın. Bunun yerine, Azure Marketi 'nden onaylanan SQL temel görüntülerini (Enterprise/Standard/Web) kullanın.

Visual Studio 'yu veya Office lisanslı herhangi bir ürünü yüklemeye çalışıyorsanız, önceki onay için destek ekibine başvurun.

Daha fazla bilgi için, onaylanan bir temel seçmek üzere [Azure sanal makineli teknik varlıklarınızı oluşturma](create-azure-vm-technical-asset.md#create-a-vm-image-using-an-approved-base)makalesini ziyaret edin.

## <a name="tool-kit-test-case-execution-failed"></a>Araç seti test çalışması yürütülemedi

Microsoft sertifika araç seti, VHD/görüntünüzün Azure ortamıyla uyumlu olduğunu doğrulamak için test çalışmalarını yürütmelerine yardımcı olur.

[Microsoft sertifika araç setini](azure-vm-image-certification.md)indirin.

## <a name="linux-test-cases"></a>Linux test çalışmaları

Aşağıda, araç setinin yürütebilmesi için Linux test çalışmaları verilmiştir. Test doğrulaması, açıklamada belirtilmiştir.

|S.No|test çalışmaları|açıklama|
|---|---|---|
|1|Bash geçmişi|VM görüntüsünü oluşturmadan önce bash geçmiş dosyaları temizlenmelidir.|
|2|Linux Aracısı sürümü|Azure Linux Aracısı 2.2.41 ve ötesinde yüklenmelidir.|
|3|Gerekli çekirdek parametreleri|Aşağıdaki çekirdek parametrelerinin ayarlandığını doğrular: <br>Konsol = ttyS0<br>earlyprintk = ttyS0<br>rootdelay = 300|
|4|İşletim sistemi diskinde takas bölümü|Takas bölümlerinin işletim sistemi diskinde oluşturulmadığını doğrular.|
|5|İşletim sistemi diskinde kök bölüm|İşletim sistemi diski için tek bir kök bölümü oluşturun.|
|6|OpenSSL sürümü|OpenSSL sürümü v 0.9.8 'den büyük veya buna eşit olmalıdır.|
|7|Python sürümü|Python sürüm 2.6 + kesinlikle önerilir.|
|8|İstemci canlı aralığı|ClientAliveInterval değerini 180 olarak ayarlayın. Uygulama gereksinimi üzerine 30 ila 235 arasında ayarlanabilir. Son kullanıcılarınız için SSH 'yi etkinleştirirseniz, bu değerin açıklanacak şekilde ayarlanması gerekir.|
|9|İşletim sistemi mimarisi|Yalnızca 64 bit işletim sistemleri desteklenir.|
|10|Otomatik güncelleştirme|Linux Aracısı otomatik güncelleştirme özelliğinin etkinleştirilip etkinleştirilmediğini belirler.|

### <a name="common-errors-found-while-executing-the-previous-test-cases"></a>Önceki test çalışmaları yürütülürken karşılaşılan ortak hatalar

Önceki test çalışmaları yürütülürken ortak hatalar bulundu.
 
|S.NO|test çalışması|error|çözümden|
|---|---|---|---|
|1|Linux Aracısı sürüm test çalışması|En düşük Linux Aracısı sürümü 2,241 veya daha yüksektir. Bu gereksinim 1 Mayıs 2020 ' den itibaren zorunludur|[İsteğin gönderileceği](https://support.microsoft.com/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support), görüntünün gereken sürümle güncelleştirilmeleri gerekir.|
|2|Bash geçmiş test çalışması|Gönderilen görüntinizdeki Bash geçmişinin boyutu 1 KB 'tan büyükse bir hata görürsünüz. Potansiyel olarak hassas bilgilerin Bash geçmiş dosyanızda yakalandığından emin olmak için Boyut 1 KB 'a kısıtlanır.|Bu sorunu çözmek için, VHD 'yi başka bir çalışan VM 'ye bağlayın ve herhangi bir değişiklik yapın (örneğin, `.bash` geçmiş dosyalarını silin) boyutu 1 KB 'tan küçük veya buna eşit olacak şekilde azaltmak istediğiniz değişiklikleri yapın.|
|3|Gerekli çekirdek parametresi test çalışması|**Konsolun** değeri **ttyS0**olarak ayarlanmadıysa bu hatayı alırsınız. Komutu yürüterek kontrol edin:<br>`cat /proc/cmdline`|**Konsol** Için değeri **ttyS0** olarak ayarlayın ve isteği yeniden gönderin.|
|4|ClientAlive Interval test çalışması|Araç seti sonucu, bu test çalışması için başarısız bir sonuç veriyorsa **ClientAliveInterval**için uygun olmayan bir değer vardır.|**ClientAliveInterval** değerini 235 ' e eşit veya daha küçük olarak ayarlayın, sonra isteği yeniden gönderin.|

### <a name="windows-test-cases"></a>Windows test çalışmaları

Araç seti 'nin yürütebilmesi için aşağıda yer alan Windows test durumları verilmiştir. Test doğrulaması, açıklamada belirtilmiştir.

|S.No|test çalışmaları|açıklama|
|---|---|---|---|
|1|İşletim sistemi mimarisi|Azure yalnızca 64 bitlik işletim sistemlerini destekler.|
|2|Kullanıcı hesabı bağımlılığı|Uygulama yürütme, yönetici hesabına bağımlı olmamalıdır.|
|3|Yük Devretme Kümesi|Windows Server Yük Devretme Kümelemesi özelliği henüz desteklenmiyor. uygulamanın bu özelliğe bağımlı olmaması gerekir.|
|4|IPV6|IPv6 henüz Azure ortamında desteklenmiyor. Uygulamanın bu özelliğe bağımlı olmaması gerekir.|
|5|DHCP|Dinamik ana bilgisayar Yapılandırma Protokolü sunucu rolü henüz desteklenmiyor. Uygulamanın bu özelliğe bağımlı olmaması gerekir.|
|6|Hyper-V|Hyper-V sunucu rolü henüz desteklenmiyor. Uygulamanın bu özelliğe bağımlı olmaması gerekir.|
|7|Uzaktan Erişim|Uzaktan erişim (doğrudan erişim) sunucu rolü henüz desteklenmiyor. Uygulamanın bu özelliğe bağımlı olmaması gerekir.|
|8|Rights Management Hizmetleri|Rights Management Hizmetleri. Sunucu rolü henüz desteklenmiyor. Uygulamanın bu özelliğe bağımlı olmaması gerekir.|
|9|Windows Dağıtım Hizmetleri|Windows Dağıtım Hizmetleri. Sunucu rolü henüz desteklenmiyor. Uygulamanın bu özelliğe bağımlı olmaması gerekir.|
|10|BitLocker Sürücü Şifrelemesi|BitLocker Sürücü Şifrelemesi işletim sistemi sabit diskinde desteklenmez, ancak veri disklerinde kullanılabilir.|
|11|Internet Depolama adı sunucusu|Internet Depolama adı sunucusu özelliği henüz desteklenmiyor. Uygulamanın bu özelliğe bağımlı olmaması gerekir.|
|12|Çok Yollu G/Ç|Çok yollu g/ç. Bu sunucu özelliği henüz desteklenmiyor. Uygulamanın bu özelliğe bağımlı olmaması gerekir.|
|13|Ağ Yükü Dengeleme|Ağ Yükü Dengeleme. Bu sunucu özelliği henüz desteklenmiyor. Uygulamanın bu özelliğe bağımlı olmaması gerekir.|
|14|Eş Adı Çözümleme Protokolü|Eş adı çözümleme protokolü. Bu sunucu özelliği henüz desteklenmiyor. Uygulamanın bu özelliğe bağımlı olmaması gerekir.|
|15|SNMP Hizmetleri|SNMP Hizmetleri özelliği henüz desteklenmiyor. Uygulamanın bu özelliğe bağımlı olmaması gerekir.|
|16|Windows Internet ad hizmeti|Windows Internet ad hizmeti. Bu sunucu özelliği henüz desteklenmiyor. Uygulamanın bu özelliğe bağımlı olmaması gerekir.|
|17|Kablosuz LAN Hizmeti|Kablosuz LAN Hizmeti. Bu sunucu özelliği henüz desteklenmiyor. Uygulamanın bu özelliğe bağımlı olmaması gerekir.|

Yukarıdaki test durumlarında tüm hatalarda karşılaşırsanız, çözüm için önceki tablodaki **Açıklama** sütununa bakın. Daha fazla bilgiye ihtiyacınız varsa destek ekibine başvurun. 

## <a name="data-disk-size-verification"></a>Veri diski boyut doğrulaması

Veri diski ile gönderilen herhangi bir isteğin boyutu 1023 GB 'den büyükse, bu istek onaylanmayacaktır. Bu kural hem Linux & Windows için geçerlidir.

isteği 1023 GB 'tan küçük veya buna eşit bir boyutla yeniden gönderin.

## <a name="os-disk-size-validation"></a>İşletim sistemi disk boyutu doğrulaması

İşletim sistemi disk boyutuyla ilgili sınırlamalar için aşağıdaki kurallara bakın. Herhangi bir istek gönderdiğinizde, işletim sistemi disk boyutunun Linux veya Windows için kısıtlama dahilinde olduğunu doğrulayın.

|İşletim Sistemi|Önerilen VHD boyutu|
|---|---|
|Linux|30 GB ila 1023 GB|
|Windows|30 GB ila 250 GB|

VM 'Ler temeldeki işletim sistemine erişime izin verse de VHD boyutunun VHD için yeterince büyük olduğundan emin olun. Diskler kesinti olmadan Genişletilebilir olmadığından, 30 ile 50 GB arasında bir disk boyutu kullanın.

|VHD boyutu|dolu gerçek boyut|çözümden|
|---|---|---|
|>500 TiB|yok|özel durum onayı için destek ekibine başvurun.|
|250-500 TiB|>200 GiB blob boyutundan farklı|özel durum onayı için destek ekibine başvurun.|

> [!NOTE]
> Daha büyük disk boyutları daha yüksek maliyetler doğurur ve sağlama ve çoğaltma adımları sırasında bir gecikme olur. Bu gecikme ve maliyet nedeniyle, destek ekibi özel durum onayı için gerekçe arayabilir.

## <a name="wannacry-patch-verification-test-for-windows"></a>Windows için WannaCry Patch doğrulama testi

WannaCry virüsü ile ilgili olası bir saldırıyı engellemek için tüm Windows yansıma isteklerinin en son düzeltme ekiyle güncelleştirildiğinden emin olun.

Windows Server 'da düzeltme eki uygulanan sürümü denetlemek için, işletim sistemi ayrıntısı ve destekleyeceği en düşük sürüm için aşağıdaki tabloya bakın. 

Görüntü dosyası sürümü veya ' den doğrulanabilir `C:\windows\system32\drivers\srv.sys` `srv2.sys` .

> [!NOTE]
> WindowsServer2019 zorunlu sürüm gereksinimlerine sahip değildir.

|İşletim Sistemi|sürüm|
|---|---|
|WindowsServer2008R2|6.1.7601.23689|
|WindowsServer2012|6.2.9200.22099|
|WindowsServer2012R2|6.3.9600.18604|
|WindowsServer2016|10.0.14393.953|
|WindowsServer2019|NA|

## <a name="sack-vulnerability-patch-verification"></a>SACK güvenlik düzeltme eki doğrulaması

Bir Linux görüntüsü gönderdiğinizde, isteğiniz çekirdek sürümü sorunları nedeniyle reddedilebilir.

Çekirdeği onaylanan bir sürümle güncelleştirin ve isteği yeniden gönderin. Onaylanan çekirdek sürümünü aşağıdaki tabloda bulabilirsiniz. Sürüm numarası aşağıda listelenen değere eşit veya ondan büyük olmalıdır.

Resminiz aşağıdaki çekirdek sürümlerinden biriyle yüklenmediyse görüntünüzü doğru düzeltme ekleriyle güncelleştirin. Aşağıdaki bağlantılardan daha fazla bilgi edinebilirsiniz. Görüntü, gereken bu düzeltme ekleriyle güncelleştirildikten sonra destek ekibinin gerekli onayını iste:

- CVE-2019-11477 
- CVE-2019-11478 
- CVE-2019-11479

|İşletim sistemi ailesi|sürüm|ine|
|---|---|---|
|Ubuntu|14,04 LTS|4.4.0-151.552| 
||14,04 LTS|4.15.0-1049-*-Azure|
||16,04 LTS|4.15.0-1049|
||18,04 LTS|4.18.0-1023|
||18,04 LTS|5.0.0-1025|
||18,10|4.18.0-1023|
||19,04|5.0.0-1010|
||19,04|5.3.0-1004|
|RHEL ve sent OS|6,10|2.6.32-754.15.3|
||7.2|3.10.0-327.79.2|
||7.3|3.10.0-514.66.2|
||7.4|3.10.0-693.50.3|
||7,5|3.10.0-862.34.2|
||7,6|3.10.0-957.21.3|
||7,7|3.10.0-1062.1.1|
||8.0|4.18.0-80.4.2|
||8.1|4.18.0-147|
||"7-RAW" (7,6)||
||"7-LVM" (7,6)|3.10.0-957.21.3|
||RHEL-SAP 7,4|TBD|
||RHEL-SAP 7,5|TBD|
|SLES|SLES11SP4 (SAP dahil)|3.0.101-108.95.2|
||SAP için SLES12SP1|3.12.74-60.64.115.1|
||SAP için SLES12SP2|4.4.121-92.114.1|
||SLES12SP3|4.4180-4.31.1 (çekirdek-Azure)|
||SAP için SLES12SP3|4.4.180-94.97.1|
||SLES12SP4|4.12.14-6.15.2 (çekirdek-Azure)|
||SAP için SLES12SP4|4.12.14-95.19.1|
||SLES15|4.12.14-5.30.1 (çekirdek-Azure)|
||SAP için SLES15|4.12.14-5.30.1 (çekirdek-Azure)|
||SLES15SP1|4.12.14-5.30.1 (çekirdek-Azure)|
|Oracle|6,10|UEK2 2.6.39-400.312.2<br>UEK3 3.8.13-118.35.2<br>RHCK 2.6.32-754.15.3 
||7.0-7.5|UEK3 3.8.13-118.35.2<br>UEK4 4.1.12-124.28.3<br>RHCK yukarıdaki RHEL 'yi izler|
||7,6|RHCK 3.10.0-957.21.3<br>UEK5 4.14.35-1902.2.0|
|CoreOS kararlı 2079.6.0|4.19.43*|
||Beta 2135.3.1|4.19.50*|
||Alpha 2163.2.1|4.19.50*|
|Debian|JESSIE (güvenlik)|3.16.68-2|
||jessıya arkabağlantı noktaları|4.9.168-1 + deb9u3|
||Esnetme (güvenlik)|4.9.168-1 + deb9u3|
||Deuter GNU/Linux 10 (Buster)|Detem 6.3.0-18 + deb9u1|
||Buster, SID (Esnetme noktaları)|4.19.37-5|

## <a name="image-size-should-be-in-multiples-of-megabytes"></a>Görüntü boyutu megabayt katları cinsinden olmalıdır

Azure 'daki tüm VHD 'Lerde 1 MB 'a kadar bir sanal boyut hizalanmış olmalıdır. VHD 'niz önerilen sanal boyuta bağlı değilse isteğiniz reddedilebilir.

ham diskten VHD 'ye dönüştürme yaparken yönergeleri izleyin ve ham disk boyutunun 1 MB 'ın katı olduğundan emin olmanız gerekir. Daha fazla bilgi için bkz. [onaylamayan dağıtımlar Için bilgi](../../virtual-machines/linux/create-upload-generic.md)

## <a name="vm-access-denied"></a>VM erişimi reddedildi

VM 'de test çalışmalarını yürütürken erişim reddi sorunları yaşıyorsanız, bu durum test çalışmalarını yürütmek için yeterli ayrıcalıklara sahip olabilir.

Kendi kendine test çalışmalarının yürütüldüğü hesap için doğru erişimin etkinleştirilip etkinleştirilmediğini denetleyin. Aksi takdirde, test çalışmalarını yürütmek için erişimi etkinleştirin. Erişimi etkinleştirmek istemiyorsanız, kendi kendine test çalışması sonuçlarını destek ekibi ile paylaşabilirsiniz.

## <a name="download-failure"></a>İndirme hatası
    
SAS URL 'sini kullanarak VM görüntüsünü indirirken oluşan herhangi bir sorun için aşağıdaki tabloya bakın.

|S.NO|error|reason|çözümden|
|---|---|---|---|
|1|Blob bulunamadı|VHD silinmiş ya da belirtilen konumdan taşınmış olabilir|| 
|2|Kullanılan blob|VHD başka bir iç işlem tarafından kullanılıyor|VHD, SAS URL 'SI kullanılarak indirilirken kullanılan bir durumda olmalıdır.|
|3|Geçersiz SAS URL 'SI|Bu VHD için ilişkili SAS URL 'SI yanlış.|Doğru SAS URL 'sini alın.|
|4|Geçersiz Imza|Bu VHD için ilişkili SAS URL 'SI yanlış.|Doğru SAS URL 'sini alın.|
|6|HTTP koşullu üstbilgisi|SAS URL 'SI geçersiz.|Doğru SAS URL 'sini alın.|
|7|Geçersiz VHD adı|**%** VHD adında ya da **""** gibi özel karakterler olup olmadığını denetleyin|Özel karakterleri kaldırarak VHD dosyasını yeniden adlandırma|

## <a name="first-1-mb-partition"></a>İlk 1 MB bölüm

VHD 'yi gönderirken, VHD 'nin ilk 1 MB bölümünün boş olduğundan emin olun. Aksi takdirde, isteğiniz başarısız olur.

## <a name="default-credentials"></a>Varsayılan kimlik bilgileri

Varsayılan kimlik bilgilerinin gönderilen VHD ile gönderilmediğinden her zaman emin olun. Varsayılan kimlik bilgilerini ekleme, VHD 'nin güvenlik tehditlerine karşı daha savunmasız olmasını sağlar. Bunun yerine, VHD 'YI gönderirken kendi kimlik bilgilerinizi oluşturun.
  
## <a name="datadisk-mapped-incorrectly"></a>Veri diski yanlış eşlendi

Bir istek birden çok veri diski ile gönderildiğinde, ancak sırası sıralı değilse, bu bir eşleme sorunu olarak kabul edilir. Örneğin, üç veri diski varsa, numaralandırma sırası **0, 1, 2**olmalıdır. Başka herhangi bir sıra, eşleme sorunu olarak kabul edilir.

isteği veri disklerinin doğru sıralaması ile yeniden gönderin.

## <a name="incorrect-os-mapping"></a>Yanlış işletim sistemi eşlemesi

Bir görüntü oluşturulduğunda, eşlenmiş veya yanlış işletim sistemi etiketine atanmış olabilir. Örneğin, görüntü oluştururken işletim sistemi adının bir parçası olarak **Windows** seçildiğinde, işletim sistemi diski yalnızca Windows ile yüklenmelidir. Aynı Linux için geçerlidir.

## <a name="vm-not-generalized"></a>VM Genelleştirilmiş değil

Azure Marketi 'nden alınan tüm görüntülerin yeniden kullanılabilmesi için, işletim sistemi VHD 'sinin genelleştirilmesi gerekir.

Linux:

Aşağıdaki işlem bir Linux sanal makinesini genelleştirir ve ayrı bir VM olarak yeniden dağıtır.

SSH penceresinde, aşağıdaki komutu girin:`sudo waagent -deprovision+user`

Windows:

Windows görüntüleri ile genelleştirilir `sysreptool` .

Bu araçla ilgili daha fazla bilgiyi [Sysprep (Sistem hazırlama) genel]( https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview) görünümünde bulabilirsiniz

## <a name="datadisk-error"></a>Veri diski hatası

Veri diskiyle ilgili hatalara çözümler için aşağıdaki tabloya bakın.

|Hata|reason|çözümden|
|---|---|---|
|`DataDisk- InvalidUrl:`|Bu hata, teklif gönderilirken LUN için belirtilen geçersiz bir sayı olabilir.|Veri diski için LUN numarası sırasının Iş Ortağı Merkezi 'nde olduğunu doğrulayın.|
|`DataDisk- NotFound:`|Bu hata, belirtilen bir SAS URL 'sinde bulunmayan bir veri diskinin olmasından dolayı olabilir|Veri diskinin istekte belirtilen SAS URL 'sinde bulunduğunu doğrulayın.|

## <a name="remote-access-issue"></a>Uzaktan erişim sorunu

Windows görüntüsü için RDP seçeneği etkinleştirilmemişse, bu hatayı alırsınız. 

Göndermeden önce Windows görüntüleri için RDP erişimini etkinleştirin.

## <a name="next-steps"></a>Sonraki Adımlar

Geliştirme için sorularınız veya geri bildiriminiz varsa, [Iş Ortağı Merkezi desteğiyle](https://partner.microsoft.com/support/v2/?stage=1)iletişim kurun.
