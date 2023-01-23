$emailFrom = "sender@example.net"
$emailTo = "User1@example.net", "user2@example.net"
$smtpServer = "smtp-relay.gmail.com"
$subnet = "192.168.1"

while ($true) {
    # List of IP addresses to ping
    $ips = Get-Content "C:\Users\Location\IP.txt"

    foreach ($ip in $ips) {
          # Test connection to specified host
    $ping = New-Object System.Net.NetworkInformation.Ping
    $pingReply = $ping.Send($ip)

Write-Host "Ping to $ip is currently $($pingReply.RoundtripTime) ms."


    # Check if ping is over 100 ms
    if ($pingReply.RoundtripTime -ge 100) {
        # Send email notification
        $emailSubject = "High ping detected"
        $emailBody = "Ping to $ip is currently $($pingReply.RoundtripTime) ms."
        $credentials = New-Object Management.Automation.PSCredential “mail@example.net”, (“Password” | ConvertTo-SecureString -AsPlainText -Force)
        Send-MailMessage -From $emailFrom -To $emailTo -Subject $emailSubject -Body $emailBody -SmtpServer $smtpServer -Port 587 -UseSsl -Credential $credentials
    }
}
    Start-Sleep -Seconds 3600
}