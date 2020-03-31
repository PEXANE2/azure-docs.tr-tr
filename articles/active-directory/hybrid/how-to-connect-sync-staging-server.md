---
title: 'Azure AD Connect eşitlemi: Operasyonel görevler ve dikkat edilecek hususlar | Microsoft Dokümanlar'
description: Bu konu, Azure AD Connect eşitlemi için operasyonel görevleri ve bu bileşeni çalıştırmaya nasıl hazırlanacağını açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: b29c1790-37a3-470f-ab69-3cee824d220d
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/27/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: bc88640cdff4f716902a80bb149913b961d40ae3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79261027"
---
# <a name="azure-ad-connect-staging-server-and-disaster-recovery"></a>Azure AD Connect: Sunucu hazırlama ve olağanüstü durum kurtarma
Evreleme modundaki bir sunucuyla, sunucuyu etkin hale getirmeden önce yapılandırmada değişiklikler yapabilir ve değişiklikleri önizleyebilirsiniz. Ayrıca, bu değişiklikleri üretim ortamınıza yapmadan önce tüm değişikliklerin beklendiğini doğrulamak için tam alma ve tam eşitleme çalıştırmanızı sağlar.

## <a name="staging-mode"></a>Hazırlama modu
Hazırlama modu, şunlardır:

* Yüksek kullanılabilirlik.
* Yeni yapılandırma değişikliklerini test edin ve dağıtın.
* Yeni bir sunucu tanıtın ve eskisini devre dışı kaldırın.

Yükleme sırasında, **sunucuyu evreleme modunda**olacak şekilde seçebilirsiniz. Bu eylem, sunucu alma ve eşitleme için etkin hale getirir, ancak herhangi bir dışa aktarım çalışmaz. Yükleme sırasında bu özellikleri seçmiş olsanız bile, hazırlama modundaki bir sunucu parola eşitleme veya parola yazma işlemi çalıştırmıyor. Hazırlama modunu devre dışı aldığınızda, sunucu dışa aktarmaya başlar, parola eşitlemesini sağlar ve parola yazmayı etkinleştirer.

> [!NOTE]
> Parola Hash Senkronizasyonu özelliği etkinleştirilmiş bir Azure AD Bağlantısı nız olduğunu varsayalım. Hazırlama modunu etkinleştirdiğinizde, sunucu şirket içi AD'den parola değişikliklerini eşitlemeyi durdurur. Hazırlama modunu devre dışı bıraktığınızda, sunucu parola değişikliklerini en son kaldığı yerden eşitlemeye devam eder. Sunucu uzun bir süre için hazırlama modunda bırakılırsa, sunucunun zaman dilimi içinde gerçekleşen tüm parola değişikliklerini eşitleması biraz zaman alabilir.
>
>

Eşitleme hizmeti yöneticisini kullanarak bir dışa aktarmayı zorlamaya devam edebilirsiniz.

Hazırlama modundaki bir sunucu Active Directory ve Azure AD'den değişiklik almaya devam ediyor ve bir hata durumunda başka bir sunucunun sorumluluklarını hızla devralabilir. Birincil sunucunuzda yapılandırma değişiklikleri yaparsanız, aynı değişiklikleri evreleme modunda sunucuda yapmak sizin sorumluluğunuzdadır.

Eski eşitleme teknolojileri bilgisine sahip olanlar için, sunucunun kendi SQL veritabanı na sahip olduğu için hazırlama modu farklıdır. Bu mimari, hazırlama modu sunucusunun farklı bir veri merkezinde bulunmasını sağlar.

### <a name="verify-the-configuration-of-a-server"></a>Sunucunun yapılandırmasını doğrulama
Bu yöntemi uygulamak için aşağıdaki adımları izleyin:

