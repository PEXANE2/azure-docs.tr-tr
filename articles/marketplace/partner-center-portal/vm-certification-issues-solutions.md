---
title: Sanal makine sertifikasyonu - sorunlar ve çözümler
description: Bu makalede VM görüntülerinin yaygın hata iletileri açıklanmaktadır. Ayrıca ilgili çözümleri de anlatmaktadır
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: troubleshooting
author: iqshahmicrosoft
ms.author: iqshah
ms.date: 06/16/2020
ms.openlocfilehash: 5b6d1ee41434d8aebac81d38ced9cadd93e51ba8
ms.sourcegitcommit: 3fb5e772f8f4068cc6d91d9cde253065a7f265d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89181451"
---
# <a name="issues-and-solutions-during-virtual-machine-certification"></a>Sanal makine sertifikası sırasında sorunlar ve çözümler 

Sanal makine (VM) görüntünüzü Azure Market 'e yayımladığınızda, Azure ekibi, onun önyükleme, güvenlik ve Azure uyumluluğunu sağlamak için onu doğrular. Yüksek kaliteli testlerin herhangi biri başarısız olursa, yayımlama başarısız olur ve sorunu açıklayan bir hata iletisi alırsınız.

Bu makalede, ilgili çözümlerle birlikte VM görüntüsü yayımlama sırasında sık karşılaşılan hata iletileri açıklanmaktadır.

