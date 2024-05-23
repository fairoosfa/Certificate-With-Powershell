# Generate Self-Signed Certificate With Powershell (VPN)
<h3> Case </h3>
1. Create a self-signed client certificate on your local computer.
2. Use PowerShell with Azure PowerShell module and Windows Certificate Manager utility

<h2> Let’s start </h2>
<h3>Create a self-signed root certificate </h3>
<center>
         $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
-Subject "CN=ExampleRootCert" -KeyExportPolicy Exportable `
-HashAlgorithm sha256 -KeyLength 2048 `
-CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
</center>

<h3>Generate a client certificate signed by the new root certificate</h3>
New-SelfSignedCertificate -Type Custom -DnsName P2SChildCert -KeySpec Signature `
-Subject "CN=ExampleChildCert" -KeyExportPolicy Exportable `
-HashAlgorithm sha256 -KeyLength 2048 `
-CertStoreLocation "Cert:\CurrentUser\My" `
-Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")

<h2> Export Certificate Public Key </h2>
<br>1. Run <i><b>certmgr</b></i> from PowerShell </br>
<br>2. Go to <i><b> Personal </b></i> and then to <i> <b> Certificates.</b> </i></br>
<br>3. Select and right click on ExampleRootCert certificate in the list choose <i> <b>All Tasks> Export. </b> </i></br>
<br>4. In the Certificate Export Wizard, check if No, <b>do not export the private key </b> is selected and click Next.</br>
<br>5. Choose <b>Base-64 encoded X.509 (.CER) </b> in Export File Format page and click next</br>
<br>6. On the <b>File to Export </b> page, under File name, browse to a location that is easy to remember, save the file as <i> <b>ExampleRootCert.cer </b></i>, and then click Next.</br>
<br>7. Click Finish on Completing the Certificate Export Wizard page</br>