1. [Hazırlama](#prepare)
2. [Yapılandırma](#configuration)
3. [Alma ve Senkronize Etme](#import-and-synchronize)
4. [Doğrula](#verify)
5. [Etkin sunucuya geçiş](#switch-active-server)

#### <a name="prepare"></a>Hazırlama
1. Azure AD Connect'i yükleyin, **hazırlama modunu**seçin ve yükleme sihirbazındaki son sayfada **seçme başlat eşitlemesi** kaldırın. Bu mod eşitleme motorunu el ile çalıştırmanızı sağlar.
   ![ReadyToConfigure](./media/how-to-connect-sync-staging-server/readytoconfigure.png)
2. Oturum kapatma/oturum açma ve başlat menüsünden **Senkronizasyon Hizmeti'ni**seçin.

#### <a name="configuration"></a>Yapılandırma
Birincil sunucuda özel değişiklikler yaptıysanız ve yapılandırmayı hazırlama sunucusuyla karşılaştırmak istiyorsanız, [Azure AD Connect yapılandırma belgeleyicisini](https://github.com/Microsoft/AADConnectConfigDocumenter)kullanın.

#### <a name="import-and-synchronize"></a>Alma ve Senkronize Etme
1. **Bağlayıcıları**seçin ve **Active Directory Domain Services**türüne sahip ilk Bağlayıcıyı seçin. **Çalıştır'ı**tıklatın, **Tam içe aktar'ı**seçin ve **Tamam'** ı seçin. Bu türdeki tüm Bağlayıcılar için bu adımları yapın.
2. Azure Etkin Dizin türüne **(Microsoft)** sahip Bağlayıcıyı seçin. **Çalıştır'ı**tıklatın, **Tam içe aktar'ı**seçin ve **Tamam'** ı seçin.
3. Sekme Bağlayıcıları'nın hala seçildiğinden emin olun. Active **Directory Etki Alanı Hizmetleri**türüne sahip her Bağlayıcı için **Çalıştır'ı**tıklatın, **Delta Senkronizasyon'u**seçin ve **Tamam'** ı seçin.
4. Azure Etkin Dizin türüne **(Microsoft)** sahip Bağlayıcıyı seçin. **Çalıştır'ı**tıklatın, **Delta Eşitlemesi'ni**seçin ve **Tamam'** ı seçin.

Artık Azure AD ve şirket içi AD'de dışa aktarma değişiklikleri düzenlediniz (Exchange karma dağıtımı kullanıyorsanız). Sonraki adımlar, dizinlere gerçekten dışa aktarmaya başlamadan önce nelerin değişmek üzere olduğunu incelemenize olanak sağlar.

#### <a name="verify"></a>Doğrulama
1. Cmd istemi başlatın ve`%ProgramFiles%\Microsoft Azure AD Sync\bin`
2. Çalıştır: `csexport "Name of Connector" %temp%\export.xml /f:x` Bağlayıcının adı Eşitleme Hizmeti'nde bulunabilir. Azure AD için "contoso.com – AAD" adlı bir adı vardır.
3. Çalıştır: `CSExportAnalyzer %temp%\export.xml > %temp%\export.csv` Microsoft Excel'de incelenebilen export.csv adlı %geçici %'lik bir dosyanız var. Bu dosya, dışa aktarılmak üzere olan tüm değişiklikleri içerir.
4. Veri veya yapılandırmada gerekli değişiklikleri yapın ve dışa aktarılmak üzere olan değişiklikler beklenene kadar bu adımları (İçe aktar ve Eşitle ve Doğrula) yeniden çalıştırın.

**export.csv dosyasını anlama** Dosyanın çoğu kendini açıklayıcı. İçeriği anlamak için bazı kısaltmalar:
* OMODT – Nesne Modifikasyon Türü. Nesne düzeyindeki işlemin Ekle, Güncelleştir veya Sil olup olmadığını gösterir.
* AMODT – Öznitelik Modifikasyon Türü. Öznitelik düzeyindeki işlemin Ekle, Güncelleştir veya silme işlemi olup olmadığını gösterir.

**Ortak tanımlayıcıları alın** Export.csv dosyası, dışa aktarmak üzere olan tüm değişiklikleri içerir. Her satır bağlayıcı uzasındaki bir nesnenin değişikliğine karşılık gelir ve nesne DN özniteliği ile tanımlanır. DN özniteliği bağlayıcı alanında bir nesneye atanan benzersiz bir tanımlayıcıdır. Çözümlemek için export.csv'de çok sayıda satır/değişiklik olduğunda, değişikliklerin yalnızca DN özniteliğine göre hangi nesnelerin için olduğunu anlamanız zor olabilir. Değişiklikleri çözümleme işlemini kolaylaştırmak için csanalyzer.ps1 PowerShell komut dosyasını kullanın. Komut dosyası, nesnelerin ortak tanımlayıcılarını (örneğin, displayName, userPrincipalName) alır. Komut dosyasını kullanmak için:
1. PowerShell komut dosyasını [CSAnalyzer](#appendix-csanalyzer) bölümünden `csanalyzer.ps1`adlı bir dosyaya kopyalayın.
2. PowerShell penceresini açın ve PowerShell komut dosyasını oluşturduğunuz klasöre göz atın.
3. Şunu çalıştırın: `.\csanalyzer.ps1 -xmltoimport %temp%\export.xml`.
4. Şimdi Microsoft Excel'de incelenebilir **işlemusers1.csv** adlı bir dosya var. Dosyanın DN özniteliğinden ortak tanımlayıcılara (örneğin, displayName ve userPrincipalName) eşleme sağladığını unutmayın. Şu anda dışa aktarılmak üzere olan gerçek öznitelik değişikliklerini içermez.

#### <a name="switch-active-server"></a>Etkin sunucuya geçiş
1. Şu anda etkin olan sunucuda, sunucuyu (DirSync/FIM/Azure AD Sync) kapatın, böylece Azure AD'ye dışa aktarmaz veya hazırlama modunda ayarlayın (Azure AD Connect).
2. Yükleme sihirbazını **sunucuda hazırlama modunda** çalıştırın ve hazırlama **modunu**devre dışı kaldırın.
   ![ReadyToConfigure](./media/how-to-connect-sync-staging-server/additionaltasks.png)

## <a name="disaster-recovery"></a>Olağanüstü durum kurtarma
Uygulama tasarımının bir parçası, eşitleme sunucusunu kaybetmeniz durumunda ne yapacağınızı planlamaktır. Kullanılacak farklı modeller vardır ve hangisini kullanacağız aşağıdakiler dahil olmak üzere çeşitli etkenlere bağlıdır:

* Kapalı kalma süresi nde Azure AD'deki nesnelerde değişiklik yapamazken toleransınız nedir?
* Parola eşitleme kullanıyorsanız, kullanıcılar eski parolayı şirket içinde değiştirmeleri durumunda Azure AD'de kullanmak zorunda olduklarını kabul ederler mi?
* Parola yazma gibi gerçek zamanlı işlemlere bağımlılığınız var mı?

Bu soruların yanıtlarına ve kuruluşunuzun ilkesine bağlı olarak aşağıdaki stratejilerden biri uygulanabilir:

* Gerektiğinde yeniden inşa edin.
* **Evreleme modu**olarak bilinen yedek bekleme sunucusuna sahip.
* Sanal makineleri kullanın.

Yerleşik SQL Express veritabanını kullanmıyorsanız, [SQL High Availability](#sql-high-availability) bölümünü de gözden geçirmeniz gerekir.

### <a name="rebuild-when-needed"></a>Gerektiğinde yeniden oluşturma
Uygulanabilir bir strateji gerektiğinde bir sunucu yeniden planlamaktır. Genellikle, eşitleme motoru yükleme ve ilk alma ve eşitleme yapmak birkaç saat içinde tamamlanabilir. Boş bir sunucu yoksa, eşitleme altyapısını barındırmak için geçici olarak bir etki alanı denetleyicisi kullanmak mümkündür.

Eşitleme motoru sunucusu nesneler le ilgili herhangi bir durum depolamaz, böylece veritabanı Active Directory ve Azure AD'deki verilerden yeniden oluşturulabilir. **SourceAnchor** özniteliği, şirket içi ve buluttaki nesneleri birleştirmek için kullanılır. Sunucuyu şirket içinde varolan nesnelerle ve bulutla yeniden yeniden bir araya getirirseniz, eşitleme altyapısı yeniden yüklemede bu nesnelerle tekrar eşleşir. Belgelemeniz ve kaydetmeniz gereken şeyler, sunucuda yapılan filtreleme ve eşitleme kuralları gibi yapılandırma değişiklikleridir. Eşitlemeye başlamadan önce bu özel yapılandırmaların yeniden uygulanması gerekir.

### <a name="have-a-spare-standby-server---staging-mode"></a>Yedek bekleme sunucusuna sahip - evreleme modu
Daha karmaşık bir ortamınız varsa, bir veya daha fazla bekleme sunucusuna sahip olması önerilir. Yükleme sırasında, bir sunucunun **evreleme modunda**olmasını etkinleştirebilirsiniz.

Daha fazla bilgi için [hazırlama moduna](#staging-mode)bakın.

### <a name="use-virtual-machines"></a>Sanal makineleri kullanma
Yaygın ve desteklenen yöntem, eşitleme motorini sanal bir makinede çalıştırmaktır. Ana bilgisayarda bir sorun olması durumunda, eşitleme motoru sunucusuna sahip görüntü başka bir sunucuya geçirilebilir.

### <a name="sql-high-availability"></a>SQL Yüksek Kullanılabilirlik
Azure AD Connect ile birlikte gelen SQL Server Express'i kullanmıyorsanız, SQL Server için yüksek kullanılabilirlik de göz önünde bulundurulmalıdır. Desteklenen yüksek kullanılabilirlik çözümleri arasında SQL kümeleme ve AOA (Her Zaman Kullanılabilirlik Grupları) yer almaktadır. Desteklenmeyen çözümler yansıtma içerir.

SQL AOA desteği, 1.1.524.0 sürümünde Azure AD Connect'e eklendi. Azure AD Connect'i yüklemeden önce SQL AOA'yı etkinleştirmelisiniz. Yükleme sırasında Azure AD Connect, sağlanan SQL örneğinin SQL AOA için etkin olup olmadığını algılar. SQL AOA etkinse, Azure AD Connect, SQL AOA'nın senkron çoğaltma veya eşzamanlı çoğaltma kullanacak şekilde yapılandırıp yapılandırılmadığını daha iyi anlayabilir. Kullanılabilirlik Grubu Dinleyicisi'ni ayarlarken RegisterAllProvidersIP özelliğini 0 olarak ayarlamanız önerilir. Bunun nedeni, Azure AD Connect'in sql yerel istemciye bağlanmak için şu anda SQL Native Client'ı kullanması ve SQL Native Client'ın MultiSubNetFailover özelliğinin kullanımını desteklememesidir.

## <a name="appendix-csanalyzer"></a>Ek CSAnalyzer
Bu komut dosyasının nasıl kullanılacağıyla ilgili [bölümüne](#verify) bakın.

```
Param(
    [Parameter(Mandatory=$true, HelpMessage="Must be a file generated using csexport 'Name of Connector' export.xml /f:x)")]
    [string]$xmltoimport="%temp%\exportedStage1a.xml",
    [Parameter(Mandatory=$false, HelpMessage="Maximum number of users per output file")][int]$batchsize=1000,
    [Parameter(Mandatory=$false, HelpMessage="Show console output")][bool]$showOutput=$false
)

#LINQ isn't loaded automatically, so force it
[Reflection.Assembly]::Load("System.Xml.Linq, Version=3.5.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089") | Out-Null

[int]$count=1
[int]$outputfilecount=1
[array]$objOutputUsers=@()

#XML must be generated using "csexport "Name of Connector" export.xml /f:x"
write-host "Importing XML" -ForegroundColor Yellow

#XmlReader.Create won't properly resolve the file location,
#so expand and then resolve it
$resolvedXMLtoimport=Resolve-Path -Path ([Environment]::ExpandEnvironmentVariables($xmltoimport))

#use an XmlReader to deal with even large files
$result=$reader = [System.Xml.XmlReader]::Create($resolvedXMLtoimport) 
$result=$reader.ReadToDescendant('cs-object')
do 
{
    #create the object placeholder
    #adding them up here means we can enforce consistency
    $objOutputUser=New-Object psobject
    Add-Member -InputObject $objOutputUser -MemberType NoteProperty -Name ID -Value ""
    Add-Member -InputObject $objOutputUser -MemberType NoteProperty -Name Type -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name DN -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name operation -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name UPN -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name displayName -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name sourceAnchor -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name alias -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name primarySMTP -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name onPremisesSamAccountName -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name mail -Value ""

    $user = [System.Xml.Linq.XElement]::ReadFrom($reader)
    if ($showOutput) {Write-Host Found an exported object... -ForegroundColor Green}

    #object id
    $outID=$user.Attribute('id').Value
    if ($showOutput) {Write-Host ID: $outID}
    $objOutputUser.ID=$outID

    #object type
    $outType=$user.Attribute('object-type').Value
    if ($showOutput) {Write-Host Type: $outType}
    $objOutputUser.Type=$outType

    #dn
    $outDN= $user.Element('unapplied-export').Element('delta').Attribute('dn').Value
    if ($showOutput) {Write-Host DN: $outDN}
    $objOutputUser.DN=$outDN

    #operation
    $outOperation= $user.Element('unapplied-export').Element('delta').Attribute('operation').Value
    if ($showOutput) {Write-Host Operation: $outOperation}
    $objOutputUser.operation=$outOperation

    #now that we have the basics, go get the details

    foreach ($attr in $user.Element('unapplied-export-hologram').Element('entry').Elements("attr"))
    {
        $attrvalue=$attr.Attribute('name').Value
        $internalvalue= $attr.Element('value').Value

        switch ($attrvalue)
        {
            "userPrincipalName"
            {
                if ($showOutput) {Write-Host UPN: $internalvalue}
                $objOutputUser.UPN=$internalvalue
            }
            "displayName"
            {
                if ($showOutput) {Write-Host displayName: $internalvalue}
                $objOutputUser.displayName=$internalvalue
            }
            "sourceAnchor"
            {
                if ($showOutput) {Write-Host sourceAnchor: $internalvalue}
                $objOutputUser.sourceAnchor=$internalvalue
            }
            "alias"
            {
                if ($showOutput) {Write-Host alias: $internalvalue}
                $objOutputUser.alias=$internalvalue
            }
            "proxyAddresses"
            {
                if ($showOutput) {Write-Host primarySMTP: ($internalvalue -replace "SMTP:","")}
                $objOutputUser.primarySMTP=$internalvalue -replace "SMTP:",""
            }
        }
    }

    $objOutputUsers += $objOutputUser

    Write-Progress -activity "Processing ${xmltoimport} in batches of ${batchsize}" -status "Batch ${outputfilecount}: " -percentComplete (($objOutputUsers.Count / $batchsize) * 100)

    #every so often, dump the processed users in case we blow up somewhere
    if ($count % $batchsize -eq 0)
    {
        Write-Host Hit the maximum users processed without completion... -ForegroundColor Yellow

        #export the collection of users as a CSV
        Write-Host Writing processedusers${outputfilecount}.csv -ForegroundColor Yellow
        $objOutputUsers | Export-Csv -path processedusers${outputfilecount}.csv -NoTypeInformation

        #increment the output file counter
        $outputfilecount+=1

        #reset the collection and the user counter
        $objOutputUsers = $null
        $count=0
    }

    $count+=1

    #need to bail out of the loop if no more users to process
    if ($reader.NodeType -eq [System.Xml.XmlNodeType]::EndElement)
    {
        break
    }

} while ($reader.Read)

#need to write out any users that didn't get picked up in a batch of 1000
#export the collection of users as CSV
Write-Host Writing processedusers${outputfilecount}.csv -ForegroundColor Yellow
$objOutputUsers | Export-Csv -path processedusers${outputfilecount}.csv -NoTypeInformation
```

## <a name="next-steps"></a>Sonraki adımlar
**Genel bakış konuları**  

* [Azure AD Connect eşitlemesi: Eşitlemeyi anlama ve özelleştirme](how-to-connect-sync-whatis.md)  
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)  