> [!NOTE]
> Geliştirme için sorularınız veya geri bildiriminiz varsa, [Iş Ortağı Merkezi desteğiyle](https://partner.microsoft.com/support/v2/?stage=1)iletişim kurun.

## <a name="approved-base-image"></a>Onaylanan temel görüntü

Görüntünüzü güncelleştirmelerle yeniden yayımlamak için bir istek gönderdiğinizde, bölüm numarası doğrulama test çalışması başarısız olabilir. Başarısız olursa, görüntünüz onaylanır.

Bu hata, başka bir yayımcıya ait olan ve görüntüyü güncelleştirdiğiniz bir temel görüntü kullandığınızda oluşur. Bu durumda, görüntünüzü yayımlamanıza izin verilmez.

Bu sorunu onarmak için, görüntüyü Azure Marketi 'nden alın ve üzerinde değişiklikler yapın. Daha fazla bilgi için aşağıdaki makaleleri inceleyin:

- [Linux görüntüleri](../../virtual-machines/linux/endorsed-distros.md?toc=/azure/virtual-machines/linux/toc.json)
- [Windows görüntüleri](create-azure-vm-technical-asset.md#create-a-vm-image-using-an-approved-base)

## <a name="vm-extension-failure"></a>VM Uzantısı hatası

Resminizin VM uzantılarını destekleyip desteklemediğini kontrol edin.

VM uzantılarını etkinleştirmek için aşağıdakileri yapın:

1. Linux VM 'nizi seçin.
1. **Tanılama ayarları**' na gidin.
1. **Depolama hesabını**güncelleştirerek taban matrislerini etkinleştirin.
1. **Kaydet**’i seçin.

   ![Konuk düzeyinde izlemeyi etkinleştir](./media/vm-certification-issues-solutions-1.png)

VM uzantılarının düzgün etkinleştirildiğini doğrulamak için aşağıdakileri yapın:

1. VM 'de **VM uzantıları** sekmesini seçin ve ardından **Linux Tanılama uzantısının**durumunu doğrulayın.
    * Durum *sağlama başarılı*olursa, uzantılar test çalışması geçirilir.  
    * Durum *sağlama başarısız*olursa, uzantılar test çalışması başarısız olur ve sağlamlaştırılmış bayrağını ayarlamanız gerekir.

      ![Sağlamanın başarılı olduğunu gösteren ekran görüntüsü](./media/vm-certification-issues-solutions-2.png)

      VM Uzantısı başarısız olursa, etkinleştirmek üzere [ölçümleri ve günlükleri izlemek için bkz. Linux Tanılama uzantısı 'Nı kullanma](../../virtual-machines/extensions/diagnostics-linux.md) . VM uzantısının etkinleştirilmesini istemiyorsanız, destek ekibine başvurun ve devre dışı bırakmasına izin vermesini isteyin.

## <a name="vm-provisioning-issue"></a>VM sağlama sorunu

Teklifinizi göndermeden önce VM sağlama sürecini dikkatli bir şekilde izlediğinizden emin olun. VM 'yi sağlamaya yönelik JSON biçimini görüntülemek için bkz. [Azure sanal makine görüntüsü sertifikası](azure-vm-image-certification.md).

Sağlama sorunları aşağıdaki başarısızlık senaryolarını içerebilir:

|Senaryo|Hata|Neden|Çözüm|
|---|---|---|---|
|1|Geçersiz sanal sabit disk (VHD)|VHD altbilgisinde belirtilen tanımlama bilgisi değeri yanlışsa, VHD geçersiz olarak kabul edilir.|Görüntüyü yeniden oluşturun ve isteği iletin.|
|2|Geçersiz blob türü|Kullanılan blok, bir sayfa türü yerine bir blob türü olduğundan VM sağlama başarısız oldu.|Görüntüyü yeniden oluşturun ve isteği iletin.|
|3|Sağlama zaman aşımı veya düzgün Genelleştirilmiş|VM Genelleştirme ile ilgili bir sorun var.|Yansımayı Genelleştirme ile yeniden oluşturun ve isteği iletin.|

> [!NOTE]
> VM Genelleştirme hakkında daha fazla bilgi için bkz.
> - [Linux belgeleri](create-azure-vm-technical-asset.md#generalize-the-image)
> - [Windows belgeleri](../../virtual-machines/windows/capture-image-resource.md#generalize-the-windows-vm-using-sysprep)

## <a name="software-compliance-for-windows"></a>Windows için yazılım uyumluluğu

Windows görüntü isteğiniz bir yazılım uyumluluğu sorunu nedeniyle reddedilirse, Azure Marketi 'nden ilgili SQL sürümü temel görüntüsünü almak yerine yüklü SQL Server örneğiyle bir Windows görüntüsü oluşturmuş olabilirsiniz.

Üzerinde SQL Server yüklü olan kendi Windows görüntünüzü oluşturmayın. Bunun yerine, Azure Marketi 'nden onaylanan SQL temel görüntülerini (Enterprise/Standard/Web) kullanın.

Visual Studio 'Yu veya Office lisanslı herhangi bir ürünü yüklemeye çalışıyorsanız, önceki onay için destek ekibine başvurun.

Onaylanan bir temel seçme hakkında daha fazla bilgi için bkz. [Azure sanal makine teknik varlıklarınızı oluşturma](create-azure-vm-technical-asset.md#create-a-vm-image-using-an-approved-base).

## <a name="tool-kit-test-case-execution-failed"></a>Araç seti test çalışması yürütülemedi 

Microsoft sertifika araç seti, test çalışmalarını çalıştırmanıza ve VHD veya görüntünüzün Azure ortamıyla uyumlu olduğunu doğrulamanıza yardımcı olabilir.

[Microsoft sertifika araç setini](azure-vm-image-certification.md)indirin.

## <a name="linux-test-cases"></a>Linux test çalışmaları

Aşağıdaki tablo, araç setinin çalışacağı Linux test çalışmalarını listeler. Test doğrulaması, açıklamada belirtilmiştir.

|Senaryo|Test çalışması|Açıklama|
|---|---|---|
|1|Bash geçmişi|VM görüntüsünü oluşturmadan önce bash geçmiş dosyaları temizlenmelidir.|
|2|Linux Aracısı sürümü|Azure Linux Agent 2.2.41 veya üzeri yüklü olmalıdır.|
|3|Gerekli çekirdek parametreleri|Aşağıdaki çekirdek parametrelerinin ayarlandığını doğrular: <br>Konsol = ttyS0<br>earlyprintk = ttyS0<br>rootdelay = 300|
|4|İşletim sistemi diskinde takas bölümü|Takas bölümlerinin işletim sistemi diskinde oluşturulmadığını doğrular.|
|5|İşletim sistemi diskinde kök bölüm|İşletim sistemi diski için tek bir kök bölüm oluşturun.|
|6|OpenSSL sürümü|OpenSSL sürümü v 0.9.8 veya üzeri olmalıdır.|
|7|Python sürümü|Python sürüm 2,6 veya sonraki bir sürümü önemle önerilir.|
|8|İstemci canlı aralığı|ClientAliveInterval değerini 180 olarak ayarlayın. Uygulama gereksinimi üzerine 30 ' dan 235 ' e ayarlanabilir. Son kullanıcılarınız için SSH 'yi etkinleştirirseniz, bu değerin açıklanacak şekilde ayarlanması gerekir.|
|9|İşletim sistemi mimarisi|Yalnızca 64 bit işletim sistemleri desteklenir.|
|10|Otomatik güncelleştirme|Linux Aracısı otomatik güncelleştirme özelliğinin etkinleştirilip etkinleştirilmediğini belirler.|

### <a name="common-errors-found-while-executing-previous-test-cases"></a>Önceki test çalışmaları yürütülürken sık karşılaşılan hatalar bulundu

Aşağıdaki tabloda, önceki test çalışmaları yürütülürken bulunan yaygın hatalar listelenmektedir:
 
|Senaryo|Test çalışması|Hata|Çözüm|
|---|---|---|---|
|1|Linux Aracısı sürüm test çalışması|En düşük Linux Aracısı sürümü 2.2.41 veya üzeri. Bu gereksinim 1 Mayıs 2020 ' den itibaren zorunludur.|Lütfen Linux Aracısı sürümünü güncelleştirin ve 2,241 veya üzeri olmalıdır. Daha fazla bilgi için [Linux Aracısı sürüm güncelleştirme sayfasını](https://support.microsoft.com/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support)ziyaret edebilirsiniz.|
|2|Bash geçmiş test çalışması|Gönderilen görüntinizdeki Bash geçmişinin boyutu 1 kilobayt (KB) daha büyükse bir hata görürsünüz. Potansiyel olarak hassas bilgilerin Bash geçmiş dosyanızda yakalandığından emin olmak için Boyut 1 KB 'a kısıtlanır.|Bu sorunu çözmek için, VHD 'yi başka bir çalışan VM 'ye bağlayın ve istediğiniz değişiklikleri yapın (örneğin, *. bash* geçmiş dosyalarını silin), boyutu 1 KB 'tan küçük veya buna eşit olacak şekilde küçültün.|
|3|Gerekli çekirdek parametresi test çalışması|**Konsolun** değeri **ttyS0**olarak ayarlanmadıysa bu hatayı alırsınız. Aşağıdaki komutu çalıştırarak kontrol edin:<br>`cat /proc/cmdline`|**Konsolun** değerini **ttyS0**olarak ayarlayın ve isteği yeniden gönderin.|
|4|ClientAlive Interval test çalışması|Araç seti sonucu, bu test çalışması için başarısız bir sonuç veriyorsa **ClientAliveInterval**için uygun olmayan bir değer vardır.|**ClientAliveInterval** değerini 235 ' e eşit veya daha küçük olarak ayarlayın ve sonra isteği yeniden gönderin.|

### <a name="windows-test-cases"></a>Windows test çalışmaları

Aşağıdaki tabloda, test doğrulamasının bir açıklamasıyla birlikte, araç setinin çalışacağı Windows test çalışmaları listelenmektedir:

|Senaryo |Test çalışmaları|Açıklama|
|---|---|---|---|
|1|İşletim sistemi mimarisi|Azure yalnızca 64 bitlik işletim sistemlerini destekler.|
|2|Kullanıcı hesabı bağımlılığı|Uygulama yürütme, yönetici hesabına bağlı olmamalıdır.|
|3|Yük devri kümesi|Windows Server Yük Devretme Kümelemesi özelliği henüz desteklenmiyor. Uygulamanın bu özelliğe bağımlı olmaması gerekir.|
|4|IPV6|IPv6 henüz Azure ortamında desteklenmiyor. Uygulamanın bu özelliğe bağımlı olmaması gerekir.|
|5|DHCP|Dinamik ana bilgisayar Yapılandırma Protokolü sunucu rolü henüz desteklenmiyor. Uygulamanın bu özelliğe bağımlı olmaması gerekir.|
|6|Hyper-V|Hyper-V sunucu rolü henüz desteklenmiyor. Uygulamanın bu özelliğe bağımlı olmaması gerekir.|
|7|Uzaktan erişim|Uzaktan erişim (doğrudan erişim) sunucu rolü henüz desteklenmiyor. Uygulamanın bu özelliğe bağımlı olmaması gerekir.|
|8|Rights Management Hizmetleri|Rights Management Hizmetleri. Sunucu rolü henüz desteklenmiyor. Uygulamanın bu özelliğe bağımlı olmaması gerekir.|
|9|Windows Dağıtım Hizmetleri|Windows Dağıtım Hizmetleri. Sunucu rolü henüz desteklenmiyor. Uygulamanın bu özelliğe bağımlı olmaması gerekir.|
|10|BitLocker Sürücü Şifrelemesi|BitLocker Sürücü Şifrelemesi, işletim sistemi sabit diskinde desteklenmez, ancak veri disklerinde kullanılıyor olabilir.|
|11|Internet Depolama adı sunucusu|Internet Depolama adı sunucusu özelliği henüz desteklenmiyor. Uygulamanın bu özelliğe bağımlı olmaması gerekir.|
|12|Çok Yollu G/Ç|Çok yollu g/ç. Bu sunucu özelliği henüz desteklenmiyor. Uygulamanın bu özelliğe bağımlı olmaması gerekir.|
|13|Ağ Yükü Dengeleme|Ağ Yükü Dengeleme. Bu sunucu özelliği henüz desteklenmiyor. Uygulamanın bu özelliğe bağımlı olmaması gerekir.|
|14|Eş Adı Çözümleme Protokolü|Eş adı çözümleme protokolü. Bu sunucu özelliği henüz desteklenmiyor. Uygulamanın bu özelliğe bağımlı olmaması gerekir.|
|15|SNMP Hizmetleri|Basit Ağ Yönetim Protokolü (SNMP) Hizmetleri özelliği henüz desteklenmiyor. Uygulamanın bu özelliğe bağımlı olmaması gerekir.|
|16|Windows Internet ad hizmeti|Windows Internet ad hizmeti. Bu sunucu özelliği henüz desteklenmiyor. Uygulamanın bu özelliğe bağımlı olmaması gerekir.|
|17|Kablosuz LAN Hizmeti|Kablosuz LAN Hizmeti. Bu sunucu özelliği henüz desteklenmiyor. Uygulamanın bu özelliğe bağımlı olmaması gerekir.|

Önceki test durumlarında tüm hatalarda karşılaşırsanız, çözüm için tablodaki **Açıklama** sütununa bakın. Daha fazla bilgiye ihtiyacınız varsa destek ekibine başvurun. 

## <a name="data-disk-size-verification"></a>Veri diski boyut doğrulaması

Veri diski ile gönderilen herhangi bir isteğin boyutu 1023 gigabayttan (GB) büyükse, istek onaylanmayacaktır. Bu kural hem Linux hem de Windows için geçerlidir.

İsteği 1023 GB 'tan küçük veya buna eşit bir boyutla yeniden gönderin.

## <a name="os-disk-size-validation"></a>İşletim sistemi disk boyutu doğrulaması

İşletim sistemi disk boyutuyla ilgili sınırlamalar için aşağıdaki kurallara bakın. Herhangi bir istek gönderdiğinizde, işletim sistemi disk boyutunun Linux veya Windows için kısıtlama dahilinde olduğunu doğrulayın.

|İşletim Sistemi|Önerilen VHD boyutu|
|---|---|
|Linux|30 GB ila 1023 GB|
|Windows|30 GB ila 250 GB|

VM 'Ler temeldeki işletim sistemine erişime izin verse de VHD boyutunun VHD için yeterince büyük olduğundan emin olun. Diskler kesinti olmadan Genişletilebilir olmadığından, 30 GB ile 50 GB arasında bir disk boyutu kullanın.

|VHD boyutu|Dolu gerçek boyut|Çözüm|
|---|---|---|
|>500 tebibayt (Tib)|yok|Özel durum onayı için destek ekibine başvurun.|
|250-500 TiB|Blob boyutundan >200 Gibibyte (gib) farkı|Özel durum onayı için destek ekibine başvurun.|

> [!NOTE]
> Daha büyük disk boyutları daha yüksek maliyetler doğurur ve kurulum ve çoğaltma işlemi sırasında bir gecikme olur. Bu gecikme ve maliyet nedeniyle, destek ekibi özel durum onayı için gerekçe arayabilir.

## <a name="wannacry-patch-verification-test-for-windows"></a>Windows için WannaCry Patch doğrulama testi

WannaCry virüsü ile ilgili olası bir saldırıyı engellemek için tüm Windows yansıma isteklerinin en son düzeltme ekiyle güncelleştirildiğinden emin olun.

İşletim sistemi ayrıntısı için Windows Server düzeltme eki uygulanan sürümü ve destekleyeceği en düşük sürümü denetlemek için aşağıdaki tabloya bakın: 

Görüntü dosyası sürümü veya ' den doğrulanabilir `C:\windows\system32\drivers\srv.sys` `srv2.sys` .

> [!NOTE]
> Windows Server 2019 zorunlu sürüm gereksinimlerine sahip değildir.

|İşletim Sistemi|Sürüm|
|---|---|
|Windows hizmeti 2008 R2|6.1.7601.23689|
|Windows Server 2012|6.2.9200.22099|
|Windows Server 2012 R2|6.3.9600.18604|
|Windows Server 2016|10.0.14393.953|
|Windows Server 2019|NA|

## <a name="sack-vulnerability-patch-verification"></a>SACK güvenlik düzeltme eki doğrulaması

Bir Linux görüntüsü gönderdiğinizde, isteğiniz çekirdek sürümü sorunları nedeniyle reddedilebilir.

Çekirdeği onaylanan bir sürümle güncelleştirin ve isteği yeniden gönderin. Onaylanan çekirdek sürümünü aşağıdaki tabloda bulabilirsiniz. Sürüm numarası, burada listelenen sayıya eşit veya ondan büyük olmalıdır.

Görüntünüz aşağıdaki çekirdek sürümlerinden biriyle yüklenmezse, doğru düzeltme ekleriyle güncelleştirin. Görüntü, gereken bu düzeltme ekleriyle güncelleştirildikten sonra destek ekibinin gerekli onayını iste:

- CVE-2019-11477 
- CVE-2019-11478 
- CVE-2019-11479

|İşletim sistemi ailesi|Sürüm|Çekirdek|
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

Azure 'daki tüm VHD 'Lerin 1 megabayt (MB) katları ile hizalı bir sanal boyutu olmalıdır. VHD 'niz önerilen sanal boyuta bağlı değilse isteğiniz reddedilebilir.

Ham diskten VHD 'ye dönüştürme yaparken yönergeleri izleyin ve ham disk boyutunun 1 MB 'ın katı olduğundan emin olun. Daha fazla bilgi için bkz. [onaylamayan dağıtımlar Için bilgi](../../virtual-machines/linux/create-upload-generic.md).

## <a name="vm-access-denied"></a>VM erişimi reddedildi

VM 'deki test çalışmalarını çalıştırırken erişim reddedildi sorunlarından geliyorsa, bu durum test çalışmalarını çalıştırmak için yeterli ayrıcalıklara sahip olabilir.

Kendi kendine test çalışmalarının çalıştığı hesap için doğru erişimin etkinleştirilip etkinleştirilmediğini denetleyin. Erişim etkinleştirilmemişse, test çalışmalarını çalıştırmak için etkinleştirin. Erişimi etkinleştirmek istemiyorsanız, kendi test çalışması sonuçlarını destek ekibi ile paylaşabilirsiniz.

## <a name="download-failure"></a>İndirme hatası
    
Paylaşılan erişim imzası (SAS) URL 'SI kullanarak VM görüntüsünü indirdiğinizde ortaya çıkan herhangi bir sorun için aşağıdaki tabloya bakın.

|Senaryo|Hata|Neden|Çözüm|
|---|---|---|---|
|1|Blob bulunamadı|VHD silinmiş ya da belirtilen konumdan taşınmış olabilir.|| 
|2|Kullanılan blob|VHD başka bir iç işlem tarafından kullanılıyor.|Bir SAS URL 'SI kullanarak karşıdan yüklerken VHD 'nin kullanılan bir durumda olması gerekir.|
|3|Geçersiz SAS URL 'SI|VHD için ilişkili SAS URL 'SI yanlış.|Doğru SAS URL 'sini alın.|
|4|Geçersiz imza|VHD için ilişkili SAS URL 'SI yanlış.|Doğru SAS URL 'sini alın.|
|6|HTTP koşullu üstbilgisi|SAS URL 'SI geçersiz.|Doğru SAS URL 'sini alın.|
|7|Geçersiz VHD adı|Yüzde işareti (%) gibi özel karakterler olup olmadığını denetleyin veya tırnak işaretleri ("), VHD adında mevcuttur.|Özel karakterleri kaldırarak VHD dosyasını yeniden adlandırın.|

## <a name="first-1-mb-partition"></a>İlk 1 MB bölüm

VHD 'yi gönderdiğinizde, VHD 'nin ilk 1 MB 'lık bölümünün boş olduğundan emin olun. Aksi takdirde, isteğiniz başarısız olur.

## <a name="default-credentials"></a>Varsayılan kimlik bilgileri

Varsayılan kimlik bilgilerinin gönderilen VHD ile gönderilmediğinden her zaman emin olun. Varsayılan kimlik bilgilerini ekleme, VHD 'nin güvenlik tehditlerine karşı daha savunmasız olmasını sağlar. Bunun yerine, VHD 'YI gönderdiğinizde kendi kimlik bilgilerinizi oluşturun.
  
## <a name="datadisk-mapped-incorrectly"></a>Veri diski yanlış eşlendi

Bir istek birden çok veri diski ile gönderildiğinde, ancak sırası sıralı değilse, bu bir eşleme sorunu olarak kabul edilir. Örneğin, üç veri diski varsa, numaralandırma sırası *0, 1, 2*olmalıdır. Diğer herhangi bir sıra, eşleme sorunu olarak kabul edilir.

İsteği veri disklerinin doğru sıralaması ile yeniden gönderin.

## <a name="incorrect-os-mapping"></a>Yanlış işletim sistemi eşlemesi

Bir görüntü oluşturulduğunda, bu, yanlış işletim sistemi etiketine eşlenmiş veya atanmış olabilir. Örneğin, görüntüyü oluştururken işletim sistemi adının bir parçası olarak **Windows** ' u seçtiğinizde, işletim sistemi diski yalnızca Windows ile yüklenmelidir. Aynı gereksinim Linux için de geçerlidir.

## <a name="vm-not-generalized"></a>VM Genelleştirilmiş değil

Azure Marketi 'nden alınan tüm görüntülerin yeniden kullanılabilmesi için işletim sistemi VHD 'sinin genelleştirilmesi gerekir.

* **Linux**için aşağıdaki Işlem BIR Linux sanal makinesini genelleştirir ve ayrı bir VM olarak yeniden dağıtır.

  SSH penceresinde, aşağıdaki komutu girin: `sudo waagent -deprovision+user`

* **Windows**için, kullanarak Windows görüntülerini genelleştirdiğinizde `sysreptool` .

Bu araç hakkında daha fazla bilgi için bkz. [Sistem Hazırlama (Sysprep) genel bakış]( https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview).

## <a name="datadisk-errors"></a>Veri diski hataları

Veri diskiyle ilgili hatalara yönelik çözümler için aşağıdaki tabloyu kullanın:

|Hata|Neden|Çözüm|
|---|---|---|
|`DataDisk- InvalidUrl:`|Bu hata, teklif gönderildiğinde mantıksal birim numarası (LUN) için belirtilen geçersiz bir sayı nedeniyle meydana gelebilir.|Veri diski için LUN numarası sırasının Iş Ortağı Merkezi 'nde olduğunu doğrulayın.|
|`DataDisk- NotFound:`|Bu hata, belirtilen bir SAS URL 'sinde bulunmayan bir veri diski nedeniyle meydana gelebilir.|Veri diskinin istekte belirtilen SAS URL 'sinde bulunduğundan emin olun.|

## <a name="remote-access-issue"></a>Uzaktan erişim sorunu

Windows görüntüsü için Uzak Masaüstü Protokolü (RDP) seçeneği etkinleştirilmemişse, bu hatayı alırsınız. 

Göndermeden önce Windows görüntüleri için RDP erişimini etkinleştirin.

## <a name="bash-history-failed"></a>Bash geçmişi başarısız oldu

Gönderilen görüntinizdeki Bash geçmişinin boyutu 1 kilobayt (KB) daha büyükse bu hatayı görürsünüz. Potansiyel olarak hassas bilgilerin Bash geçmiş dosyanızda yakalandığından emin olmak için Boyut 1 KB 'a kısıtlanır.

Aşağıda "Bash History" öğesini silme adımları verilmiştir.

Adım 1. VM 'yi dağıtın ve Azure portal üzerinde "komut Çalıştır" seçeneğine tıklayın.
![Azure portal komutu Çalıştır](./media/vm-certification-issues-solutions-3.png)

Adım 2. İlk "Runshellscrıpt" seçeneğini belirleyin ve aşağıdaki komutu çalıştırın.

Komut: "Cat/dev/null > ~/. bash_history && History-c" ![ Azure Portal üzerinde Bash History komutu](./media/vm-certification-issues-solutions-4.png)

3. Adım Komutu başarıyla yürüttükten sonra, sanal makineyi yeniden başlatın.

4. Adım: VM 'yi genelleştirin, görüntü VHD 'sini alın ve VM 'yi durdurun.

5. Adım.     Genelleştirilmiş görüntüyü yeniden gönderebilirsiniz.

## <a name="requesting-exceptions-custom-templates-on-vm-images-for-selective-tests"></a>Seçmeli testler için VM görüntülerinde özel durumlar (özel şablonlar) isteme

Yayımcılar, VM sertifikası sırasında gerçekleştirilen birkaç test için özel durumlar istemek üzere kullanılabilir. Yayımcı isteği desteklemek için kanıt sunmakta olduğu için çok nadir durumlarda özel durumlar sağlanır.
Sertifika ekibi, her zaman özel durumları reddetme veya onaylama hakkını saklı tutar.

Aşağıdaki bölümlerde, özel durumların istendiği ana senaryolar ve özel durum isteme hakkında konuşacağız.

Özel durum senaryoları

Yayımcıların bu özel durumları genellikle istemesi durumunda üç senaryo/durum vardır. 

* **Bir veya daha fazla test çalışması Için özel durum:** Yayımcılar, test çalışmaları için [Market yayımcısı destek](https://aka.ms/marketplacepublishersupport) isteği özel durumlarına ulaşabilir. 

* **Kilitli VM 'ler/kök erişimi yok:** VM 'lerde yüklü olan güvenlik duvarları gibi yazılımlar olduğundan, birkaç Yayımcı, sanal makinelerin kilitlenebileceği senaryolar vardır. 
       Bu durumda, yayımcılar [sertifikalı test aracını](https://aka.ms/AzureCertificationTestTool) buradan indirebilir ve raporu [Market yayımcı desteği](https://aka.ms/marketplacepublishersupport) ' nde sağlayabilir


* **Özel şablonlar:** Bazı yayımcılar VM 'Leri dağıtmak için özel bir ARM şablonu gerektiren VM görüntülerini yayımlar. Bu durumda, yayımcıların sertifika ekibi tarafından doğrulama için kullanılabilmesi için [Market yayımcı desteği](https://aka.ms/marketplacepublishersupport) ' nde özel şablonlar sağlaması istenir. 

### <a name="information-to-provide-for-exception-scenarios"></a>Özel durum senaryoları için sağlanacak bilgiler

Yayımcılar, yukarıdaki senaryo için aşağıdaki ek bilgilerle özel durumlar istemek üzere [Market yayımcısındaki](https://aka.ms/marketplacepublishersupport) desteğe ulaşmalıdır:

   1.   Yayımcı KIMLIĞI – Iş Ortağı Merkezi portalındaki yayımcı KIMLIĞI
   2.   Teklif KIMLIĞI/adı – özel durumun istendiği teklif KIMLIĞI/adı 
   3.   SKU/plan KIMLIĞI – özel durumun istendiği sanal makine teklifinin plan KIMLIĞI/SKU 'su
   4.    Sürüm: özel durum istenen VM teklifinin sürümü
   5.   Özel durum türü – testler, kilitli VM, özel şablonlar
   6.   İsteğin nedeni-bu özel durumun nedeni ve dışarıda bırakılan testler hakkında bilgi 
   7. Zaman çizelgesi-bu özel durumun istendiği Tarih 
   8.   Ek-tüm önem bulgu belgelerini ekleyin. Kilitli VM 'Ler için, test raporunu ve özel şablonlar için ek olarak özel ARM şablonunu sağlayın. Kilitli VM 'Ler için rapor iliştirilemedi ve özel şablonlar için özel ARM şablonu, istek reddine neden olacak


## <a name="next-steps"></a>Sonraki adımlar

Geliştirme için sorularınız veya geri bildiriminiz varsa, [Iş Ortağı Merkezi desteğiyle](https://partner.microsoft.com/support/v2/?stage=1)iletişim kurun.
